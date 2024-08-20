+++
title = "Lazy Loading"
template = "demo.html"
+++

이 예는 페이지에서 요소를 느리게 로드하는 방법을 보여줍니다.  
다음과 같은 초기 상태로 시작합니다:

```html
<div hx-get="/graph" hx-trigger="load">
  <img  alt="Result loading..." class="htmx-indicator" width="150" src="/img/bars.svg"/>
</div>
```

그래프를 로드하는 동안 진행률 표시기가 표시됩니다.  
그런 다음 안정된 CSS 전환을 통해 시야에서 부드럽게 사라지며 그래프가 로드되고 표시됩니다:

```css
.htmx-settling img {
  opacity: 0;
}
img {
 transition: opacity 300ms ease-in;
}
```

<style>
.htmx-settling img {
  opacity: 0;
}
img {
 transition: opacity 300ms ease-in;
}
</style>

{{ demoenv() }}

<script>
    server.autoRespondAfter = 2000; // longer response for more drama

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params){
      return lazyTemplate();
    });

    onGet("/graph", function(request, params){
      return "<img alt='Tokyo Climate' src='/img/tokyo.png'>";
    });

    // templates
    function lazyTemplate(page) {
      return `<div hx-get="/graph" hx-trigger="load">
  <img  alt="Result loading..." class="htmx-indicator" width="150" src="/img/bars.svg"/>
</div>`;
    }
</script>
