+++
title = "Template Fragments"
date = 2022-08-03
updated = 2023-03-18
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

템플릿 조각(Template fragments)은 서버 사이드 렌더링(SSR) 템플릿 라이브러리에서 비교적 드문 기능으로, 
전체 템플릿이 아닌 템플릿 내의 **일부 조각(fragment)** 또는 부분 콘텐츠를 렌더링할 수 있게 해줍니다. 
이 기능은 [하이퍼미디어 구동 애플리케이션(Hypermedia Driven Applications)](@/essays/hypermedia-driven-applications.md)에서 매우 유용합니다. 
왜냐하면 템플릿의 일부를 렌더링하기 위해 별도의 파일로 분리하지 않고, 템플릿을 내부적으로 분해하여 부분 업데이트를 수행할 수 있기 때문입니다. 이는 개별 템플릿 파일의 수를 줄일 수 있습니다.

모든 HTML을 하나의 파일에 유지함으로써 기능이 어떻게 작동하는지 더 쉽게 이해할 수 있습니다. 
이는 [행동의 지역성(Locality of Behavior)](@/essays/locality-of-behaviour.md) 설계 원칙을 따르는 것입니다.

## 동기 부여

Java용으로 만들어진 잘 알려지지 않은 템플릿 언어인 
[Chill Templates](https://github.com/bigskysoftware/chill/tree/master/chill-script)를 사용하여 템플릿 조각이 HDA를 구축하는 데 어떻게 도움이 될 수 있는지 살펴보겠습니다.

여기 연락처를 표시하는 간단한 Chill 템플릿 `/contacts/detail.html`이 있습니다:

##### /contacts/detail.html
```html
<html>
    <body>
        <div hx-target="this">
          #if contact.archived
          <button hx-patch="/contacts/${contact.id}/unarchive">Unarchive</button>
          #else
          <button hx-delete="/contacts/${contact.id}">Archive</button>
          #end
        </div>
        <h3>Contact</h3>
        <p>${contact.email}</p>
    </body>
</html>
```

이 템플릿에는 아카이빙 기능이 있습니다. 연락처의 아카이브 상태에 따라 "Archive" 또는 "Unarchive" 버튼 중 하나가 표시됩니다. 
이 두 버튼은 모두 htmx에 의해 구동되며, 서로 다른 엔드포인트에 HTTP 요청을 보냅니다.

우리는 표시된 버튼을 클릭할 때, 해당 버튼을 둘러싼 `div`의 콘텐츠를 업데이트된 버튼으로 교체하고 싶습니다. 
(div의 `hx-target="this"`를 주목하세요. 이는 그 div의 innerHTML을 교체 대상으로 지정합니다.) 이렇게 하면 "Archive"와 "Unarchive" 사이를 번갈아가며 전환할 수 있습니다.

하지만 불행히도, 이 템플릿의 나머지 부분이 아닌 버튼만 렌더링하려면, 일반적으로 버튼을 별도의 템플릿 파일로 분리하고 이 템플릿에 포함시켜야 합니다. 다음과 같이 말이죠:

##### /contacts/detail.html
```html
<html>
    <body>
        <div hx-target="this">
          #include archive-ui.html
        </div>
        <h3>Contact</h3>
        <p>${contact.email}</p>
    </body>
</html>
```

##### /contacts/archive-ui.html
```html
#if contact.archived
<button hx-patch="/contacts/${contact.id}/unarchive">Unarchive</button>
#else
<button hx-delete="/contacts/${contact.id}">Archive</button>
#end
```

이제 두 개의 템플릿이 생겼습니다. 이제 `archive-ui.html` 템플릿을 별도로 렌더링할 수 있지만, 이렇게 분리하면 아카이빙 기능의 가시성이 줄어듭니다. 
`detail.html` 템플릿만 보고 있을 때 무슨 일이 일어나고 있는지 덜 명확해집니다.

이러한 방식으로 템플릿을 극단적으로 분해하면 관리 및 이해하기 어려운 작은 템플릿 조각들이 많이 생길 수 있습니다.

### 템플릿 조각으로 문제 해결하기

이 문제를 해결하기 위해, Chill 템플릿에는 `#fragment` 지시어가 있습니다. 이 지시어를 사용하면 템플릿 내에서 콘텐츠의 **일부 블록**을 지정하고 **그 부분만** 렌더링할 수 있습니다.

##### 템플릿 조각을 사용하는 /contacts/detail.html
```html
<html>
    <body>
        <div hx-target="this">
          #fragment archive-ui
            #if contact.archived
            <button hx-patch="/contacts/${contact.id}/unarchive">Unarchive</button>
            #else
            <button hx-delete="/contacts/${contact.id}">Archive</button>
            #end
          #end
        </div>
        <h3>Contact</h3>
        <p>${contact.email}</p>
    </body>
</html>
```

이제 템플릿 내에 이 조각이 정의되었으므로, 전체 템플릿을 렌더링할 수 있을 뿐만 아니라:

```java
  Contact c = getContact();
  ChillTemplates.render("/contacts/detail.html", "contact", c);
```

템플릿의 `archive-ui` **조각**만 렌더링할 수도 있습니다:

```java
  Contact c = getContact();
  ChillTemplates.render("/contacts/detail.html#archive-ui", "contact", c);
```

첫 번째 옵션은 연락처의 전체 상세 페이지를 렌더링하고자 할 때 사용합니다.

두 번째 옵션은 아카이브/아카이브 해제 작업을 처리하고 버튼만 다시 렌더링하고자 할 때 사용합니다.

조각을 사용하면, UI를 단일 파일에 통합하여 이 기능이 어떻게 동작하는지 정확히 파악할 수 있으며, 여러 템플릿 파일을 오가며 작업할 필요가 없습니다. 
이렇게 하면 기능의 구현이 더 명확하고 깨끗해집니다.

## 알려진 템플릿 조각 구현

템플릿 조각(그리고 이를 컨트롤러에서 직접 렌더링하는 기능)은 템플릿 라이브러리에서 비교적 드문 기능이지만, 
htmx와 같은 하이퍼미디어 지향 라이브러리와 작업할 때 개발자 경험을 크게 개선할 수 있는 좋은 기회를 제공합니다.

다음은 템플릿 조각 개념이 구현된 것으로 알려진 몇 가지 예입니다:

* Go
  * [표준 라이브러리 (block actions 사용)](https://pkg.go.dev/text/template) [[데모]](https://gist.github.com/benpate/f92b77ea9b3a8503541eb4b9eb515d8a)
* Java
  * [Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#fragment-specification-syntax)
  * [Chill Templates (현재 초기 알파 버전)](https://github.com/bigskysoftware/chill/tree/master/chill-script)
  * [Quarkus Qute](https://quarkus.io/guides/qute-reference#fragments)
  * [JStachio (mustache)](https://jstach.io/doc/jstachio/current/apidocs/#mustache_fragments)
* PHP
  * [Latte](https://latte.nette.org/en/template-inheritance#toc-blocks) - 세 번째 매개변수를 사용하여 템플릿에서 단일 블록만 렌더링할 수 있습니다 -  `$Latte_Engine->render('path/to/template.latte', [ 'foo' => 'bar' ], 'content');`
  * [Laravel Blade](https://laravel.com/docs/10.x/blade#rendering-blade-fragments) - v9.x부터 템플릿 조각에 대한 기본 지원 포함
  * [Twig](https://twig.symfony.com/doc/3.x/api.html#rendering-templates) - `$template->renderBlock('block_name', ['the' => 'variables', 'go' => 'here']);`
* Python
  * [Django Render Block Extension](https://pypi.org/project/django-render-block/) - [htmx 예제 코드](https://github.com/spookylukey/django-htmx-patterns/blob/master/inline_partials.rst) 참조
  * [jinja2-fragments 패키지](https://github.com/sponsfreixes/jinja2-fragments)
  * [jinja_partials 패키지](https://github.com/mikeckennedy/jinja_partials) ([동기](https://github.com/mikeckennedy/jinja_partials/issues/1) 논의)
  * [chameleon_partials 패키지](https://github.com/mikeckennedy/chameleon_partials)
  * [htmlgenerator](https://github.com/basxsoftwareassociation/htmlgenerator)
  * [django-template-partials](https://pypi.org/project/django-template-partials/) ([레포지토리](https://github.com/carltongibson/django-template-partials))
* .NET
  * [Giraffe.ViewEngine.Htmx](https://github.com/bit-badger/Giraffe.Htmx/tree/main/src/ViewEngine.Htmx)
* Rust
  * [MiniJinja](https://docs.rs/minijinja/latest/minijinja/struct.State.html#method.render_block)

다른 예를 알고 있다면, [연락해 주세요](/discord). 목록에 추가하겠습니다.
