+++
title = "Tabs (Using HATEOAS)"
template = "demo.html"
+++

이 예는 htmx를 사용하여 탭을 구현하는 것이 얼마나 쉬운지 보여줍니다.  
[애플리케이션 상태의 엔진으로서의 하이퍼텍스트](https://en.wikipedia.org/wiki/HATEOAS)의 원칙에 따라 선택한 탭은 애플리케이션 상태의 일부가 됩니다.  
따라서 애플리케이션에서 탭을 표시하고 선택하려면 반환된 HTML에 탭 마크업을 포함하기만 하면 됩니다.  
이 방법이 애플리케이션 서버 디자인에 적합하지 않은 경우 [자바스크립트로 탭 선택하기](@/examples/tabs-javascript.md)를 사용할 수도 있습니다.

## Example Code (Main Page)
메인 페이지에는 초기 탭을 DOM에 로드하는 다음 HTML이 포함되어 있습니다.
```html
<div id="tabs" hx-get="/tab1" hx-trigger="load delay:100ms" hx-target="#tabs" hx-swap="innerHTML"></div>
```

## Example Code (Each Tab)
이후 탭 페이지에는 모든 탭이 표시되고 선택한 탭이 그에 따라 강조 표시됩니다.

```html
<div class="tab-list" role="tablist">
	<button hx-get="/tab1" class="selected" role="tab" aria-selected="true" aria-controls="tab-content">Tab 1</button>
	<button hx-get="/tab2" role="tab" aria-selected="false" aria-controls="tab-content">Tab 2</button>
	<button hx-get="/tab3" role="tab" aria-selected="false" aria-controls="tab-content">Tab 3</button>
</div>

<div id="tab-content" role="tabpanel" class="tab-content">
	Commodo normcore truffaut VHS duis gluten-free keffiyeh iPhone taxidermy godard ramps anim pour-over.
	Pitchfork vegan mollit umami quinoa aute aliquip kinfolk eiusmod live-edge cardigan ipsum locavore.
	Polaroid duis occaecat narwhal small batch food truck.
	PBR&B venmo shaman small batch you probably haven't heard of them hot chicken readymade.
	Enim tousled cliche woke, typewriter single-origin coffee hella culpa.
	Art party readymade 90's, asymmetrical hell of fingerstache ipsum.
</div>
```

{{ demoenv() }}

<div id="tabs" hx-target="this" hx-swap="innerHTML">
		<div class="tab-list" role="tablist">
			<button hx-get="/tab1" class="selected" role="tab" aria-selected="true" aria-controls="tab-content">Tab 1</button>
			<button hx-get="/tab2" role="tab" aria-selected="false" aria-controls="tab-content">Tab 2</button>
			<button hx-get="/tab3" role="tab" aria-selected="false" aria-controls="tab-content">Tab 3</button>
		</div>
		<div id="tab-content" role="tabpanel" class="tab-content">
			Commodo normcore truffaut VHS duis gluten-free keffiyeh iPhone taxidermy godard ramps anim pour-over.
			Pitchfork vegan mollit umami quinoa aute aliquip kinfolk eiusmod live-edge cardigan ipsum locavore.
			Polaroid duis occaecat narwhal small batch food truck.
			PBR&B venmo shaman small batch you probably haven't heard of them hot chicken readymade.
			Enim tousled cliche woke, typewriter single-origin coffee hella culpa.
			Art party readymade 90's, asymmetrical hell of fingerstache ipsum.
		</div>
</div>


<script>
	onGet("/tab1", function() {
		return `
		<div class="tab-list" role="tablist">
			<button hx-get="/tab1" class="selected" aria-selected="true" autofocus role="tab" aria-controls="tab-content">Tab 1</button>
			<button hx-get="/tab2" role="tab" aria-selected="false" aria-controls="tab-content">Tab 2</button>
			<button hx-get="/tab3" role="tab" aria-selected="false" aria-controls="tab-content">Tab 3</button>
		</div>

		<div id="tab-content" role="tabpanel" class="tab-content">
			Commodo normcore truffaut VHS duis gluten-free keffiyeh iPhone taxidermy godard ramps anim pour-over.
			Pitchfork vegan mollit umami quinoa aute aliquip kinfolk eiusmod live-edge cardigan ipsum locavore.
			Polaroid duis occaecat narwhal small batch food truck.
			PBR&B venmo shaman small batch you probably haven't heard of them hot chicken readymade.
			Enim tousled cliche woke, typewriter single-origin coffee hella culpa.
			Art party readymade 90's, asymmetrical hell of fingerstache ipsum.
		</div>`
	})

	onGet("/tab2", function() {
		return `
		<div class="tab-list" role="tablist">
			<button hx-get="/tab1" role="tab" aria-selected="false" aria-controls="tab-content">Tab 1</button>
			<button hx-get="/tab2" class="selected" aria-selected="true" autofocus role="tab" aria-controls="tab-content">Tab 2</button>
			<button hx-get="/tab3" role="tab" aria-selected="false" aria-controls="tab-content">Tab 3</button>
		</div>

		<div id="tab-content" role="tabpanel" class="tab-content">
			Kitsch fanny pack yr, farm-to-table cardigan cillum commodo reprehenderit plaid dolore cronut meditation.
			Tattooed polaroid veniam, anim id cornhole hashtag sed forage.
			Microdosing pug kitsch enim, kombucha pour-over sed irony forage live-edge.
			Vexillologist eu nulla trust fund, street art blue bottle selvage raw denim.
			Dolore nulla do readymade, est subway tile affogato hammock 8-bit.
			Godard elit offal pariatur you probably haven't heard of them post-ironic.
			Prism street art cray salvia.
		</div>`
	})

	onGet("/tab3", function() {
		return `
		<div class="tab-list" role="tablist">
			<button hx-get="/tab1" role="tab" aria-selected="false" aria-controls="tab-content">Tab 1</button>
			<button hx-get="/tab2" role="tab" aria-selected="false" aria-controls="tab-content">Tab 2</button>
			<button hx-get="/tab3" class="selected" aria-selected="true" autofocus role="tab" aria-controls="tab-content">Tab 3</button>
		</div>

		<div id="tab-content" role="tabpanel" class="tab-content">
			Aute chia marfa echo park tote bag hammock mollit artisan listicle direct trade.
			Raw denim flexitarian eu godard etsy.
			Poke tbh la croix put a bird on it fixie polaroid aute cred air plant four loko gastropub swag non brunch.
			Iceland fanny pack tumeric magna activated charcoal bitters palo santo laboris quis consectetur cupidatat portland aliquip venmo.
		</div>`
	})

</script>

<style>
	#demo-canvas {
		display:none;
	}

	#tabs > .tab-list button {
		border: none;
		display: inline-block;
		padding: 5px 10px;
		cursor:pointer;
		background-color: transparent;
		color: var(--textColor);
		border: solid 3px rgba(0,0,0,0);
		border-bottom: solid 3px #eee;
	}

	#tabs > .tab-list button:hover {
		color: var(--midBlue);
	}

	#tabs > .tab-list button.selected {
		border: solid 3px var(--midBlue);
	}

	#tabs > .tab-content {
		padding:10px;
		margin-bottom:100px;
	}
</style>
