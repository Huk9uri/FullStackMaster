# Day 04 - HTTP/1.1 vs HTTP/2 vs HTTP/3

> 웹 · 네트워크

---

# 📅 기본 정보

- **Day**: 04
- **날짜**: 2026-07-24
- **카테고리**: Web / Network
- **주제**: HTTP/1.1 vs HTTP/2 vs HTTP/3
- **난이도**: ★★★★☆
- **완료 여부**: ✅

---

# 🔁 복습

## Q1. TCP 3-Way Handshake를 하는 이유는?

양방향 송수신이 가능한지 확인하고 초기 Sequence Number를 동기화하여 신뢰성 있는 연결을 생성하기 위해 수행한다.

## Q2. TLS Handshake는 언제 수행되는가?

TCP 연결 이후, HTTP 데이터를 전송하기 전에 수행한다.

## Q3. HTTPS에서도 암호화되지 않는 정보는?

- Domain
- 목적지 IP
- Port

Path, Query String, Header, Body는 TLS로 암호화된다.

## Q4. 왜 TCP 종료는 4-Way Handshake인가?

TCP는 Full Duplex 통신이므로 양방향 연결을 각각 종료한다. 서버가 아직 보낼 데이터가 남아 있을 수 있으므로 ACK와 FIN을 분리한다.

---

# 🎤 오늘의 면접 질문

> HTTP/1.1, HTTP/2, HTTP/3의 차이를 설명해주세요.

---

# ✍️ 내가 먼저 답변

HTTP/1.1은 요청을 순차적으로 처리하여 HOL Blocking 문제가 있다.

HTTP/2는 Multiplexing을 통해 여러 요청을 동시에 처리하지만 TCP 기반이라 TCP HOL Blocking이 남아있다.

HTTP/3는 UDP 기반 QUIC을 사용하여 Stream별로 독립적으로 데이터를 관리하고 TCP HOL Blocking을 해결한다.

---

# 📊 답변 평가

## 점수

**95 / 100**

### ✅ 맞은 부분

- HTTP 버전 발전 이유 이해
- HOL Blocking 설명
- Multiplexing 이해
- QUIC 역할 이해

### ⚠️ 보완할 부분

- HTTP/1.1은 브라우저가 여러 TCP 연결을 열어 병렬 다운로드를 수행한다는 점
- HTTP/2는 요청 하나를 여러 Frame으로 분할하여 Stream 단위로 전송한다는 점

---

# 🧠 핵심 원리

## HTTP/1.1

기본적으로 하나의 TCP 연결에서 요청과 응답을 순차적으로 처리한다.

```
Request A
↓
Response A
↓
Request B
↓
Response B
```

브라우저는 성능 향상을 위해 동일 서버에 여러 TCP 연결(일반적으로 6개 내외)을 생성하여 병렬 다운로드를 수행한다.

### 문제점

- TCP 연결 여러 개 생성
- TCP/TLS Handshake 반복
- Slow Start 반복
- 혼잡 제어, 흐름 제어, 버퍼를 연결마다 관리

---

## Keep-Alive

TCP 연결을 종료하지 않고 재사용한다.

```
TCP 연결
↓
Request A
↓
Response A
↓
Request B
↓
Response B
```

---

## Pipelining

응답을 기다리지 않고 여러 요청을 연속 전송한다.

```
GET A
GET B
GET C
```

하지만 응답은 반드시

```
A
↓
B
↓
C
```

순서대로 와야 하므로 HOL Blocking이 발생한다.

---

## HTTP/2

HTTP 메시지를 여러 개의 Frame으로 분할하고, 하나의 TCP 연결에서 여러 Stream의 Frame을 섞어서(interleave) 전송한다.

```
Stream1 : A1 A2 A3
Stream3 : B1 B2
Stream5 : C1 C2

전송

A1
B1
C1
A2
C2
B2
A3
```

### 장점

- TCP 연결 1개
- Handshake 1번
- 혼잡 제어 1번
- Multiplexing 지원

### 한계

TCP는 바이트 순서를 보장한다.

따라서 하나의 패킷(Frame이 포함된 TCP 데이터)이 유실되면 이후 데이터도 재전송 완료까지 애플리케이션으로 전달되지 않는다.

이를 TCP 레벨 HOL Blocking이라 한다.

---

## HTTP/3

HTTP/3는 UDP 위에서 QUIC 프로토콜을 사용한다.

QUIC은

- 재전송
- 순서 보장
- 혼잡 제어
- 흐름 제어

를 UDP 위에서 직접 구현한다.

또한 Stream별로 독립적으로 관리하므로 한 Stream의 데이터가 유실되어도 다른 Stream은 계속 처리된다.

---

# 🔍 꼬리 질문

### Q1. HTTP/2가 있는데 왜 HTTP/3가 필요한가?

TCP 레벨 HOL Blocking을 해결하기 위해서이다.

### Q2. Keep-Alive와 Pipelining의 차이는?

- Keep-Alive : TCP 연결 재사용
- Pipelining : 여러 HTTP 요청을 연속 전송

### Q3. Multiplexing과 Pipelining의 차이는?

Pipelining은 응답 순서를 바꿀 수 없지만, Multiplexing은 Frame을 Stream 단위로 섞어 전송한다.

### Q4. HTTP/3는 UDP를 사용하는데 왜 신뢰성이 있는가?

QUIC이 UDP 위에서 재전송, 순서 보장 등을 직접 구현하기 때문이다.

---

# 🛠 프로젝트 연결

React는 HTML, JS, CSS, 이미지 등을 동시에 요청한다.

HTTP/2에서는 하나의 TCP 연결에서 Multiplexing으로 병렬 다운로드가 가능하여 초기 렌더링 속도가 향상된다.

Spring Boot는

```properties
server.http2.enabled=true
```

설정을 통해 HTTP/2를 사용할 수 있으며, 실무에서는 Nginx + HTTP/2 + Spring Boot 구성이 많이 사용된다.

---

# 💬 면접 모범답변

HTTP/1.1은 하나의 TCP 연결에서 요청과 응답을 순차적으로 처리하기 때문에 HOL Blocking 문제가 있습니다. 이를 완화하기 위해 브라우저는 여러 TCP 연결을 생성하지만 각 연결마다 Handshake와 혼잡 제어를 수행해야 하므로 비효율적입니다.

HTTP/2는 하나의 TCP 연결에서 HTTP 메시지를 여러 Frame으로 분할하고 Stream 단위로 Multiplexing하여 병렬 처리합니다. 하지만 TCP의 순서 보장 특성 때문에 TCP 레벨 HOL Blocking은 여전히 존재합니다.

HTTP/3는 UDP 기반 QUIC을 사용하며, QUIC이 재전송과 혼잡 제어를 직접 구현하고 Stream별로 독립적으로 데이터를 관리하여 TCP HOL Blocking을 해결합니다.

---

# 🔑 핵심 키워드

- HTTP/1.1
- Keep-Alive
- Pipelining
- HTTP/2
- Multiplexing
- Stream
- Frame
- TCP HOL Blocking
- HTTP/3
- QUIC
- UDP

---

# 📌 오늘 가장 중요한 한 문장

> HTTP/2는 요청 단위 HOL Blocking을 해결했고, HTTP/3는 TCP HOL Blocking까지 해결하였다.

---

# ✅ 완료 체크리스트

- [x] HTTP/1.1 한계 이해
- [x] Keep-Alive 이해
- [x] Pipelining 이해
- [x] Multiplexing 이해
- [x] Stream과 Frame 이해
- [x] TCP HOL Blocking 이해
- [x] QUIC 이해
- [x] 면접 답변 작성
