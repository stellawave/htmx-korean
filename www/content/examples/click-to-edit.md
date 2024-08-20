+++
title = "Click to Edit"
template = "demo.html"
+++

클릭하여 편집하는 패턴은 페이지 새로고침 없이 레코드의 일부 또는 전체를 인라인으로 편집할 수 있는 방법을 제공합니다.

* 이 패턴은 연락처의 세부 정보를 표시하는 UI로 시작합니다. 이 `div`에는 `/contact/1/edit`에서 연락처 편집 UI를 가져올 버튼이 있습니다.

```html
<div hx-target="this" hx-swap="outerHTML">
    <div><label>First Name</label>: Joe</div>
    <div><label>Last Name</label>: Blow</div>
    <div><label>Email</label>: joe@blow.com</div>
    <button hx-get="/contact/1/edit" class="btn primary">
    Click To Edit
    </button>
</div>
```

* 이것은 연락처를 편집할 수 있는 form을 반환합니다.

```html
<form hx-put="/contact/1" hx-target="this" hx-swap="outerHTML">
  <div>
    <label>First Name</label>
    <input type="text" name="firstName" value="Joe">
  </div>
  <div class="form-group">
    <label>Last Name</label>
    <input type="text" name="lastName" value="Blow">
  </div>
  <div class="form-group">
    <label>Email Address</label>
    <input type="email" name="email" value="joe@blow.com">
  </div>
  <button class="btn">Submit</button>
  <button class="btn" hx-get="/contact/1">Cancel</button>
</form>
```

* 이 form은 일반적인 REST-ful 패턴에 따라 `/contact/1`로 `PUT` 요청을 보냅니다.

{{ demoenv() }}

<script>
    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // data
    var contact = {
        "firstName" : "Joe",
        "lastName" : "Blow",
        "email" : "joe@blow.com"
    };

    // routes
    init("/contact/1", function(request){
        return displayTemplate(contact);
    });

    onGet("/contact/1/edit", function(request){
        return formTemplate(contact);
    });

    onPut("/contact/1", function (req, params) {
        contact.firstName = params['firstName'];
        contact.lastName = params['lastName'];
        contact.email = params['email'];
        return displayTemplate(contact);
    });

    // templates
    function formTemplate(contact) {
return `<form hx-put="/contact/1" hx-target="this" hx-swap="outerHTML">
  <div>
    <label for="firstName">First Name</label>
    <input autofocus type="text" id="firstName" name="firstName" value="${contact.firstName}">
  </div>
  <div class="form-group">
    <label for="lastName">Last Name</label>
    <input type="text" id="lastName" name="lastName" value="${contact.lastName}">
  </div>
  <div class="form-group">
    <label for="email">Email Address</label>
    <input type="email" id="email" name="email" value="${contact.email}">
  </div>
  <button class="btn" type="submit">Submit</button>
  <button class="btn" hx-get="/contact/1">Cancel</button>
</form>`
    }

    function displayTemplate(contact) {
        return `<div hx-target="this" hx-swap="outerHTML">
    <div><label>First Name</label>: ${contact.firstName}</div>
    <div><label>Last Name</label>: ${contact.lastName}</div>
    <div><label>Email Address</label>: ${contact.email}</div>
    <button hx-get="/contact/1/edit" class="btn primary">
    Click To Edit
    </button>
</div>`;
    }
</script>
