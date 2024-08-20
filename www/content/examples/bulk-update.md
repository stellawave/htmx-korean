+++
title = "Bulk Update"
template = "demo.html"
+++

이 데모는 행을 선택한 후 일괄 업데이트하는 일반적인 패턴을 구현하는 방법을 보여줍니다. 이는 테이블 주위에 form을 배치하고, 
테이블 안에 체크박스를 넣고, 체크된 값을 form 제출(`POST` 요청)에 포함시켜 수행됩니다:

```html
<form id="checked-contacts"
      hx-post="/users"
      hx-swap="outerHTML settle:3s"
      hx-target="#toast">
    <table>
      <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Active</th>
      </tr>
      </thead>
      <tbody id="tbody">
        <tr>
          <td>Joe Smith</td>
          <td>joe@smith.org</td>
          <td><input type="checkbox" name="active:joe@smith.org"></td>
        </tr>
        ...
      </tbody>
    </table>
    <input type="submit" value="Bulk Update" class="btn primary">
    <span id="toast"></span>
</form>
```

서버는 체크박스의 값에 따라 상태를 일괄 업데이트합니다. 
서버는 업데이트에 대한 작은 토스트 메시지로 응답하여 사용자에게 알리고, 접근성을 위해 ARIA를 사용하여 업데이트를 공손하게 알립니다.

```css
#toast.htmx-settling {
  opacity: 100;
}

#toast {
  background: #E1F0DA;
  opacity: 0;
  transition: opacity 3s ease-out;
}
```

흥미로운 점은 HTML form 입력이 이미 자체 상태를 관리하기 때문에 사용자 테이블의 일부를 다시 렌더링할 필요가 없다는 것입니다. 
활성 사용자 체크박스는 이미 체크되어 있고, 비활성 사용자 체크박스는 체크 해제되어 있습니다!

아래에서 이 코드의 작동 예제를 확인할 수 있습니다.

<style scoped="">
#toast.htmx-settling {
  opacity: 100;
}

#toast {
  background: #E1F0DA;
  opacity: 0;
  transition: opacity 3s ease-out;
}
</style>

{{ demoenv() }}

<script>
    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    const dataStore = (() => {
      const data = {
        "joe@smith.org": {name: 'Joe Smith', status: 'Active'},
        "angie@macdowell.org": {name: 'Angie MacDowell', status: 'Active'},
        "fuqua@tarkenton.org": {name: 'Fuqua Tarkenton', status: 'Active'},
        "kim@yee.org": {name: 'Kim Yee', status: 'Inactive'},
      };

      return {
        all() {
          return data;
        },

        activate(email) {
          if (data[email].status === 'Active') {
            return 0;
          } else {
            data[email].status = 'Active';
            return 1;
          }
        },

        deactivate(email) {
          if (data[email].status === 'Inactive') {
            return 0;
          } else {
            data[email].status = 'Inactive';
            return 1;
          }
        },
      };
    })();

    // routes
    init("/demo", function(request){
        return displayUI(dataStore.all());
    });

    /*
    Params look like:
    {"active:joe@smith.org":"on","active:angie@macdowell.org":"on","active:fuqua@tarkenton.org":"on"}
    */
    onPost("/users", function (req, params) {
      const actives = {};
      let activated = 0;
      let deactivated = 0;

      // Build a set of active users for efficient lookup
      for (const param of Object.keys(params)) {
        const nameEmail = param.split(':');
        if (nameEmail[0] === 'active') {
          actives[nameEmail[1]] = true;
        }
      }

      // Activate or deactivate users based on the lookup
      for (const email of Object.keys(dataStore.all())) {
        if (actives[email]) {
          activated += dataStore.activate(email);
        } else {
          deactivated += dataStore.deactivate(email);
        }
      }

      return `<span id="toast" aria-live="polite">Activated ${activated} and deactivated ${deactivated} users</span>`;
    });

    // templates
    function displayUI(contacts) {
      return `<h3>Select Rows And Activate Or Deactivate Below</h3>
               <form
                id="checked-contacts"
                hx-post="/users"
                hx-swap="outerHTML settle:3s"
                hx-target="#toast"
              >
                <table>
                  <thead>
                  <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Active</th>
                  </tr>
                  </thead>
                  <tbody id="tbody">
                    ${displayTable(contacts)}
                  </tbody>
                </table>
                <input type="submit" value="Bulk Update" class="btn primary">
                <span id="toast"></span>
              </form>
              <br>`;
    }

    function displayTable(contacts) {
      var txt = "";

      for (email of Object.keys(contacts)) {
        txt += `
<tr>
  <td>${contacts[email].name}</td>
  <td>${email}</td>
  <td>
    <input
      type="checkbox"
      name="active:${email}"
      ${contacts[email].status === 'Active' ? 'checked' : ''}>
  </td>
</tr>
`;
      }

      return txt;
    }
</script>
