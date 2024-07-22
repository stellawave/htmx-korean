+++
title = "hx-swap-oob"
+++

`hx-swap-oob` 속성은 응답의 일부 콘텐츠를 대상 외부의 DOM 어딘가에 교체하도록 지정할 수 있게 합니다. 
이는 "Out of Band"를 의미합니다. 이를 통해 다른 요소 업데이트에 대한 응답에 편승하여 업데이트를 수행할 수 있습니다.

다음 응답 HTML을 보세요:

```html
<div>
 ...
</div>
<div id="alerts" hx-swap-oob="true">
    Saved!
</div>

```

첫 번째 div는 일반적인 방식으로 목표 대상을 교체합니다. 그러나 두 번째 div는 ID가 `alerts`인 있는 요소를 교체하며, 기존 교체 목표 대상에 포함되지 않습니다.

`hx-swap-oob`의 값은 다음과 같을 수 있습니다:

* `true`
* 유효한 모든 [`hx-swap`](@/attributes/hx-swap.md) 값
* 유효한 콜론, CSS 선택자가 뒤에 오는 모든 [`hx-swap`](@/attributes/hx-swap.md) 값

값이 `true` 또는 `outerHTML`(이와 동등한 값)이면 요소는 인라인으로 바뀝니다.

swap 값이 지정되면 해당 교체 전략이 사용됩니다.

선택자가 지정되면 해당 선택자와 일치하는 모든 요소가 교체됩니다. 그렇지 않은 경우 새 콘텐츠와 일치하는 ID를 가진 요소가 바뀝니다.

### Troublesome Tables

`template` 태그를 사용하여 HTML 사양에 따라 DOM에서 독립적으로 존재할 수 없는 요소 유형
(`<tr>`, `<td>`, `<th>`, `<thead>`, `<tbody>`, `<tfoot>`, `<colgroup>`, `<caption>` 및 `<col>`)을 캡슐화할 수 있다는 점을 참고하세요.

다음은 이러한 방식으로 테이블 행의 out of band가 캡슐화된 예제입니다:

```html
<div>
    ...
</div>
<template>
    <tr id="row" hx-swap-oob="true">
        ...
    </tr>
</template>
```

이러한 template 태그는 페이지의 최종 콘텐츠에서 제거된다는 점에 유의하세요.

## Nested OOB Swaps

기본적으로, 응답 내 어디에든 `hx-swap-oob=` 속성이 있는 모든 요소는 out of band 교체 동작으로 처리됩니다. 이는 요소가 주요 응답 요소 내에 중첩된 경우도 포함됩니다. 이는 조각(fragment)을 
oob-swap 대상 및 더 큰 조각의 일부로 재사용할 수 있는 [template fragments](https://htmx.org/essays/template-fragments/)을 사용할 때 문제가 될 수 있습니다. 더 큰 조각이 주요 응답인 경우 내부 조각도 여전히 oob swap으로 처리되어서 DOM에서 제거됩니다.

이 동작은 `htmx.config.allowNestedOobSwaps`를 `false`로 설정하여 변경할 수 있습니다. 이 구성 옵션이 `false`이면 요소가 기본 응답 요소에 *인접한* 경우에만 OOB 교체가 처리되고, 다른 곳의 OOB 교체은 무시되고 OOB 교체 관련 속성이 제거됩니다.

## Notes

* `hx-swap-oob`은 상속되지 않습니다.
