+++
title = "Why I Tend Not To Use Content Negotiation"
date = 2023-11-18
updated = 2023-11-18
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

저는 하이퍼미디어 API와 데이터(JSON) API에 대해 많이 글을 썼습니다. 
이 글들에는 [두 가지의 차이점](@/essays/hypermedia-apis-vs-data-apis.md), [REST의 "진짜" 의미](@/essays/how-did-rest-come-to-mean-the-opposite-of-rest.md), 
그리고 [HATEOAS](@/essays/hateoas.md)가 [하이퍼미디어 클라이언트](@/essays/hypermedia-clients.md)와 상호작용하는 한 그렇게 나쁘지 않다는 내용이 포함되어 있습니다.

종종 "REST는 HTTP를 통한 JSON"이라고 생각하는 사람들(즉, 일반적인 사람들)과의 대화에서 여러 가지 언어적 및 개념적 문제를 해결해야 할 때가 있습니다:

* 아니요, 저는 일반적인 목적의 API로 HTML을 반환하자는 것이 아닙니다. 하이퍼미디어는 일반적인 목적의 API로는 적합하지 않습니다.
* 네, 저는 웹 애플리케이션을 하이퍼미디어 API에 [강하게 결합시키는 것](@/essays/two-approaches-to-decoupling.md)을 주장하고 있습니다.
* 아니요, 업계에서 [REST라는 용어를 사용하는 방식](@/essays/how-did-rest-come-to-mean-the-opposite-of-rest.md)을 우리가 고칠 수 있을 것이라고 생각하지 않습니다.
* 네, 저는 [데이터 API와 하이퍼미디어 API를 분리하는 것](@/essays/splitting-your-apis.md)을 주장하고 있습니다.

마지막 요점은 일반적으로 단일, 일반적인 목적의 JSON API를 사용하는 사람들에게는 바보처럼 보일 수 있습니다. 
왜 여러 유형의 클라이언트를 만족시킬 수 있는 단일 API가 있는데 두 개의 API를 가져야 합니까? 저는 위의 에세이에서 이 질문에 최선을 다해 답변하려고 했지만, 확실히 타당한 질문입니다.

일반적인 API를 갖는 것과 비교할 때, 이 방법은 여러 면에서 추가적인 작업처럼 보일 수 있습니다.

이 대화에서, 저와 비슷한 REST, [하이퍼미디어 기반 애플리케이션](@/essays/hypermedia-driven-applications.md) 등의 견해를 대체로 동의하는 사람이 종종 다음과 같은 말을 할 것입니다.

> "아, 간단해요. 그냥 _콘텐츠 협상_을 사용하면 됩니다. HTTP에 내장되어 있어요!"

일반적인 JSON API 애호가들만 소외시키는 데 그치지 않고, 이번에는 하이퍼미디어 애호가 동지들도 소외시킬 수 있는 말을 하겠습니다:

*저는 대부분의 애플리케이션에서 JSON과 HTML을 모두 반환하는 데 있어서 콘텐츠 협상이 일반적으로 올바른 접근 방식이라고 생각하지 않습니다.*

## 콘텐츠 협상이란 무엇인가?

우선, "콘텐츠 협상"이란 무엇일까요?

[콘텐츠 협상](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation)은 클라이언트가 서버로부터 응답의 콘텐츠 유형을 협상할 수 있게 해주는 HTTP의 기능입니다. 
HTTP에서의 구현에 대한 완전한 설명은 이 에세이의 범위를 벗어나지만, HTTP에서 콘텐츠 협상을 위한 가장 잘 알려진 메커니즘인 
[`Accept` 요청 헤더](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation#the_accept_header)를 고려해 보겠습니다.

`Accept` 요청 헤더는 브라우저와 같은 클라이언트가 서버로부터 응답에서 수락할 수 있는 `MIME` 유형을 나타낼 수 있게 해줍니다.

이 헤더의 예시는 다음과 같습니다:

```http request
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, image/webp, */*;q=0.8
```

이 `Accept` 헤더는 클라이언트가 수락할 수 있는 형식을 서버에 알려줍니다. 선호도는 `q` 가중치 요인을 통해 표현됩니다. 와일드카드는 별표 `*`로 표현됩니다.

이 경우 클라이언트는 다음과 같이 말하고 있습니다:

> 나는 가장 받고 싶은 형식은 text/html, application/xhtml+xml 또는 image/webp입니다. 그다음으로는 application/xml을 선호합니다. 마지막으로, 당신이 주는 무엇이든 수락하겠습니다.

서버는 이 정보를 사용하여 클라이언트에게 제공할 최적의 콘텐츠 유형을 결정할 수 있습니다.

이것이 "콘텐츠 협상"의 행위이며, 분명 HTTP의 흥미로운 기능입니다.

## API에서 콘텐츠 협상 사용하기

제가 알고 있는 한, [Ruby On Rails](https://rubyonrails.org/) 커뮤니티가 처음으로 콘텐츠 협상을 대규모로 사용하여 동일한 URL에서 HTML과 JSON(및 기타) 형식을 제공하는 방식을 도입했습니다.

Rails에서는 이 작업을 컨트롤러에서 사용할 수 있는 [`respond_to`](https://apidock.com/rails/ActionController/MimeResponds/respond_to) 헬퍼 메서드를 통해 수행합니다.

Rails의 세부적인 내용을 생략하고, `/contacts`에 대한 HTTP `GET` 요청이 `ContactsController` 클래스의 함수를 호출하는 상황을 가정해 보겠습니다. 이 함수는 다음과 같이 생겼습니다:

```ruby
def index
  @contacts = Contacts.all

  respond_to do |format|
    format.html # 기본 렌더링 로직
    format.json { render json: @contacts }
  end
end
```

`respond_to` 헬퍼 메서드를 사용하면, 클라이언트가 위의 `Accept` 헤더를 포함하여 요청을 보낼 때, 컨트롤러는 Rails 템플릿 시스템을 사용하여 HTML 응답을 렌더링합니다.

그러나 클라이언트가 `Accept` 헤더에 `application/json` 값을 포함하여 요청을 보내면, Rails는 연락처를 JSON 배열로 렌더링하여 클라이언트에게 보냅니다.

이것은 꽤 깔끔한 트릭입니다. 연락처 조회와 같은 모든 컨트롤러 로직을 그대로 유지하면서, 약간의 Ruby/Rails 매직을 사용해 콘텐츠 협상을 통해 두 가지 다른 응답 형식을 렌더링할 수 있습니다. 
일반적인 Model/View/Controller 로직에 거의 추가 작업이 필요하지 않습니다.

왜 사람들이 이 아이디어를 좋아하는지 알 수 있습니다!

## 그래서 문제는 무엇일까요?

그렇다면 왜 저는 JSON과 HTML API를 분리하는 데 이 접근 방식이 좋지 않다고 생각할까요?

그 이유는 제가 이전에 언급한 [JSON API와 하이퍼미디어(HTML) API 간의 차이점](@/essays/hypermedia-apis-vs-data-apis.md)에 있습니다. 특히:

* 데이터 API는 버전 관리가 필요하며 특정 버전 내에서 매우 안정적이어야 합니다.
* 데이터 API는 소비자의 임의의 데이터 요구를 충족시키기 위해 규칙성과 표현력을 추구해야 합니다.
* 데이터 API는 일반적으로 토큰 기반 인증을 사용합니다.
* 데이터 API는 속도 제한을 받아야 합니다.
* 하이퍼미디어 API는 일반적으로 세션 쿠키 기반 인증을 사용합니다.
* 하이퍼미디어 API는 기본 하이퍼미디어 애플리케이션의 요구에 의해 구동되어야 합니다.

이 모든 차이점이 중요하며, 각각의 차이점이 컨트롤러 코드에 영향을 미쳐 두 가지 다른 방향으로 끌고 가지만, 
제가 콘텐츠 협상을 애플리케이션에서 사용하지 않기로 자주 선택하는 이유는 첫 번째와 마지막 항목입니다.

JSON API는 클라이언트 코드가 신뢰할 수 있는 안정적인 엔드포인트 세트여야 합니다.

반면에 하이퍼미디어 API는 애플리케이션의 사용자 인터페이스 요구에 따라 크게 변화할 수 있습니다.

이 두 가지는 잘 섞이지 않습니다.

구체적인 예를 들어보겠습니다. 연락처의 세부 정보를 렌더링하는 엔드포인트가 `/contacts/:id`(여기서 `:id`는 렌더링할 연락처의 ID를 포함하는 매개변수)라고 가정해 보겠습니다. 
이 페이지에는 "관련 연락처"라는 UI 섹션이 있고, 이 관련 연락처를 계산하는 데 비용이 많이 드는 이유가 있다고 가정해 보겠습니다.

이 상황에서는 [Lazy Loading](https://htmx.org/examples/lazy-load/) 패턴을 사용하여 초기 연락처 세부 정보 화면이 렌더링된 후 관련 연락처를 로드하도록 할 수 있습니다. 
이렇게 하면 사용자에게 페이지의 지각 성능이 향상됩니다.

이 경우, 지연된 로드 콘텐츠를 `/contacts/:id/related` 엔드포인트에 넣을 수 있습니다.

그런데 나중에 관련 연락처의 계산을 최적화할 수 있게 되었을 때, 이 시점에서 `/contacts/:id/related` 엔드포인트를 제거하고 관련 연락처 정보를 초기 페이지 렌더링에 포함시킬 수 있습니다.

이 모든 것이 하이퍼미디어 API에는 문제가 되지 않습니다. 하이퍼미디어는 [일관된 인터페이스와 HATEOAS](@/essays/hateoas.md)을 통해 이러한 변화를 처리하도록 _설계_되었기 때문입니다.

그러나 JSON API는 그렇지 않습니다.

JSON API는 안정적이어야 합니다. 엔드포인트를 자의적으로 추가하거나 제거할 수는 없습니다. 
네, 일부 엔드포인트는 JSON 또는 HTML로 응답할 수 있고, 다른 일부는 HTML로만 응답할 수 있지만, 이 방법은 복잡해집니다. 
예를 들어, 잘못된 코드가 복사되어 잘못된 곳에 붙여넣어지면 어떻게 될까요?

이 모든 것을 고려할 때, 속도 제한과 같은 요소를 감안하여 JSON API와 하이퍼미디어 API 사이에 
[관심사 분리](https://en.wikipedia.org/wiki/Separation_of_concerns)가 있어야 한다고 강력히 주장할 수 있습니다.

(네, [행동의 지역성](@/essays/locality-of-behaviour.md)이라는 용어를 만든 사람이 SoC 주장을 한다는 것은 역설적이라는 것을 알고 있습니다.)

## 그렇다면 대안은 무엇일까요?

대안은 제가 [API 분리](@/essays/splitting-your-apis.md)에서 주장하는 대로, 즉, API를 분리하는 것입니다. 이는 JSON API와 하이퍼미디어(HTML) API에 대해 서로 다른 경로(또는 하위 도메인 등)를 제공하는 것을 의미합니다.

다시 연락처 API로 돌아가서, 우리는 다음과 같은 구조를 가질 수 있습니다:

* 모든 연락처를 가져오는 JSON API는 `/api/v1/contacts`에 위치합니다.
* 모든 연락처를 가져오는 하이퍼미디어 API는 `/contacts`에 위치합니다.

이 구조는 두 개의 다른 컨트롤러를 암시하며, 저는 이것이 좋은 것이라고 생각합니다. JSON API 컨트롤러는 JSON API의 요구 사항을 구현할 수 있습니다: 
속도 제한, 안정성, 그리고 GraphQL과 같은 표현력 있는 쿼리 메커니즘.

한편, 하이퍼미디어 API(사실상 하이퍼미디어 기반 애플리케이션의 엔드포인트)는 사용자 인터페이스 요구 사항에 따라 크게 변경될 수 있으며, 고도로 최적화된 데이터베이스 쿼리, 
특수 UI 요구 사항을 지원하는 엔드포인트 등을 포함할 수 있습니다.

이 두 가지 관심사를 분리함으로써, JSON API는 안정적이고 규칙적이며 유지 관리가 적게 필요하며, 하이퍼미디어 API는 혼란스럽고 전문화되며 유연할 수 있습니다.
각각은 서로 충돌하지 않고 번성할 수 있는 자신만의 컨트롤러 환경을 갖게 됩니다.

이것이 제가 JSON과 하이퍼미디어 API를 분리하여 별도의 컨트롤러로 분리하는 것을 선호하는 이유이며, 
HTTP 콘텐츠 협상을 사용하여 두 API를 재사용하려는 시도보다는 이 방법이 더 나은 선택이라고 생각합니다.
