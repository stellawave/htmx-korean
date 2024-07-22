+++
title = "hx-trigger"
+++

`hx-trigger` 속성은 AJAX 요청을 트리거하는 방법을 지정할 수 있게 해줍니다. 트리거 값은 다음 중 하나일 수 있습니다:

* 이벤트 이름 (예: "click" 또는 "my-custom-event") 뒤에 이벤트 필터 및 일련의 이벤트 수정자를 추가할 수 있습니다.
* `every <timing declaration>` 형식의 polling 정의
* 이러한 이벤트의 쉼표로 구분된 목록

### Standard Events

`click`과 같은 표준 이벤트는 다음과 같이 트리거로 지정할 수 있습니다:

```html
<div hx-get="/clicked" hx-trigger="click">Click Me</div>
```

#### Standard Event Filters

이벤트는 이벤트 이름 뒤에 대괄호로 감싼 boolean JavaScript 표현식을 포함하여 필터링할 수 있습니다. 이 표현식이 `true`로 평가되면 이벤트가 트리거되고, 그렇지 않으면 무시됩니다.

```html
<div hx-get="/clicked" hx-trigger="click[ctrlKey]">Control Click Me</div>
```

이 이벤트는 클릭 이벤트가 `event.ctrlKey` 속성이 true로 설정된 경우에 트리거됩니다.

이 조건식은 전역 함수나 상태를 참조할 수도 있습니다:

```html
<div hx-get="/clicked" hx-trigger="click[checkGlobalState()]">Control Click Me</div>
```

또한 표준 JavaScript 구문을 사용하여 결합할 수도 있습니다:

```html
<div hx-get="/clicked" hx-trigger="click[ctrlKey&&shiftKey]">Control-Shift Click Me</div>
```

모든 기호는 먼저 트리거 이벤트를 기준으로 해석되고, 다음으로 전역 네임스페이스를 기준으로 해석됩니다. 따라서 `myEvent[foo]`는 먼저 이벤트에서 `foo`라는 속성을 찾고, 다음으로 `foo`라는 이름의 전역 기호를 찾습니다.

#### Standard Event Modifiers

표준 이벤트에는 동작을 변경하는 수정자가 있을 수 있습니다. 수정자는 다음과 같습니다:

* `once` - 이벤트가 한 번만 트리거됩니다 (예: 첫 번째 클릭).
* `changed` - 요소의 값이 변경된 경우에만 이벤트가 트리거됩니다. 주의할 점은 `change`는 이벤트 이름이고, `changed`는 수정자 이름입니다.
* `delay:<timing declaration>` - 이벤트가 요청을 트리거하기 전에 지연이 발생합니다. 이벤트가 다시 발생하면 지연이 초기화됩니다.
* `throttle:<timing declaration>` - 이벤트가 요청을 트리거한 후에 스로틀이 발생합니다. 지연이 완료되기 전에 이벤트가 다시 발생하면 무시되고, 지연이 끝날 때 요소가 트리거됩니다.
* `from:<Extended CSS selector>` - 요청을 트리거하는 이벤트가 문서의 다른 요소에서 발생하도록 허용합니다 (예: 단축키를 지원하기 위해 body에서 키 이벤트를 듣는 경우).
  * 표준 CSS 선택자는 해당 선택자와 일치하는 모든 요소로 해석됩니다. 따라서 `from:input`은 페이지의 모든 입력 요소를 듣습니다.
  * 여기서 확장된 CSS 선택자는 다음과 같은 비표준 CSS 값을 허용합니다:
    * `document` - 문서에서 이벤트를 듣습니다.
    * `window` - 창에서 이벤트를 듣습니다.
    * `closest <CSS selector>` - 주어진 CSS 선택자와 일치하는 [가장 가까운](https://developer.mozilla.org/docs/Web/API/Element/closest) 상위 요소 또는 자기 자신을 찾습니다.
    * `find <CSS selector>` - 주어진 CSS 선택자와 일치하는 가장 가까운 자식을 찾습니다.
    * `next`는 [element.nextElementSibling](https://developer.mozilla.org/docs/Web/API/Element/nextElementSibling)으로 해석됩니다.
    * `next <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 앞으로 스캔합니다 (예: `next .error`는 `error` 클래스를 가진 가장 가까운 다음 형제 요소를 대상으로 합니다).
    * `previous`는 [element.previousElementSibling](https://developer.mozilla.org/docs/Web/API/Element/previousElementSibling)으로 해석됩니다.
    * `previous <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 뒤로 스캔합니다 (예: `previous .error`는 `error` 클래스를 가진 가장 가까운 이전 형제 요소를 대상으로 합니다).
* `target:<CSS selector>` - 이벤트의 대상을 CSS 선택자로 필터링할 수 있습니다. 이는 초기화 시점에 DOM에 없는 요소에서 트리거를 듣고자 할 때 유용합니다. 예를 들어, body에서 듣지만 하위 요소에 대한 대상 필터를 사용하여 초기화 시점에 DOM에 없을 수 있는 요소의 트리거를 수신하려는 경우에 유용할 수 있습니다.
* `consume` - 이 옵션이 포함되면 이벤트가 부모 요소에서 다른 htmx 요청을 트리거하지 않습니다 (또는 부모 요소에서 듣는 요소에서도).
* `queue:<queue option>` - 다른 이벤트에 대한 요청이 진행 중일 때 이벤트가 발생하면 이벤트가 큐에 어떻게 저장될지를 결정합니다. 옵션은 다음과 같습니다:
  * `first` - 첫 번째 이벤트를 큐에 저장합니다.
  * `last` - 마지막 이벤트를 큐에 저장합니다 (기본값).
  * `all` - 모든 이벤트를 큐에 저장합니다 (각 이벤트에 대한 요청을 발행합니다).
  * `none` - 새로운 이벤트를 큐에 저장하지 않습니다.

다음은 `keyup` 시 검색되지만 검색 값이 변경되고 사용자가 
1초 동안 새로운 내용을 입력하지 않은 경우에만 검색되는 검색창의 예입니다:

```html
<input name="q"
       hx-get="/search" hx-trigger="keyup changed delay:1s"
       hx-target="#search-results"/>
```

`/search` URL의 응답은 id가 `search-results`인 `div`에 추가됩니다.

### Non-standard Events

htmx는 몇 가지 비표준 이벤트를 지원합니다:

* `load` - 로드 시 트리거됨 (지연 로드에 유용)
* `revealed` - 요소가 뷰포트에 스크롤될 때 트리거됨 (지연 로드에 유용). CSS에서 `overflow-y: scroll`과 같은 `overflow`를 사용하는 경우 `revealed` 대신 `intersect once`를 사용해야 합니다.
* `intersect` - 요소가 뷰포트와 처음 교차할 때 한 번 트리거됨. 이 이벤트는 두 가지 추가 옵션을 지원합니다:
  * `root:<selector>` - 교차를 위한 루트 요소의 CSS 선택자
  * `threshold:<float>` - 이벤트를 트리거할 교차 비율을 나타내는 0.0에서 1.0 사이의 부동 소수점 숫자

### Triggering via the `HX-Trigger` header

<code>HX-Trigger</code> 응답 헤더를 통해 이벤트를 트리거하려는 경우 `from:body` 수정자를 사용하는 것이 좋습니다. 예를 들어, 응답과 함께 <code>HX-Trigger: my-custom-event</code>와 같은 헤더를 보낸다면, 요소는 다음과 같아야 합니다:

```html
  <div hx-get="/example" hx-trigger="my-custom-event from:body">
    Triggered by HX-Trigger header...
  </div>
```

이렇게 해야 이벤트가 발생합니다.

이는 헤더가 트리거하려는 요소와 다른 DOM 계층 구조에서 이벤트를 트리거할 가능성이 있기 때문입니다. 비슷한 이유로 단축키를 body에서 듣는 경우가 많습니다.

### Polling

`every <timing declaration>` 구문을 사용하여 요소가 주기적으로 Polling하도록 할 수 있습니다:

```html
<div hx-get="/latest_updates" hx-trigger="every 1s">
  Nothing Yet!
</div>
```

이 예제는 `/latest_updates` URL에 매초 `GET` 요청을 보내고 결과를 이 div의 innerHTML에 교체합니다.

Polling에 필터를 추가하려면, 필터는 Polling 선언 *뒤에* 추가해야 합니다:

```html
<div hx-get="/latest_updates" hx-trigger="every 1s [someConditional]">
  Nothing Yet!
</div>
```

### Multiple Triggers

여러 트리거를 쉼표로 구분하여 제공할 수 있습니다. 각 트리거는 고유한 옵션을 가집니다.

```html
  <div hx-get="/news" hx-trigger="load, click delay:1s"></div>
```

이 예제는 페이지 로드 시 즉시 `/news`를 로드하고, 클릭 후 1초의 지연 후 다시 로드합니다.

### Via JavaScript

AJAX 요청은 JavaScript [`htmx.trigger()`](@/api.md#trigger)를 통해서도 트리거할 수 있습니다.

## Notes

* `hx-trigger`는 상속되지 않습니다.
* `hx-trigger`는 AJAX 요청 없이도 사용할 수 있으며, 이 경우 `htmx:trigger` 이벤트만 트리거합니다.
* 공백이 포함된 CSS 선택자(예: `form input`)를 `from` 또는 `target` 수정자에 전달하려면 선택자를 괄호나 중괄호로 감싸야 합니다 (예: `from:(form input)` 또는 `from:nearest (form input)`).
