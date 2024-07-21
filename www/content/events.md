+++
title = "Events"
+++

Htmx는 동작을 수정하고 개선하는 데 사용할 수 있는 광범위한 이벤트 시스템을 제공합니다. 
이벤트는 다음과 같습니다.

### Event - `htmx:abort` {#htmx:abort}

이 이벤트는 다른 이벤트와 달리 htmx가 *트리거*하지 않고 *대기*한다는 점에서 다릅니다. 

요청을 하는 요소에 `htmx:abort` 이벤트를 보내면 요청을 중단합니다:

```html
<button id="request-button" hx-post="/example">Issue Request</button>
<button onclick="htmx.trigger('#request-button', 'htmx:abort')">Cancel Request</button>
```

### Event - `htmx:afterOnLoad` {#htmx:afterOnLoad}

이 이벤트는 AJAX `onload`가 완료된 후에 트리거됩니다. 
이는 콘텐츠가 아직 교체되거나 정리되었음을 의미하는 것이 아니라 요청이 완료되었음을 의미한다는 점에 유의하세요.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:afterProcessNode` {#htmx:afterProcessNode}

이 이벤트는 htmx가 DOM 노드를 초기화한 후에 트리거됩니다. 확장 프로그램이 노드에 추가 기능을 빌드하려 할 때 유용할 수 있습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소

### Event - `htmx:afterRequest` {#htmx:afterRequest}

이 이벤트는 요청이 성공한 경우(`404`와 같은 원격 오류 코드를 반환했을 수 있음) 또는 
네트워크 오류 상황에서 AJAX 요청이 완료된 후에 트리거됩니다. 이 이벤트는 요청 주기에 따라 
동작을 wrap하기 위해 [`htmx:beforeRequest`](#htmx:beforeRequest)와 함께 사용할 수 있습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성
* `detail.successful` - 응답에 20x 상태 코드가 있거나 `htmx:beforeSwap` 이벤트에서 `detail.isError = false`로 표시되면 true이고, 그렇지 않으면 false입니다.
  `htmx:beforeSwap` event, else false
* `detail.failed` - 응답에 20x 상태 코드가 없거나 `htmx:beforeSwap` 이벤트에서 `detail.isError = true`로 표시된 경우 true, 그렇지 않으면 false입니다.

### Event - `htmx:afterSettle` {#htmx:afterSettle}

이 이벤트는 [DOM이 정리](@/docs.md#swapping)된 후에 트리거됩니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:afterSwap` {#htmx:afterSwap}

이 이벤트는 [DOM안에서 교체가 일어난](@/docs.md#swapping) 후에 트리거됩니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:beforeCleanupElement` {#htmx:beforeCleanupElement}

이 이벤트는 htmx가 요소를 [비활성화](@/attributes/hx-disable.md)하거나 DOM에서 제거하기 전에 트리거됩니다.

##### Details

* `detail.elt` - 정리된 요소

### Event - `htmx:beforeOnLoad` {#htmx:beforeOnLoad}

이 이벤트는 응답 처리가 발생하기 전에 트리거됩니다. 이벤트가 취소되면 교체가 발생하지 않습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:beforeProcessNode` {#htmx:beforeProcessNode}

이 이벤트는 htmx가 DOM 노드를 초기화하고 모든 `hx-` 속성을 처리하기 전에 트리거됩니다. 이를 통해 확장 프로그램 및 기타 외부 코드가 처리되기 전에 DOM 노드의 내용을 수정할 수 있습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소

### Event - `htmx:beforeRequest` {#htmx:beforeRequest}

이 이벤트는 AJAX 요청이 보내지기 전에 트리거됩니다. 이벤트가 취소되면 요청이 발생하지 않습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:beforeSend` {#htmx:beforeSend}

이 이벤트는 요청이 전송되기 직전에 트리거됩니다. 이 이벤트가 발생하면 요청을 취소할 수 없습니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:beforeSwap` {#htmx:beforeSwap}

이 이벤트는 새 콘텐츠가 [DOM 안에서 교체가 이루지기](@/docs.md#swapping) 전에 트리거됩니다. 이벤트가 취소되면 교체가 발생하지 않습니다. 

이벤트 세부정보의 `shouldSwap` 및 `target` property를 수정하여 기본 교체 동작을 수정할 수 있습니다. 
자세한 내용은 [교체 작업 구성](@/docs.md#modifying_swapping_behavior_with_events)에 대한 문서를 참조하세요.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.requestConfig` - AJAX 요청의 구성
* `detail.shouldSwap` - 교체될 콘텐츠(200이 아닌 응답 코드의 경우 기본값은 `false`)
* `detail.ignoreTitle` - `true`인 경우 응답의 제목 태그가 무시됩니다.
* `detail.target` - 교체 대상

### Event - `htmx:beforeTransition` {#htmx:beforeTransition}

이 이벤트는 [View Transition](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)으로 wrap된 교체가 발생하기 전에 트리거됩니다. 
이벤트가 취소되면 View Transition이 발생하지 않고 대신 일반 교체 로직이 발생합니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.requestConfig` - AJAX 요청의 구성
* `detail.shouldSwap` - 교체될 콘텐츠(200이 아닌 응답 코드의 경우 기본값은 `false`)
* `detail.target` - 교체 대상

### Event - `htmx:configRequest` {#htmx:configRequest}

이 이벤트는 htmx가 요청에 포함할 매개변수를 수집한 후에 트리거됩니다. 
이 이벤트는 htmx가 전송할 매개 변수를 포함하거나 업데이트하는 데 사용할 수 있습니다:

```javascript
document.body.addEventListener('htmx:configRequest', function(evt) {
    evt.detail.parameters['auth_token'] = getAuthToken(); // 새 매개변수 추가
});
```

입력 값이 두 번 이상 표시되는 경우 `parameters` 객체의 값은 단일 값이 아닌 배열이 됩니다.

##### Details

* `detail.parameters` - 요청 작업에 제출될 매개변수
* `detail.unfilteredParameters` - [`hx-select`](@/attributes/hx-select.md)로 필터링하기 전에 발견된 매개변수
* `detail.headers` - 요청 헤더
* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청 대상
* `detail.verb` - 사용된 HTTP 메소드

### Event - `htmx:confirm` {#htmx:confirm}

이 이벤트는 요소에서 트리거가 발생한 직후에 트리거됩니다. 이를 통해 AJAX 요청을 취소(또는 지연)할 수 있습니다. 
이벤트에서 `preventDefault()`를 호출하면 지정된 요청을 보내지 않습니다. 
`detail` 객체에는 나중에 실제 AJAX 요청을 발행하는 데 사용할 수 있는 함수인 `evt.detail.issueRequest()`가 포함되어 있습니다. 
이 두 기능을 결합하면 비동기 confirmation 대화 상자를 만들 수 있습니다.

다음은 `confirm-with-sweet-alert='true'` 속성이 있는 모든 요소에 [sweet alert](https://sweetalert.js.org/guides/)를 사용하는 예시입니다:

```javascript
document.body.addEventListener('htmx:confirm', function(evt) {
  if (evt.target.matches("[confirm-with-sweet-alert='true']")) {
    evt.preventDefault();
    swal({
      title: "Are you sure?",
      text: "Are you sure you are sure?",
      icon: "warning",
      buttons: true,
      dangerMode: true,
    }).then((confirmed) => {
      if (confirmed) {
        evt.detail.issueRequest();
      }
    });
  }
});
```

##### Details

{target: target, elt: elt, path: path, verb: verb, triggeringEvent: event, etc: etc, issueRequest: issueRequest}

* `detail.elt` - 해당 요소
* `detail.etc` - 추가적인 요청의 정보 (대부분 미사용)
* `detail.issueRequest` - 요청을 보내기 위해 호출할 수 있는 인수 없는 함수(`evt.preventDefault()`와 쌍을 이루어야 함!)
* `detail.path` - 요청의 경로
* `detail.target` - 요청의 대상
* `detail.triggeringEvent` - 이 요청을 트리거한 원래 이벤트
* `detail.verb` - 요청 메소드 (예를 들어 `GET`)

### Event - `htmx:historyCacheError` {#htmx:historyCacheError}

이 이벤트는 `localStorage`에 캐시를 저장하려는 시도가 실패할 때 트리거됩니다.

##### Details

* `detail.cause` - 히스토리를 `localStorage`에 저장하려는 시도 중에 발생한 `Exception`

### Event - `htmx:historyCacheMiss` {#htmx:historyCacheMiss}

이 이벤트는 히스토리를 복원하는 동안 캐시 미스가 발생할 때 트리거됩니다.

##### Details

* `detail.xhr` - 복원을 위해 원격 콘텐츠를 가져올 `XMLHttpRequest`
* `detail.path` - 복원 중인 페이지의 경로와 쿼리

### Event - `htmx:historyCacheMissError` {#htmx:historyCacheMissError}

이 이벤트는 캐시 미스가 발생하고 복원을 위한 콘텐츠가 서버에서 응답되었지만, 응답이 오류인 경우(예: `404`) 트리거됩니다.

##### Details

* `detail.xhr` - `XMLHttpRequest`
* `detail.path` - 복원 중인 페이지의 경로와 쿼리

### Event - `htmx:historyCacheMissLoad` {#htmx:historyCacheMissLoad}

이 이벤트는 캐시 미스가 발생하고 복원을 위한 콘텐츠가 서버에서 성공적으로 응답되었을 때 트리거됩니다.

##### Details

* `detail.xhr` - `XMLHttpRequest`
* `detail.path` - 복원 중인 페이지의 경로와 쿼리

### Event - `htmx:historyRestore` {#htmx:historyRestore}

이 이벤트는 htmx가 히스토리 복원 작업을 처리할 때 트리거됩니다.

##### Details

* `detail.path` - 복원 중인 페이지의 경로와 쿼리

### Event - `htmx:beforeHistorySave` {#htmx:beforeHistorySave}

이 이벤트는 htmx가 히스토리 복원 작업을 처리할 때 트리거됩니다.

##### Details

* `detail.path` - 복원 중인 페이지의 경로와 쿼리
* `detail.historyElt` - 복원될 히스토리 요소

### Event - `htmx:load` {#htmx:load}

이 이벤트는 htmx가 새로운 노드를 DOM에 로드할 때 트리거됩니다.

##### Details

* `detail.elt` - 새로 추가된 요소

### Event - `htmx:noSSESourceError` {#htmx:noSSESourceError}

이 이벤트는 요소가 트리거에서 SSE 이벤트를 참조하지만, 상위 SSE 소스가 정의되지 않은 경우 트리거됩니다.

##### Details

* `detail.elt` - 잘못된 SSE 트리거가 있는 요소

### Event - `htmx:oobAfterSwap` {#htmx:oobAfterSwap}

이 이벤트는 [out of band 교체 작업](@/docs.md#oob_swaps)의 일부로 트리거되며 [교체 작업 이후 이벤트](#htmx:afterSwap)와 동일하게 작동합니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.shouldSwap` -  콘텐츠가 교체되었는 가(기본값은 `true`)
* `detail.target` - 교체 대상
* `detail.fragment` - 응답 조각

### Event - `htmx:oobBeforeSwap` {#htmx:oobBeforeSwap}

이 이벤트는 [out of band 교체 작업](@/docs.md#oob_swaps)의 일부로 트리거되며 [교체 작업 이전 이벤트](#htmx:beforeSwap)와 동일하게 작동합니다.

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.shouldSwap` - 콘텐츠가 교체되었는 가(기본값은 `true`)
* `detail.target` - 교체 대상
* `detail.fragment` - 응답 조각

### Event - `htmx:oobErrorNoTarget` {#htmx:oobErrorNoTarget}

이 이벤트는 [out of band swap](@/docs.md#oob_swaps)이 DOM에 전환할 대응하는 요소가 없을 때 트리거됩니다.

##### Details

* `detail.content` - 잘못된 OOB `ID`를 가진 요소

### Event - `htmx:onLoadError` {#htmx:onLoadError}

이 이벤트는 AJAX 호출의 `load` 처리 중에 오류가 발생하면 트리거됩니다.

##### Details

* `detail.xhr` - the `XMLHttpRequest`
* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청 대상
* `detail.exception` - 발생한 예외
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:prompt` {#htmx:prompt}

이 이벤트는 [`hx-prompt`](@/attributes/hx-prompt.md) 속성을 사용하여 사용자에게 프롬프트가 표시된 후에 트리거됩니다. 
이 이벤트가 취소되면 AJAX 요청이 발생하지 않습니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청 대상
* `detail.prompt` - 프롬프트에 대한 사용자 응답

### Event - `htmx:beforeHistoryUpdate` {#htmx:beforeHistoryUpdate}

이 이벤트는 히스토리 업데이트가 수행되기 전에 트리거됩니다. 히스토리 업데이트에 사용되는 `path` 또는 `type`을 수정하는 데 사용할 수 있습니다.

##### Details

* `detail.history` - 히스토리 업데이트를 위한 `path`와 `type` (push, replace)
* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - `XMLHttpRequest`
* `detail.target` - 요청의 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:pushedIntoHistory` {#htmx:pushedIntoHistory}

이 이벤트는 URL이 히스토리에 추가된 후에 트리거됩니다.

##### Details

* `detail.path` - 히스토리에 추가된 URL의 경로와 쿼리

### Event - `htmx:replacedInHistory` {#htmx:replacedInHistory}

이 이벤트는 URL이 히스토리에서 교체된 후에 트리거됩니다.

##### Details

* `detail.path` - 히스토리에서 교체된 URL의 경로와 쿼리

### Event - `htmx:responseError` {#htmx:responseError}

이 이벤트는 HTTP 오류 응답이 발생했을 때 트리거됩니다.

##### Details

* `detail.xhr` - `XMLHttpRequest`
* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청의 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:sendError` {#htmx:sendError}

이 이벤트는 네트워크 오류로 인해 HTTP 요청이 발생하지 못할 때 트리거됩니다.

##### Details

* `detail.xhr` - `XMLHttpRequest`
* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청의 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:sseError` {#htmx:sseError}

이 이벤트는 SSE 소스에서 오류가 발생했을 때 트리거됩니다.

##### Details

* `detail.elt` - 잘못된 SSE 소스가 있는 요소
* `detail.error` - 오류
* `detail.source` - SSE 소스

### Event - `htmx:swapError` {#htmx:swapError}

이 이벤트는 교체 단계에서 오류가 발생했을 때 트리거됩니다.

##### Details

* `detail.xhr` - `XMLHttpRequest`
* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 요청의 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:targetError` {#htmx:targetError}

이 이벤트는 [`hx-target`](@/attributes/hx-target.md) 속성에 잘못된 선택자(예: 앞에 `#`가 없는 요소 ID)가 사용될 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소
* `detail.target` - 잘못된 CSS 선택자

### Event - `htmx:timeout` {#htmx:timeout}

이 이벤트는 요청 시간 초과가 발생하면 트리거됩니다. 이 이벤트는 XMLHttpRequest의 일반적인 `timeout` 이벤트를 wrap합니다.

timeout 시간은 `htmx.config.timeout`을 사용하여 설정하거나 [`hx-request`](@/attributes/hx-request.md)를 사용하여 요소별로 설정할 수 있습니다. 

##### Details

* `detail.elt` - 요청을 보낸 요소
* `detail.xhr` - the `XMLHttpRequest`
* `detail.target` - 요청의 대상
* `detail.requestConfig` - AJAX 요청의 구성

### Event - `htmx:trigger` {#htmx:trigger}

이 이벤트는 AJAX 요청이 지정되지 않은 경우에도 AJAX 요청이 있을 때마다 트리거됩니다. 이 이벤트는 주로 `hx-trigger`가 
클라이언트 측 스크립트를 실행할 수 있도록 하기 위한 것으로, AJAX 요청에는 
[`htmx:beforeRequest`](#htmx:beforeRequest) 또는 [`htmx:afterRequest`](#htmx:afterRequest)와 같은 더 세분화된 이벤트를 사용할 수 있습니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소

### Event - `htmx:validateUrl` {#htmx:validateUrl}

이 이벤트는 요청이 이루어지기 전에 트리거되어 htmx가 요청할 URL의 유효성을 검사할 수 있습니다. 
이벤트에서 `preventDefault()`가 호출되면 요청이 이루어지지 않습니다.

```javascript
document.body.addEventListener('htmx:validateUrl', function (evt) {
  // 현재 서버와 myserver.com에 대한 요청만 허용합니다.
  if (!evt.detail.sameHost && evt.detail.url.hostname !== "myserver.com") {
    evt.preventDefault();
  }
});
```

##### Details

* `detail.elt` - 요청을 트리거한 요소
* `detail.url` - 요청이 전송될 URL을 나타내는 URL 객체입니다.
* `detail.sameHost` - 요청이 document와 동일한 호스트에 대한 경우 `true`입니다.

### Event - `htmx:validation:validate` {#htmx:validation:validate}

이 이벤트는 요소의 유효성을 검사하기 전에 트리거됩니다. 이 이벤트는 
사용자 정의 유효성 검사 규칙을 구현하기 위해 `elt.setCustomValidity()` 메서드와 함께 사용할 수 있습니다.

```html
<form hx-post="/test">
  <input _="on htmx:validation:validate
               if my.value != 'foo'
                  call me.setCustomValidity('Please enter the value foo')
               else
                  call me.setCustomValidity('')"
         name="example">
</form>
```

##### Details

* `detail.elt` - 요청을 트리거한 요소

### Event - `htmx:validation:failed` {#htmx:validation:failed}

이 이벤트는 요소가 유효성 검사에 실패할 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소
* `detail.message` - 유효성 검사 오류 메시지
* `detail.validity` - 유효성 검사 실패 방법을 지정하는 속성을 포함하는 유효성 객체

### Event - `htmx:validation:halted` {#htmx:validation:halted}

이 이벤트는 유효성 검사 오류로 인해 요청이 중단될 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소
* `detail.errors` - 유효하지 않은 요소 및 이와 관련된 오류가 있는 오류 객체 배열입니다.

### Event - `htmx:xhr:abort` {#htmx:xhr:abort}

이 이벤트는 ajax 요청이 중단될 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소

### Event - `htmx:xhr:loadstart` {#htmx:xhr:loadstart}

이 이벤트는 ajax 요청이 시작될 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소

### Event - `htmx:xhr:loadend` {#htmx:xhr:loadend}

이 이벤트는 ajax 요청이 완료될 때 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소

### Event - `htmx:xhr:progress` {#htmx:xhr:progress}

이 이벤트는 진행 상태를 지원하는 ajax 요청이 진행 중일 때 주기적으로 트리거됩니다.

##### Details

* `detail.elt` - 요청을 트리거한 요소
