+++
title = "Click to Load"
template = "demo.html"
+++

이 예제는 데이터 테이블에서 다음 페이지를 클릭하여 로드하는 방법을 보여줍니다. 이 데모의 핵심은 마지막 행입니다:

```html
<tr id="replaceMe">
  <td colspan="3">
    <button class='btn primary' hx-get="/contacts/?page=2"
                        hx-target="#replaceMe"
                        hx-swap="outerHTML">
         Load More Agents... <img class="htmx-indicator" src="/img/bars.svg">
    </button>
  </td>
</tr>
```

이 행에는 버튼이 포함되어 있으며, 이 버튼을 클릭하면 다음 페이지의 결과로 해당 행 전체가 대체됩니다(그리고 이 새로운 행에는 또 다른 페이지를 로드할 버튼이 포함됩니다). 
이런 식으로 계속해서 다음 페이지를 로드할 수 있습니다.

{{ demoenv() }}

<script>
    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // data
    var dataStore = function(){
      var contactId = 9;
      function generateContact() {
        contactId++;
        var idHash = "";
        var possible = "ABCDEFG0123456789";
        for( var i=0; i < 15; i++ ) idHash += possible.charAt(Math.floor(Math.random() * possible.length));
        return { name: "Agent Smith", email: "void" + contactId + "@null.org", id: idHash }
      }
      return {
        contactsForPage : function(page) {
          var vals = [];
          for( var i=0; i < 10; i++ ){
            vals.push(generateContact());
          }
          return vals;
        }
      }
    }()

    // routes
    init("/demo", function(request, params){
        var contacts = dataStore.contactsForPage(1)
        return tableTemplate(contacts)
    });

    onGet(/\/contacts.*/, function(request, params){
        var page = parseInt(params['page']);
        var contacts = dataStore.contactsForPage(page)
        return rowsTemplate(page, contacts);
    });

    // templates
    function tableTemplate(contacts) {
        return `<table><thead><tr><th>Name</th><th>Email</th><th>ID</th></tr></thead><tbody>
                ${rowsTemplate(1, contacts)}
                </tbody></table>`
    }

    function rowsTemplate(page, contacts) {
      var txt = "";
      for (var i = 0; i < contacts.length; i++) {
        var c = contacts[i];
        txt += `<tr><td>${c.name}</td><td>${c.email}</td><td>${c.id}</td></tr>\n`;
      }
      txt += loadMoreRow(page);
      return txt;
    }

    function loadMoreRow(page) {
      return `<tr id="replaceMe">
  <td colspan="3">
    <center>
      <button class='btn primary' hx-get="/contacts/?page=${page + 1}"
                       hx-target="#replaceMe"
                       hx-swap="outerHTML">
         Load More Agents... <img class="htmx-indicator" src="/img/bars.svg">
       </button>
    </center>
  </td>
</tr>`;
    }
</script>
