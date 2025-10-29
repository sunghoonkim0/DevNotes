# Spring WebFlux

## 🌊 개요
[[Spring WebFlux]]는 [[Spring Framework 5]]에서 도입된 **리액티브 프로그래밍 기반의 웹 프레임워크**입니다. 기존 [[Spring MVC]]가 [[Servlet API]] 기반의 동기/블로킹 모델인 반면, WebFlux는 **논블로킹(Non-blocking)** 방식으로 높은 동시성과 성능을 제공합니다.

## 🚀 주요 특징

- **Reactive Streams 기반**: [[Project Reactor]]를 기반으로 하며, [[Reactive Streams]] 사양을 따릅니다.
- **논블로킹 I/O**: 요청-응답 처리 시 스레드를 점유하지 않고, 이벤트 기반으로 처리
- **Netty 지원**: 기본적으로 [[Netty]]를 내장 서버로 사용하지만, [[Servlet 3.1]] 이상을 지원하는 컨테이너에서도 실행 가능
- **함수형 라우팅 지원**: 어노테이션 기반(@RestController)뿐 아니라 함수형 스타일의 라우팅도 지원
- **Backpressure 지원**: 데이터 흐름 제어를 통해 시스템 과부하 방지

## 🧱 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| `Mono<T>` | 0 또는 1개의 데이터를 비동기적으로 처리 |
| `Flux<T>` | 0개 이상의 데이터를 스트림으로 처리 |
| `WebClient` | [[RestTemplate]]을 대체하는 논블로킹 HTTP 클라이언트 |
| `RouterFunction` | 함수형 라우팅 정의 |
| `HandlerFunction` | 요청을 처리하는 함수형 핸들러 |

## 🔄 WebFlux vs Spring MVC

| 항목 | Spring WebFlux | Spring MVC |
|------|----------------|------------|
| 처리 방식 | 논블로킹 (Reactive) | 블로킹 (Servlet 기반) |
| 스레드 모델 | 이벤트 루프 기반 | 요청당 스레드 할당 |
| 성능 | 고부하 환경에 유리 | 단순 요청에 적합 |
| 주요 API | `Mono`, `Flux`, `WebClient` | `RestTemplate`, `@Controller` |
| 서버 지원 | Netty, Undertow, Servlet 3.1+ | Servlet 3.0+ |

## 🧩 Jakarta EE와의 관계

- WebFlux는 **[[Jakarta EE]] 명세와 직접적인 연관은 없음**
- [[Servlet API]]를 사용하지 않고, 자체적인 논블로킹 모델을 사용
- Jakarta EE는 주로 동기/블로킹 기반 명세를 제공하며, WebFlux는 **Spring 독자 기술**

## ✅ 사용 시기

- 고성능, 고동시성 환경 (예: 채팅, 스트리밍, IoT)
- 비동기 API 호출이 많은 마이크로서비스 환경
- 리액티브 데이터베이스([[R2DBC]] 등)와 함께 사용할 때

---

# 🧠 WebFlux 실전 고찰

## 🔍 WebFlux의 장점이 제한되는 경우

- 대부분의 비즈니스 로직은 [[JDBC]] 기반의 블로킹 DB를 사용
- DB 수정이 발생하는 [[Create]], [[Update]] 작업은 블로킹 처리로 인해 WebFlux의 논블로킹 모델이 무력화됨
- 외부 API가 블로킹 방식일 경우에도 병목 발생 가능

## 🧱 WebFlux와 블로킹 DB의 병존 전략

- `Mono.fromCallable(...)` + `Schedulers.boundedElastic()`으로 블로킹 작업을 별도 스레드 풀에서 처리
- 완전한 논블로킹을 원한다면 [[R2DBC]] 같은 리액티브 DB 드라이버 사용 필요

## 📉 기술 복잡도 vs 실익

| 항목 | WebFlux | Spring MVC |
|------|---------|------------|
| 개발 난이도 | 높음 | 낮음 |
| 디버깅/운영 | 복잡함 | 쉬움 |
| 트랜잭션 처리 | 제한적 (R2DBC) | 안정적 (JDBC) |
| 레거시 호환성 | 낮음 | 높음 |
| 실시간/스트리밍 | 최적화됨 | 제한적 |
| 단순 CRUD | 과도한 복잡도 | 적합 |

## 💸 서버 비용 관점

- WebFlux는 적은 스레드로 많은 요청을 처리할 수 있어 **고동시성 환경에서 서버 비용 절감 가능**
- 하지만 블로킹 DB를 사용하면 이점이 줄어들며, MVC와 큰 차이가 없을 수 있음

## ✅ 결론

> WebFlux는 **특정 환경에서만 빛나는 기술**입니다.  
> 전체 아키텍처가 논블로킹으로 설계되어야 진가를 발휘하며, 그렇지 않다면 **Spring MVC가 더 실용적이고 안정적인 선택**일 수 있습니다.

---

## 📚 확장 키워드
- [[Reactive Streams]]
- [[R2DBC]]
- [[JDBC]]
- [[Schedulers.boundedElastic]]
- [[Backpressure]]
- [[Mono.fromCallable]]
- [[Spring Observability]]
- [[Micrometer]]
- [[Sentry]]
- [[New Relic]]