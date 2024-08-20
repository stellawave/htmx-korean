+++
title = "Another Real World React -> htmx Port"
date = 2023-09-20
updated = 2023-09-20
[taxonomies]
author = ["Carson Gross"]
tag = ["posts"]
+++

[모든 htmx 데모의 어머니](@/essays/a-real-world-react-to-htmx-port.md)에서 React 기반 프론트엔드를 htmx 기반 프론트엔드로 전환한 실제 사례를 확인할 수 있습니다. 
결과는 매우 좋았지만, 다음과 같은 단서를 붙였습니다:

> 이 숫자들은 놀라운 결과이며, Contexte 애플리케이션이 하이퍼미디어에 매우 적합하다는 사실을 반영합니다. 이 애플리케이션은 많은 텍스트와 이미지를 표시하는 콘텐츠 중심 애플리케이션입니다. 모든 웹 애플리케이션에서 이러한 결과를 기대할 수 있는 것은 아닙니다.
>
> 그러나 시스템의 일부에서라도 하이퍼미디어/htmx 접근 방식을 채택함으로써 **많은** 애플리케이션에서 극적인 개선을 기대할 수 있습니다.

운 좋게도, React 프론트엔드에서 htmx 프론트엔드로 전환된 또 다른 애플리케이션이 있습니다. 이 애플리케이션은 [OpenUnited](https://openunited.com/)입니다(서버 측은 역시 Django 기반).

다음은 Adrian McPhee의 [원래 LinkedIn 게시물](https://www.linkedin.com/feed/update/urn:li:activity:7109116330770878464/)에서 가져온 그래픽으로, 
전환 전후의 코드베이스 총 줄 수를 보여줍니다:

![Open United Before & After](/img/open_united_before_after_htmx.png)

### 전환 전/후 소스 코드

이번 전환의 매우 좋은 측면은 OpenUnited가 오픈 소스라는 점입니다. Contexte와 달리, 전환 전후의 코드를 확인할 수 있습니다:

전환 전: <https://github.com/OpenUnited/old-codebase>

전환 후: <https://github.com/OpenUnited/platform>

## 요약

다음은 전환에 대한 고수준 요약입니다:

* **코드베이스 크기**를 **61%** 감소시켰습니다(31237 LOC에서 12044 LOC로).
* **전체 파일 수**를 **72%** 감소시켰습니다(588개 파일에서 163개 파일로).
* **전체 파일 유형 수**를 **38%** 감소시켰습니다(18개 파일 유형에서 11개 파일 유형으로).
* 주관적으로, 개발 속도는 최소 **5배** 빨라졌습니다.
* Figma에서 프로토타이핑 후 HTML로 포팅하는 대신, UX 개발이 직접 HTML에서 이루어졌습니다.

## 분석

다시 한 번 놀라운 결과를 볼 수 있습니다. OpenUnited 애플리케이션도 하이퍼미디어에 매우 적합합니다. 
Contexte와 마찬가지로, 이 애플리케이션도 많은 텍스트와 이미지를 표시하는 콘텐츠 중심 애플리케이션입니다.

이 경험은 적어도 특정 클래스의 웹 애플리케이션에서는 htmx와 하이퍼미디어 아키텍처가 매우 훌륭한 선택이 될 수 있음을 다시 한번 보여줍니다.
