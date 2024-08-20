+++
title = "Modal Dialogs with UIKit"
template = "demo.html"
+++

많은 CSS 툴킷에는 모달 대화 상자를 생성하기 위한 스타일(및 JavaScript)이 포함되어 있습니다. 
이 예제에서는 HTMX를 사용하여 동적 대화 상자를 UIKit을 사용하여 표시하는 방법과 JavaScript를 거의 또는 전혀 사용하지 않고도 애니메이션 스타일을 트리거하는 방법을 보여줍니다.

먼저, 대화 상자를 트리거하는 버튼과 대화 상자가 로드될 DIV를 마크업 하단에 추가합니다:

이 예제는 HTMX를 사용하여 원격으로 모달 대화 상자를 로드하고 UIKit을 사용하는 방법을 보여줍니다. 
이 예제에서는 [Hyperscript](https://hyperscript.org)를 사용하여 htmx와 다른 라이브러리들을 얼마나 깔끔하게 결합할 수 있는지 시연합니다.

```html
<button 
	id="showButton"
	hx-get="/uikit-modal.html" 
	hx-target="#modals-here" 
	class="uk-button uk-button-primary" 
	_="on htmx:afterOnLoad wait 10ms then add .uk-open to #modal">Open Modal</button>

<div id="modals-here"></div>
```

이 버튼은 클릭 시 `/uikit-modal.html`에 `GET` 요청을 보냅니다. 이 파일의 내용은 `#modals-here` DIV 아래에 DOM에 추가됩니다.

표준 UIKit JavaScript 라이브러리를 사용하는 대신, 우리는 Hyperscript를 사용하여 UIKit의 부드러운 애니메이션을 트리거합니다. 
애니메이션이 올바르게 실행될 수 있도록 10ms의 지연이 있습니다.

마지막으로, 서버는 UIKit의 표준 모달을 약간 수정한 버전으로 응답합니다:

```html
<div id="modal" class="uk-modal" style="display:block;">
	<div class="uk-modal-dialog uk-modal-body">
		<h2 class="uk-modal-title">Modal Dialog</h2>
		<p>This modal dialog was loaded dynamically by HTMX.</p>

		<form _="on submit take .uk-open from #modal">
			<div class="uk-margin">
				<input class="uk-input" placeholder="What is Your Name?">
			</div>
			<button type="button" class="uk-button uk-button-primary">Save Changes</button>
			<button 
				id="cancelButton"
				type="button" 
				class="uk-button uk-button-default" 
				_="on click take .uk-open from #modal wait 200ms then remove #modal">Close</button>
		</form>
	</div>
</div>
```

버튼과 폼의 Hyperscript는 이 대화 상자가 완료되거나 취소될 때 애니메이션을 트리거합니다. 
Hyperscript를 사용하지 않더라도 모달은 작동하지만 UIKit의 페이드 인 애니메이션이 트리거되지 않습니다.

물론, 이 작업을 위해 JavaScript를 사용할 수도 있지만, 코드가 훨씬 길어집니다.

```javascript

// This triggers the fade-in animation when a modal dialog is loaded and displayed
window.document.getElementById("showButton").addEventListener("htmx:afterOnLoad", function() {
	setTimeout(function(){
		window.document.getElementById("modal").classList.add("uk-open")
	}, 10)
})


// This triggers the fade-out animation when the modal is closed.
window.document.getElementById("cancelButton").addEventListener("click", function() {
	window.document.getElementById("modal").classList.remove("uk-open")
	setTimeout(function(){
		window.document.getElementById("modals-here").innerHTML = ""
		,200
	})
})
```

<div id="modals-here"></div>

{{ demoenv() }}

<style>
	@import "https://cdnjs.cloudflare.com/ajax/libs/uikit/3.5.9/css/uikit-core.min.css";
</style>

<script src="https://unpkg.com/hyperscript.org"></script>
<script>
    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params) {
		return `
<button 
	class="uk-button uk-button-primary" 
	hx-get="/modal" 
	hx-trigger="click" 
	hx-target="#modals-here"
	_="on htmx:afterOnLoad wait 10ms then add .uk-open to #modal">Show Modal Dialog</button>`
	})
		
	onGet("/modal", function(request, params){
	  return `
<div id="modal" class="uk-modal" style="display:block;">
	<div class="uk-modal-dialog uk-modal-body">
		<h2 class="uk-modal-title">Modal Dialog</h2>
		<p>This modal dialog was loaded dynamically by HTMX.  You can put any server request here and you don't (necessarily) need to use the UIKit Javascript file to make it work</p>

		<form _="on submit take .uk-open from #modal">
			<div class="uk-margin">
				<input class="uk-input" placeholder="What is Your Name?">
			</div>

			<div class="uk-margin">
				<input class="uk-input" placeholder="What is Your Quest?">
			</div>

			<div class="uk-margin">
				<input class="uk-input" placeholder="What is Your Favorite Color?">
			</div>

			<button type="button" class="uk-button uk-button-primary" _="on click call alert('submit to server and close dialog.')">Save Changes</button>
			<button type="button" class="uk-button uk-button-default" _="on click take .uk-open from #modal wait 200ms then remove #modal">Close</button>
		</form>
	</div>
</div>`
});
</script>
