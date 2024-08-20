+++
title = "Model/View/Controller (MVC)"
date = 2024-01-16
updated = 2024-01-16
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

htmx와 하이퍼미디어를 사용하는 것에 대한 흔한 반대 의견 중 하나는 다음과 같은 것입니다:

> HTML을 반환하고 JSON을 반환하지 않는다면, 모바일 앱에도 서비스를 제공해야 하므로 API를 중복하지 않으려 할 것이다.

이미 [다른 에세이](@/essays/splitting-your-apis.md)에서 저는 JSON API와 하이퍼미디어 API를 별개의 구성 요소로 분리하는 것이 좋다고 설명한 바 있습니다.

그 에세이에서 저는 HTML을 반환하는 웹 애플리케이션 API 엔드포인트를 안정적이고 규칙적이며 표현력이 있는 JSON 데이터 API로부터 분리하기 위해 API를 "중복"할 것을 (어느 정도까지) 명시적으로 권장했습니다.

이 아이디어와 관련된 대화를 되돌아보면서, 많은 사람들이 저만큼 익숙하지 않은 패턴을 당연히 알고 있을 것이라고 가정하고 있었다고 생각합니다. 
그것은 바로 [모델/뷰/컨트롤러](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) (MVC) 패턴입니다.

## MVC 소개

최근 [팟캐스트](https://www.youtube.com/watch?v=9H5VK9vJ-aw)에서 많은 젊은 웹 개발자들이 MVC에 대한 경험이 많지 않다는 사실을 알고 조금 충격을 받았습니다. 
이는 아마도 싱글 페이지 애플리케이션(SPA)이 표준이 되면서 프론트엔드와 백엔드가 분리된 것과 관련이 있을 것입니다.

MVC는 웹 이전에 존재하던 간단한 패턴으로, 사용자에게 그래픽 인터페이스를 제공하는 거의 모든 프로그램에서 사용할 수 있습니다.

대략적인 아이디어는 다음과 같습니다:

* "모델" 계층은 ["도메인 모델"](https://en.wikipedia.org/wiki/Domain_model)을 포함합니다. 이 계층은 애플리케이션에 특화된 도메인 로직을 포함합니다. 
* 예를 들어, 연락처 관리 애플리케이션에서는 연락처 관련 로직이 이 계층에 포함됩니다. 이 계층에는 시각적 요소에 대한 참조가 없어야 하며, 상대적으로 "순수"해야 합니다.

* "뷰" 계층은 사용자에게 표시되는 "뷰" 또는 시각적 요소를 포함합니다. 이 계층은 종종 (항상은 아니지만) 모델 값을 사용하여 사용자에게 시각적 정보를 제공합니다.

* 마지막으로 "컨트롤러" 계층은 이 두 계층을 조정합니다. 
* 예를 들어, 사용자의 업데이트를 받고, 모델을 업데이트한 후 업데이트된 모델을 뷰에 전달하여 사용자에게 업데이트된 사용자 인터페이스를 표시할 수 있습니다.

많은 변형이 있지만, 기본 아이디어는 이렇습니다.

초기 웹 개발에서는 많은 서버 측 프레임워크가 명시적으로 MVC 패턴을 채택했습니다. 
제가 가장 익숙한 구현은 [Ruby On Rails](https://rubyonrails.org/)로, 각 주제에 대한 문서를 제공합니다: 
데이터베이스에 지속되는 [모델](https://guides.rubyonrails.org/active_record_basics.html), HTML 뷰 생성을 위한 [뷰](https://guides.rubyonrails.org/action_view_overview.html), 
그리고 둘 사이를 조정하는 [컨트롤러](https://guides.rubyonrails.org/action_controller_overview.html).

Rails에서 대략적인 아이디어는 다음과 같습니다:

* 모델은 애플리케이션 로직과 데이터베이스 액세스를 수집합니다.
* 뷰는 모델을 사용하여 템플릿 언어([ERB](https://github.com/ruby/erb))를 통해 HTML을 생성합니다. (참고로, 이곳에서 [HTML sanitizing](https://en.wikipedia.org/wiki/HTML_sanitization)이 이루어집니다.)
* 컨트롤러는 HTTP 요청을 받아 일반적으로 모델에서 어떤 동작을 수행한 다음 그 결과를 뷰에 전달합니다 (또는 리디렉션 등을 수행할 수 있습니다).

Rails는 HTML 및 HTTP 요청/응답 라이프사이클을 기반으로 한 표준적(다소 "얕고" 단순화된) MVC 패턴의 구현을 제공합니다.

### Fat Model/Skinny Controller

Rails 커뮤니티에서 자주 언급되는 개념 중 하나는 ["Fat Model, Skinny Controller"](https://riptutorial.com/ruby-on-rails/example/9609/fat-model--skinny-controller)입니다. 
이 아이디어는 컨트롤러가 상대적으로 간단해야 하며, 모델에서 메서드 하나 또는 두 개를 호출한 후 바로 결과를 뷰에 전달해야 한다는 것입니다.

모델은 반면에 도메인 특화 로직이 많이 포함된 "두꺼운" 구조를 가질 수 있습니다. 
(이것이 [God Objects](https://en.wikipedia.org/wiki/God_object)로 이어질 수 있다는 반대 의견도 있지만, 이는 지금 논외로 두겠습니다.)

이 Fat Model/Skinny Controller 개념을 염두에 두고, MVC 패턴의 간단한 예제와 이 패턴이 왜 유용한지 알아보겠습니다.

## MVC 스타일 웹 애플리케이션

이번 예제로는 제가 좋아하는 온라인 연락처 애플리케이션을 살펴보겠습니다. 이 애플리케이션의 주어진 연락처 페이지를 HTML 페이지로 생성하여 표시하는 컨트롤러 메서드는 다음과 같습니다:

```python
@app.route("/contacts")
def contacts():
    contacts = Contact.all(page=request.args.get('page', default=0, type=int))
    return render_template("index.html", contacts=contacts)
```

여기서 저는 [Python](https://www.python.org/)과 [Flask](https://flask.palletsprojects.com/en/3.0.x/)를 사용하고 있습니다. 
이는 제가 [Hypermedia Systems](https://hypermedia.systems/) 책에서 사용하는 도구들입니다.

이 예제에서 컨트롤러는 매우 "얇습니다": 컨트롤러는 `Contact` 모델 객체를 통해 연락처를 조회하고, 요청에서 전달된 `page` 인자를 전달합니다.

이는 매우 전형적인 형태입니다: 컨트롤러의 역할은 HTTP 요청을 도메인 로직에 매핑하고, HTTP 특정 정보를 추출하여 모델이 이해할 수 있는 데이터(예: 페이지 번호)로 변환하는 것입니다.

컨트롤러는 이후 `index.html` 템플릿에 페이지된 연락처 컬렉션을 전달하여, 이를 HTML 페이지로 렌더링하고 사용자가 볼 수 있도록 합니다.

반면, `Contact` 모델은 내부적으로 비교적 "두꺼울" 수 있습니다: 
`all()` 메서드는 데이터베이스 조회, 데이터 페이지 처리, 변환 또는 비즈니스 규칙 적용 등을 포함하는 도메인 로직을 내부적으로 가질 수 있습니다. 이는 괜찮습니다. 
그 로직은 Contact 모델에 캡슐화되어 있고, 컨트롤러는 이를 처리할 필요가 없습니다.

### JSON 데이터 API 컨트롤러 생성

이렇게 비교적 잘 개발된 Contact 모델이 도메인을 캡슐화하고 있다면, 비슷한 작업을 수행하면서 HTML 문서가 아닌 JSON 문서를 반환하는 _다른_ API 엔드포인트/컨트롤러를 쉽게 생성할 수 있습니다:

```python
@app.route("/api/v1/contacts")
def contacts():
    contacts = Contact.all(page=request.args.get('page', default=0, type=int))
    return jsonify(contacts=contacts)
```

### 하지만 코드를 중복하고 있잖아요!

이 두 컨트롤러 함수들을 보면 "이건 어리석어, 메서드가 거의 동일하잖아"라고 생각할 수 있습니다.

당신이 맞습니다. 현재로서는 거의 동일합니다.

하지만 시스템에 두 가지 잠재적 추가 사항을 고려해 보겠습니다.

#### JSON API에 대한 레이트 리미팅 추가

먼저, DDOS 공격이나 잘못 작성된 자동화 클라이언트가 시스템을 범람하지 않도록 JSON API에 레이트 리미팅을 추가해 보겠습니다. 
이를 위해 [Flask-Limiter](https://flask-limiter.readthedocs.io/en/stable/) 라이브러리를 추가할 것입니다:

```python
@app.route("/api/v1/contacts")
@limiter.limit("1 per second")
def contacts():
    contacts = Contact.all(page=request.args.get('page', default=0, type=int))
    return jsonify(contacts=contacts)
```

간단합니다.

하지만 주의할 점은: 우리는 웹 애플리케이션에 이 제한을 적용하고 싶지 않고, JSON 데이터 API에만 적용하고 싶습니다. 그리고 두 가지를 분리했기 때문에 이를 쉽게 달성할 수 있습니다.

### 웹 애플리케이션에 그래프 추가하기

이제 다른 변경 사항을 고려해 봅시다. HTML 기반 웹 애플리케이션의 `index.html` 템플릿에 하루에 추가된 연락처 수를 나타내는 그래프를 추가하려고 합니다. 
그런데 이 그래프를 계산하는 데 비용이 많이 듭니다.

우리는 `index.html` 템플릿의 렌더링을 그래프 생성에 의해 차단되지 않도록 하고 싶습니다. 
따라서 이를 위해 [Lazy Loading](https://htmx.pinstella.com/examples/lazy-load/) 패턴을 사용할 것입니다. 
이를 위해 HTML로 해당 지연 로딩 콘텐츠를 반환하는 새로운 엔드포인트 `/graph`를 만들어야 합니다.

```python
@app.route("/graph")
def graph():
    graphInfo = Contact.computeGraphInfo(page=request.args.get('page', default=0, type=int))
    return render_template("graph.html", info=graphInfo)
```

여기서도 역시, 우리의 컨트롤러는 여전히 "얇습니다": 단순히 모델에 작업을 위임하고, 결과를 뷰로 전달합니다.

여기서 주목할 점은, 웹 애플리케이션의 HTML API에 새로운 엔드포인트를 추가했지만, _JSON 데이터 API에는 추가하지 않았다는 것_입니다. 
즉, 이 (UI 요구에 의해 완전히 주도되는) 특수한 엔드포인트가 영구적으로 유지될 것이라고 다른 비웹 클라이언트들에게 약속하지 않는다는 것입니다.

우리는 이 데이터가 `/graph`에 영구적으로 제공될 것이라고 *모든* 클라이언트에게 약속하지 않기 때문에, HTML 기반 웹 애플리케이션에서 
[응용 상태의 엔진으로서의 하이퍼미디어(HATEOAS)](https://htmx.pinstella.com/essays/hateoas/)를 사용함으로써 이 URL을 나중에 자유롭게 제거하거나 리팩터링할 수 있습니다.

예를 들어, 데이터베이스 최적화로 인해 갑자기 그래프 계산이 빨라져서 `/contacts` 응답에 인라인으로 포함시킬 수 있다면, 
이 엔드포인트는 다른 클라이언트에게 노출되지 않았기 때문에 웹 애플리케이션을 지원하기 위해서만 존재하는 것으로 제거할 수 있습니다.

따라서 우리는 하이퍼미디어 API에 필요한 [유연성](https://htmx.pinstella.com/essays/hypermedia-apis-vs-data-apis.md)과 JSON 데이터 API에 필요한 
[기능들](https://htmx.pinstella.com/essays/hypermedia-apis-vs-data-apis.md)을 얻을 수 있습니다.

MVC 관점에서 가장 중요한 것은, 도메인 로직이 모델에 수집되었기 때문에, 두 개의 API를 유연하게 변경하면서도 상당한 코드 재사용을 달성할 수 있다는 점입니다. 
JSON과 HTML 컨트롤러 간에 초기에는 많은 유사성이 있었지만, 시간이 지나면서 차별화되었습니다.

동시에, 모델 로직을 중복하지 않았습니다: 두 컨트롤러 모두 상대적으로 "얇게" 유지되었으며, 대부분의 작업을 수행하기 위해 모델 객체에 위임했습니다.

따라서 두 API는 서로 분리되었으며, 도메인 로직은 중앙에 집중되어 있습니다.

(이 점은 제가 [왜 콘텐츠 협상을 사용하지 않는지](https://htmx.pinstella.com/essays/why-tend-not-to-use-content-negotiation.md)와 같은 엔드포인트에서 HTML과 JSON을 반환하지 않는 이유와도 연결됩니다.)

## MVC 프레임워크

[Spring](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/mvc.html), 
[ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet/mvc), Rails와 같은 많은 오래된 웹 프레임워크는 매우 강력한 MVC 개념을 가지고 있으며, 
이를 통해 로직을 매우 효과적으로 분리할 수 있습니다.

Django는 [MVT](https://www.askpython.com/django/django-mvt-architecture)라는 개념을 사용하여 MVC와 유사한 변형을 가지고 있습니다.

이러한 MVC에 대한 강력한 지원은 이러한 프레임워크가 htmx와 잘 어울리는 이유 중 하나이며, 이러한 커뮤니티가 htmx에 대해 흥미를 느끼는 이유이기도 합니다.

위의 예시들은 분명 [객체 지향](https://www.azquotes.com/picture-quotes/quote-object-oriented-programming-is-an-exceptionally-bad-idea-which-could-only-have-originated-edsger-dijkstra-7-85-25.jpg) 프로그래밍에 편향되어 있지만, 
동일한 아이디어는 함수형 컨텍스트에서도 적용될 수 있습니다.

## 결론

MVC 개념이 새로운 분들에게는, 이 글이 MVC 개념에 대해 좋은 감을 주고, 
웹 애플리케이션에서 이 조직 원칙을 채택함으로써 API를 효과적으로 분리하면서도 코드의 중복을 피할 수 있는 방법을 보여주기를 바랍니다.
