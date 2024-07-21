+++
title = "Documentation"
[extra]
custom_classes = "wide-content"
+++
<div class="row">
<div class="2 col nav">

**Contents**

<div id="contents">

* [introduction](#introduction)
* [installing](#installing)
* [ajax](#ajax)
  * [triggers](#triggers)
    * [trigger modifiers](#trigger-modifiers)
    * [trigger filters](#trigger-filters)
    * [special events](#special-events)
    * [polling](#polling)
    * [load polling](#load_polling)
  * [indicators](#indicators)
  * [targets](#targets)
  * [swapping](#swapping)
  * [synchronization](#synchronization)
  * [css transitions](#css_transitions)
  * [out of band swaps](#oob_swaps)
  * [parameters](#parameters)
  * [confirming](#confirming)
* [inheritance](#inheritance)
* [boosting](#boosting)
* [websockets & SSE](#websockets-and-sse)
* [history](#history)
* [requests & responses](#requests)
* [validation](#validation)
* [animations](#animations)
* [extensions](#extensions)
* [events & logging](#events)
* [debugging](#debugging)
* [scripting](#scripting)
  * [hx-on attribute](#hx-on)
* [3rd party integration](#3rd-party)
  * [Web Components](#web-components)
* [caching](#caching)
* [security](#security)
* [configuring](#config)

</div>

</div>
<div class="10 col">

## htmx in a Nutshell {#introduction}

htmx는 자바스크립트를 사용하는 대신 HTML에서 직접 최신 브라우저 기능에 액세스할 수 있게 해주는 라이브러리입니다.

htmx를 이해하려면 먼저 anchor 태그를 살펴봐야 합니다:

```html
<a href="/blog">Blog</a>
```

이 anchor 태그는 브라우저에게 아래를 말해줍니다:

> "사용자가 이 링크를 클릭하면 '/blog'에 HTTP GET 요청을 보내고 응답 콘텐츠를 
>  브라우저 창에 로드합니다".

이를 염두에 두고 다음 HTML을 살펴 보세요.:

```html
<button hx-post="/clicked"
    hx-trigger="click"
    hx-target="#parent-div"
    hx-swap="outerHTML"
>
    Click Me!
</button>
```

이것은 htmx에게 아래를 말해줍니다:

> "사용자가 이 버튼을 클릭하면 '/clicked'로 HTTP POST 요청을 발행하고 DOM에서 id가 `parent-div`인
>  요소를 응답으로 온 콘텐츠를 사용하여 바꿉니다."

htmx는 하이퍼텍스트로서의 HTML의 핵심 개념을 확장하고 일반화하여 언어 내에서 직접 더 많은 가능성을 열어줍니다:

* 이제 anchor와 form뿐만 아니라 모든 요소에서 HTTP 요청을 발행할 수 있습니다
* 이제 click이나 form 제출뿐만 아니라 모든 이벤트가 요청을 트리거할 수 있습니다
* 이제 GET 및 POST뿐만 아니라 모든 [HTTP verb](https://en.wikipedia.org/wiki/HTTP_Verbs)를 사용할 수 있습니다
* 이제 전체 창뿐만 아니라 모든 요소가 요청에 의한 업데이트 대상이 될 수 있습니다

htmx를 사용하는 경우 서버 측에서는 일반적으로 *JSON*이 아닌 *HTML*로 응답한다는 점에 유의하세요. 
이렇게 하면 [하이퍼텍스트를 애플리케이션 상태 엔진](https://en.wikipedia.org/wiki/HATEOAS)으로 사용하는 
[오리지널 웹 프로그래밍 모델](https://en.wikipedia.org/wiki/HATEOAS)에 충실하게 유지되므로 해당 개념을 제대로 이해할 필요도 없습니다.

원하는 경우 htmx를 사용할 때 [`data-`](https://html.spec.whatwg.org/multipage/dom.html#attr-data-*) 접두사를 사용할 수 있다는 점을 언급할 가치가 있습니다:

```html
<a data-hx-post="/click">Click Me!</a>
```

마지막으로 htmx [Version 1](https://v1.htmx.org)은 여전히 지원되며 IE11을 지원합니다.

## 1.x to 2.x Migration Guide

[htmx 1.x](https://v1.htmx.org)에서 htmx 2.x로 마이그레이션하는 경우 [htmx 1.x migration guide](@/migration-guide-htmx-1.md)를 참조하세요.

intercooler.js에서 htmx로 마이그레이션하는 경우 [intercooler 마이그레이션 가이드](@/migration-guide-intercooler.md)를 참조하세요.

## Installing

Htmx는 종속성이 없는 브라우저 지향 자바스크립트 라이브러리입니다. 즉, 문서 head에 `<script>` 태그를 추가하는 것만큼이나 간단하게 사용할 수 있습니다. 
이를 사용하기 위한 빌드 시스템은 필요하지 않습니다.

### Via A CDN (e.g. unpkg.com)

htmx를 사용하는 가장 빠른 방법은 CDN을 통해 로드하는 것입니다. 
아래를 head 태그에 추가하고 시작하면 됩니다:

```html
<script src="https://unpkg.com/htmx.org@2.0.0" integrity="sha384-wS5l5IKJBvK6sPTKa2WZ1js3d947pvWXbPJ1OmWfEuxLgeHcEbjUUA5i9V5ZkpCw" crossorigin="anonymous"></script>
```

디버깅을 위한 비마이닝 버전도 사용할 수 있습니다:

```html
<script src="https://unpkg.com/htmx.org@2.0.0/dist/htmx.js" integrity="sha384-Xh+GLLi0SMFPwtHQjT72aPG19QvKB8grnyRbYBNIdHWc2NkCrz65jlU7YrzO6qRp" crossorigin="anonymous"></script>
```

CDN 접근 방식은 매우 간단하지만 [프로덕션 환경에서는 CDN을 사용하지 않는 것](https://blog.wesleyac.com/posts/why-not-javascript-cdn)을 
고려해보아야 합니다.

### Download a copy

다음으로 가장 쉽게 설치하는 방법은 프로젝트에 htmx를 복사하는 것입니다.

[unpkg.com](https://unpkg.com/htmx.org/dist/htmx.min.js)에서 `htmx.min.js`를 다운로드하여 프로젝트의 적절한 디렉터리에 추가하고 
필요한 경우 `<script>` 태그에 포함하여 사용하세요:

```html
<script src="/path/to/htmx.min.js"></script>
```

### npm

[npm](https://www.npmjs.com/) 스타일 빌드 시스템의 경우 npm을 통해 htmx를 설치할 수 있습니다:

```sh
npm install htmx.org
```

설치 후에는 적절한 툴을 사용하여 `node_modules/htmx.org/dist/htmx.js`(또는 `.min.js`)를 사용해야 합니다. 
예를 들어, 일부 확장 프로그램 및 프로젝트별 코드와 함께 htmx를 번들로 제공하는 것이 가능합니다.

### Webpack

Webpack을 사용하여 자바스크립트를 관리하는 경우:

* 선호하는 패키지 관리자(예: npm 또는 yarn)를 통해 `htmx`를 설치합니다.
* `index.js`에 import 구문을 추가합니다.

```js
import 'htmx.org';
```

전역 `htmx` 변수를 사용하려면(권장) window 범위에 변수를 삽입해야 합니다:

* 커스텀 JS 파일을 생성합니다
* 이 파일을 (2번째 단계 밑에 있던 import 구문처럼) `index.js`로 가져옵니다.

```js
import 'path/to/my_custom.js';
```

* 그런 다음 이 코드를 파일에 추가합니다:

```js
window.htmx = require('htmx.org');
```

* 마지막으로 번들을 다시 빌드합니다.

## AJAX

htmx의 핵심은 HTML에서 직접 AJAX 요청을 발행할 수 있는 일련의 속성입니다:

| Attribute                              | Description                 |
|----------------------------------------|-----------------------------|
| [hx-get](@/attributes/hx-get.md)       | 지정된 URL로 `GET` 요청을 보냅니다.    |
| [hx-post](@/attributes/hx-post.md)     | 지정된 URL로 `POST` 요청을 보냅니다.   |
| [hx-put](@/attributes/hx-put.md)       | 지정된 URL로 `PUT` 요청을 보냅니다.    |
| [hx-patch](@/attributes/hx-patch.md)   | 지정된 URL로 `PATCH` 요청을 보냅니다.  |
| [hx-delete](@/attributes/hx-delete.md) | 지정된 URL로 `DELETE` 요청을 보냅니다. |

이러한 각 속성은 AJAX 요청을 보낼 때 URL을 사용합니다. 
요소는 요소가 [트리거](#triggers)될 때 지정된 URL에 지정된 유형의 요청을 보냅니다:

```html
<button hx-put="/messages">
    Put To Messages
</button>
```

이 코드는 브라우저에게 알려줍니다:

> 사용자가 이 버튼을 클릭하면 URL /messages에 PUT 요청을 보내고 응답을 button에 로드합니다.

### Triggering Requests {#triggers}

기본적으로 AJAX 요청은 요소의 '자연스러운' 이벤트에 의해 트리거됩니다:

* `input`, `textarea` 및 `select`은 `change` 이벤트에서 트리거됩니다.
* `form`은 `submit` 이벤트에서 트리거됩니다.
* 이외 나머지는 `click` 이벤트에서 트리거됩니다.

다른 동작을 원한다면 [hx-trigger](@/attributes/hx-trigger.md) 속성을 사용하여 
요청을 유발할 이벤트를 지정할 수 있습니다.

다음은 마우스를 올려두면 `/mouse_entered`에 post 요청을 보내는 `div`입니다:

```html
<div hx-post="/mouse_entered" hx-trigger="mouseenter">
    [Here Mouse, Mouse!]
</div>
```

#### Trigger Modifiers

트리거는 동작을 변경하는 몇 가지 추가 수정자를 가질 수도 있습니다. 
예를 들어 요청이 한 번만 발생하도록 하려면 트리거에 `once` 수정자를 사용할 수 있습니다:

```html
<div hx-post="/mouse_entered" hx-trigger="mouseenter once">
    [Here Mouse, Mouse!]
</div>
```

트리거에 사용할 수 있는 다른 수정자는 다음과 같습니다:

* `changed` - 요소의 값이 변경된 경우에만 요청을 발행합니다.
*  `delay:<time interval>` - 요청을 보내기 전에 주어진 시간(예: `1s`)만큼 기다립니다. 이벤트가 다시 트리거되면 카운트다운이 초기화됩니다.
*  `throttle:<time interval>` - 요청을 보내기 전에 주어진 시간(예: `1s`)만큼 기다립니다. 
`delay`와 달리 시간 제한에 도달하기 전에 새로운 이벤트가 발생하면 그 이벤트는 무시되며, 요청은 처음 트리거될 때의 시간 간격이 끝날 때 보내집니다.
*  `from:<CSS Selector>` - 다른 요소에서 이벤트를 감지합니다. 이는 키보드 단축키와 같은 기능에 사용할 수 있습니다.

이러한 속성을 사용하여 [Active 검색](@/examples/active-search.md)과 같은 많은 일반적인 UX 패턴을 구현할 수 있습니다:

```html
<input type="text" name="q"
    hx-get="/trigger_delay"
    hx-trigger="keyup changed delay:500ms"
    hx-target="#search-results"
    placeholder="Search..."
>
<div id="search-results"></div>
```

이 입력은 입력이 변경된 경우 key up 이벤트가 발생한 후 500밀리초 후에 요청을 보내고 
ID가 `search-results`인 `div`에 결과를 삽입합니다.

여러 트리거를 쉼표로 구분하여 [hx-trigger](@/attributes/hx-trigger.md) 속성에 지정할 수 있습니다.

#### Trigger Filters

이벤트 이름 뒤에 대괄호를 사용하여, 평가할 자바스크립트 표현식을 대괄호로 묶어 트리거 필터를 적용할 수도 있습니다. 
표현식이 `true`로 평가되면 이벤트가 트리거되고, 그렇지 않으면 트리거되지 않습니다.

다음은 요소를 Control-클릭할 때만 트리거되는 예제입니다.

```html
<div hx-get="/clicked" hx-trigger="click[ctrlKey]">
    Control Click Me
</div>
```

`ctrlKey`와 같은 프로퍼티는 트리거 이벤트에 대해 먼저 확인된 다음 전역 범위에 대해 확인됩니다. 
`this` 심볼은 현재 요소로 설정됩니다.

#### Special Events

htmx는 [hx-trigger](@/attributes/hx-trigger.md)에서 사용할 수 있는 몇 가지 특별한 이벤트를 제공합니다:

* `load` - 요소가 처음 로드될 때 한 번 발동됩니다.
* `revealed` - 요소가 뷰포트에 처음 스크롤될 때 한 번 발동됩니다.
* `intersect` - 요소가 뷰포트와 처음 교차할 때 한 번 발동됩니다. 이 옵션에는 두 가지 추가 옵션이 있습니다:
    * `root:<selector>` - 교차되는 루트 요소에 대한 CSS 선택자입니다.
    * `threshold:<float>` - 0.0에서 1.0 사이의 부동 소수점 숫자로, 이벤트를 발동되게 할 교차점의 양을 나타냅니다.

고급 사용 사례가 있는 경우 사용자 지정 이벤트를 사용하여 요청을 트리거할 수도 있습니다.

#### Polling

요소가 이벤트를 기다리지 않고 지정된 URL을 polling하도록 하려면 
[`hx-trigger`](@/attributes/hx-trigger.md) 속성과 함께 `every` 구문을 사용하면 됩니다:

```html
<div hx-get="/news" hx-trigger="every 2s"></div>
```

이것은 htmx에게 말해줍니다.

> 2초마다 /news로 GET을 보내고 div에 응답을 로드합니다.

서버 응답에서 polling을 중지하려면 HTTP 응답 코드 [`286`](https://en.wikipedia.org/wiki/86_(term))으로 응답하면 
요소에서 polling을 자동으로 취소합니다.

#### Load Polling {#load_polling}

htmx에서 polling을 달성하는 데 사용할 수 있는 또 다른 기술은 요소가 delay와 함께 
`load` 트리거를 지정하고 응답으로 자신을 대체하는 "load polling"입니다.

```html
<div hx-get="/messages"
    hx-trigger="load delay:1s"
    hx-swap="outerHTML"
>
</div>
```

`/messages`의 엔드포인트가 이런 식으로 설정된 div를 계속 반환하면 매초마다 URL에 대한 'polling'을 계속합니다.

load polling은 사용자에게 [progress bar](@/examples/progress-bar.md)을 표시하는 경우와 같이 
polling이 종료되는 종료 지점이 있는 상황에서 유용할 수 있습니다.

### Request Indicators {#indicators}

브라우저에서는 피드백을 제공하지 않기 때문에 AJAX 요청이 실행되면 사용자에게 어떤 일이 일어나고 있음을 알려주는 것이 좋습니다. 
htmx에서는 `htmx-indicator` 클래스를 사용하여 이 작업을 수행할 수 있습니다.

`htmx-indicator` 클래스는 이 클래스가 있는 모든 요소의 불투명도가 
기본적으로 0이 되도록 정의되어 보이지 않지만 DOM에는 존재하게 만듭니다.

htmx가 요청을 보내면 요소(요청하는 요소 또는 지정된 경우 다른 요소)에 `htmx-request` 클래스를 넣습니다. 
`htmx-request` 클래스는 `htmx-indicator` 클래스가 있는 하위 요소의 불투명도를 1로 전환하여 indicator를 표시합니다.

```html
<button hx-get="/click">
    Click Me!
    <img class="htmx-indicator" src="/spinner.gif">
</button>
```

여기 버튼이 있습니다. 이 버튼을 클릭하면 `htmx-request` 클래스가 추가되어 
spinner gif 요소가 표시됩니다. (저는 요즘 [SVG spinners](http://samherbert.net/svg-loaders/)를 좋아합니다.)

`htmx-indicator` 클래스는 불투명도를 사용하여 진행률 표시기를 숨기고 표시하지만, 
다른 방법을 선호하는 경우 다음과 같이 자체 CSS 전환을 만들 수 있습니다:

```css
.htmx-indicator{
    display:none;
}
.htmx-request .htmx-indicator{
    display:inline;
}
.htmx-request.htmx-indicator{
    display:inline;
}
```

`htmx-request` 클래스를 다른 요소에 추가하려면 
CSS 선택자와 함께 [hx-indicator](@/attributes/hx-indicator.md) 속성을 사용하면 됩니다:

```html
<div>
    <button hx-get="/click" hx-indicator="#indicator">
        Click Me!
    </button>
    <img id="indicator" class="htmx-indicator" src="/spinner.gif"/>
</div>
```

여기서는 ID로 indicator를 명시적으로 호출합니다. 
부모 `div`에 클래스를 배치해도 동일한 효과를 얻을 수 있다는 점에 유의하세요.

또한 [hx-disabled-elt](@/attributes/hx-disabled-elt.md) 속성을 사용하여 요청 기간 동안 요소에 
[`disabled` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled) 속성을 추가할 수도 있습니다.

### Targets

요청을 한 요소가 아닌 다른 요소에 응답을 로드하려는 경우 CSS 선택자를 사용하는 
[hx-target](@/attributes/hx-target.md) 속성을 사용할 수 있습니다. 라이브 검색 예시를 다시 살펴보겠습니다:

```html
<input type="text" name="q"
    hx-get="/trigger_delay"
    hx-trigger="keyup delay:500ms changed"
    hx-target="#search-results"
    placeholder="Search..."
>
<div id="search-results"></div>
```

검색 결과가 input 태그가 아닌 `div#search-results`에 로드되는 것을 볼 수 있습니다.


#### Extended CSS Selectors {#extended-css-selectors}

`hx-target` 및 대부분의 CSS 선택자를 사용하는 속성들은 "확장된" CSS 구문을 지원합니다:

* `this` 키워드를 사용할 수 있으며, 이는 `hx-target` 속성이 있는 요소 자체가 대상임을 나타냅니다.
* `closest <CSS 선택자>` 구문은 주어진 CSS 선택자와 일치하는 가장 [가까운](https://developer.mozilla.org/docs/Web/API/Element/closest) 상위 요소 또는 자기 자신을 찾습니다.
  (예: `closest tr`은 요소에서 가장 가까운 테이블 행을 대상으로 합니다)
* `next <CSS 선택자>` 구문은 DOM에서 주어진 CSS 선택자와 일치하는 다음 요소를 찾습니다.
* `previous <CSS 선택자>` 구문은 DOM에서 주어진 CSS 선택자와 일치하는 이전 요소를 찾습니다.
* `find <CSS 선택자>` 구문은 주어진 CSS 선택자와 일치하는 첫 번째 하위 자손 요소를 찾습니다.
  (예: `find tr`은 요소의 첫 번째 하위 자손 행을 대상으로 합니다)

추가로, CSS 선택자는 `<` 및 `/>` 문자로 감싸서 hyperscript의 
[query literal](https://hyperscript.org/expressions/query-reference/) 구문을 모방할 수 있습니다.

이와 같이 상대적으로 조절할 수 있는 대상들은 많은 `id` 속성을 DOM에 추가하지 않고도 
유연한 사용자 인터페이스를 만드는 데 유용할 수 있습니다.

### Swapping {#swapping}

htmx는 DOM으로 반환되는 HTML을 교체하는 몇 가지 방법을 제공합니다. 
기본적으로 콘텐츠는 대상 요소를 `innerHTML`으로 대체합니다. 
다음 값 중 하나와 함께 [hx-swap](@/attributes/hx-swap.md) 속성을 사용하여 이를 수정할 수 있습니다:

| Name | Description
|------|-------------
| `innerHTML` | 기본값으로, 대상 요소 안에 콘텐츠를 넣습니다.
| `outerHTML` | 전체 대상 요소를 응답으로 받은 콘텐츠로 바꿉니다.
| `afterbegin` | 대상 내부의 첫 번째 자식 앞에 콘텐츠를 추가합니다.
| `beforebegin` | 대상의 부모 요소 안에서 대상 앞에 콘텐츠를 추가합니다.
| `beforeend` | 대상 내부의 마지막 자식 다음에 콘텐츠를 추가합니다.
| `afterend` | 대상의 부모 요소 안에서 대상 뒤에 콘텐츠를 추가합니다.
| `delete` | 응답에 관계없이 대상 요소를 삭제합니다.
| `none` | 응답으로 온 콘텐츠를 추가하지 않습니다([Out of Band 교체](#oob_swaps) 및 [Response Headers](#response-headers)는 계속 처리됩니다).

#### Morph Swaps {#morphing}

위의 표준 교체 메커니즘 외에도 htmx는 확장을 통해 _Morphing_ Swap도 지원합니다.
Morph Swap은 단순히 교체하는 것이 아니라 새 콘텐츠를 기존 DOM에 _병합_하려고 시도합니다. 
교체 작업 중에 기존 노드를 제자리에서 변경하여 focus, 동영상 상태 등을 더 잘 보존하는 경우가 많지만 CPU를 더 많이 사용하게 됩니다.

morph-style swaps에 사용할 수 있는 확장 기능은 다음과 같습니다:

* [Idiomorph](https://github.com/bigskysoftware/idiomorph#htmx) - htmx 개발자가 만든 morphing 알고리즘입니다.
* [Morphdom Swap](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/morphdom-swap/README.md) - 오리지널 DOM morphing 라이브러리인 
[morphdom](https://github.com/patrick-steele-idem/morphdom)을 기반으로 합니다.
* [Alpine-morph](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/alpine-morph/README.md) - [alpine morph](https://alpinejs.dev/plugins/morph) 플러그인을 기반으로 하며, alpine.js와 잘 조합됩니다.

#### View Transitions {#view-transitions}

새롭고 실험적인 [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)는 개발자가 서로 다른 DOM 상태 간에 애니메이션 전환을 만들 수 있는 방법을 제공합니다. 
아직 개발 중이며 모든 브라우저에서 사용할 수 있는 것은 아니지만, 특정 브라우저에서 API를 사용할 수 없는 경우
non-transition 메커니즘으로 돌아가는 이 새로운 API로 작업할 수 있는 방법을 htmx를 통해 제공합니다.

다음 방법을 사용하여 이 새로운 API를 실험해 볼 수 있습니다:

* 모든 교체 작업에 트랜지션을 사용하려면 `htmx.config.globalViewTransitions` 구성 변수를 `true`로 설정합니다.
* `hx-swap` 속성에서 `transition:true` 옵션을 사용합니다.
* 위의 구성 중 하나로 인해 요소 교체가 전환되는 경우 
`htmx:beforeTransition` 이벤트를 포착하고 이에 대해 `preventDefault()`를 호출하여 전환을 취소하는 것이 가능합니다.

View Transitions은 [해당 기능에 대한 Chrome 문서](https://developer.chrome.com/docs/web-platform/view-transitions/#simple-customization)에 설명된 대로 CSS를 사용하여 구성할 수 있습니다.

[Animation Examples](/examples/animations#view-transitions) 페이지에서 view transition 예제를 확인할 수 있습니다.

#### Swap Options

[hx-swap](@/attributes/hx-swap.md) 속성은 htmx의 스왑 동작을 조정하기 위한 다양한 옵션을 지원합니다. 
예를 들어, 기본적으로 htmx는 웹 페이지의 title을 새 콘텐츠의 어느 곳에서나 발견되는 title 태그의 제목으로 바뀝니다. 
`ignoreTitle` 수정자를 true로 설정하여 이 동작을 해제할 수 있습니다:

```html
    <button hx-post="/like" hx-swap="outerHTML ignoreTitle:true">Like</button>
```

`hx-swap`에서 사용할 수 있는 수정자는 다음과 같습니다:

| Option        | Description                                                                  |
|---------------|------------------------------------------------------------------------------|
| `transition`  | `true` 또는 `false`, 이 교체에 대해 View Transitions API를 사용할지 여부                    |
| `swap`        | 오래된 콘텐츠가 지워지고 새로운 콘텐츠가 삽입될 때까지 사용할 교체 지연 시간 (예: `100ms`)                     |
| `settle`      | 새로운 콘텐츠가 삽입되고 정착될 때까지 사용할 정착 지연 시간 (예: `100ms`)                              |
| `ignoreTitle` | `true`로 설정되면, 새로운 콘텐츠에서 발견된 title은 무시되고 문서 title이 업데이트되지 않습니다                |
| `scroll`      | `top` 또는 `bottom`, 대상 요소를 상단 또는 하단으로 스크롤합니다                                  |
| `show`        | `top` 또는 `bottom`, 대상 요소의 상단 또는 하단을 뷰에 보이도록 스크롤합니다                           |

모든 교체 수정자는 교체 스타일이 지정된 후에 나타나며 콜론으로 구분됩니다.

이러한 옵션에 대한 자세한 내용은 [hx-swap](@/attributes/hx-swap.md) 문서를 참조하세요.

### Synchronization {#synchronization}

종종 두 요소 간의 요청을 조정하고 싶을 때가 있습니다. 예를 들어 한 요소의 요청이 다른 요소의 요청을 대체하거나 
다른 요소의 요청이 완료될 때까지 기다리기를 원할 수 있습니다.

htmx는 이를 달성하는 데 도움이 되는 [`hx-sync`](@/attributes/hx-sync.md) 속성을 제공합니다.

이 HTML에서 form 제출과 개별 input의 유효성 검사 요청 사이의 경합 조건을 생각해 보세요:

```html
<form hx-post="/store">
    <input id="title" name="title" type="text"
        hx-post="/validate"
        hx-trigger="change">
    <button type="submit">Submit</button>
</form>
```

`hx-sync`를 사용하지 않고 입력을 작성하고 즉시 양식을 제출하면 `/validate` 및 `/store`에 대한 두 개의 병렬 요청이 트리거됩니다.

`hx-sync="closest form:abort"`를 input 요소에 사용하면 form에서의 요청을 감시하고, 
input 요청이 진행 중일 때 form 요청이 존재하거나 시작되면 input 요청을 중단합니다.

```html
<form hx-post="/store">
    <input id="title" name="title" type="text"
        hx-post="/validate"
        hx-trigger="change"
        hx-sync="closest form:abort">
    <button type="submit">Submit</button>
</form>
```

이렇게 하면 두 요소 간의 동기화가 선언적 방식으로 해결됩니다.

htmx는 요청을 취소하는 프로그래밍 방식도 지원합니다. 
`htmx:abort` 이벤트를 요소에 전달하여 모든 in-flight 요청을 취소할 수 있습니다:

```html
<button id="request-button" hx-post="/example">
    Issue Request
</button>
<button onclick="htmx.trigger('#request-button', 'htmx:abort')">
    Cancel Request
</button>
```

더 많은 예제와 자세한 내용은 [`hx-sync` attribute page.](@/attributes/hx-sync.md)에서 확인할 수 있습니다.

### CSS Transitions {#css_transitions}

htmx를 사용하면 자바스크립트 없이도 [CSS 트랜지션](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)을 쉽게 사용할 수 있습니다. 아래 HTML 콘텐츠를 살펴보세요:

```html
<div id="div1">Original Content</div>
```

이 콘텐츠가 htmx에 의해 ajax 요청을 통해 이 새 콘텐츠로 대체된다고 상상해 보세요:

```html
<div id="div1" class="red">New Content</div>
```

두 가지 사항에 유의하세요:

* 원본과 새 콘텐츠에 *동일한* ID를 가진 div가 있습니다.
* 새 콘텐츠에 `red` 클래스가 추가되었습니다.

이 상황을 고려하여 이전 상태에서 새 상태로의 CSS 트랜지션을 작성할 수 있습니다:

```css
.red {
    color: red;
    transition: all ease-in 1s ;
}
```

이 새 콘텐츠에서 htmx를 교체하면 새 콘텐츠에 CSS 트랜지션이 적용되어 
새 상태로 멋지고 부드럽게 전환됩니다.

요약하자면, 요소에 CSS 트랜지션을 사용하려면 요청에 따라 해당 `ID`를 안정적으로 유지하기만 하면 됩니다!

자세한 내용과 라이브 데모는 [Animation Examples](@/examples/animations.md)에서 확인할 수 있습니다.

#### Details

htmx에서 CSS 트랜지션이 실제로 어떻게 작동하는지 이해하려면 htmx에서 사용하는 기본 교체 및 정리 모델을 이해해야 합니다.

서버에서 새 콘텐츠를 수신하면 콘텐츠가 교체되기 전에 페이지의 기존 콘텐츠에서 `ID` 속성이 일치하는 요소가 있는지 검사합니다. 
새 콘텐츠의 요소와 일치하는 항목이 발견되면 교체가 발생하기 전에 이전 콘텐츠의 속성이 새 요소에 복사됩니다. 
그런 다음 새 콘텐츠가 *이전* 속성 값을 사용하며 교체됩니다. 마지막으로 '정리' 지연(기본값 20밀리초) 후 새 속성 값으로 바뀝니다. 
약간 이상하지만, 개발자가 자바스크립트를 사용하지 않고도 CSS 전환이 작동할 수 있는 이유입니다.

### Out of Band Swaps {#oob_swaps}

`id` 속성을 사용하여 응답의 콘텐츠를 DOM으로 직접 바꾸려면 
*응답* HTML에서 [hx-swap-oob](@/attributes/hx-swap-oob.md) 속성을 사용하면 됩니다.

```html
<div id="message" hx-swap-oob="true">Swap me directly!</div>
Additional Content
```

이 응답에서 `div#message`는 id가 일치하는 DOM 요소로 직접 교체되고 
추가 콘텐츠는 대상에 일반적인 방식으로 교체됩니다.

이 기술을 사용하여 다른 요청에 대한 업데이트를 "piggy-back"할 수 있습니다.

#### Troublesome Tables

테이블 요소는 HTML 사양에 따라 DOM에서 독립적으로 존재할 수 없는 경우가 많기 때문에 
out of band와 결합하면 문제가 될 수 있습니다(예: `<tr>` 또는 `<td>`).

이 문제를 방지하려면 `template` 태그를 사용하여 이러한 요소를 캡슐화하면 됩니다:

```html
<template>
  <tr id="message" hx-swap-oob="true"><td>Joe</td><td>Smith</td></tr>
</template>
```

#### Selecting Content To Swap

응답 HTML의 하위 집합만 선별해서 대상을 교체하려는 경우
CSS 선택자를 사용하여 응답에서 일치하는 요소를 선택하는 [hx-select](@/attributes/hx-select.md) 속성을 사용할 수 있습니다.

또한 out of band를 위해 선택 및 교체할 요소 ID 목록을 가져오는 
[hx-select-oob](@/attributes/hx-select-oob.md) 속성을 사용하여 콘텐츠 조각을 선택할 수도 있습니다.

#### Preserving Content During A Swap

교체가 발생하더라도 계속 재생하고 싶은 동영상 플레이어와 같이, 
교체가 발생하더라도 보존하려는 콘텐츠가 있는 경우 보존하려는 요소에는 
[hx-preserve](@/attributes/hx-preserve.md) 속성을 사용할 수 있습니다.

### Parameters

기본적으로 요청을 유발하는 요소에 value가 있는 경우 해당 값이 포함됩니다. 
요소가 form인 경우 그 안에 있는 모든 입력값이 포함됩니다. 

HTML 양식과 마찬가지로 input의 `name` 속성은 htmx가 보내는 요청에서 매개 변수 이름으로 사용됩니다. 

또한 요소가 비-`GET` 요청을 유발하는 경우 가장 가까운 둘러싸는 양식의 모든 입력값이 포함됩니다.

다른 요소의 값을 포함하려면 요청에 포함하려는 모든 요소의 CSS 선택자와 함께 [hx-include](@/attributes/hx-include.md) 속성을 사용하면 됩니다. 

일부 매개변수를 필터링하려면 [hx-params](@/attributes/hx-params.md) 속성을 사용하면 됩니다. 

마지막으로 프로그래밍 방식으로 매개변수를 수정하려면 [htmx:configRequest](@/events.md#htmx:configRequest) 이벤트를 사용할 수 있습니다.

#### File Upload {#files}

htmx 요청을 통해 파일을 업로드하려는 경우 [hx-encoding](@/attributes/hx-encoding.md) 속성을 `multipart/form-data`로 설정하면 됩니다. 
그러면 요청을 제출하는 데 `FormData` 객체가 사용되며 요청에 파일이 올바르게 포함됩니다. 

서버 측 기술에 따라 이러한 유형의 본문 콘텐츠가 포함된 요청을 매우 다르게 처리해야 할 수도 있습니다.

htmx는 업로드 중 표준 `progress` 이벤트에 따라 주기적으로 `htmx:xhr:progress` 이벤트를 실행하며, 이를 연결하여 업로드 진행률을 표시할 수 있습니다. 

[progress bars](@/examples/file-upload.md) 및 [오류 처리](@/examples/file-upload-input.md) 등 고급 양식 패턴에 대한 자세한 내용은 [examples section](@/examples/_index.md)을 참고하세요.

#### Extra Values

[hx-vals](@/attributes/hx-vals.md)(JSON 형식의 이름-표현식 쌍)을 사용하여 요청에 추가 값을 포함할 수 있습니다.

### Confirming Requests {#confirming}

요청을 발행하기 전에 작업을 확인하려는 경우가 종종 있습니다. 
htmx는 간단한 자바스크립트 대화 상자를 사용하여 작업을 확인할 수 있는 [`hx-confirm`](@/attributes/hx-confirm.md) 속성을 지원합니다:

```html
<button hx-delete="/account" hx-confirm="Are you sure you wish to delete your account?">
    Delete My Account
</button>
```

이벤트를 사용하면 보다 정교한 확인 대화 상자를 구현할 수 있습니다. [confirm example](@/examples/confirm.md)에서는 
[sweetalert2](https://sweetalert2.github.io/) 라이브러리를 사용하여 htmx 작업을 확인하는 방법을 보여줍니다.

#### Confirming Requests Using Events

확인을 수행하는 또 다른 옵션은 [`htmx:confirm` event](@/events.md#htmx:confirm) 이벤트를 사용하는 것입니다. 
이 이벤트는 요청에 대한 *모든* 트리거(`hx-confirm` 속성이 있는 요소뿐만 아니라)에서 
실행되며 요청의 비동기 확인을 구현하는 데 사용할 수 있습니다.

다음은 `confirm-with-sweet-alert='true'` 속성이 있는 모든 요소에 [sweet alert](https://sweetalert.js.org/guides/)를 사용하는 예시입니다:

```javascript
document.body.addEventListener('htmx:confirm', function(evt) {
  if (evt.target.matches("[confirm-with-sweet-alert='true']")) {
    evt.preventDefault();
    swal({
      title: "Are you sure?",
      text: "Are you sure you are sure?",
      icon: "warning",
      buttons: true,
      dangerMode: true,
    }).then((confirmed) => {
      if (confirmed) {
        evt.detail.issueRequest();
      }
    });     
  }
});
```


## Attribute Inheritance {#inheritance}

htmx의 대부분의 속성은 상속되며, 해당 속성이 있는 요소와 모든 하위 요소에 적용됩니다. 
이를 통해 속성을 DOM 위로 '올리기'해서 코드 중복을 피할 수 있습니다. 다음 htmx를 고려해 보세요:

```html
<button hx-delete="/account" hx-confirm="Are you sure?">
    Delete My Account
</button>
<button hx-put="/account" hx-confirm="Are you sure?">
    Update My Account
</button>
```

여기에는 중복된 `hx-confirm` 속성이 있습니다. 이 속성을 부모 요소로 올릴 수 있습니다:

```html
<div hx-confirm="Are you sure?">
    <button hx-delete="/account">
        Delete My Account
    </button>
    <button hx-put="/account">
        Update My Account
    </button>
</div>
```

이 `hx-confirm` 속성은 이제 그 안의 모든 htmx 기반 요소에 적용됩니다.

때때로 이 상속을 취소하고 싶을 때가 있습니다. 이 그룹에 취소 버튼이 있지만 확인을 원하지 않는 경우를 생각해 보세요. 
다음과 같이 `unset` 지시문을 추가할 수 있습니다:

```html
<div hx-confirm="Are you sure?">
    <button hx-delete="/account">
        Delete My Account
    </button>
    <button hx-put="/account">
        Update My Account
    </button>
    <button hx-confirm="unset" hx-get="/">
        Cancel
    </button>
</div>
```

그러면 상단 두 개의 버튼에는 확인 대화 상자가 표시되지만 하단 취소 버튼은 표시되지 않습니다.

상속은 [`hx-disinherit`](@/attributes/hx-disinherit.md) 속성을 사용하여 요소별 및 속성별로 비활성화할 수 있습니다.

속성 상속을 완전히 비활성화하려면 `htmx.config.disableInheritance` 구성 변수를 `true`로 설정하면 됩니다. 
이렇게 하면 기본적으로 상속이 비활성화되고 [`hx-inherit`](@/attributes/hx-inherit.md) 속성을 사용하여 
상속을 명시적으로 지정할 수 있습니다.

## Boosting

Htmx는 [hx-boost](@/attributes/hx-boost.md) 속성을 사용하여 일반 HTML anchor 및 form의 "boosting"을 지원합니다.
이 속성은 모든 anchor 태그와 form을 기본적으로 웹 페이지의 본문을 대상으로 하는 AJAX 요청으로 변환합니다.

여기 예시가 있습니다:

```html
<div hx-boost="true">
    <a href="/blog">Blog</a>
</div>
```

이 div 안의 앵커 태그는 `/blog`로 AJAX `GET` 요청을 보내고, 응답으로 `body` 태그 안을 교체합니다.


### Progressive Enhancement {#progressive_enhancement}

`hx-boost`의 기능 중 하나는 JavaScript가 활성화되지 않은 경우에도 점진적으로 저하된다는 것입니다: 
link와 form은 계속 작동하지만, 단지 Ajax 요청을 사용하지 않습니다. 
이를 "[점진적 향상(Progressive Enhancement)](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)"이라고 하며, 
이를 통해 더 넓은 사용자층이 사이트의 기능을 이용할 수 있게 합니다.

다른 htmx 패턴들도 점진적 향상을 달성하도록 조정할 수 있지만, 이는 더 많은 고민을 필요로 합니다.

[active search](@/examples/active-search.md) 예시를 생각해 보세요. 자바스크립트를 활성화하지 않은 사람은 이 기능을 사용할 수 없습니다. 
이 예제는 최대한 간략하게 작성하기 위해 그런 것입니다.

그러나 htmx로 강화된 input 요소를 form 요소로 감쌀 수 있습니다:

```html
<form action="/search" method="POST">
    <input class="form-control" type="search"
        name="search" placeholder="Begin typing to search users..."
        hx-post="/search"
        hx-trigger="keyup changed delay:500ms, search"
        hx-target="#search-results"
        hx-indicator=".htmx-indicator">
</form>
```

이렇게 하면 자바스크립트를 사용하는 클라이언트는 여전히 멋진 액티브 검색 UX를 사용할 수 있지만, 
자바스크립트를 사용하지 않는 클라이언트도 엔터 키를 눌러 검색할 수 있습니다. 더 좋은 방법은 '검색' 버튼도 추가하는 것입니다. 
그런 다음 `action` 속성을 미러링한 `hx-post`로 form을 업데이트하거나 `hx-boost`를 사용해야 합니다. 

서버 측에서 `HX-Request` 헤더를 확인하여 htmx 기반 요청과 일반 요청을 구분하여 클라이언트에 렌더링할 내용을 정확히 결정해야 합니다. 

애플리케이션의 점진적인 개선 요구를 달성하기 위해 다른 패턴도 유사하게 적용할 수 있습니다. 보시다시피, 더 많은 생각과 더 많은 작업이 필요합니다. 
또한 일부 기능은 완전히 제한될 수도 있습니다. 이러한 절충점은 개발자가 프로젝트 목표와 대상에 따라 결정해야 합니다. 

[접근성](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/What_is_accessibility)은 점진적 향상과 밀접한 관련이 있는 개념입니다. 
`hx-boost`와 같은 점진적 향상 기술을 사용하면 다양한 사용자가 htmx 애플리케이션에 더 쉽게 액세스할 수 있습니다. 

htmx 기반 애플리케이션은 HTML 지향적이기 때문에 일반적인 비AJAX 구동 웹 애플리케이션과 매우 유사합니다.

따라서 일반적인 HTML 접근성 권장 사항이 권장됩니다. 예를 들어:

* 가능한 한 시맨틱 HTML을 사용하세요(즉, 적합한 태그에 적합한 태그 사용).
* 초점 상태가 명확하게 표시되는지 확인
* text label을 모든 form 필드와 연결
* 적절한 글꼴, 대비 등으로 애플리케이션의 가독성을 극대화하세요.

## Web Sockets & SSE {#websockets-and-sse}

웹소켓과 서버 전송 이벤트(SSE)는 확장 프로그램을 통해 지원됩니다. 
자세한 내용은 [SSE 확장](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/sse/README.md) 프로그램 및
[WebSocket 확장](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/ws/README.md) 프로그램 페이지를 참조하세요.

## History Support {#history}

Htmx는 [browser history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)와 상호 작용할 수 있는 간단한 메커니즘을 제공합니다:

지정된 요소가 브라우저 탐색 모음에 요청 URL을 푸시하고 페이지의 현재 상태를 
브라우저 히스토리에 추가하려면 [hx-push-url](@/attributes/hx-push-url.md)속성을 포함하세요:

```html
<a hx-get="/blog" hx-push-url="true">Blog</a>
```

사용자가 이 링크를 클릭하면, htmx는 /blog로 요청을 보내기 전에 현재 DOM을 스냅샷으로 저장합니다. 
그런 다음 교체 작업을 수행하고 새로운 위치를 히스토리 스택에 추가합니다.

사용자가 뒤로 가기 버튼을 누르면, htmx는 저장된 이전 콘텐츠를 가져와 대상에 다시 교체하여 이전 상태로 "돌아가는" 것을 시뮬레이션합니다. 
위치가 캐시에 없으면, htmx는 `HX-History-Restore-Request` 헤더를 true로 설정하여 
주어진 URL로 ajax 요청을 보내며, 전체 페이지에 필요한 HTML을 기대합니다. 
또는, `htmx.config.refreshOnHistoryMiss` 구성 변수가 true로 설정된 경우, 브라우저를 강제로 새로 고침합니다.

**NOTE:**: URL을 히스토리에 추가하면 해당 URL로 이동하여 전체 페이지를 **다시 볼 수 있어야 합니다**! 
사용자가 URL을 복사하여 이메일이나 새 탭에 붙여넣을 수 있습니다. 
또한, 페이지가 히스토리 캐시에 없으면 htmx가 히스토리를 복원할 때 전체 페이지가 필요합니다.

### Specifying History Snapshot Element

기본적으로 htmx는 본문을 사용하여 히스토리 스냅샷을 만들고 복원합니다.
일반적으로 이 방법이 맞지만 더 좁은 요소를 스냅샷에 사용하려면 
[hx-history-elt](@/attributes/hx-history-elt.md) 속성을 사용하여 다른 요소를 지정할 수 있습니다.

주의: 이 요소는 모든 페이지에 있어야 하며 그렇지 않으면 히스토리에서 복원하는 것이 안정적으로 작동하지 않습니다.

### Undoing DOM Mutations By 3rd Party Libraries

타사 라이브러리를 사용 중이고 htmx 히스토리 기능을 사용하려는 경우 스냅샷을 찍기 전에 DOM을 정리해야 합니다. 
선택한 요소를 훨씬 더 풍부한 사용자 경험으로 만들어주는 [Tom Select](https://tom-select.js.org/) 라이브러리를 살펴봅시다. 
`.tomselect` 클래스가 있는 모든 입력 요소를 rich 선택 요소로 바꾸도록 TomSelect를 설정해 보겠습니다.

먼저 새 콘텐츠에 클래스가 있는 요소를 초기화해야 합니다:

```javascript
htmx.onLoad(function (target) {
    // find all elements in the new content that should be
    // an editor and init w/ TomSelect
    var editors = target.querySelectorAll(".tomselect")
            .forEach(elt => new TomSelect(elt))
});
```

이렇게 하면 `.tomselect` 클래스가 있는 모든 입력 요소에 대한 rich 선택기가 생성됩니다. 
그러나 히스토리 콘텐츠가 화면에 다시 로드될 때 TomSelect가 다시 초기화되므로 
DOM을 변경하고 기록 캐시에 해당 변경 사항을 저장하지 않으려는 것입니다.

이 문제를 해결하려면 `htmx:beforeHistorySave` 이벤트를 포착하고 
`destroy()`를 호출하여 TomSelect 돌연변이를 정리해야 합니다:

```javascript
htmx.on('htmx:beforeHistorySave', function() {
    // find all TomSelect elements
    document.querySelectorAll('.tomSelect')
            .forEach(elt => elt.tomselect.destroy()) // and call destroy() on them
})
```

이렇게 하면 DOM이 원래 HTML로 되돌아가므로 깨끗한 스냅샷을 만들 수 있습니다.

### Disabling History Snapshots

기록 스냅샷은 현재 문서의 모든 요소 또는 현재 문서에 htmx로 로드된 html 조각에서 [hx-history](@/attributes/hx-history.md) 속성을 `false`로 설정하여 
URL에 대한 히스토리 스냅샷을 비활성화할 수 있습니다. 이렇게 하면 민감한 데이터가 localStorage 캐시에 들어가는 것을 방지할 수 있으며, 
이는 공유 사용/공용 컴퓨터에서 중요할 수 있습니다. 기록 탐색은 예상대로 작동하지만 복원 시 로컬 기록 캐시 대신 
서버에서 URL을 요청합니다.

## Requests &amp; Responses {#requests}

Htmx는 AJAX 요청에 대한 응답이 HTML(일반적으로 HTML 조각)이 될 것으로 예상합니다
([hx-select](@/attributes/hx-select.md) 태그와 일치하는 전체 HTML 문서도 유용할 수 있음). 그런 다음 Htmx는 
반환된 HTML을 지정된 대상과 지정된 교체 전략을 사용하여 문서로 교체합니다.

때로는 교체 단계에서 아무것도 하지 않으면서도 클라이언트 측 이벤트를 트리거하고 싶을 수도 있습니다([아래 참조](#response-headers)).

이 상황에서는 기본적으로 `204 - No Content` 응답 코드를 반환할 수 있으며, 이 경우 htmx는 응답으로 온 콘텐츠를 무시합니다.

서버에서 오류 응답(예: 404 또는 501)이 발생하면 htmx는 개발자가 처리할 수 있는 [`htmx:responseError`](@/events.md#htmx:responseError) 이벤트를 트리거합니다.

연결 오류가 발생하면 `htmx:sendError` 이벤트가 트리거됩니다.

### Configuring Response Handling {#response-handling}

`htmx.config.responseHandling` 배열을 변경하거나 대체하여 위의 htmx 동작을 구성할 수 있습니다. 
이 객체는 이렇게 정의된 JavaScript 객체의 모음입니다:

```js
    responseHandling: [
        {code:"204", swap: false},   // 204 - No Content by default does nothing, but is not an error
        {code:"[23]..", swap: true}, // 200 & 300 responses are non-errors and are swapped
        {code:"[45]..", swap: false, error:true}, // 400 & 500 responses are not swapped and are errors
    ]
```

htmx가 응답을 받으면 `htmx.config.responseHandling` 배열을 순서대로 반복하고 
지정된 객체의 `code` 속성이 정규식으로 처리될 때 현재 응답과 일치하는지 테스트합니다. 
항목이 현재 응답 코드와 일치하면 응답을 처리할지 여부와 방법을 결정하는 데 사용됩니다.

이 배열의 항목에 대한 응답 처리 구성에 사용할 수 있는 필드는 다음과 같습니다:

* `code` - 응답 코드에 대해 테스트할 정규식을 나타내는 문자열입니다.
* `swap` - 응답을 DOM 안에서 교체해야 하는 경우 `true`, 그렇지 않으면 `false`입니다.
* `error` - 이 응답을 오류로 처리해야 하는 경우 `true`입니다.
* `ignoreTitle` - 응답에서 title 태그를 무시해야 하는 경우 `true`입니다.
* `select` - 응답에서 콘텐츠를 선택하는 데 사용할 CSS 선택자
* `target` - 응답의 대체 대상을 지정하는 CSS 선택자
* `swapOverride` - 응답을 위한 대체 교체 메커니즘

#### Configuring Response Handling Examples {#response-handling}

이 구성을 사용하는 방법의 예로 유효성 검사 오류가 발생했을 때 서버 측 프레임워크가 
[`422 - Unprocessable Entity`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422) 응답으로 응답하는 상황을 생각해 보겠습니다. 
기본적으로 htmx는 정규식 `[45]..`과 일치하므로 이 응답을 무시합니다.

응답 처리 구성을 위한 [meta config](#configuration-options) 메커니즘을 사용하여 다음과 같은 구성을 
추가할 수 있습니다:

```html
<meta name="htmx-config" content='{code:"204", swap: false},   // 204 - No Content by default does nothing, but is not an error
                                  {code:"[23]..", swap: true}, // 200 & 300 responses are non-errors and are swapped
                                  {code:"422", swap: true}, // 422 responses are swapped
                                  {code:"[45]..", swap: false, error:true}, // 400 & 500 responses are not swapped and are errors'>
```

HTTP 응답 코드에 관계없이 모든 것을 교체하려면 이 구성을 사용할 수 있습니다:

```html
<meta name="htmx-config" content='{code:".*", swap: true}, // all responses are swapped'>
```

마지막으로 속성을 통해 응답 코드의 동작을 선언적으로 구성할 수 있는 
[Response Targets](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/response-targets/README.md) 확장 기능을 사용하는 것도 고려해 볼 만합니다.

### CORS

cross origin context에서 htmx를 사용하는 경우 클라이언트 측에서 
htmx 헤더를 볼 수 있도록 웹 서버에서 Access-Control 헤더를 설정하도록 
구성하는 것을 잊지 마세요.

- [Access-Control-Allow-Headers (for request headers)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)
- [Access-Control-Expose-Headers (for response headers)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)

[htmx에 구현된 모든 요청 및 응답 헤더를 확인하세요.](@/reference.md#request_headers)

### Request Headers

htmx에는 요청에 유용한 헤더가 많이 포함되어 있습니다:

| Header | Description |
|--------|-------------|
| `HX-Boosted` | 요청이 [hx-boost](@/attributes/hx-boost.md)를 사용하는 요소를 통해 이루어졌음을 나타냅니다.
| `HX-Current-URL` | 브라우저의 현재 URL
| `HX-History-Restore-Request` | 로컬 히스토리 캐시 누락 후 히스토리 복원 요청인 경우 "true"입니다.
| `HX-Prompt` | [hx-prompt](@/attributes/hx-prompt.md)에 대한 사용자 응답.
| `HX-Request` | 항상 "true"
| `HX-Target` | 대상 요소가 있는 경우 그것의 `id`
| `HX-Trigger-Name` | 트리거된 요소가 존재하는 경우 그것의 `name`
| `HX-Trigger` | 트리거된 요소가 존재하는 경우 그것의 `id`

### Response Headers

htmx는 일부 htmx 전용 응답 헤더를 지원합니다:

* [`HX-Location`](@/headers/hx-location.md) - 전체 페이지를 새로 고침하지 않고 클라이언트 측에서 리디렉션을 수행할 수 있습니다
* [`HX-Push-Url`](@/headers/hx-push-url.md) - 새 URL을 히스토리 스택에 추가합니다
* `HX-Redirect` - 클라이언트 측에서 새로운 위치로 리디렉션하는 데 사용할 수 있습니다
* `HX-Refresh` - "true"로 설정되면 클라이언트 측에서 페이지를 전체 새로 고침합니다
* [`HX-Replace-Url`](@/headers/hx-replace-url.md) - 위치 표시줄의 현재 URL을 대체합니다
* `HX-Reswap` - 응답이 어떻게 교체될지를 지정할 수 있습니다. 가능한 값은 [hx-swap](@/attributes/hx-swap.md)을 참조하세요
* `HX-Retarget` - 콘텐츠 업데이트의 대상을 페이지의 다른 요소로 업데이트하는 CSS 선택자입니다
* `HX-Reselect` - 응답의 어느 부분을 교체할지 선택할 수 있는 CSS 선택자입니다. 트리거 요소에 존재하는 [`hx-select`](@/attributes/hx-select.md)을 재정의합니다
* [`HX-Trigger`](@/headers/hx-trigger.md) - 클라이언트 측 이벤트를 트리거할 수 있습니다
* [`HX-Trigger-After-Settle`](@/headers/hx-trigger.md) - 정착 단계 후에 클라이언트 측 이벤트를 트리거할 수 있습니다
* [`HX-Trigger-After-Swap`](@/headers/hx-trigger.md) - 교체 단계 후에 클라이언트 측 이벤트를 트리거할 수 있습니다

`HX-Trigger` 헤더에 대한 자세한 내용은 [`HX-Trigger` Response Headers](@/headers/hx-trigger.md)를 참조하세요.

htmx를 통해 양식을 제출하면 더 이상 [Post/Redirect/Get Pattern](https://en.wikipedia.org/wiki/Post/Redirect/Get)이 필요하지 않다는 이점이 있습니다. 
서버에서 POST 요청을 성공적으로 처리한 후에는 [HTTP 302 (Redirect)](https://en.wikipedia.org/wiki/HTTP_302)를 반환할 필요가 없습니다. 새 HTML 조각을 직접 반환할 수 있습니다.

### Request Order of Operations {#request-operations}

htmx 요청에서 작업 순서는 다음과 같습니다:

* 요소가 트리거되고 요청을 시작합니다
  * 요청에 필요한 값들이 수집됩니다
  * `htmx-request` 클래스가 적절한 요소에 적용됩니다
  * 요청이 AJAX를 통해 비동기로 보내집니다
    * 응답을 받으면 대상 요소에 `htmx-swapping` 클래스가 표시됩니다
    * 선택적으로 교체 지연이 적용됩니다 (자세한 내용은 [hx-swap](@/attributes/hx-swap.md) 속성을 참조하세요)
    * 실제 콘텐츠 교체가 이루어집니다
        * 대상에서 `htmx-swapping` 클래스가 제거됩니다
        * 각 새 콘텐츠 조각에 `htmx-added` 클래스가 추가됩니다
        * 대상에 `htmx-settling` 클래스가 적용됩니다
        * 정착 지연이 이루어집니다 (기본값: 20ms)
        * DOM이 정착됩니다
        * 대상에서 `htmx-settling` 클래스가 제거됩니다
        * 각 새 콘텐츠 조각에서 `htmx-added` 클래스가 제거됩니다

`htmx-swapping` 및 `htmx-settling` 클래스를 사용하여 페이지 간 
[CSS transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)을 생성할 수 있습니다.

## Validation

Htmx는 [HTML5 유효성 검사](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)와 통합되어 유효성 검사 가능한 입력이 
유효하지 않은 경우 form 요청을 보내지 않습니다. 
이는 AJAX 요청과 WebSocket 전송 모두에 해당됩니다.

Htmx는 사용자 정의 유효성 검사 및 오류 처리를 연결하는 데 사용할 수 있는 유효성 검사 관련 이벤트를 실행합니다:

* `htmx:validation:validate` - 요소의 `checkValidity()` 메서드가 호출되기 전에 호출됩니다. 
사용자 정의 유효성 검사 로직을 추가하는 데 사용할 수 있습니다.
* `htmx:validation:failed` - `checkValidity()`가 잘못된 입력을 나타내는 false를 반환할 때 호출됩니다.
* `htmx:validation:halted` - 유효성 검사 오류로 인해 요청이 보내지지 않을 때 호출됩니다. 
특정 오류는 `event.detail.errors` 객체에서 찾을 수 있습니다.

Non-form 요소는 기본적으로 요청을 하기 전에 유효성 검사를 수행하지 않지만, 
[`hx-validate`](@/attributes/hx-validate.md) 속성을 "true"로 설정하여 유효성 검사를 활성화할 수 있습니다.

### Validation Example

다음은 [`hx-on`](/attributes/hx-on) 속성을 사용하여 `htmx:validation:validate` 이벤트를 포착하고 
입력값이 `foo`여야 하는 입력의 예입니다:

```html
<form id="example-form" hx-post="/test">
    <input name="example"
           onkeyup="this.setCustomValidity('') // reset the validation on keyup"
           hx-on:htmx:validation:validate="if(this.value != 'foo') {
                    this.setCustomValidity('Please enter the value foo') // set the validation error
                    htmx.find('#example-form').reportValidity()          // report the issue
                }">
</form>
```

Note 모든 클라이언트 측 유효성 검사는 항상 우회할 수 있으므로 서버 측에서 다시 수행해야 합니다.

## Animations

HTML과 CSS만 사용하여 다양한 상황에서 
[CSS 전환](#css_transitions)을 사용할 수 있습니다. 

사용 가능한 옵션에 대한 자세한 내용은 [Animation Guide](@/examples/animations.md) 가이드를 참조하세요.

## Extensions

Htmx에는 라이브러리의 동작을 사용자 정의할 수 있는 확장 메커니즘이 있습니다. 
확장은 [자바스크립트에서 정의한](https://github.com/bigskysoftware/htmx-extensions/tree/main?tab=readme-ov-file#defining-an-extension) 다음 
[`hx-ext`](@/attributes/hx-ext.md) 속성을 통해 사용합니다:

```html
<div hx-ext="debug">
    <button hx-post="/example">This button used the debug extension</button>
    <button hx-post="/example" hx-ext="ignore:debug">This button does not</button>
</div>
```

htmx에 자신만의 확장 기능을 추가하려면 [extension docs](https://github.com/bigskysoftware/htmx-extensions/tree/main?tab=readme-ov-file#defining-an-extension)를 참조하세요.

## Events & Logging {#events}

Htmx에는 로깅 시스템을 겸하는 광범위한 [events mechanism](@/reference.md#events)이 있습니다.

특정 htmx 이벤트에 등록하려면 다음을 사용할 수 있습니다.

```js
document.body.addEventListener('htmx:load', function(evt) {
    myJavascriptLib.init(evt.detail.elt);
});
```

원하는 경우 다음 htmx 도우미를 사용할 수 있습니다:

```javascript
htmx.on("htmx:load", function(evt) {
    myJavascriptLib.init(evt.detail.elt);
});
```

`htmx:load` 이벤트는 htmx에 의해 요소가 DOM에 로드될 때마다 발생하며, 
사실상 일반 `load` 이벤트와 동일합니다. 

htmx 이벤트의 일반적인 용도는 다음과 같습니다:

### Initialize A 3rd Party Library With Events {#init_3rd_party_with_events}

콘텐츠를 초기화하기 위해 `htmx:load` 이벤트를 사용하는 것은 매우 일반적이기 때문에 htmx에서 도우미 기능을 제공합니다:

```javascript
htmx.onLoad(function(target) {
    myJavascriptLib.init(target);
});
```
이것은 첫 번째 예제와 동일한 작업을 수행하지만 조금 더 깔끔합니다.

### Configure a Request With Events {#config_request_with_events}

AJAX 요청이 발행되기 전에 수정하기 위해 [`htmx:configRequest`](@/events.md#htmx:configRequest) 이벤트를 처리할 수 있습니다:

```javascript
document.body.addEventListener('htmx:configRequest', function(evt) {
    evt.detail.parameters['auth_token'] = getAuthToken(); // 요청에 새 매개변수를 추가합니다.
    evt.detail.headers['Authentication-Token'] = getAuthToken(); // 요청에 새 헤더를 추가합니다.
});
```

여기서는 요청이 전송되기 전에 매개변수와 헤더를 요청에 추가합니다.

### Modifying Swapping Behavior With Events {#modifying_swapping_behavior_with_events}

htmx의 교체 동작을 수정하기 위해 [`htmx:beforeSwap`](@/events.md#htmx:beforeSwap) 이벤트를 처리할 수도 있습니다:

```javascript
document.body.addEventListener('htmx:beforeSwap', function(evt) {
    if(evt.detail.xhr.status === 404){
        // 404가 발생하면 사용자에게 경고합니다(alert()보다 더 좋은 메커니즘을 사용할 수도 있습니다).
        alert("Error: Could Not Find Resource");
    } else if(evt.detail.xhr.status === 422){
        // 잘못된 데이터로 양식이 제출되었다는 신호로 사용하므로
        // 사용하므로 422 응답이 교체되도록 허용하고
        // 오류를 수정하여 다시 렌더링합니다.
        //
        // 콘솔에서 오류 로깅을 방지하려면 isError를 false로 설정합니다.
        evt.detail.shouldSwap = true;
        evt.detail.isError = false;
    } else if(evt.detail.xhr.status === 418){
        // 응답 코드 418(나는 찻주전자다)이 반환되면, 응답의 내용을 리타겟팅하여 
        // 응답의 내용을 `teapot` ID를 가진 요소로 리타겟팅합니다.
        evt.detail.shouldSwap = true;
        evt.detail.target = htmx.find("#teapot");
    }
});
```

여기서는 일반적으로 htmx에서 교체를 수행하지 않는 
몇 가지 [400-level 에러 응답 코드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses)를 처리합니다.

### Event Naming {#event_naming}

모든 이벤트는 두 가지 다른 이름으로 실행됩니다.

* Camel Case
* Kebab Case

예를 들어 `htmx:afterSwap` 또는 `htmx:after-swap`을 모두 사용할 수 있습니다. 이렇게 하면 다른 라이브러리와의 
상호 운용성이 용이해집니다. 예를 들어 [Alpine.js](https://github.com/alpinejs/alpine/)에는 Kebab Case가 필요합니다.

### Logging

`htmx.logger`에서 logger를 설정하면 모든 이벤트가 기록됩니다. 이는 문제 해결에 매우 유용할 수 있습니다:

```javascript
htmx.logger = function(elt, event, data) {
    if(console) {
        console.log(event, elt, data);
    }
}
```

## Debugging

htmx(또는 다른 선언적 언어)를 사용한 선언적 및 이벤트 중심 프로그래밍은 매우 생산적이고 멋진 활동이 될 수 있지만, 
명령형 접근 방식과 비교했을 때 한 가지 단점은 디버깅이 더 까다로울 수 있다는 점입니다.

예를 들어, 어떤 일이 발생하지 *않는* 이유를 알아내는 것은 트릭을 모르면 어려울 수 있습니다.

여기 그 트릭들이 있습니다:

사용할 수 있는 첫 번째 디버깅 도구는 `htmx.logAll()` 메서드입니다. 이 메서드는 htmx가 트리거하는 모든 이벤트를 기록하며, 
라이브러리가 정확히 무엇을 하고 있는지 볼 수 있게 해줍니다.

```javascript
htmx.logAll();
```

물론 그렇다고 해서 htmx가 무언가를 수행하지 않는 이유를 알 수는 없습니다. 
또한 DOM 요소가 트리거로 사용하기 위해 어떤 이벤트를 발생시키는지 모를 수도 있습니다. 
이 문제를 해결하려면 브라우저 콘솔에서 [`monitorEvents()`](https://developers.google.com/web/updates/2015/05/quickly-monitor-events-from-the-console-panel) 메서드를 사용할 수 있습니다:

```javascript
monitorEvents(htmx.find("#theElement"));
```

이렇게 하면 id가 `theElement`인 요소에서 발생하는 모든 이벤트가 콘솔로 뱉어져 어떤 일이 일어나고 있는지 정확히 확인할 수 있습니다. 

이 기능은 `오직` 콘솔에서만 작동하며 페이지의 script 태그에 삽입할 수는 없습니다. 

마지막으로, 최소화되지 않은 버전을 로드하여 `htmx.js`를 디버그할 수 있습니다. 
약 2500줄의 자바스크립트이므로 극복할 수 없는 양의 코드는 아닙니다. 
`issueAjaxRequest()` 및 `handleAjaxResponse()` 메서드에 중단점을 설정하여 무슨 일이 일어나고 있는지 
확인하는 것이 가장 좋습니다. 도움이 필요하면 언제든지 [Discord](https://htmx.org/discord)로 문의해 주세요.

### Creating Demos

때로는 버그를 시연하거나 사용법을 명확히 설명하기 위해 [jsfiddle](https://jsfiddle.net/)과 같은 자바스크립트 스니펫 사이트를 사용할 수 있으면 좋습니다.
데모를 쉽게 생성할 수 있도록 htmx는 다음을 설치할 데모 스크립트 사이트를 호스팅합니다.

* htmx
* hyperscript
* a request mocking library

demo/fiddle/whatever 등에 다음 스크립트 태그를 추가하기만 하면 됩니다::

```html
<script src="https://demo.htmx.org"></script>
```

이 도우미를 사용하면 URL 속성이 있는 `template` 태그를 추가하여 어떤 `URL`을 나타내는 모의 응답을 추가할 수 있습니다. 
해당 URL에 대한 응답은 템플릿의 내부 HTML이 되므로 모의 응답을 쉽게 구성할 수 있습니다.
`delay` 속성을 사용하여 응답에 지연을 추가할 수 있으며, 지연할 밀리초 수를 나타내는 정수여야 합니다. 

템플릿에 `${}` 구문을 사용하여 간단한 표현식을 포함할 수 있습니다. 

이는 데모용으로만 사용해야 하며 항상 최신 버전인 htmx 및 하이퍼스크립트를 가져오므로 안정적인 작동이 보장되지 않는다는 점에 유의하세요!

#### Demo Example

다음은 실제로 작동하는 코드의 예입니다:

```html
<!-- load demo environment -->
<script src="https://demo.htmx.org"></script>

<!-- post to /foo -->
<button hx-post="/foo" hx-target="#result">
    Count Up
</button>
<output id="result"></output>

<!-- respond to /foo with some dynamic content in a template tag -->
<script>
    globalInt = 0;
</script>
<template url="/foo" delay="500"> <!-- note the url and delay attributes -->
    ${globalInt++}
</template>

```

## Scripting {#scripting}

htmx는 웹 애플리케이션 구축에 하이퍼미디어 접근 방식을 권장하지만 클라이언트 스크립팅을 위한 다양한 옵션을 제공합니다. 
스크립팅은 웹 아키텍처에 대한 REST 풀 설명에 포함되어 있습니다: [Code-On-Demand](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1_7)를 보세요. 가능한 한 웹 애플리케이션에서 스크립팅에 [하이퍼미디어 친화적인](/essays/hypermedia-friendly-scripting) 접근 방식을 사용하는 것이 좋습니다:

* [Respect HATEOAS](/essays/hypermedia-friendly-scripting#prime_directive)
* [이벤트를 사용하여 컴포넌트 간 통신](/essays/hypermedia-friendly-scripting#events)
* [islands를 사용하여 애플리케이션의 나머지 부분에서 하이퍼미디어가 아닌 컴포넌트를 분리하기](/essays/hypermedia-friendly-scripting#islands)
* [Consider inline scripting](/essays/hypermedia-friendly-scripting#inline)

htmx와 스크립팅 솔루션 간의 주요 통합 지점은 htmx가 전송하고 응답할 수 있는 [events](#events)입니다. 
이벤트를 통해 JavaScript 라이브러리를 htmx와 통합하기 위한 좋은 템플릿은
[3rd Party Javascript](#3rd-party) 섹션의 SortableJS 예제를 참조하세요.

htmx와 잘 어울리는 스크립팅 솔루션은 다음과 같습니다:

* [VanillaJS](http://vanilla-js.com/) - JavaScript에 내장된 기능을 사용하여 이벤트 핸들러를 연결하여 htmx가 내보내는 이벤트에 응답하는 것만으로도 
스크립팅에 매우 효과적일 수 있습니다. 이는 매우 가볍고 점점 더 많이 사용되는 접근 방식입니다.
* [AlpineJS](https://alpinejs.dev/) - Alpine.js는 반응형 프로그래밍 지원을 포함하여 정교한 프런트엔드 스크립트를 작성할 수 있는 다양한 도구를 제공하면서도 
매우 가벼운 무게를 유지합니다. Alpine은 "inline scripting" 접근 방식을 권장하며, 이는 htmx와 잘 어울린다고 생각합니다.
* [jQuery](https://jquery.com/) - 일부 분야에서의 오랜 세월과 명성에도 불구하고 jQuery는 htmx와 매우 잘 어울립니다.
특히 이미 jQuery가 많이 포함된 오래된 코드베이스에서는 htmx와 매우 잘 어울립니다.
* [hyperscript](https://hyperscript.org) - hyperscript는 htmx를 만든 팀이 만든 실험적인 프런트엔드 스크립팅 언어입니다. 
HTML에 잘 삽입되고 이벤트에 응답하고 이벤트를 생성하도록 설계되었으며 htmx와 매우 잘 어울립니다.

[우리의 책](https://hypermedia.systems)에는 ["Client-Side Scripting"](https://hypermedia.systems/client-side-scripting/)이라는 제목의 전체 챕터가 있으며, 
스크립팅을 htmx 기반 애플리케이션에 통합하는 방법을 살펴봅니다.

### <a name="hx-on"></a>[The `hx-on*` Attributes](#hx-on)

HTML에서는 `onClick`과 같은 [`onevent` properties](https://developer.mozilla.org/en-US/docs/Web/Events/Event_handlers#using_onevent_properties)을 통해 
인라인 스크립트를 삽입할 수 있습니다:

```html
<button onclick="alert('You clicked me!')">
    Click Me!
</button>
```

이 기능을 사용하면 스크립팅 로직을 해당 로직이 적용되는 HTML 요소와 함께 배치할 수 있어 우수한 [Locality of Behaviour (LoB)](/essays/locality-of-behaviour)을 제공합니다. 
안타깝게도 HTML은 정해진 수의 [특정 DOM 이벤트](https://www.w3schools.com/tags/ref_eventattributes.asp)(예: `onclick`)에 대해서만 `on*` 속성을 허용하고 요소의 임의 이벤트에 응답하는 
일반화된 메커니즘을 제공하지 않습니다. 

이 단점을 해결하기 위해 htmx는 [`hx-on*`](/attributes/hx-on) 속성을 제공합니다. 이러한 속성을 사용하면 표준 `on*` 속성의 LoB를 
보존하는 방식으로 모든 이벤트에 응답할 수 있습니다. 

`hx-on` 속성을 사용하여 `click` 이벤트에 응답하려면 다음과 같이 작성하면 됩니다:

```html
<button hx-on:click="alert('You clicked me!')">
    Click Me!
</button>
```

즉, 문자열 `hx-on` 다음에 콜론(또는 대시)을 붙인 다음 이벤트 이름을 붙이면 됩니다.

물론 `click` 이벤트의 경우 표준 `onclick` 속성을 사용하는 것이 좋습니다. 그러나 `htmx:config-request` 이벤트를 
사용하여 요청에 매개 변수를 추가하려는 htmx 기반 버튼을 생각해 보세요. 이 작업은 표준 `on*` 속성을 사용하면 불가능하지만 
`hx-on:htmx:config-request` 속성을 사용하면 가능합니다:

```html
<button hx-post="/example"
        hx-on:htmx:config-request="event.detail.parameters.example = 'Hello Scripting!'">
    Post Me!
</button>
```

여기서 `example` 매개변수는 'Hello Scripting!' 값과 함께 `POST` 요청이 보내지기 전에 추가됩니다. 

`hx-on*` 속성은 일반화된 임베디드 스크립팅을 위한 매우 간단한 메커니즘입니다. AlpineJS나 hyperscript 같은 보다 완벽하게 개발된 
프런트엔드 스크립팅 솔루션을 _대체할_ 수는 없습니다. 그러나 htmx 기반 애플리케이션의 스크립팅에 대한 VanillaJS 기반 접근 방식을 
보강할 수는 있습니다. 

HTML 속성은 대소문자를 *구분하지 않는다는* 점에 유의하세요. 즉, 안타깝게도 capitalization/camel casing에 의존하는 이벤트는 
응답할 수 없습니다. camel 케이스 이벤트를 지원해야 하는 경우 AlpineJS 또는 hyperscript와 같은 보다 완전한 기능을 갖춘 
스크립팅 솔루션을 사용하는 것이 좋습니다. htmx는 바로 이러한 이유로 모든 이벤트를
camelCase와 kebab-case 모두로 전송합니다.

### 3rd Party Javascript {#3rd-party}

Htmx는 타사 라이브러리와 상당히 잘 통합됩니다. 라이브러리가 DOM에서 이벤트를 발생시키면 
해당 이벤트를 사용하여 htmx의 요청을 트리거할 수 있습니다.

이에 대한 좋은 예로 [SortableJS demo](@/examples/sortable.md)를 들 수 있습니다:

```html
<form class="sortable" hx-post="/items" hx-trigger="end">
    <div class="htmx-indicator">Updating...</div>
    <div><input type='hidden' name='item' value='1'/>Item 1</div>
    <div><input type='hidden' name='item' value='2'/>Item 2</div>
    <div><input type='hidden' name='item' value='2'/>Item 3</div>
</form>
```

대부분의 자바스크립트 라이브러리와 마찬가지로 Sortable을 사용하면 어느 시점에서 콘텐츠를 초기화해야 합니다. 

jquery에서는 다음과 같이 초기화할 수 있습니다:

```javascript
$(document).ready(function() {
    var sortables = document.body.querySelectorAll(".sortable");
    for (var i = 0; i < sortables.length; i++) {
        var sortable = sortables[i];
        new Sortable(sortable, {
            animation: 150,
            ghostClass: 'blue-background-class'
        });
    }
});
```

htmx에서는 대신 `htmx.onLoad` 함수를 사용하며 전체 문서가 아닌 
새로 로드된 콘텐츠 중에서만 선택하게 됩니다:

```js
htmx.onLoad(function(content) {
    var sortables = content.querySelectorAll(".sortable");
    for (var i = 0; i < sortables.length; i++) {
        var sortable = sortables[i];
        new Sortable(sortable, {
            animation: 150,
            ghostClass: 'blue-background-class'
        });
    }
})
```

이렇게 하면 htmx로 새 콘텐츠가 DOM에 추가될 때 정렬 가능한 요소가 제대로 초기화됩니다. 

자바스크립트가 htmx 속성이 있는 콘텐츠를 DOM에 추가하는 경우 이 콘텐츠가 
`htmx.process()` 함수를 사용하여 초기화되는지 확인해야 합니다. 

예를 들어 `fetch` API를 사용하여 일부 데이터를 가져와서 div에 넣고 HTML에 
htmx 속성이 있는 경우 다음과 같이 `htmx.process()` 호출을 추가하면 됩니다:

```js
let myDiv = document.getElementById('my-div')
fetch('http://example.com/movies.json')
    .then(response => response.text())
    .then(data => { myDiv.innerHTML = data; htmx.process(myDiv); } );
```

일부 타사 라이브러리는 HTML 템플릿 요소에서 콘텐츠를 생성합니다. 예를 들어, Alpine JS는 템플릿에서 
`x-if` 속성을 사용하여 조건부로 콘텐츠를 추가합니다. 이러한 템플릿은 처음에 DOM의 일부가 아니며, 
htmx 속성이 포함된 경우 로드된 후 `htmx.process()`를 호출해야 합니다. 
다음 예제는 조건부 콘텐츠를 트리거하는 값의 변경을 찾기 위해 Alpine의 `$watch` 함수를 사용합니다:

```html
<div x-data="{show_new: false}"
    x-init="$watch('show_new', value => {
        if (show_new) {
            htmx.process(document.querySelector('#new_content'))
        }
    })">
    <button @click = "show_new = !show_new">Toggle New Content</button>
    <template x-if="show_new">
        <div id="new_content">
            <a hx-get="/server/newstuff" href="#">New Clickable</a>
        </div>
    </template>
</div>
```

#### Web Components {#web-components}

htmx를 웹 컴포넌트와 통합하는 방법에 대한 예제는 
[Web Components Examples](@/examples/web-components.md) 페이지를 참조하세요.

## Caching

htmx는 기본적으로 표준 [HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) 메커니즘과 
함께 작동합니다. 

서버가 특정 URL에 대한 응답에 [`Last-Modified`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Last-Modified) HTTP 응답 헤더를 추가하면 브라우저는 
동일한 URL에 대한 다음 요청에 [`If-Modified-Since`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since) 요청 HTTP 헤더를 자동으로 추가합니다. 
서버가 다른 헤더에 따라 동일한 URL에 대해 다른 콘텐츠를 렌더링할 수 있는 경우 [`Vary`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#vary) 응답 HTTP 헤더를 
사용해야 한다는 점에 유의하세요. 예를 들어, 서버에서 `HX-Request` 헤더가 없거나 `false`일 때는 
전체 HTML을 렌더링하고 `HX-Request: true`일 때는 해당 HTML의 일부를 렌더링하는 경우, `Vary: HX-Request`를 
추가해야 합니다. 이렇게 하면 캐시가 응답 URL만을 기반으로 하는 것이 아니라 응답 URL과 HX-Request 요청 헤더의 
조합을 기반으로 키가 지정됩니다.

`Vary` 헤더를 사용할 수 없거나 원하지 않는 경우 구성 매개변수 `getCacheBusterParam`을 `true`로 설정할 수 있습니다. 
이 구성 변수를 설정하면 htmx가 생성하는 `GET` 요청에 cache-busting 매개 변수가 포함되어 브라우저가 동일한 캐시 슬롯에서 
htmx 기반 응답과 비-htmx 기반 응답을 캐싱하지 못하게 됩니다. 

htmx는 예상대로 [`ETag`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)와도 작동합니다. 서버가 동일한 URL에 대해 다른 콘텐츠를 렌더링할 수 있는 경우(예: `HX-Request` 헤더의 값에 따라) 
서버는 각 콘텐츠에 대해 다른 `ETag`를 생성해야 한다는 점에 유의하세요.

## Security

htmx를 사용하면 DOM에서 직접 로직을 정의할 수 있습니다. 이 방식에는 여러 가지 장점이 있지만, 
가장 큰 장점은 시스템을 더 쉽게 이해하고 유지 관리할 수 있는 [행동의 지역성](@/essays/locality-of-behaviour.md)입니다. 

그러나 이 접근 방식에서 우려되는 점은 보안입니다. htmx는 HTML의 표현력을 높이기 때문에 악의적인 사용자가 애플리케이션에 HTML을 삽입할 수 있다면 
악의적인 목적으로 htmx의 이러한 표현력을 활용할 수 있다는 점입니다.

### Rule 1: Escape All User Content

HTML 기반 웹 개발의 첫 번째 규칙은 항상 *사용자의 입력을 신뢰하지 말라는 것*입니다. 사이트에 삽입되는 
신뢰할 수 없는 타사 콘텐츠를 모두 제거해야 합니다. 이는 다른 문제 중에서도 [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting)을 방지하기 위한 것입니다. 

[Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)를 포함하여 훌륭한 [OWASP Website](https://owasp.org/www-community/attacks/xss/)에 XSS와 
이를 방지하는 방법에 대한 광범위한 문서가 있습니다. 

좋은 소식은 이 문제는 매우 오래되고 잘 알려진 주제이며 대부분의 서버 측 템플릿 언어는 
이러한 문제를 방지하기 위해 콘텐츠의 [automatic escaping](https://docs.djangoproject.com/en/4.2/ref/templates/language/#automatic-html-escaping)를 지원합니다. 

그렇지만 사람들이 종종 템플릿 언어의 일종의 `raw()` 메커니즘을 통해 더 위험한 HTML 삽입을 
선택하는 경우가 있다는 점입니다. 이는 정당한 이유가 있을 수 있지만, 삽입되는 콘텐츠가 타사에서 제공된 것이라면 
`hx-` 및 `data-hx`로 시작하는 속성, 인라인 `<script>` 태그 등을 제거하는 등 스크러빙해야 합니다. 

원시 HTML을 삽입하고 자체 이스케이프를 수행하는 경우 허용하는 속성 및 태그를 
블랙리스트에 올리는 대신 *화이트리스트*에 올리는 것이 모범 사례에 해당합니다.

### htmx Security Tools

물론 버그가 발생하고 개발자는 완벽하지 않으므로 웹 애플리케이션의 보안을 위해 
계층화된 접근 방식을 취하는 것이 좋으며, htmx는 애플리케이션 보안에 도움이 되는 도구도 제공합니다. 

이를 살펴보도록 하겠습니다.

#### `hx-disable`

애플리케이션의 보안을 강화하기 위해 htmx가 제공하는 첫 번째 도구는 [`hx-disable`](/attributes/hx-disable) 속성입니다. 
이 속성은 지정된 요소와 그 안의 모든 요소에 대한 모든 htmx 속성의 처리를 방지합니다. 예를 들어 템플릿에 
원시 HTML 콘텐츠를 포함하는 경우(다시 말하지만 권장하지 않습니다!) 
콘텐츠 주위에 `hx-disable` 속성이 있는 div를 배치할 수 있습니다:

```html
<div hx-disable>
    <%= raw(user_content) %>
</div>
```

또한 htmx는 해당 콘텐츠에서 발견된 htmx 관련 속성이나 기능을 처리하지 않습니다. 
이 속성은 추가 콘텐츠를 삽입하여 비활성화할 수 없습니다. 요소의 상위 계층 구조에서 
`hx-disable` 속성이 발견되면 htmx에서 처리되지 않습니다.

#### `hx-history`

또 다른 보안 고려 사항은 htmx 히스토리 캐시입니다. 사용자의 `localStorage` 캐시에 저장하지 않아야 하는 
민감한 데이터가 있는 페이지가 있을 수 있습니다. 페이지의 아무 곳에나 [`hx-history`](/attributes/hx-history) 속성을 포함시키고 
해당 값을 `false`로 설정하여 히스토리 캐시에서 특정 페이지를 생략할 수 있습니다.

#### Configuration Options

htmx는 보안과 관련된 구성 옵션도 제공합니다:

* `htmx.config.selfRequestsOnly` - `true`로 설정하면 현재 문서와 동일한 도메인에 대한 요청만 허용됩니다.
* `htmx.config.allowScriptTags` - htmx는 로드되는 새 콘텐츠에서 발견된 `<script>` 태그를 처리합니다. 
이 태그 허용을 비활성화하려면 이 구성 변수를 `false`로 설정하면 됩니다.
* `htmx.config.historyCacheSize` - `0`으로 설정하여 `localStorage` 캐시에 HTML을 저장하지 않도록 할 수 있습니다.
* `htmx.config.allowEval` - eval에 의존하는 htmx의 모든 기능을 비활성화하려면 `false`로 설정할 수 있습니다.
  * event filters
  * `hx-on:` 속성
  * 접두사가 `js:`인 `hx-vals` 
  * 접두사가 `js:`인 `hx-headers`

`eval()`을 비활성화하여 제거된 모든 기능은 사용자 정의 자바스크립트 및 htmx 이벤트 모델을 사용하여 
다시 구현할 수 있습니다.

#### Events

현재 호스트 이외의 일부 도메인에 대한 요청을 허용하되 완전히 열어두지 않으려면 `htmx:validateUrl` 이벤트를 사용할 수 있습니다. 
이 이벤트에는 `detail.url` 슬롯에서 사용할 수 있는 요청 URL과 `sameHost` 속성이 있습니다.

이러한 값을 검사하고 요청이 유효하지 않은 경우 이벤트에서 `preventDefault()`를 호출하여 요청이 보내지지 않도록 할 수 있습니다.

```javascript
document.body.addEventListener('htmx:validateUrl', function (evt) {
  // 현재 서버와 myserver.com에 대한 요청만 허용합니다.
  if (!evt.detail.sameHost && evt.detail.url.hostname !== "myserver.com") {
    evt.preventDefault();
  }
});
```

### CSP Options

브라우저는 웹 애플리케이션을 더욱 안전하게 보호할 수 있는 도구도 제공합니다. 가장 강력한 도구는 
[Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)입니다. 예를 들어 CSP를 사용하면 브라우저에 
원본이 아닌 호스트에 요청을 발행하지 않거나 인라인 스크립트 태그를 평가하지 않도록 지시할 수 있습니다.

다음은 `meta` 태그의 CSP 예시입니다:

```html
    <meta http-equiv="Content-Security-Policy" content="default-src 'self';">
```

이는 브라우저에 "원본(소스) 도메인에 대한 연결만 허용"이라고 알려줍니다. 이는 `htmx.config.selfRequestsOnly`와 중복될 수 있지만 
애플리케이션 보안을 다룰 때는 보안에 대한 계층화된 접근 방식이 필요하며, 이런 방식이 실제로 이상적입니다.

CSP에 대한 자세한 논의는 이 문서의 범위를 벗어납니다.
[MDN 문서](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)를 보면 이 주제를 살펴보는 데 좋은 출발점이 될 수 있습니다.

## Configuring htmx {#config}

Htmx에는 프로그래밍 방식으로 또는 선언적으로 액세스할 수 있는 몇 가지 구성 옵션이 있습니다. 아래에 나열되어 있습니다:

<div class="info-table">

| Config Variable                       | Info                                                                                                                                                                                                                                    |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `htmx.config.historyEnabled`          | defaults to `true`, 테스트에만 유용합니다                                                                                                                                                                                                         |
| `htmx.config.historyCacheSize`        | defaults to 10                                                                                                                                                                                                                          |
| `htmx.config.refreshOnHistoryMiss`    | defaults to `false`, `true`로 설정하면 htmx는 히스토리 누락 시 AJAX 요청을 사용하는 대신 전체 페이지 새로 고침을 실행합니다                                                                                                                                                  |
| `htmx.config.defaultSwapStyle`        | defaults to `innerHTML`                                                                                                                                                                                                                 |
| `htmx.config.defaultSwapDelay`        | defaults to 0                                                                                                                                                                                                                           |
| `htmx.config.defaultSettleDelay`      | defaults to 20                                                                                                                                                                                                                          |
| `htmx.config.includeIndicatorStyles`  | defaults to `true` (indicator 스타일이 로드되는지 여부를 결정합니다)                                                                                                                                                                                     |
| `htmx.config.indicatorClass`          | defaults to `htmx-indicator`                                                                                                                                                                                                            |
| `htmx.config.requestClass`            | defaults to `htmx-request`                                                                                                                                                                                                              |
| `htmx.config.addedClass`              | defaults to `htmx-added`                                                                                                                                                                                                                |
| `htmx.config.settlingClass`           | defaults to `htmx-settling`                                                                                                                                                                                                             |
| `htmx.config.swappingClass`           | defaults to `htmx-swapping`                                                                                                                                                                                                             |
| `htmx.config.allowEval`               | defaults to `true`, 특정 기능(예: 트리거 필터)에 대한 htmx의 eval 사용을 비활성화하는 데 사용할 수 있습니다                                                                                                                                                             |
| `htmx.config.allowScriptTags`         | defaults to `true`, 새 콘텐츠에서 발견된 스크립트 태그를 htmx가 처리할지 여부를 결정합니다                                                                                                                                                                           |
| `htmx.config.inlineScriptNonce`       | defaults to `''`, 이것의 의미는 인라인 스크립트에 nonce가 추가되지 않는 것입니다                                                                                                                                                                                 |
| `htmx.config.inlineSlyeNonce`         | defaults to `''`, 이것의 의미는 인라인 스타일에 nonce가 추가되지 않는 것입니다                                                                                                                                                                                  |
| `htmx.config.attributesToSettle`      | defaults to `["class", "style", "width", "height"]`, 정리 단계에서 정리할 속성                                                                                                                                                                     |
| `htmx.config.wsReconnectDelay`        | defaults to `full-jitter`                                                                                                                                                                                                               |
| `htmx.config.wsBinaryType`            | defaults to `blob`, WebSocket 연결을 통해 수신되는 [binary 데이터 유형](https://developer.mozilla.org/docs/Web/API/WebSocket/binaryType)                                                                                                              |
| `htmx.config.disableSelector`         | defaults to `[hx-disable], [data-hx-disable]`, htmx는 이 속성이 있는 요소나 상위 요소를 처리하지 않습니다                                                                                                                                                      |
| `htmx.config.withCredentials`         | defaults to `false`, 쿠키, 인증 헤더 또는 TLS 클라이언트 인증서와 같은 자격 증명을 사용하여 크로스-사이트 액세스 제어 요청을 허용합니다                                                                                                                                                |
| `htmx.config.timeout`                 | defaults to 0, 요청이 자동으로 종료되기까지 걸릴 수 있는 시간(milliseconds)                                                                                                                                                                                 |
| `htmx.config.scrollBehavior`          | defaults to 'smooth', 페이지 전환 시 부스트 링크의 동작을 설정합니다. 허용되는 값은 `auto` 및 `Smooth`입니다. Smooth는 페이지 상단으로 부드럽게 스크롤되는 반면 Auto는 바닐라 링크처럼 작동합니다.                                                                                                    |
| `htmx.config.defaultFocusScroll`      | focused 요소를 스크롤하여 뷰에 표시해야 하는 경우 기본값은 false이며 [focus-scroll](@/attributes/hx-swap.md#focus-scroll) 교체 수정자를 사용하여 재정의할 수 있습니다.                                                                                                             |
| `htmx.config.getCacheBusterParam`     | defaults to false, true로 설정하면 htmx는 `org.htmx.cache-buster=targetElementId` 형식으로 `GET` 요청에 대상 요소를 추가합니다.                                                                                                                                |
| `htmx.config.globalViewTransitions`   | `true`로 설정하면 htmx는 새 콘텐츠를 교체할 때 [View Transition](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) API를 사용합니다.                                                                                                    |
| `htmx.config.methodsThatUseUrlParams` | defaults to `["get"]`, htmx는 요청 본문이 아닌 URL에서 매개 변수를 인코딩하여 이러한 메서드를 사용하는 요청의 형식을 지정합니다.                                                                                                                                                  |
| `htmx.config.selfRequestsOnly`        | 기본값은 현재 문서와 동일한 도메인에 대한 AJAX 요청만 허용할지 여부를 나타내는 true입니다.                                                                                                                                                                                 |
| `htmx.config.ignoreTitle`             | defaults to `false`, `true`로 설정하면 htmx는 새 콘텐츠에서 `title` 태그가 발견될 때 문서 제목을 업데이트하지 않습니다.                                                                                                                                                   |
| `htmx.config.scrollIntoViewOnBoost`   | 부스트된 요소의 대상이 뷰포트로 스크롤되는지 여부에 관계없이 기본값이 true로 설정됩니다. 부스트된 요소에서 `hx-target`을 생략하면 대상은 기본적으로 `body`로 설정되어 페이지가 맨 위로 스크롤됩니다.                                                                                                                |
| `htmx.config.triggerSpecsCache`       | 기본값은 평가된 트리거 사양을 저장할 캐시인 `null`이므로 메모리 사용량을 늘리는 대신 구문 분석 성능을 향상시킬 수 있습니다. 절대 지워지지 않는 캐시를 사용하도록 간단한 객체를 정의하거나 [proxy object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Proxy)를 사용하여 자체 시스템을 구현할 수 있습니다. |
| `htmx.config.allowNestedOobSwaps`     | 기본 응답 요소 내에 중첩된 요소에 대해 OOB 교체를 처리할지 여부, 기본값은 `true`입니다. [Nested OOB Swaps](@/attributes/hx-swap-oob.md#nested-oob-swaps)를 참조하세요.                                                                                                        |

</div>

자바스크립트에서 직접 설정하거나 `meta` 태그를 사용할 수 있습니다:

```html
<meta name="htmx-config" content='{"defaultSwapStyle":"outerHTML"}'>
```

## Conclusion

여기까지입니다!

htmx를 즐겨보세요! 많은 코드를 작성하지 않고도 [꽤 많은](@/examples/_index.md) 것을 성취할 수 있습니다!

</div>
</div>
