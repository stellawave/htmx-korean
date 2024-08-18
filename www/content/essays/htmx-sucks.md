+++
title = "htmx sucks"
date = 2024-02-01
updated = 2024-04-01
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

저는 [htmx](https://htmx.org)를 한동안 지켜보아 왔습니다. 처음에는 약간 웃기거나 민망한 밈처럼 느껴졌고, 실제 웹 개발 작업에서의 코믹한 휴식으로 여겨졌습니다. 
예를 들어 [React Server Components](https://react.dev/reference/react/use-server), [Svelte Runes](https://svelte.dev/blog/runes) 및 
[Signals](https://www.solidjs.com/tutorial/introduction_signals)과 같은 것들이 실제로 기술의 최첨단을 밀고 나가는 작업이라고 생각했습니다.

불행히도, [2023년 중반](https://star-history.com/#bigskysoftware/htmx&Date)에 이르러 [어떤 이유](https://www.youtube.com/watch?v=zjHHIqI9lUY)로 
사람들은 htmx를 진지하게 받아들이기 시작했습니다. 이는 웹 개발의 미래에 대해 깊은 우려를 불러일으키는 매우 심각한 사태입니다.

그리고 저만 이런 우려를 가진 것이 아닙니다. [여기에서 htmx에 대한 훌륭한 비판](https://archive.is/rQrl7)을 읽어보세요:

> 기본적으로 그들은 자신의 무지를 완전히 드러내고, 자신들이 해낸 것에 대해 근거 없는 장점을 부여하며, 다른 사람들이 그것에 대해 칭찬해 주기를 바란다.

정말 정확한 말입니다. 정말, 정말 정확합니다.

불행히도, 그 훌륭한 매체 게시물의 언어는 학문적이며, 이론적인 HTML에 대한 탄탄한 이해가 없다면 
그 글의 더 중요한 요점들은 일반 웹 개발자에게는 이해되지 않을 것입니다.

그래서 이 기사에서는 [htmx가 왜 별로인지를](@/essays/htmx-sucks.md) 평범한 웹 개발자를 위해 쉽게 설명해 보려고 합니다.

## 코드가 형편없다

우선, htmx의 코드를 살펴보세요.

[이 쓰레기를 보세요.](https://github.com/bigskysoftware/htmx/blob/master/src/htmx.js)

그들은 `var`를 여기저기서 사용하고, 거의 최신 JavaScript 기능을 사용하지 않으며 
(안녕하세요, htmx 사람들, [모듈](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)에 대해 들어보셨나요?), `window` 네임스페이스를 오염시키고 있습니다.

최악인 것은, 이 코드가 그저 하나의 거대한 JavaScript 덩어리라는 점입니다! 파일 하나로 구성되어 있으며 전혀 분해되지 않았습니다. 
만약 이 사람이 제가 [MSU에서 가르치는 수업](https://www.cs.montana.edu/directory/2256398/carson-gross)을 들었다면, 
저는 이 사람을 [관심사의 분리](https://en.wikipedia.org/wiki/Separation_of_concerns)에 대한 완전한 오해 때문에 낙제시켰을 것입니다. 
이는 컴퓨터 과학을 전공한 신입생이라면 누구나 이해할 수 있어야 하는 것입니다.

좋은 소프트웨어는 깔끔한 코드에서 시작되며, 이 코드는 매우 지저분합니다.

## 빌드 도구가 없다

다음으로 문제가 되는 것은 빌드 단계의 부재입니다. htmx는 전통적인 빌드 단계를 가지고 있지 않을 뿐만 아니라, 
[JavaScript 커뮤니티가 누리고 있는 이점](https://vitejs.dev/guide/why.html)을 스스로 박탈하고 있습니다. 
그들은 심지어 [이를 자랑스럽게 여기고](https://htmx.org/essays/no-build-step/) 있습니다!

그리고 더 나쁜 점이 있습니다.

자세히 살펴보면, 그들이 빌드 단계가 없다고 주장하면서도 
[사실은 가지고 있다](https://github.com/bigskysoftware/htmx/blob/bedee219329117fff8d58e33678d82f7c34b08b5/package.json#L30)는 것을 알 수 있습니다. 
그저 자신들이 작성한 임시적인 bash 스크립트일 뿐입니다.

웃기고도 부정직합니다. 부끄러운 일입니다.

## 타입스크립트가 없다

이런 프로젝트에 [TypeScript](https://www.typescriptlang.org/)를 사용하는 
[명백한 이점](https://blog.logrocket.com/understanding-typescripts-benefits-pitfalls)에도 불구하고, 저자들은 이를 고집스럽게 사용하지 않고 있습니다. 
이 중 일부는 빌드 단계에 대한 비합리적인 반대(그들은 사실 빌드 단계를 가지고 있지만!) 때문이지만, 또 다른 이유는 "디버깅 가능한 소스 코드를 제공"하겠다는 말도 안 되는 약속 때문입니다. 
물론, JavaScript 개발자라면 누구나 알듯이, TypeScript는 완벽하게 디버깅 가능한 [소스 맵](https://www.typescriptlang.org/tsconfig#sourceMap)을 지원합니다. 
이 사실에도 불구하고 저자들은 여전히 구식 JavaScript 버전을 고집하며 개발하고 있습니다.

이들이 잘못되었음을 묵시적으로 인정하며, 이제 [JSDoc](https://jsdoc.app/) 주석을 코드베이스에 추가하기 시작했습니다 
(여기서 "코드베이스"라는 말을 느슨하게 사용합니다). 이 모든 것은 처음부터 현대적이고 모듈화된 TypeScript로 프로젝트를 작성하는 대신, 잘못된 선택을 보완하기 위한 것입니다.

그나마 다행인 점은 적어도 이들은 [절반 정도 괜찮은 테스트 스위트](https://github.com/bigskysoftware/htmx/blob/master/test/index.html)를 가지고 있다는 것입니다. 
그 코드베이스의 상태를 고려하면 당연히 그래야 합니다!

## 구식 기술

이제 htmx 코드베이스 자체의 주요 (하지만 결코 모든 것은 아닙니다!) 문제를 다루었으니, 더 일반적인 htmx의 문제로 넘어가 보겠습니다.

가장 눈에 띄는 문제는 저자들이 또다시 자랑스러워하는 것 중 하나입니다: [하이퍼미디어](https://hypermedia.systems)를 사용한다는 것입니다. 
사실 이것은 단순히 "HTML을 사용한다"는 말의 우아한 표현일 뿐입니다. 왜 그들이 다른 혼란스러운 용어를 사용하는지 모르겠지만, 어쨌든 그렇습니다.

자, 만약 눈치채지 못하셨다면, HTML은 이제 30년이 넘었습니다. 아주 오래된 기술입니다. 
그리고 더 나아가, 우리는 이 사람들이 추천하는 접근 방식에 대한 많은 경험을 가지고 있습니다. 
htmx가 새로운 것을 하는 것은 아닙니다: [intercooler.js](https://intercoolerjs.org), 
[PJax](https://github.com/defunkt/jquery-pjax) 및 [Unpoly](https://unpoly.com/) (htmx보다 훨씬 낫습니다, 참고로)는 이미 오래전부터 존재해왔습니다.

그 이전에도 [`jquery.load()`](https://api.jquery.com/load/#load-url-data-complete)라는 것이 있었습니다.

다음은 2008년의 jQuery 코드입니다:

```js
$( "#result" ).load( "ajax/test.html" );
```

그리고 이제 htmx 사람들이 제공하는 혁신적인 코드를 보세요:

```html
<button hx-get="/ajax/test.html"
        hx-target="#result">
    Load
</button>
```

와우. 놀랍군요.

웃기지 않았다면 정말 화가 날 뻔했습니다.

## 컴포넌트가 없음

htmx를 사용하지 말아야 할 또 다른 이유는 사용할 수 있는 컴포넌트가 없다는 것입니다. 
React를 선택하면 [MUI](https://mui.com/), [NextUI](https://nextui.org/) 및 [Chakra](https://chakra-ui.com/)와 같은 것들을 사용할 수 있습니다.

htmx에서는... 아무것도 없습니다. 원하는 컴포넌트를 직접 찾아서 htmx와 이벤트 등을 통해 통합하는 방법을 알아내야 합니다.

정말로 [lit](https://lit.dev/)과 같은 것이 어떻게 작동하는지 알아내고, 그것을 htmx와 통합하는 방법까지 시간을 쓰고 싶습니까? 
전혀 말이 되지 않습니다. 통합된, 즉시 사용할 수 있는 컴포넌트를 제공하는 프론트엔드 라이브러리를 사용하는 것이 훨씬 낫습니다.

## 프론트엔드/백엔드 분할이 없음

htmx를 피해야 할 또 다른 주요 이유는 프론트엔드와 백엔드 팀 간의 분할을 제거한다는 점입니다. 
그들은 심지어 회사가 React에서 htmx로 전환했을 때 이를 [장점으로 자랑](https://htmx.org/essays/a-real-world-react-to-htmx-port/)하는 페이지까지 가지고 있습니다.

프론트엔드/백엔드 분할은 많은 회사에서 매우 성공적이었으며, 프론트엔드 엔지니어가 좋은 사용자 경험을 구축하는 데 집중하고, 
백엔드 엔지니어가 데이터 액세스 계층을 올바르게 만드는 데 집중할 수 있게 했습니다.

물론, 두 팀 간의 조율에 어려움이 있을 때도 있습니다. 
백엔드 엔지니어는 프론트엔드 엔지니어가 요구 사항을 너무 자주 변경한다고 불평하고, 프론트엔드 엔지니어는 백엔드 엔지니어가 너무 느리게 움직인다고 불평합니다. 
하지만 [GraphQL](https://graphql.org/)과 [RSC](https://react.dev/reference/react/use-server)와 같은 기술이 있어 이러한 문제를 해결할 수 있습니다. 
이 문제는 현재 기존의 [표준 웹 애플리케이션 모델](https://macwright.com/2020/10/28/if-not-spas.html) 내에서 해결된 문제입니다.

프론트엔드/백엔드 분할은 특히 개발 팀이 확장됨에 따라 회사에 매우 좋은 조직 모델로 입증되었으며, 이를 (이른바) "풀스택" 개발로 전환하는 것은 위험하고, 솔직히 말해 어리석은 일입니다.

## 백엔드 엔지니어는 쓰레기 같은 UI를 만듭니다

프론트엔드/백엔드 분할이 좋은지 아닌지에 대한 논쟁은 차치하고, 백엔드 엔지니어가 쓰레기 같은 사용자 인터페이스를 만든다는 것은 확실히 말할 수 있습니다.

[htmx 웹사이트](https://htmx.org)를 보세요. 인라인 스타일, 테이블, 오랫동안 `alt` 설명이 없는 이미지 태그가 잔뜩 있습니다. 
HTML을 사용하라고 권장하는 사람들로부터 나온 형편없는 HTML입니다!

사용자 인터페이스는 이를 올바르게 구축하는 방법을 이해하는 사람들에게 맡겨야 하며, 오늘날 그런 사람들은 대부분 프론트엔드 SPA 개발자들입니다.

## XSS 취약점

htmx를 사용하지 말아야 할 더 기술적인 이유로, 그것은 [크로스 사이트 스크립팅 공격](https://owasp.org/www-community/attacks/xss/)이라고 불리는 일련의 보안 문제에 노출될 수 있습니다.

여기서 문제는 htmx의 설계에 근본적으로 있습니다: 그것은 마크업에 *동작*을 넣을 수 있게 하고, 심지어 권장하기도 합니다. 
다시 한 번 우리는 [관심사의 분리](https://en.wikipedia.org/wiki/Separation_of_concerns)의 명백한 위반을 보게 됩니다. 
웹 개발에서 마크업, 스타일링, 동작을 각각 HTML, CSS 및 JavaScript 파일로 분리해야 한다는 것은 오래전부터 알고 있는 사실입니다.

이 명백하고 잘 알려진 진리를 위반함으로써, htmx는 HTML을 [적절하게 정화](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)하지 
않으면 다른 사람이 귀하의 웹 애플리케이션에 동작을 주입할 수 있는 취약성을 제공합니다.

가끔 htmx 저자는 "그렇다면 [href](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#href) 속성에 대해 어떻게 생각하십니까?"와 
같은 똑똑한 척하는 댓글을 달기도 하지만, 그건 분명히 다릅니다.

## 일자리 없음

htmx를 사용하지 말아야 할 또 다른 실용적인 이유는 htmx 관련 일자리가 거의 없다는 점입니다.

저는 방금 indeed에서 htmx 일자리를 검색해 보았고, 총 두 개의 일자리를 찾았습니다: 하나는 Microsoft, 다른 하나는 Oak Ridge National Laboratory에서입니다.

반면에 "react"를 검색하면 13,758개의 일자리가 나옵니다.

진지하게 말하자면, 어떤 기술에 경력을 걸고 싶습니까?

## 고용할 사람 없음

위의 문제의 다른 측면은 회사라면, htmx 개발자가 거의 없다는 점입니다.

모두가 부트캠프에 가서 React를 배웁니다. 만약 여러분의 회사가 큰 직원 풀을 필요로 하거나
(어쩌면 회사에 높은 이직률이 있을 수도 있고, 직원 임금을 낮추고 싶을 수도 있으며, 풀 스택 엔지니어의 작은 팀이 회사 확장을 방해할 수도 있습니다), 
프론트엔드 개발에서 가장 인기 있는 기술을 사용하는 것이 훨씬 더 합리적입니다.

오늘날, 그 기술은 React입니다.

## API 중복 (또는 그 이상!)

더 기술적인 측면으로 돌아가서, htmx를 채택하고 _또한_ 모바일 앱이나 제3자가 사용할 API를 만들고 싶다면, 
해당 API를 웹 애플리케이션의 엔드포인트와 완전히 별도로 만들어야 할 것입니다.

여기서 다시 한번, 믿을 수 없게도, [htmx 사람들은 이 사실을 자랑합니다](https://htmx.org/essays/splitting-your-apis/), 
여기에는 중복 작업이 수반된다는 점을 완전히 무시하고 있습니다.

단일 API를 유지 관리하는 것이 훨씬 더 합리적이며, 단일 백엔드 팀이 모든 요구 사항을 유연하게 처리할 수 있도록 하는 것이 합리적입니다.

이는 명백하며, 솔직히 말해 논쟁의 여지조차 없는 것입니다.

## 확장되지 않음

htmx의 또 다른 기술적 문제는 그것이 확장되지 않는다는 것입니다. 작은 애플리케이션에서는 작동할 수 있지만, 애플리케이션이 커지면 htmx 모델은 무너지고 엉망이 됩니다. 
React와 같은 프론트엔드 도구의 컴포넌트 모델은 모든 것을 구획화하고 테스트 가능하게 유지합니다. 이것은 큰 코드베이스를 깔끔하게 유지하는 데 훨씬 더 쉽게 만듭니다.

예를 들어, [Github](https://github.com/)은 htmx와 매우 유사한 기술을 사용하여 시작했습니다. 그러나 최근 React를 도입하기 시작했으며, 이전보다 훨씬 더 안정적입니다. 
처음부터 React로 시작하고 모든 것을 현대적인 컴포넌트 기반 방식으로 구축했다면 훨씬 더 나았을 것입니다. 하지만 늦게라도 올바른 선택을 하고 있는 것은 다행입니다. 
늦더라도 안 하는 것보다 낫습니다.

## 창작자가 미쳐있음

마지막으로, 아마도 htmx를 사용하지 말아야 할 가장 중요한 이유는 창작자가 분명히 정신이 이상하다는 점입니다.

그의 [트위터 피드](https://twitter.com/htmx_org)를 보세요: 비전문적이고, 유치하며, 고의적으로 도발적입니다.

또는 자신이 [동의하지 않는 에세이](https://htmx.org/essays/is-htmx-another-javascript-framework/)를 자신의 사이트에 
[게시](https://htmx.org/essays/htmx-sucks/)하는 사실을 고려해 보세요.

에세이 탭에는 [밈 섹션](https://htmx.org/essays/#memes)이 있는데, 대부분이 민망하고 프론트엔드 라이브러리의 웹사이트에 있어야 할 것이 전혀 아닙니다.

그는 [누구든 htmx의 CEO가 될 수 있게](https://htmx.ceo) 하여 LinkedIn에서 매우 민망한 직책 발표를 할 수 있게 합니다.

무분별한 광대짓입니다.

프론트엔드 라이브러리를 선택할 때, 어느 정도는 그 라이브러리의 저자를 동료로 선택하는 것입니다. 정말로 이 사람과 함께 일하고 싶습니까?

저는 절대 그렇게 하고 싶지 않습니다.

## 결론

이 글을 통해 htmx 및 하이퍼미디어를 웹 애플리케이션에 선택하는 것이 
[극도로 나쁜 아이디어](https://www.reddit.com/r/ProgrammerHumor/comments/zmyug8/edsger_dijkstra_math_and_memes/)이며, 
이는 [몬태나](https://bigsky.software)에서만 기원할 수 있었던 생각이라는 것을 확신시켜드리고 싶습니다. 
"이제 끝났다", "다시 시작한다"는 식의 헛소리, CEO 프로필과 유치한 밈을 가진 팬보이와 팬걸의 말을 듣지 마세요.

소프트웨어, 특히 프론트엔드 소프트웨어는 법률 및 정치와 같은 다른 매우 심각하고 생산적인 활동만큼이나 진지한 비즈니스로 취급되어야 합니다. 
우리는 혁신과 정교함에 중점을 둔 라이브러리를 지지해야 하며, 창작자가 대부분의 시간을 트위터에 말도 안 되는 것을 올리는 깨진, 과거를 돌아보는 라이브러리를 지지해서는 안 됩니다.

이것은 상식입니다: htmx는 별로입니다.
