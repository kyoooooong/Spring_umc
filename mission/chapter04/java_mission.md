## 🔥 추가과제) 해당 키워드에 대해서 공부한 후 간단정리후 제출 & 면접질문 최소 한개 만들어서 제출 후 스터디 시간에 스터디원들과 질문하기!

과제는 아래와 같이 수행해 주시면 됩니다!

1. 혼자 공부한 다음에
2. 문제 풀이
3. 123주차와 같이 키워드 및 면접질문 하나씩


- 1. 함수형 인터페이스 - java.util.function 패키지의 Function<T,R>을 사용해서 Integer을 인자로 받아 String으로 변환하는 함수를 만들어주세요! Function클래스의 apply를 네가지 방법으로 구현해 주세요

  1. 익명클래스 정의
  2. 클래스 파일을 만들어 상속받아서 정의 (class MyFunction extends Function<Integer, String> 과 같이 정의해주시면 됩니다!) 
  3. 람다식으로 정의
  4. 메서드 참조로 정의 (String.valueOf()메서드 사용하시면 됩니다!)

위에서 정의한 Function클래스를 각각 사용해서 정수를 인자로 넣은 후 문자로 변환해서 출력하는 예제를 만들어주세요! 출력 4가지 만드시면 될것같습니다!

- 2. stream api - 1부터 10까지 있는 정수 배열을 stream으로 다루는 코드를 작성해 주세요!

  1. 배열을 stream으로 만들어 요소를 모두 2배로 만든 배열을 반환 후 원본 배열과 스트림 배열을 비교하기 (출력문 사용하시면 됩니다!)
  2. 배열중 짝수만 4 + "is even number"와 같이 String으로 변환한 배열을 만들어 출력하기

---

***1.  함수형 인터페이스***

- 함수형 인터페이스는 하나의 추상 메서드만을 가지는 인터페이스를 뜻한다. Java 8 이후에는 @FunctionalInterface 애노테이션을 사용하여 함수형 인터페이스를 정의할 수 있고, 이를 통해 인터페이스가 단 하나의 추상 메서드만 가지도록 강제할 수 있다. 

- 이렇게 함수형 인터페이스를 사용하면 람다 표현식이나 메서드 참조와 함께 사용할 수 있어 코드의 가독성을 높이고 코드량을 줄이는 데 도움이 된다. Java 8에서는 이를 위해 여러 함수형 인터페이스(Function, Predicate, Supplier, Consumer 등)를 java.util.function 패키지에 제공.

- Java의 함수형 인터페이스는 주로 java.util.function 패키지에서 제공하며, 몇 가지 주요 인터페이스는 다음과 같다
 - 1. Function<T,R>: 입력을 받아 결과를 반환하는 함수형 인터페이스로, 인자 타입 T를 받아 결과 R을 반환함.
 - 2. Predicate<T>: 입력값을 받아 boolean 값을 반환하며, 주로 조건을 지정하는 필터링에 사용됨.
 - 3. Supplier<T>: 값을 생성하여 반환하는 역할을 하며, 인자는 없고 반환값이 있음.
 - 4. Consumer<T>: 입력값을 받아 소비하지만, 반환값이 없는 함수형 인터페이스로 주로 출력 작업에 활용됨.


'''JAVA
import java.util.function.Function;

public class FunctionExamples {
    public static void main(String[] args) {
        Integer input = 10;

        // 1. 익명 클래스 정의
        Function<Integer, String> anonymousFunction = new Function<Integer, String>() {
            @Override
            public String apply(Integer t) {
                return "익명 클래스: " + String.valueOf(t);
            }
        };
        System.out.println(anonymousFunction.apply(input));

        // 2. 클래스 상속 정의 (Function 인터페이스 구현)
        class MyFunction implements Function<Integer, String> {
            @Override
            public String apply(Integer t) {
                return "클래스 상속: " + String.valueOf(t);
            }
        }
        MyFunction myFunction = new MyFunction();
        System.out.println(myFunction.apply(input));

        // 3. 람다식 정의
        Function<Integer, String> lambdaFunction = t -> "람다식: " + String.valueOf(t);
        System.out.println(lambdaFunction.apply(input));

        // 4. 메서드 참조 정의
        Function<Integer, String> methodReferenceFunction = String::valueOf;
        System.out.println("메서드 참조: " + methodReferenceFunction.apply(input));
    }
}
'''

---


***2. stream api***

- Stream API는 데이터의 흐름을 처리하는 Java 8 기능으로, 컬렉션이나 배열과 같은 데이터를 함수형 스타일로 다루기 위한 연속적 데이터 흐름을 제공한다. 이 API는 선언적 코드 스타일과 병렬 처리를 지원하며, 필터링/매핑/정렬/집계와 같은 작업을 용이하게 해준다.

'''JAVA
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExamples {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        // 1. 모든 요소를 2배로 만든 배열
        int[] doubledArray = Arrays.stream(array)
                                   .map(i -> i * 2)
                                   .toArray();

        System.out.println("원본 배열: " + Arrays.toString(array));
        System.out.println("2배로 만든 배열: " + Arrays.toString(doubledArray));

        // 2. 짝수만 String 변환하여 "is even number" 추가
        List<String> evenStringList = Arrays.stream(array)
                                            .filter(i -> i % 2 == 0)
                                            .mapToObj(i -> i + " is even number")
                                            .collect(Collectors.toList());
        System.out.println("짝수 변환 배열: " + evenStringList);
    }
}
'''

---


***3. 면접 질문 만들어보기***

- Stream API가 성능을 최적화하는 방법은 무엇인가요?
 : Stream API는 내부 반복과 지연 실행을 통해 불필요한 연산을 줄여 성능을 최적화한다. 또한, 병렬 처리를 지원하여 대용량 데이터 처리에 적합하다.
