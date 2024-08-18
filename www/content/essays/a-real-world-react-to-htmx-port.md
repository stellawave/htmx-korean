+++
title = "A Real World React -> htmx Port"
date = 2022-09-29
updated = 2022-10-15
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

이론적으로 [REST와 HATEOAS](@/essays/hateoas.md)에 대해 이야기하거나 [하이퍼미디어 기반 애플리케이션](@/essays/hypermedia-driven-applications.md) 아키텍처를 설명하는 것도 좋지만, 
결국 소프트웨어에서 중요한 것은 실용성입니다: 실제로 작동하는가? 그리고 그것이 개선을 가져오는가?

htmx가 _작동한다_는 것은 우리가 자체 소프트웨어에서 사용하고 있기 때문에 확실하게 말할 수 있습니다. 
하지만 htmx가 다른 접근 방식보다 _개선_된 것인지 말하기는 어렵습니다. 
왜냐하면 htmx가 [react](https://reactjs.org/) 같은 다른 방법과 비교해서 어떻게 작동할지에 대한 비교가 이루어진 적이 없기 때문입니다.

지금까지는요.

[David Guillot](https://github.com/David-Guillot)가 [Contexte](https://www.contexte.com/)에서
[DjangoCon 2022](https://pretalx.evolutio.pt/djangocon-europe-2022/talk/MZWJEA/)에서
["The Mother of All htmx Demos"](https://en.wikipedia.org/wiki/The_Mother_of_All_Demos)라 불리는 시연을 제공했습니다:

> **실제 SaaS 제품에서 React에서 htmx로: 우리가 해냈고, 정말 멋졌습니다!**
>
> 우리는 큰 결단을 내리고, SaaS 제품의 2년간의 React UI 작업을 간단한 Django 템플릿과 htmx로 몇 달 만에 교체했습니다.
> 이 경험을 구체적인 지표와 함께 공유하고, 당신의 CTO를 설득해 드리겠습니다!

## 비디오

전체 프레젠테이션을 여기에서 볼 수 있습니다 (꼭 보셔야 합니다!):

<iframe style="max-width: 100%" width="618" height="352" src="https://www.youtube.com/embed/3GObi93tjZI" title="DjangoCon 2022 | From React to htmx on a real-world SaaS product: we did it, and it's awesome!" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 요약

* 이 작업에는 약 **2개월**이 소요되었습니다 (대부분 JavaScript로 이루어진 21,000 LOC 코드 기반)
* 애플리케이션의 사용자 경험(UX)에 **감소 없음**
* **코드 베이스 크기**를 **67%** 줄였습니다 (21,500 LOC에서 7,200 LOC로)
* **Python 코드**가 **140%** 증가했습니다 (500 LOC에서 1,200 LOC로), Python을 JS보다 선호한다면 좋은 일입니다.
* 전체 **JS 종속성**을 **96%** 줄였습니다 (255에서 9로)
* **웹 빌드 시간**을 **88%** 단축했습니다 (40초에서 5초로)
* **최초 로드의 상호작용 가능 시간**이 **50-60%** 줄었습니다 (2~6초에서 1~2초로)
* React는 데이터를 처리할 수 없었기 때문에 htmx를 사용할 때 **훨씬 더 큰 데이터 세트**를 처리할 수 있었습니다.
* 웹 애플리케이션의 **메모리 사용량**이 **46%** 줄었습니다 (75MB에서 45MB로)

## 분석

이 숫자들은 눈에 띄는 성과이며, Contexte 애플리케이션이 하이퍼미디어에 매우 적합하다는 사실을 반영합니다. 
이는 텍스트와 이미지를 많이 표시하는 콘텐츠 중심의 애플리케이션입니다. 모든 웹 애플리케이션이 이러한 숫자를 보일 것이라고 기대하지는 않습니다.

그러나 _많은_ 애플리케이션이 시스템의 일부에서 하이퍼미디어/htmx 접근 방식을 채택함으로써 극적인 개선을 기대할 수 있습니다.

### 개발 팀 구성

포트 작업이 팀 구조에 미친 영향을 간과하기 쉽습니다. Contexte가 React를 사용하고 있었을 때, 백엔드와 프론트엔드 간에 명확한 분리가 있었고, 
두 명의 개발자는 전적으로 백엔드 작업을, 한 명의 개발자는 전적으로 프론트엔드 작업을, 그리고 한 명의 개발자는 "풀스택" 작업을 수행했습니다.

(여기서 "풀스택"은 프론트엔드와 백엔드 작업 모두를 편하게 수행할 수 있으며, 따라서 전체 "스택"에서 기능을 완전히 독립적으로 개발할 수 있는 개발자를 의미합니다.)

htmx로 포트한 후에는 *팀 전체*가 "풀스택" 개발자가 되었습니다. 
이는 각 팀원이 더 효과적이 되고 더 많은 가치를 제공할 수 있게 하며, 개발이 더 즐거워지고, 개발자가 전체 기능을 소유할 수 있게 만듭니다. 
마지막으로, 소프트웨어 최적화를 더 잘 수행할 수 있으며, 개발자가 스택의 어느 부분이든 최적화를 수행할 수 있기 때문에 다른 개발자와의 조율이 필요하지 않습니다.

## 슬라이드

프레젠테이션의 슬라이드는 여기에서 찾을 수 있습니다 (훌륭한 발표자의 노트를 꼭 확인하세요!).

<https://docs.google.com/presentation/d/1jW7vTiHFzA71m2EoCywjNXch-RPQJuAkTiLpleYFQjI/edit?usp=sharing>
