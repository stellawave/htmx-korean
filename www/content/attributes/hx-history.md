+++
title = "hx-history"
+++

현재 문서나 htmx에 의해 현재 문서에 로드된 HTML 조각의 어떤 요소라도 `hx-history` 속성을 `false`로 설정하면, htmx가 페이지 상태의 스냅샷을 찍을 때 민감한 데이터가 localStorage 캐시에 저장되지 않도록 할 수 있습니다.

히스토리 탐색은 예상대로 작동하지만, 복원 시에는 히스토리 캐시 대신 서버에서 URL을 요청합니다.

다음은 예제입니다:

```html
<html>
<body>
<div hx-history="false">
 ...
</div>
</body>
</html>
```

## Notes

* `hx-history="false"`는 문서의 *어디에나* 존재할 수 있으며, 현재 페이지 상태를 히스토리 캐시에서 제외시킵니다 (즉, 히스토리 스냅샷을 위한 요소 [hx-history-elt](@/attributes/hx-history-elt.md) 외부에도 존재할 수 있습니다).
