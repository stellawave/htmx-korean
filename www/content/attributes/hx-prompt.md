+++
title = "hx-prompt"
+++

`hx-prompt` 속성은 요청을 보내기 전에 프롬프트를 표시할 수 있게 합니다. 프롬프트의 값은 `HX-Prompt` 헤더에 포함되어 요청에 전달됩니다.

다음은 예제입니다:

```html
<button hx-delete="/account" hx-prompt="Enter your account name to confirm deletion">
  Delete My Account
</button>
```

## 참고 사항

* `hx-prompt`는 상속되며 부모 요소에 배치할 수 있습니다.
