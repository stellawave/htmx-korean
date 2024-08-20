+++
title = "Cascading Selects"
template = "demo.html"
+++

이 예제에서는 한 `select`의 값이 다른 `select`에서 선택된 값에 따라 달라지도록 만드는 방법을 보여줍니다.

먼저 `make` 선택에 대해 기본값으로 Audi를 설정합니다. 그런 다음 이 `make`에 대한 `model` 선택 항목을 렌더링합니다. 
`make` 선택 항목은 `models` 선택 항목을 대상으로 `/models`로 `GET` 요청을 트리거하도록 설정합니다.

여기 코드가 있습니다:

```html
<div>
    <label>Make</label>
    <select name="make" hx-get="/models" hx-target="#models" hx-indicator=".htmx-indicator">
      <option value="audi">Audi</option>
      <option value="toyota">Toyota</option>
      <option value="bmw">BMW</option>
    </select>
  </div>
  <div>
    <label>Model</label>
    <select id="models" name="model">
      <option value="a1">A1</option>
      ...
    </select>
    <img class="htmx-indicator" width="20" src="/img/bars.svg">
</div>
```

`/models` 엔드포인트로 요청이 전송될 때, 해당 `make`에 대한 모델이 반환됩니다.

```html
<option value='325i'>325i</option>
<option value='325ix'>325ix</option>
<option value='X5'>X5</option> 
```

그리고 `model` select에서 사용할 수 있게 됩니다.

{{ demoenv() }}

<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params){
      return formTemplate();
    });
    
    onGet(/models.*/, function (request, params) {
        var make = dataStore.findMake(params['make']);
        return modelOptionsTemplate(make['models']);
    });
    
    // templates
    function formTemplate() {
      return `  <h3>Pick A Make/Model</h3>              
<form>
  <div>
    <label >Make</label>
    <select name="make" hx-get="/models" hx-target="#models" hx-indicator=".htmx-indicator">
      <option value="audi">Audi</option>
      <option value="toyota">Toyota</option>
      <option value="bmw">BMW</option>
    </select>
  </div>
  <div>
    <label>Model</label>
    <select id="models" name="model">
      <option value="a1">A1</option>
      <option value="a3">A3</option>
      <option value="a6">A6</option>
    </select>
    <img class="htmx-indicator" width="20" src="/img/bars.svg">    
  </div>
</form>`;
    }

    function modelOptionsTemplate(make) {
      return make.map(function(val) {
        return "<option value='" + val + "'>" + val +"</option>";
      }).join("\n");
    }

    var dataStore = function(){
      var data = {
        audi : { models : ["A1", "A4", "A6"] },
        toyota : { models : ["Landcruiser", "Tacoma", "Yaris"] },
        bmw : { models : ["325i", "325ix", "X5"] }
      };
      return {
        findMake : function(make) {
          return data[make];
        }
      }
    }()
</script>
