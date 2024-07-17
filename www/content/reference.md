+++
title = "Reference"
+++

## Contents

* [htmx Core Attributes](#attributes)
* [htmx Additional Attributes](#attributes-additional)
* [htmx CSS Classes](#classes)
* [htmx Request Headers](#request_headers)
* [htmx Response Headers](#response_headers)
* [htmx Events](#events)
* [htmx Extensions](https://extensions.htmx.org)
* [JavaScript API](#api)
* [Configuration Options](#config)

## Core Attribute Reference {#attributes}

htmx를 사용할 때 가장 일반적으로 사용되는 속성입니다.

<div class="info-table">

| Attribute                                        | Description                                                  |
|--------------------------------------------------|--------------------------------------------------------------|
| [`hx-get`](@/attributes/hx-get.md)               | 지정된 URL로 `GET` 요청을 보냅니다                                      |
| [`hx-post`](@/attributes/hx-post.md)             | 지정된 URL로 `POST` 요청을 보냅니다                                     |
| [`hx-on*`](@/attributes/hx-on.md)                | 요소에서 인라인 스크립트로 이벤트를 처리합니다                                    |
| [`hx-push-url`](@/attributes/hx-push-url.md)     | 히스토리를 생성하기 위해 브라우저 주소창에 URL을 추가합니다                           |
| [`hx-select`](@/attributes/hx-select.md)         | 응답으로 교체할 콘텐츠를 선택합니다                                          |
| [`hx-select-oob`](@/attributes/hx-select-oob.md) | 대상 이외의 위치(out of band)에서 응답으로 교체할 콘텐츠를 선택합니다                 |
| [`hx-swap`](@/attributes/hx-swap.md)             | 콘텐츠 교체 방식을 제어합니다 (`outerHTML`, `beforeend`, `afterend`, ...) |
| [`hx-swap-oob`](@/attributes/hx-swap-oob.md)     | 응답으로 교체할 요소 표시 (out of band)                                 |
| [`hx-target`](@/attributes/hx-target.md)         | 교체할 대상 요소를 지정합니다                                             |
| [`hx-trigger`](@/attributes/hx-trigger.md)       | 요청을 트리거하는 이벤트를 지정합니다                                         |
| [`hx-vals`](@/attributes/hx-vals.md)             | 요청과 함께 보낼 값을 추가합니다 (JSON 양식으로)                               |

</div>

## Additional Attribute Reference {#attributes-additional}

기타 모든 속성은 htmx에서 사용할 수 있습니다.

<div class="info-table">

| Attribute                                            | Description                                                                                                                       |
|------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| [`hx-boost`](@/attributes/hx-boost.md)               | 링크 및 양식에 [점진적 향상](https://en.wikipedia.org/wiki/Progressive_enhancement)을 추가합니다                                                   |
| [`hx-confirm`](@/attributes/hx-confirm.md)           | 요청을 실행하기 전에 `confirm()` 대화 상자를 표시합니다                                                                                              |
| [`hx-delete`](@/attributes/hx-delete.md)             | 지정된 URL로 `delete` 요청을 보냅니다                                                                                                        |
| [`hx-disable`](@/attributes/hx-disable.md)           | 지정된 노드 및 모든 하위 노드에 대한 HTMX 처리를 비활성화합니다                                                                                            |
| [`hx-disabled-elt`](@/attributes/hx-disabled-elt.md) | 요청이 전송되는 동안 지정된 요소에 `disabled` 속성을 추가합니다                                                                                          |
| [`hx-disinherit`](@/attributes/hx-disinherit.md)     | 자식 노드에 대한 자동 속성 상속 제어 및 비활성화                                                                                                      |
| [`hx-encoding`](@/attributes/hx-encoding.md)         | 요청의 인코딩 유형 변경                                                                                                                     |
| [`hx-ext`](@/attributes/hx-ext.md)                   | 이 요소에 사용할 확장 지정                                                                                                                   |
| [`hx-headers`](@/attributes/hx-headers.md)           | 요청과 함께 보내질 헤더를 추가합니다                                                                                                              |
| [`hx-history`](@/attributes/hx-history.md)           | 민감한 데이터가 히스토리 캐시에 저장되는 것을 방지합니다                                                                                                   |
| [`hx-history-elt`](@/attributes/hx-history-elt.md)   | 히스토리 탐색 중 스냅샷 및 복원할 요소를 선택합니다                                                                                                     |
| [`hx-include`](@/attributes/hx-include.md)           | 요청에 추가 데이터를 포함합니다                                                                                                                 |
| [`hx-indicator`](@/attributes/hx-indicator.md)       | 요청 중에 `htmx-request` 클래스를 넣을 요소 지정                                                                                                |
| [`hx-inherit`](@/attributes/hx-inherit.md)           | 기본적으로 비활성화된 경우, 자식 노드에 대한 자동 속성 상속을 제어하고 활성화합니다                                                                                   |
| [`hx-params`](@/attributes/hx-params.md)             | 요청과 함께 제출될 매개변수를 필터링합니다                                                                                                           |
| [`hx-patch`](@/attributes/hx-patch.md)               | 지정된 URL로 `PATCH` 요청을 보냅니다                                                                                                         |
| [`hx-preserve`](@/attributes/hx-preserve.md)         | 요청 사이에 변경되지 않을 요소를 지정합니다                                                                                                          |
| [`hx-prompt`](@/attributes/hx-prompt.md)             | 요청을 제출하기 전에 `prompt()`를 보여줍니다                                                                                                     |
| [`hx-put`](@/attributes/hx-put.md)                   | 지정된 URL로 `PUT` 요청을 보냅니다                                                                                                           |
| [`hx-replace-url`](@/attributes/hx-replace-url.md)   | 브라우저 위치 표시줄에 URL을 바꿉니다                                                                                                            |
| [`hx-request`](@/attributes/hx-request.md)           | 요청의 다양한 측면을 구성합니다                                                                                         |
| [`hx-sync`](@/attributes/hx-sync.md)                 | 다른 요소에 의해 수행된 요청이 어떻게 동기화되는지를 제어합니다                                                                                               |
| [`hx-validate`](@/attributes/hx-validate.md)         | 요청 전에 요소가 스스로 유효성을 검사하도록 강제                                                                                                       |
| [`hx-vars`](@/attributes/hx-vars.md)                 | adds values dynamically to the parameters to submit with the request (deprecated, please use [`hx-vals`](@/attributes/hx-vals.md)) |

</div>

## CSS Class Reference {#classes}

<div class="info-table">

| Class | Description |
|-----------|-------------|
| `htmx-added` | 새 콘텐츠가 교체되기 전에 적용되며, 정착된 후에 제거됩니다.
| `htmx-indicator` | `htmx-request` 클래스가 존재할 때 가시성(opacity:1)이 전환되는 동적으로 생성된 클래스입니다.
| `htmx-request` | 요청이 진행되는 동안 요소나 [`hx-indicator`](@/attributes/hx-indicator.md)로 지정된 요소에 적용됩니다.
| `htmx-settling` | 콘텐츠가 교체된 후 대상에 적용되며, 정착된 후에 제거됩니다. 지속 시간은 [`hx-swap`](@/attributes/hx-swap.md)을 통해 수정할 수 있습니다.
| `htmx-swapping` | 콘텐츠가 교체되기 전에 대상에 적용되며, 교체된 후에 제거됩니다. 지속 시간은 [`hx-swap`](@/attributes/hx-swap.md)을 통해 수정할 수 있습니다.

</div>

## HTTP Header Reference {#headers}

### Request Headers Reference {#request_headers}

<div class="info-table">

| Header | Description |
|--------|-------------|
| `HX-Boosted` | 요청이 [hx-boost](@/attributes/hx-boost.md)를 사용하는 요소를 통해 이루어졌음을 나타냅니다.
| `HX-Current-URL` | 브라우저의 현재 URL
| `HX-History-Restore-Request` | 로컬 히스토리 캐시 누락 후 히스토리 복원 요청인 경우 "true"입니다.
| `HX-Prompt` | [hx-prompt](@/attributes/hx-prompt.md)에 대한 사용자 응답.
| `HX-Request` | 항상 "true"
| `HX-Target` | 대상 요소가 있는 경우 그것의 `id`
| `HX-Trigger-Name` | 트리거된 요소가 존재하는 경우 그것의 `name`
| `HX-Trigger` | 트리거된 요소가 존재하는 경우 그것의 `id`

</div>

### Response Headers Reference {#response_headers}

<div class="info-table">

| Header                                               | Description |
|------------------------------------------------------|-------------|
| [`HX-Location`](@/headers/hx-location.md)            | 전체 페이지를 새로 고침하지 않고 클라이언트 측에서 리디렉션을 수행할 수 있습니다
| [`HX-Push-Url`](@/headers/hx-push-url.md)            | 새 URL을 히스토리 스택에 추가합니다
| `HX-Redirect`                                        | 클라이언트 측에서 새로운 위치로 리디렉션하는 데 사용할 수 있습니다
| `HX-Refresh`                                         | "true"로 설정되면 클라이언트 측에서 페이지를 전체 새로 고침합니다
| [`HX-Replace-Url`](@/headers/hx-replace-url.md)      | 위치 표시줄의 현재 URL을 대체합니다
| `HX-Reswap`                                          | 응답이 어떻게 교체될지를 지정할 수 있습니다. 가능한 값은 [hx-swap](@/attributes/hx-swap.md)을 참조하세요
| `HX-Retarget`                                        | 콘텐츠 업데이트의 대상을 페이지의 다른 요소로 업데이트하는 CSS 선택자입니다
| `HX-Reselect`                                        | 응답의 어느 부분을 교체할지 선택할 수 있는 CSS 선택자입니다. 트리거 요소에 존재하는 [`hx-select`](@/attributes/hx-select.md)을 재정의합니다
| [`HX-Trigger`](@/headers/hx-trigger.md)              | 클라이언트 측 이벤트를 트리거할 수 있습니다
| [`HX-Trigger-After-Settle`](@/headers/hx-trigger.md) | 정착 단계 후에 클라이언트 측 이벤트를 트리거할 수 있습니다
| [`HX-Trigger-After-Swap`](@/headers/hx-trigger.md)   | 교체 단계 후에 클라이언트 측 이벤트를 트리거할 수 있습니다

</div>

## Event Reference {#events}

<div class="info-table">

| Event | Description |
|-------|-------------|
| [`htmx:abort`](@/events.md#htmx:abort) | 이 이벤트를 요소에 전송하여 요청을 중단합니다
| [`htmx:afterOnLoad`](@/events.md#htmx:afterOnLoad) | AJAX 요청이 성공적인 응답 처리를 완료한 후 트리거됩니다
| [`htmx:afterProcessNode`](@/events.md#htmx:afterProcessNode) | htmx가 노드를 초기화한 후 트리거됩니다
| [`htmx:afterRequest`](@/events.md#htmx:afterRequest)  | AJAX 요청이 완료된 후 트리거됩니다
| [`htmx:afterSettle`](@/events.md#htmx:afterSettle)  | DOM이 안정된 후 트리거됩니다
| [`htmx:afterSwap`](@/events.md#htmx:afterSwap)  | 새 콘텐츠가 교체된 후 트리거됩니다.
| [`htmx:beforeCleanupElement`](@/events.md#htmx:beforeCleanupElement)  | htmx [비활성화](@/attributes/hx-disable.md) 전에 트리거되거나 DOM에서 요소를 제거하기 전에 트리거됩니다
| [`htmx:beforeOnLoad`](@/events.md#htmx:beforeOnLoad)  | 응답 처리가 발생하기 전에 트리거됩니다
| [`htmx:beforeProcessNode`](@/events.md#htmx:beforeProcessNode) | htmx가 노드를 초기화하기 전에 트리거됩니다
| [`htmx:beforeRequest`](@/events.md#htmx:beforeRequest)  | AJAX 요청이 이루어지기 전에 트리거됩니다
| [`htmx:beforeSwap`](@/events.md#htmx:beforeSwap)  | 교체가 완료되기 전에 트리거되어 교체를 설정할 수 있습니다
| [`htmx:beforeSend`](@/events.md#htmx:beforeSend)  | AJAX 요청이 전송되기 직전에 트리거됩니다
| [`htmx:configRequest`](@/events.md#htmx:configRequest)  | 요청 전에 트리거되며 매개변수, 헤더를 사용자 정의할 수 있습니다
| [`htmx:confirm`](@/events.md#htmx:confirm)  | 요소에서 트리거가 발생한 후에 트리거되며, AJAX 요청 발행을 취소하거나 지연시킬 수 있습니다
| [`htmx:historyCacheError`](@/events.md#htmx:historyCacheError)  | 캐시 쓰기 중 오류가 발생했을 때 트리거됩니다
| [`htmx:historyCacheMiss`](@/events.md#htmx:historyCacheMiss)  | 히스토리 하위 시스템에서 캐시 미스가 발생했을 때 트리거됩니다
| [`htmx:historyCacheMissError`](@/events.md#htmx:historyCacheMissError)  | 원격 검색이 실패했을 때 트리거됩니다
| [`htmx:historyCacheMissLoad`](@/events.md#htmx:historyCacheMissLoad)  | 원격 검색이 성공했을 때 트리거됩니다
| [`htmx:historyRestore`](@/events.md#htmx:historyRestore)  | htmx가 히스토리 복원 작업을 처리할 때 트리거됩니다
| [`htmx:beforeHistorySave`](@/events.md#htmx:beforeHistorySave)  | 콘텐츠가 히스토리 캐시에 저장되기 전에 트리거됩니다
| [`htmx:load`](@/events.md#htmx:load)  | 새 콘텐츠가 DOM에 추가될 때 트리거됩니다
| [`htmx:noSSESourceError`](@/events.md#htmx:noSSESourceError)  | 요소가 트리거에서 SSE 이벤트를 참조하지만 상위 SSE 소스가 정의되지 않은 경우 트리거됩니다
| [`htmx:onLoadError`](@/events.md#htmx:onLoadError)  | htmx에서 onLoad 처리 중 예외가 발생하면 트리거됩니다
| [`htmx:oobAfterSwap`](@/events.md#htmx:oobAfterSwap)  | out of band 요소가 교체된 후 트리거됩니다
| [`htmx:oobBeforeSwap`](@/events.md#htmx:oobBeforeSwap)  | out of band 요소 교체가 완료되기 전에 트리거되어 교체를 설정할 수 있습니다
| [`htmx:oobErrorNoTarget`](@/events.md#htmx:oobErrorNoTarget)  | out of band 요소와 현재 DOM에 일치하는 ID가 없을 때 트리거됩니다
| [`htmx:prompt`](@/events.md#htmx:prompt)  | prompt가 표시된 후 트리거됩니다
| [`htmx:pushedIntoHistory`](@/events.md#htmx:pushedIntoHistory)  | URL이 히스토리에 들어간 후 트리거됩니다
| [`htmx:responseError`](@/events.md#htmx:responseError)  | HTTP 응답 오류(`200` 또는 `300`이 아닌 응답 코드)가 발생하면 트리거됩니다
| [`htmx:sendError`](@/events.md#htmx:sendError)  | 네트워크 오류로 인해 HTTP 요청이 발생하지 않을 때 트리거됩니다
| [`htmx:sseError`](@/events.md#htmx:sseError)  | SSE 소스에서 오류가 발생하면 트리거됩니다
| [`htmx:sseOpen`](/events#htmx:sseOpen)  | SSE 소스가 열릴 때 트리거됩니다
| [`htmx:swapError`](@/events.md#htmx:swapError)  | 교체 단계에서 오류가 발생하면 트리거됩니다
| [`htmx:targetError`](@/events.md#htmx:targetError)  | 잘못된 대상을 지정하면 트리거됩니다
| [`htmx:timeout`](@/events.md#htmx:timeout)  | 요청 시간 초과가 발생하면 트리거됩니다
| [`htmx:validation:validate`](@/events.md#htmx:validation:validate)  | 요소의 유효성을 검사하기 전에 트리거됩니다
| [`htmx:validation:failed`](@/events.md#htmx:validation:failed)  | 요소 유효성 검사에 실패하면 트리거됩니다
| [`htmx:validation:halted`](@/events.md#htmx:validation:halted)  | 유효성 검사 오류로 인해 요청이 중단될 때 트리거됩니다
| [`htmx:xhr:abort`](@/events.md#htmx:xhr:abort)  | ajax 요청이 중단될 때 트리거됩니다
| [`htmx:xhr:loadend`](@/events.md#htmx:xhr:loadend)  | ajax 요청이 종료되면 트리거됩니다
| [`htmx:xhr:loadstart`](@/events.md#htmx:xhr:loadstart)  | ajax 요청이 시작될 때 트리거됩니다
| [`htmx:xhr:progress`](@/events.md#htmx:xhr:progress)  | 진행 이벤트를 지원하는 Ajax 요청 중에 주기적으로 트리거됩니다

</div>

## JavaScript API Reference {#api}

<div class="info-table">

| Method | Description |
|-------|-------------|
| [`htmx.addClass()`](@/api.md#addClass)  | 주어진 요소에 클래스를 추가합니다
| [`htmx.ajax()`](@/api.md#ajax)  | htmx 스타일 ajax 요청을 보냅니다
| [`htmx.closest()`](@/api.md#closest)  | 주어진 선택자와 일치하는 요소랑 가장 가까운 부모를 찾습니다
| [`htmx.config`](@/api.md#config)  | 현재 htmx 구성 객체를 보유하는 property
| [`htmx.createEventSource`](@/api.md#createEventSource)  | htmx용 SSE 이벤트 소스 객체를 생성하는 함수를 보유하는 property
| [`htmx.createWebSocket`](@/api.md#createWebSocket)  | htmx용 WebSocket 객체를 생성하는 함수를 보유하는 property
| [`htmx.defineExtension()`](@/api.md#defineExtension)  | htmx [확장자](https://extensions.htmx.org)를 정의합니다
| [`htmx.find()`](@/api.md#find)  | 선택자와 일치하는 단일 요소를 찾습니다
| [`htmx.findAll()` `htmx.findAll(elt, selector)`](@/api.md#find)  | 지정된 선택자와 일치하는 모든 요소를 찾습니다
| [`htmx.logAll()`](@/api.md#logAll)  | 모든 htmx 이벤트를 기록하는 logger를 설치합니다
| [`htmx.logger`](@/api.md#logger)  | 현재 로거로 설정된 property (기본값은 `null`)
| [`htmx.off()`](@/api.md#off)  | 지정된 요소에서 event listener를 제거합니다
| [`htmx.on()`](@/api.md#on)  | 지정된 요소에 event listener를 생성하여 반환합니다
| [`htmx.onLoad()`](@/api.md#onLoad)  | `htmx:load` 이벤트에 대한 callback handler를 추가합니다
| [`htmx.parseInterval()`](@/api.md#parseInterval)  | 간격 선언을 밀리초 값으로 구문 분석합니다
| [`htmx.process()`](@/api.md#process)  | 주어진 요소와 그 자식을 처리하여 모든 htmx 동작을 연결합니다
| [`htmx.remove()`](@/api.md#remove)  | 지정된 요소를 제거합니다
| [`htmx.removeClass()`](@/api.md#removeClass)  | 지정된 요소에서 클래스를 제거합니다
| [`htmx.removeExtension()`](@/api.md#removeExtension)  | htmx [확장](https://extensions.htmx.org)을 제거합니다
| [`htmx.swap()`](@/api.md#swap)  | HTML 콘텐츠의 교체(및 정리)을 수행합니다.
| [`htmx.takeClass()`](@/api.md#takeClass)  | 주어진 요소에 대해 다른 요소에서 클래스를 가져옵니다
| [`htmx.toggleClass()`](@/api.md#toggleClass)  | 주어진 요소에서 클래스를 toggle합니다
| [`htmx.trigger()`](@/api.md#trigger)  | 요소에서 이벤트를 트리거합니다
| [`htmx.values()`](@/api.md#values)  | 주어진 요소와 연관된 입력 값을 반환합니다

</div>


## Configuration Reference {#config}

Htmx에는 프로그래밍 방식으로 또는 선언적으로 액세스할 수 있는 몇 가지 구성 옵션이 있습니다.  
그 옵션은 아래에 나열되어 있습니다:

<div class="info-table">

| Config Variable                       | Info                                                                                                                                                                                                                                    |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `htmx.config.historyEnabled`          | defaults to `true`, 테스트에만 유용합니다                                                                                                                                                                                                         |
| `htmx.config.historyCacheSize`        | defaults to 10                                                                                                                                                                                                                          |
| `htmx.config.refreshOnHistoryMiss`    | defaults to `false`, `true`로 설정하면 htmx는 히스토리 누락 시 AJAX 요청을 사용하는 대신 전체 페이지 새로 고침을 실행합니다                                                                                                                                                  |
| `htmx.config.defaultSwapStyle`        | defaults to `innerHTML`                                                                                                                                                                                                                 |
| `htmx.config.defaultSwapDelay`        | defaults to 0                                                                                                                                                                                                                           |
| `htmx.config.defaultSettleDelay`      | defaults to 20                                                                                                                                                                                                                          |
| `htmx.config.includeIndicatorStyles`  | defaults to `true` (indicator 스타일이 로드되는지 여부를 결정합니다)                                                                                                                                                                                     |
| `htmx.config.indicatorClass`          | defaults to `htmx-indicator`                                                                                                                                                                                                            |
| `htmx.config.requestClass`            | defaults to `htmx-request`                                                                                                                                                                                                              |
| `htmx.config.addedClass`              | defaults to `htmx-added`                                                                                                                                                                                                                |
| `htmx.config.settlingClass`           | defaults to `htmx-settling`                                                                                                                                                                                                             |
| `htmx.config.swappingClass`           | defaults to `htmx-swapping`                                                                                                                                                                                                             |
| `htmx.config.allowEval`               | defaults to `true`, 특정 기능(예: 트리거 필터)에 대한 htmx의 eval 사용을 비활성화하는 데 사용할 수 있습니다                                                                                                                                                             |
| `htmx.config.allowScriptTags`         | defaults to `true`, 새 콘텐츠에서 발견된 스크립트 태그를 htmx가 처리할지 여부를 결정합니다                                                                                                                                                                           |
| `htmx.config.inlineScriptNonce`       | defaults to `''`, 이것의 의미는 인라인 스크립트에 nonce가 추가되지 않는 것입니다                                                                                                                                                                                 |
| `htmx.config.inlineSlyeNonce`         | defaults to `''`, 이것의 의미는 인라인 스타일에 nonce가 추가되지 않는 것입니다                                                                                                                                                                                  |
| `htmx.config.attributesToSettle`      | defaults to `["class", "style", "width", "height"]`, 정리 단계에서 정리할 속성                                                                                                                                                                     |
| `htmx.config.wsReconnectDelay`        | defaults to `full-jitter`                                                                                                                                                                                                               |
| `htmx.config.wsBinaryType`            | defaults to `blob`, WebSocket 연결을 통해 수신되는 [binary 데이터 유형](https://developer.mozilla.org/docs/Web/API/WebSocket/binaryType)                                                                                                              |
| `htmx.config.disableSelector`         | defaults to `[hx-disable], [data-hx-disable]`, htmx는 이 속성이 있는 요소나 상위 요소를 처리하지 않습니다                                                                                                                                                      |
| `htmx.config.withCredentials`         | defaults to `false`, 쿠키, 인증 헤더 또는 TLS 클라이언트 인증서와 같은 자격 증명을 사용하여 크로스-사이트 액세스 제어 요청을 허용합니다                                                                                                                                                |
| `htmx.config.timeout`                 | defaults to 0, 요청이 자동으로 종료되기까지 걸릴 수 있는 시간(milliseconds)                                                                                                                                                                                 |
| `htmx.config.scrollBehavior`          | defaults to 'smooth', 페이지 전환 시 부스트 링크의 동작을 설정합니다. 허용되는 값은 `auto` 및 `Smooth`입니다. Smooth는 페이지 상단으로 부드럽게 스크롤되는 반면 Auto는 바닐라 링크처럼 작동합니다.                                                                                                    |
| `htmx.config.defaultFocusScroll`      | focused 요소를 스크롤하여 뷰에 표시해야 하는 경우 기본값은 false이며 [focus-scroll](@/attributes/hx-swap.md#focus-scroll) 교체 수정자를 사용하여 재정의할 수 있습니다.                                                                                                             |
| `htmx.config.getCacheBusterParam`     | defaults to false, true로 설정하면 htmx는 `org.htmx.cache-buster=targetElementId` 형식으로 `GET` 요청에 대상 요소를 추가합니다.                                                                                                                                |
| `htmx.config.globalViewTransitions`   | `true`로 설정하면 htmx는 새 콘텐츠를 교체할 때 [View Transition](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) API를 사용합니다.                                                                                                    |
| `htmx.config.methodsThatUseUrlParams` | defaults to `["get"]`, htmx는 요청 본문이 아닌 URL에서 매개 변수를 인코딩하여 이러한 메서드를 사용하는 요청의 형식을 지정합니다.                                                                                                                                                  |
| `htmx.config.selfRequestsOnly`        | 기본값은 현재 문서와 동일한 도메인에 대한 AJAX 요청만 허용할지 여부를 나타내는 true입니다.                                                                                                                                                                                 |
| `htmx.config.ignoreTitle`             | defaults to `false`, `true`로 설정하면 htmx는 새 콘텐츠에서 `title` 태그가 발견될 때 문서 제목을 업데이트하지 않습니다.                                                                                                                                                   |
| `htmx.config.scrollIntoViewOnBoost`   | 부스트된 요소의 대상이 뷰포트로 스크롤되는지 여부에 관계없이 기본값이 true로 설정됩니다. 부스트된 요소에서 `hx-target`을 생략하면 대상은 기본적으로 `body`로 설정되어 페이지가 맨 위로 스크롤됩니다.                                                                                                                |
| `htmx.config.triggerSpecsCache`       | 기본값은 평가된 트리거 사양을 저장할 캐시인 `null`이므로 메모리 사용량을 늘리는 대신 구문 분석 성능을 향상시킬 수 있습니다. 절대 지워지지 않는 캐시를 사용하도록 간단한 객체를 정의하거나 [proxy object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Proxy)를 사용하여 자체 시스템을 구현할 수 있습니다. |
| `htmx.config.allowNestedOobSwaps`     | 기본 응답 요소 내에 중첩된 요소에 대해 OOB 교체를 처리할지 여부, 기본값은 `true`입니다. [Nested OOB Swaps](@/attributes/hx-swap-oob.md#nested-oob-swaps)를 참조하세요.                                                                                                        |

</div>

자바스크립트에서 직접 설정하거나 `meta` 태그를 사용할 수 있습니다:

```html
<meta name="htmx-config" content='{"defaultSwapStyle":"outerHTML"}'>
```
