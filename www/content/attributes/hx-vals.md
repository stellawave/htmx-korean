+++
title = "hx-vals"
+++

`hx-vals` 속성을 사용하면 AJAX 요청과 함께 제출할 매개변수를 추가할 수 있습니다. 

기본적으로 이 속성의 값은 [JSON (JavaScript Object Notation)](https://www.json.org/json-en.html) 형식의 
이름-표현식 값 목록입니다.

`hx-vals`가 주어진 값을 *평가하도록* 하려면 값 앞에 `javascript:` 또는 `js:` 를 붙이면 됩니다.

```html
  <div hx-get="/example" hx-vals='{"myVal": "My Value"}'>Get Some HTML, Including A Value in the Request</div>

  <div hx-get="/example" hx-vals='js:{myVal: calculateValue()}'>Get Some HTML, Including a Dynamic Value from Javascript in the Request</div>
```

평가된 코드를 사용하면 `event` 객체에 액세스할 수 있습니다. 이 예제에는 입력 내에서 마지막으로 입력한 key의 값이 포함됩니다.

```html
  <div hx-get="/example" hx-trigger="keyup" hx-vals='js:{lastKey: event.key}'>
    <input type="text" />
  </div>
```

## Security Considerations

* 기본적으로 `hx-vals`의 값은 유효한 [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON)이어야 합니다. 동적으로 계산되지 **않습니다**. 
`javascript:` 접두사를 사용하는 경우, 특히 쿼리 문자열이나 사용자 생성 콘텐츠와 같은 사용자 입력을 처리할 때 
[Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) 취약점이 발생할 수 있는 보안 고려 사항이 발생하므로 주의하세요.

## Notes

* `hx-vals`는 상속되며 부모 요소에 배치할 수 있습니다.
* 자식에서 변수의 선언을 해서 부모 선언을 override할 수 있습니다.
* `hx-vals`에서의 변수 선언과 input 값의 이름이 같은 경우, 변수 선언이 우선됩니다.
