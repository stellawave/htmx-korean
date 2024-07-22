+++
title = "hx-select"
+++

`hx-select` 속성은 응답에서 교체할 콘텐츠를 선택할 수 있게 해줍니다. 이 속성의 값은 응답에서 선택할 요소 또는 요소들을 나타내는 CSS 쿼리 선택자입니다.

다음은 응답 콘텐츠의 일부를 선택하는 예제입니다:

```html
<div>
    <button hx-get="/info" hx-select="#info-details" hx-swap="outerHTML">
        Get Info!
    </button>
</div>
```

이 버튼은 `/info`에 `GET` 요청을 보낸 후, id가 `info-details`인 요소를 선택하여, 해당 요소가 DOM에서 버튼 전체를 교체하도록 합니다.

## Notes

* `hx-select`는 상속되며, 부모 요소에 배치할 수 있습니다.
