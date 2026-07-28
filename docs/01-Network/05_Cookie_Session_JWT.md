# Day 05. Cookie / Session / JWT

> 📅 Date: 2026-07-28
>
> FullStack Interview Master - Network

---

# 🎯 오늘의 목표

- HTTP의 Stateless 특징을 이해한다.
- Cookie의 역할을 이해한다.
- Session 인증 방식을 이해한다.
- JWT 인증 방식을 이해한다.
- Session과 JWT의 차이점을 설명할 수 있다.
- Access Token / Refresh Token의 필요성을 이해한다.

---

# 1. HTTP의 Stateless

HTTP는 Stateless 프로토콜이다.

즉, 서버는 이전 요청을 기억하지 않는다.

예를 들어

```
POST /login

↓

로그인 성공

↓

GET /mypage
```

서버는

> "이 요청이 방금 로그인했던 사용자 요청인지"

알 수 없다.

따라서 사용자를 식별할 방법이 필요하다.

---

# 2. Cookie

## Cookie란?

브라우저가 데이터를 저장하는 저장소이다.

서버가

```
Set-Cookie
```

헤더를 통해 쿠키를 전달하면

브라우저는 Cookie 저장소에 저장한다.

이후 같은 서버로 요청할 때

```
Cookie
```

헤더를 통해 자동으로 전송한다.

---

## Cookie의 역할

Cookie는

- 사용자 식별 정보 전달
- 로그인 상태 유지
- 사용자 설정 저장

등에 사용된다.

예시

```
theme=dark

language=ko

JSESSIONID=abc123

remember-me=true
```

즉,

Cookie는 저장소일 뿐이며

JSESSIONID나 JWT를 저장하는 공간이다.

---

# 3. Session

## Session이란?

Session은

사용자 정보를 서버에서 관리하는 인증 방식이다.

브라우저에는

Session ID만 저장된다.

```
Browser

Cookie

↓

JSESSIONID=abc123
```

↓

```
Server

Session

abc123

↓

userId=10

role=USER
```

---

## 요청 과정

브라우저

↓

```
Cookie: JSESSIONID=abc123
```

↓

서버

↓

Session 조회

↓

사용자 인증 성공

---

## 왜 Session ID만 저장할까?

Cookie는 클라이언트에 저장되므로

사용자가 개발자 도구를 이용하여

내용을 수정할 수 있다.

만약

```
userId=1

role=ADMIN
```

을 Cookie에 저장하면

사용자가 직접 변경할 수 있다.

따라서

Cookie에는

Session ID만 저장하고

실제 사용자 정보는

서버에서 관리한다.

---

## Session의 장점

- 사용자 정보를 서버에서 관리한다.
- 권한 정보를 노출하지 않는다.
- 사용자가 Session을 위조하기 어렵다.

---

## Session의 단점

서버가 모든 사용자의 Session을 저장해야 한다.

서버가 여러 대인 경우

Session 공유(DB, Redis 등)가 필요하다.

---

# 4. JWT

## JWT란?

JWT(Json Web Token)는

사용자 정보를 토큰 안에 저장하는 인증 방식이다.

Session처럼

서버가 로그인 상태를 저장하지 않는다.

---

## JWT 구조

```
Header

Payload

Signature
```

Payload에는

```
userId

role

exp
```

등이 저장된다.

※ Payload는 암호화가 아닌 Base64 Encoding이다.

---

## Signature의 역할

Payload를 수정하면

Signature가 일치하지 않는다.

서버는

Secret Key를 이용하여

Signature를 다시 계산한다.

Signature가 다르면

위조된 토큰으로 판단한다.

따라서

Payload를 수정해도

인증되지 않는다.

---

## JWT 인증 과정

로그인

↓

JWT 발급

↓

브라우저 저장

↓

API 요청

```
Authorization: Bearer JWT
```

↓

서버

↓

Signature 검증

↓

인증 완료

---

# 5. Access Token / Refresh Token

## 왜 필요한가?

Access Token만 사용하는 경우

### 만료 시간이 길면

토큰이 탈취되면

오랫동안 사용할 수 있다.

### 만료 시간이 짧으면

사용자가 계속 로그인해야 한다.

따라서

Access Token과 Refresh Token을 함께 사용한다.

---

## Access Token

- 인증에 사용
- 짧은 만료시간
- 자주 재발급

---

## Refresh Token

- Access Token 재발급
- 긴 만료시간

클라이언트에도 저장되며

서버(DB 또는 Redis)에도 저장하여

유효성을 관리한다.

이를 통해

- 로그아웃
- 강제 만료
- 토큰 폐기

등이 가능하다.

---

# Session vs JWT

| Session | JWT |
|----------|-----|
| 서버가 사용자 정보를 저장 | 토큰이 사용자 정보를 저장 |
| Cookie에는 Session ID 저장 | Cookie 또는 LocalStorage 등에 JWT 저장 |
| Stateful | Stateless |
| Session 저장소 필요 | Signature 검증만 수행 |

---

# ⭐ 면접 핵심 질문

### Q. Cookie란?

브라우저가 데이터를 저장하고 요청마다 자동으로 서버에 전송하는 저장소이다.

---

### Q. Session이란?

Cookie에는 Session ID만 저장하고 실제 사용자 정보는 서버에서 관리하는 Stateful 인증 방식이다.

---

### Q. JWT란?

사용자 정보를 토큰에 담고 Signature를 이용하여 위변조를 검증하는 Stateless 인증 방식이다.

---

### Q. Cookie와 Session의 차이?

Cookie는 데이터를 저장하는 저장소이고,

Session은 서버에서 사용자 정보를 관리하는 인증 방식이다.

---

### Q. Session과 JWT의 차이?

Session은 서버가 로그인 상태를 저장하고,

JWT는 서버가 로그인 상태를 저장하지 않고

토큰의 Signature를 검증하여 인증한다.

---

# 📝 오늘 배운 핵심

- HTTP는 Stateless이다.
- Cookie는 브라우저 저장소이다.
- Session은 서버에서 사용자 정보를 관리한다.
- Cookie에는 Session ID만 저장한다.
- JWT는 사용자 정보를 토큰에 저장한다.
- JWT는 Signature로 위변조를 검증한다.
- Access Token은 인증에 사용한다.
- Refresh Token은 Access Token 재발급에 사용하며 서버에서도 관리한다.