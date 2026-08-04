# Day 07 - Proxy / Reverse Proxy / Load Balancer

> 웹 · 네트워크

------------------------------------------------------------------------

# 📅 기본 정보

-   **Day**: 07
-   **날짜**: 2026-08-04
-   **카테고리**: Web / Network
-   **주제**: Proxy / Reverse Proxy / Load Balancer
-   **난이도**: ★★★★☆
-   **예상 학습 시간**: 1시간 30분
-   **완료 여부**: ✅

------------------------------------------------------------------------

# 🔁 복습

## 이전 학습 내용 복습 질문

### Q1. CORS는 어디에서 적용되는 정책인가?

**내 답변**

브라우저에서 적용되는 정책이다. 브라우저는 서버의 응답을 받은 뒤
JavaScript에게 해당 응답을 전달해도 되는지 판단한다.

### Q2. Preflight Request는 언제 발생하는가?

**내 답변**

브라우저가 안전하지 않을 수도 있다고 판단하는 요청에서 발생한다.

대표적으로 PUT, DELETE, PATCH, Authorization 헤더, Content-Type:
application/json 등의 경우 OPTIONS 요청을 먼저 보낸다.

### Q3. 왜 GET 요청은 대부분 Preflight가 발생하지 않는가?

**내 답변**

GET은 대부분 Simple Request 조건을 만족하기 때문이다. 단, Authorization
헤더 등이 포함되면 GET이어도 Preflight가 발생할 수 있다.

------------------------------------------------------------------------

# 🎤 오늘의 면접 질문

> Proxy, Reverse Proxy, Load Balancer의 차이를 설명해주세요.

------------------------------------------------------------------------

# ✍️ 내가 먼저 답변

-   Proxy는 대리자 역할을 한다.
-   Reverse Proxy는 서버를 대신한다.
-   Load Balancer는 여러 서버에 요청을 분산한다.

------------------------------------------------------------------------

# 📊 답변 평가

## 점수

**85 / 100**

### ✅ 맞은 부분

-   Proxy가 대리자 역할이라는 점을 알고 있었다.
-   Reverse Proxy가 서버를 대신한다는 개념을 이해하고 있었다.
-   Load Balancer가 요청을 분산한다는 점을 알고 있었다.

### ⚠️ 부족한 부분

-   Proxy가 필요한 이유
-   Forward Proxy와 Reverse Proxy의 차이
-   Reverse Proxy와 Load Balancer의 관계
-   TLS Termination의 의미

------------------------------------------------------------------------

# 🧠 핵심 원리

## Proxy

Proxy는 클라이언트와 서버 사이에서 요청과 응답을 대신 처리하는 중간
계층이다.

공통 기능을 중앙에서 수행한다.

-   인증
-   캐시
-   압축
-   접근 제어
-   로그

이를 통해 서버는 비즈니스 로직에만 집중할 수 있다.

## Forward Proxy

클라이언트를 대신하는 Proxy이다.

``` text
Client
   ↓
Forward Proxy
   ↓
Internet
```

대표 기능

-   IP 숨김
-   인터넷 접근 제어
-   캐시

## Reverse Proxy

서버를 대신하는 Proxy이다.

``` text
Client
   ↓
Reverse Proxy (Nginx)
   ↓
Spring Boot
```

대표 기능

-   TLS Termination
-   캐시
-   압축
-   접근 제어
-   로그
-   Load Balancing

## Load Balancer

Load Balancer는 여러 서버에 요청을 분산하는 기능이다.

대표 알고리즘

-   Round Robin
-   Least Connections
-   Weighted Round Robin
-   IP Hash

> Reverse Proxy는 역할(Role), Load Balancer는 기능(Function)이다.

Nginx는 Reverse Proxy이면서 Load Balancer 기능도 수행할 수 있다.

------------------------------------------------------------------------

# 🔐 TLS Termination

HTTPS는

> HTTP + TLS

이다.

요청 흐름은 다음과 같다.

``` text
URL 입력
    ↓
DNS 조회
    ↓
TCP 3-Way Handshake
    ↓
TLS Handshake (HTTPS인 경우)
    ↓
HTTP Request를 TLS로 암호화하여 전송
    ↓
Nginx
    ↓
TLS Termination
(HTTPS를 복호화하여 원래 HTTP Request 획득)
    ↓
Load Balancing
    ↓
Spring Boot
```

Nginx는 클라이언트와 TLS Handshake를 수행한 뒤 Session Key를 이용하여
HTTPS를 복호화하고 원래의 HTTP Request를 내부 Spring Boot 서버로
전달한다.

## 왜 Nginx에서 TLS를 종료하는가?

-   Spring Boot마다 TLS를 처리하지 않아도 된다.
-   인증서를 한 곳에서 관리할 수 있다.
-   TLS 연산(CPU 사용량)을 줄일 수 있다.
-   Spring Boot는 비즈니스 로직에만 집중할 수 있다.

## 내부 통신은 왜 HTTP를 사용하는가?

일반적으로 Nginx와 Spring Boot는 동일한 내부망(VPC)에서 통신하므로
HTTP를 사용하는 경우가 많다.

다만 Zero Trust 환경에서는 내부망도 신뢰하지 않기 때문에 내부도 HTTPS를
사용하는 경우가 증가하고 있다.

------------------------------------------------------------------------

# 🔍 꼬리 질문

### Q1. Proxy는 왜 필요한가?

공통 기능을 중앙에서 처리하여 서버는 비즈니스 로직에만 집중하기 위해.

### Q2. Reverse Proxy는 누구를 대신하는가?

클라이언트 입장에서 서버를 대신한다.

### Q3. Reverse Proxy와 Load Balancer의 차이는?

Reverse Proxy는 역할이고, Load Balancer는 여러 서버에 요청을 분산하는
기능이다.

### Q4. 왜 TLS를 Spring Boot가 아니라 Nginx에서 처리하는가?

Nginx에서 TLS를 종료하면 인증서 관리가 쉬워지고 TLS 연산을 한 곳에서
처리할 수 있기 때문이다.

### Q5. 내부망에서도 HTTPS를 사용하는 경우는?

Zero Trust 환경에서는 내부망도 신뢰하지 않으므로 내부 통신도 HTTPS를
사용할 수 있다.

------------------------------------------------------------------------

# 💬 면접 모범답변

Proxy는 클라이언트와 서버 사이에서 요청을 대신 처리하는 중간 계층입니다.

Forward Proxy는 클라이언트를 대신하고, Reverse Proxy는 서버를
대신합니다.

실무에서는 Nginx를 Reverse Proxy로 사용하여 TLS 종료, 캐싱, 압축, 접근
제어 등의 공통 기능을 수행합니다.

또한 Load Balancer 기능을 통해 여러 Spring Boot 서버로 요청을 분산하며,
애플리케이션 서버는 비즈니스 로직에만 집중할 수 있습니다.

------------------------------------------------------------------------

# 🔑 핵심 키워드

-   Proxy
-   Forward Proxy
-   Reverse Proxy
-   Load Balancer
-   Nginx
-   TLS Termination
-   TLS Handshake
-   Round Robin
-   Least Connections
-   Zero Trust

------------------------------------------------------------------------

# 🔗 관련 개념

## 이전 주제

➡ CORS

## 다음 주제

➡ CDN

## 함께 알아두면 좋은 개념

-   API Gateway
-   Ingress
-   Service Mesh
-   TLS Re-encryption
-   Zero Trust

------------------------------------------------------------------------

# 📌 오늘 가장 중요한 한 문장

> Reverse Proxy는 서버를 대신하여 공통 기능을 수행하고, TLS를 종료한 뒤
> 적절한 서버로 요청을 전달하는 중간 계층이다.

------------------------------------------------------------------------

# ✅ 완료 체크리스트

-   [x] 내가 먼저 답변했다.
-   [x] 피드백을 받았다.
-   [x] 원리를 이해했다.
-   [x] 꼬리 질문에 답했다.
-   [x] TLS Termination을 이해했다.
-   [x] Reverse Proxy와 Load Balancer의 관계를 이해했다.
-   [x] 면접 모범답변을 작성했다.
-   [x] 핵심 키워드를 정리했다.
-   [x] 다음 학습 주제를 정했다.
