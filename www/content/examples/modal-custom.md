+++
title = "Custom Modal Dialogs"
template = "demo.html"
+++

htmx는 [Bootstrap](@/examples/modal-bootstrap.md)이나 [UIKit](@/examples/modal-uikit.md)과 같은 CSS 프레임워크에 내장된 다이얼로그와 잘 작동하지만, 
htmx를 사용하면 모달 다이얼로그를 처음부터 쉽게 만들 수도 있습니다. 
여기에서는 모달 다이얼로그를 만드는 한 가지 방법을 간단히 예시로 보여드립니다.

최종 결과의 데모를 보려면 여기를 클릭하세요:

```html
<button class="btn primary" hx-get="/modal" hx-target="body" hx-swap="beforeend">Open a Modal</button>
```

## 전반적인 계획

우리는 서버에서 원격 콘텐츠를 로드한 다음, 이를 모달 다이얼로그에 표시하는 버튼을 만들 것입니다. 모달 콘텐츠는 `<body>` 요소의 끝에 `#modal`이라는 이름의 `div`에 추가될 것입니다.

이 데모에서는 CSS로 멋진 애니메이션을 정의한 다음, 사용자가 완료되었을 때 모달을 DOM에서 제거하기 위해 [Hyperscript](https://hyperscript.org)를 사용할 것입니다. 
htmx와 함께 Hyperscript를 반드시 사용할 필요는 없지만, 
두 가지는 함께 사용하도록 설계되었으며 비동기 및 이벤트 중심의 코드를 작성하는 데 JavaScript보다 훨씬 더 편리하므로 이 예시에서는 Hyperscript를 선택했습니다.

## Main Page HTML

```html
<button class="btn primary" hx-get="/modal" hx-target="body" hx-swap="beforeend">Open a Modal</button>
```

## Modal HTML Fragment
```html
<div id="modal" _="on closeModal add .closing then wait for animationend then remove me">
	<div class="modal-underlay" _="on click trigger closeModal"></div>
	<div class="modal-content">
		<h1>Modal Dialog</h1>
		This is the modal content.
		You can put anything here, like text, or a form, or an image.
		<br>
		<br>
		<button class="btn danger" _="on click trigger closeModal">Close</button>
	</div>
</div>
```

## Custom Stylesheet
```css
/***** MODAL DIALOG ****/
#modal {
	/* Underlay covers entire screen. */
	position: fixed;
	top:0px;
	bottom: 0px;
	left:0px;
	right:0px;
	background-color:rgba(0,0,0,0.5);
	z-index:1000;

	/* Flexbox centers the .modal-content vertically and horizontally */
	display:flex;
	flex-direction:column;
	align-items:center;

	/* Animate when opening */
	animation-name: fadeIn;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal > .modal-underlay {
	/* underlay takes up the entire viewport. This is only
	required if you want to click to dismiss the popup */
	position: absolute;
	z-index: -1;
	top:0px;
	bottom:0px;
	left: 0px;
	right: 0px;
}

#modal > .modal-content {
	/* Position visible dialog near the top of the window */
	margin-top:10vh;

	/* Sizing for visible dialog */
	width:80%;
	max-width:600px;

	/* Display properties for visible dialog*/
	border:solid 1px #999;
	border-radius:8px;
	box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.3);
	background-color:white;
	padding:20px;

	/* Animate when opening */
	animation-name:zoomIn;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal.closing {
	/* Animate when closing */
	animation-name: fadeOut;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal.closing > .modal-content {
	/* Animate when closing */
	animation-name: zoomOut;
	animation-duration:150ms;
	animation-timing-function: ease;
}

@keyframes fadeIn {
	0% {opacity: 0;}
	100% {opacity: 1;}
}

@keyframes fadeOut {
	0% {opacity: 1;}
	100% {opacity: 0;}
}

@keyframes zoomIn {
	0% {transform: scale(0.9);}
	100% {transform: scale(1);}
}

@keyframes zoomOut {
	0% {transform: scale(1);}
	100% {transform: scale(0.9);}
}
```

<script src="https://unpkg.com/htmx.org"></script>
<script src="https://unpkg.com/hyperscript.org"></script>
<script type="text/javascript">

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/modal", function(request){
		return `
		<div id="modal" _="on closeModal add .closing wait for animationend then remove me">
			<div class="modal-underlay" _="on click trigger closeModal"></div>
			<div class="modal-content">
				<h1>Modal Dialog</h1>
				This is the modal content.
				You can put anything here, like text, or a form, or an image.
				<br>
				<br>
				<button class="btn danger" _="on click trigger closeModal">Close</button>
			</div>
		</div>
		`
      });
</script>

<style>
/***** MODAL DIALOG ****/

#modal {
	/* Underlay covers entire screen. */
	position: fixed;
	top:0px;
	bottom: 0px;
	left:0px;
	right:0px;
	background-color:rgba(0,0,0,0.5);
	z-index:1000;

	/* Flexbox centers the .modal-content vertically and horizontally */
	display:flex;
	flex-direction:column;
	align-items:center;

	/* Animate when opening */
	animation-name: fadeIn;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal > .modal-underlay {
	/* underlay takes up the entire viewport. This is only
	required if you want to click to dismiss the popup */
	position: absolute;
	z-index: -1;
	top:0px;
	bottom:0px;
	left: 0px;
	right: 0px;
}

#modal > .modal-content {
	/* Position visible dialog near the top of the window */
	margin-top:10vh;

	/* Sizing for visible dialog */
	width:80%;
	max-width:600px;

	/* Display properties for visible dialog*/
	border:solid 1px #999;
	border-radius:8px;
	box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.3);
	background-color:white;
	padding:20px;

	/* Animate when opening */
	animation-name:zoomIn;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal.closing {
	/* Animate when closing */
	animation-name: fadeOut;
	animation-duration:150ms;
	animation-timing-function: ease;
}

#modal.closing > .modal-content {
	/* Animate when closing */
	animation-name: zoomOut;
	animation-duration:150ms;
	animation-timing-function: ease;
}

@keyframes fadeIn {
	0% {opacity: 0;}
	100% {opacity: 1;}
}

@keyframes fadeOut {
	0% {opacity: 1;}
	100% {opacity: 0;}
}

@keyframes zoomIn {
	0% {transform: scale(0.9);}
	100% {transform: scale(1);}
}

@keyframes zoomOut {
	0% {transform: scale(1);}
	100% {transform: scale(0.9);}
}
</style>
