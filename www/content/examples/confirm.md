+++
title = "A Customized Confirmation UI"
template = "demo.html"
+++

htmx는 [`hx-confirm`](@/attributes/hx-confirm.md) 속성을 지원하여 사용자의 작업을 확인하는 간단한 메커니즘을 제공합니다. 
이 속성은 기본적으로 자바스크립트의 `confirm()` 함수를 사용하지만, 이는 신뢰할 만한 기능이긴 해도 애플리케이션의 UX와 일치하지 않을 수 있습니다.

이 예제에서는 [sweetalert2](https://sweetalert2.github.io)를 사용하여 사용자 정의 확인 대화 상자를 구현하는 방법을 보여줍니다. 
아래에는 클릭+커스텀 이벤트 방법을 사용하는 예제와 내장된 `hx-confirm` 속성과 [`htmx:confirm`](@/events.md#htmx:confirm) 이벤트를 사용하는 예제가 있습니다.

## 클릭+커스텀 이벤트 사용

```html
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
<button hx-get="/confirmed" 
        hx-trigger='confirmed'
        onClick="Swal.fire({title: 'Confirm', text:'Do you want to continue?'}).then((result)=>{
            if(result.isConfirmed){
              htmx.trigger(this, 'confirmed');  
            } 
        })">
  Click Me
</button>
```

여기서는 자바스크립트를 사용하여 클릭 시 Sweet Alert 2를 표시하고 사용자의 확인을 요청합니다. 
사용자가 대화 상자를 확인하면, 커스텀 "confirmed" 이벤트를 트리거하여 요청을 실행합니다. 이 이벤트는 `hx-trigger`에 의해 포착됩니다.

## Vanilla JS, hx-confirm 사용

```html
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
<script>
  document.addEventListener("htmx:confirm", function(e) {
    e.preventDefault()
    Swal.fire({
      title: "Proceed?",
      text: `I ask you... ${e.detail.question}`
    }).then(function(result) {
      if(result.isConfirmed) e.detail.issueRequest(true) // use true to skip window.confirm
    })
  })
</script>
  
<button hx-get="/confirmed" hx-confirm="Some confirm text here">
  Click Me
</button>
```

이 방법에서는 자바스크립트를 추가하여 클릭 시 Sweet Alert 2를 호출하고 확인을 요청합니다. 
사용자가 대화 상자를 확인하면 `issueRequest` 메서드를 호출하여 요청을 트리거합니다. 이때 `skipConfirmation=true`를 인수로 전달하여 `window.confirm`을 건너뛰도록 합니다.

이 방식은 프롬프트에서 `hx-confirm`의 값을 사용할 수 있게 하여, 예를 들어 Django 리스트처럼 요소에 따라 질문이 달라질 때 편리하게 사용할 수 있습니다.

```html
{% for client in clients %}
<button hx-post="/delete/{{client.pk}}" hx-confirm="Delete {{client.name}}??">Delete</button>
{% endfor %}
```
