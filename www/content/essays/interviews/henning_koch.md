+++
title = "An interview with Henning Koch, Creator of Unpoly"
date = 2022-06-13
updated = 2023-06-13
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

Henning Koch, [Unpoly](https://unpoly.com)의 창시자 중 한 명과 인터뷰하게 되어 매우 기쁩니다. Unpoly는 intercooler.js와 병행하여 개발된 하이퍼미디어 지향 자바스크립트 라이브러리입니다. 인터뷰에 응해주셔서 감사합니다!

**Q**: 먼저, 독자들에게 당신의 직업적, 기술적 배경에 대해 간단히 소개해 주실 수 있을까요?

> 물론입니다! 저는 현재 [makandra](https://makandra.de/en)라는 루비 온 레일즈(Ruby on Rails) 컨설팅 회사의 개발 책임자를 맡고 있으며, 2009년에 공동 설립했습니다. 그 전에는 웹 개발자로 프리랜서로 활동했습니다. 제 일의 맥락은 여러 웹 애플리케이션을 동시에 작업하고, 오랜 기간 동안 유지보수하는 것입니다. 특정 주간에 우리는 교육, 자동차, 사이버 보안 등 다양한 산업 분야의 10개 이상의 프로젝트를 다루고 있습니다. Unpoly는 고객 프로젝트에서 반복적으로 나타나는 패턴에서 추출된 결과물입니다.

**Q**: intercooler.js를 만들게 된 주요 이유 중 하나는 당시 인기 있던 SPA 라이브러리(예: Angular, ExtJS)를 다루기 싫었기 때문이었습니다. Unpoly에도 비슷한 역사가 있나요?

> 우리 팀은 사실 한동안 AngularJS에 전적으로 의존했습니다. 이는 이전에 가지고 있던 방대한 jQuery 스파게티 코드를 대체하기 위한 노력이었습니다. 그러나 구글이 Angular 2 리라이트로 AngularJS를 중단했을 때, 우리는 그 시기를 돌아보며 혼합된 결과를 얻었습니다. SPA 모델에 적합한 몇 가지 앱을 구축했지만, 대부분의 프로젝트는 더 큰 코드베이스, 더 많은 종속성, 클라이언트와 서버 간의 로직 분할, 데이터를 이미 있는 곳(서버)에서 필요한 곳(브라우저)으로 이동하기 위한 많은 API 보일러플레이트로 고통받았습니다.
>
> 그래서 우리는 점진적 향상을 다시 시도했지만, 이번에는 수동으로 AJAX 요청을 수행하고 개별 DOM 요소를 조작하는 것에서 벗어나 고급 구조를 제공했습니다. 기본적으로 "HTML6이 서버 렌더링된 앱에 관한 것이라면 어떤 기능이 포함될까?"라는 가상의 HTML6 사양을 만들어 보는 것이었습니다. 이 사고 실험이 Unpoly로 이어졌습니다.

**Q**: Unpoly는 매우 "배터리 포함" 라이브러리로, 점진적 향상에 대한 훌륭한 지원을 제공합니다. 당신도 Rails 개발자인 걸로 알고 있는데, 이것이 Unpoly 접근 방식에 영향을 미쳤나요?

> 확실히 그렇습니다! Rails처럼 Unpoly도 모든 것에 대해 강력한 기본값을 제공하며, 명시적인 설정보다 비침습적 관례를 선호합니다. 예를 들어, Unpoly가 모든 링크와 폼을 처리하도록 설정하려면 전역적으로 설정할 수 있으며, HTML을 전혀 변경하지 않아도 됩니다.
>
> 최근의 Rails 모토 중에는 "현대 웹 애플리케이션의 복잡성을 압축하자"와 "한 사람을 위한 프레임워크"라는 것이 있습니다. makandra에서 젊은 개발자들을 훈련하는 것이 제 또 다른 책임이기 때문에, 이 모토는 저에게 매우 공감됩니다. 저는 한 사람이 풀스택 개발자가 되어 일관되게 좋은 결과를 낼 수 있는 스택을 유지하는 것에 대해 정말 신경 씁니다.
>
> 또한, 루비 개발자로서 저는 코드의 호출 방식에 대한 인간공학과 미학에 대해 과도한 집착을 가지고 있습니다. 기능이 클라이언트 코드에서 어떻게 사용되는지에 대해 많은 고민을 합니다. 작은 아이디어가 불균형하게 많은 코드가 필요한 경우, 이것은 저에게 잠을 설치게 만드는 문제입니다.

**Q**: Unpoly를 만들 때 하이퍼미디어, REST 등에 대해 많이 생각했나요? 그런 것들이 유용하다고 생각하나요? 흥미로운가요?

> 저도 인터랙티브한 문서가 UI와 콘텐츠를 함께 스트리밍하는 것을 좋아합니다. 저에게는 1990년대에 문자 기반 BBS UI와 WinHelp 파일에서 시작되었고, 결국 웹이 모든 것을 대체하게 되었습니다.
>
> 오늘날 저는 그것에 대해 매우 철학적이지는 않지만, 하이퍼미디어 접근 방식이 적은 코드로 좋은 UI 충실도를 제공하는 달콤한 지점이라고 믿습니다. 대부분의 애플리케이션에서는 하이퍼미디어가 SPA 모델보다 더 나은 결과를 제공합니다. SPA 모델의 이론적인 한계와 대부분의 SPA가 제공하는 것 사이에는 엄청난 괴리가 있다고 생각합니다. SPA는 낙관적인 UI를 허용하지만(이는 훌륭합니다!), 이는 JSON 엔드포인트를 기다리는 것보다 더 많은 코드를 필요로 합니다. 그래서 연결이 불안정한 환경에서 의미 있는 상호작용을 시도하면, 많은 SPA는 스피너와 빈 페이지로 전락하게 됩니다.

**Q**: Unpoly에서 얻은 가장 중요한 기술적 교훈은 무엇인가요?

> 자바스크립트를 추가하기 전에 브라우저가 많은 엣지 케이스를 정확하게 처리한다는 것을 배웠습니다. 포커스 관리, 동시 입력, 불안정한 연결 등입니다. 자바스크립트에서 페이지 전환을 에뮬레이트할 때, 동일한 수준의 정확성을 제공하는 것은 간단하지 않습니다. 이를 해결하는 데 많은 코드가 필요합니다. 작은 마이크로 라이브러리가 React를 2000바이트로 재구현했다고 주장할 때마다 이 점을 항상 기억합니다. 번들 크기의 절반을 줄이려면 대가로 일부 정확성을 포기해야 합니다.