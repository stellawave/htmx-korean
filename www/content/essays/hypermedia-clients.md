+++
title = "Hypermedia Clients"
date = 2023-01-28
updated = 2023-01-29
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

종종, 우리는 [REST](@/essays/how-did-rest-come-to-mean-the-opposite-of-rest.md) 및 [HATEOAS](@/essays/hateoas.md)에 대한 
[온라인 토론](https://news.ycombinator.com/item?id=32141027)에서 참을 수 없을 정도로 고지식하게 행동하며 다음과 같은 말을 하곤 합니다:

> JSON은 하이퍼미디어가 아닙니다. 왜냐하면 하이퍼미디어 컨트롤이 없기 때문입니다.
>
> 이 JSON을 보세요:

 ```json
{
  "account": {
    "account_number": 12345,
    "balance": {
      "currency": "usd",
      "value": 50.00
    },
    "status": "open"
  }
}
```

> 보셨죠? 하이퍼미디어 컨트롤이 없습니다.
>
> 그래서 이 JSON은 하이퍼미디어가 아니며, 따라서 이 JSON을 반환하는 API는 RESTful하지 않습니다.

이에 대해, 가끔은 똑똑하고 경험이 많은 웹 개발자가 다음과 같은 반응을 보이기도 합니다:

> 좋아요, REST 전문가님, 그럼 이 JSON은 어떤가요?
```json
{
  "account": {
    "account_number": 12345,
    "balance": {
      "currency": "usd",
      "value": 50.00
    },
    "status": "open",
    "links": {
      "deposits": "/accounts/12345/deposits",
      "withdrawals": "/accounts/12345/withdrawals",
      "transfers": "/accounts/12345/transfers",
      "close-requests": "/accounts/12345/close-requests"
    }
  }
}
```
> 보세요, 이제 이 응답에 하이퍼미디어 컨트롤이 있습니다(일반 사람들은 이것을 링크라고 부르죠). 그래서 이 JSON은 하이퍼미디어입니다.
>
> 이제 이 JSON API는 RESTful입니다. 기분이 좀 나아졌나요?

😑

우리는 온라인 논쟁 상대방이 어느 정도 요점을 가지고 있다는 것을 인정해야 합니다. 
이 JSON 응답에 하이퍼미디어 컨트롤이 포함되어 있으며, 실제로 JSON 응답에 있습니다. 그래서 이 JSON 응답을 RESTful이라고 부를 수 있을까요?

그러나 우리는 고집스럽기 때문에 즉각적인 양보를 하기 전에 몇 가지 [‘사실은 말이죠’](https://i.imgur.com/DpQ9YJl.png) 핑계를 대고 싶습니다:

* 첫째, 이 링크는 접근에 사용할 HTTP 메서드에 대한 정보를 제공하지 않습니다.
* 둘째, 이 링크는 JSON의 *네이티브* 부분이 아니며, 예를 들어, HTML의 앵커와 폼 태그처럼 자연스럽게 존재하지 않습니다.
* 셋째, 각 엔드포인트에서의 하이퍼미디어 상호작용에 관한 많은 정보가 누락되어 있습니다 (예: 요청과 함께 올라가야 할 데이터 등).

그리고 이 모든 것은 인터넷에서 REST에 대한 기술적인 논쟁을 *특별히* 즐겁게 만드는 고지식한 꼬치꼬치 캐기입니다.

그러나 여기에는 더 깊은 [‘사실은 말이죠’](https://i.imgur.com/DpQ9YJl.png) 요소가 있으며, 그것은 *JSON API* 자체가 아니라, 와이어의 반대편, 
즉 *JSON을 받는 클라이언트*에 관련된 것입니다.

## 하이퍼미디어 클라이언트와 표현 정보

이 제안된 비RESTful JSON API 수정의 더 깊은 문제는 이 JSON 응답이 제대로 된 [하이퍼미디어 시스템](https://hypermedia.systems)에 참여하기 위해서는, 
이 JSON을 소비하는 *클라이언트*가 또한 RESTful 아키텍처 스타일이 시스템 전체에 부여하는 
[제약](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)을 충족해야 한다는 점입니다.

특히, 클라이언트는 REST의 [유니폼 인터페이스](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1_5)를 충족해야 하며, 
클라이언트 코드가 주어진 하이퍼미디어를 사용자에게 표시하는 것 외에 응답의 "형태"나 세부 사항에 대해 아무것도 알지 못해야 합니다. 
제대로 작동하는 RESTful 시스템에서는 클라이언트가 특정 하이퍼미디어 표현이 나타내는 도메인에 대해 "out of band" 지식을 가질 수 없습니다.

제안된 JSON-as-hypermedia를 다시 살펴보겠습니다:

```json
{
  "account": {
    "account_number": 12345,
    "balance": {
      "currency": "usd",
      "value": 50.00
    },
    "status": "open",
    "links": {
      "deposits": "/accounts/12345/deposits",
      "withdrawals": "/accounts/12345/withdrawals",
      "transfers": "/accounts/12345/transfers",
      "close-requests": "/accounts/12345/close-requests"
    }
  }
}
```

이제, 이 API의 클라이언트는 이 JSON을 예를 들어, HTML로 변환하기 위해 일반적인 알고리즘을 사용할 수 있습니다. 
클라이언트 측 템플릿 언어를 통해 JSON 객체의 모든 속성을 반복하면서 이 작업을 수행할 수 있습니다.

그러나 여기에는 문제가 있습니다: 이 JSON 응답에는 *표현 정보*가 많지 않습니다. 
이것은 관련된 계정에 대한 상당히 원시적인 데이터 표현이며, 몇 가지 추가 URL이 있을 뿐입니다.

REST의 유니폼 인터페이스 제약을 만족시키려는 클라이언트는 이 데이터를 사용자에게 표시하는 방법에 대해 많은 정보를 가지고 있지 않습니다. 
따라서 클라이언트는 이 계정을 최종 사용자에게 표시하기 위해 매우 기본적인 접근 방식을 채택해야 할 것입니다.

결국, 그것은 이름/값 쌍과 동작을 위한 일련의 일반적인 버튼 또는 링크 세트로 표현될 것입니다, 그렇지 않나요?

JSON 응답의 형식에 대해 무관심하게 남아 있으려면 더 이상 할 수 있는 일이 거의 없습니다.

### 우리의 JSON API를 더 발전시키기

이 문제를 해결하려면 우리의 JSON API를 더 정교하게 만들어 정보의 레이아웃을 어떻게 해야 할지에 대한 정보를 포함시켜야 합니다: 
예를 들어, 일부 필드를 강조하거나 숨겨야 한다는 지시사항 등이 필요할 것입니다.

그러나 그것은 이야기의 일부분에 불과합니다.

클라이언트 측에서 이러한 새로운 JSON API 요소를 적절하게 해석하도록 업데이트해야 할 것입니다. 
그래서 우리는 더 이상 단순한 API 설계자가 아니며, 하이퍼미디어 *클라이언트* 생성 사업에도 참여하게 됩니다. 
또는 더 가능성이 높은 것은, 우리의 *API 클라이언트*가 하이퍼미디어 클라이언트 비즈니스에도 참여하도록 요구하는 것입니다.

Mike Amundsen은 훌륭한 [책](https://www.oreilly.com/library/view/restful-web-clients/9781491921890)을 통해 올바른, 
일반적인 하이퍼미디어 클라이언트를 구축하는 방법에 대해 설명했습니다. 그러나 그 책에서 볼 수 있듯이, 좋은 하이퍼미디어 클라이언트를 만드는 것은 간단하지 않으며, 
또한 *대부분의* 엔지니어가 JSON API를 소비하기 위해 구축할 것이 아닙니다, 심지어 JSON API에 점점 더 정교한 하이퍼미디어 컨트롤과 표현 정보가 포함되더라도 말입니다.

### "비효율적인" 표현

우리가 JSON 응답에 더 많은 정보를 추가하는 것을 고려하기 시작하면, Roy Fielding의 논문에서 나온 한 구절이 떠오릅니다:

> 그러나, 유니폼 인터페이스는 표준화된 형식으로 정보를 전송하기 때문에, 애플리케이션의 요구에 맞춘 형식보다 효율성이 떨어진다는 단점이 있습니다.

_-Roy Fielding, <https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_1_5>_

HTML에 대한 비판 중 하나는 그것이 "표현" 정보와 "의미론적" 정보를 혼합한다는 것입니다. 이는 종종 전형적인 JSON API 응답의 간결함과 불리하게 비교됩니다.

그러나 중요한 점은, HTML이 웹이라는 더 큰 하이퍼미디어 시스템의 구성 요소로 작동하는 이유는 
바로 그 표현 정보와 웹 브라우저(즉, 하이퍼미디어 클라이언트)가 그것을 인간이 상호작용할 수 있는 UI로 변환하는 능력에 있습니다.

그리고 그것이 바로 우리가 제대로 된 하이퍼미디어 클라이언트를 지원하기 위해 우리의 JSON API에 추가하게 되는 것입니다.

## 하이퍼미디어 클라이언트 구축

따라서, JSON API 응답에서 하이퍼미디어 컨트롤을 제공하는 것만으로는 충분하지 않습니다. 그것은 REST 이야기의 *일부*일 뿐, 전체 이야기는 아닙니다. 
그리고 나는 이것이 정말로 *어려운* 부분이 아니라는 것을 깨달았습니다. 사실, 하이퍼미디어 *클라이언트*를 만드는 것이 어려운 부분이며, 
*좋은* 하이퍼미디어 클라이언트를 만드는 것이 *정말 어려운 부분*입니다.

우리는 웹 브라우저가 항상 존재한다고 생각하지만, 일상적인 웹 요청에서 HTML을 단순히 파싱하고 렌더링하는 데 얼마나 많은 기술이 들어가는지 잠시 생각해 보세요. 그것은 *극도로* 복잡합니다.

그래서, 만약 우리가 웹 기반 [하이퍼미디어-구동 애플리케이션](@/essays/hypermedia-driven-applications.md)을 만들고 싶다면, 
표준 웹 기반 하이퍼미디어 클라이언트인 브라우저를 사용하는 것이 아마도 좋은 생각일 것입니다.

브라우저는 이미 매우 강력하고, 잘 테스트된 하이퍼미디어 클라이언트입니다. 그리고 [약간의 도움만 받으면](@/docs.md), 더 나은 하이퍼미디어 클라이언트가 될 수 있습니다.

일반적으로, 모든 REST 제약을 충족하는 좋은 하이퍼미디어 클라이언트를 만드는 것은 어렵습니다. 
따라서 새로운 클라이언트를 구축하기보다는 기존 클라이언트를 사용하고 확장하는 쪽으로 기울어야 합니다.

### Hyperview

그렇긴 하지만, 새로운 하이퍼미디어 클라이언트를 구축하는 것이 적절한 경우도 있습니다. 
예를 들어, [Hyperview](https://hyperview.org/)와 같은 기술이 매우 인상적이고 특별한 이유입니다. 
Hyperview는 모바일 친화적인 새로운 하이퍼미디어, [HXML](https://hyperview.org/docs/guide_html)에 대한 명세를 제공할 뿐만 아니라, 
HXML을 이해하고 렌더링할 수 있는 하이퍼미디어 *클라이언트*를 개발자에게 제공합니다.

하이퍼미디어 클라이언트가 없었다면, Hyperview는 위에서 언급한 JSON과 같은 또 다른 이론상의 하이퍼미디어에 불과할 것이며, 
강력하고 실용적인 *완전한* RESTful 하이퍼미디어 솔루션은 아닐 것입니다.

하이퍼미디어 클라이언트가 없는 하이퍼미디어는 자전거가 없는 물고기와 같으며, 자전거를 탈 수 있는 물고기일 뿐입니다.

## 결론

RESTful 하이퍼미디어 시스템에서 *클라이언트*가 얼마나 중요한지 깨닫는 데 오랜 시간이 걸렸습니다. 
이는 당연한 일입니다. 왜냐하면 초기 REST에 대한 논의는 대부분 [API 설계](https://www.martinfowler.com/articles/richardsonMaturityModel.html)에 관한 것이었고, 
라이언트는 많이 언급되지 않았기 때문입니다.

이제는 많은 이러한 논의가 말보다 앞서 나갔다는 것을 깨닫습니다: 
RESTful 하이퍼미디어 API가 유용하려면, 제대로 된 하이퍼미디어 클라이언트에 의해 소비되어야 합니다. 
그렇지 않으면, 결국 하이퍼미디어 컨트롤은 특정 도메인의 두꺼운 클라이언트에서 낭비될 뿐입니다.

게다가, 하이퍼미디어 API가 정말로 작동하게 하려면 프레젠테이션 레이어 정보를 많이 포함해야 할 가능성이 큽니다. 
"Level 3"의 [Richardson 성숙도 모델](https://martinfowler.com/articles/richardsonMaturityModel.html), 
즉 하이퍼미디어 컨트롤만으로는 "REST의 영광"을 달성하기에 충분하지 않다는 것이 밝혀졌습니다.

실제로, 하이퍼미디어 API를 실제로 작동시키려면 실용적인 프레젠테이션 레벨 기술을 많이 추가해야 하며, 그것을 소비할 제대로 된 하이퍼미디어 클라이언트도 필요합니다.

제가 [HATEOAS는 인간을 위한 것이다](https://intercoolerjs.org/2016/05/08/hatoeas-is-for-humans.html)를 작성할 때는 이러한 개념이 태동하고 있었지만, 
그 당시에는 클라이언트/웹 브라우저가 얼마나 특별한지 깨닫지 못했습니다.

REST는 단순히 API에 관한 것이 아닙니다. Roy Fielding이 그의 논문에서 분명히 밝힌 것처럼, 그것은 *시스템* 아키텍처입니다.

[Mike](https://training.amundsen.com/)와 같은 몇몇 사람들을 제외하고는, 우리는 REST 이야기에 대한 더 큰 부분(사실은 *훨씬 더 큰* 부분)을 무시해 왔습니다.

<div style="text-align:center;padding-top: 24px">

<img src="/img/creating-client.png" alt="Creating A Hypermedia Client Is Hard Joke" style="max-width: 95%">

</div>
