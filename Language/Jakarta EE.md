# Jakarta EE 개요

## 📌 Jakarta EE란?
- 자바 기반의 **엔터프라이즈 애플리케이션 개발을 위한 오픈소스 플랫폼**
- 과거 Java EE → Jakarta EE로 명칭 변경 (2017년, Oracle → Eclipse Foundation 이관)
- **명세(specification)** 중심: 실행 코드를 제공하지 않으며, 구현체가 따로 존재함

## 🧱 Jakarta EE의 주요 명세
| 명세 이름 | 기능 |
|-----------|------|
| Jakarta Servlet | HTTP 요청/응답 처리 |
| Jakarta Persistence (JPA) | ORM 기반 데이터베이스 연동 |
| Jakarta CDI | 의존성 주입 관리 |
| Jakarta Transactions | 트랜잭션 처리 |
| Jakarta Bean Validation | 유효성 검사 |
| Jakarta RESTful Web Services (JAX-RS) | REST API 개발 |
| Jakarta Faces (JSF) | UI 컴포넌트 기반 웹 프레임워크 |
| Jakarta Mail | 이메일 처리 |

## 🛠️ Jakarta EE 구현체
- Jakarta EE 명세를 만족하는 서버 또는 런타임
- 대표 구현체:
  - Payara Server / Payara Micro
  - WildFly
  - Open Liberty
  - GlassFish

## ☁️ 클라우드 네이티브 지향
- **마이크로서비스 아키텍처** 지원
- **컨테이너 친화적**: Docker, Kubernetes와 통합 용이
- **DevOps/CI/CD** 환경에 적합
- **MicroProfile**과 연계하여 클라우드 기능 강화 (헬스 체크, 설정 관리 등)

## 🧩 Jakarta EE와 Java SE 버전 호환
| Jakarta EE 버전 | 지원 Java SE 버전 |
|------------------|--------------------|
| Jakarta EE 8     | Java 8 |
| Jakarta EE 9     | Java 8 이상 |
| Jakarta EE 9.1   | Java 11 이상 |
| Jakarta EE 10    | Java 11 이상 |
| Jakarta EE 11 (예정) | Java 17 이상 예정 |

---

# Spring과 Jakarta EE의 관계

## 🎯 Spring은 Jakarta EE 명세를 활용함
Spring은 Jakarta EE 명세를 직접 구현하지 않지만, 내부적으로 다음 명세들을 적극 활용함:

| Jakarta EE 명세 | Spring에서의 활용 |
|------------------|--------------------|
| Jakarta Persistence (JPA) | Spring Data JPA |
| Jakarta Servlet | Spring Web MVC |
| Jakarta Bean Validation | `@Valid`, `@NotNull` 등 |
| Jakarta Transactions | `@Transactional` |
| Jakarta Mail | Spring Email 모듈 |
| Jakarta CDI | Spring DI와 유사 개념 일부 반영 |

## 🔄 패키지 변화 대응
- Jakarta EE 9부터 `javax.*` → `jakarta.*`로 변경
- Spring Framework 6 / Spring Boot 3부터 이 변경을 반영
- Spring 개발자는 Jakarta EE 명세를 이해하면 **마이그레이션, 디버깅, 확장성** 측면에서 유리함

## ✅ Spring 개발자가 Jakarta EE를 알아야 하는 이유
- Spring이 Jakarta EE 명세 기반으로 동작하는 부분이 많음
- 표준 API 이해를 통해 유지보수 및 라이브러리 교체에 유리
- 최신 Spring 버전은 Jakarta EE와의 호환성이 중요해짐

---

# 📚 참고 키워드
- [[Jakarta EE]]
- [[Java EE]]
- [[JPA]]
- [[Servlet]]
- [[Spring Boot 3]]
- [[MicroProfile]]
- [[Eclipse Foundation]]