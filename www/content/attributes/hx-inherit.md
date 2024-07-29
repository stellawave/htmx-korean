+++
title = "hx-inherit"
+++

htmx의 기본 동작은 많은 속성을 자동으로 "상속"하는 것입니다. 
즉, [hx-target](@/attributes/hx-target.md)과 같은 속성은 부모 요소에 배치될 수 있으며, 
모든 자식 요소는 해당 대상을 상속받습니다. 
일부 사람들은 이 기능을 좋아하지 않으며 속성에 대한 상속을 명시적으로 지정하는 것을 선호합니다.

이 개발 모드를 지원하기 위해, htmx는 `htmx.config.disableInheritance` 설정을 제공합니다. 
이 설정을 `false`로 설정하면 htmx 속성에 대해 상속이 기본 동작이 되지 않습니다.

`hx-inherit` 속성은 속성 상속을 수동으로 제어할 수 있게 합니다.

htmx는 속성 상속을 다음과 같이 평가합니다:

* `hx-inherit`가 부모 노드에 설정된 경우
  * `inherit="*"`는 이 요소에 대한 모든 속성 상속을 활성화합니다.
  * `hx-inherit="hx-select hx-get hx-target"`는 하나 이상의 지정된 속성에 대한 상속만 활성화합니다.

다음은 `htmx.config.disableInheritance`가 false로 설정된 경우, anchor 태그 집합에 대해 `hx-target` 속성을 공유하는 div의 예입니다:

```html
<div hx-target="#tab-container" hx-inherit="hx-target">
  <a hx-boost="true" href="/tab1">Tab 1</a>
  <a hx-boost="true" href="/tab2">Tab 2</a>
  <a hx-boost="true" href="/tab3">Tab 3</a>
</div>
```

## Notes

* [속성 상속](https://htmx.org/docs/#inheritance)에 대해 더 읽어보세요.
