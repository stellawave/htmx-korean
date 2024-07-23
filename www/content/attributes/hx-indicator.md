+++
title = "hx-indicator"
+++

`hx-indicator` 속성은 요청이 진행되는 동안 `htmx-request` 클래스가 추가될 요소를 명시적으로 지정할 수 있게 해줍니다. 이는 요청이 진행 중일 때 스피너나 진행 표시기를 표시하는 데 사용할 수 있습니다.

이 속성의 값은 클래스를 적용할 요소 또는 요소들의 CSS 쿼리 선택자입니다. 또는 키워드 [`closest`](https://developer.mozilla.org/docs/Web/API/Element/closest) 뒤에 CSS 선택자를 추가하여, 주어진 CSS 선택자와 일치하는 가장 가까운 상위 요소나 자기 자신을 찾을 수 있습니다 (예: `closest tr`).

다음은 버튼 옆에 스피너가 있는 예제입니다:

```html
<div>
    <button hx-post="/example" hx-indicator="#spinner">
        Post It!
    </button>
    <img  id="spinner" class="htmx-indicator" src="/img/bars.svg"/>
</div>
```

요청이 진행 중일 때, 이는 `#spinner` 이미지에 `htmx-request` 클래스를 추가합니다. 이미지에는 또한 `htmx-indicator` 클래스가 있으며, 이는 스피너를 표시하는 불투명도 전환을 정의합니다:

```css
    .htmx-indicator{
        opacity:0;
        transition: opacity 500ms ease-in;
    }
    .htmx-request .htmx-indicator{
        opacity:1
    }
    .htmx-request.htmx-indicator{
        opacity:1
    }
```

스피너를 표시하기 위해 다른 효과를 선호한다면, 자체 indicator CSS를 정의하여 사용할 수 있습니다. 다음은 불투명도 대신 `display`를 사용하는 예제입니다 (참고로 `htmx-indicator` 대신 `my-indicator`를 사용합니다):

```css
    .my-indicator{
        display:none;
    }
    .htmx-request .my-indicator{
        display:inline;
    }
    .htmx-request.my-indicator{
        display:inline;
    }
```

`hx-indicator` 선택자의 대상은 표시하려는 정확한 요소일 필요가 없다는 점에 유의하십시오. indicator의 상위 계층의 어떤 요소라도 될 수 있습니다.

마지막으로, 기본적으로 `htmx-request` 클래스는 요청을 발생시키는 요소에 추가되므로, 그 요소 안에 인디케이터를 배치하면 `hx-indicator` 속성을 명시적으로 호출할 필요가 없습니다:

```html
<button hx-post="/example">
    Post It!
   <img  class="htmx-indicator" src="/img/bars.svg"/>
</button>
```

## Demo

다음은 위 상황에서 스피너가 어떻게 보일지를 시뮬레이션한 것입니다:

<button class="btn" classes="toggle htmx-request:3s">
    Post It!
   <img  class="htmx-indicator" src="/img/bars.svg"/>
</button>

## Notes

* `hx-indicator`는 상속되며 부모 요소에 배치할 수 있습니다.
* 명시적인 indicator가 없으면, `htmx-request` 클래스는 요청을 트리거하는 요소에 추가됩니다.
* `htmx-indicator`를 클래스 이름으로 사용하면서 자체 CSS를 사용하려면, `includeIndicatorStyles`를 비활성화해야 합니다. 자세한 내용은 [Configuring htmx](@/docs.md#config)을 참조하십시오. 가장 쉬운 방법은 HTML의 `<head>`에 다음을 추가하는 것입니다:
```html
<meta name="htmx-config" content='{"includeIndicatorStyles": false}'>
```
