+++
title = "File Upload"
template = "demo.html"
+++

이 예제에서는 파일 업로드 양식을 생성하고 Ajax를 통해 제출하는 방법과 함께 진행 상황을 표시하는 진행 막대를 구현하는 방법을 보여줍니다.

여기서는 두 가지 구현 방식을 소개합니다. 하나는 순수 JavaScript를 사용한 방식이고, 다른 하나는 [hyperscript](https://hyperscript.org)를 사용한 방식입니다.

먼저 순수 JavaScript 버전입니다.

* 파일이 올바르게 인코딩되도록 `multipart/form-data` 타입의 폼을 사용합니다.
* 폼을 `/upload`로 POST 요청을 보냅니다.
* `progress` 요소가 있습니다.
* `htmx:xhr:progress` 이벤트를 듣고, 이벤트의 `loaded` 및 `total` 속성에 따라 진행 막대의 `value` 속성을 업데이트합니다.

```html
<form id='form' hx-encoding='multipart/form-data' hx-post='/upload'>
    <input type='file' name='file'>
    <button>
        Upload
    </button>
    <progress id='progress' value='0' max='100'></progress>
</form>
<script>
    htmx.on('#form', 'htmx:xhr:progress', function(evt) {
      htmx.find('#progress').setAttribute('value', evt.detail.loaded/evt.detail.total * 100)
    });
</script>
```

Hyperscript 버전은 매우 유사하지만 다음과 같은 차이가 있습니다:

* 스크립트가 폼 요소에 직접 포함됩니다.
* Hyperscript는 더 깔끔한 문법을 제공합니다 (하지만 htmx API도 꽤 간편합니다!).

```html
    <form hx-encoding='multipart/form-data' hx-post='/upload'
          _='on htmx:xhr:progress(loaded, total) set #progress.value to (loaded/total)*100'>
        <input type='file' name='file'>
        <button>
            Upload
        </button>
        <progress id='progress' value='0' max='100'></progress>
    </form>
```

참고로, hyperscript는 `details`의 속성을 변수로 직접 구조 분해 할당할 수 있습니다.
