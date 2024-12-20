## 🎯핵심 키워드

---

***1.  @RestControllerAdvice***

- @RestControllerAdvice는 REST API 전용으로 예외를 전역에서 처리하도록 돕는 어노테이션이다. 스프링에서 예외를 다루는 여러 방식 중 @RestControllerAdvice는 API의 전반적인 예외 처리를 한 곳에서 담당하여, 일관된 응답 구조로 에러를 반환할 수 있게 한다.


- @RestControllerAdvice의 주요 기능
 - 전역 예외 처리: 특정 컨트롤러에 종속되지 않고, 모든 @RestController의 예외를 포괄적으로 관리한다.
 - 일관된 JSON 응답: 응답 형식이 JSON으로 고정되므로 RESTful 서비스에 적합하며, API 사용자에게 일관된 에러 형식을 제공할 수 있다.
 - 핸들러 메서드: @ExceptionHandler를 통해 예외 유형별로 메서드를 정의하여, 상황에 맞는 에러 메시지와 HTTP 상태 코드를 설정할 수 있다.


- @RestControllerAdvice 활용 시 주의사항
 - 클래스 분리: 예외의 복잡성이 커질 경우, 여러 @RestControllerAdvice 클래스로 예외 처리를 모듈화하여 관리하는 것이 좋다.
 - 응답 형식 통일: 에러 응답의 포맷을 정해 클라이언트가 일관된 에러 메시지를 받을 수 있게 한다.
 - 최상위 핸들러 설정: 예상하지 못한 예외에 대비해 최상위 예외 핸들러를 정의하여, 모든 에러가 적절한 에러 메시지로 응답될 수 있도록 한다.


- ExceptionHandler 사용 예시

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<ErrorResponse> handleCustomException(CustomException ex) {
        return buildErrorResponse(ex.getErrorCode());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneralException(Exception ex) {
        return buildErrorResponse(CommonErrorCode.INTERNAL_SERVER_ERROR);
    }

    private ResponseEntity<ErrorResponse> buildErrorResponse(ErrorCode errorCode) {
        return ResponseEntity.status(errorCode.getHttpStatus())
                .body(new ErrorResponse(errorCode.name(), errorCode.getMessage()));
    }
}


---


***2. Lombok***

- Lombok은 자주 사용되는 코드 요소를 자동 생성하는 자바 라이브러리로, 다양한 어노테이션을 제공하여 생산성을 높이고 코드 가독성을 향상시킨다.


- Lombok의 주요 어노테이션 및 기능
 - @Getter, @Setter: 클래스나 필드에 적용하여 getter, setter 메서드를 자동 생성한다.
 - @AllArgsConstructor / @NoArgsConstructor: 모든 필드를 매개변수로 받는 생성자나 기본 생성자를 자동 생성한다.
 - @Builder: 빌더 패턴을 적용해 객체 생성 시 유연성을 제공한다.
 - @ToString: toString() 메서드를 자동 생성하여 객체의 정보를 문자열로 반환한다. ( 객체를 호출할 때 나올 메세지를 설정할 수 있음)


@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class User {
    private Long id;
    private String name;
    private String email;
}


- Lombok 사용 시 유의점
 - 디버깅의 어려움: 자동 생성된 코드가 보이지 않기 때문에 디버깅 시 생성된 메서드의 호출 흐름을 추적하기 어렵다.
 - 의존성: Lombok은 컴파일 타임에 동작하므로, 프로젝트의 빌드 도구와의 호환성 문제에 주의가 필요하다.


---
