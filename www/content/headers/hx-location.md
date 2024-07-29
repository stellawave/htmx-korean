+++
title = "HX-Location Response Header"
+++

이 응답 헤더는 전체 페이지를 다시 로드하지 않고 클라이언트 측 리디렉션을 트리거하는 데 사용할 수 있습니다. 페이지의 위치를 변경하는 대신 [`hx-boost` 링크](@/attributes/hx-boost.md)를 따르는 것처럼 작동하여 새로운 히스토리 항목을 생성하고, 헤더의 값에 대한 ajax 요청을 발행하며, 경로를 히스토리에 푸시합니다.

예시 응답은 다음과 같습니다:

```html
HX-Location: /test
```

이것은 사용자가 `<a href="/test" hx-boost="true">`를 클릭한 것처럼 클라이언트를 /test로 이동시킵니다.

기본값인 document.body 대신 페이지의 특정 대상으로 리디렉션하려면, 헤더 값에 JSON을 사용하여 이벤트와 함께 더 많은 세부 정보를 전달할 수 있습니다:

```html
HX-Location: {"path":"/test2", "target":"#testdiv"}
```

path는 필수이며 응답을 로드할 URL입니다. 나머지 데이터는 [`ajax` API](@/api.md#ajax) 컨텍스트와 일치하며 다음과 같습니다:

* `source` - 요청의 소스 요소
* `event` - 요청을 "트리거"한 이벤트
* `handler` - 응답 HTML을 처리할 콜백
* `target` - 응답을 교체할 대상
* `swap` - 응답이 대상에 대해 어떻게 교체될지
* `values` - 요청과 함께 제출할 값
* `headers` - 요청과 함께 제출할 헤더
* `select` - 응답에서 교체할 콘텐츠를 선택할 수 있습니다.
