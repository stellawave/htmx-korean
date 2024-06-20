[![</> htmx](https://raw.githubusercontent.com/bigskysoftware/htmx/master/www/static/img/htmx_logo.1.png "HTML을 위한 고성능 도구")](https://htmx.org)

*HTML을 위한 고성능 도구*

[![Discord](https://img.shields.io/discord/725789699527933952)](https://htmx.org/discord)
[![Netlify](https://img.shields.io/netlify/dba3fc85-d9c9-476a-a35a-e52a632cef78)](https://app.netlify.com/sites/htmx/deploys)
[![Bundlephobia](https://badgen.net/bundlephobia/dependency-count/htmx.org)](https://bundlephobia.com/result?p=htmx.org)
[![Bundlephobia](https://badgen.net/bundlephobia/minzip/htmx.org)](https://bundlephobia.com/result?p=htmx.org)

## 소개

htmx를 사용하면 [속성들(attributes)](https://htmx.org/reference#attributes)을 사용하여 HTML에서 직접
[AJAX](https://htmx.org/docs#ajax), [CSS Transitions](https://htmx.org/docs#css_transitions), [WebSockets](https://htmx.org/docs#websockets) 및 [Server Sent Events](https://htmx.org/docs#sse)를
사용할 수 있으므로 하이퍼텍스트의 [강력함](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)과 [단순성](https://en.wikipedia.org/wiki/HATEOAS)으로
[최신 사용자 인터페이스](https://htmx.org/examples)를 구축할 수 있습니다.

htmx는 크기가 작고 ([~14k min.gz'd](https://unpkg.com/htmx.org/dist/)),
[종속성이 없으며](https://github.com/bigskysoftware/htmx/blob/master/package.json),
[확장가능하며](https://htmx.org/extensions) &
IE11에서도 호환됩니다.

## 시작을 위한 동기부여

* 왜 `<a>`와 `<form>`만 HTTP 요청을 할 수 있나요?
* 왜 오직 `click`과 `submit` 이벤트만 트리거할 수 있나요?
* 왜 GET과 POST만 사용할 수 있나요?
* 왜 우리는 *전체* 화면만 교체할 수 있나요?

htmx는 이런 제멋대로인 제약 조건을 제거함으로써 HTML을
[hypertext](https://en.wikipedia.org/wiki/Hypertext)로 완성합니다.

## 빠른 시작

```html
  <script src="https://unpkg.com/htmx.org@2.0.0"></script>
  <!-- AJAX를 통해 클릭으로 POST를 요청하는 버튼입니다 -->
  <button hx-post="/clicked" hx-swap="outerHTML">
    Click Me
  </button>
```

[`hx-post`](https://htmx.org/attributes/hx-post)와 [`hx-swap`](https://htmx.org/attributes/hx-swap) 속성들은 htmx에게 알려줍니다:

> "사용자가 이 버튼을 클릭하면 /clicked으로 AJAX 요청을 보내고 전체 버튼을 응답으로 바꿉니다."

htmx는 [intercooler.js](http://intercoolerjs.org)의 후계 프로젝트입니다.

### node 패키지로 설치하기

npm을 이용하여 설치하기:

```
npm install htmx.org --save
```

주의. `htmx`라는 오래된 깨진 패키지가 있습니다. 여기는 `htmx.org`입니다.

## 웹사이트 & 문서

* <https://htmx.org>
* <https://htmx.org/docs>

## 기여하기
기여하기를 원하시나요? [기여 가이드라인](CONTRIBUTING.md)을 확인해보세요.

시간이 없다면 [후원자가 되어주실 수도](https://github.com/sponsors/bigskysoftware#sponsors) 있습니다.

## 한국어 번역
[번역자 블로그](https://jason-in-cosmos.blogspot.com/)


이 한국어 번역은 htmx 개발과 별개로 이루어지고 있는 독자적인 문서 번역입니다.
htmx 같은 완전한 고유명사의 경우 영어로 표기하였으며, 한국어로 변환할 수 있는 경우 가능하면 변환하였습니다.
번역 등에 대해 의견을 제안해주시려면 이슈나 풀 리퀘스트를 올려주세요.

### 개발 가이드

로컬에서 htmx를 개발하려면 개발 종속성을 설치해야 합니다.

실행:

```
npm install
```

그런 다음 root에서 웹 서버를 실행합니다.

가장 쉬운 방법은 다음과 같습니다:

```
npx serve
```

그런 다음 아래 주소로 이동하여 테스트 세트를 실행할 수 있습니다:

<http://0.0.0.0:3000/test/>

이 시점에서 `/src/htmx.js`를 수정하여 기능을 추가한 다음 `/test` 아래의 해당 영역에 테스트를 추가할 수 있습니다.

* `/test/index.html` - 다른 모든 테스트가 포함된 루트 테스트 페이지입니다.
* `/test/attributes` - 속성별 테스트
* `/test/core` - 핵심 기능 테스트
* `/test/core/regressions.js` - 회귀 테스트
* `/test/ext` - 확장 테스트
* `/test/manual` - 자동화할 수 없는 수동 테스트

htmx는 [mocha](https://mochajs.org/) 테스트 프레임워크, [chai](https://www.chaijs.com/) assertion 프레임워크
그리고 [sinon](https://sinonjs.org/releases/v9/fake-xhr-and-server/)을 사용하여 AJAX 요청을 모의 테스트합니다. 이 프레임워크들은 모두 괜찮습니다.

또한 `npm run ws-tests`를 사용하여 서버측 이벤트 확장과 웹소켓 라이브 테스트와 데모를 실행할 수 있습니다.

## 하이쿠

*피곤한 자스:<br/>
하이퍼텍스트는<br/>
준비 완료다*
