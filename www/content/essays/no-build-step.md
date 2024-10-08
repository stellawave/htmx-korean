+++
title = "Why htmx Does Not Have a Build Step"
date = 2023-08-19
[taxonomies]
author = ["Alexander Petros"]
tag = ["posts"]
+++

htmx의 몇몇 기여자들로부터 반복적으로 제기되는 질문 중 하나는 htmx가 왜 TypeScript로 작성되지 않았는지, 아니면 왜 htmx에 빌드 단계가 전혀 없는지에 관한 것입니다. 
htmx의 전체 소스 코드는 단일 3,500줄의 JavaScript 파일로 구성되어 있습니다. htmx에 기여하려면 `htmx.js` 파일을 수정하면 되며, 
이는 미니파이(minification) 및 압축을 거친 후 프로덕션 환경에서 브라우저에 전송되는 파일과 동일합니다.

저는 htmx 프로젝트를 대표하지는 않지만, 몇 가지 중요한 기여를 했으며, 이 문제가 제기될 때마다 빌드 단계가 없는 설정을 유지하는 것을 적극적으로 옹호해왔습니다. 
제 관점에서, htmx에 빌드 단계가 없는 이유는 다음과 같습니다.

## 한 번 작성하면 영원히 실행된다
라이브러리를 순수 JavaScript로 작성하는 가장 큰 이유는 그것이 영원히 지속될 수 있기 때문입니다. 
이는 아마도 JavaScript의 가장 과소평가된 기능일 것입니다. 특정 예외가 있을 수 있지만, 1999년에 작성된 JavaScript는 현대의 코드와 함께 최신 Google Chrome에서도 수정 없이 실행됩니다. 
이는 매우 적은 프로그래밍 환경에서 가능한 일입니다. 
Python, Java, C와 같은 언어들은 새로운 언어 기능을 선택하면 호환되지 않는 API로부터 벗어나게 되는 버전 관리 메커니즘을 가지고 있기에 이러한 특성을 가지고 있지 않습니다.

물론, 대부분의 사람들이 경험한 JavaScript는 시간이 지남에 따라 문제가 되는 경우가 많습니다. 
3개월 후에 node 저장소를 다시 열어보면, 프로젝트가 보안 경고, 호환되지 않는 라이브러리 "업그레이드", 
그리고 프로젝트를 시작할 당시에는 최고로 여겨졌던 프론트엔드 프레임워크가 이제는 기술 부채로 여겨지고 있는 상황에 빠져 있는 것을 발견하게 될 것입니다. 
이 상황에 대한 책임이 누구에게 있는지는 논의하지 않겠지만, 어쨌든 JavaScript 런타임 외의 의존성을 없애면 이러한 문제 전체를 제거할 수 있습니다.

오늘날 JavaScript를 작성하는 인기 있는 방법 중 하나는 TypeScript에서 컴파일하는 것입니다(이 글에서는 TypeScript를 예로 많이 사용하겠습니다. 
TypeScript가 아마 빌드 시스템을 사용하는 [가장 좋은 이유](https://en.wikipedia.org/wiki/Straw_man#Steelmanning)일 것입니다). 
TypeScript는 웹 브라우저에서 네이티브로 실행되지 않기 때문에, TypeScript 코드는 [ECMA](https://developer.mozilla.org/en-US/docs/Glossary/ECMA)의 엄격한 하위 호환성에 대한 집착으로부터 보호되지 않습니다. 
다른 모든 의존성과 마찬가지로, 새로운 주요 TypeScript 버전이 이전 버전과 하위 호환성을 보장하지 않을 수도 있습니다. 
만약 그렇지 않다면, 최신 개발 도구 체인을 사용하려면 유지 보수를 해야 할 것입니다.

유지 보수는 노동으로 지불하는 비용이며, 오픈 소스 코드베이스는 가장 유지 보수를 감당하기 어려운 프로젝트들입니다. 
빌드 단계를 사용하지 않음으로써 htmx를 최신 상태로 유지하는 데 필요한 노동을 크게 줄일 수 있습니다. 
이 경험은 htmx의 전신인 [intercooler.js](https://intercoolerjs.org)에서도 확인되었습니다. intercooler.js는 (제가 이해하기로는) 매우 적은 노력으로 무기한 유지 관리되고 있습니다. 
htmx 1.0이 출시되었을 때 TypeScript는 4.1 버전이었고, intercooler.js가 출시되었을 때 TypeScript는 1.0 버전 이전이었습니다. 
그 당시 TypeScript 버전으로 작성된 코드가 오늘날의 TypeScript 컴파일러(이 글을 작성할 당시 5.1 버전)에서 수정 없이 컴파일될 수 있을까요? 그럴 수도 있고, 아닐 수도 있습니다.

그러나 htmx는 JavaScript로 작성되었고, 의존성이 없기 때문에 웹 브라우저가 유효한 한 수정 없이 실행될 것입니다. 브라우저 공급업체들이 어려운 작업을 대신하게 하십시오.

## 개발자 경험
TypeScript의 개발자 경험(DX)이 많은 면에서 JavaScript보다 우수하다는 것은 사실입니다. 
그러나 TypeScript DX가 *모든* 면에서 우수하다는 것은 사실이 아니며, 소프트웨어 엔지니어들이 진보를 기능의 완전성으로 보는 경향이 가끔씩 그들이 좋아하는 DX 측면에서 대가로 지불한 비용을 간과하게 만듭니다. 
예를 들어, TypeScript를 사용하는 작은 대가는 컴파일하는 데 시간이 걸리고, 변경 사항을 테스트하려면 재컴파일을 기다려야 한다는 것입니다. 
보통 이 비용은 무시할 수 있을 정도로 적고 충분히 지불할 가치가 있지만, 어쨌든 비용입니다.

TypeScript를 사용할 때 더 중요한 비용은 브라우저에서 실행되는 코드가 여러분이 작성한 코드가 아니라는 점입니다. 이는 브라우저의 개발자 도구를 사용하기 어렵게 만듭니다. 
TypeScript 코드에서 예외가 발생하면, 해당 예외의 스택 트레이스(자바스크립트 줄 번호, 자바스크립트 함수 시그니처 등)가 여러분이 작성한 TypeScript 코드와 어떻게 일치하는지 파악해야 합니다. 
반면, JavaScript 코드에서 예외가 발생하면, 바로 소스 코드로 이동하여 작성한 내용을 읽고 디버거에서 중단점을 설정할 수 있습니다. 
이는 *엄청난* DX입니다. 이러한 방식으로 작업해 본 적이 없는 젊은 웹 개발자들에게는 계시와 같은 경험이 될 수 있습니다.

빌드 단계 옹호자들은 TypeScript가 [소스 맵](https://firefox-source-docs.mozilla.org/devtools-user/debugger/how_to/use_a_source_map/index.html)을 생성할 수 있다고 지적합니다. 
이는 브라우저에 TypeScript와 JavaScript가 어떻게 연결되어 있는지 알려줍니다. 맞습니다! 
그러나 이제는 TypeScript로 작성한 코드, 생성된 JavaScript 코드, 이 둘을 연결하는 소스 맵이라는 *또 다른* 것을 관리해야 합니다. 
이제는 핫 리로딩 개발 서버에 의존하게 되었으며, 이 서버가 로컬호스트에서 이들을 최신 상태로 유지해 줄 것입니다. 그러나 스테이징 서버에서는 어떻게 될까요? 
프로덕션 환경에서는요? 이러한 환경에서 발생하는 버그를 추적하기가 더 어려워질 것입니다. 이는 해결할 수 있는 문제이지만, 여러분이 만든 문제이며, 이는 대가입니다.

htmx의 DX는 매우 간단합니다. 브라우저는 단일 파일을 로드하며, 이 파일은 모든 환경에서 여러분이 작성한 것과 동일한 파일입니다. 
이러한 경험을 유지하기 위해 필요한 대가도 존재하지만, 이 프로젝트에 적합한 선택입니다.

## 명확성 강제
모듈화는 소프트웨어의 [훌륭한 아이디어](https://legacy.python.org/dev/peps/pep-0020/) 중 하나입니다. 
모듈은 코드를 더 작은 문제를 해결하는 잘 정의된 하위 구조로 나눔으로써 매우 복잡한 문제를 해결할 수 있게 해줍니다. 모듈은 매우 유용합니다.

그러나 때때로 여러분이 단순한 문제를 해결하고자 할 때, 또는 상대적으로 단순한 문제를 해결하고자 할 때, 더 복잡한 소프트웨어의 구성 요소를 사용하지 않는 것이 도움이 될 수 있습니다. 
그렇지 않으면 이들의 복잡성을 흉내 내면서 동등한 가치를 창출하지 못할 수 있습니다. 기본적으로 htmx는 상대적으로 단순한 문제를 해결합니다. 
즉, 선언적인 하이퍼텍스트 특성을 사용하여 DOM 요소를 더 쉽게 대체할 수 있도록 HTML에 몇 가지 속성을 추가하는 것입니다. 
htmx를 단일 파일로 유지하는 것(현재 약 3,500줄)은 라이브러리에 의도성을 부여하며, 새로운 코드를 추가할 때 실질적인 압박감을 줍니다. 
이는 상대적으로 단순함을 유지하는 균형을 유지하는 데 도움이 됩니다.

DX 비용은 명확하지만, 놀라운 DX 이점도 있습니다. 소스 파일에서 함수 이름을 검색하면 해당 함수의 모든 호출을 즉시 찾을 수 있습니다(이것은 더 고급 코드 탐색의 필요성을 줄입니다). 
기능이 숨겨질 수 있는 곳이 없기 때문에 htmx 작업이 훨씬 더 접근하기 쉬워집니다. 훨씬 더 복잡한 프로젝트에서도 이 접근 방식을 부분적으로 사용합니다. 
예를 들어, SQLite3는 [단일 파일 소스 합병](https://www.sqlite.org/amalgamation.html)에서 컴파일되며(물론 개발을 위해 별도의 파일을 사용하지만, 그들도 *미쳤*지는 않았습니다), 
이를 통해 [해킹하기가 훨씬 쉬워집니다](https://jvns.ca/blog/2019/10/28/sqlite-is-really-easy-to-compile/). 리눅스 커널을 이러한 방식으로 구축할 수는 없겠지만, htmx는 리눅스 커널이 아닙니다.

# 비용
모든 기술 선택에는 장점과 단점이 있습니다. 빌드 단계를 생략하는 선택 역시 마찬가지입니다. 
이러한 선택의 장단점을 인식하여 정보에 입각한 결정을 내릴 수 있어야 하며, 어떤 장점이나 단점이 더 이상 적용되지 않을 때 그 결정을 재검토할 수 있어야 합니다. 
순수 JavaScript로 코드를 작성하는 장점을 염두에 두고, 이로 인해 발생하는 문제점들에 대해 살펴보겠습니다.

## 정적 타입이 없다
TypeScript는 JavaScript의 엄격한 상위 집합이며, TypeScript가 제공하는 몇 가지 기능은 매우 유용합니다. 
TypeScript는... 타입을 제공하며, 이는 [IDE가 코드를 제안하는 능력](https://grugbrain.dev/#grug-on-type-systems)을 향상시키고, 메서드를 잘못 사용했을 가능성이 있는 부분을 지적해 줍니다. 
TypeScript에서는 코드의 이름 변경 및 리팩터링 도구가 JavaScript보다 훨씬 더 신뢰할 수 있습니다. 하지만 htmx 코드는 JavaScript로 작성되어야 합니다. 
이는 브라우저가 JavaScript를 실행하기 때문입니다. 그리고 [JavaScript가 동적 타입을 유지하는 한](https://github.com/tc39/proposal-type-annotations), 
htmx 소스 코드에서 진정한 정적 타입을 얻기 위한 트레이드오프는 가치가 없습니다(하지만 htmx *사용자*는 여전히 `.d.ts` 파일로 선언된 타입 API를 활용할 수 있습니다).

미래의 htmx 버전에서는 JSDoc을 사용하여 빌드 단계 없이도 일부 보장을 제공할 수 있을 것입니다. 
다른 라이브러리들, 예를 들어 [Svelte](https://github.com/sveltejs/svelte/pull/8569), 역시 [디버깅 마찰](https://news.ycombinator.com/item?id=35892250)로 인해 이 방향으로 나아가고 있습니다.

## ES6 사용 불가
htmx는 Internet Explorer 11을 지원하고 있으며, 빌드 단계가 없기 때문에 htmx의 모든 코드는 IE11과 호환되는 JavaScript로 작성되어야 합니다. 
이는 [ES6](https://262.ecma-international.org/6.0/)를 사용할 수 없음을 의미합니다. 
사람들이 JavaScript가 꽤 좋아졌다고 말할 때, 그들은 일반적으로 ES6에서 도입된 `async/await`, 익명 함수, 함수형 배열 메서드(`.map`, `.forEach`)와 같은 언어 기능을 말하는데, 이러한 기능들은 htmx 소스 코드에서 사용할 수 없습니다.

이것은 매우 짜증나는 일이지만, 실제로는 큰 장애물이 되지는 않습니다. 몇 가지 멋진 언어 기능이 부족하다고 해서 함수형 패러다임으로 코드를 작성할 수 없게 되는 것은 아닙니다. 
[forEach 메서드](https://github.com/bigskysoftware/htmx/blob/b4a61c543b283eb2315a47708006783efb78f563/src/htmx.js#L375-L381)를 커스텀으로 작성할 필요가 없으면 좋겠죠? 
물론입니다. 하지만 htmx가 목표로 하는 모든 브라우저가 ES6를 지원할 때까지 ES5에 몇 가지 헬퍼 함수를 추가하는 것은 어렵지 않습니다. 
ES6에 익숙하다면 더 나은 ES5 코드를 자동으로 작성할 수 있을 것입니다.

htmx 2.0에서는 IE11 지원이 중단되며, 그때부터는 소스 코드에 ES6 사용이 허용될 것입니다.

## 핵심에서 모듈 사용 불가
이 점은 분명하지만, 다시 한 번 언급할 가치가 있습니다. htmx 소스는 모듈로 분리할 수 있다면 훨씬 더 깔끔할 것입니다. 
[청결함](https://www.steveonstuff.com/2022/01/27/no-such-thing-as-clean-code) 외에도 코드 품질에 영향을 미치는 다른 요소들이 있지만, htmx 소스가 높은 품질을 유지하는 이유는 깔끔함 때문이 아닙니다.

이로 인해 htmx로 특정 작업을 수행하는 것이 매우 어려워집니다. 
[idiomorph 알고리즘](https://github.com/bigskysoftware/idiomorph)이 htmx 2.0 핵심에 포함될 수 있지만, 사람들이 htmx 없이 DOM 변환 알고리즘을 사용할 수 있도록 별도의 패키지로 유지되고 있습니다. 
핵심에 여러 파일을 포함할 수 있다면, git 서브모듈과 같은 미러링 스킴으로 쉽게 이를 해결할 수 있었을 것입니다. 
하지만 핵심이 단일 파일로 구성되어 있으므로 idiomorph 코드도 그 안에 포함되어야 할 것입니다.

# 최종 생각
이 에세이는 "왜 htmx가 <em>지금 당장</em> 빌드 단계가 없는가"라는 제목이 더 적절할 것입니다. 앞서 언급했듯이, 상황은 변하며 이러한 트레이드오프는 언제든지 재검토될 수 있습니다! 
현재 우리가 탐구 중인 문제 중 하나는 릴리스와 관련이 있습니다. 
htmx가 릴리스를 할 때, 몇 가지 셸 명령어를 사용하여 `dist` 디렉토리에 미니파이되고 압축된 `htmx.js` 버전을 채웁니다(엄밀히 말하자면, 이것은 어떤 의미에서는 빌드 단계라고 할 수 있습니다). 
앞으로는 이 스크립트를 확장하여 [유니버설 모듈 정의(UMD)](https://github.com/umdjs/umd)를 자동으로 생성할 수 있습니다. 또는 새로운 배포 요구 사항이 생겨 더 복잡한 설정이 필요할 수도 있습니다. 
누가 알겠습니까!

htmx의 핵심 가치는 지난 10년 동안 점점 더 복잡해진 JavaScript 스택이 지배해 온 웹 개발 생태계에서 *선택*을 제공하는 것입니다. 
프론트엔드 JavaScript 코드베이스가 거대하지 않으면 백엔드에 JavaScript를 채택해야 한다는 압박이 훨씬 적어집니다. Python, Go, 심지어 NodeJS로 백엔드를 작성해도 상관없습니다. 
htmx에는 중요하지 않기 때문입니다. 주요 언어마다 HTML을 포맷팅하기 위한 성숙한 솔루션이 있기 마련입니다. 
이는 [HOWL(Hypermedia On Whatever you'd Like)](https://htmx.org/essays/hypermedia-on-whatever-youd-like/) 원칙입니다.

빌드 프로세스 없이 JavaScript를 작성하는 것은 더 이상 NextJS나 SvelteKit이 필요하지 않은 상태에서 SPA 프레임워크의 복잡성을 관리하기 위한 선택 중 하나입니다. 
이 선택은 오늘날 htmx 개발에 적합하며, 여러분의 앱에도 적합할 수 있습니다.
