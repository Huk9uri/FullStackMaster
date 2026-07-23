
# Day 03 - TCP 3-Way Handshake가 필요한 이유

  

> 웹 · 네트워크

  

---

  

# 📅 기본 정보

  

-  **Day**: 03

-  **날짜**: 2026-07-23

-  **카테고리**: Web / Network

-  **주제**: TCP 3-Way Handshake가 필요한 이유

-  **난이도**: ★★★☆☆

-  **예상 학습 시간**: 1시간

-  **완료 여부**: ✅

  

---

  

# 🔁 복습

  

## 이전 학습 내용 복습 질문

  

### Q1. HTTP는 왜 안전하지 않은가?

  

### 내 답변

  

HTTP는 평문을 암호화하지 못한다.

  

따라서 기밀성과 무결성을 보장하지 못하며, 같은 네트워크의 공격자가 패킷을 가로채면 내부 정보를 그대로 확인할 수 있다.

  

### 피드백

  

HTTP는 기밀성과 무결성뿐 아니라 서버 인증도 제공하지 않는다.

  

HTTPS는 TLS를 통해 다음을 제공한다.

  

- 기밀성(Confidentiality)

- 무결성(Integrity)

- 서버 인증(Authentication)

  

---

  

### Q2. HTTPS에서 암호화되는 정보와 암호화되지 않는 정보는?

  

### 내 답변

  

TLS보다 먼저 TCP 연결이 진행되고, TCP 연결에 사용할 IP 주소를 찾기 위해 DNS 조회가 수행된다.

  

따라서 일반 DNS 조회에 사용되는 Domain과 목적지 IP 주소는 TLS로 암호화되지 않는다.

  

반면 URL Path는 HTTP Request에 포함되므로 TLS 연결 이후 암호화된다.

  

### 피드백

  

HTTP Request에 포함되는 다음 정보는 TLS로 암호화된다.

  

- HTTP Method

- URL Path

- Query String

- Header

- Body

  

TLS 연결 이전과 패킷 전달에 필요한 다음 정보는 노출될 수 있다.

  

- 일반 DNS 조회에 사용되는 Domain

- 목적지 IP 주소

- Port 번호

  

---

  

### Q3. TLS는 어디에서 동작하는가?

  

### 내 답변

  

TLS는 라우터에서 진행된다.

  

### 피드백

  

TLS는 라우터가 아니라 클라이언트와 서버의 종단에서 수행된다.

  

라우터는 IP 주소를 기준으로 패킷을 다음 경로에 전달할 뿐, 암호화된 HTTP 내용을 해석하지 않는다.

  

```

브라우저

  

↓ DNS 조회

  

↓ TCP 3-Way Handshake

  

↓ TLS Handshake

  

↓ 암호화된 HTTP 통신

  

서버

```

  

---

  

# 🎤 오늘의 면접 질문

  

> TCP는 왜 3-Way Handshake를 사용할까요? 2번도 아니고 4번도 아닌 이유는 무엇인가요?

  

---

  

# ✍️ 내가 먼저 답변

  

TCP는 클라이언트와 서버가 연결을 시도하는 프로토콜이다.

  

우선 클라이언트가 서버에 동기화 요청인 SYN을 보낸다.

  

서버는 SYN에 대한 ACK 확인 신호를 보내고, TCP는 양방향 통신이 가능한 전이중 통신이기 때문에 ACK와 함께 서버 자신의 SYN도 클라이언트에게 보낸다.

  

마지막으로 클라이언트가 서버의 SYN에 대한 ACK를 보내면서 3-Way Handshake가 완료된다.

  

---

  

# 📊 답변 평가

  

## 점수

  

**90 / 100**

  

### ✅ 맞은 부분

  

- SYN → SYN + ACK → ACK의 순서를 정확히 설명했다.

- SYN이 연결을 시작하기 위한 동기화 요청이라는 점을 알고 있었다.

- ACK가 상대의 요청을 수신했다는 확인이라는 점을 알고 있었다.

- TCP가 전이중 통신이므로 서버도 자신의 SYN을 보내야 한다는 점을 설명했다.

- 단순 암기가 아니라 양방향 연결이라는 이유와 Handshake 흐름을 연결했다.

  

### ⚠️ 부족한 부분

  

- TCP를 단순히 연결을 시도하는 프로토콜이라고 표현했다.

- TCP가 신뢰성 있는 바이트 스트림 전송을 제공하는 전송 계층 프로토콜이라는 설명이 필요했다.

- 왜 2-Way Handshake로는 충분하지 않은지 최초 답변에 포함되지 않았다.

- SYN과 ACK가 Sequence Number를 기준으로 동작한다는 설명이 필요했다.

  

### ❌ 틀린 부분

  

- 큰 흐름에서 틀린 부분은 없었다.

- 다만 TCP와 HTTP의 역할을 구분하는 꼬리 질문에서 HTTP를 전송 프로토콜이라고 표현한 부분은 수정이 필요했다.

  

---

  

# 🧠 핵심 원리

  

## 한 줄 정의

  

> TCP 3-Way Handshake는 클라이언트와 서버가 서로의 송신·수신 가능 여부와 초기 Sequence Number를 확인하여 신뢰성 있는 양방향 연결을 설정하는 과정이다.

  

---

  

## TCP란?

  

TCP는 전송 계층에서 동작하며, 애플리케이션이 생성한 데이터를 신뢰성 있는 바이트 스트림으로 전달한다.

  

TCP가 제공하는 대표적인 기능은 다음과 같다.

  

- 연결 지향 통신

- 데이터 순서 보장

- 데이터 유실 감지 및 재전송

- 중복 데이터 처리

- 흐름 제어

- 혼잡 제어

- 전이중 통신

  

TCP는 전달되는 데이터가 로그인 요청인지, 이미지인지, JSON인지 이해하지 않는다.

  

TCP는 데이터를 바이트의 연속으로 보고 신뢰성 있게 전달하는 역할을 담당한다.

  

---

  

## 3-Way Handshake 동작 과정

  

### 1단계: Client → Server

  

```text

SYN

Seq = x

```

  

클라이언트가 서버에 연결을 요청한다.

  

클라이언트는 자신의 초기 Sequence Number인 `x`를 함께 전달한다.

  

클라이언트 상태는 일반적으로 다음과 같이 변경된다.

  

```text

CLOSED → SYN_SENT

```

  

---

  

### 2단계: Server → Client

  

```text

SYN + ACK

Seq = y

Ack = x + 1

```

  

서버는 클라이언트의 SYN을 정상적으로 받았다는 ACK를 보낸다.

  

동시에 서버도 양방향 통신을 위해 자신의 초기 Sequence Number인 `y`를 담은 SYN을 보낸다.

  

`Ack = x + 1`의 의미는 다음과 같다.

  

> 클라이언트가 보낸 Sequence Number x를 확인했으며, 다음 번호인 x + 1을 기대한다.

  

서버 상태는 일반적으로 다음과 같이 변경된다.

  

```text

LISTEN → SYN_RECEIVED

```

  

---

  

### 3단계: Client → Server

  

```text

ACK

Seq = x + 1

Ack = y + 1

```

  

클라이언트는 서버의 SYN을 정상적으로 받았다는 ACK를 보낸다.

  

이를 통해 서버는 클라이언트가 서버의 응답을 정상적으로 수신할 수 있다는 사실까지 확인한다.

  

이후 양측은 `ESTABLISHED` 상태가 되고 데이터를 주고받을 수 있다.

  

```text

Client: ESTABLISHED

Server: ESTABLISHED

```

  

---

  

## 전체 흐름

  

```text

Client Server

  

| -------- SYN, Seq = x ------------> |

| |

| <--- SYN + ACK, Seq = y, Ack=x+1 -- |

| |

| -------- ACK, Ack = y+1 ----------> |

| |

| Connection Established |

```

  

---

  

## 왜 2-Way Handshake로는 부족한가?

  

2-Way Handshake를 사용한다고 가정하면 다음 단계에서 연결이 끝난다.

  

```text

Client → Server : SYN

Server → Client : SYN + ACK

```

  

서버가 보낸 `SYN + ACK`가 네트워크에서 유실되면 상태가 달라질 수 있다.

  

```text

Client

"서버 응답을 받지 못했으므로 연결되지 않았다."

  

Server

"SYN + ACK를 보냈으므로 연결되었다."

```

  

서버는 클라이언트가 자신의 SYN을 실제로 수신했는지 확인하지 못한다.

  

따라서 마지막 ACK가 있어야 서버가 다음 사실을 확인할 수 있다.

  

> 클라이언트가 서버의 응답과 초기 Sequence Number를 정상적으로 수신했다.

  

즉 3-Way Handshake는 양측의 송신·수신 가능 여부를 모두 확인하기 위한 최소 과정이다.

  

---

  

## 왜 4-Way가 아니라 3-Way인가?

  

개념적으로는 다음과 같이 4번 전송할 수도 있다.

  

```text

1. Client → Server : SYN

2. Server → Client : ACK

3. Server → Client : SYN

4. Client → Server : ACK

```

  

하지만 서버가 보내는 ACK와 SYN은 같은 시점에 하나의 TCP 세그먼트로 합칠 수 있다.

  

```text

SYN + ACK

```

  

따라서 연결 설정은 3번의 통신으로 충분하다.

  

반면 TCP 연결 종료에서는 한쪽이 FIN을 보냈더라도 상대방에게 남은 데이터가 있을 수 있으므로 ACK와 FIN이 분리될 수 있다. 그래서 일반적으로 4-Way Handshake를 사용한다.

  

---

  

## Sequence Number가 필요한 이유

  

### 내 답변

  

ACK가 어느 SYN 패킷에 대한 확인 신호인지 알기 위한 구분자이다.

  

### 피드백

  

맞는 설명이지만 Sequence Number의 역할은 더 넓다.

  

Sequence Number는 TCP가 바이트의 순서와 수신 상태를 관리하기 위해 사용한다.

  

이를 통해 다음 작업을 수행할 수 있다.

  

- 수신 데이터의 순서 재조립

- 데이터 유실 감지

- 중복 데이터 구분

- 재전송할 데이터 판단

- ACK가 확인하는 데이터 범위 판단

  

ACK Number는 일반적으로 다음에 받기를 기대하는 Sequence Number를 의미한다.

  

예를 들어 다음과 같다.

  

```text

Client → Server

SYN, Seq = 100

  

Server → Client

SYN + ACK, Seq = 500, Ack = 101

```

  

`Ack = 101`은 다음 의미를 가진다.

  

> Sequence Number 100인 SYN을 정상적으로 받았으며, 다음에는 101부터 받기를 기대한다.

  

---

  

## TCP와 HTTP의 역할 차이

  

### 꼬리 질문

  

> TCP가 데이터의 순서를 보장하는데, HTTP에는 왜 Request와 Response 구조가 필요한가?

  

### 내 답변

  

TCP는 신뢰성 있는 연결을 보장하는 프로토콜이고, HTTP는 전송 프로토콜이다.

  

TCP와 HTTP는 별개의 프로토콜이므로 HTTP를 기반으로 데이터를 송수신하려면 HTTP의 구조를 따라야 한다.

  

### 피드백

  

핵심 방향은 맞지만 HTTP는 전송 계층 프로토콜이 아니다.

  

- HTTP: 애플리케이션 계층 프로토콜

- TCP: 전송 계층 프로토콜

- IP: 인터넷 계층 프로토콜

  

TCP는 바이트를 순서대로 신뢰성 있게 전달하지만, 해당 바이트의 의미는 이해하지 못한다.

  

HTTP는 전달되는 바이트가 어떤 요청이며 어떤 응답인지 해석할 수 있도록 메시지 형식을 정의한다.

  

예를 들어 다음 HTTP 메시지는 TCP 입장에서는 단순한 바이트 배열이다.

  

```http

GET /api/wish HTTP/1.1

Host: example.com

Authorization: Bearer token

```

  

그러나 HTTP 규약을 따르는 Spring 서버는 다음 정보를 해석할 수 있다.

  

- Method: GET

- Path: /api/wish

- Host

- Authorization Header

  

즉 TCP는 전달을 담당하고, HTTP는 전달되는 데이터의 의미와 형식을 정의한다.

  

---

  

# 🔍 꼬리 질문

  

## Q1. SYN은 무엇인가?

  

### 내 답변

  

연결을 시작하고 Sequence Number를 동기화하기 위한 요청이다.

  

---

  

## Q2. ACK는 무엇인가?

  

### 내 답변

  

상대가 보낸 TCP 세그먼트를 정상적으로 받았음을 확인하는 신호이다.

  

ACK Number에는 일반적으로 다음에 받기를 기대하는 Sequence Number가 담긴다.

  

---

  

## Q3. 왜 2-Way Handshake는 사용할 수 없는가?

  

### 내 답변

  

2-Way만으로는 서버가 보낸 SYN을 클라이언트가 정상적으로 수신했는지 서버가 확인할 수 없다.

  

서버와 클라이언트의 연결 상태가 다르게 판단될 수 있으므로 마지막 ACK가 필요하다.

  

---

  

## Q4. 왜 4-Way가 아니라 3-Way인가?

  

### 내 답변

  

서버가 클라이언트의 SYN을 확인하는 ACK와 서버 자신의 SYN을 하나의 TCP 세그먼트에 함께 담을 수 있기 때문이다.

  

---

  

## Q5. Sequence Number는 왜 필요한가?

  

### 내 답변

  

TCP가 데이터의 순서를 재조립하고 유실이나 중복을 감지하며, ACK가 어느 데이터까지 수신했는지 표현하기 위해 필요하다.

  

---

  

## Q6. TCP와 HTTP는 무엇이 다른가?

  

### 내 답변

  

TCP는 전송 계층에서 바이트를 순서대로 신뢰성 있게 전달한다.

  

HTTP는 애플리케이션 계층에서 Request와 Response의 구조 및 의미를 정의한다.

  

---

  

## Q7. SYN도 Sequence Number를 1만큼 소비하는가?

  

### 답변

  

그렇다.

  

SYN과 FIN은 실제 애플리케이션 데이터가 없어도 Sequence Number 공간을 1만큼 사용한다.

  

그래서 `Seq = x`인 SYN에 대한 ACK는 일반적으로 `Ack = x + 1`이 된다.

  

---

  

# 🛠 실무 연결

  

## React → Spring Boot API 요청

  

React 애플리케이션에서 Axios로 Spring Boot API를 호출한다고 가정한다.

  

```typescript

axios.get('/api/wish');

```

  

애플리케이션 관점에서는 한 줄의 API 요청이지만, 네트워크에서는 다음 과정이 필요할 수 있다.

  

```text

React / Browser

  

↓ DNS 조회

  

↓ TCP 3-Way Handshake

  

↓ TLS Handshake (HTTPS)

  

↓ HTTP Request

  

↓ Spring Boot Controller 처리

  

↓ HTTP Response

```

  

TCP 연결은 브라우저나 운영체제의 네트워크 스택이 관리한다.

  

Spring Boot의 Controller가 직접 3-Way Handshake를 구현하는 것은 아니다.

  

Spring Boot 애플리케이션은 이미 연결된 소켓을 통해 들어온 HTTP 요청을 Tomcat 등의 웹 서버로부터 전달받는다.

  

---

  

## Keep-Alive와 Connection 재사용

  

API 요청마다 항상 새로운 TCP 연결을 생성하면 다음 비용이 반복된다.

  

- TCP 3-Way Handshake

- HTTPS인 경우 TLS Handshake

- 네트워크 왕복 시간 증가

  

HTTP Keep-Alive와 커넥션 재사용을 사용하면 기존 TCP 연결에서 여러 HTTP 요청과 응답을 처리할 수 있다.

  

따라서 실무에서는 TCP 연결 횟수를 줄이는 것이 API 응답 성능에 영향을 줄 수 있다.

  

---

  

## Connection Pool과의 연결

  

백엔드에서 사용하는 DB Connection Pool도 연결을 매번 새로 생성하지 않고 재사용한다는 점에서 비슷한 목적을 가진다.

  

다만 다음 연결은 서로 다른 종류이다.

  

- 브라우저 ↔ Spring 서버: TCP 연결

- Spring 서버 ↔ MySQL: DB 프로토콜을 사용하는 TCP 연결

  

HikariCP는 MySQL과의 연결을 미리 생성하고 재사용하여 연결 설정 비용을 줄인다.

  

---


# 💬 면접 모범답변

  

## 1분 답변

  

TCP는 신뢰성 있는 양방향 연결을 설정하기 위해 3-Way Handshake를 수행합니다.

  

먼저 클라이언트가 자신의 초기 Sequence Number를 담은 SYN을 보내 연결을 요청합니다. 서버는 이를 정상적으로 받았다는 ACK와 서버 자신의 초기 Sequence Number를 담은 SYN을 함께 보냅니다. 마지막으로 클라이언트가 서버의 SYN에 대한 ACK를 보내면 양측이 서로의 송신과 수신이 가능하고 초기 Sequence Number를 확인했다는 것이 검증됩니다.

  

2-Way Handshake만 사용하면 서버가 보낸 SYN을 클라이언트가 실제로 받았는지 서버가 확인할 수 없어 양측의 연결 상태가 달라질 수 있습니다. 반대로 ACK와 SYN은 하나의 세그먼트로 함께 보낼 수 있으므로 연결 설정에 4번까지 전송할 필요는 없습니다.

  

따라서 3-Way Handshake는 신뢰성 있는 양방향 TCP 연결을 설정하기 위한 최소한의 과정입니다.

  

---

  

## 30초 답변

  

TCP 3-Way Handshake는 양측의 송수신 가능 여부와 초기 Sequence Number를 확인하기 위한 과정입니다.

  

클라이언트가 SYN을 보내면 서버는 SYN과 ACK로 응답하고, 클라이언트가 마지막 ACK를 보냅니다.

  

2-Way만으로는 서버가 보낸 응답을 클라이언트가 받았는지 서버가 알 수 없고, 서버의 SYN과 ACK는 함께 보낼 수 있으므로 4-Way까지는 필요하지 않습니다.

  

---

  

# 🔑 핵심 키워드

  

- TCP

- Transport Layer

- Connection-Oriented

- 3-Way Handshake

- SYN

- ACK

- Sequence Number

- Acknowledgment Number

- Full Duplex

- ESTABLISHED

- SYN_SENT

- SYN_RECEIVED

- Half-Open Connection

- Reliable Byte Stream

- HTTP

- Application Layer

- Keep-Alive

  

---

  

# 🔗 관련 개념

  

## 이전 주제

  

➡ HTTP와 HTTPS의 차이

  

## 다음 주제

  

➡ HTTP/1.1 vs HTTP/2 vs HTTP/3

  

## 함께 알아두면 좋은 개념

  

- TCP 4-Way Handshake

- TIME_WAIT

- Flow Control

- Congestion Control

- Sliding Window

- Retransmission

- RTT

- TCP Keep-Alive

- HTTP Keep-Alive

- QUIC

  

---

  

# 📌 오늘 가장 중요한 한 문장

  

> TCP 3-Way Handshake는 양측의 송수신 가능 여부와 초기 Sequence Number를 모두 확인하여 신뢰성 있는 양방향 연결을 설정하는 최소 과정이다.

  

---

  

# ✅ 완료 체크리스트

  

- [x] 이전 주제를 복습했다.

- [x] 내가 먼저 답변했다.

- [x] 피드백을 받았다.

- [x] 3-Way Handshake의 흐름을 이해했다.

- [x] 2-Way로 부족한 이유를 이해했다.

- [x] 4-Way가 필요하지 않은 이유를 이해했다.

- [x] Sequence Number와 ACK의 관계를 이해했다.

- [x] TCP와 HTTP의 역할을 구분했다.

- [x] 프로젝트와 연결했다.

- [x] 면접 모범답변을 작성했다.

- [x] 핵심 키워드를 정리했다.

- [x] 다음 학습 주제를 정했다.