+++
title = "Sortable"
template = "demo.html"
+++

이 예제에서는 [Sortable](https://sortablejs.github.io/Sortable/) 자바스크립트 라이브러리를 htmx와 통합하는 방법을 보여줍니다.

먼저, `Sortable` 자바스크립트 라이브러리로 `.sortable` 클래스를 초기화합니다:

```js
htmx.onLoad(function(content) {
    var sortables = content.querySelectorAll(".sortable");
    for (var i = 0; i < sortables.length; i++) {
      var sortable = sortables[i];
      var sortableInstance = new Sortable(sortable, {
          animation: 150,
          ghostClass: 'blue-background-class',

          // `.htmx-indicator`를 정렬 불가능하게 만듭니다.
          filter: ".htmx-indicator",
          onMove: function (evt) {
            return evt.related.className.indexOf('htmx-indicator') === -1;
          },

          // `end` 이벤트 시 정렬을 비활성화합니다.
          onEnd: function (evt) {
            this.option("disabled", true);
          }
      });

      // `htmx:afterSwap` 이벤트에서 정렬을 다시 활성화합니다.
      sortable.addEventListener("htmx:afterSwap", function() {
        sortableInstance.option("disabled", false);
      });
    }
})
```

다음으로, 내부에 정렬 가능한 div들이 있는 폼을 생성하고, Sortable.js의 `end` 이벤트에서 ajax 요청을 트리거합니다:

```html
<form class="sortable" hx-post="/items" hx-trigger="end">
  <div class="htmx-indicator">Updating...</div>
  <div><input type='hidden' name='item' value='1'/>Item 1</div>
  <div><input type='hidden' name='item' value='2'/>Item 2</div>
  <div><input type='hidden' name='item' value='3'/>Item 3</div>
  <div><input type='hidden' name='item' value='4'/>Item 4</div>
  <div><input type='hidden' name='item' value='5'/>Item 5</div>
</form>
```

각 div에는 해당 행의 항목 ID를 지정하는 숨겨진 input이 포함되어 있습니다.

Sortable.js 드래그 앤 드롭으로 목록이 재정렬되면 `end` 이벤트가 발생합니다. htmx는 새로운 순서의 항목 ID들을 `/items`로 POST하여 서버에 저장하게 됩니다.

이게 전부입니다!

{{ demoenv() }}

<script src="https://cdn.jsdelivr.net/npm/sortablejs@latest/Sortable.min.js"></script>
<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================
    htmx.onLoad(function(content) {
        var sortables = content.querySelectorAll(".sortable");
        for (var i = 0; i < sortables.length; i++) {
          var sortable = sortables[i];
          var sortableInstance = new Sortable(sortable, {
              animation: 150,
              ghostClass: 'blue-background-class',

              // Make the `.htmx-indicator` unsortable
              filter: ".htmx-indicator",
              onMove: function (evt) {
                return evt.related.className.indexOf('htmx-indicator') === -1;
              },

              // Disable sorting on the `end` event
              onEnd: function (evt) {
                this.option("disabled", true);
              }
          });

          // Re-enable sorting on the `htmx:afterSwap` event
          sortable.addEventListener("htmx:afterSwap", function() {
            sortableInstance.option("disabled", false);
          });
        }
    })
    
    var listItems = [1, 2, 3, 4, 5]
    // routes
    init("/demo", function(request, params){
      return '<form id="example1" class="list-group col sortable" hx-post="/items" hx-trigger="end">' +
      listContents()
      + "\n</form>";
    });
    
    onPost("/items", function (request, params) {
      console.log(params);
      listItems = params.item;
      return listContents();
    });
    
    // templates
    function listContents() {
      return '<div class="htmx-indicator" style="cursor: default">Updating...</div>' + listItems.map(function(val) {
        return `  <div style="border:1px solid #DEDEDE; padding:12px; margin: 8px; width:200px; cursor: grab" ondrag="this.style.cursor = 'grabbing'" ><input type="hidden" name="item" value="` + val + `"/> Item ` + val + `</div>`;
      }).join("\n");
    }

</script>
