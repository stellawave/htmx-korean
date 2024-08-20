+++
title = "Keyboard Shortcuts"
template = "demo.html"
+++

이 예시에서는 특정 동작을 위한 키보드 단축키를 만드는 방법을 보여줍니다.

먼저 서버에서 콘텐츠를 로드하는 간단한 버튼을 만듭니다:

```html
<button class="btn primary" hx-trigger="click, keyup[altKey&&shiftKey&&key=='D'] from:body"
        hx-post="/doit">Do It! (alt-shift-D)</button>
```

이 버튼은 일반적인 `click` 이벤트뿐만 아니라 `alt-shift-D` 키가 눌렸을 때 발생하는 `keyup` 이벤트에도 반응합니다. 
`from:` 수정자는 `keyup` 이벤트를 `body` 요소에서 듣도록 설정하여 이 키보드 단축키를 "전역"으로 만듭니다.

아래의 데모에서 버튼을 클릭하거나 `alt-shift-D`를 눌러 트리거할 수 있습니다.

지정된 키보드 단축키에 필요한 조건을 확인할 수 있는 링크는 다음과 같습니다:

[https://javascript.info/keyboard-events](https://javascript.info/keyboard-events)

{{ demoenv() }}

<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/init", function(request, params){
        return "<button class='btn primary' style='font-size:20pt' hx-trigger='click, keyup[altKey&&shiftKey&&key==\"D\"] from:body'" +
                      " hx-post='/doit'>Do It! (alt-shift-D) </button>";
    });

    onPost("/doit", function (request, params) {
        return "Did it!";
    });

</script>
