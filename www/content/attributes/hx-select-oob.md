+++
title = "hx-select-oob"
+++

`hx-select-oob` 속성은 응답에서 콘텐츠를 선택하여 out of band 방식으로 교체할 수 있게 합니다. 
이 속성의 값은 쉼표로 구분된 요소 목록이며, 거의 항상 [hx-select](@/attributes/hx-select.md)와 함께 사용됩니다.

다음은 응답 콘텐츠의 일부를 선택하는 예제입니다:

```html
<div>
   <div id="alert"></div>
    <button hx-get="/info" 
            hx-select="#info-details" 
            hx-swap="outerHTML"
            hx-select-oob="#alert">
        Get Info!
    </button>
</div>
```

이 버튼은 `/info`에 `GET` 요청을 보내고, id가 `info-details`인 요소를 선택하여 DOM에서 버튼 전체를 교체합니다. 추가로 응답에서 id가 `alert`인 요소를 선택하여 동일한 ID를 가진 DOM의 div와 교체합니다.

쉼표로 구분된 목록의 각 값은 선택자와 교체 전략을 `:`로 구분하여 유효한 [`hx-swap`](@/attributes/hx-swap.md) 전략을 지정할 수 있습니다.

예를 들어, alert 콘텐츠를 교체하는 대신 prepend하려면:

```html
<div>
   <div id="alert"></div>
    <button hx-get="/info"
            hx-select="#info-details"
            hx-swap="outerHTML"
            hx-select-oob="#alert:afterbegin">
        Get Info!
    </button>
</div>
```

## Notes

* `hx-select-oob`는 상속되며 부모 요소에 배치할 수 있습니다.
