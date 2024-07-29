+++
title = "hx-disinherit"
+++

htmx의 기본 동작은 많은 속성을 자동으로 "상속"하는 것입니다. 즉, [hx-target](@/attributes/hx-target.md)과 같은 속성은 부모 요소에 배치될 수 있으며, 
모든 자식 요소는 해당 대상을 상속받습니다.

`hx-disinherit` 속성은 이러한 자동 속성 상속을 제어할 수 있게 합니다. 예를 들어, 페이지의 `body` 요소에 `hx-boost`를 배치했는데, 
페이지의 특정 부분에서 해당 동작을 무시하고 더 구체적인 동작을 허용해야할 수도 있습니다.

htmx는 속성 상속을 다음과 같이 평가합니다:

* `hx-disinherit`가 부모 노드에 설정된 경우
  * `hx-disinherit="*"`는 이 요소에 대한 모든 속성 상속을 비활성화합니다.
  * `hx-disinherit="hx-select hx-get hx-target"`는 하나 이상의 지정된 속성에 대한 상속만 비활성화합니다.

```html
<div hx-boost="true" hx-select="#content" hx-target="#content" hx-disinherit="*">
  <a href="/page1">Go To Page 1</a> <!-- 위의 속성 설정으로 부스트됨 -->
  <a href="/page2" hx-boost="unset">Go To Page 1</a> <!-- 부스트되지 않음 -->
  <button hx-get="/test" hx-target="this"></button> <!-- hx-select는 상속되지 않음 -->
</div>
```

```html
<div hx-boost="true" hx-select="#content" hx-target="#content" hx-disinherit="hx-target">
  <!-- hx-select는 부모의 값으로 자동 설정됨; hx-target은 상속되지 않음 -->
  <button hx-get="/test"></button>
</div>
```

```html
<div hx-select="#content">
  <div hx-boost="true" hx-target="#content" hx-disinherit="hx-select">
    <!-- hx-target은 부모의 값으로 자동 상속됨 -->
    <!-- hx-select는 상속되지 않음, 직접 부모가 상속을 비활성화하기 때문
    비록 자신이 hx-select를 지정하지 않았어도 -->
    <button hx-get="/test"></button>
  </div>
</div>
```

## Notes

* [속성 상속](@/docs.md#inheritance)에 대해 더 읽어보세요.
