+++
title = "hx-ext"
+++

`hx-ext` 속성은 요소와 그 모든 자식 요소에 대해 htmx [확장](https://extensions.htmx.org)을 활성화합니다.

값은 단일 확장 이름이거나 적용할 확장의 쉼표로 구분된 목록일 수 있습니다.

플러그인을 DOM의 전체 범위에 적용하려면 `hx-ext` 태그를 부모 요소에 배치할 수 있으며, 모든 htmx 요청에 적용하려면 `body` 태그에 배치할 수 있습니다.

## Notes

* `hx-ext`는 부모 요소와 상속 및 병합되므로, DOM 계층 구조의 어떤 요소에 확장을 지정해도 모든 자식 요소에 적용됩니다.
* `hx-ext="ignore:extensionName"`을 사용하여 부모 노드에 정의된 확장을 무시할 수 있습니다.


```html
<div hx-ext="example">
  "Example" extension is used in this part of the tree...
  <div hx-ext="ignore:example">
    ... but it will not be used in this part.
  </div>
</div>
```

