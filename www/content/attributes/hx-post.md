+++
title = "hx-post"
+++

`hx-post` 속성은 요소가 지정된 URL로 `POST` 요청을 보내고,
교체 전략을 사용하여 HTML을 DOM에서 교체되게 만듭니다:

```html
<button hx-post="/account/enable" hx-target="body">
  Enable Your Account
</button>
```

이 예제는 `button`을 누르면 `/account/enable`로 `POST` 방식으로 보내고, 반환된 HTML을 
`body`에 `innerHTML` 방식으로 집어넣습니다.
 
## Notes

* `hx-post`은 상속되지 않습니다.
* [hx-target](@/attributes/hx-target.md) 속성을 사용하여 교체의 대상을 제어할 수 있습니다.
* [hx-swap](@/attributes/hx-swap.md) 속성을 사용하여 교체 전략을 제어할 수 있습니다.
* [hx-trigger](@/attributes/hx-trigger.md) 속성을 사용하여 요청을 트리거하는 이벤트를 제어할 수 있습니다.
* 요청과 함께 제출되는 데이터를 다양한 방법으로 제어할 수 있으며, 이는 다음 문서에 기록되어 있습니다: [Parameters](@/docs.md#parameters)
