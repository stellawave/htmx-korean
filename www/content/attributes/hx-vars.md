+++
title = "hx-vars"
+++

**NOTE: `hx-vars`는 더 안전한 [`hx-vals`](@/attributes/hx-vals.md)에 의해 대체되었습니다**

`hx-vars` 속성을 사용하면 AJAX 요청과 함께 제출될 매개변수에 동적으로 추가할 수 있습니다.

이 속성의 값은 자바스크립트 [Object Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals)의 내부 구문과 동일하게 
쉼표로 구분된 `name`:`<expression>` 값의 목록입니다.

```html
  <div hx-get="/example" hx-vars="myVar:computeMyVar()">Get Some HTML, Including A Dynamic Value in the Request</div>
```

## Security Considerations

* `hx-vars`의 표현식은 동적으로 계산되므로 실행될 JavaScript 코드를 추가할 수 있습니다. 표현식에 포함된 사용자 입력은 [Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) 취약점으로 이어질 수 있으므로 **절대** 신뢰하지 않도록 주의하세요. 쿼리 문자열이나 사용자 생성 콘텐츠와 같은 사용자 입력을 처리하는 경우 더 안전한 대안인 hx-vals를 사용하는 것이 좋습니다.

## Notes

* `hx-vars`는 상속되며 부모 요소에 배치할 수 있습니다.
* 자식에서 변수의 선언을 해서 부모 선언을 override할 수 있습니다.
* `hx-vars`에서의 변수 선언과 input 값의 이름이 같은 경우, 변수 선언이 우선됩니다.
