+++
title = "HX-Trigger Response Headers"
+++

이 응답 헤더는 htmx의 응답 내에서 대상 요소에 클라이언트 측 동작을 트리거하는 데 사용할 수 있습니다. 단일 이벤트를 트리거하거나 원하는 만큼 고유한 이름의 이벤트를 트리거할 수 있습니다.

헤더는 다음과 같습니다:

* `HX-Trigger` - 응답을 받자마자 이벤트를 트리거합니다.
* `HX-Trigger-After-Settle` - [정리 단계](https://htmx.org/docs/#request-operations) 후에 이벤트를 트리거합니다.
* `HX-Trigger-After-Swap` - [교체 단계](https://htmx.org/docs/#request-operations) 후에 이벤트를 트리거합니다.

추가 세부 정보 없이 단일 이벤트를 트리거하려면 헤더에 이벤트 이름을 다음과 같이 간단히 보낼 수 있습니다:

`HX-Trigger: myEvent`

이것은 triggering 요소에서 `myEvent`를 트리거하고 `body`로 버블링됩니다. 예를 들어, 다음과 같이 이 이벤트를 들을 수 있습니다:

```javascript
document.body.addEventListener("myEvent", function(evt){
    alert("myEvent was triggered!");
})
```

또는 JS 코드를 사용하지 않고 일부 요소를 트리거하려는 경우 다음과 같이 할 수 있습니다:

```html
<!-- 이벤트가 <body>로 버블링되므로, 아래의 `from:body` 수정자를 사용해야 합니다 -->
<div hx-trigger="myEvent from:body" hx-get="/example"></div>
```

이벤트와 함께 세부 정보를 전달하려면 트리거의 값을 JSON으로 변경할 수 있습니다:

`HX-Trigger: {"showMessage":"Here Is A Message"}`

이 이벤트를 처리하려면 다음 코드를 작성합니다:

```javascript
document.body.addEventListener("showMessage", function(evt){
    alert(evt.detail.value);
})
```

메시지의 값이 `detail.value` 슬롯에 들어간 것을 주목하십시오. 여러 개의 데이터를 전달하려면 JSON 객체의 오른쪽에 중첩된 JSON 객체를 사용할 수 있습니다:

`HX-Trigger: {"showMessage":{"level" : "info", "message" : "Here Is A Message"}}`

이 이벤트를 다음과 같이 처리합니다:

```javascript
document.body.addEventListener("showMessage", function(evt){
   if(evt.detail.level === "info"){
     alert(evt.detail.message);   
   }
})
```

오른쪽의 JSON 객체의 각 속성은 이벤트의 세부 정보 객체에 복사됩니다.

### Multiple Triggers

여러 이벤트를 호출하려면 최상위 JSON 객체에 추가 속성을 간단히 추가할 수 있습니다:

`HX-Trigger: {"event1":"A message", "event2":"Another message"}`

추가 세부 정보 없이 여러 이벤트를 트리거하려면 이벤트 이름을 쉼표로 구분하여 보낼 수도 있습니다:

`HX-Trigger: event1, event2`

이벤트를 사용하면 일반 htmx 응답에 기능을 추가할 수 있는 많은 유연성을 제공합니다.
