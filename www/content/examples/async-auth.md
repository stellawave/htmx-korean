+++
title = "Async Authentication"
template = "demo.html"
+++

이 예시는 htmx를 사용하여 비동기 인증 토큰 흐름을 구현하는 방법을 보여줍니다.

여기서 사용할 기술은 [`htmx:confirm`](https://htmx.org/events/#htmx:confirm) 이벤트를 사용하여 요청을 지연시킬 수 있다는 점을 활용합니다.

우선, 인증 토큰이 검색될 때까지 요청을 보내지 않아야 하는 버튼을 생성합니다:

```html
  <button hx-post="/example" hx-target="next output">
    An htmx-Powered button
  </button>
  <output>
    --
  </output>
```

다음으로, `auth` 프로미스(라이브러리에서 반환된)를 처리하기 위한 스크립트를 추가합니다:

```html
<script>
  // auth는 인증 시스템에서 반환된 프로미스입니다.

  // 인증 토큰을 대기하고, 이를 어디에 저장합니다.
  let authToken = null;
  auth.then((token) => {
    authToken = token
  })
  
  // 인증 토큰에 따라 htmx 요청을 제어합니다.
  htmx.on("htmx:confirm", (e)=> {
    // 인증 토큰이 없을 경우
    if(authToken == null) {
      // 정규 요청이 발행되지 않도록 막습니다.
      e.preventDefault() 
      // 인증 프로미스가 해결된 후에만 요청을 발행합니다.
      auth.then(() => e.detail.issueRequest()) 
    }
  })

  // 요청에 인증 토큰을 헤더로 추가합니다.
  htmx.on("htmx:configRequest", (e)=> {
    e.detail.headers["AUTH"] = authToken
  })
</script>
```

여기에서는 글로벌 변수를 사용했지만, `localStorage`나 다른 선호하는 메커니즘을 사용하여 인증 토큰을 `htmx:configRequest` 이벤트에 전달할 수도 있습니다.

이 코드를 사용하면 `auth` 프로미스가 해결될 때까지 htmx가 요청을 발행하지 않습니다.
