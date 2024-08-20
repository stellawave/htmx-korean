+++
title = "Dialogs"
template = "demo.html"
+++

다이얼로그는 [`hx-prompt`](@/attributes/hx-prompt.md)와 [`hx-confirm`](@/attributes/hx-confirm.md) 속성을 사용하여 트리거될 수 있습니다. 
이러한 속성들은 AJAX 요청을 트리거하는 사용자 상호작용에 의해 활성화되며, 다이얼로그가 승인된 경우에만 요청이 전송됩니다.

```html
<div>
  <button class="btn primary"
          hx-post="/submit"
          hx-prompt="Enter a string"
          hx-confirm="Are you sure?"
          hx-target="#response">
    Prompt Submission
  </button>
  <div id="response"></div>
</div>
```

사용자가 프롬프트 다이얼로그에 입력한 값은 `HX-Prompt` 헤더로 서버에 전송됩니다. 이 예제에서는 서버가 단순히 사용자의 입력값을 그대로 반환합니다.

```html
User entered <i>${response}</i>
```

{{ demoenv() }}

<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params){
      return submitButton();
    });

    onPost("/submit", function(request, params){
        var response = request.requestHeaders['HX-Prompt'];
        return promptSubmit(response);
    });

    // templates
    function submitButton() {
      return `<div>
  <button class="btn primary"
          hx-post="/submit"
          hx-prompt="Enter a string"
          hx-confirm="Are you sure?"
          hx-target="#response">
    Prompt Submission
  </button>
  <div id="response"></div>
</div>`;
    }

    function promptSubmit(response) {
        return `User entered <i>${response}</i>`;
    }
</script>
