+++
title = "How Did REST Come To Mean The Opposite of REST?"
date = 2022-07-18
updated = 2022-11-26
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

<style>
  pre {
        margin: 32px !important;
  }
</style>

## 표지판을 두드리기

<img src="/img/tap-the-sign.png" alt="You are wrong" style="width: 80%; margin-left:10%; margin-top: 16px;margin-bottom: 16px">

> HTTP 기반의 모든 인터페이스를 REST API라고 부르는 사람들이 너무 많아져서 좌절감을 느낍니다. 오늘의 예시는 SocialSite REST API입니다. 그것은 RPC입니다. 완전히 RPC로 보입니다. 너무 많은 결합이 드러나 있어서 X 등급을 받아야 할 정도입니다.
>
> REST 아키텍처 스타일에서 하이퍼텍스트가 제약이라는 개념을 명확히 하려면 무엇을 해야 할까요? 다시 말해, 애플리케이션 상태(따라서 API)의 엔진이 하이퍼텍스트에 의해 구동되지 않는다면 RESTful할 수 없으며, REST API라고 할 수 없습니다. 끝입니다. 어딘가 잘못된 매뉴얼이 있는 건가요?
>
> _--Roy Fielding, REST 용어 창시자_
>
> _&nbsp;&nbsp;&nbsp;[REST APIs must be hypertext-driven](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)_

[REST](https://en.wikipedia.org/wiki/Representational_state_transfer)는 컴퓨터 프로그래밍 역사상 가장 잘못 사용된 기술 용어일 것입니다.

비교할 만한 다른 용어가 떠오르지 않습니다.

오늘날 누군가 REST라는 용어를 사용할 때, 거의 항상 HTTP를 사용하는 JSON 기반 API를 언급하는 것입니다.

REST 또는 REST 가이드라인을 언급하는 구인 공고나 회사에서 [REST 가이드라인](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md)을 논의할 때, 
하이퍼텍스트나 하이퍼미디어는 거의 언급되지 않으며 대신 JSON, GraphQL(!) 등이 언급됩니다.

그러나 몇몇 고집스러운 사람들은 이렇게 불평합니다: 하지만 이 JSON API들은 RESTful하지 않아요!

<iframe src="https://www.youtube.com/embed/HOK6mE7sdvs" title="Doesn't anyone notice this?" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope" style="width: 400px;height:300px;margin-left:15%;margin-top: 16px;margin-bottom: 16px"></iframe>

이 게시물에서는 REST의 [짧고, 불완전하며, 대부분 잘못된](https://james-iry.blogspot.com/2009/05/brief-incomplete-and-mostly-wrong.html) 역사를 제공하고, 
REST의 의미가 어떻게 거의 완벽하게 뒤바뀌어 REST가 원래 대조하던 것, 즉 RPC를 의미하게 되었는지를 설명하고자 합니다.

## REST는 어디에서 왔을까?

REST라는 용어는 REpresentational State Transfer의 약자로, 
[Fielding의 박사 학위 논문](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) 5장에서 비롯되었습니다. 
Fielding은 (당시 새로 등장한) 월드 와이드 웹의 네트워크 아키텍처를 설명하며, 다른 네트워크 아키텍처, 특히 RPC 스타일의 네트워크 아키텍처와 대조했습니다.

그가 글을 썼던 시점(1999-2000년)에 JSON API는 존재하지 않았습니다. 그는 사람들이 "웹을 서핑"하며 HTML을 HTTP로 주고받던 당시의 웹을 설명하고 있었습니다. 
JSON은 아직 만들어지지 않았으며, JSON의 광범위한 채택은 10년 뒤의 일이었습니다.

REST는 _네트워크 아키텍처_를 설명했으며, RESTful API로 간주되기 위해 충족되어야 하는 API에 대한 _제약_으로 정의되었습니다. 
이 언어는 학문적이어서 혼란을 일으켰지만, 대부분의 개발자가 이해할 수 있을 만큼 충분히 명확합니다.

### REST의 핵심: 통일된 인터페이스 & HATEOAS

REST에는 많은 제약과 개념이 포함되어 있지만, REST를 다른 네트워크 아키텍처와 비교할 때 가장 구별되고 중요한 특징이 있다고 생각합니다.

이것은 [통일된 인터페이스 제약](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface)으로 알려져 있으며, 
그 개념 내에서 특히 [애플리케이션 상태의 엔진으로서의 하이퍼미디어(HATEOAS)](https://htmx.org/essays/hateoas/) 또는 Fielding이 선호하는 "하이퍼미디어 제약"이라고 불립니다.

이 통일된 인터페이스 제약을 이해하기 위해, 은행 계좌에 대한 정보를 반환하는 두 가지 HTTP 응답을 고려해 보겠습니다. 첫 번째는 HTML(하이퍼텍스트)로, 두 번째는 JSON입니다:

#### HTML 응답

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
            <a href="/accounts/12345/close-requests">해지 요청</a>
        </div>
    <body>
</html>
```

#### JSON 응답

```json
HTTP/1.1 200 OK

{
    "account_number": 12345,
    "balance": {
        "currency": "usd",
        "value": 100.00
     },
     "status": "good"
}
```

이 두 응답의 중요한 차이점, 그리고 _HTML 응답_이 RESTful이고 _JSON 응답_은 그렇지 않은 이유는 다음과 같습니다:

<p style="margin:32px;text-align: center;font-weight: bold">HTML 응답은 완전히 자체 설명적입니다.</p>

이 응답을 받은 적절한 하이퍼미디어 클라이언트는 은행 계좌가 무엇인지, 잔액이 무엇인지 등을 알지 못합니다. 그것은 단순히 하이퍼미디어, HTML을 렌더링하는 방법을 알고 있을 뿐입니다.

클라이언트는 이 데이터와 관련된 API 엔드포인트에 대해 HTML 자체 내에서 탐색 가능한 URL과 하이퍼미디어 컨트롤(링크와 폼) 외에는 아무것도 알지 못합니다. 
만약 리소스의 상태가 변경되어 해당 리소스에서 사용할 수 있는 작업이 변경된다면(예: 계좌가 마이너스가 되는 경우), HTML 응답은 사용 가능한 새 작업 세트를 표시하도록 변경됩니다.

클라이언트는 "마이너스"가 무엇인지, 또는 은행 계좌가 무엇인지조차 알지 못한 채 이 새로운 HTML을 렌더링합니다.

이와 같이 하이퍼텍스트는 애플리케이션 상태의 엔진 역할을 합니다: HTML 응답은 시스템과 상호 작용을 계속하기 위해 필요한 모든 API 정보를 자체 내에 담고 있습니다.

이제 두 번째 JSON 응답과 비교해 보겠습니다.

이 경우 메시지는 _자체 설명적이지 않습니다_. 오히려 클라이언트는 적절한 사용자 인터페이스를 표시하기 위해 `status` 필드를 해석하는 방법을 알아야 합니다. 
또한, 클라이언트는 JSON 응답 외부의 다른 정보 소스(예: Swagger API 문서)에서 파생된 "대역 외" 정보, 즉 URL, 매개변수 등을 알아야 계좌에서 사용할 수 있는 작업을 알 수 있습니다.

JSON 응답은 자체 설명적이지 않으며 하이퍼미디어 내에 리소스의 상태를 인코딩하지 않으므로 REST의 통일된 인터페이스 제약을 충족하지 못하며, 따라서 RESTful하지 않습니다.

### 창시자: RESTful API는 하이퍼미디어 구동이어야 한다

[REST API는 하이퍼미디어 구동이어야 한다](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)에서 Fielding은 다음과 같이 말합니다:

> REST API는 초기 URI(북마크)와 예상되는 청중에게 적합한 표준화된 미디어 유형의 집합을 제외하고는 사전 지식 없이 들어가야 합니다. 그 시점부터 모든 애플리케이션 상태 전환은 수신된 표현에 존재하거나 사용자가 해당 표현을 조작하여 암시하는 서버 제공 선택에 의해 구동되어야 합니다.

따라서 RESTful 시스템에서는 단일 URL을 통해 시스템에 진입할 수 있어야 하며, 그 시점부터 시스템 내에서 수행되는 모든 탐색 및 작업은 자체 설명적인 하이퍼미디어를 통해 제공되어야 합니다: 
예를 들어 HTML의 링크와 폼을 통해. 적절한 RESTful 시스템에서는 진입점 이후 API에 대한 추가 정보가 필요하지 않아야 합니다.

이것이 RESTful 시스템의 놀라운 유연성의 원천입니다: 모든 응답이 자체 설명적이고 현재 사용할 수 있는 모든 작업을 인코딩하기 때문에 예를 들어 API 버전 관리에 대해 걱정할 필요가 없습니다!
실제로, API를 문서화할 필요조차 없습니다!

상황이 바뀌면 하이퍼미디어 응답이 변경되며, 그게 전부입니다.

분산 시스템을 구축하는 데 있어 매우 유연하고 혁신적인 개념입니다.

### 업계: 농담이 아니라, RESTful API는 JSON이다

오늘날 대부분의 웹 개발자와 대부분의 회사는 _두 번째 예제_를 RESTful API라고 부를 것입니다.

그들은 아마도 첫 번째 응답을 _API 응답_으로 간주하지 않을 것입니다. 그저 HTML일 뿐이죠!

(불쌍한 HTML, 존중받을 수 없군요.)

API는 항상 JSON이거나, 아니면 좀 더 고급스러운 경우 Protobuf 같은 것이죠, 맞나요?

<img src="/img/you-are-wrong.png" alt="You are wrong" style="width: 80%;margin-left:10%; margin-top: 16px;margin-bottom: 16px">

틀렸습니다.

여러분 모두 틀렸고, 부끄러워해야 합니다.

첫 번째 응답은 _API 응답_이며, 실제로 RESTful한 응답입니다!

두 번째 응답은 실제로 _원격 프로시저 호출_(RPC) 스타일의 API입니다. 클라이언트와 서버는 결합되어 있으며, 이는 Fielding이 2008년에 불평했던 SocialSite API와 마찬가지입니다: 
클라이언트는 JSON 응답 자체 외부의 다른 소스에서 파생된 리소스와 관련된 추가 정보를 알아야 합니다.

이 API는 정신적으로 REST의 거의 반대에 해당합니다.

이 스타일의 API를 "RESTless"라고 부르겠습니다.

## "REST"가 "RESTless"로 바뀐 과정

그렇다면, 어떻게 업계의 99.9%가 RESTful하지 않은 API들을 RESTful하다고 부르게 된 걸까요?

흥미로운 이야기입니다:

Roy Fielding은 2000년에 그의 논문을 발표했습니다.

비슷한 시기에, [XML-RPC](https://en.wikipedia.org/wiki/XML-RPC)라는 명백히 RPC에서 영감을 받은 프로토콜이 등장했으며, HTTP를 사용하여 API를 구축하는 방법으로 주목받기 시작했습니다. 
XML-RPC는 Microsoft의 더 큰 프로젝트인 [SOAP](https://en.wikipedia.org/wiki/SOAP)의 일부였습니다. 
XML-RPC는 주로 엔터프라이즈 세계에서 나온 여러 RPC 스타일 프로토콜의 전통을 이어받았으며, 많은 정적 타이핑과 초기 XML 극대화의 요소들이 포함되었습니다.

또한 이 시점에 등장한 것이 [AJAX](https://en.wikipedia.org/wiki/Ajax_(programming))였습니다. 비동기 자바스크립트와 XML이죠. 
여기서 중요한 것은 XML입니다. 이제 모두가 알고 있는 것처럼, AJAX는 브라우저가 백그라운드에서 서버로 HTTP 요청을 보내고, 
그 응답을 자바스크립트로 직접 처리할 수 있게 하여 웹 프로그래밍의 새로운 세계를 열었습니다.

문제는: 이러한 요청은 어떤 형식을 가져야 할까요? 당연히 XML이 될 것이었습니다. 이름에도 나와 있잖아요. 그리고 새로운 SOAP/XML-RPC 표준이 나왔으니, 그게 적합한 것일까요?

### 웹 서비스에 REST가 적용될 수 있을까?

일부 사람들은 Fielding이 설명한 웹의 다른 아키텍처를 인지하고, SOAP 대신 REST가 "웹 서비스"로 불리기 시작한 것에 접근하는 선호 메커니즘이 되어야 하는지 묻기 시작했습니다. 
웹은 매우 유연하고 빠르게 성장하는 것으로 입증되고 있었기 때문에, 브라우저와 인간에게 잘 작동했던 동일한 네트워크 아키텍처, 즉 REST가 API에도 잘 맞을 수 있을 것이라고 생각되었습니다.

이는 그럴듯하게 들렸습니다, 특히 XML이 API의 형식이었을 때 더욱 그랬습니다: XML은 확실히 _HTML과_ 매우 비슷하게 생겼습니다, 그렇죠? 
XML API가 RESTful 제약 조건, 특히 통일된 인터페이스를 포함한 모든 것을 충족할 수 있다고 상상할 수 있습니다.

그래서 사람들은 이 경로를 탐색하기 시작했습니다.

이 모든 일이 벌어지는 동안, 또 다른 중요한 기술이 탄생 중이었습니다: [JSON](https://www.json.org/json-en.html)

JSON은 SOAP/RPC-XML의 자바와 비교할 때 문자 그대로 자바스크립트와 같았습니다: 단순하고, 동적이며, 쉬웠습니다. 
지금은 JSON이 대부분의 웹 API에서 지배적인 형식이 된 것이 믿기 어려울 정도로, 실제로 JSON이 유행하는 데는 시간이 걸렸습니다. 
2008년까지도 API 개발에 대한 논의는 주로 XML에 관한 것이었고, JSON에 대한 것은 아니었습니다.

### REST API의 공식화

2008년에 Martin Fowler는 [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)을 대중화한 기사를 발표했으며, 
이 모델은 주어진 API가 얼마나 RESTful한지 판단하는 데 사용되었습니다.

이 모델은 네 가지 "레벨"을 제안했으며, 첫 번째 레벨은 "Plain Old XML" 또는 "The Swamp of POX"로 불렸습니다.

<img src="/img/rmm.png" alt="Richardson Maturity Model" style="width: 80%;margin-left:10%; margin-top: 16px;margin-bottom: 16px">

그 후, API는 다음 아이디어들을 채택함에 따라 더 "성숙한" REST API로 간주될 수 있었습니다:

* 레벨 1: 리소스 (예: 리소스 인식 URL 레이아웃, XML-RPC에서 사용되는 불투명한 URL 레이아웃과 대조됨)
* 레벨 2: HTTP 동사 (`GET`, `POST`, `DELETE` 등을 올바르게 사용)
* 레벨 3: 하이퍼미디어 컨트롤 (예: 링크)

레벨 3은 통일된 인터페이스가 들어가는 곳으로, 이 레벨이 가장 성숙하고 진정한 "REST의 영광"으로 간주됩니다.

### "REST"의 승리, 어느 정도는...

불행히도 REST라는 용어에 있어 두 가지 일이 일어났습니다:

* 모두가 JSON으로 전환했습니다.
* 모두가 RMM의 레벨 2에서 멈췄습니다.

JSON은 SOAP/XML-RPC가 너무 과도하게 설계된 반면, JSON은 단순하고 "그냥 작동"하며 읽고 이해하기 쉬웠기 때문에 웹 서비스/API 세계에서 빠르게 자리를 잡았습니다.

이 변화와 함께 웹 개발 세계는 [J2EE 마인드셋](https://en.wikipedia.org/wiki/Jakarta_EE)의 족쇄를 확실히 떨쳐내고, SOAP/XML-RPC를 엔터프라이즈 전용으로 한정시켰습니다.

REST 접근법은 SOAP/XML-RPC처럼 XML에 너무 얽매이지 않았으며, 엔드포인트에 대한 형식성을 덜 요구했기 때문에, REST는 JSON이 주도권을 잡기 위한 자연스러운 장소가 되었습니다. 
그리고 그것은 빠르게 이루어졌습니다.

이 중요한 변화 동안, 대부분의 JSON API가 RMM의 레벨 2에서 멈춘다는 것이 점점 더 분명해졌습니다.

일부는 응답에 하이퍼미디어 컨트롤을 포함시켜 레벨 3에 도달하려 했지만, 거의 모든 API가 여전히 문서를 게시해야 한다는 사실은 "REST의 영광"이 달성되지 않았음을 시사했습니다.

응답 형식으로 JSON을 사용하는 것은 또 다른 강력한 힌트가 되어야 했습니다: JSON은 분명히 하이퍼텍스트가 아닙니다. 
하이퍼미디어 컨트롤을 그 위에 적용할 수 있지만, 이는 자연스럽지 않습니다. XML은 적어도 _어떻게든_ HTML처럼 보였기 때문에 하이퍼미디어를 만들 수 있다는 가능성이 있었습니다.

JSON은 단지... 데이터일 뿐이었습니다. 하이퍼미디어 컨트롤을 추가하는 것은 어색하고, 표준화되지 않았으며, 통일된 인터페이스 제약에서 설명된 방식으로 거의 사용되지 않았습니다.

이러한 어려움에도 불구하고 REST라는 용어는 붙어 있었습니다: REST는 SOAP의 반대였으며, JSON API는 SOAP이 아니었습니다. 따라서 JSON API는 REST였습니다.

이것이 우리가 현재에 이르게 된 한 문장으로 설명할 수 있는 과정입니다.

### REST 전쟁

JSON API 세계가 진정으로 RESTful한 API를 일관되게 달성하지 못했음에도 불구하고, 생성된 RESTless API가 "RESTful"인지 여부에 대한 많은 논쟁이 있었습니다. 
URL 레이아웃에 대한 논쟁, 주어진 작업에 적절한 HTTP 동사에 대한 논쟁, 미디어 타입에 대한 불꽃 튀는 논쟁 등이 있었습니다.

저는 당시 어렸고, 전체 상황이 불투명하고, 순수주의적이며, 멀게 느껴졌기 때문에 REST라는 개념에 대해 거의 포기했습니다. 그것은 인터넷에서 사람들이 싸우는 주제처럼 느껴졌습니다.

제가 거의 접하지 못했던 (혹은 접했어도 이해하지 못했던) 것은 통일된 인터페이스의 개념과 그것이 RESTful 시스템에서 얼마나 중요한지에 대한 것이었습니다.

그러다가 제가 [intercooler.js](https://intercoolerjs.org)를 만들고, 몇몇 똑똑한 사람들이 그것이 RESTful하다고 말하기 시작했을 때 다시 이 개념에 관심을 갖게 되었습니다.

RESTful? 그건 JSON API에 대한 이야기인데, 어떻게 내 작은 프론트엔드 라이브러리가 RESTful할 수 있을까?

그래서 저는 이 문제를 다시 조사하고, Fielding의 논문을 새롭게 읽어보았습니다. 
그리고 놀랍게도 intercooler가 RESTful일 뿐만 아니라, 제가 다루고 있던 "RESTful" JSON API들이 전혀 RESTful하지 않다는 사실을 발견했습니다!

그리고 나서 저는 인터넷을 지루하게 만들기 시작했습니다:

* [API 겨울에서 REST 구출하기](https://intercoolerjs.org/2016/01/18/rescuing-rest.html)
* [API 변동성/보안 절충안](https://intercoolerjs.org/2016/02/17/api-churn-vs-security.html)
* [HATEOAS는 인간을 위한 것](https://intercoolerjs.org/2016/05/08/hatoeas-is-for-humans.html)
* [HTML을 진지하게 받아들이기](https://intercoolerjs.org/2020/01/14/taking-html-seriously)
* [Hypermedia API 대 데이터 API](@/essays/hypermedia-apis-vs-data-apis.md)
* [HATEOAS](@/essays/hateoas.md)
* [Hypermedia Driven Applications](@/essays/hypermedia-driven-applications.md)
* 그리고 지금 당신이 읽고 있는 이 글 [당신의 현재 기사](@/essays/how-did-rest-come-to-mean-the-opposite-of-rest.md).

### 오늘날의 REST 상태

결국 대부분의 사람들은 JSON API에 하이퍼미디어 컨트롤을 추가하려는 시도를 포기했습니다. 
이러한 컨트롤들이 특정 전문 상황에서는 잘 작동했지만 (예: 페이징), 일반적인 인간 지향 인터넷에서 REST가 찾은 넓고 명백한 유용성을 달성하지 못했습니다. 
[(그 이유에 대한 제 이론이 있습니다.)](https://intercoolerjs.org/2016/05/08/hatoeas-is-for-humans.html)

이렇게 해서 REST는 RESTless 상태로 굳어졌고, REST라는 의미는 RMM의 레벨 1 또는 2에 있는 JSON API로 천천히 자리잡았습니다. 
그러나 우리는 항상 레벨 3에 도달하여 REST의 영광을 누릴 가능성이 있었습니다.

그러나 Single Page Applications(SPA)가 등장했습니다.

SPA가 등장하면서 웹 개발은 원래의 RESTful 아키텍처와 완전히 단절되었습니다. 
SPA 애플리케이션의 *전체 네트워킹 아키텍처*는 JSON RPC 스타일로 이동했습니다. 게다가 이러한 애플리케이션의 복잡성 때문에 개발자들은 프론트엔드와 백엔드로 전문화되었습니다.

프론트엔드 개발자들은 분명히 RESTful한 작업을 전혀 하지 않았습니다. 그들은 자바스크립트를 사용하여 DOM 객체를 생성하고, 필요할 때 AJAX API를 호출하며 작업을 진행했습니다. 
이는 초기 웹보다는 두꺼운 클라이언트 작성과 더 비슷했습니다.

백엔드 엔지니어들은 어느 정도 네트워크 아키텍처에 대해 여전히 신경을 썼으며, 그들이 하고 있는 작업을 설명하기 위해 "REST"라는 용어를 계속 사용했습니다.

비록 그들이 RESTful API를 위한 swagger 문서를 게시하거나 [RESTful API의 API 변동성에 대해 불평](https://www.infoq.com/articles/no-more-mvc-frameworks/)하더라도 말이죠. 
실제로 RESTful API를 생성하고 있다면 발생하지 않았을 일들입니다.

결국 2010년대 후반에 이르러 사람들은 참을 만큼 참았습니다: REST, 심지어 RESTless 상태에서도, 점점 복잡해지는 SPA 애플리케이션의 요구를 따라가지 못했습니다. 
애플리케이션은 점점 더 두꺼운 클라이언트처럼 변했고, 두꺼운 클라이언트 문제는 두꺼운 클라이언트 솔루션을 필요로 했으며, 하이퍼미디어 클라이언트 솔루션이 아니라는 것이 명백해졌습니다.

결국 [GraphQL](https://en.wikipedia.org/wiki/GraphQL)이 출시되면서 상황은 확실히 무너졌습니다.

GraphQL은 RESTful과는 거리가 멀었습니다: GraphQL을 사용하는 API와 작업하려면 반드시 문서가 필요합니다. 
클라이언트와 서버는 매우 긴밀하게 결합되어 있습니다. GraphQL에는 네이티브 하이퍼미디어 컨트롤이 없습니다. 
GraphQL은 스키마를 제공하며, 여러 면에서 업데이트된 간단한 XML-RPC 버전처럼 느껴집니다.

여기서 저는 이렇게 말하고 싶습니다: 그것은 괜찮습니다!

많은 경우 사람들은 GraphQL을 정말로 좋아합니다. 두꺼운 클라이언트 스타일 애플리케이션을 구축한다면, 그것이 아주 합리적입니다:

> 이 질문에 대한 간단한 대답은 HATEOAS가 대부분의 현대 API 사용 사례에 적합하지 않다는 것입니다. 그래서 거의 20년이 지난 지금도 HATEOAS는 개발자들 사이에서 널리 채택되지 못했습니다. 반면 GraphQL은 실질적인 문제를 해결하기 때문에 광범위하게 확산되고 있습니다.
>
> _[GraphQL과 REST 레벨 3 (HATEOAS)](https://techblog.commercetools.com/graphql-and-rest-level-3-hateoas-70904ff1f9cf)_

따라서 GraphQL은 REST가 아니며, REST가 되고자 하지도 않고, REST가 되려고 하지도 않습니다.

하지만 오늘날 대다수의 개발자와 회사들은 여전히 GraphQL 기능을 API에 열정적으로 추가하면서도, 그들이 구축하는 것이 REST라고 계속 부르고 있습니다.

## 이 상황에 대해 우리가 할 수 있는 것은 무엇일까요?

불행히도, [voidfunc](https://news.ycombinator.com/item?id=32073545)의 말이 맞을 가능성이 큽니다:

> 아무리 표지판을 두드려도 소용없습니다. 그 전쟁은 오래 전에 졌습니다. REST는 사람들이 HTTP+JSON RPC를 위해 사용하는 일반적인 용어일 뿐입니다.

우리는 RESTful하지 않은 JSON API를 계속 REST라고 부를 것입니다. 그것이 이제는 모두가 부르는 명칭이니까요.

제가 점점 더 열심히 표지판을 두드리더라도, 50년 후에도 Global Omni Corp.는 여전히 그들의 RESTful JSON API의 swagger 문서 작업을 위한 일자리를 광고하고 있을 것입니다.

<img src="/img/punished-fielding.png" alt="Roy Fielding은 승인하지 않습니다" style="width: 80%;margin-left:10%; margin-top: 16px;margin-bottom: 16px">

[상황은 절망적이지만, 심각하지는 않습니다.](https://wwnorton.com/books/9780393310214)

그럼에도 불구하고, REST를 설명할 기회는 여전히 있으며, 특히 통일된 인터페이스 개념을 원래의 맥락에서 들어본 적이 없고, 
REST가 JSON API와 같다고 가정하는 새로운 세대의 웹 개발자들에게 설명할 기회가 있습니다.

[사람들은 무언가 잘못되었다고 느낍니다](@/essays/a-response-to-rich-harris.md), 그리고 아마도 REST, 진정한 실제 REST, RESTless가 아닌 REST가 
[그에 대한 답변의 일부가 될 수 있을 것입니다](@/essays/spa-alternative.md).

최소한 REST의 아이디어는 흥미롭고, 일반적인 소프트웨어 엔지니어링 지식으로서 알 가치가 있습니다.

여기에는 더 큰 메타포인트도 있습니다: 상대적으로 똑똑한 사람들(초기 웹 개발자들), 인터넷의 혜택을 누리며, 
REST라는 용어에 대한 꽤 명확한(때때로 학문적인) 사양을 가지고 있는 이들도 20년 동안 그 의미를 원래의 의미와 일치시킬 수 없었다는 점입니다.

우리가 이토록 명백하게 잘못될 수 있다면, 다른 무엇을 잘못 생각하고 있을 수 있을까요?
