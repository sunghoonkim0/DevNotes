---
tags: [jwt, security, springboot, authentication]
---

# 🔐 JWT (JSON Web Token) 정리

## ✅ 개요
- JWT는 JSON 기반의 인증 토큰으로, **서버가 상태를 저장하지 않고도 인증 정보를 유지**할 수 있게 해주는 **표준(Open Standard, RFC 7519)** 기술입니다.
- 주로 **인증(Authentication)**, **인가(Authorization)**, **정보 교환**에 사용됩니다.

---

## 🧩 구성 요소

JWT는 세 부분으로 구성되며, `.`으로 구분됩니다:

1. **Header**
   - 토큰의 타입과 서명 알고리즘 정보
   - 예시:
     ```json
     {
       "alg": "HS256",
       "typ": "JWT"
     }
     ```

2. **Payload**
   - 사용자 정보 및 클레임(Claims)
   - 클레임 종류:
     - 등록된 클레임: `iss`, `exp`, `sub`, `aud` 등
     - 공개 클레임: 표준화된 이름을 가진 클레임
     - 비공개 클레임: 사용자 정의 정보
   - 예시:
     ```json
     {
       "sub": "user123",
       "name": "dolsan",
       "exp": 1700000000
     }
     ```

3. **Signature**
   - 위변조 방지를 위한 서명
   - 생성 방식:
     ```
     HMACSHA256(
       base64UrlEncode(header) + "." + base64UrlEncode(payload),
       secret
     )
     ```

---

## ⚙️ Spring Boot에서 JWT 적용 방식

### 1. Spring Security 기반
- `UserDetailsService`, `JWTAuthenticationFilter`, `SecurityConfig` 등 구성
- 인증 후 JWT 발급 → 요청마다 검증
- 권한별 접근 제어 가능

### 2. 커스텀 필터 방식
- Spring Security 없이 필터 직접 구현
- 구조가 단순하고 빠르게 적용 가능
- 학습용/소규모 프로젝트에 적합

### 3. WebFlux 기반
- `SecurityWebFilterChain`, `ReactiveAuthenticationManager` 등 사용
- 논블로킹 방식 → 고성능 API에 적합
- 기존 MVC 방식과 구조 다름

---

## 📌 JWT 활용 체크리스트

- [x] 토큰 저장 위치: `Authorization: Bearer <token>`
- [x] 만료 시간 설정 (`exp`)
- [x] Refresh Token 사용 여부
- [x] 사용자 인증 → 토큰 발급
- [x] 인가 처리 → 권한 기반 접근 제어
- [x] 예외 처리 (만료, 위변조, 미인증 등)

---

## 🛡️ 보안 주의사항

- HTTPS 필수
- 민감 정보는 Payload에 담지 말 것
- 짧은 만료 시간 설정
- 서명 키는 안전하게 관리
- 로그아웃 시 토큰 무효화 전략 필요 (ex. 블랙리스트)

---

## 📚 참고 자료

- [RFC 7519 - JWT 공식 문서](https://tools.ietf.org/html/rfc7519)
- [Spring Security + JWT 실습 예제](https://velog.io/@soyeon207/JWT-%EC%8B%A4%EC%8A%B5)
- [Spring WebFlux + JWT 인증 예제](https://velog.io/@kinggusrl3/Spring-WebFlux-JWT-%EC%9D%B8%EC%A6%9D-%EA%B5%AC%ED%98%84)

---

## 🧠 추가로 알아두면 좋은 개념

- OAuth2 + JWT 연동
- JWE (JWT 암호화 버전)
- 토큰 블랙리스트 전략
- JWT 관련 라이브러리: `jjwt`, `auth0`, `nimbus-jose-jwt` 등