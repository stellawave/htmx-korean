+++
title = "Progress Bar"
template = "demo.html"
+++

이 예제는 부드럽게 스크롤되는 진행 바를 구현하는 방법을 보여줍니다.

우리는 `/start`로 `POST` 요청을 보내 작업을 시작하는 버튼이 있는 초기 상태로 시작합니다:

```html
<div hx-target="this" hx-swap="outerHTML">
  <h3>Start Progress</h3>
  <button class="btn primary" hx-post="/start">
            Start Job
  </button>
</div>
```

이 div는 상태와 진행 바가 포함된 새로운 div로 교체되며, 이 진행 바는 600ms마다 자동으로 새로 고쳐집니다:

```html
<div hx-trigger="done" hx-get="/job" hx-swap="outerHTML" hx-target="this">
  <h3 role="status" id="pblabel" tabindex="-1" autofocus>Running</h3>

  <div
    hx-get="/job/progress"
    hx-trigger="every 600ms"
    hx-target="this"
    hx-swap="innerHTML">
    <div class="progress" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0" aria-labelledby="pblabel">
      <div id="pb" class="progress-bar" style="width:0%">
    </div>
  </div>
</div>
```

이 진행 바는 600밀리초마다 업데이트되며, `width` 스타일 속성과 `aria-valuenow` 속성이 현재 진행 상태로 설정됩니다. 
진행 바 div에 ID가 있기 때문에, htmx는 스타일 속성을 새로운 값으로 설정하면서 요청 사이의 변화를 부드럽게 전환합니다. 
이는 CSS 전환과 결합되어 시각적인 전환이 끊김 없이 부드럽게 진행됩니다.

마지막으로, 프로세스가 완료되면 서버는 `HX-Trigger: done` 헤더를 반환하여 UI를 "Complete" 상태로 업데이트하며, 
UI에 다시 시작 버튼이 추가됩니다(이 예제에서는 버튼에 페이드인 효과를 추가하기 위해 [`class-tools`](https://github.com/bigskysoftware/htmx-extensions/blob/main/src/class-tools/README.md) 확장을 사용하고 있습니다):

```html
<div hx-trigger="done" hx-get="/job" hx-swap="outerHTML" hx-target="this">
  <h3 role="status" id="pblabel" tabindex="-1" autofocus>Complete</h3>

  <div
    hx-get="/job/progress"
    hx-trigger="none"
    hx-target="this"
    hx-swap="innerHTML">
      <div class="progress" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="122" aria-labelledby="pblabel">
        <div id="pb" class="progress-bar" style="width:122%">
      </div>
    </div>
  </div>

  <button id="restart-btn" class="btn primary" hx-post="/start" classes="add show:600ms">
    Restart Job
  </button>
</div>
```

이 예제에서는 Bootstrap 진행 바의 스타일을 참조하고 있습니다.

```css
.progress {
    height: 20px;
    margin-bottom: 20px;
    overflow: hidden;
    background-color: #f5f5f5;
    border-radius: 4px;
    box-shadow: inset 0 1px 2px rgba(0,0,0,.1);
}
.progress-bar {
    float: left;
    width: 0%;
    height: 100%;
    font-size: 12px;
    line-height: 20px;
    color: #fff;
    text-align: center;
    background-color: #337ab7;
    -webkit-box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
    box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
    -webkit-transition: width .6s ease;
    -o-transition: width .6s ease;
    transition: width .6s ease;
}
```

{{ demoenv() }}

<style>
.progress {
    height: 20px;
    margin-bottom: 20px;
    overflow: hidden;
    background-color: #f5f5f5;
    border-radius: 4px;
    box-shadow: inset 0 1px 2px rgba(0,0,0,.1);
}
.progress-bar {
    float: left;
    width: 0%;
    height: 100%;
    font-size: 12px;
    line-height: 20px;
    color: #fff;
    text-align: center;
    background-color: #337ab7;
    -webkit-box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
    box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
    -webkit-transition: width .6s ease;
    -o-transition: width .6s ease;
    transition: width .6s ease;
}
#restart-btn {
  opacity:0;
}
#restart-btn.show {
  opacity:1;
  transition: opacity 100ms ease-in;
}
</style>
<script>

    //=========================================================================
    // Fake Server Side Code
    //=========================================================================

    // routes
    init("/demo", function(request, params){
      return startButton("Start Progress");
    });

    onPost("/start", function(request, params){
        var job = jobManager.start();
        return jobStatusTemplate(job);
    });

    onGet("/job", function(request, params){
        var job = jobManager.currentProcess();
        return jobStatusTemplate(job);
    });

    onGet("/job/progress", function(request, params, responseHeaders){
        var job = jobManager.currentProcess();

        if (job.complete) {
          responseHeaders["HX-Trigger"] = "done";
        }
        return jobProgressTemplate(job);
    });

    // templates
    function startButton(message) {
      return `<div hx-target="this" hx-swap="outerHTML">
  <h3>${message}</h3>
  <button class="btn primary" hx-post="/start">
            Start Job
  </button>
</div>`;
    }

    function jobProgressTemplate(job) {
      return `<div class="progress" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="${job.percentComplete}" aria-labelledby="pblabel">
      <div id="pb" class="progress-bar" style="width:${job.percentComplete}%">
    </div>
  </div>`
    }

    function jobStatusTemplate(job) {
        return `<div hx-trigger="done" hx-get="/job" hx-swap="outerHTML" hx-target="this">
  <h3 role="status" id="pblabel" tabindex="-1" autofocus>${job.complete ? "Complete" : "Running"}</h3>

  <div
    hx-get="/job/progress"
    hx-trigger="${job.complete ? 'none' : 'every 600ms'}"
    hx-target="this"
    hx-swap="innerHTML">
    ${jobProgressTemplate(job)}
  </div>
  ${restartButton(job)}`;
    }

    function restartButton(job) {
      if(job.complete){
        return `
<button id="restart-btn" class="btn primary" hx-post="/start" classes="add show:600ms">
  Restart Job
</button>`
      } else {
        return "";
      }
    }

    var jobManager = (function(){
      var currentProcess = null;
      return {
        start : function() {
          currentProcess = {
            complete : false,
            percentComplete : 0
          }
          return currentProcess;
        },
        currentProcess : function() {
          currentProcess.percentComplete += Math.min(100, Math.floor(33 * Math.random()));  // simulate progress
          currentProcess.complete = currentProcess.percentComplete >= 100;
          return currentProcess;
        }
      }
    })();
</script>
