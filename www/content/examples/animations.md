+++
title = "Animations"
template = "demo.html"
+++

htmx는 [CSS 전환](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)을 사용하여 
CSS와 HTML만으로 웹 페이지에 부드러운 애니메이션과 전환 효과를 추가할 수 있도록 설계되었습니다. 아래에는 다양한 애니메이션 기법의 몇 가지 예시가 나와 있습니다.

또한 htmx는 새로운 [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)를 사용하여 애니메이션을 생성할 수 있도록 합니다.

### 기본 CSS 애니메이션 {#basic}

### 색상 변화

htmx에서 가장 간단한 애니메이션 기법은 콘텐츠 교체 중에 요소의 `id`를 유지하는 것입니다. 요소의 `id`가 유지되면, 
htmx는 CSS 전환을 통해 해당 요소의 이전 버전과 새 버전 사이에 전환 효과를 작성할 수 있도록 교체를 구조화합니다.

다음의 div를 예로 들어보겠습니다:

```html
<style>
.smooth {
  transition: all 1s ease-in;
}
</style>
<div id="color-demo" class="smooth" style="color:red"
      hx-get="/colors" hx-swap="outerHTML" hx-trigger="every 1s">
  Color Swap Demo
</div>
```

이 div는 매초마다 폴링(polling)하며 `color` 스타일을 새로운 값(예: `blue`)으로 변경하는 새로운 콘텐츠로 교체됩니다:

```html
<div id="color-demo" class="smooth" style="color:blue"
      hx-get="/colors" hx-swap="outerHTML" hx-trigger="every 1s">
  Color Swap Demo
</div>
```

이 div는 `color-demo`라는 안정적인 id를 가지고 있기 때문에, htmx는 교체를 구조화하여 `.smooth` 클래스에 정의된 CSS 전환이 `red`에서 `blue`로 스타일 업데이트에 적용되고, 
두 색상 사이를 부드럽게 전환할 수 있도록 합니다.

#### Demo {#throb-demo}

<style>
.smooth {
  transition: all 1s ease-in;
}
</style>
<div id="color-demo" class="smooth" style="color:red"
      hx-get="/colors" hx-swap="outerHTML" hx-trigger="every 1s">
  Color Swap Demo
</div>

<script>
    var colors = ['blue', 'green', 'orange', 'red'];
    onGet("/colors", function () {
      var color = colors.shift();
      colors.push(color);
      return '<div id="color-demo" hx-get="/colors" hx-swap="outerHTML" class="smooth" hx-trigger="every 1s" style="color:' + color + '">\n'+
             '  Color Swap Demo\n'+
             '</div>\n'
    });
</script>

### Smooth Progress Bar

[진행률 표시줄](@/examples/progress-bar.md) 데모에서는 진행률 표시줄 요소의 `length` 속성을 업데이트하여 부드러운 애니메이션을 구현하는 이 기본 CSS 애니메이션 기법도 사용하고 있습니다.

## Swap Transitions {#swapping}

### Fade Out On Swap

요청이 종료될 때 제거될 요소를 페이드 아웃하려면 일부 CSS를 사용하여 
`htmx-swapping` 클래스를 활용하고 애니메이션이 완료될 때까지 교체 단계를 충분히 길게 연장할 수 있습니다.  
이렇게 하면 됩니다:

```html
<style>
.fade-me-out.htmx-swapping {
  opacity: 0;
  transition: opacity 1s ease-out;
}
</style>
<button class="fade-me-out"
        hx-delete="/fade_out_demo"
        hx-swap="outerHTML swap:1s">
        Fade Me Out
</button>
```

#### Demo {#fade-swap-demo}

<style>
.fade-me-out.htmx-swapping {
  opacity: 0;
  transition: opacity 1s ease-out;
}
</style>

<button class="fade-me-out"
        hx-delete="/fade_out_demo"
        hx-swap="outerHTML swap:1s">
        Delete Me
</button>

<script>
    onDelete("/fade_out_demo", function () {return ""});
</script>

## Settling Transitions {#settling}

### Fade In On Addition

마지막 예제를 기반으로 설정 단계에서 `htmx-added` 클래스를 사용하여 새 콘텐츠를 페이드 인할 수 있습니다.  
새 콘텐츠가 아닌 타깃에 대한 CSS Transition을 `htmx-settling` 클래스를 사용하여 작성할 수도 있습니다.

```html
<style>
#fade-me-in.htmx-added {
  opacity: 0;
}
#fade-me-in {
  opacity: 1;
  transition: opacity 1s ease-out;
}
</style>
<button id="fade-me-in"
        class="btn primary"
        hx-post="/fade_in_demo"
        hx-swap="outerHTML settle:1s">
        Fade Me In
</button>
```

#### Demo {#fade-settle-demo}

<style>
#fade-me-in.htmx-added {
  opacity: 0;
}
#fade-me-in {
  opacity: 1;
  transition: opacity 1s ease-out;
}
</style>

<button id="fade-me-in"
        class="btn primary"
        hx-post="/fade_me_in"
        hx-swap="outerHTML settle:1s">
        Fade Me In
</button>

<script>
    onPost("/fade_me_in", function () {return "<button id=\"fade-me-in\"\n"+
                                               "        class=\"btn primary\"\n"+
                                               "        hx-post=\"/fade_me_in\"\n"+
                                               "        hx-swap=\"outerHTML settle:1s\">\n"+
                                               "        Fade Me In\n"+
                                               "</button>"});
</script>

## Request In Flight Animation {#request}

요청을 트리거하는 요소에 적용되는 `htmx-request` 클래스를 활용할 수도 있습니다.  
아래는 제출 시 요청이 처리 중임을 나타내기 위해 모양이 변경되는 Form입니다:

```html
<style>
  form.htmx-request {
    opacity: .5;
    transition: opacity 300ms linear;
  }
</style>
<form hx-post="/name" hx-swap="outerHTML">
<label>Name:</label><input name="name"><br/>
<button class="btn primary">Submit</button>
</form>
```

#### Demo {#request-demo}

<style>
  form.htmx-request {
    opacity: .5;
    transition: opacity 300ms linear;
  }
</style>

<div aria-live="polite">
<form hx-post="/name" hx-swap="outerHTML">
<label>Name:</label><input name="name"><br/>
<button class="btn primary">Submit</button>
</form>
</div>

<script>
  onPost("/name", function(){ return "Submitted!"; });
</script>

## Using the htmx `class-tools` Extension

[`class-tools`](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/class-tools/README.md) 확장을 사용하면 흥미로운 애니메이션을 많이 만들 수 있습니다.

다음은 div의 불투명도를 토글하는 예제입니다. 토글 시간을 전환 시간보다 약간 길게 설정한 것을 주목하세요.  
이렇게 하면 클래스 변경으로 인해 전환이 중단될 때 발생할 수 있는 깜박임을 방지할 수 있습니다.

```html
<style>
.demo.faded {
  opacity:.3;
}
.demo {
  opacity:1;
  transition: opacity ease-in 900ms;
}
</style>
<div class="demo" classes="toggle faded:1s">Toggle Demo</div>
```

#### Demo {#class-tools-demo}

<style>
.demo.faded {
  opacity:.3;
}
.demo {
  opacity:1;
  transition: opacity ease-in 900ms;
}
</style>
<div class="demo" classes="toggle faded:1s">Toggle Demo</div>

### Using the View Transition API {#view-transitions}

htmx는 [`hx-swap`](/attributes/hx-swap) 속성의 `transition` 옵션을 통해 새로운 
[View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)에 접근할 수 있도록 지원합니다.

아래는 뷰 전환을 사용하는 교체(swap)의 예시입니다. 이 전환은 CSS에서 `view-transition-name` 속성을 통해 외부 div에 연결되며, 해당 전환은 
`::view-transition-old`와 `::view-transition-new`를 사용하여 정의되며, `@keyframes`를 통해 애니메이션을 정의합니다. 
(View Transition API에 대한 자세한 내용은 [Chrome Developer Page](https://developer.chrome.com/docs/web-platform/view-transitions/)에서 확인할 수 있습니다.)

이 전환의 이전 콘텐츠는 왼쪽으로 슬라이드 아웃되고, 새로운 콘텐츠는 오른쪽에서 슬라이드 인되어야 합니다.

작성 시점에서 이 시각적 전환은 Chrome 111+에서만 발생하지만, 가까운 미래에 더 많은 브라우저에서 이 기능을 구현할 것으로 예상됩니다.

```html
<style>
   @keyframes fade-in {
     from { opacity: 0; }
   }

   @keyframes fade-out {
     to { opacity: 0; }
   }

   @keyframes slide-from-right {
     from { transform: translateX(90px); }
   }

   @keyframes slide-to-left {
     to { transform: translateX(-90px); }
   }

   .slide-it {
     view-transition-name: slide-it;
   }

   ::view-transition-old(slide-it) {
     animation: 180ms cubic-bezier(0.4, 0, 1, 1) both fade-out,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-to-left;
   }
   ::view-transition-new(slide-it) {
     animation: 420ms cubic-bezier(0, 0, 0.2, 1) 90ms both fade-in,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-from-right;
   }
</style>


<div class="slide-it">
   <h1>Initial Content</h1>
   <button class="btn primary" hx-get="/new-content" hx-swap="innerHTML transition:true" hx-target="closest div">
     Swap It!
   </button>
</div>
```

#### Demo

<style>
   @keyframes fade-in {
     from { opacity: 0; }
   }

   @keyframes fade-out {
     to { opacity: 0; }
   }

   @keyframes slide-from-right {
     from { transform: translateX(90px); }
   }

   @keyframes slide-to-left {
     to { transform: translateX(-90px); }
   }

   .slide-it {
     view-transition-name: slide-it;
   }

   ::view-transition-old(slide-it) {
     animation: 180ms cubic-bezier(0.4, 0, 1, 1) both fade-out,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-to-left;
   }
   ::view-transition-new(slide-it) {
     animation: 420ms cubic-bezier(0, 0, 0.2, 1) 90ms both fade-in,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-from-right;
   }
</style>

<div class="slide-it">
   <h1>Initial Content</h1>
   <button class="btn primary" hx-get="/new-content" hx-swap="innerHTML transition:true" hx-target="closest div">
     Swap It!
   </button>
</div>

<script>
    var originalContent = htmx.find(".slide-it").innerHTML;

    this.server.respondWith("GET", "/new-content", function(xhr){
        xhr.respond(200,  {}, "<h1>New Content</h1> <button class='btn danger' hx-get='/original-content' hx-swap='innerHTML transition:true' hx-target='closest div'>Restore It! </button>")
    });

    this.server.respondWith("GET", "/original-content", function(xhr){
        xhr.respond(200,  {}, originalContent)
    });
</script>

## Conclusion

위의 기법을 사용하면 htmx를 사용하면서 일반 HTML로 흥미롭고 즐거운 효과를 꽤 많이 만들 수 있습니다.
