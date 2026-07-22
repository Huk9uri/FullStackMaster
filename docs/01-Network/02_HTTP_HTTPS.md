# Day 02 - HTTP와 HTTPS의 차이

> 웹 · 네트워크

---

# 📅 기본 정보

- **Day**: 02
- **날짜**: 2026-07-22
- **카테고리**: Web / Network
- **주제**: HTTP와 HTTPS의 차이
- **난이도**: ★★★☆☆
- **예상 학습 시간**: 1시간
- **완료 여부**: ✅

---

# 🔁 복습

## 이전 학습 내용 복습 질문

### Q1. 브라우저에서 URL을 입력하면 어떤 순서로 화면이 출력되는가?

### 내 답변

```
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

HTTP Response

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
```

---

### Q2. TCP Handshake와 TLS Handshake의 차이는?

### 내 답변

TCP는 서버와의 연결을 생성한다.

TLS는 TCP 연결 이후

- 서버 인증
- 암호화 알고리즘 협상
- Session Key 생성

을 수행한다.

---

### Q3. 왜 TCP 종료는 4-Way Handshake인가?

### 내 답변

TCP는 Full Duplex 통신이다.

서버가 아직 보낼 데이터가 남아 있을 수 있기 때문에

ACK와 FIN을 분리하여 전송한다.

---

# 🎤 오늘의 면접 질문

> HTTP와 HTTPS의 차이를 설명해주세요.

---

# ✍️ 내가 먼저 답변

HTTP는 데이터를 주고받는 프로토콜이다.

HTTPS는 HTTP를 암호화하는 프로토콜이다.

암호화된다는 것 외에는 정확한 차이는 잘 모르겠다.

---

# 📊 답변 평가

## 점수

**40 / 100**

### ✅ 맞은 부분

- HTTPS가 HTTP보다 안전하다는 점을 알고 있었다.
- HTTPS가 암호화를 사용한다는 점을 알고 있었다.

### ⚠️ 부족한 부분

- HTTP가 평문으로 통신한다는 이유를 설명하지 못했다.
- TLS가 언제 수행되는지 몰랐다.
- HTTPS가 HTTP Request 전체를 암호화한다는 점을 몰랐다.
- 기밀성, 무결성, 서버 인증을 제공하는 이유를 설명하지 못했다.
- 암호화되는 정보와 암호화되지 않는 정보를 구분하지 못했다.
- HSTS의 존재를 알지 못했다.

### ❌ 틀린 부분

없음

---

# 🧠 핵심 원리

## 한 줄 정의

> HTTPS는 TLS를 이용하여 HTTP Request와 Response 전체를 안전하게 전송하는 프로토콜이다.

---

## 왜 필요한가?

HTTP는 평문으로 데이터를 전송한다.

따라서

- 도청
- 데이터 변조
- 서버 위장

공격에 취약하다.

HTTPS는

- 기밀성(Confidentiality)
- 무결성(Integrity)
- 서버 인증(Authentication)

을 제공하여 이러한 문제를 해결한다.

---

## 내부적으로 어떻게 동작하는가?

```
URL 입력

↓

DNS 조회

↓

TCP 3-Way Handshake

↓

TLS Handshake

↓

HTTP Request (암호화)

↓

HTTP Response (암호화)
```

---

## HTTP와 HTTPS 비교

| HTTP | HTTPS |
|------|-------|
| 평문 전송 | TLS 암호화 |
| 도청 가능 | 도청 방지 |
| 변조 가능 | 무결성 검증 |
| 서버 인증 없음 | 서버 인증 |
| 기본 포트 80 | 기본 포트 443 |

---

## 암호화되는 정보

TLS는 HTTP Request 전체를 암호화한다.

- HTTP Method
- URL Path
- Query String
- Header
- Body

---

## 암호화되지 않는 정보

TLS 이전에 필요한 정보는 암호화되지 않는다.

- DNS 조회에 사용되는 Domain
- 목적지 IP 주소
- Port 번호

---

# 🔍 꼬리 질문

---

## Q1. HTTPS는 비밀번호만 암호화하는가?

### 내 답변

아니다.

HTTP Request 전체를 암호화한다.

---

## Q2. 왜 Domain은 암호화되지 않는가?

### 내 답변

DNS 조회가 TLS 이전에 수행되므로

Domain은 암호화 전에 사용된다.

---

## Q3. Path와 Query String은 암호화되는가?

### 내 답변

HTTP Request의 일부이므로

TLS가 암호화한다.

---

## Q4. HTTPS를 사용하면 SQL Injection도 막을 수 있는가?

### 내 답변

아니다.

HTTPS는 전송 중 데이터를 보호하는 기술이다.

SQL Injection은 서버가 입력값을 처리하는 방식의 문제이므로

Prepared Statement 등을 사용하여 방어해야 한다.

---

## Q5. HTTP는 왜 위험한가?

### 내 답변

암호화와 무결성 검증이 없기 때문에

중간에서 데이터를 읽거나 수정할 수 있다.

---

# 🛠 실무 연결

## Spring Boot

Spring Security는 HTTPS 환경에서

로그인 요청과 JWT 등을 안전하게 전달한다.

---

## React

React는 HTTPS를 통해

HTML

JavaScript Bundle

API 요청을 안전하게 주고받는다.

---

# 🧪 실습

## 실습 목표

Chrome DevTools에서

HTTPS 요청과 HSTS를 확인한다.

---

## 사용 도구

- ✅ Chrome DevTools

---

## 실습

1.

https://google.com 접속

2.

Network 탭 실행

3.

http://google.com 입력

4.

307 Internal Redirect 확인

5.

Headers 확인

---

## 확인한 내용

- Request URL
- Request Method
- Status Code

모두 DevTools에서 확인할 수 있었다.

이는 DevTools가 브라우저 내부에서 암호화 이전의 HTTP Request를 보여주기 때문이다.

또한

```
307 Internal Redirect
```

와

```
Non-Authoritative-Reason: HSTS
```

를 확인하였다.

이를 통해

브라우저가 서버에 HTTP 요청을 보내기 전에

HSTS 정책에 따라 HTTPS로 내부 리다이렉트한다는 것을 확인하였다.

---

# 💬 면접 모범답변

## 1분 답변

HTTP는 데이터를 평문으로 전송하는 프로토콜로 암호화, 무결성, 서버 인증을 제공하지 않습니다.

반면 HTTPS는 TCP 연결 이후 TLS Handshake를 수행하여 서버를 인증하고 암호화에 사용할 세션 키를 생성합니다.

이후 HTTP Request와 Response 전체를 암호화하여 전송하므로 도청과 변조를 방지할 수 있습니다.

다만 DNS 조회에 사용되는 Domain과 통신에 필요한 IP 주소는 TLS 이전에 사용되므로 기본적인 HTTPS에서는 암호화되지 않습니다.

또한 HSTS가 적용된 사이트는 브라우저가 307 Internal Redirect를 수행하여 HTTP 요청을 보내기 전에 HTTPS로 변경합니다.

---

# 🔑 핵심 키워드

- HTTP
- HTTPS
- TLS
- Confidentiality
- Integrity
- Authentication
- HSTS
- 307 Internal Redirect
- Domain
- Path

---

# 🔗 관련 개념

## 이전 주제

➡ URL 입력부터 화면 렌더링까지

## 다음 주제

➡ HTTP/1.1 vs HTTP/2 vs HTTP/3

## 함께 알아두면 좋은 개념

- SSL
- TLS Handshake
- Certificate
- Public Key
- Session Key

---

# 📌 오늘 가장 중요한 한 문장

> HTTPS는 단순히 암호화만 하는 것이 아니라 TLS를 이용하여 기밀성, 무결성, 서버 인증을 제공하는 안전한 HTTP 통신 방식이다.

---

# ✅ 완료 체크리스트

- [x] 내가 먼저 답변했다.
- [x] 피드백을 받았다.
- [x] 원리를 이해했다.
- [x] 꼬리 질문에 답했다.
- [x] 실습을 완료했다.
- [x] 면접 모범답변을 작성했다.
- [x] 핵심 키워드를 정리했다.
- [x] 다음 학습 주제를 정했다.