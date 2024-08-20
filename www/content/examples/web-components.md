+++
title = "Web Components"
template = "demo.html"
+++

이 예제는 HTMX를 웹 컴포넌트와 통합하여 shadow DOM 내에서 사용할 수 있도록 하는 방법을 보여줍니다.

기본적으로 HTMX는 웹 컴포넌트에 대해 아무것도 알지 못하며, shadow DOM 내부를 인식하지 않습니다. 
따라서 HTMX에게 컴포넌트의 shadow DOM에 대해 수동으로 알려줘야 합니다. 이를 위해 [`htmx.process`](https://htmx.org/api/#process)를 사용해야 합니다.

```js
customElements.define('my-component', class MyComponent extends HTMLElement {
  // 이 메서드는 커스텀 엘리먼트가 페이지에 추가될 때 실행됩니다.
  connectedCallback() {
    const root = this.attachShadow({ mode: 'closed' })
    root.innerHTML = `
      <button hx-get="/my-component-clicked" hx-target="next div">Click me!</button>
      <div></div>
    `
    htmx.process(root) // HTMX에게 이 컴포넌트의 shadow DOM에 대해 알립니다.
  }
})
```

한번 HTMX에게 컴포넌트의 shadow DOM을 알리면, 대부분의 기능이 예상대로 동작할 것입니다. 
그러나 `hx-target`과 같은 셀렉터는 같은 shadow DOM 내의 요소들만 볼 수 있다는 점에 유의해야 합니다. 웹 컴포넌트 외부의 요소에 접근해야 할 경우, 다음 옵션 중 하나를 사용할 수 있습니다:

- `host`: 현재 shadow DOM을 호스팅하는 요소를 선택합니다.
- `global`: 접두사로 사용하면 현재 shadow DOM 대신 메인 문서에서 선택합니다.

동일한 원칙이 shadow DOM을 사용하지 않는 웹 컴포넌트에도 적용됩니다. 
shadow DOM처럼 셀렉터가 캡슐화되지는 않지만, 여전히 `htmx.process`를 호출하여 HTMX에게 컴포넌트의 콘텐츠를 알려줘야 합니다.

{{ demoenv() }}

<script>
  //=========================================================================
  // Fake Server Side Code
  //=========================================================================

  // data
  let timesClicked = 0

  customElements.define('my-component', class MyComponent extends HTMLElement {
    // This method runs when your custom element is added to the page
    connectedCallback() {
      const root = this.attachShadow({ mode: 'closed' })
      root.innerHTML = `
        <button hx-get="/my-component-clicked" hx-target="next div">Click me!</button>
        <div></div>
      `
      htmx.process(root) // Tell HTMX about this component's shadow DOM
    }
  })

  // routes
  init('/demo', function() {
    return `<my-component></my-component>`
  })

  onGet('/my-component-clicked', function() {
    return `<p>Clicked ${++timesClicked} time${timesClicked > 1 ? 's' : ''}!</p>`
  })
</script>