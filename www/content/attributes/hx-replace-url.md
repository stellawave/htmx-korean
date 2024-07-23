+++
title = "hx-replace-url"
+++

`hx-replace-url` 속성은 브라우저 [location history](https://developer.mozilla.org/en-US/docs/Web/API/History_API)의 현재 URL을 교체할 수 있게 합니다.

이 속성의 가능한 값은 다음과 같습니다:

1. `true` - 가져온 URL을 브라우저 탐색 표시줄에서 교체합니다.
2. `false` - 상속으로 인해 URL이 교체될 경우 이를 비활성화합니다.
3. 위치 표시줄에 교체할 URL - 이는 [`history.replaceState()`](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState)에 따라 상대적 또는 절대적일 수 있습니다.

다음은 예제입니다:

```html
<div hx-get="/account" hx-replace-url="true">
  Go to My Account
</div>
```

이 예제는 htmx가 현재 DOM을 `localStorage`에 스냅샷으로 저장하고, 브라우저 위치 표시줄의 URL을 `/account`로 교체하도록 합니다.

또 다른 예제:

```html
<div hx-get="/account" hx-replace-url="/account/home">
  Go to My Account
</div>
```

이 예제는 브라우저 위치 표시줄의 URL을 `/account/home`으로 교체합니다.

## 참고 사항

* `hx-replace-url`은 상속되며 부모 요소에 배치할 수 있습니다.
* [`HX-Replace-Url` 응답 헤더](@/headers/hx-replace-url.md)는 이 속성과 유사한 동작을 하며, 이 속성을 재정의할 수 있습니다.
* [`hx-history-elt` 속성](@/attributes/hx-history-elt.md)은 히스토리 캐시에 저장될 요소를 변경할 수 있게 합니다.
* [`hx-push-url` 속성](@/attributes/hx-push-url.md)은 유사하고 더 일반적으로 사용되는 속성으로, 현재 항목을 교체하는 대신 새로운 히스토리 항목을 생성합니다.
