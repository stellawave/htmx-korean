+++
title = "hx-headers"
+++

`hx-headers` 속성은 AJAX 요청과 함께 제출될 헤더를 추가할 수 있게 해줍니다.

기본적으로 이 속성의 값은 [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) 형식의 이름-표현식 값 목록입니다.

`hx-headers`가 주어진 값을 *평가*하도록 하려면, 값 앞에 `javascript:` 또는 `js:` 접두사를 추가할 수 있습니다.

```html
  <div hx-get="/example" hx-headers='{"myHeader": "My Value"}'>Get Some HTML, Including A Custom Header in the Request</div>
```

## Security Considerations

* 기본적으로 `hx-headers`의 값은 유효한 [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON)이어야 합니다.
  동적으로 계산되지 **않습니다**. 만약 `javascript:` 접두사를 사용하는 경우, 특히 쿼리 문자열이나 사용자 생성 콘텐츠와 같은 사용자 입력을 처리할 때, 
이는 [Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) 취약성이 발생할 수 있다는 점을 유의해야 합니다.

## Notes

* `hx-headers`는 상속되며 부모 요소에 배치할 수 있습니다.
* 자식 요소의 헤더 선언은 부모 요소의 선언을 재정의합니다.
