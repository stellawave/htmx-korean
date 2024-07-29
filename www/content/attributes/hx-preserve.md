+++
title = "hx-preserve"
+++

`hx-preserve` 속성은 HTML 교체 중에 요소를 변경되지 않도록 유지할 수 있게 합니다. 
`hx-preserve`가 설정된 요소는 htmx가 상위 요소를 업데이트할 때 `id`를 통해 보존됩니다. 
`hx-preserve`가 작동하려면 *반드시* 요소에 변하지 않는 `id`를 설정해야 합니다. 
응답에는 동일한 `id`를 가진 요소가 필요하지만, 해당 요소의 타입 및 다른 속성은 무시됩니다.

다음과 같은 일부 요소는 불행히도 제대로 보존될 수 없습니다: `<input type="text">` (focus 및 caret 위치가 손실됨), 
iframe 또는 특정 유형의 비디오. 이러한 경우 일부 문제를 해결하기 위해 더 정교한 DOM 동기화를 수행하는 
[morphdom 확장](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/morphdom-swap/README.md)을 권장합니다.

## Notes

* `hx-preserve`는 상속되지 않습니다.
