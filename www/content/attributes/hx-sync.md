+++
title = "hx-sync"
+++

`hx-sync` 속성은 여러 요소 간의 AJAX 요청을 동기화할 수 있게 합니다.

`hx-sync` 속성은 동기화할 요소를 나타내는 CSS 선택자로 구성되며, 선택적으로 콜론 뒤에 동기화 전략을 지정할 수 있습니다. 사용 가능한 전략은 다음과 같습니다:

* `drop` - 기존 요청이 진행 중인 경우 이 요청을 무시합니다 (기본값)
* `abort` - 기존 요청이 진행 중인 경우 이 요청을 무시하며, 그렇지 않은 경우 이 요청이 진행 중일 때 다른 요청이 발생하면 이 요청을 중단합니다
* `replace` - 현재 요청이 있다면 중단하고 이 요청으로 교체합니다
* `queue` - 이 요청을 지정된 요소와 관련된 요청 대기열에 넣습니다

`queue` 수정자는 대기 방법을 정확히 지정하는 추가 인수를 가질 수 있습니다:

* `queue first` - 요청이 진행 중일 때 나타나는 첫 번째 요청을 대기열에 넣습니다
* `queue last` - 요청이 진행 중일 때 나타나는 마지막 요청을 대기열에 넣습니다
* `queue all` - 요청이 진행 중일 때 나타나는 모든 요청을 대기열에 넣습니다

## Notes

* `hx-sync`는 상속되며 부모 요소에 배치할 수 있습니다

이 예제는 form의 제출 요청과 개별 입력 검증 요청 간의 경쟁 조건을 해결합니다. 
`hx-sync`를 사용하지 않으면 input을 채우고 form을 즉시 제출할 때 `/validate`와 `/store`로 두 개의 병렬 요청이 트리거됩니다. 
input에 `hx-sync="closest form:abort"`를 사용하면 form에서 요청을 감시하고, form 요청이 존재하거나 input 요청이 진행 중일 때 시작되면 input 요청을 중단합니다.

```html
<form hx-post="/store">
    <input id="title" name="title" type="text" 
        hx-post="/validate" 
        hx-trigger="change"
        hx-sync="closest form:abort">
    <button type="submit">Submit</button>
</form>
```

검증 요청을 제출 요청보다 우선시하려면 `drop` 전략을 사용할 수 있습니다. 이 예제는 검증 요청이 진행 중인 경우 form을 제출할 수 없도록 검증 요청을 제출 요청보다 우선시합니다.

```html
<form hx-post="/store">
    <input id="title" name="title" type="text" 
        hx-post="/validate" 
        hx-trigger="change"
        hx-sync="closest form:drop"
    >
    <button type="submit">Submit</button>
</form>
```

많은 입력을 포함하는 form을 다룰 때, form 태그에 hx-sync `replace` 전략을 사용하여 제출 요청을 모든 입력 검증 요청보다 우선시할 수 있습니다. 
이는 진행 중인 모든 검증 요청을 취소하고 `hx-post="/store"` 요청만 수행합니다. 제출 요청을 중단하고 기존 검증 요청을 우선시하려면 form 태그에 `hx-sync="this:abort"` 전략을 사용할 수 있습니다.

```html
<form hx-post="/store" hx-sync="this:replace">
    <input id="title" name="title" type="text" hx-post="/validate" hx-trigger="change" />
    <button type="submit">Submit</button>
</form>
```

활성 검색 기능을 구현할 때 `hx-trigger` 속성의 `delay` 수정자를 사용하여 사용자의 입력을 debounce하고 사용자가 입력하는 동안 여러 요청을 하지 않도록 할 수 있습니다. 
그러나 한 번 요청이 이루어지면 이전 요청이 완료되지 않은 상태에서 사용자가 다시 입력을 시작하면 새 요청이 시작됩니다. 이 예제에서는 진행 중인 요청을 취소하고 마지막 요청만 사용합니다. 
검색 입력이 대상에 포함된 경우, 이렇게 `hx-sync`를 사용하면 사용자가 입력하는 동안 입력이 교체될 가능성을 줄이는 데 도움이 됩니다.

```html
<input type="search" 
    hx-get="/search" 
    hx-trigger="keyup changed delay:500ms, search" 
    hx-target="#search-results"
    hx-sync="this:replace">
```
