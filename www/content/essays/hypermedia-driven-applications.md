+++
title = "Hypermedia-Driven Applications"
date = 2022-02-06
updated = 2022-10-18
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

## 기원

> 논제: MPA - 다중 페이지 애플리케이션
>
> 반논제: SPA - 단일 페이지 애플리케이션
>
> 종합: HDA - 하이퍼미디어 구동 애플리케이션
>
> \-\-[@htmx_org](https://twitter.com/htmx_org/status/1490318550170357760)

## 하이퍼미디어 구동 애플리케이션 아키텍처

**하이퍼미디어 구동 애플리케이션(Hypermedia Driven Application, HDA)** 아키텍처는 전통적인 다중 페이지 애플리케이션(MPA)의 단순성과 유연성을
단일 페이지 애플리케이션(SPA)의 향상된 사용자 경험과 결합한 새로운/오래된 접근 방식입니다.

HDA 아키텍처는 웹의 기존 HTML 인프라를 확장하여 하이퍼미디어 개발자가 더 강력한 하이퍼미디어 구동 상호작용을 생성할 수 있도록 함으로써 이러한 목표를 달성합니다.

REST 아키텍처의 [제약](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) 개념을 따르며, 다음과 같은 두 가지 제약이 HDA 아키텍처를 특징짓습니다:

* HDA는 더 나은 프론트엔드 상호작용을 달성하기 위해 명령형 스크립팅이 아닌 *선언적, HTML에 내장된 문법*을 사용합니다.

* HDA는 비하이퍼미디어 형식(e.g. JSON) 대신 **하이퍼미디어(HTML)**로 서버와 상호작용합니다.

이 두 가지 제약을 채택함으로써, HDA 아키텍처는 SPA 아키텍처가 그렇지 않은 방식으로 웹의 원래 
[REST-ful](https://developer.mozilla.org/en-US/docs/Glossary/REST) 아키텍처 내에 머무릅니다.

특히, HDA는 [하이퍼미디어가 애플리케이션 상태의 엔진으로 작동하는 것(HATEOAS)](@/essays/hateoas.md)을 계속 사용하지만, 
대부분의 SPA는 클라이언트 측 모델과 데이터(API)를 사용하고 HATEOAS를 포기합니다.

## HDA 프래그먼트 예시

htmx의 [실시간 검색](@/examples/active-search.md) 예시를 고려해보세요:

```html
<h3> 
  Search Contacts 
  <span class="htmx-indicator"> 
    <img src="/img/bars.svg"/> Searching... 
   </span> 
</h3>
<input class="form-control" type="search" 
       name="search" placeholder="Begin Typing To Search Users..." 
       hx-post="/search" 
       hx-trigger="keyup changed delay:500ms, search" 
       hx-target="#search-results" 
       hx-indicator=".htmx-indicator">

<table class="table">
    <thead>
    <tr>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Email</th>
    </tr>
    </thead>
    <tbody id="search-results">
    </tbody>
</table>
```

이 UX 패턴은 일반적으로 SPA와 연관되지만, 이 경우는 HTML 내에서 완전히 달성되며, HTML과 조화를 이룹니다.

이 예시는 HDA의 본질적인 특징을 효과적으로 보여줍니다:

* 기능의 프론트엔드는 선언적 htmx 속성으로 완전히 지정되며, HTML 내에서 직접 작성됩니다.
* 서버와의 상호작용은 HTTP와 HTML을 통해 이루어집니다: HTTP `POST` 요청이 서버로 전송되고, 서버는 HTML을 반환하며, htmx는 이 HTML을 DOM에 삽입합니다.

## HDA에서의 스크립팅

[Code-On-Demand](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1_7)는 웹의 원래 REST-ful 아키텍처의 선택적 제약 조건입니다.

마찬가지로, HDA 아키텍처는 마지막 선택적 제약 조건을 가집니다:

* 스크립팅(Code-On-Demand)은 가능한 한 *주요 하이퍼미디어 내에서* 직접 이루어져야 합니다.

이것은 Roy Fielding이 자신의 논문에서 언급한 Code-On-Demand에 대한 우려를 다룹니다:

> 그러나, (Code-On-Demand는) 가시성을 줄이므로, REST 내에서 선택적 제약일 뿐입니다.

스크립팅을 HTML 내에 직접 내장함으로써 가시성이 향상되며, [행동의 지역성](@/essays/locality-of-behaviour.md) 소프트웨어 설계 원칙을 충족시킵니다.

이 세 가지 접근 방식은 이 세 번째 제약을 만족시키는 스크립팅 방법들입니다: 
[hyperscript](https://hyperscript.org), [AlpineJS](https://alpinejs.dev) 및 [VanillaJS](http://vanilla-js.com/) (HTML 요소에 직접 내장된 경우).

다음은 각 접근 방식의 예시입니다:

```html
<!-- hyperscript -->
<button _="on click toggle .red-border">
  Toggle Class
</button>

<!-- Alpine JS -->
<button @click="open = !open" :class="{'red-border' : open, '' : !open}">
  Toggle Class
</button>

<!-- VanillaJS -->
<button onclick="this.classList.toggle('red-border')">
  Toggle Class
</button>
```

HDA에서는 하이퍼미디어(HTML)가 애플리케이션을 구축하는 데 있어 주요 매체이므로:

* 모든 서버와의 통신은 여전히 HTTP 요청과 하이퍼미디어(HTML) 응답을 통해 관리됩니다.
* 스크립팅은 주로 애플리케이션의 *프론트엔드 경험*을 향상시키기 위해 사용됩니다.

스크립팅은 기존의 하이퍼미디어(HTML)를 보강하지만, 이를 *대체*하거나 HDA의 기본 REST-ful 아키텍처를 훼손하지 않습니다.

## HDA 스타일 라이브러리

다음 라이브러리는 개발자가 HDA를 만들 수 있도록 도와줍니다:

* <https://htmx.org>
* <https://unpoly.com/>
* <https://piranha.github.io/twinspark-js/>
* <https://hotwire.dev>
* <https://hyperview.org/> (모바일 하이퍼미디어!)

다음 스크립팅 라이브러리는 적절히 사용될 때 HDA 접근 방식을 보완합니다:

* <https://hyperscript.org>
* <https://alpinejs.dev/>
* <http://vanilla-js.com/> (HTML에 직접 내장된 경우)

## 결론

HDA 아키텍처는 두 가지 이전 아키텍처, 즉 원래의 다중 페이지 애플리케이션(MPA) 아키텍처와 (상대적으로) 새로운 단일 페이지 애플리케이션 아키텍처의 종합입니다.

이 아키텍처는 두 아키텍처의 장점을 결합하려고 시도합니다: REST-ful 아키텍처를 사용하는 MPA의 단순성과 신뢰성, 그리고 많은 경우 SPA와 동등한 사용자 경험을 제공합니다.
