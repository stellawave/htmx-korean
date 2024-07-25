+++
title = "hx-params"
+++

`hx-params` 속성은 AJAX 요청과 함께 제출될 매개변수를 필터링할 수 있게 합니다.

이 속성의 가능한 값은 다음과 같습니다:

* `*` - 모든 매개변수를 포함 (기본값)
* `none` - 매개변수를 포함하지 않음
* `not <param-list>` - 쉼표로 구분된 매개변수 이름 목록을 제외한 모든 매개변수를 포함
* `<param-list>` - 쉼표로 구분된 매개변수 이름 목록을 포함

```html
  <div hx-get="/example" hx-params="*">Get Some HTML, Including Params</div>
```

이 div는 `POST`가 포함할 모든 매개변수를 포함하지만, 일반적인 `GET` 요청처럼 URL 인코딩되어 URL에 포함됩니다.

## Notes

* `hx-params`는 상속되며 부모 요소에 배치할 수 있습니다.
