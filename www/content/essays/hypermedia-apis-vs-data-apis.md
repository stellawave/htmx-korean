+++
title = "Hypermedia APIs vs. Data APIs"
date = 2021-07-17
updated = 2022-04-07
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

## 하이퍼미디어 API

**하이퍼미디어** API는 [하이퍼미디어](https://en.wikipedia.org/wiki/Hypermedia)를 반환하는 API로, 일반적으로 HTML을 HTTP를 통해 전송합니다. 이러한 스타일의 API는 하이퍼미디어를 반환하지 않는 데이터 API와 구별됩니다. 오늘날 이 후자의 스타일은 일반적으로 JSON API로 알려져 있습니다.

이 두 가지 유형의 API는 각각 고유한 설계 요구 사항을 가지고 있으므로, 서로 다른 설계 제약 조건과 목표를 채택해야 합니다.

### 하이퍼미디어 API:

* [REST-ful](https://en.wikipedia.org/wiki/Representational_state_transfer) 방식으로 간단히 구현될 수 있으며, 이는 [Roy Fielding이 설명한 것](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)과 같습니다.
* 기본 하이퍼미디어 애플리케이션의 요구 사항에 의해 구동되어야 합니다.
* [자기 설명적 메시지](https://en.wikipedia.org/wiki/Representational_state_transfer#Uniform_interface)를 사용하기 때문에 버전 정보를 포함하지 않고도 크게 변경될 수 있습니다.
* 시스템의 유연성을 최대화하기 위해 직접 인간에게 전달되어야 합니다.

### 데이터 API:

* [Richardson Maturity Model](https://en.wikipedia.org/wiki/Richardson_Maturity_Model)의 레벨 2를 넘어서는 REST-ful 방식의 큰 이점을 누리지 못합니다.
* 소비자의 임의적인 데이터 요구를 충족하기 위해 규칙성과 표현력을 갖추기 위해 노력해야 합니다.
* 버전이 있어야 하며, 특정 API 버전 내에서 매우 안정적이어야 합니다.
* 코드에 의해 소비되어 처리된 후 잠재적으로 인간에게 제공되어야 합니다.

## 오늘날의 API

오늘날 API는 일반적으로 HTTP를 통한 JSON으로 생각됩니다. 
이러한 API는 거의 항상 하이퍼미디어 API보다는 데이터 지향 API이지만, 때로는 하이퍼미디어 개념이 통합되기도 합니다(대부분 최종 사용자에게 큰 이점을 주지 못합니다). 
업계는 [REST-ful API에서 벗어나](https://graphql.org/) 데이터 API를 REST-ful 모델에 맞추는 데서 발생하는 [문제를 인식하기 시작했습니다.](https://kieranpotts.com/rebranding-rest/)

이것은 좋은 일입니다. 업계는 데이터 API 분야에서 REST-ful 아이디어에 대해 의문을 제기하고, REST가 설명하려 했던 네트워크 아키텍처 대신, 
해당 특정 네트워크 아키텍처를 더 잘 서비스한 이전의 클라이언트-서버 기술을 살펴보기 시작해야 합니다.

## 하이퍼미디어 API 설계

하이퍼미디어 API가 데이터 API와 다르게 설계될 수 있는 방식을 보여주기 위해, 최근 [htmx 디스코드](https://htmx.org/discord)에서 제기된 다음 상황을 고려해 보겠습니다:

> 저는 폼과 테이블이 있는 페이지를 원합니다. 이 폼은 테이블에 새 요소를 추가할 것이며, 테이블은 30초마다 폴링하여 다른 사용자의 업데이트를 표시할 것입니다.

이 UI를 기본 URL `/contacts`로 고려해 보겠습니다.

첫 번째로 필요한 것은 폼과 현재 연락처 테이블을 가져오는 엔드포인트입니다. 이 엔드포인트는 `/contacts`에 위치하며 다음과 같이 작동합니다:

```txt
  GET /contacts -> 폼 및 연락처 테이블 렌더링
```

다음으로, 연락처를 생성할 수 있어야 합니다. 이는 동일한 URL로 POST 요청을 통해 수행됩니다:

```txt
  GET /contacts -> 폼 및 연락처 테이블 렌더링
  POST /contacts -> 새 연락처 생성, GET /contacts로 리디렉션
```

그리고 HTML은 다음과 같이 생겼을 것입니다:

```html
<div>
    <form action='/contacts' method="post">
      <!-- 연락처 추가를 위한 폼 -->
    </form>
    <table>
      <!-- 연락처 테이블 -->
    </table>
</div>
```

지금까지는 매우 전형적인 웹 1.0 애플리케이션입니다. 
이 시점까지는 데이터 API와 하이퍼미디어 API의 요구 사항이 크게 다르지 않지만, 
하이퍼미디어 API가 **자기 설명적**이며 URL을 변경해도 하이퍼미디어 애플리케이션이 손상되지 않는다는 점은 주목할 만합니다.

이제 htmx가 필요한 부분에 도달했습니다. 테이블 업데이트를 위해 주기적으로 서버를 폴링해야 합니다. 
이를 위해 `/contacts/table`이라는 새 엔드포인트를 추가하여 연락처 테이블만 렌더링하도록 합니다:

```txt
  GET /contacts -> 폼 및 연락처 테이블 렌더링
  POST /contacts -> 새 연락처 생성, GET /contacts로 리디렉션
  GET /contacts/table -> 연락처 테이블 렌더링
```

그리고 테이블에 폴 트리거를 추가합니다:

```html
<div>
    <form action='/contacts' method="post">
      <!-- 연락처 추가를 위한 폼 -->
    </form>
    <table hx-trigger="every 30s" hx-get="/contacts/table" hx-swap="outerHTML">
      <!-- 연락처 테이블 -->
    </table>
</div>
```

여기서 하이퍼미디어 API와 데이터 API의 차이가 나타나기 시작합니다. 
이 새 엔드포인트는 데이터 모델의 필요가 아니라 하이퍼미디어의 필요에 의해 완전히 구동됩니다. 
이 엔드포인트는 애플리케이션의 하이퍼미디어 요구 사항이 변경되면 사라질 수 있으며, 그 형태가 크게 바뀔 수 있습니다. 이는 시스템이 자기 설명적이기 때문에 전적으로 허용됩니다.

HTML을 htmx를 사용하여 폴링하도록 업데이트했으므로, 더 나은 사용자 경험(UX)을 위해 폼에서도 htmx를 사용하는 것이 좋습니다:

```html
<div>
    <form action='/contacts' method="post" hx-boost="true">
      <!-- 연락처 추가를 위한 폼 -->
    </form>
    <table hx-trigger="every 30s" hx-get="/contacts/table" hx-swap="outerHTML">
      <!-- 연락처 테이블 -->
    </table>
</div>
```

필요에 따라 입력의 서버 측 검증, 동적 폼 등을 위한 추가 엔드포인트를 추가할 수도 있습니다. 
이러한 엔드포인트는 데이터 모델 고려 사항이 아닌 **하이퍼미디어의 필요**에 따라 구동됩니다. 우리는 애플리케이션에서 달성하려는 목표를 기준으로 생각합니다.

## API 변화

이 짧은 글의 핵심은 다음과 같습니다: 하이퍼미디어 시스템에서는 **하이퍼미디어 시스템의 메시지가 자기 설명적**이기 때문에 API 변경이 문제가 되지 않습니다. 
API를 여러 번 변경해도 애플리케이션이 깨지지 않습니다. 인간 사용자는 새로운 하이퍼미디어(HTML)를 보고 원하는 작업을 선택할 수 있습니다.

컴퓨터와 비교할 때 인간은 [무엇을 해야 할지 결정하는 데](https://intercoolerjs.org/2016/05/08/hatoeas-is-for-humans.html) 뛰어나며, 변화에 비교적 잘 적응합니다.

이는 데이터 API와는 대조적입니다. 데이터 API는 클라이언트 코드를 깨뜨리지 않고는 수정할 수 없으므로, 변경에 있어 훨씬 더 신중해야 합니다. 
데이터 API는 더 많은 클라이언트 요구를 수정 없이 충족시킬 수 있도록 표현력을 높이기 위한 압박을 받습니다.

<aside>

*이러한 상황은 특히 브라우저에서 데이터 API가 소비될 때 [매우 위험](https://intercoolerjs.org/2016/02/17/api-churn-vs-security.html)합니다. 프론트엔드 개발자가 사용할 수 있는 모든 데이터 API 표현력은 잠재적인 악의적인 사용자에게도 노출되며, 이들은 콘솔을 열고 API를 마음대로 조작할 수 있습니다. 페이스북은 이를 해결하기 위해 [화이트리스트](https://twitter.com/AdamChainz/status/1392162996844212232)를 사용한다고 합니다.*

*당신은 그렇게 하고 있습니까?*

</aside>

## 결론

하이퍼미디어 API를 설계할 때는 데이터 API를 설계할 때와 다른 사고방식을 사용해야 합니다. 
변경이 큰 문제가 되지 않으며, 좋은 하이퍼미디어 경험을 제공하기 위해 필요한 엔드포인트를 제공하는 것이 주요 목표가 되어야 합니다.
