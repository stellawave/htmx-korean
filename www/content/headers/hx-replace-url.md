+++
title = "HX-Replace-Url Response Header"
+++

`HX-Replace-Url` 헤더는 브라우저 [위치 기록](https://developer.mozilla.org/en-US/docs/Web/API/History_API)의 현재 URL을 교체할 수 있게 합니다.
이것은 새로운 히스토리 항목을 생성하지 않으며, 사실상 이전의 현재 URL을 브라우저의 히스토리에서 제거합니다.
이는 [`hx-replace-url` 속성](@/attributes/hx-replace-url.md)과 유사합니다.

이 헤더가 존재하면, 속성으로 정의된 동작을 무시하고 이 헤더의 동작을 따릅니다.

이 헤더의 가능한 값은 다음과 같습니다:

1. 위치 표시줄의 현재 URL을 교체할 URL.
   이는 [`history.replaceState()`](https://developer.mozilla.org/en-US/docs/Web/API/History/replaceState)에 따라 상대적 또는 절대적일 수 있지만, 현재 URL과 동일한 출처를 가져야 합니다.
2. `false`, 브라우저의 현재 URL이 업데이트되지 않도록 합니다.
