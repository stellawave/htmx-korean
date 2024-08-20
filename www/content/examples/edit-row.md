+++
title = "Edit Row"
template = "demo.html"
+++

이 예제는 편집 가능한 테이블 행을 구현하는 방법을 보여줍니다. 먼저 테이블 본문을 살펴보겠습니다:

```html
<table class="table delete-row-example">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th></th>
    </tr>
  </thead>
  <tbody hx-target="closest tr" hx-swap="outerHTML">
    ...
  </tbody>
</table>
```

이 코드는 테이블 내에서 발생하는 요청이 트리거된 가장 가까운 행을 대상으로 하여 해당 행 전체를 교체하도록 설정합니다.

다음은 각 행에 대한 HTML 코드입니다:

```html
<tr>
  <td>${contact.name}</td>
  <td>${contact.email}</td>
  <td>
    <button class="btn danger"
            hx-get="/contact/${contact.id}/edit"
            hx-trigger="edit"
            onClick="let editing = document.querySelector('.editing')
                     if(editing) {
                       Swal.fire({title: 'Already Editing',
                                  showCancelButton: true,
                                  confirmButtonText: 'Yep, Edit This Row!',
                                  text:'Hey! You are already editing a row! Do you want to cancel that edit and continue?'})
                       .then((result) => {
                            if(result.isConfirmed) {
                               htmx.trigger(editing, 'cancel')
                               htmx.trigger(this, 'edit')
                            }
                        })
                     } else {
                        htmx.trigger(this, 'edit')
                     }">
      Edit
    </button>
  </td>
</tr>
```

여기서는 조금 더 복잡한 동작을 추가하여 한 번에 하나의 행만 편집할 수 있도록 하고 있습니다. 
JavaScript를 사용하여 `.editing` 클래스가 적용된 행이 있는지 확인한 후, 사용자가 다른 행을 편집할지 여부를 확인합니다. 
만약 편집을 계속하고 싶다면, 이전 행에 `cancel` 이벤트를 트리거하여 해당 행이 원래 상태로 돌아가게 합니다.

그 후, 현재 요소에 `edit` 이벤트를 트리거하여 행의 편집 가능한 버전을 가져오는 htmx 요청을 트리거합니다.

여러 행을 동시에 편집하는 것이 상관없다면, JavaScript와 커스텀 `hx-trigger`를 생략하고 htmx의 기본 클릭 핸들링을 사용해도 됩니다. 
또는, 편집 버튼을 클릭할 때 전체 테이블을 대상으로 하는 방식으로 상호 배타성을 구현할 수도 있습니다. 
여기서는 htmx와 JavaScript를 통합하여 문제를 해결하고 서버와의 상호작용을 최소화하는 방법을 보여주고 있으며, SweetAlert 확인 대화 상자를 사용하고 있습니다.

마지막으로, 데이터가 편집 중일 때의 행은 다음과 같습니다:

```html
<tr hx-trigger='cancel' class='editing' hx-get="/contact/${contact.id}">
  <td><input name='name' value='${contact.name}'></td>
  <td><input name='email' value='${contact.email}'></td>
  <td>
    <button class="btn danger" hx-get="/contact/${contact.id}">
      Cancel
    </button>
    <button class="btn danger" hx-put="/contact/${contact.id}" hx-include="closest tr">
      Save
    </button>
  </td>
</tr>
```

여기서 몇 가지 중요한 점이 있습니다: 첫째, 행 자체가 `cancel` 이벤트에 응답하여 행을 읽기 전용 상태로 되돌릴 수 있습니다. 
취소 버튼을 통해 현재 편집을 취소할 수 있으며, 저장 버튼을 통해 `PUT` 요청을 보내 연락처를 업데이트할 수 있습니다. 
또한 [`hx-include`](@/attributes/hx-include.md)를 사용하여 가장 가까운 행에 있는 모든 입력을 포함합니다. 
HTML의 제약으로 인해 테이블 행 내부에 `form`을 직접 넣을 수 없으므로 이를 통해 처리가 좀 더 수월해집니다.

{{ demoenv() }}

<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
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
        id: 0
      },
      {
        name: "Angie MacDowell",
        email: "angie@macdowell.org",
        status: "Active",
        id: 1
      },
      {
        name: "Fuqua Tarkenton",
        email: "fuqua@tarkenton.org",
        status: "Active",
        id: 2
      },
      {
        name: "Kim Yee",
        email: "kim@yee.org",
        status: "Inactive",
        id: 3
      },
    ];

    // routes
    init("/demo", function(request, params){
      return tableTemplate(contacts);
    });

    onGet(/\/contact\/\d+/, function(request, params){
      var id = parseInt(request.url.split("/")[2]); // get the contact
      var contact = contacts[id];
      console.log(request, id, contact)
      if(request.url.endsWith("/edit")) {
        return editTemplate(contacts[id])
      } else {
        return rowTemplate(contacts[id])
      }
    });

    onPut(/\/contact\/\d+/, function(request, params){
      var id = parseInt(request.url.split("/")[2]); // get the contact
      contact = contacts[id]
      contact.name = params['name'];
      contact.email = params['email'];
      return rowTemplate(contact);
    });

    // templates
    function rowTemplate(contact) {
      return `<tr>
      <td>${contact.name}</td>
      <td>${contact.email}</td>
      <td>
        <button class="btn danger"
                hx-get="/contact/${contact.id}/edit"
                hx-trigger="edit"
                onClick="let editing = document.querySelector('.editing')
                         if(editing) {
                           Swal.fire({title: 'Already Editing',
                                      showCancelButton: true,
                                      confirmButtonText: 'Yep, Edit This Row!',
                                      text:'Hey!  You are already editing a row!  Do you want to cancel that edit and continue?'})
                           .then((result) => {
                                if(result.isConfirmed) {
                                   htmx.trigger(editing, 'cancel')
                                   htmx.trigger(this, 'edit')
                                }
                            })
                         } else {
                            htmx.trigger(this, 'edit')
                         }">
          Edit
        </button>
      </td>
    </tr>`;
    }

    function editTemplate(contact) {
      return `<tr hx-trigger='cancel' class='editing' hx-get="/contact/${contact.id}">
      <td><input name='name' value='${contact.name}'</td>
      <td><input name='email' value='${contact.email}'</td>
      <td>
        <button class="btn danger" hx-get="/contact/${contact.id}">
          Cancel
        </button>
        <button class="btn danger" hx-put="/contact/${contact.id}" hx-include="closest tr">
          Save
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
      <th></th>
    </tr>
  </thead>
  <tbody hx-target="closest tr" hx-swap="outerHTML">
    ${rows}
  </tbody>
</table>`;
    }
</script>
