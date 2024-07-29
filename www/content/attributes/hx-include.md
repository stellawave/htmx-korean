+++
title = "hx-include"
+++

`hx-include` 속성은 AJAX 요청에 추가 요소 값을 포함할 수 있게 합니다. 이 속성의 값은 다음과 같습니다:

* 포함할 요소들의 CSS 쿼리 선택자.
* `this`는 요소의 자손을 포함합니다.
* `closest <CSS selector>`는 주어진 CSS 선택자와 일치하는 [가장 가까운](https://developer.mozilla.org/docs/Web/API/Element/closest) 상위 요소나 자기 자신을 찾습니다
  (예: `closest tr`은 요소에 가장 가까운 테이블 행을 대상으로 합니다).
* `find <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 하위 자손 요소를 찾습니다.
* `next <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 앞으로 스캔합니다
  (예: `next .error`는 `error` 클래스를 가진 가장 가까운 다음 형제 요소를 대상으로 합니다).
* `previous <CSS selector>`는 주어진 CSS 선택자와 일치하는 첫 번째 요소를 찾아 DOM을 뒤로 스캔합니다
  (예: `previous .error`는 `error` 클래스를 가진 가장 가까운 이전 형제 요소를 대상으로 합니다).

다음은 별도의 입력 값을 포함하는 예제입니다:

```html
<div>
    <button hx-post="/register" hx-include="[name='email']">
        Register!
    </button>
    Enter email: <input name="email" type="email"/>
</div>
```

이 예제는 일반적으로 두 요소를 `form`에 포함하여 자동으로 값을 제출하는 경우가 많지만, 개념을 설명하기 위해 사용되었습니다.

비입력 요소를 포함하는 경우, 해당 요소에 포함된 모든 입력 요소가 포함된다는 점에 유의하십시오.

## Notes

* `hx-include`는 상속되며 부모 요소에 배치할 수 있습니다.
* `hx-include`는 상속되지만, 요청을 트리거하는 요소에서 평가됩니다. `find`와 `closest` 같은 확장 선택자를 사용할 때 혼동하기 쉽습니다.
  ```html
  <div hx-include="find input">
      <button hx-post="/register">
          Register!
      </button>
      Enter email: <input name="email" type="email"/>
  </div>
  ```
  위 예제에서 버튼을 클릭하면, `find input` 선택자는 button 자체에서 해결됩니다. 여기서 button은 `input` 자식을 포함하지 않으므로 요소를 반환하지 않으며, 따라서 오류를 발생시킵니다.
* 표준 CSS 선택자는 [document.querySelectorAll](https://developer.mozilla.org/docs/Web/API/Document/querySelectorAll)로 
해결되며 여러 요소를 포함할 수 있는 반면, `find` 또는 `next`와 같은 확장 선택자는 최대 한 개의 요소만 반환하여 포함됩니다.