+++
title = "hx-push-url"
+++

`hx-push-url` 속성은 브라우저 [location history](https://developer.mozilla.org/en-US/docs/Web/API/History_API)에 URL을 추가할 수 있게 합니다.
이 속성은 새로운 히스토리 항목을 생성하여 브라우저의 뒤로 및 앞으로 버튼으로 탐색할 수 있게 합니다.
htmx는 현재 DOM을 스냅샷으로 저장하고 이를 히스토리 캐시에 저장하며, 탐색 시 이 캐시에서 복원합니다.

이 속성의 가능한 값은 다음과 같습니다:

1. `true` - 가져온 URL을 히스토리에 추가합니다.
2. `false` - 상속이나 [`hx-boost`](/attributes/hx-boost)에 의해 URL이 추가될 경우 이를 비활성화합니다.
3. 위치 표시줄에 추가할 URL - 이는 [`history.pushState()`](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)에 따라 상대적 또는 절대적일 수 있습니다.

다음은 예제입니다:

```html
<div hx-get="/account" hx-push-url="true">
  Go to My Account
</div>
```

이 예제는 htmx가 현재 DOM을 `localStorage`에 스냅샷으로 저장하고, URL `/account`를 브라우저 위치 표시줄에 추가하도록 합니다.

또 다른 예제:

```html
<div hx-get="/account" hx-push-url="/account/home">
  Go to My Account
</div>
```

이 예제는 URL `/account/home`을 location history에 추가합니다.

## Notes

* `hx-push-url`은 상속되며 부모 요소에 배치할 수 있습니다.
* [`HX-Push-Url` 응답 헤더](@/headers/hx-push-url.md)는 이 속성과 유사한 동작을 하며, 이 속성을 재정의할 수 있습니다.
* [`hx-history-elt` 속성](@/attributes/hx-history-elt.md)은 히스토리 캐시에 저장될 요소를 변경할 수 있게 합니다.
