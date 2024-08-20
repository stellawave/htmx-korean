+++
title = "Inline Validation"
template = "demo.html"
+++

이 예시는 이메일 주소의 인라인 필드 유효성 검사를 수행하는 방법을 보여줍니다. 
이를 위해 유효성을 검사할 값을 서버에 `POST`하고, 그 결과로 DOM을 업데이트하는 입력 필드가 있는 폼을 만들어야 합니다.

다음과 같은 폼으로 시작합니다:

```html
<h3>Signup Form</h3>
<form hx-post="/contact">
  <div hx-target="this" hx-swap="outerHTML">
    <label>Email Address</label>
    <input name="email" hx-post="/contact/email" hx-indicator="#ind">
    <img id="ind" src="/img/bars.svg" class="htmx-indicator"/>
  </div>
  <div class="form-group">
    <label>First Name</label>
    <input type="text" class="form-control" name="firstName">
  </div>
  <div class="form-group">
    <label>Last Name</label>
    <input type="text" class="form-control" name="lastName">
  </div>
  <button class="btn primary">Submit</button>
</form>
```

폼의 첫 번째 `div`는 요청의 타겟으로 설정되어 있으며, `outerHTML` 스왑 전략이 지정되어 있어 응답에 의해 완전히 교체됩니다. 
입력 필드는 변경 이벤트가 발생할 때 유효성 검사를 위해 `/contact/email`로 `POST`를 보내도록 지정되어 있습니다(이것이 입력 필드의 기본 동작입니다). 
또한, 요청에 대한 인디케이터를 지정합니다.

요청이 발생하면, 바깥 `div`를 교체하는 부분 응답을 반환합니다. 그 예시는 다음과 같습니다:

```html
<div hx-target="this" hx-swap="outerHTML" class="error">
  <label>Email Address</label>
  <input name="email" hx-post="/contact/email" hx-indicator="#ind" value="test@foo.com">
  <img id="ind" src="/img/bars.svg" class="htmx-indicator"/>
  <div class='error-message'>That email is already taken. Please enter another email.</div>
</div>
```

이 `div`는 `error` 클래스로 주석이 달려 있으며, 오류 메시지 요소를 포함합니다.

이 폼은 다음 CSS를 사용하여 간단히 스타일링할 수 있습니다:

```css
  .error-message {
    color:red;
  }
  .error input {
      box-shadow: 0 0 3px #CC0000;
   }
  .valid input {
      box-shadow: 0 0 3px #36cc00;
   }
```

이렇게 하면 더 나은 시각적 피드백을 제공할 수 있습니다.

아래는 이 예시의 작동하는 데모입니다. 유효한 이메일로는 `test@test.com`만 허용됩니다.

<style>
  .error-message {
    color:red;
  }
  .error input {
      box-shadow: 0 0 3px #CC0000;
   }
  .valid input {
      box-shadow: 0 0 3px #36cc00;
   }
</style>

{{ demoenv() }}

<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params){
      return demoTemplate();
    });

    onPost("/contact", function(request, params){
      return formTemplate();
    });

    onPost(/\/contact\/email.*/, function(request, params){
        var email = params['email'];
        if(!/\S+@\S+\.\S+/.test(email)) {
          return emailInputTemplate(email, "Please enter a valid email address");
        } else if(email != "test@test.com") {
          return emailInputTemplate(email, "That email is already taken.  Please enter another email.");
        } else {
          return emailInputTemplate(email);
        }
     });

    // templates
    function demoTemplate() {

        return `<h3>Signup Form</h3><p>Enter an email into the input below and on tab out it will be validated.  Only "test@test.com" will pass.</p> ` + formTemplate();
    }

    function formTemplate() {
      return `<form hx-post="/contact">
  <div hx-target="this" hx-swap="outerHTML">
    <label for="email">Email Address</label>
    <input name="email" id="email" hx-post="/contact/email" hx-indicator="#ind">
    <img id="ind" src="/img/bars.svg" class="htmx-indicator"/>
  </div>
  <div class="form-group">
    <label for="firstName">First Name</label>
    <input type="text" class="form-control" name="firstName" id="firstName">
  </div>
  <div class="form-group">
    <label for="lastName">Last Name</label>
    <input type="text" class="form-control" name="lastName" id="lastName">
  </div>
  <button type='submit' class="btn primary" disabled>Submit</button>
</form>`;
    }

        function emailInputTemplate(val, errorMsg) {
            return `<div hx-target="this" hx-swap="outerHTML" class="${errorMsg ? "error" : "valid"}">
  <label>Email Address</label>
  <input name="email" hx-post="/contact/email" hx-indicator="#ind" value="${val}" aria-invalid="${!!errorMsg}">
  <img id="ind" src="/img/bars.svg" class="htmx-indicator"/>
  ${errorMsg ? (`<div class='error-message' >${errorMsg}</div>`) : ""}
</div>`;
        }
</script>
