## 🎯핵심 키워드

---

***1.  java의 Exception 종류들***

- 자바에서의 예외(Exception)는 크게 에러(Error)와 예외(Exception)로 구분된다.

- 1. 에러(Error)와 예외(Exception)의 차이
 - 에러(Error): 하드웨어의 고장이나 JVM의 실행 문제로 인해 발생하는 오류이다. 이는 프로그램에서 처리할 수 없으며, 프로그램을 종료시키는 원인이 된다. 예를 들어, 메모리 부족이나 JVM의 내부 오류가 해당된다.
 - 예외(Exception): 사용자나 개발자의 잘못된 코드로 인해 발생하는 오류이다. 예외는 예외처리를 통해 프로그램을 종료하지 않고 정상적인 실행 상태를 유지할 수 있다.

- 2. 예외의 종류
자바의 예외는 크게 일반 예외(Exception)와 실행 예외(RuntimeException)로 나눌 수 있다.
 - 1. 일반 예외(Exception)
일반 예외는 컴파일 체크 예외(Checked Exception)로, 예외 처리 코드가 필수적이다. 컴파일 시 예외 처리 여부를 확인하며, 처리하지 않으면 컴파일 오류가 발생한다.
 - 2. 실행 예외(RuntimeException)
실행 예외는 컴파일 시 예외 처리 코드가 필수적인지 검사하지 않는 예외(Unchecked Exception)이다. 예외 처리 코드가 없어도 컴파일은 가능하지만, 실행 중 예외가 발생할 수 있다.

- 3. 자주 발생하는 예외
 - 1. NullPointerException (NPE)
 원인: null 값을 가진 참조 변수를 사용하여 메서드나 속성에 접근할 때 발생한다.

public class NullPointerExceptionExample {
    public static void main(String[] args) {
        String data = null;
        System.out.println(data.toString());
    }
}

 - 2. ArrayIndexOutOfBoundsException
 원인: 배열의 인덱스를 벗어난 값에 접근할 때 발생한다.

public class ArrayIndexOutOfBoundsExceptionExample {
    public static void main(String[] args) {
        int[] arr = new int[3];
        for (int i = 0; i <= 3; i++) {
            arr[i] = i;
        }
    }
}

 - 3. NumberFormatException
 원인: 문자열을 숫자로 변환할 수 없을 때 발생한다.

public class NumberFormatExceptionExample {
    public static void main(String[] args) {
        String corretData = "100";
        String wrongData = "A100";
        int value1 = Integer.parseInt(corretData);
        int value2 = Integer.parseInt(wrongData);
    }
}

 - 4. ClassCastException
 원인: 클래스 간의 불가능한 형변환을 시도할 때 발생한다.

public class ClassCastExceptionExample {
    static class Animal {}
    static class Dog extends Animal {}
    static class Cat extends Animal {}

    public static void main(String[] args) {
        Animal animal = new Cat();
        Dog dog = (Dog) animal;
    }
}

- 4. 예외 처리
자바에서 예외는 try-catch 블록을 통해 처리할 수 있다. 예외가 발생할 수 있는 코드 블록을 try로 감싸고, 예외가 발생하면 catch 블록에서 처리한다.

public class NullPointerExceptionHandling {
    public static void main(String[] args) {
        String data = null;
        try {
            System.out.println(data.toString());
        } catch (NullPointerException e) {
            System.out.println("NullPointerException 예외 처리됨");
        }
    }
}

---


***2. @Valid***

- @Valid는 JSR-303 표준에 기반한 자바의 빈 검증기(Bean Validator) 어노테이션으로, 객체의 제약 조건을 검증하는 데 사용된다. Spring에서는 LocalValidatorFactoryBean을 통해 검증을 처리한다. 이를 사용하려면 의존성 추가 후 빈으로 등록해야 한다.

- 1. 사용 방법
@Valid 어노테이션은 RequestBody 객체와 같은 데이터의 유효성을 검증하는 데 사용된다. 예를 들어, 클라이언트가 전달하는 JSON 데이터를 검증할 때 사용된다.
객체의 필드에 대해 제약 조건을 설정하고, 해당 객체를 @Valid로 검증할 수 있다.

@RestController
@Slf4j
public class TestController {
    @PostMapping("/todos/save")
    public ResponseEntity<String> createJsonTodo(@RequestBody @Valid TodoRequest request){
        log.info("Post : Todo Save -: {}", request.toString);
        return ResponseEntity.ok().body("Todo 객체 검증 성공");
    }
}

@Getter
@Setter
@ToString
public class TodoRequest {
    @NotNull(message = "제목은 필수입니다.")
    private String title;

    @NotEmpty(message = "내용은 필수입니다.")
    private String content;

    private boolean completed;

    @Min(1)
    @Max(7)
    private int dDay;
}

위 코드에서 @NotNull, @NotEmpty, @Min, @Max와 같은 제약 조건 어노테이션을 통해 필드값을 검증한다. 요청이 잘못된 값으로 들어오면 자동으로 SpringBoot가 에러 응답을 생성한다.

- 2. 동작 원리
Spring은 DispatcherServlet을 통해 모든 요청을 처리하고, 컨트롤러 메소드에 필요한 객체를 생성하는 ArgumentResolver가 동작한다.
@Valid는 이 ArgumentResolver에 의해 처리되어, 객체가 검증된다. 검증 중 오류가 발생하면 MethodArgumentNotValidException 예외가 발생하고, DefaultHandlerExceptionResolver가 이를 처리하여 400 BadRequest 상태를 반환한다.

- 3. @Validated와의 차이점
@Valid는 기본적으로 Controller에서만 동작한다. Service 계층이나 다른 곳에서 검증을 적용하려면 @Validated를 사용해야 한다.

- 4. @Validated 어노테이션
@Validated는 Spring에서 제공하는 어노테이션으로, AOP 기반으로 메소드 요청을 가로채어 유효성 검증을 처리한다. @Valid가 메소드 매개변수에 대해서만 동작하는 반면, @Validated는 클래스에 적용하여 클래스 내 모든 메소드에 유효성 검증을 적용할 수 있다.

 - 사용 방법
@Service
@Validated
public class TestTodoService {
    public ResponseEntity todoSave(@Valid TodoRequest request) {
        return ResponseEntity.ok().body("Todo 객체 검증 성공");
    }
}

 - 동작 원리
 @Validated는 AOP를 사용하여 메소드 호출 시 요청을 가로채고, 유효성 검증을 수행한다. 인터셉터(MethodValidationInterceptor)를 사용하여 메소드의 요청을 처리하고 검증을 진행한다.
 @Validated를 사용하면 Controller, Service, Repository 등 모든 계층에서 유효성 검증을 수행할 수 있다.

 - 예외 처리
 @Valid에서 발생한 예외는 MethodArgumentNotValidException으로, Spring Boot에서 자동으로 처리된다.
 @Validated에서는 ConstraintViolationException이 발생할 수 있다.

 - JSR 표준 제약 조건 어노테이션
 JSR 표준 스펙은 다양한 제약 조건 어노테이션을 제공한다. 이 어노테이션은 각 필드에 적용하여 값의 유효성을 검증하는 데 사용된다.

---
