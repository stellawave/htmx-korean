+++
title = "hx-boost"
+++

`hx-boost` 속성은 일반 anchor와 form 태그에서 AJAX를 사용하도록 "부스트"할 수 있게 합니다. 
이것은 [점진적 향상](https://en.wikipedia.org/wiki/Progressive_enhancement)이라는 좋은 점을 가지고 있는데, 사용자가 자바스크립트를 활성화하지 않은 경우에도 사이트가 계속 작동하게 됩니다.

anchor 태그의 경우, anchor를 클릭하면 `href`에 지정된 URL로 `GET` 요청을 보내고 URL을 푸시하여 히스토리 항목이 생성됩니다. 
대상은 `<body>` 태그이며 기본적으로 `innerHTML` 교체 전략이 사용됩니다. 이러한 모든 항목은 적절한 속성을 사용하여 수정할 수 있지만, `click` 트리거는 제외됩니다.

form의 경우 요청은 `method` 속성에 따라 `GET` 또는 `POST`로 변환되며 `submit`에 의해 트리거됩니다. 
다시 말해, 대상은 페이지의 `body`가 되며 `innerHTML` 교체가 사용됩니다. 
그러나 URL은 푸시되지 않으며 히스토리 항목이 생성되지 않습니다. (URL을 푸시하려면 [hx-push-url](@/attributes/hx-push-url.md) 속성을 사용할 수 있습니다.)

다음은 몇 가지 부스트된 링크의 예입니다:

```html
<div hx-boost="true">
  <a href="/page1">Go To Page 1</a>
  <a href="/page2">Go To Page 2</a>
</div>
```

이 링크들은 해당 URL로 ajax `GET` 요청을 보내고 페이지 본문의 내용을 해당 응답으로 교체합니다.

다음은 부스트된 폼의 예입니다:

```html
<form hx-boost="true" action="/example" method="post">
    <input name="email" type="email" placeholder="Enter email...">
    <button>Submit</button>
</form>
```

이 폼은 지정된 URL로 ajax `POST` 요청을 보내고 페이지 본문의 내용을 해당 응답으로 교체합니다.

## Notes

* `hx-boost`는 상속되며 부모 요소에 배치할 수 있습니다.
* 동일 도메인에 대한 링크만 부스트되며 로컬 anchor는 부스트되지 않습니다.
* 모든 요청은 AJAX를 통해 수행되므로 리디렉션과 같은 작업을 할 때 이를 염두에 두십시오.
* 요청이 부스트된 anchor나 form에서 발생했는지 확인하려면 요청 헤더에서 [`HX-Boosted`](@/reference.md#request_headers)를 찾으면 됩니다.
* 자식 요소에서 선택적으로 부스트를 비활성화하려면 `hx-boost="false"`를 사용할 수 있습니다.
* [`hx-preserve="true"`](@/attributes/hx-preserve.md)를 사용하여 부스트된 요소와 그 자식 요소의 교체를 비활성화할 수 있습니다.
