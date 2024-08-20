+++
template = "demo.html"
title = "View Transitions"
date = 2023-04-11
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

우리는 이미 웹 애플리케이션에서 SPA(단일 페이지 애플리케이션) 아키텍처가 널리 채택된 주요 이유 중 하나가 미학적인 고려사항 때문이라는 점을 강조해왔습니다.

우리의 책 [Hypermedia Systems](https://hypermedia.systems)에서 언급했듯이, Web 1.0 스타일의 연락처 관리 애플리케이션을 다룰 때, 
SPA 버전과 기능상 동일하더라도 해당 애플리케이션에는 심각한 **미학적** 문제가 존재합니다.

> 사용자 경험 관점에서: 애플리케이션의 페이지를 이동하거나, 연락처를 생성, 업데이트 또는 삭제할 때 눈에 띄는 새로고침이 발생합니다. 이는 모든 사용자 상호작용(링크 클릭 또는 폼 제출)이 전체 페이지 새로고침을 요구하며, 각 작업 후에는 새로운 HTML 문서를 처리해야 하기 때문입니다.
>
> *–Hypermedia Systems - [Chapter 5](https://hypermedia.systems/book/extending-html-as-hypermedia/)*

이러한 페이지 간의 눈에 띄는 "카-척(ka-chunk)" 효과와 종종 발생하는 [미 스타일링된 콘텐츠의 깜빡임](https://webkit.org/blog/66/the-fouc-problem/)은 오랫동안 문제로 지적되어 왔습니다. 
현대 브라우저가 이 상황을 다소 개선했지만(물론, 요청이 진행 중임을 덜 명확하게 만드는 단점도 있습니다), 특히 잘 제작된 SPA와 비교했을 때 여전히 문제가 됩니다.

초창기 웹에서는 이러한 문제가 크게 문제가 되지 않았습니다. 
당시 우리는 브라우저의 도구 모음에서 공룡 주위로 별이 날아다니는 것을 보거나, 불타는 텍스트, 테이블 기반 레이아웃, 춤추는 아기 등을 보면서 FTP 클라이언트와 비교하고 있었습니다.

기준은 **낮았고**, 그 시절은 **좋았습니다**.

하지만 웹은 이제 이러한 유치한 것들을 벗어나야 했고, 이제 우리는 사용자에게 매끄럽고 매력적인 인터페이스를 제공할 것으로 기대됩니다. 
이는 하나의 보기 상태에서 다른 상태로의 부드러운 전환을 포함합니다.

다시 말해, 많은 팀들이 SPA 접근 방식을 기본으로 사용하는 이유는 "옛 방식"이 그냥... **투박해 보이기 때문**입니다.

## CSS 전환

초기 웹 엔지니어들은 웹 개발자들이 서로 다른 보기 상태 간의 부드러운 전환을 제공하고 싶어할 것이라는 점을 깨달았고, 이를 위해 다양한 기술을 제공했습니다. 
그중 주요 기술은 [CSS 전환](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)으로, 상태에서 다른 상태로 수학적 **전환**을 지정할 수 있습니다.

안타깝게도 HTML에서 CSS 전환은 JavaScript를 사용해야만 가능했습니다. 즉, 전환을 트리거하려면 요소를 동적으로 변경해야 하며, "순수" HTML로는 이를 수행할 수 없습니다. 
실제로 이는 JavaScript를 사용하여 SPA를 구축하는 개발자들만이 이러한 도구를 사용할 수 있었음을 의미하며, 이는 SPA의 **미학적 우월성**을 더욱 공고히 했습니다.

htmx를 통해, 아마도 아시겠지만, CSS 전환을 [순수 HTML에서 사용할 수 있도록](https://htmx.org/examples/animations/) 만들 수 있으며, 
이는 다소 복잡한 [교체 모델](https://htmx.org/docs/#request-operations)을 통해 구현됩니다. 여기서 우리는 새 콘텐츠와 오래된 콘텐츠에 모두 있는 요소들을 가져와 "정착" 속성을 설정합니다. 이 방법을 사용하면 하이퍼미디어 기반 애플리케이션도 잘 제작된 SPA처럼 매끄럽게 느껴지게 할 수 있습니다.

하지만 새로운 API가 등장했습니다: [View Transition API](https://developer.chrome.com/docs/web-platform/view-transitions/).

## View Transition API

View Transition API는 CSS 전환보다 훨씬 더 야심 찬 목표를 가지고 있습니다. 
이 API는 단순하면서도 직관적인 API를 제공하여, **전체 DOM**을 하나의 상태에서 다른 상태로 전환하는 방법을 제공합니다. 이를 통해 일반 사용자들도 쉽게 사용할 수 있습니다.

더욱이 이 API는 JavaScript뿐만 아니라 HTML에서도 사용할 수 있어, Web 1.0 접근 방식을 사용해도 훨씬 더 멋진 사용자 인터페이스를 구축할 수 있게 됩니다.

이 기능이 가능해지면 "Hypermedia Systems"에서 다루는 연락처 애플리케이션을 다시 살펴보는 것도 재미있을 것입니다!

하지만 이 글을 쓰는 시점에서, 이 API는 CSS 전환처럼 JavaScript에서만 사용할 수 있으며, Chrome 111+에서만 막 출시되었습니다.

JavaScript에서 이 API는 매우 간단합니다:

```js
  // 이것만으로도 상태 간의 매끄러운 전환이 가능합니다!
  document.startViewTransition(() => updateTheDOMSomehow(data));
```

이제, 이건 내가 좋아하는 API입니다.

운 좋게도, 이 API를 일반 htmx 교체 모델 주위에 쉽게 감쌀 수 있어서, HTML에서 일반적으로 사용할 수 있기 전에 htmx에서 View Transition을 탐색할 수 있습니다!

그리고 [htmx 1.9.0](https://unpkg.com/htmx.org@1.9.0)부터는 [`hx-swap`](/attributes/hx-swap) 속성에 `transition:true` 속성을 추가하여 이 API를 실험해볼 수 있습니다.

## 실용적인 예제

이제 이 새로운 멋진 장난감을 htmx와 함께 사용한 간단한 예제를 살펴보겠습니다.

이 작업에는 두 가지 단계가 필요합니다:

* CSS를 통해 View Transition 애니메이션 정의하기
* htmx로 구동되는 버튼에 작은 주석 추가하기

### CSS

먼저 해야 할 일은 우리가 원하는 View Transition 애니메이션을 정의하는 것입니다.

* 콘텐츠를 슬라이드하고 페이드하는 애니메이션을 정의하기 위해 `@keyframes`를 사용합니다.
* `:view-transition-old()` 및 `:view-transition-new()` 의사 선택자를 사용하여 `slide-it`이라는 이름의 View Transition을 정의합니다.
* `.sample-transition` 클래스를 방금 정의한 `slide-it` View Transition에 연결하여 CSS 클래스 이름을 통해 요소에 바인딩할 수 있도록 합니다.

(View Transition API에 대한 자세한 내용은 이를 문서화한 [Chrome Developer Page](https://developer.chrome.com/docs/web-platform/view-transitions/)에서 확인할 수 있습니다.)

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

   /* 이전 및 새로운 콘텐츠에 대한 애니메이션 정의 */
   ::view-transition-old(slide-it) {
     animation: 180ms cubic-bezier(0.4, 0, 1, 1) both fade-out,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-to-left;
   }
   ::view-transition-new(slide-it) {
     animation: 420ms cubic-bezier(0, 0, 0.2, 1) 90ms both fade-in,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-from-right;
   }

   /* 지정된 CSS 클래스에 View Transition 연결 */
   .sample-transition {
       view-transition-name: slide-it;
   }
</style>
```

이 CSS는 `.sample-transition` 클래스를 가진 콘텐츠가 제거될 때 페이드아웃하고 왼쪽으로 슬라이드하며, 새 콘텐츠는 페이드인하고 오른쪽에서 슬라이드해 들어오도록 설정합니다.

### HTML

CSS를 통해 View Transition을 정의했으므로, 다음으로 할 일은 htmx가 변경할 실제 요소에 이 View Transition을 연결하고, htmx가 View Transition API를 활용하도록 지정하는 것입니다.

```html
<div class="sample-transition">
   <h1>초기 콘텐츠</h1>
   <button hx-get="/new-content" 
           hx-swap="innerHTML transition:true" 
           hx-target="closest div">
     Swap It!
   </button>
</div>
```

여기에는 새 콘텐츠를 가져오기 위해 `GET` 요청을 보내고, 응답을 사용하여 가장 가까운 div의 내부 HTML을 교체하는 버튼이 있습니다.

해당 div에는 `sample-transition` 클래스가 있으므로, 위에서 정의한 View Transition이 적용됩니다.

마지막으로, `hx-swap` 속성에 `transition:true` 옵션이 포함되어 있으며, 이는 htmx가 전환 시 내부 View Transition JavaScript API를 사용하도록 지시합니다.

## 데모

이제 모든 것을 연결했으므로, htmx와 함께 View Transition API를 사용해 볼 준비가 되었습니다. 
다음은 Chrome 111+에서 작동하는 데모입니다(다른 브라우저에서도 작동하지만 멋진 애니메이션은 표시되지 않습니다):


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

   /* 이전 및 새로운 콘텐츠에 대한 애니메이션 정의 */
   ::view-transition-old(slide-it) {
     animation: 180ms cubic-bezier(0.4, 0, 1, 1) both fade-out,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-to-left;
   }
   ::view-transition-new(slide-it) {
     animation: 420ms cubic-bezier(0, 0, 0.2, 1) 90ms both fade-in,
     600ms cubic-bezier(0.4, 0, 0.2, 1) both slide-from-right;
   }

   /* 지정된 CSS 클래스에 View Transition 연결 */
   .sample-transition {
       view-transition-name: slide-it;
   }
</style>


<div class="sample-transition" style="padding: 24px">
   <h1>초기 콘텐츠</h1>
   <button hx-get="/new-content" hx-swap="innerHTML transition:true" hx-target="closest div">
     Swap It!
   </button>
</div>

<script>
    var originalContent = htmx.find(".sample-transition").innerHTML;

    this.server.respondWith("GET", "/new-content", function(xhr){
        xhr.respond(200,  {}, "<h1>새 콘텐츠</h1> <button hx-get='/original-content' hx-swap='innerHTML transition:true' hx-target='closest div'>복원하기</button>")
    });

    this.server.respondWith("GET", "/original-content", function(xhr){
        xhr.respond(200,  {}, originalContent)
    });
</script>


Chrome 111+에서 이 페이지를 보고 있다고 가정하면, 위의 콘텐츠가 왼쪽으로 부드럽게 슬라이드아웃되고 오른쪽에서 새 콘텐츠가 슬라이드인하는 것을 볼 수 있습니다. 멋지네요!

## 결론

이제, 꽤 멋지지 않나요? 그리고 이 개념을 이해하고 나면 그렇게 많은 작업이 필요하지 않습니다! 이 새로운 API는 많은 가능성을 보여줍니다.

View Transitions는 매우 흥미로운 새로운 기술로, 현재 널리 사용되는 SPA 아키텍처와 
[하이퍼미디어 기반 애플리케이션](https://htmx.org/essays/hypermedia-driven-applications/) 간의 격차를 크게 줄일 수 있다고 생각합니다.

Web 1.0 애플리케이션의 보기 흉한 "카-척"을 없애고, SPA 접근 방식의 미학적 이점을 줄임으로써, 
다양한 아키텍처와 관련된 실제 [기술적 절충점](https://htmx.org/essays/when-to-use-hypermedia/)에 더 집중할 수 있게 될 것입니다.

View Transitions가 순수 HTML에서 사용 가능해지는 것을 고대하고 있지만, 그때까지는 htmx에서 이를 오늘부터 사용해 볼 수 있습니다!

