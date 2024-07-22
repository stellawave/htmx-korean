+++
title = "hx-confirm"
+++

`hx-confirm` 속성은 요청을 발행하기 전에 동작을 확인할 수 있게 합니다. 이 속성은 파괴적인 동작을 수행할 때 사용자가 정말로 이를 수행하고자 하는지 확인하는 데 유용합니다.

다음은 예제입니다:

```html
<button hx-delete="/account" hx-confirm="Are you sure you wish to delete your account?">
  Delete My Account
</button>
```

## Event details

`hx-confirm`에 의해 트리거된 이벤트는 `detail`에 추가 속성을 포함합니다:

* triggeringEvent: 원래 요청을 트리거한 이벤트
* issueRequest(skipConfirmation=false): AJAX 요청을 확인하는 데 사용할 수 있는 callback
* question: HTML 요소의 `hx-confirm` 속성 값

## Notes

* `hx-confirm`은 상속되며 부모 요소에 배치할 수 있습니다.
* `hx-confirm`은 기본적으로 브라우저의 `window.confirm`을 사용합니다. 이 동작을 사용자 지정하는 방법은 [이 예제](@/examples/confirm.md)에서 볼 수 있습니다.
* boolean 값 `skipConfirmation`을 `issueRequest` callback에 전달할 수 있습니다. true로 설정하면 (기본값은 false) `window.confirm`이 호출되지 않고 AJAX 요청이 직접 발행됩니다.
