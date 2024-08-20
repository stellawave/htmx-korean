+++
title = "Tabs (Using JavaScript)"
template = "demo.html"
+++

이 예제에서는 htmx를 사용하여 탭 내용을 로드하고, 자바스크립트를 사용하여 "활성" 탭을 선택하는 방법을 보여줍니다. 
이를 통해 탭 HTML을 다시 렌더링하는 작업의 일부를 애플리케이션 서버에서 클라이언트의 브라우저로 오프로드하여 중복을 줄일 수 있습니다.

[Hypertext As The Engine Of Application State](https://en.wikipedia.org/wiki/HATEOAS) 원칙을 따르는 
[더 정통적인 접근 방식](https://htmx.org/examples/tabs-hateoas/)을 고려해 볼 수도 있습니다.

## 예제 코드

아래 HTML은 탭 목록을 표시하며, htmx를 추가하여 서버에서 각 탭 패널을 동적으로 로드합니다. 
간단한 JavaScript 이벤트 핸들러는 [`take` 함수](https://hyperscript.org/commands/take/)를 사용하여 콘텐츠가 DOM에 스왑될 때 선택된 탭을 전환합니다.

```html

<div id="tabs" hx-target="#tab-contents" role="tablist"
     hx-on:htmx-after-on-load="let currentTab = document.querySelector('[aria-selected=true]');
                               currentTab.setAttribute('aria-selected', 'false')
                               currentTab.classList.remove('selected')
                               let newTab = event.target
                               newTab.setAttribute('aria-selected', 'true')
                               newTab.classList.add('selected')">
    <button role="tab" aria-controls="tab-contents" aria-selected="true" hx-get="/tab1" class="selected">Tab 1</button>
    <button role="tab" aria-controls="tab-contents" aria-selected="false" hx-get="/tab2">Tab 2</button>
    <button role="tab" aria-controls="tab-contents" aria-selected="false" hx-get="/tab3">Tab 3</button>
</div>

<div id="tab-contents" role="tabpanel" hx-get="/tab1" hx-trigger="load"></div>
```

{{ demoenv() }}

<div id="tabs" hx-target="#tab-contents" role="tablist"
     hx-on:htmx-after-on-load="console.log(event)
                               let currentTab = document.querySelector('[aria-selected=true]');
                                          currentTab.setAttribute('aria-selected', 'false')
                                          currentTab.classList.remove('selected')
                                          let newTab = event.target
                                          newTab.setAttribute('aria-selected', 'true')
                                          newTab.classList.add('selected')">
	<button role="tab" aria-controls="tab-contents" aria-selected="true" hx-get="/tab1" class="selected">Tab 1</button>
	<button role="tab" aria-controls="tab-contents" aria-selected="false" hx-get="/tab2">Tab 2</button>
	<button role="tab" aria-controls="tab-contents" aria-selected="false" hx-get="/tab3">Tab 3</button>
</div>

<div id="tab-contents" role="tabpanel" hx-get="/tab1" hx-trigger="load"></div>

<script src="https://unpkg.com/hyperscript.org"></script>
<script>
	onGet("/tab1", function() {
		return `
			<p>Commodo normcore truffaut VHS duis gluten-free keffiyeh iPhone taxidermy godard ramps anim pour-over.
			Pitchfork vegan mollit umami quinoa aute aliquip kinfolk eiusmod live-edge cardigan ipsum locavore.
			Polaroid duis occaecat narwhal small batch food truck.
			PBR&B venmo shaman small batch you probably haven't heard of them hot chicken readymade.
			Enim tousled cliche woke, typewriter single-origin coffee hella culpa.
			Art party readymade 90's, asymmetrical hell of fingerstache ipsum.</p>
		`});
	onGet("/tab2", function() {
		return `
			<p>Kitsch fanny pack yr, farm-to-table cardigan cillum commodo reprehenderit plaid dolore cronut meditation.
			Tattooed polaroid veniam, anim id cornhole hashtag sed forage.
			Microdosing pug kitsch enim, kombucha pour-over sed irony forage live-edge.
			Vexillologist eu nulla trust fund, street art blue bottle selvage raw denim.
			Dolore nulla do readymade, est subway tile affogato hammock 8-bit.
			Godard elit offal pariatur you probably haven't heard of them post-ironic.
			Prism street art cray salvia.</p>
		`
	});
	onGet("/tab3", function() {
		return `
			<p>Aute chia marfa echo park tote bag hammock mollit artisan listicle direct trade.
			Raw denim flexitarian eu godard etsy.
			Poke tbh la croix put a bird on it fixie polaroid aute cred air plant four loko gastropub swag non brunch.
			Iceland fanny pack tumeric magna activated charcoal bitters palo santo laboris quis consectetur cupidatat portland aliquip venmo.</p>
		`
	});

</script>

<style>

	#demo-canvas {
		display:none;
	}

	#tabs {
	}

	#tabs > button {
		border: none;
		display: inline-block;
		padding: 5px 10px;
		cursor:pointer;
		background-color: transparent;
		border: solid 3px rgba(0,0,0,0);
		border-bottom: solid 3px #eee;
	}

	#tabs > button:hover {
		color: var(--midBlue);
	}

	#tabs > button.selected {
		border: solid 3px var(--midBlue);
	}

	#tab-contents {
		padding:10px;
	}
</style>
