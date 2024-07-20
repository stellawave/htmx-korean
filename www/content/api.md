+++
title = "Javascript API"
+++

라이브러리의 초점은 아니지만 htmx는 주로 [extension development](https://extensions.htmx.org)이나 [events](@/events.md) 작업을 위한 헬퍼 메서드의 작은 API를 제공합니다.

[hyperscript](https://hyperscript.org) 프로젝트는 htmx 기반 애플리케이션에 보다 광범위한 
스크립팅 지원을 제공하기 위한 것입니다.

### Method - `htmx.addClass()` {#addClass}

이 메서드는 지정된 요소에 클래스를 추가합니다.

##### Parameters

* `elt` - 클래스가 추가될 요소
* `class` - 추가할 클래스

or

* `elt` - 클래스가 추가될 요소
* `class` - 추가할 클래스
* `delay` - 클래스가 추가되기 전 지연시간(밀리초)

##### Example

```js
  // ID가 'demo'인 요소에 'myClass' 클래스를 추가합니다.
  htmx.addClass(htmx.find('#demo'), 'myClass');

  // 1초 후에 ID가 'demo'인 요소에 'myClass' 클래스를 추가합니다.
  htmx.addClass(htmx.find('#demo'), 'myClass', 1000);
```

### Method - `htmx.ajax()` {#ajax}

htmx 스타일의 AJAX 요청을 보냅니다. 이 메서드는 Promise를 반환하므로 콘텐츠가 DOM에 삽입된 후 callback을 실행할 수 있습니다.

##### Parameters

* `verb` - 'GET', 'POST', etc.
* `path` - AJAX 요청을 보내기 위한 URL 경로
* `element` - target 요소 (defaults to the `body`)

or

* `verb` - 'GET', 'POST', etc.
* `path` - AJAX 요청을 보내기 위한 URL 경로
* `selector` - target 선택자

or

* `verb` - 'GET', 'POST', etc.
* `path` - AJAX 요청을 보내기 위한 URL 경로
* `context` - 다음 중 하나를 포함하는 컨텍스트 객체
    * `source` - 요청의 source 요소
    * `event` - 요청을 "트리거"한 이벤트
    * `handler` - 응답 HTML을 처리할 callback
    * `target` - 응답으로 교체될 대상
    * `swap` - target을 기준으로 응답이 어떻게 교체될지 결정합니다.
    * `values` - 요청과 함께 제출할 값들
    * `headers` - 요청과 함께 제출할 헤더들
    * `select` - 응답에서 교체하려는 콘텐츠를 선택할 수 있습니다.


##### Example

```js
    // /example로 GET을 보내고 응답 HTML을 #myDiv에 입력합니다.
    htmx.ajax('GET', '/example', '#myDiv')

    // /example에 GET을 보내고 #myDiv를 응답으로 바꿉니다.
    htmx.ajax('GET', '/example', {target:'#myDiv', swap:'outerHTML'})

    // 콘텐츠가 DOM에 삽입된 후 일부 코드를 실행합니다.
    htmx.ajax('GET', '/example', '#myDiv').then(() => {
      // 이 코드는 'htmx:afterOnLoad' 이벤트 이후와 
      // 'htmx:xhr:loadend' 이벤트 전에 실행됩니다.
      console.log('Content inserted successfully!');
    });

```

### Method - `htmx.closest()` {#closest}

본인 요소를 포함하여 주어진 요소의 부모에서 가장 일치하는 요소를 찾습니다.

##### Parameters

* `elt` - 기준이 되는 요소
* `selector` - 찾을 선택자

##### Example

```js
  // ID가 'demo'인 요소를 가장 가까운 위치에서 둘러싸는 div를 찾습니다.
  htmx.closest(htmx.find('#demo'), 'div');
```

### Property - `htmx.config` {#config}

런타임에 htmx가 사용하는 구성을 저장하는 속성입니다.

[meta tag](@/docs.md#config)를 사용하는 것이 이러한 속성을 설정하는 데 선호되는 메커니즘이라는 점에 유의하세요.

##### Properties

* `attributesToSettle:["class", "style", "width", "height"]` - array of strings: 정리 단계에서 정리할 속성
* `refreshOnHistoryMiss:false` - boolean: `true`로 설정하면 htmx는 히스토리 누락 시 AJAX 요청을 사용하는 대신 전체 페이지 새로 고침을 실행합니다.
* `defaultSettleDelay:20` - int: 콘텐츠 교체 완료와 속성 정리 단계 사이의 기본 지연 시간입니다.
* `defaultSwapDelay:0` - int: 서버로부터 응답을 받은 후 교체를 수행할 때까지의 기본 지연 시간입니다.
* `defaultSwapStyle:'innerHTML'` - string: [`hx-swap`](@/attributes/hx-swap.md)이 생략된 경우 사용할 기본 교체 스타일입니다.
* `historyCacheSize:10` - int: 히스토리 지원을 위해 `localStorage`에 보관할 페이지 수입니다.
* `historyEnabled:true` - boolean: 히스토리 사용 여부
* `includeIndicatorStyles:true` - boolean: true이면 htmx는 페이지에 소량의 CSS를 삽입하여 `htmx-indicator` 클래스가 없으면 표시기를 보이지 않게 만듭니다.
* `indicatorClass:'htmx-indicator'` - string: 요청이 보내지는 중일 때 indicator에 표시할 클래스
* `requestClass:'htmx-request'` - string: 요청이 보내지는 중일 때 트리거 요소에 배치할 클래스입니다.
* `addedClass:'htmx-added'` - string: htmx가 DOM에 추가한 요소에 임시로 배치할 클래스입니다.
* `settlingClass:'htmx-settling'` - string: HTMX가 정리 단계에 있을 때 대상 요소에 배치할 클래스입니다.
* `swappingClass:'htmx-swapping'` - string: HTMX가 교체 단계에 있을 때 대상 요소에 배치할 클래스입니다.
* `allowEval:true` - boolean: `hx-vars`, 트리거 조건 및 script 태그 평가를 활성화하기 위해 htmx에서 eval과 유사한 기능을 사용할 수 있게 합니다. CSP 호환성을 위해 `false`로 설정할 수 있습니다.
* `allowScriptTags:true` - boolean: 새로운 콘텐츠에서 script 태그가 평가되도록 허용합니다.
* `inlineScriptNonce:''` - string: 인라인 스크립트에 추가할 [nonce](https://developer.mozilla.org/docs/Web/HTML/Global_attributes/nonce)
* `inlineStyleNonce:''` - string: 인라인 스타일에 추가할 [nonce](https://developer.mozilla.org/docs/Web/HTML/Global_attributes/nonce)
* `withCredentials:false` - boolean: 쿠키, 인증 헤더 또는 TLS 클라이언트 인증서와 같은 자격 증명을 사용하여 교차 사이트 Access-Control을 허용합니다.
* `timeout:0` - int: 요청이 자동으로 종료되기 전까지 걸릴 수 있는 시간(밀리초)입니다.
* `wsReconnectDelay:'full-jitter'` - string/function: `Abnormal Closure`, `Service Restart` 또는 `Try Again Later` 이벤트 코드에 의해 예기치 않은 연결 손실 후 재연결을 위한 `getWebSocketReconnectDelay`의 기본 구현입니다.
* `wsBinaryType:'blob'` - string: WebSocket 연결을 통해 수신되는 [바이너리 데이터 유형](https://developer.mozilla.org/docs/Web/API/WebSocket/binaryType)
* `disableSelector:"[hx-disable], [data-hx-disable]"` - array of strings: htmx는 이 속성이 있는 요소(부모 요소가 가지는 것도 포함)를 처리하지 않습니다.
* `scrollBehavior:'smooth'` - string: 페이지 전환 시 부스트된 링크의 동작. 허용되는 값은 `auto`와 `smooth`입니다. `smooth`는 페이지 맨 위로 부드럽게 스크롤하며, `auto`는 기본 링크처럼 동작합니다.
* `defaultFocusScroll:false` - boolean: focus된 요소가 view로 스크롤되어야 하는지 여부를 나타내며, [focus-scroll](@/attributes/hx-swap.md#focus-scroll) 교체 수정자를 사용하여 재정의할 수 있습니다.
* `getCacheBusterParam:false` - boolean: true로 설정하면 htmx는 `GET` 요청에 `org.htmx.cache-buster=targetElementId` 형식으로 대상 요소를 추가합니다.
* `globalViewTransitions:false` - boolean: `true`로 설정하면, htmx는 새로운 콘텐츠를 교체할 때 [View Transition](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) API를 사용합니다.
* `methodsThatUseUrlParams:["get"]` - array of strings: htmx는 이러한 메서드를 사용할 때 매개변수를 요청 본문이 아닌 URL에 인코딩하여 요청을 포맷합니다.
* `selfRequestsOnly:false` - boolean: `true`로 설정하면 현재 문서와 동일한 도메인으로만 AJAX 요청을 허용합니다.
* `ignoreTitle:false` - boolean: `true`로 설정하면 htmx는 새로운 콘텐츠에서 `title` 태그를 발견했을 때 문서 제목을 업데이트하지 않습니다.
* `scrollIntoViewOnBoost:true` - boolean: 부스트된 요소의 대상이 뷰포트로 스크롤되는지 여부를 나타냅니다. 부스트된 요소에 `hx-target`이 생략된 경우, 대상은 기본적으로 `body`로 설정되어 페이지가 맨 위로 스크롤됩니다.
* `triggerSpecsCache:null` - object: 평가된 트리거 사양을 저장하여 더 많은 메모리를 사용하는 대신 구문 분석 성능을 향상시키는 캐시입니다. 항상 비우지 않는 캐시를 사용하기 위해 간단한 객체를 정의하거나 [proxy object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Proxy)를 사용하여 자체 시스템을 구현할 수 있습니다. |

##### Example

```js
  // 히스토리 캐시 크기를 30으로 업데이트합니다.
  htmx.config.historyCacheSize = 30;
```

### Property - `htmx.createEventSource` {#createEventSource}

A새 [Server Sent Event](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/sse/README.md) 소스를 만드는 데 사용되는 속성입니다. 
사용자 지정 SSE 설정을 제공하도록 업데이트할 수 있습니다.

##### Value

* `func(url)` - URL 문자열을 가져와 새 `EventSource`를 반환하는 함수

##### Example

```js
  // 자격 증명을 사용하지 않도록 SSE 이벤트 소스를 재정의
  htmx.createEventSource = function(url) {
    return new EventSource(url, {withCredentials:false});
  };
```

### Property - `htmx.createWebSocket` {#createWebSocket}

새 [WebSocket](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/ws/README.md)을 만드는 데 사용되는 property입니다. 
사용자 지정 웹소켓 설정을 제공하도록 업데이트할 수 있습니다.

##### Value

* `func(url)` - URL 문자열을 가져와 새 `WebSocket`을 반환하는 함수

##### Example

```js
  // 특정 프로토콜을 사용하도록 WebSocket을 재정의합니다.
  htmx.createWebSocket = function(url) {
    return new WebSocket(url, ['wss']);
  };
```

### Method - `htmx.defineExtension()` {#defineExtension}

새 htmx [확장](https://extensions.htmx.org)을 정의합니다.

##### Parameters

* `name` - 확장 이름
* `ext` - 확장 정의

##### Example

```js
  // defines a silly extension that just logs the name of all events triggered
  htmx.defineExtension("silly", {
    onEvent : function(name, evt) {
      console.log("Event " + name + " was triggered!")
    }
  });
```

### Method - `htmx.find()` {#find}

선택자와 일치하는 요소를 찾습니다.

##### Parameters

* `selector` - 일치시킬 선택자

or

* `elt` - 일치하는 요소를 찾을 루트 요소(본인 포함)
* `selector` - 일치시킬 선택자

##### Example

```js
    // ID가 my-div인 div 찾기
    var div = htmx.find("#my-div")

    // 해당 div 내에서 ID가 another-div인 div를 찾습니다.
    var anotherDiv = htmx.find(div, "#another-div")
```

### Method - `htmx.findAll()` {#findAll}

선택자와 일치하는 모든 요소를 찾습니다.

##### Parameters

* `selector` - 일치시킬 선택자

or

* `elt` - 일치하는 요소를 찾을 루트 요소(본인 포함)
* `selector` - 일치시킬 선택자

##### Example

```js
    // 모든 div 찾기
    var allDivs = htmx.findAll("div")

    // 주어진 div 내의 모든 p를 찾습니다.
    var allParagraphsInMyDiv = htmx.findAll(htmx.find("#my-div"), "p")
```

### Method - `htmx.logAll()` {#logAll}

디버깅에 유용한 모든 htmx 이벤트를 기록합니다.

##### Example

```js
    htmx.logAll();
```

### Method - `htmx.logNone()` {#logNone}

이전에 디버거를 사용하도록 설정한 경우 이 옵션을 호출하여 htmx가 이벤트를 기록하지 않도록 합니다.

##### Example

```js
    htmx.logNone();
```

### Property - `htmx.logger` {#logger}

htmx가 로그하는 데 사용하는 logger

##### Value

* `func(elt, eventName, detail)` - 요소를 가지고 있는 함수, eventName, 이벤트 세부 정보 및 로그

##### Example

```js
    htmx.logger = function(elt, event, data) {
        if(console) {
            console.log("INFO:", event, elt, data);
        }
    }
```

### Method - `htmx.off()` {#off}

요소에서 이벤트 리스너를 제거합니다.

##### Parameters

* `eventName` - 리스너를 제거할 이벤트 이름
* `listener` - 제거할 리스너

or

* `target` - 리스너를 제거할 요소
* `eventName` - 리스너를 제거할 이벤트 이름
* `listener` - 제거할 리스너

##### Example

```js
    // BODY에서 click 리스너를 제거합니다.
    htmx.off("click", myEventListener);

    // 주어진 div에서 click 리스너를 제거합니다.
    htmx.off("#my-div", "click", myEventListener)
```

### Method - `htmx.on()` {#on}

요소에 이벤트 리스너를 추가합니다.

##### Parameters

* `eventName` - 리스너를 추가할 이벤트 이름
* `listener` - 추가할 리스너

or

* `target` - 리스너를 추가할 요소
* `eventName` - 리스너를 추가할 이벤트 이름
* `listener` - 추가할 리스너

##### Example

```js
    // body에 click 리스너 추가
    var myEventListener = htmx.on("click", function(evt){ console.log(evt); });

    // 주어진 div에 click 리스너를 추가합니다.
    var myEventListener = htmx.on("#my-div", "click", function(evt){ console.log(evt); });
```

### Method - `htmx.onLoad()` {#onLoad}

`htmx:load` 이벤트에 대한 callback을 추가합니다. 예를 들어 자바스크립트 라이브러리로 
콘텐츠를 초기화하는 등 새로운 콘텐츠를 처리하는 데 사용할 수 있습니다.


##### Parameters

* `callback(elt)` - 새로 로드된 콘텐츠를 호출하기 위한 callback

##### Example

```js
    htmx.onLoad(function(elt){
        MyLibrary.init(elt);
    })
```

### Method - `htmx.parseInterval()` {#parseInterval}

htmx가 수행하는 방식과 일치하는 간격 문자열을 구문 분석합니다. 타이밍 관련 속성이 있는 플러그인에 유용합니다.

주의: int 다음에 `s` 또는 `ms`가 오는 것을 허용합니다. 다른 모든 값은 `parseFloat`를 사용합니다.

##### Parameters

* `str` - timing string

##### Example

```js
    // returns 3000
    var milliseconds = htmx.parseInterval("3s");

    // returns 3 - Caution
    var milliseconds = htmx.parseInterval("3m");
```

### Method - `htmx.process()` {#process}

새 콘텐츠를 처리하여 htmx 동작을 활성화합니다. 이 기능은 일반적인 htmx 요청 외에 
DOM에 새로 추가되는 콘텐츠가 있지만 htmx 속성이 그 콘텐츠에서도 작동하도록 하려는 경우에 유용할 수 있습니다.

##### Parameters

* `elt` - 처리할 요소

##### Example

```js
  document.body.innerHTML = "<div hx-get='/example'>Get it!</div>"
  // 새로 추가된 콘텐츠를 처리합니다.
  htmx.process(document.body);
```

### Method - `htmx.remove()` {#remove}

DOM에서 요소를 제거합니다.

##### Parameters

* `elt` - 제거할 요소

or

* `elt` - 제거할 요소
* `delay` - 요소가 제거되기 전의 지연 시간(밀리초)

##### Example

```js
  // DOM에서 my-div를 제거합니다.
  htmx.remove(htmx.find("#my-div"));

  // 2초 지연 후 DOM에서 my-div를 제거합니다.
  htmx.remove(htmx.find("#my-div"), 2000);
```

### Method - `htmx.removeClass()` {#removeClass}

주어진 요소에서 클래스를 제거합니다.

##### Parameters

* `elt` - 클래스를 제거할 요소
* `class` - 제거될 클래스

or

* `elt` - 클래스를 제거할 요소
* `class` - 제거될 클래스
* `delay` - 클래스가 제거되기 전의 지연 시간(밀리초)

##### Example

```js
  // .myClass를 my-div에서 제거합니다.
  htmx.removeClass(htmx.find("#my-div"), "myClass");

  // .myClass를 my-div에서 6초 후에 제거합니다.
  htmx.removeClass(htmx.find("#my-div"), "myClass", 6000);
```

### Method - `htmx.removeExtension()` {#removeExtension}

htmx에서 지정된 확장을 제거합니다.

##### Parameters

* `name` - 제거할 확장 이름

##### Example

```js
  htmx.removeExtension("my-extension");
```

### Method - `htmx.swap()` {#swap}

HTML 콘텐츠의 교체(및 정리)을 수행합니다.

##### Parameters

* `target` - 교체 대상의 HTML 요소 또는 문자열로 된 선택자
* `content` - 교체할 콘텐츠의 문자열 표현
* `swapSpec` - `hx-swap`의 매개변수를 나타내는 교체 사양
  * `swapStyle` (required) - 교체 스타일(`innerHTML`, `externalHTML`, `beforebegin` 등)
  * `swapDelay`, `settleDelay` (number) - 교체 및 정리 전 각각의 지연
  * `transition` (bool) - 교체에 HTML transition을 사용할지 여부
  * `ignoreTitle` (bool) - 페이지 title 업데이트를 비활성화 여부
  * `head` (string) - `head` 태그 처리 전략(`merge`, `append`)을 지정합니다. `head` 처리를 비활성화하려면 비워 두세요.
  * `scroll`, `scrollTarget`, `show`, `showTarget`, `focusScroll` - 교체 후 스크롤 처리를 지정합니다.
* `swapOptions` - 교체를 위한 추가 *선택적* 매개변수
  * `select` - 교체할 콘텐츠에 대한 선택자(`hx-select`와 동일)
  * `selectOOB` - out of band에서 교체할 콘텐츠에 대한 선택자(`hx-select-oob`와 동일)
  * `eventInfo` - `htmx:afterSwap` 및 `htmx:afterSettle` 요소에 첨부할 개체
  * `anchor` - 스크롤을 트리거한 anchor 요소는 정착 시 view로 스크롤됩니다. 전체 스크롤 처리에 대한 간단한 대안 제공
  * `contextElement` - 교체 작업의 컨텍스트 역할을 하는 DOM 요소입니다. 특정 요소에 대해 현재 활성화된 확장을 찾는 데 사용됩니다.
  * `afterSwapCallback`, `afterSettleCallback` - 각각 교체 및 정리 후에 호출되는 callback 함수입니다. 어떤 인자도 주지 마세요


##### Example

```js
    // "Swapped!"이라는 텍스트를 사용하여 #output 요소 내부 HTML을 div 요소로 교체합니다.
    htmx.swap("#output", "<div>Swapped!</div>", {swapStyle: 'innerHTML'});
```

### Method - `htmx.takeClass()` {#takeClass}

주어진 요소의 형제 요소들로부터 주어진 클래스를 가져와서, 형제 요소들 중 주어진 요소만이 그 클래스를 가지게 합니다.

##### Parameters

* `elt` - 클래스를 가져갈 요소
* `class` - 가져가질 클래스

##### Example

```js
  // tab2의 형제자매로부터 selected 클래스를 가져옵니다.
  htmx.takeClass(htmx.find("#tab2"), "selected");
```

### Method - `htmx.toggleClass()` {#toggleClass}

요소에서 지정된 클래스를 토글합니다.

##### Parameters

* `elt` - 클래스를 토글하는 요소
* `class` - 토클될 클래스

##### Example

```js
  // tab2에서 selected 클래스를 토글합니다.
  htmx.toggleClass(htmx.find("#tab2"), "selected");
```

### Method - `htmx.trigger()` {#trigger}

요소에서 지정된 이벤트를 트리거합니다.

##### Parameters

* `elt` - 이벤트를 트리거하는 요소
* `name` - 트리거할 이벤트의 이름
* `detail` - 이벤트 세부정보

##### Example

```js
  // answer 42로 #tab2에서 myEvent 이벤트를 트리거합니다.
  htmx.trigger("#tab2", "myEvent", {answer:42});
```

### Method - `htmx.values()` {#values}

htmx 값 확인 메커니즘을 통해 주어진 요소에 대해 확인되는 input 값을 반환합니다.

##### Parameters

* `elt` - 값을 확인할 요소
* `request type` - 요청 유형 (예: `get` 또는 `post`). GET이 아닌 경우 요소를 포함하는 form이 포함됩니다. 기본값은 `post`입니다.

##### Example

```js
  // 이 form과 관련된 값을 가져옵니다.
  var values = htmx.values(htmx.find("#myForm"));
```
