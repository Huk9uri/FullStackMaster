# Day 06 - CORS (Cross-Origin Resource Sharing)

> 웹 · 네트워크

---

# 📅 기본 정보

- **Day**: 06
- **날짜**: 2026-07-30
- **카테고리**: Web / Network
- **주제**: CORS (Cross-Origin Resource Sharing)
- **난이도**: ★★★★☆
- **예상 학습 시간**: 1시간 30분
- **완료 여부**: ✅

---

# 🔁 복습

## 이전 학습 내용 복습 질문

### Q1. Cookie와 Session의 관계를 설명해주세요.

### 내 답변

Cookie는 브라우저의 저장소이다.

Session은 서버가 사용자 인증 정보를 저장하는 방식이다.

Session을 사용할 경우 Cookie에 저장된 JSESSIONID를 이용하여 서버가 Session을 조회하고 인증한다.

---

### Q2. Cookie와 JWT의 관계를 설명해주세요.

### 내 답변

JWT는 인증 토큰이다.

Authorization Header에 직접 담아도 되고,

Cookie에 저장하여 브라우저가 자동으로 전송하도록 사용할 수도 있다.

---

### Q3. Session과 JWT의 가장 큰 차이는?

### 내 답변

Session은 서버가 인증 상태를 저장하는 Stateful 방식이다.

JWT는 토큰 자체에 인증 정보를 포함하는 Stateless 방식이다.

---

# 🎤 오늘의 면접 질문

> CORS(Cross-Origin Resource Sharing)에 대해 설명해주세요.

---

# 📝 학습 진행 방식

오늘은 기존 학습 방식과 다르게 진행되었다.

원래는

```
면접 질문
↓

내가 먼저 답변

↓

답변 평가

↓

개념 학습
```

순서로 진행하지만,

오늘은 ChatGPT가 먼저 CORS의 개념과 동작 원리를 설명한 뒤,

질문과 꼬리 질문을 통해 이해도를 확인하는 방식으로 학습하였다.

따라서 이번 Day에서는

- 내가 먼저 답변
- 답변 평가

항목을 기록하지 않는다.

※ Day07부터는 기존 방식으로 다시 진행한다.

---

# 🧠 핵심 원리

## 한 줄 정의

> CORS는 브라우저가 다른 Origin의 응답을 JavaScript가 읽을 수 있는지 판단하는 브라우저의 보안 정책이다.

---

## 왜 필요한가?

브라우저에는

SOP(Same-Origin Policy)

라는 보안 정책이 존재한다.

SOP는

다른 Origin의 응답을 JavaScript가 읽지 못하도록 제한한다.

하지만

Frontend와 Backend가 서로 다른 Origin인 경우가 많다.

예를 들어

```
React

http://localhost:5173
```

```
Spring Boot

http://localhost:8080
```

Port가 다르므로

다른 Origin이다.

이를 허용하기 위해 CORS가 만들어졌다.

---

## Origin이란?

Origin은

- Protocol
- Host
- Port

세 가지가 모두 같아야 한다.

예시

```
http://localhost:5173

↓

Protocol : http

Host : localhost

Port : 5173
```

---

## CORS는 서버가 아닌 브라우저 정책이다.

많이 오해하는 부분이다.

CORS는

서버가 막는 것이 아니다.

브라우저가

JavaScript에게 응답을 전달할지 결정하는 보안 정책이다.

---

## Simple Request

다음 조건을 만족하면

Simple Request이다.

### Method

- GET
- HEAD
- POST

### Header

CORS Safelisted Header만 사용

### Content-Type

- application/x-www-form-urlencoded
- multipart/form-data
- text/plain

application/json은

Simple Request가 아니다.

---

## Simple Request 동작 과정

```
GET Request

↓

Server

↓

GET Response

↓

브라우저가

Access-Control-Allow-Origin 검사

↓

허용

↓

JavaScript에게 Response 전달
```

허용되지 않으면

브라우저는

응답은 받았지만

JavaScript가 접근하지 못하도록 차단한다.

즉

```
Server

↓

Browser

(성공)

↓

JavaScript

(차단 가능)
```

---

## Preflight Request

다음과 같은 요청은

Preflight Request가 발생한다.

- PUT
- DELETE
- PATCH
- Authorization Header
- application/json

---

## OPTIONS Request

브라우저는 먼저

OPTIONS 요청을 보낸다.

예시

```
OPTIONS /users

Origin:
http://localhost:5173

Access-Control-Request-Method:
DELETE

Access-Control-Request-Headers:
Authorization,
Content-Type
```

---

## OPTIONS Response

서버는

```
Access-Control-Allow-Origin

Access-Control-Allow-Methods

Access-Control-Allow-Headers

Access-Control-Max-Age
```

등을 응답한다.

브라우저는

이를 검사한 뒤

실제 요청을 보낼지 결정한다.

---

## Preflight 동작 과정

```
OPTIONS

↓

OPTIONS Response

↓

브라우저가 CORS 검사

↓

허용

↓

DELETE Request

↓

DELETE Response
```

허용되지 않으면

DELETE 자체를 보내지 않는다.

---

## Simple Request vs Preflight

| Simple Request | Preflight Request |
|----------------|-------------------|
| OPTIONS 없음 | OPTIONS 수행 |
| 요청 먼저 전송 | OPTIONS 먼저 |
| 응답 후 CORS 검사 | OPTIONS 응답 후 검사 |
| 요청은 항상 서버 도착 | 실패하면 실제 요청 안 감 |

---

## Preflight Cache

```
Access-Control-Max-Age: 1800
```

이면

30분 동안

동일한

- Origin
- Method
- Header

조합에서는

OPTIONS를 다시 보내지 않는다.

예시

```
12:00

OPTIONS

↓

DELETE

----------------

12:10

DELETE

----------------

12:20

DELETE

----------------

12:31

OPTIONS

↓

DELETE
```

---

## Browser와 Postman 차이

브라우저

- SOP 적용
- CORS 검사

Postman

- SOP 없음
- CORS 검사 안 함

따라서

Postman에서는 성공하지만

브라우저에서는

CORS Error가 발생할 수 있다.

---

# 🔍 꼬리 질문

---

## Q1. CORS는 서버의 보안 정책인가?

### 내 답변

아니다.

브라우저의 보안 정책이다.

---

## Q2. Simple Request에서도 CORS 검사를 하나요?

### 내 답변

한다.

실제 요청을 먼저 보내고,

응답을 받은 뒤

브라우저가 CORS를 검사한다.

---

## Q3. 브라우저는 응답을 받고도 CORS Error가 발생할 수 있는가?

### 내 답변

그렇다.

브라우저는 이미 응답을 받은 상태이다.

하지만

JavaScript에게 전달하지 않는다.

---

## Q4. Preflight Request가 실패하면 어떻게 되는가?

### 내 답변

OPTIONS 요청만 서버에 도착한다.

브라우저가 허용하지 않으면

DELETE나 PUT 요청은 보내지 않는다.

---

## Q5. Postman에서는 되는데 브라우저에서는 왜 안 되는가?

### 내 답변

브라우저만 CORS를 검사하기 때문이다.

---

# 🛠 실무 연결

## Spring Boot

Spring에서는

- @CrossOrigin
- CorsConfiguration
- WebMvcConfigurer

등을 이용하여

허용할 Origin, Method, Header를 설정한다.

---

## React

React 개발 서버와

Spring Boot 서버는

Port가 다르므로

CORS 설정이 필요하다.

---

# 🧪 실습

## 실습 목표

Chrome DevTools에서

Simple Request와

Preflight Request를 확인한다.

---

## 사용 도구

- Chrome DevTools

---

## 실습

1.

GET 요청 확인

2.

DELETE 요청 확인

3.

OPTIONS 요청 확인

4.

Response Header 확인

5.

Access-Control-Allow-Origin 확인

6.

Access-Control-Max-Age 확인

---

## 확인한 내용

- Simple Request에서는 OPTIONS가 발생하지 않는다.
- DELETE 요청에서는 OPTIONS가 먼저 발생한다.
- OPTIONS 응답을 검사한 뒤 실제 요청을 전송한다.
- Access-Control-Max-Age를 통해 OPTIONS가 캐시된다.

---

# 💬 면접 모범답변

## 1분 답변

CORS는 브라우저의 SOP를 완화하기 위한 보안 정책입니다.

SOP는 다른 Origin의 응답을 JavaScript가 읽지 못하도록 제한합니다.

Simple Request는 실제 요청을 먼저 보내고 응답을 받은 뒤 브라우저가 CORS를 검사하여 JavaScript에 응답을 전달할지 결정합니다.

반면 Preflight Request는 OPTIONS 요청을 먼저 보내 Origin, Method, Header 사용 가능 여부를 확인한 뒤 허용된 경우에만 실제 요청을 전송합니다.

또한 Access-Control-Max-Age를 이용하면 Preflight 결과를 캐시하여 동일한 Origin, Method, Header 조합에서는 OPTIONS 요청을 생략할 수 있습니다.

---

# 🔑 핵심 키워드

- SOP
- Origin
- Protocol
- Host
- Port
- CORS
- Simple Request
- Preflight Request
- OPTIONS
- Access-Control-Allow-Origin
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers
- Access-Control-Max-Age

---

# 🔗 관련 개념

## 이전 주제

➡ Cookie / Session / JWT

## 다음 주제

➡ REST API & RESTful API

## 함께 알아두면 좋은 개념

- CSRF
- SameSite Cookie
- Spring Security CORS
- Reverse Proxy
- API Gateway

---

# 📌 오늘 가장 중요한 한 문장

> CORS는 서버 간 통신을 막는 기술이 아니라, 브라우저가 다른 Origin의 응답을 JavaScript가 읽을 수 있는지 판단하는 브라우저의 보안 정책이다.

---

# ✅ 완료 체크리스트

- [x] 이전 내용을 복습했다.
- [x] CORS의 원리를 이해했다.
- [x] Simple Request와 Preflight의 차이를 이해했다.
- [x] 꼬리 질문에 답했다.
- [x] 실습을 완료했다.
- [x] 면접 모범답변을 작성했다.
- [x] 핵심 키워드를 정리했다.
- [x] 다음 학습 주제를 정했다.