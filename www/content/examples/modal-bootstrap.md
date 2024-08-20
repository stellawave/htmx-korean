+++
title = "Modal Dialogs in Bootstrap"
template = "demo.html"
+++

많은 CSS 툴킷에는 모달 대화 상자를 만들기 위한 스타일(및 자바스크립트)이 포함되어 있습니다.
이 예는 부트스트랩에서 제공하는 원본 JavaScript와 함께 HTMX를 사용하는 방법을 보여줍니다.

먼저 대화 상자를 트리거하는 버튼과 마크업 하단에 대화 상자가 로드되는 DIV가 있습니다:

```html
<button
    hx-get="/modal"
    hx-target="#modals-here"
    hx-trigger="click"
    data-bs-toggle="modal"
    data-bs-target="#modals-here"
    class="btn primary">Open Modal</button>

<div id="modals-here"
    class="modal modal-blur fade"
    style="display: none"
    aria-hidden="false"
    tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered" role="document">
        <div class="modal-content"></div>
    </div>
</div>
```

이 버튼을 클릭하면 `/modal`에 `GET` 요청을 보냅니다.  
응답의 내용은 `#modals-here` DIV 아래의 DOM에 추가됩니다.

서버는 부트스트랩의 표준 모달을 약간 수정한 버전으로 응답합니다.

```html
<div class="modal-dialog modal-dialog-centered">
  <div class="modal-content">
    <div class="modal-header">
      <h5 class="modal-title">Modal title</h5>
    </div>
    <div class="modal-body">
      <p>Modal body text goes here.</p>
    </div>
    <div class="modal-footer">
      <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
    </div>
  </div>
</div>
```

<div id="modals-here"
class="modal modal-blur fade"
style="display: none"
aria-hidden="false"
tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered" role="document">
        <div class="modal-content"></div>
    </div>
</div>

{{ demoenv() }}

<style>
	@import "https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/5.2.2/css/bootstrap.min.css";
</style>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
<script>

	//=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params) {
		return `<button
	hx-get="/modal"
	hx-target="#modals-here"
	hx-trigger="click"
    data-bs-toggle="modal"
    data-bs-target="#modals-here"
	class="btn primary">Open Modal</button>
	`})

	onGet("/modal", function(request, params){
	  return `<div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title">Modal title</h5>
        </div>
        <div class="modal-body">
            <p>Modal body text goes here.</p>
        </div>
    <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
    </div>
    </div>
</div>`
});
</script>
