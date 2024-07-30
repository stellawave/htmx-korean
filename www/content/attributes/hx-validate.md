+++
title = "hx-validate"
+++

`hx-validate` 속성은 [HTML5 Validation API](@/docs.md#validation)를 통해 요소가 요청을 제출하기 전에 스스로를 검증하도록 합니다.

기본적으로 `<form>` 요소만 데이터를 검증하지만, 다른 요소들은 그렇지 않습니다. `<input>`, `<textarea>` 또는 `<select>`에 `hx-validate="true"`를 추가하면 요청을 보내기 전에 검증이 가능합니다.

## Notes

* `hx-validate`는 상속되지 않습니다.
