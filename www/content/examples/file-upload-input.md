+++
title = "Preserving File Inputs after Form Errors"
template = "demo.html"
+++

서버 측 오류 처리 및 유효성 검사를 포함하는 폼에 기본 값과 파일 입력이 모두 포함된 경우, 오류 메시지와 함께 폼이 반환될 때 파일 입력의 값이 손실됩니다. 
그 결과, 사용자가 파일을 다시 업로드해야 하며, 이는 사용자 친화적인 경험을 저해할 수 있습니다.

이 문제를 간단하게 해결하기 위해 다음과 같은 접근 방식을 사용할 수 있습니다:

변경 전:

```html
<form method="POST" id="binaryForm" enctype="multipart/form-data" hx-swap="outerHTML" hx-target="#binaryForm">
    <input type="file" name="binaryFile">
    <button type="submit">Submit</button>
</form>
```

변경 후:

```html
<input form="binaryForm" type="file" name="binaryFile">

<form method="POST" id="binaryForm" enctype="multipart/form-data" hx-swap="outerHTML" hx-target="#binaryForm">
    <button type="submit">Submit</button>
</form>
```

1. 폼 구조 변경: 바이너리 파일 입력을 HTML 구조에서 주 폼 요소 외부로 이동시킵니다.

2. form 속성 사용: 바이너리 파일 입력에 `form` 속성을 추가하고, 그 값을 주 폼의 ID로 설정합니다. 이렇게 하면 파일 입력이 폼 요소 외부에 있어도 폼과 연관되도록 링크됩니다.