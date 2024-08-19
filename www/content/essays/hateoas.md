+++
title = "HATEOAS"
date = 2021-10-16
updated = 2022-02-06
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
[extra]
show_title = false
show_author = false
+++

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Lexend+Zetta:wght@900&display=swap&text=HATEOAS" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Lexend+Zetta:wght@900&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Source+Serif+Pro:ital,wght@0,400;0,600;0,700;1,400;1,700&display=swap" rel="stylesheet">

<h1>HATEOAS</h1>

<section>

## 서문: _HATEOAS &mdash; 대체 설명_

이 페이지는 [HATEOAS의 위키피디아 항목](https://en.wikipedia.org/wiki/HATEOAS)을 HTML을 사용해 재구성한 것입니다. 
이 설명은 JSON API와 비교하면서 개념을 설명하며, 위키피디아에 적합한 중립적인 설명보다는 다소 의견이 들어간 설명이지만, 우리의 관점에서는 더 정확하다고 생각합니다.

</section>
---

애플리케이션 상태의 엔진으로서의 하이퍼미디어(Hypermedia as the Engine of Application State, HATEOAS)는 
[REST 애플리케이션 아키텍처](https://en.wikipedia.org/wiki/Representational_state_transfer)의 제약 중 하나로, 이를 다른 네트워크 애플리케이션 아키텍처와 구분 짓습니다.

HATEOAS에서는 클라이언트가 애플리케이션 서버가 동적으로 제공하는 [*하이퍼미디어*](https://en.wikipedia.org/wiki/Hypermedia)를 통해 네트워크 애플리케이션과 상호작용합니다. 
REST 클라이언트는 하이퍼미디어에 대한 일반적인 이해 이외에는 애플리케이션이나 서버와 상호작용하는 방법에 대해 거의 또는 전혀 사전 지식이 필요하지 않습니다.

반면에 오늘날의 JSON 기반 웹 클라이언트는 주로 [스웨거(swagger)](https://swagger.io/)와 같은 도구를 통해 문서화된 고정된 인터페이스를 통해 상호작용합니다.

HATEOAS가 부과하는 제약은 클라이언트와 서버를 분리시켜 줍니다. 이는 서버 기능이 독립적으로 발전할 수 있게 해줍니다.

## 예시

HTTP를 구현한 사용자 에이전트가 간단한 URL을 통해 REST 엔드포인트에 HTTP 요청을 보냅니다. 
사용자 에이전트가 수행할 수 있는 모든 후속 요청은 각 요청에 대한 하이퍼미디어 응답에서 발견됩니다. 
이러한 표현에 사용되는 미디어 타입과 그들이 포함할 수 있는 링크 관계는 표준화되어 있습니다. 
클라이언트는 하이퍼미디어 표현 내의 링크를 선택하거나 해당 미디어 타입이 제공하는 다른 방법으로 표현을 조작함으로써 애플리케이션 상태를 전환합니다.

이렇게 RESTful 상호작용은 외부 정보가 아닌 하이퍼미디어에 의해 구동됩니다.

구체적인 예를 통해 이를 명확히 해보겠습니다. 웹 브라우저가 은행 계좌 리소스를 가져오는 GET 요청을 발행한다고 가정해 봅시다:

```txt
GET /accounts/12345 HTTP/1.1
Host: bank.example.com
```

서버는 HTML을 사용한 하이퍼미디어 표현으로 응답합니다:

```html
HTTP/1.1 200 OK

<html>
  <body>
    <div>계좌 번호: 12345</div>
    <div>잔액: $100.00 USD</div>
    <div>링크:
        <a href="/accounts/12345/deposits">입금</a>
        <a href="/accounts/12345/withdrawals">출금</a>
        <a href="/accounts/12345/transfers">이체</a>
        <a href="/accounts/12345/close-requests">계좌 폐쇄 요청</a>
    </div>
  <body>
</html>
```

이 응답에는 다음과 같은 가능한 후속 작업이 포함되어 있습니다: 입금, 출금, 이체를 위해 UI로 이동하거나 계좌 폐쇄 요청을 할 수 있습니다.

나중에 계좌가 초과 인출된 후의 상황을 생각해 봅시다. 이제 이 계좌 상태 변화로 인해 사용 가능한 링크 세트가 달라집니다.

```html
HTTP/1.1 200 OK

<html>
  <body>
    <div>계좌 번호: 12345</div>
    <div>잔액: -$50.00 USD</div>
    <div>링크:
        <a href="/accounts/12345/deposits">입금</a>
    </div>
  <body>
</html>
```

이제 사용 가능한 링크는 하나뿐입니다: 더 많은 돈을 입금하는 링크입니다. 
현재 초과 인출 상태의 계좌에서는 다른 작업을 수행할 수 없으며, 이 사실은 *하이퍼미디어*에 내포되어 있습니다. 
웹 브라우저는 초과 인출 계좌의 개념이나 계좌가 무엇인지조차 알지 못합니다. 단지 하이퍼미디어 표현을 사용자에게 표시하는 방법만 알고 있을 뿐입니다.

따라서 우리는 애플리케이션 상태의 엔진으로서의 하이퍼미디어라는 개념을 갖게 됩니다. 리소스의 상태가 변함에 따라 가능한 작업이 달라지며, 이 정보는 하이퍼미디어에 인코딩됩니다.

위의 HTML 응답과 일반적인 JSON API를 비교해보면, JSON API는 대신 계좌의 상태 필드를 포함하는 표현을 반환할 것입니다:

```json
HTTP/1.1 200 OK

{
    "account": {
        "account_number": 12345,
        "balance": {
            "currency": "usd",
            "value": -50.00
        },
        "status": "overdrawn"
    }
}
```

여기서 클라이언트는 `status` 필드의 값이 무엇을 의미하는지, 이것이 사용자 인터페이스의 렌더링에 어떤 영향을 미칠지에 대해 명확히 알고 있어야 합니다. 
클라이언트는 또한 응답에 인코딩되지 않은 리소스를 조작하기 위한 URL도 알고 있어야 합니다. 이는 일반적으로 JSON API에 대한 문서를 참조하여 이루어집니다.

이렇게 외부 정보가 필요한 점이 이 JSON API를 HATEOAS를 구현하는 RESTful API와 구분 짓는 요소입니다.

이 예시는 두 가지 접근 방식의 핵심적인 차이를 보여줍니다: RESTful, HATEOAS HTML 표현에서는 모든 작업이 응답에 직접 인코딩됩니다. 
반면 JSON API 예시에서는 원격 리소스를 처리하고 작업하는 데 외부 정보가 필요합니다.

## 기원

HATEOAS 제약은 Roy Fielding의 [박사 논문](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)에 정의된 REST의 
["통일된 인터페이스"](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface) 기능의 필수적인 부분입니다. 
Fielding의 논문은 주로 HTML과 HTTP로 구성된 초기 웹 아키텍처에 대한 논의였습니다.

Fielding은 자신의 [블로그](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)에서 하이퍼미디어의 개념과 그 중요한 요구 사항에 대해 더 자세히 설명했습니다.

## HATEOAS와 JSON

*참고: 이 섹션의 중립적인 어조는 논쟁의 여지가 있습니다.*

2000년대 초반에 REST 개념은 초기 웹의 설명으로서의 개념적 환경에서 벗어나 XML API 개발(주로 [SOAP](https://en.wikipedia.org/wiki/SOAP)을 사용) 및 
JSON API 개발과 같은 다른 웹 개발 분야로 도입되었습니다. 이는 XML이나 JSON이 HTML과 같은 자연스러운 하이퍼미디어가 아님에도 불구하고 이루어졌습니다.

이 새로운 영역에서 REST에 대한 준수 수준을 특징짓기 위해 [리처드슨 성숙도 모델](https://en.wikipedia.org/wiki/Richardson_Maturity_Model)이 제안되었습니다. 
이 모델의 가장 높은 수준(Level 3)은 "하이퍼미디어 컨트롤"로 구성됩니다.

JSON은 자연스러운 하이퍼미디어가 아니므로, 하이퍼미디어 개념은 JSON 위에 강제로 추가될 수밖에 없습니다. 
리처드슨 성숙도 모델의 Level 3을 충족하려는 JSON 엔지니어는 위의 은행 계좌 예시에 해당하는 다음 JSON을 반환할 수 있습니다:

```json
HTTP/1.1 200 OK

{
    "account": {
        "account_number": 12345,
        "balance": {
            "currency": "usd",
            "value": 100.00
        },
        "links": {
            "deposits": "/accounts/12345/deposits",
            "withdrawals": "/accounts/12345/withdrawals",
            "transfers": "/accounts/12345/transfers",
            "close-requests": "/accounts/12345/close-requests"
        }
    }
}
```

여기에서 "하이퍼미디어 컨트롤"은 계정 객체의 `links` 속성에 인코딩되어 있습니다.

하지만 이 API의 클라이언트는 여전히 상당한 추가 정보를 알아야 합니다:

* 이러한 URL에 대해 어떤 HTTP 메서드를 사용할 수 있는가?
* 이러한 URL에 `GET` 요청을 보내서 해당 변경 사항의 표현을 받을 수 있는가?
* 특정 URL에 `POST` 요청을 할 수 있다면, 어떤 값들이 기대되는가?

위의 JSON을 첫 번째 HTML 예시에 있는 `/accounts/12345/deposits` 링크를 클릭한 후 브라우저에서 가져온 다음 HTTP 응답과 비교해 보세요:

```html
HTTP/1.1 200 OK

<html>
  <body>
    <form method="post" action="/accounts/12345/deposits">
        <input name="amount" type="number" />
        <button>Submit</button>
    </form>
  <body>
</html>
```

이 HTML 응답은 계좌 잔액을 업데이트하는 데 필요한 모든 정보를 인코딩하고 있으며, `form` 요소에 `method`와 `action` 속성이 포함되어 있으며, 
리소스를 올바르게 업데이트하는 데 필요한 입력 필드를 제공합니다.

JSON 표현에는 HTML 표현에서와 같은 자체 포함된 "통일된 인터페이스"가 없습니다.

JSON API가 RESTful 개념에서 얼마나 멀리 벗어나더라도, 이를 'REST'라고 부르는 것에 대해 Roy Fielding은 다음과 같이 말했습니다:

> HTTP 기반의 모든 인터페이스를 REST API라고 부르는 사람들이 너무 많아지면서 나는 점점 좌절하고 있습니다. 오늘의 예시는 SocialSite REST API입니다. 그것은 RPC입니다. 완전히 RPC로 보입니다. 너무 많은 결합이 드러나 있어서 X 등급을 받아야 할 정도입니다.

JSON API에 더 복잡한 하이퍼미디어 컨트롤을 도입하려는 시도가 있었지만, 전반적으로 업계는 HATEOAS와 RESTful 아키텍처의 다른 요소들을 포기하고 더 간단한 RPC 스타일의 API를 선호했습니다.

이 사실은 HTML과 같은 자연스러운 하이퍼미디어가 RESTful 시스템을 구축하는 데 실질적으로 필수적이라는 주장을 강력히 뒷받침합니다.


<style>
  .content {
    font-family: 'Source Serif Pro', serif;
    text-align: justify;
    hyphens: auto;
    margin-bottom: 3em;
  }

  .content h1 {
    font-family: 'Lexend Zetta', Haettenschweiler, Impact, sans-serif;
    margin: 16px;
    font-size: min(10vw, 6em);
    line-height: 1em;
    margin-bottom: 5rem;
    text-align: center;
  }

  .content section:after {
    content: '< / >';
    content: '< / >' / '';
    display: block;
    margin-bottom: 32px;
    text-align: center;
    color: #aaa;
    font-weight: bold;
    letter-spacing: .5em;
  }

  .content h2 {
    font-size: 1em;
    margin: 16px;
    margin-top: 32px;
    text-transform: uppercase;
    letter-spacing: .1em;
    text-align: center;
  }
    .content h2 em {
      text-transform: none;
      letter-spacing: 0;
    }

  .content a {
    font-variant: all-small-caps;
    letter-spacing: .08em;
    font-weight: 600;
  }

  .content blockquote {
    border: none;
    font-style: italic;
    font-size: 1.1em;
  }
</style>
