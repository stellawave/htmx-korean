+++
title = "hx-on"
+++

`hx-on*` 속성을 사용하면 스크립트를 인라인으로 삽입하여 요소의 이벤트에 직접 응답할 수 있습니다. 
`onClick`과 같은 HTML의 [`onevent` 속성](https://developer.mozilla.org/en-US/docs/Web/Events/Event_handlers#using_onevent_properties)과 유사합니다.

`hx-on*` 속성은 임의의 자바스크립트 이벤트를 처리할 수 있도록 하여 
비표준 DOM 이벤트를 처리할 때에도 향상된 [Locality of Behaviour (LoB)](/essays/locality-of-behaviour/)를 제공함으로써 `onevent`를 개선합니다. 
예를 들어, 이러한 속성을 사용하면 [htmx events](/reference#events)를 처리할 수 있습니다.

`hx-on` 속성을 사용하면 콜론 뒤에 이벤트 이름을 속성 이름의 일부로 지정합니다. 
예를 들어 클릭 이벤트에 응답하려면 `hx-on:click` 속성을 사용하면 됩니다:

```html
<div hx-on:click="alert('Clicked!')">Click</div>
```

이 구문은 표준 DOM 이벤트 외에도 모든 htmx 이벤트와 대부분의 다른 사용자 정의 이벤트를 캡처하는 데 사용할 수 있습니다.

한 가지 주의해야 할 점은 DOM 속성은 대소문자를 보존하지 않는다는 것입니다. 
즉, 안타깝게도 DOM은 속성 이름을 소문자로 처리하기 때문에 `hx-on:htmx:beforeRequest`와 같은 속성은 **작동하지 않습니다**. 
다행히도 htmx는 camel 이벤트 이름과 [kebab-case 이벤트 이름](@/docs.md#events)을 모두 지원하므로 대신 hx-on:htmx:before-request를 사용할 수 있습니다.

htmx 기반 이벤트 핸들러를 좀 더 쉽게 작성하려면 htmx 이벤트에 속기 이중 콜론 
`hx-on::`을 사용하고 "htmx" 부분을 생략할 수 있습니다:

```html
<!-- 아래 둘은 똑같습니다 -->
<button hx-get="/info" hx-on:htmx:before-request="alert('Making a request!')">
    Get Info!
</button>

<button hx-get="/info" hx-on::before-request="alert('Making a request!')">
    Get Info!
</button>

```

여러 개의 서로 다른 이벤트를 처리하려면 요소에 여러 개의 속성을 추가하면 됩니다:

```html
<button hx-get="/info"
        hx-on::before-request="alert('Making a request!')"
        hx-on::after-request="alert('Done making a request!')">
    Get Info!
</button>
```

마지막으로 이 기능을 HTML 속성에 콜론(`:`)을 사용하지 않는 일부 템플릿 언어(예: [JSX](https://react.dev/learn/writing-markup-with-jsx))와 호환되도록 하려면,
긴 형식과 속기 형식 모두에 콜론 대신 대시를 사용하면 됩니다:

```html
<!-- 아래 둘은 똑같습니다 -->
<button hx-get="/info" hx-on-htmx-before-request="alert('Making a request!')">
    Get Info!
</button>

<button hx-get="/info" hx-on--before-request="alert('Making a request!')">
    Get Info!
</button>

```

### hx-on (사용 중단됨)
값은 이벤트 이름, 콜론 `:`, 스크립트로 구성됩니다:

```html
<button hx-get="/info" hx-on="htmx:beforeRequest: alert('Making a request!')">
    Get Info!
</button>
```

여러 핸들러는 새 줄에 작성하여 정의할 수 있습니다:
```html
<button hx-get="/info" hx-on="htmx:beforeRequest: alert('Making a request!')
                              htmx:afterRequest: alert('Done making a request!')">
    Get Info!
</button>
```

### Symbols

`onevent`처럼, 이벤트 핸들러 스크립트에서 사용할 수 있는 두 가지 기호가 있습니다:

* `this` - `hx-on` 속성이 정의된 요소
* `event` - 핸들러를 트리거한 이벤트

### Notes

* `hx-on`은 상속되지 않습니다. 하지만 [이벤트 버블링](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling_and_capture)으로 인해, 
부모 요소의 `hx-on` 속성은 자식 요소의 이벤트에 의해 트리거될 수 있습니다.
* `hx-on:*`과 `hx-on`은 같은 요소에서 함께 사용할 수 없습니다. 
`hx-on:*`이 존재하는 경우, 동일한 요소에 있는 `hx-on` 속성의 값은 무시됩니다. 그러나 두 형태는 동일한 문서에서 혼합하여 사용할 수 있습니다.
