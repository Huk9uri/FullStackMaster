# Day 01 - URL 입력부터 화면이 나타날 때까지

> 웹 · 네트워크

---

# 📅 기본 정보

- **Day**: 01
- **날짜**: 2026-07-20
- **카테고리**: Web / Network
- **주제**: URL 입력부터 화면이 나타날 때까지
- **난이도**: ★★★☆☆
- **예상 학습 시간**: 1시간
- **완료 여부**: ✅

---

# 🔁 복습

> 첫 학습이므로 생략

---

# 🎤 오늘의 면접 질문

> 사용자가 브라우저 주소창에 URL을 입력했을 때 화면이 나타날 때까지 어떤 일이 발생하는지 설명해주세요.

---

# ✍️ 내가 먼저 답변

1. URL의 도메인을 DNS를 통해 IP 주소로 변환한다.
2. 해당 IP 주소의 서버와 TCP 연결을 수행한다.
3. HTTPS라면 TCP 이후 TLS Handshake를 수행한다.
4. HTTP Request를 전송한다.
5. HTML을 응답받는다.
6. 필요한 CSS, JavaScript를 추가 요청한다.
7. HTML은 DOM, CSS는 CSSOM을 생성한다.
8. Render Tree를 생성한다.
9. Layout과 Paint를 거쳐 화면을 출력한다.
10. SPA라면 JavaScript가 화면을 생성한다.

---

# 📊 답변 평가

## 점수

**95 / 100**

## ✅ 맞은 부분

- DNS → IP 변환
- TCP 연결
- TLS 수행 순서
- HTTP Request / Response
- DOM
- CSSOM
- Render Tree
- Layout
- Paint
- SPA에서 JS가 화면 생성

## ⚠️ 부족한 부분

- Composite 단계가 빠짐
- DNS 조회 순서(Cache → OS → DNS Server)
- 브라우저 캐시 확인 과정 생략

## ❌ 틀린 부분

없음

---

# 🧠 핵심 원리

## 한 줄 정의

> 브라우저는 URL을 IP로 변환하여 서버와 연결한 후 HTML, CSS, JavaScript를 받아 렌더링 엔진이 화면을 생성한다.

---

## 왜 필요한가?

사용자가 입력한 URL은 사람이 읽기 쉬운 이름이다.

컴퓨터는 IP 주소를 이용해 통신하기 때문에

DNS가 URL을 IP 주소로 변환해야 한다.

---

## 내부적으로 어떻게 동작하는가?

```text
URL 입력

↓

DNS 조회

↓

TCP 3-Way Handshake

↓

TLS Handshake (HTTPS)

↓

HTTP Request

↓

HTTP Response (HTML)

↓

추가 리소스(CSS, JS)

↓

DOM + CSSOM

↓

Render Tree

↓

Layout

↓

Paint

↓

Composite

↓

화면 출력
```

---

## 핵심 개념

- DNS
- TCP
- TLS
- HTTP
- DOM
- CSSOM
- Render Tree
- Layout
- Paint
- Composite

---

# 🔍 꼬리 질문

---

## Q1. 왜 DNS가 필요한가?

### 내 답변

URL은 사람이 읽기 쉽지만 컴퓨터는 IP 주소로 통신하기 때문이다.

---

## Q2. TLS는 TCP 전에 수행되는가?

### 내 답변

아니다.

TCP 연결 이후 수행된다.

---

## Q3. TCP와 TLS Handshake의 차이는?

### 내 답변

TCP는 연결을 생성한다.

TLS는

- 서버 인증
- 암호화 알고리즘 협상
- Session Key 생성

을 수행한다.

---

## Q4. 왜 TCP는 3-way Handshake인가?

### 내 답변

양방향 연결을 생성하기 위해

SYN

↓

SYN + ACK

↓

ACK

순으로 진행된다.

---

## Q5. 왜 종료는 4-way Handshake인가?

### 내 답변

서버는 아직 응답을 보내는 중일 수 있으므로

ACK와 FIN을 분리한다.

---

## Q6. FIN + ACK를 같이 보낼 수 없는가?

### 내 답변

가능하다.

하지만 일반적으로

아직 서버가 보낼 데이터가 남아 있으므로

ACK만 먼저 보낸다.

---

## Q7. 전이중(Full Duplex)이란?

### 내 답변

양쪽이 동시에 송신과 수신을 할 수 있는 방식이다.

TCP는

Client → Server

Server → Client

두 개의 독립적인 단방향 통신으로 이해할 수 있다.

---

# 🛠 실무 연결

## 실제 프로젝트에서는?

Spring Boot에서

```
GET /api/users
```

요청도

DNS

↓

TCP

↓

TLS

↓

HTTP

↓

DispatcherServlet

↓

Controller

순으로 처리된다.

React 역시 최초 index.html을 받은 뒤

JavaScript Bundle을 다운로드하여 화면을 생성한다.

---

# 🧪 실습

## 목표

Chrome DevTools에서 실제 요청 과정을 확인한다.

---

## 사용 도구

- ✅ Chrome DevTools
- ⬜ Postman
- ⬜ Spring Debug

---

## 실습

1.

Network 탭 실행

2.

Disable Cache 체크

3.

새로고침

4.

HTML

↓

CSS

↓

JS

↓

Image

요청 순서 확인

---

## 확인한 내용

브라우저가 HTML만 받는 것이 아니라

CSS

JS

이미지 등을 추가 요청한다는 것을 확인하였다.

---

# 💬 면접 모범답변

## 1분 답변

사용자가 URL을 입력하면 브라우저는 먼저 도메인을 DNS를 통해 IP 주소로 변환합니다. 이후 서버와 TCP 3-Way Handshake를 통해 연결을 수립하고, HTTPS라면 TCP 연결 위에서 TLS Handshake를 수행하여 서버를 인증하고 암호화 통신을 준비합니다.

그 후 HTTP Request를 전송하고 HTML을 응답받습니다. HTML을 파싱하여 DOM을 생성하고 CSS는 CSSOM을 생성합니다. DOM과 CSSOM을 합쳐 Render Tree를 만든 뒤 Layout을 통해 각 요소의 위치와 크기를 계산하고 Paint를 통해 화면을 그립니다. 마지막으로 Composite 과정을 거쳐 사용자가 최종 화면을 보게 됩니다. React와 같은 SPA는 HTML이 거의 비어 있으며 JavaScript가 실행되면서 화면을 생성합니다.

---

# 🔑 핵심 키워드

- DNS
- TCP
- TLS
- HTTP
- DOM
- CSSOM
- Render Tree
- Layout
- Paint
- Composite

---

# 🔗 관련 개념

## 이전 주제

없음

## 다음 주제

➡ HTTP와 HTTPS의 차이

## 함께 알아두면 좋은 개념

- TCP 3-Way Handshake
- TLS Handshake
- HTTP Header
- Browser Cache

---

# ✅ 완료 체크리스트

- [x] 내가 먼저 답변했다.
- [x] 피드백을 받았다.
- [x] 원리를 이해했다.
- [x] 꼬리 질문에 답했다.
- [x] 실무와 연결했다.
- [x] DevTools 실습을 진행했다.
- [x] 면접 답변을 완성했다.
- [x] 다음 학습 주제를 정했다.