+++
title = "hx-disabled-elt"
+++

`hx-disabled-elt` 속성은 요청이 진행되는 동안 `disabled` 속성이 추가될 요소를 지정할 수 있게 합니다. 이 속성의 값은 다음과 같습니다:

* 비활성화할 요소의 CSS 쿼리 선택자.
* `this`는 요소 본인을 비활성화.
* `closest <CSS selector>`는 주어진 CSS 선택자와 일치하는 [가장 가까운](https://developer.mozilla.org/docs/Web/API/Element/closest) 상위 요소나 자기 자신을 찾습니다 (예: `closest fieldset`은 요소에 가장 가까운 `fieldset`을 비활성화).
* `find <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 하위 자식 요소를 찾습니다.
* `next`는 [element.nextElementSibling](https://developer.mozilla.org/docs/Web/API/Element/nextElementSibling)으로 해석됩니다.
* `next <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 앞으로 스캔합니다 (예: `next button`은 가장 가까운 다음 `button` 형제 요소를 비활성화).
* `previous`는 [element.previousElementSibling](https://developer.mozilla.org/docs/Web/API/Element/previousElementSibling)으로 해석됩니다.
* `previous <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 뒤로 스캔합니다 (예: `previous input`은 가장 가까운 이전 `input` 형제 요소를 비활성화).

다음은 요청 동안 자신을 비활성화하는 버튼의 예제입니다:

```html
<button hx-post="/example" hx-disabled-elt="this">
    Post It!
</button>
```

요청이 진행 중일 때, 이는 버튼에 [`disabled` 속성](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled)을 추가하여 추가 클릭을 방지합니다.

`hx-disabled-elt` 속성은 요청 동안 여러 요소를 비활성화하기 위해 쉼표로 구분된 여러 CSS 선택자를 지정하는 것도 지원합니다. 다음은 요청 동안 특정 폼의 버튼과 텍스트 입력 필드를 비활성화하는 예제입니다:

```html
<form hx-post="/example" hx-disabled-elt="find input[type='text'], find button">
    <input type="text" placeholder="Type here...">
    <button type="submit">Send</button>
</form>
```

## Notes

* `hx-disabled-elt`는 상속되며 부모 요소에 배치할 수 있습니다.

[hx-trigger]: https://htmx.org/attributes/hx-trigger/
