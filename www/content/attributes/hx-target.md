+++
title = "hx-target"
+++

`hx-target` 속성은 AJAX 요청을 보내는 요소와 다른 요소를 대상으로 교체할 수 있게 합니다. 이 속성의 값은 다음과 같습니다:

* 교체할 요소의 CSS 쿼리 선택자.
* `this`는 `hx-target` 속성이 적용된 요소 자기자신이 대상임을 나타냅니다.
* `closest <CSS selector>`는 주어진 CSS 선택자와 일치하는 [가장 가까운](https://developer.mozilla.org/docs/Web/API/Element/closest) 상위 요소 또는 자기 자신을 찾습니다
  (예: `closest tr`은 요소에서 가장 가까운 테이블 행을 대상으로 합니다).
* `find <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 하위 자손 요소를 찾습니다.
* `next`는 [element.nextElementSibling](https://developer.mozilla.org/docs/Web/API/Element/nextElementSibling)을 해결합니다.
* `next <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 앞으로 스캔합니다
  (예: `next .error`는 `error` 클래스를 가진 가장 가까운 다음 형제 요소를 대상으로 합니다).
* `previous`는 [element.previousElementSibling](https://developer.mozilla.org/docs/Web/API/Element/previousElementSibling)을 해결합니다.
* `previous <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 뒤로 스캔합니다
  (예: `previous .error`는 `error` 클래스를 가진 가장 가까운 이전 형제 요소를 대상으로 합니다).

다음은 `div`를 대상으로 하는 예제입니다:

```html
<div>
    <div id="response-div"></div>
    <button hx-post="/register" hx-target="#response-div" hx-swap="beforeend">
        Register!
    </button>
</div>
```

`/register` URL의 응답은 `response-div` ID를 가진 `div`에 추가됩니다.

다음 예제는 `hx-target="this"`를 사용하여 클릭 시 자신을 업데이트하는 링크를 만듭니다:

```html
<a hx-post="/new-link" hx-target="this" hx-swap="outerHTML">New link</a>
```

## Notes

* `hx-target`은 상속되며 부모 요소에 배치할 수 있습니다.
