+++
title = "Delete Row"
template = "demo.html"
+++

이 예제에서는 삭제 버튼을 구현하여 완료 시 테이블 행을 제거하는 방법을 보여줍니다. 먼저 테이블 본문을 살펴보겠습니다:

```html
<table class="table delete-row-example">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Status</th>
      <th></th>
    </tr>
  </thead>
  <tbody hx-confirm="Are you sure?" hx-target="closest tr" hx-swap="outerHTML swap:1s">
    ...
  </tbody>
</table>
```

테이블 본문에는 삭제 작업을 확인하기 위한 [`hx-confirm`](@/attributes/hx-confirm.md) 속성이 있습니다. 
또한 모든 버튼에 대해 가장 가까운 테이블 행(`tr`)을 타겟으로 설정했습니다. ([`hx-target`](@/attributes/hx-target.md) 속성은 DOM에서 부모 요소로부터 상속됩니다.) 
[`hx-swap`](@/attributes/hx-swap.md) 속성의 교체 명세는 타겟 전체를 교체하고 응답을 받은 후 1초 동안 대기하도록 설정되어 있습니다. 
이 마지막 부분은 다음 CSS를 사용하여 행이 교체/제거되기 전에 페이드 아웃되도록 하기 위함입니다:

```css
tr.htmx-swapping td {
  opacity: 0;
  transition: opacity 1s ease-out;
}
```

각 행에는 `DELETE` 요청을 서버로 보내어 행을 삭제할 수 있는 URL을 포함한 [`hx-delete`](@/attributes/hx-delete.md) 속성이 있는 버튼이 있습니다. 
이 요청은 `200` 상태 코드와 빈 콘텐츠로 응답하여 해당 행을 아무것도 없는 상태로 교체해야 함을 나타냅니다.

```html
<tr>
  <td>Angie MacDowell</td>
  <td>angie@macdowell.org</td>
  <td>Active</td>
  <td>
    <button class="btn danger" hx-delete="/contact/1">
      Delete
    </button>
  </td>
</tr>
```

<style>
tr.htmx-swapping td {
  opacity: 0;
  transition: opacity 1s ease-out;
}
</style>

{{ demoenv() }}

<script>
    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // data
    var contacts = [
      {
        name: "Joe Smith",
        email: "joe@smith.org",
        status: "Active",
      },
      {
        name: "Angie MacDowell",
        email: "angie@macdowell.org",
        status: "Active",
      },
      {
        name: "Fuqua Tarkenton",
        email: "fuqua@tarkenton.org",
        status: "Active",
      },
      {
        name: "Kim Yee",
        email: "kim@yee.org",
        status: "Inactive",
      },
    ];

    // routes
    init("/demo", function(request, params){
      return tableTemplate(contacts);
    });

    onDelete(/\/contact\/\d+/, function(request, params){
      return "";
    });

    // templates
    function rowTemplate(contact, i) {
      return `<tr>
      <td>${contact["name"]}</td>
      <td>${contact["email"]}</td>
      <td>${contact["status"]}</td>
      <td>
        <button class="btn danger" hx-delete="/contact/${i}">
          Delete
        </button>
      </td>
    </tr>`;
    }

    function tableTemplate(contacts) {
      var rows = "";

      for (var i = 0; i < contacts.length; i++) {
        rows += rowTemplate(contacts[i], i, "");
      }

      return `
<table class="table delete-row-example">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Status</th>
      <th></th>
    </tr>
  </thead>
  <tbody hx-confirm="Are you sure?" hx-target="closest tr" hx-swap="outerHTML swap:1s">
    ${rows}
  </tbody>
</table>`;
    }

</script>
