+++
title = "hx-delete"
+++

`hx-delete` 속성은 요소가 지정된 URL로 `DELETE` 요청을 보내고,
교체 전략을 사용하여 HTML을 DOM에서 교체되게 만듭니다:

```html
<button hx-delete="/account" hx-target="body">
  Delete Your Account
</button>
```

이 예제는 `button`을 누르면 `/account`에 `DELETE` 방식으로 보내고, 반환된 HTML을
`body`에 `innerHTML` 방식으로 집어넣습니다.

## Notes

* `hx-delete`은 상속되지 않습니다.
* [hx-target](@/attributes/hx-target.md) 속성을 사용하여 교체의 대상을 제어할 수 있습니다.
* [hx-swap](@/attributes/hx-swap.md) 속성을 사용하여 교체 전략을 제어할 수 있습니다.
* [hx-trigger](@/attributes/hx-trigger.md) 속성을 사용하여 요청을 트리거하는 이벤트를 제어할 수 있습니다.
* 요청과 함께 제출되는 데이터를 다양한 방법으로 제어할 수 있으며, 이는 다음 문서에 기록되어 있습니다: [Parameters](@/docs.md#parameters)
* `DELETE`가 성공한 후 요소를 제거하려면 본문이 비어 있는 `200` 상태 코드를 반환하세요. 서버가 `204`로 응답하면 교체 작업이 발생하지 않습니다. 여기에 설명되어 있습니다. [Requests & Responses](@/docs.md#requests)
