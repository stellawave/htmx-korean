+++
title = "hx-request"
+++

`hx-request` 속성은 다음 속성을 통해 요청의 다양한 측면을 구성할 수 있게 합니다:

* `timeout` - 요청의 타임아웃 시간(밀리초 단위)
* `credentials` - 요청에 자격 증명이 포함될지 여부
* `noHeaders` - 요청에서 모든 헤더를 제거

이 속성들은 JSON 유사 구문을 사용하여 설정됩니다:

```html
<div ... hx-request='{"timeout":100}'>
  ...
</div>
```

값을 동적으로 평가하려면 `javascript:` 또는 `js:` 접두사를 추가하면 됩니다:

```html
<div ... hx-request='js: timeout:getTimeoutSetting() '>
  ...
</div>
```

## Notes

* `hx-request`는 병합-상속되며 부모 요소에 배치할 수 있습니다.
