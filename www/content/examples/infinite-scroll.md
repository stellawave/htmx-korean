+++
title = "Infinite Scroll"
template = "demo.html"
+++

무한 스크롤 패턴은 사용자가 스크롤할 때 콘텐츠를 동적으로 로드하는 방법을 제공합니다.

여기서는 마지막 행(또는 콘텐츠의 마지막 요소)에 집중해보겠습니다:

```html
<tr hx-get="/contacts/?page=2"
    hx-trigger="revealed"
    hx-swap="afterend">
  <td>Agent Smith</td>
  <td>void29@null.org</td>
  <td>55F49448C0</td>
</tr>
```

이 마지막 요소는 스크롤되어 화면에 보일 때 요청을 트리거하는 리스너를 포함하고 있습니다. 결과는 이 요소 뒤에 추가됩니다. 
결과의 마지막 요소 역시 다음 페이지의 결과를 로드할 리스너를 포함하게 되며, 이 과정은 계속 반복됩니다.

> `revealed` - 요소가 뷰포트에 스크롤되어 나타날 때 트리거됩니다(지연 로딩에도 유용합니다). CSS에서 `overflow-y: scroll`과 같은 `overflow`를 사용하는 경우, `revealed` 대신 `intersect once`를 사용하는 것이 좋습니다.

{{ demoenv() }}

<script>
    server.autoRespondAfter = 1000; // longer response for more drama

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
        for( var i=0; i < 10; i++ ) idHash += possible.charAt(Math.floor(Math.random() * possible.length));
        return { name: "Agent Smith", email: "void" + contactId + "@null.org", id: idHash }
      }
      return {
        contactsForPage : function(page) {
          var vals = [];
          for( var i=0; i < 20; i++ ){
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
      return `<table hx-indicator=".htmx-indicator"><thead><tr><th>Name</th><th>Email</th><th>ID</th></tr></thead><tbody>
              ${rowsTemplate(1, contacts)}
              </tbody></table><center><img class="htmx-indicator" width="60" src="/img/bars.svg"></center>`
    }

    function rowsTemplate(page, contacts) {
      var txt = "";
      var trigger_attributes = "";

      for (var i = 0; i < contacts.length; i++) {
        var c = contacts[i];

        if (i == (contacts.length - 1)) {
         trigger_attributes = ` hx-get="/contacts/?page=${page + 1}" hx-trigger="revealed" hx-swap="afterend"`
        }

        txt += "<tr" + trigger_attributes +"><td>" + c.name + "</td><td>" + c.email + "</td><td>" + c.id + "</td></tr>\n";
      }
      return txt;
    }
</script>



