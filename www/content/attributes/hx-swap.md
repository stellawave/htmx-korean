+++
title = "hx-swap"
+++

`hx-swap` 속성을 사용하면 AJAX 요청의 [target](@/attributes/hx-target.md)을 기준으로 
응답이 어떻게 교체될지 지정할 수 있습니다. 
옵션을 지정하지 않으면 기본값은 `htmx.config.defaultSwapStyle`(`innerHTML`)입니다.

이 속성의 가능한 값은 다음과 같습니다:

* `innerHTML` - 대상 요소의 내부 HTML을 교체합니다.
* `outerHTML` - 대상 요소 전체를 응답으로 대체합니다.
* `textContent` - 응답을 HTML로 구문 분석하지 않고 대상 요소의 텍스트 콘텐츠를 바꿉니다.
* `beforebegin` - 대상 요소 앞에 응답을 삽입합니다.
* `afterbegin` - 대상 요소의 첫 번째 자식 앞에 응답을 삽입합니다.
* `beforeend` - 대상 요소의 마지막 자식 뒤에 응답을 삽입합니다.
* `afterend` - 대상 요소 뒤에 응답을 삽입합니다.
* `delete` - 응답에 관계없이 대상 요소를 삭제합니다.
* `none`- 응답의 콘텐츠를 추가하지 않습니다(out of band는 계속 처리됩니다).

이러한 옵션은 표준 DOM 이름 지정과 
[`Element.insertAdjacentHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML) 
사양을 기반으로 합니다.

이 코드에서도 마찬가지입니다:

```html
  <div hx-get="/example" hx-swap="afterend">Get Some HTML & Append It</div>
```

`div`는 `/example`에 요청을 보내고 반환된 콘텐츠를 `div` 뒤에 추가합니다.

### Modifiers

`hx-swap` 속성은 교체 동작을 변경하기 위한 수정자를 지원합니다. 아래에 설명되어 있습니다.

#### Transition: `transition`

교체가 발생할 때 새로운 [View Transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) API를 사용하려면 
교체에 `transition:true` 옵션을 사용하면 됩니다. `htmx.config.globalViewTransitions` 구성 설정을 
`true`로 설정하여 이 기능을 전역적으로 활성화할 수도 있습니다.

#### Timing: `swap` & `settle`

`swap` 수정자를 포함하여 콘텐츠 교체를 위해 응답을 받은 후 
htmx가 대기하는 시간을 수정할 수 있습니다:

```html
  <!-- this will wait 1s before doing the swap after it is received -->
  <div hx-get="/example" hx-swap="innerHTML swap:1s">Get Some HTML & Append It</div>
```

마찬가지로 `settle` 수정자를 포함하여 교체와 정리 로직 사이의 시간을 수정할 수 있습니다:

```html
  <!-- this will wait 1s before doing the swap after it is received -->
  <div hx-get="/example" hx-swap="innerHTML settle:1s">Get Some HTML & Append It</div>
```

이러한 속성을 사용하여 htmx를 CSS 전환 효과의 타이밍과 동기화할 수 있습니다.

#### Title: `ignoreTitle`

기본적으로 htmx는 응답 콘텐츠에서 `<title>` 태그를 찾으면 페이지의 제목을 업데이트합니다. 
`ignoreTitle` 옵션을 true로 설정하여 이 동작을 해제할 수 있습니다.

#### Scrolling: `scroll` & `show`

`scroll`과 `show` 수정자를 사용하여 대상 요소의 스크롤 동작을 변경할 수 있습니다. 
이 수정자들은 각각 `top`과 `bottom` 값을 가집니다:

```html
  <!-- 이 고정 높이 div는 콘텐츠가 추가된 후 div의 하단으로 스크롤됩니다 -->
  <div style="height:200px; overflow: scroll" 
       hx-get="/example" 
       hx-swap="beforeend scroll:bottom">
     Get Some HTML & Append It & Scroll To Bottom
  </div>
```

```html
  <!-- 이는 일부 콘텐츠를 가져와 #another-div에 추가한 후,
       #another-div의 상단이 뷰포트에 보이도록 합니다 -->
  <div hx-get="/example" 
       hx-swap="innerHTML show:top"
       hx-target="#another-div">
    Get Some Content
  </div>
```

스크롤링 또는 표시를 위해 다른 요소를 대상으로 지정하려면 `scroll:` 또는 `show:` 다음에 
CSS 선택자를 배치하고 `:top` 또는 `:bottom`을 추가하면 됩니다:

```html
  <!-- 이는 일부 콘텐츠를 가져와 현재 div에 교체한 후,
       #another-div의 상단이 뷰포트에 보이도록 합니다 -->
  <div hx-get="/example" 
       hx-swap="innerHTML show:#another-div:top">
    Get Some Content
  </div>
```

현재 창의 상단 및 하단으로 스크롤하려면 `window:top` 및 `window:bottom`을 사용할 수도 있습니다.

```html
  <!-- 이는 일부 콘텐츠를 가져와 현재 div에 교체한 후,
       뷰포트가 맨 위로 스크롤되도록 합니다 -->
  <div hx-get="/example" 
       hx-swap="innerHTML show:window:top">
    Get Some Content
  </div>
```

Boost된 링크와 폼의 기본 동작은 `show:top`입니다. 이를 전역적으로 비활성화하려면 
[htmx.config.scrollIntoViewOnBoost](@/api.md#config)를 사용하거나 요소별로 `hx-swap="show:none"`을 사용할 수 있습니다.

```html
<form action="/example" hx-swap="show:none">
  ...
</form>
```

#### Focus scroll

htmx는 정의된 ID 속성이 있는 입력에 대한 요청 간에 focus를 유지합니다. 기본적으로 htmx는 사용자가 이미 했을 때 긴 요청에서 원치 않는 동작이 될 수 있는, 요청 간 focus 입력에 대한 자동 스크롤을 방지합니다. focus 스크롤을 활성화하려면 `focus-scroll:true`를 사용하면 됩니다.

```html
  <input id="name" hx-get="/validation" 
       hx-swap="outerHTML focus-scroll:true"/>
```

또는 각 요청 후 페이지가 focus가 맞춰진 요소로 자동 스크롤되도록 하려면 htmx 전역 구성 값 `htmx.config.defaultFocusScroll`을 true로 변경하면 됩니다. 그런 다음 `focus-scroll:false`를 사용하면 특정 요청에 대해 이 기능을 비활성화합니다.

```html
  <input id="name" hx-get="/validation" 
       hx-swap="outerHTML focus-scroll:false"/>
```

## Notes

* `hx-swap`은 상속되며 부모 요소에 배치할 수 있습니다.
* 속성의 기본값은 `innerHTML`입니다.
* DOM 제한으로 인해 `<body>` 요소에서 `outerHTML` 메서드를 사용할 수 없습니다. 
htmx는 `<body>`의 `outerHTML`을 `innerHTML`을 사용하도록 변경합니다.
* 기본 교체 지연 시간은 0ms입니다.
* 기본 정리 지연 시간은 20ms입니다.
