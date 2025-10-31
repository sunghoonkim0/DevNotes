---
tags: [spring, spring-boot, controller, parameter-mapping, validation]
status: done
created: 2025-10-31
updated: 2025-10-31
---

# Spring Boot 컨트롤러 파라미터 매핑

> Spring Boot에서는 HTTP 요청의 다양한 요소(Request Body, Query Parameter, Path Variable 등)를 컨트롤러 메서드의 파라미터로 매핑할 수 있습니다.

## 목차
- [[#주요 매핑 어노테이션]]
- [[#매핑 방식별 예제]]
- [[#검증(Validation)]]
- [[#예외 처리(Exception Handling)]]
- [[#확장 가능한 개념]]

## 주요 매핑 어노테이션

| 구분 | 어노테이션 / 객체 | 설명 |
|------|-------------------|------|
| Request Body | `@RequestBody` | 요청 본문(JSON 등)을 객체로 매핑 |
| Query Parameter | `@RequestParam` | 쿼리 파라미터를 매핑 |
| Path Variable | `@PathVariable` | URL 경로의 일부를 매핑 |
| Header | `@RequestHeader` | 요청 헤더를 매핑 |
| Cookie | `@CookieValue` | 쿠키 값을 매핑 |
| Form Data | `@ModelAttribute` | 폼 데이터를 객체로 매핑 |
| HttpServletRequest | `HttpServletRequest` | 서블릿 요청 객체 직접 사용 |
| HttpServletResponse | `HttpServletResponse` | 서블릿 응답 객체 직접 사용 |

## 매핑 방식별 예제

### 1. Request Body (@RequestBody)
```java
@PostMapping("/users")
public String createUser(@RequestBody UserDto user) {
    return "Created user: " + user.getName();
}
```

### 2. Query Parameter (@RequestParam)
```java
@GetMapping("/search")
public String search(@RequestParam String keyword,
                     @RequestParam(defaultValue = "1") int page) {
    return "Search keyword: " + keyword + ", page: " + page;
}
```

### 3. Path Variable (@PathVariable)
```java
@GetMapping("/users/{id}")
public String getUser(@PathVariable Long id) {
    return "User ID: " + id;
}
```

### 4. Request Header (@RequestHeader)
```java
@GetMapping("/headers")
public String getHeader(@RequestHeader("User-Agent") String userAgent) {
    return "User-Agent: " + userAgent;
}
```

### 5. Cookie (@CookieValue)
```java
@GetMapping("/cookie")
public String getCookie(@CookieValue("SESSIONID") String sessionId) {
    return "Session ID: " + sessionId;
}
```

### 6. Form Data (@ModelAttribute)
```java
@PostMapping("/form")
public String submitForm(@ModelAttribute UserForm form) {
    return "Form submitted: " + form.getName();
}
```

### 7. HttpServletRequest / HttpServletResponse 직접 사용
```java
@GetMapping("/raw")
public void rawAccess(HttpServletRequest request, HttpServletResponse response) throws IOException {
    String param = request.getParameter("q");
    response.getWriter().write("Query: " + param);
}
```

## 검증(Validation)

Spring Boot에서는 `javax.validation`(Jakarta Validation)과 `@Valid` 또는 `@Validated`를 활용해 요청 데이터를 검증할 수 있습니다.

### DTO 예시
```java
public class UserDto {
    @NotBlank
    private String name;

    @Email
    private String email;

    @Min(18)
    private int age;

    // getters, setters
}
```

### 컨트롤러에서 검증 적용
```java
@PostMapping("/users")
public String createUser(@Valid @RequestBody UserDto user, BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        return "Validation failed: " + bindingResult.getAllErrors();
    }
    return "User created: " + user.getName();
}
```

- `@Valid`: DTO 필드에 선언된 제약 조건을 검증
- `BindingResult`: 검증 실패 시 에러 정보를 담음

## 예외 처리(Exception Handling)

Spring Boot에서는 `@ControllerAdvice`와 `@ExceptionHandler`를 활용해 전역 예외 처리를 할 수 있습니다.

### 전역 예외 처리 예시
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<String> handleValidationException(MethodArgumentNotValidException ex) {
        return ResponseEntity.badRequest().body("Validation error: " + ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneralException(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body("Server error: " + ex.getMessage());
    }
}
```

- `@RestControllerAdvice`: 모든 컨트롤러에 적용되는 전역 예외 처리기
- `@ExceptionHandler`: 특정 예외 타입을 잡아 커스텀 응답 반환

## 확장 가능한 개념

- `@RequestPart`: Multipart 파일 업로드 처리
- `@SessionAttribute`: 세션에 저장된 값 매핑
- `@RequestAttribute`: request scope에 저장된 값 매핑
- `@Valid + BindingResult`: 요청 값 검증 및 에러 처리
- `@ControllerAdvice`: 전역 예외 처리

## 관련 링크
- [[Spring WebFlux]] - 리액티브 웹 애플리케이션에서의 컨트롤러 처리
- #spring #controller #validation #exception-handling