# LGD-Production-Quality-Dashboard

# LG Display 생산·품질 업무 보고서 웹 서비스

<img width="493" height="398" alt="2026-09-14_171234" src="https://github.com/user-attachments/assets/461b2edf-fd31-4f49-971e-d2c95f700ded" />

**날짜 : 09월 14일**

## 1. 프로젝트 설명

LG디스플레이의 생산·품질 업무 흐름을 이해하기 위해 제작한 **교육용 생산·품질 보고서 웹 서비스**입니다.

Google Colab 환경에서 하나의 Python 셀로 실행할 수 있도록 구성하였으며, 생산 실적과 검사 결과를 조회하고 설비 사건 이력을 확인한 뒤 해당 데이터를 근거로 업무 보고서 초안을 생성할 수 있습니다.

사용자는 조회 기간과 생산 라인을 선택하여 다음 정보를 확인할 수 있습니다.

* 목표 생산량
* 실제 생산량
* 생산 달성률
* 검사 수량
* 불량 수량
* 불량률
* LOT별 생산·검사 기록
* 날짜별 불량률
* 설비 사건 이력
* 현재 모의 설비 상태

생산 달성률과 불량률은 다음과 같이 계산합니다.

```text
생산 달성률 = 실제 생산수량 합계 ÷ 목표 생산수량 합계 × 100

불량률 = 불량수량 합계 ÷ 검사수량 합계 × 100
```

검사 수량이 0인 경우에는 불량률을 계산하지 않고 **판단 불가(—)** 로 표시합니다.

또한 Three.js를 이용하여 패널 검사·이송 설비를 3D로 표현하였습니다. 설비에는 금속 프레임, 롤러 컨베이어, 유리 커버, 검사 헤드, 상태등, 패널 이송 애니메이션 등이 포함되어 있습니다.

---

## 2. 주요 기능

### 생산·품질 데이터 조회

조회 시작일, 종료일, 생산라인을 선택하면 해당 조건에 맞는 생산·품질 데이터를 집계합니다.

조회 가능한 생산라인은 다음과 같습니다.

```text
전체
라인 A
라인 B
```

조회 결과에서는 LOT별 목표수량, 생산수량, 검사수량, 불량수량을 확인할 수 있습니다.

### 생산 달성률 및 불량률 계산

조회된 LOT의 데이터를 단순 평균하지 않고 **수량 합계를 기준으로 계산**합니다.

```text
달성률 = 생산 합계 / 목표 합계

불량률 = 불량 합계 / 검사 합계
```

이를 통해 조회 범위 전체의 실제 생산성과 품질 수준을 확인할 수 있습니다.

### 날짜별 불량률 표시

조회 기간 내 날짜별 검사수량과 불량수량을 집계하여 일자별 불량률을 표시합니다.

불량률은 막대 형태로 시각화하여 날짜별 품질 상태를 쉽게 비교할 수 있도록 구성했습니다.

### 설비 사건 이력 확인

조회 기간에 발생한 설비 관련 사건을 함께 표시합니다.

예시 데이터에는 다음과 같은 상태가 포함됩니다.

```text
모의 정지
온도 주의
정상 복귀
미해제 상태
```

실제 고장 원인을 임의로 판단하지 않고, 저장된 교육용 이력만 화면에 표시하도록 구성했습니다.

### Three.js 3D 설비 모델

Three.js를 이용하여 패널 검사·이송 설비를 3D로 구현했습니다.

구현 요소는 다음과 같습니다.

```text
금속 설비 외장
설비 프레임
롤러 컨베이어
패널
반투명 유리 커버
검사 헤드
검사 스캔 표시
상태 표시등
조명 및 그림자
설비 패널 이송 애니메이션
```

마우스를 이용하여 설비를 회전하거나 확대·축소할 수 있습니다.

### 업무 보고서 초안 생성

현재 조회된 생산·품질 데이터와 설비 사건을 근거로 업무 보고서 초안을 생성합니다.

보고서에는 다음 내용이 포함됩니다.

```text
조회 기간
생산라인
생산량
목표량
생산 달성률
검사수량
불량수량
불량률
LOT 기록 수
설비 사건
현재 모의 설비 상태
추가 확인 사항
검토 메모
```

보고서가 생성되는 시점의 조회 데이터를 별도의 근거 데이터로 저장하기 때문에 이후 조회 조건이 변경되더라도 기존 보고서의 근거는 변경되지 않습니다.

### 보고서 저장 및 다운로드

작성한 보고서 본문과 검토 여부를 저장할 수 있습니다.

저장된 보고서는 다음 형식으로 다운로드할 수 있습니다.

```text
TXT
JSON
```

TXT 파일에는 작성된 업무 보고서가 포함되며, JSON 파일에는 보고서 작성 당시 사용한 근거 데이터가 저장됩니다.

### Cloudflared 외부 접속

Colab 내부에서 실행한 Flask 웹 서버를 Cloudflared Tunnel과 연결하여 외부 브라우저에서도 웹 서비스에 접속할 수 있도록 구성했습니다.

실행 시 자동으로 다음 형태의 주소가 생성됩니다.

```text
https://xxxxx.trycloudflare.com
```

Colab 런타임이 종료되거나 코드를 다시 실행하면 접속 주소가 변경될 수 있습니다.

---

## 3. 사용 라이브러리

### Python

```text
Flask
Werkzeug
asyncio
threading
subprocess
urllib
json
pathlib
socket
re
datetime
```

### Front-End

```text
HTML5
CSS3
JavaScript
Three.js
OrbitControls
WebGL
```

### Server / Network

```text
Flask Web Server
Werkzeug Server
Cloudflared Tunnel
```

### 실행 환경

```text
Google Colab
Python 3
Chrome / WebGL 지원 브라우저
```

---

## 4. 프로젝트 구조

본 프로젝트는 Google Colab에서 **1개의 Python 셀로 전체 서비스를 실행**할 수 있도록 구성되어 있습니다.

실행 과정은 다음과 같습니다.

```text
1. Flask 설치 여부 확인
2. 프로젝트 디렉터리 생성
3. HTML/CSS/JavaScript 파일 생성
4. Three.js 파일 다운로드
5. Flask 웹 서버 실행
6. 생산·품질 API 실행
7. 보고서 저장 기능 실행
8. Cloudflared 다운로드 및 실행
9. 외부 접속 URL 생성
10. 브라우저에서 웹 서비스 접속
```

Three.js는 CDN에서 필요한 모듈을 내려받아 로컬 정적 파일로 사용합니다.

---

## 5. 실행 방법

Google Colab에서 프로젝트 코드를 하나의 셀에 입력한 후 실행합니다.

정상적으로 실행되면 다음과 같은 메시지가 출력됩니다.

```text
1/3 Three.js 구성요소를 준비합니다.
2/3 웹 서버가 실행되었습니다.
3/3 Cloudflared 접속 주소를 생성합니다.

웹 화면 주소:
https://xxxxx.trycloudflare.com
```

출력된 URL을 클릭하면 웹 서비스를 사용할 수 있습니다.

Colab 런타임은 웹 서버 역할을 수행하기 때문에 웹 서비스를 사용하는 동안 런타임을 유지해야 합니다.

---

## 6. 교육용 데이터

본 프로젝트에서 사용하는 생산·품질 데이터 및 설비 상태는 실제 생산 데이터를 사용하지 않은 **교육용 가상 데이터**입니다.

예를 들어 다음과 같은 LOT 데이터가 포함되어 있습니다.

```text
라인 A
A01
A02
A11
A12
A13

라인 B
B01
B02
B11
B12
```

또한 현재 모의 설비 상태는 다음과 같이 설정되어 있습니다.

```text
라인 A : 정상 / 28°C
라인 B : 온도 주의 / 42°C
```

실제 LG디스플레이 생산 설비의 품질 기준이나 고장 원인을 의미하지 않습니다.

---

## 7. 참고 문헌

### Three.js

Three.js Documentation
https://threejs.org/docs/

Three.js GitHub
https://github.com/mrdoob/three.js

Three.js OrbitControls
https://threejs.org/docs/#examples/en/controls/OrbitControls

### Flask

Flask Documentation
https://flask.palletsprojects.com/

Flask GitHub
https://github.com/pallets/flask

### Werkzeug

Werkzeug Documentation
https://werkzeug.palletsprojects.com/

### Cloudflare

Cloudflare Tunnel Documentation
https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/

Cloudflared GitHub
https://github.com/cloudflare/cloudflared

### Google Colab

Google Colaboratory
https://colab.research.google.com/

Google Colab FAQ
https://research.google.com/colaboratory/faq.html

### WebGL

MDN Web Docs - WebGL
https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API

---

## 8. 참고 사항

본 프로젝트는 **LG디스플레이 직무 이해 및 웹 프로그래밍 교육을 위한 실습 프로젝트**입니다.

실제 생산·품질 시스템과 연결되어 있지 않으며, 프로젝트에서 사용되는 생산량, 검사 결과, 불량 데이터, 설비 사건 및 온도 값은 모두 교육 목적으로 작성된 가상 데이터입니다.

Three.js를 이용한 설비 역시 실제 설비를 그대로 재현한 것이 아니라 생산·검사 공정의 이해를 돕기 위한 교육용 3D 모델입니다.
