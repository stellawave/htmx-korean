+++
title = "HX-Push-Url Response Header"
+++

`HX-Push-Url` 헤더는 브라우저 [위치 기록](https://developer.mozilla.org/en-US/docs/Web/API/History_API)에 URL을 추가할 수 있게 합니다.
이 헤더는 새로운 히스토리 항목을 생성하여 브라우저의 뒤로 및 앞으로 버튼으로 탐색할 수 있게 합니다.
이는 [`hx-push-url` 속성](@/attributes/hx-push-url.md)과 유사합니다.

이 헤더가 존재하면, 속성으로 정의된 동작을 무시하고 이 헤더의 동작을 따릅니다.

이 헤더의 가능한 값은 다음과 같습니다:

1. 위치 표시줄에 추가할 URL.
   이는 [`history.pushState()`](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)에 따라 상대적 또는 절대적일 수 있습니다.
2. `false`, 브라우저의 히스토리가 업데이트되지 않도록 합니다.