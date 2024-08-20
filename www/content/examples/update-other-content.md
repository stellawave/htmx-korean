+++
title = "Updating Other Content"
template = "demo.html"
+++

사람들이 htmx를 처음 사용할 때 자주 묻는 질문 중 하나는 다음과 같습니다:

> "화면의 다른 콘텐츠를 업데이트해야 합니다. 어떻게 해야 하나요?"

이 작업을 수행할 수 있는 여러 가지 방법이 있으며, 이 예제에서는 몇 가지 방법을 안내해 드리겠습니다.

아래의 기본 UI를 사용하여 이 개념을 설명하겠습니다: 간단한 연락처 테이블과, 이 테이블에 새로운 연락처를 추가하기 위해 [hx-post](@/attributes/hx-post.md)을 사용하는 폼입니다.

```html
<h2>Contacts</h2>
<table class="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th></th>
    </tr>
  </thead>
  <tbody id="contacts-table">
    ...
  </tbody>
</table>
<h2>Add A Contact</h2>
<form hx-post="/contacts">
  <label>
    Name
        <input name="name" type="text">  
  </label>
  <label>
    Email
        <input name="email" type="email">  
  </label>
</form>
```

여기서 문제는, 폼에서 새로운 연락처를 제출할 때, 연락처 테이블이 새로 고침되어 방금 폼에서 추가된 연락처를 포함하도록 해야 한다는 것입니다.

어떤 해결책이 있을까요?

## 해결책 1: 타겟 확장 {#expand}

여기서 가장 간단한 해결책은 폼의 타겟을 테이블과 폼을 모두 포함하도록 "확장"하는 것입니다. 예를 들어, 전체를 `div`로 감싸고 폼에서 해당 `div`를 타겟으로 설정할 수 있습니다:

```html
<div id="table-and-form">
    <h2>Contacts</h2>
    <table class="table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th></th>
        </tr>
      </thead>
      <tbody id="contacts-table">
        ...
      </tbody>
    </table>
    <h2>Add A Contact</h2>
    <form hx-post="/contacts" hx-target="#table-and-form">
      <label>
        Name
            <input name="name" type="text">  
      </label>
      <label>
        Email
            <input name="email" type="email">  
      </label>
    </form>
</div>
```

여기서 우리는 [hx-target](@/attributes/hx-target.md) 속성을 사용하여 포함하는 `div`를 타겟으로 지정하고 있습니다. 
`/contacts`에 대한 `POST` 응답에서 테이블과 폼을 모두 렌더링해야 합니다.

이 방법은 간단하고 신뢰할 수 있지만, 특별히 우아하다고 느껴지지는 않을 수 있습니다.

## 해결책 2: Out of Band Responses {#oob}

이 문제를 해결하는 더 정교한 접근 방식은 [Out of Band 스왑](@/attributes/hx-swap-oob.md)을 사용하여 DOM에 업데이트된 콘텐츠를 삽입하는 것입니다.

이 방법을 사용하면 HTML은 처음 설정한 그대로 유지될 수 있습니다:

```html
<h2>Contacts</h2>
<table class="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th></th>
    </tr>
  </thead>
  <tbody id="contacts-table">
    ...
  </tbody>
</table>
<h2>Add A Contact</h2>
<form hx-post="/contacts">
  <label>
    Name
        <input name="name" type="text">  
  </label>
  <label>
    Email
        <input name="email" type="email">  
  </label>
</form>
```

프론트 엔드에서 무언가를 수정하는 대신, `/contacts`에 대한 `POST` 응답에서 추가적인 콘텐츠를 포함할 수 있습니다:

```html
<tbody hx-swap-oob="beforeend:#contacts-table">
  <tr>
    <td>Joe Smith</td>
    <td>joe@smith.com</td>
  </tr>
</tbody>

<label>
  Name
      <input name="name" type="text">
</label>
<label>
  Email
      <input name="email" type="email">
</label>
```

이 콘텐츠는 [hx-swap-oob](@/attributes/hx-swap-oob.md) 속성을 사용하여 `#contacts-table`에 자신을 추가하여 연락처가 성공적으로 추가된 후 테이블을 업데이트합니다.

## 해결책 3: 이벤트 트리거링 {#events}

더 정교한 접근 방식은 성공적으로 연락처가 생성되었을 때 클라이언트 측 이벤트를 트리거하고, 그 이벤트를 테이블에서 감지하여 테이블을 새로고침하는 것입니다.

```html
<h2>Contacts</h2>
<table class="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th></th>
    </tr>
  </thead>
  <tbody id="contacts-table" hx-get="/contacts/table" hx-trigger="newContact from:body">
    ...
  </tbody>
</table>
<h2>Add A Contact</h2>
<form hx-post="/contacts">
  <label>
    Name
        <input name="name" type="text">  
  </label>
  <label>
    Email
        <input name="email" type="email">  
  </label>
</form>
```

여기서는 연락처 테이블을 다시 렌더링하는 `/contacts/table`이라는 새로운 엔드포인트를 추가했습니다. 
이 요청의 트리거는 `newContact`이라는 사용자 정의 이벤트입니다. 이 이벤트는 폼의 응답에 의해 트리거될 때 이벤트 버블링으로 인해 `body`에 도달하게 되므로, 이를 `body`에서 감지합니다.

`/contacts`로의 `POST` 요청이 성공적으로 완료되어 연락처가 생성되면, 응답에는 다음과 같은 [HX-Trigger](@/headers/hx-trigger.md) 응답 헤더가 포함됩니다:

```txt
HX-Trigger:newContact
```

이 헤더는 테이블이 `/contacts/table`로 `GET` 요청을 보내도록 트리거하며, 이를 통해 새로 추가된 연락처 행(및 테이블의 나머지 부분)이 렌더링됩니다.

아주 깔끔한 이벤트 기반 프로그래밍 방식입니다!

## 해결책 4: Path Dependencies 확장 사용 {#path-deps}

마지막 접근 방식은 REST-ful 경로 의존성을 사용하여 테이블을 새로고침하는 것입니다. 
htmx의 전신인 Intercooler.js는 [경로 기반 의존성](https://intercoolerjs.org/docs.html#dependencies)을 라이브러리에 통합했습니다.

htmx는 이 기능을 기본 기능에서 제외했지만, 
[path deps](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/path-deps/README.md)라는 확장을 통해 비슷한 기능을 제공합니다.

확장을 사용하도록 예제를 업데이트하려면, 확장 자바스크립트를 로드한 다음 다음과 같이 HTML을 주석 처리해야 합니다:

```html
<h2>Contacts</h2>
<table class="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th></th>
    </tr>
  </thead>
  <tbody id="contacts-table" hx-get="/contacts/table" hx-ext="path-deps" hx-trigger="path-deps" path-deps="/contacts">
    ...
  </tbody>
</table>
<h2>Add A Contact</h2>
<form hx-post="/contacts">
  <label>
    Name
        <input name="name" type="text">  
  </label>
  <label>
    Email
        <input name="email" type="email">  
  </label>
</form>
```

이제 폼이 `/contacts` URL로 `POST`를 할 때, `path-deps` 확장은 이를 감지하여 연락처 테이블에 `path-deps` 이벤트를 트리거하여 요청을 발생시킵니다.

이 접근 방식의 장점은 응답 헤더와 관련된 복잡한 처리를 피할 수 있다는 점입니다. 단점은 연락처가 성공적으로 생성되지 않았더라도 모든 `POST` 요청에서 요청이 발생한다는 것입니다.

## 어떤 방법을 사용해야 할까요?

일반적으로는 타겟을 확장하는 첫 번째 접근 방식을 권장합니다. 특히 업데이트해야 하는 요소들이 DOM에서 서로 가까이 있는 경우에는 더욱 그렇습니다. 이 방법은 간단하고 신뢰할 수 있습니다.

그 다음으로는 사용자 정의 이벤트와 OOB 스왑 접근 방식 중 하나를 선택할 수 있습니다. 
저는 이벤트 지향 시스템을 선호하기 때문에 사용자 정의 이벤트 접근 방식을 선호하지만, 이는 개인적인 취향입니다. 
어느 것을 선택할지는 본인의 소프트웨어 엔지니어링 취향과 서버 측 기술과의 적합성에 따라 결정해야 합니다.

마지막으로, path-deps 접근 방식은 흥미로운 접근 방식으로, 만약 이 개념이 본인의 정신적 모델과 전체 시스템 아키텍처에 잘 맞는다면, 명시적인 새로고침을 피하는 재미있는 방법이 될 수 있습니다. 
하지만 이 개념이 크게 끌리지 않는다면, 이 접근 방식을 마지막으로 고려해보시길 권장합니다.
