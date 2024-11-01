## 🔥 추가과제) 해당 키워드에 대해서 공부한 후 간단정리후 제출 & 면접질문 최소 한개 만들어서 제출 후 스터디 시간에 스터디원들과 질문하기!

추가과제는 이전에 하던 방식 그대로 진행하시면 될것같습니다! 이번주도 화이팅입니다!
---

***1.  제네릭과 와일드카드***

- generic은 데이터 타입을 미리 지정하지 않고, 외부에서 지정할 수 있도록 하는 기능이다. 이를 통해 다양한 타입에 대해 같은 코드로 처리함으로 코드의 재사용성과 유지보수성을 높일 수 있다. 예를 들어, 동일한 클래스나 메서드가 여러 타입의 데이터를 처리해야 할 때, generic을 사용하면 별도의 중복 코드를 작성할 필요 없이 한 번의 구현으로 모든 타입을 처리할 수 있다.

 - > generic 타입은 기본 자료형(Primitive type)을 사용할 수 없고, 반드시 참조형(Reference type)이어야 한다. 예를 들어, int 대신 Integer를 사용해야 한다.

 - > generic 배열은 생성할 수 없다. new T[10]과 같은 코드가 허용되지 않는다.


- 와일드카드는 generic에서 불확실한 타입을 처리하기 위한 기법이다. generic 타입을 사용할 때, 명확한 타입을 지정하지 않고 범용적으로 여러 타입을 지원할 수 있게 하는 역할을 한다. 와일드카드는 주로 ?, ? extends T, ? super T 형태로 사용된다.
 
 - > 무제한 와일드카드 (<?>): 어떠한 타입이든 허용한다. generic의 불특정 타입을 표현할 때 사용한다.
 - > 상한 경계 와일드카드 (<? extends T>): 특정 타입의 하위 클래스만 허용한다. 데이터를 읽을 때 유용하다.
 - > 하한 경계 와일드카드 (<? super T>): 특정 타입의 상위 클래스만 허용한다. 데이터를 쓸 때 유용하다.



---


***2. 래퍼 클래스***

- 래퍼 클래스는 기본 자료형(예: int, char)을 객체로 다룰 수 있게 해주는 클래스이다. 자바에서 제공하는 제네릭이나 컬렉션 프레임워크는 기본 자료형을 지원하지 않기 때문에, 이들을 사용하기 위해서는 기본 자료형을 래퍼 클래스로 변환해야 한다. 예를 들어, int는 Integer, double은 Double 같은 래퍼 클래스로 변환할 수 있다.

Integer num = 100;  // 박싱
int n = num;        // 언박싱

- 주요 래퍼 클래스
  - > int → Integer
  - > char → Character
  - > double → Double

- 박싱과 언박싱
 - 박싱(Boxing): 기본 자료형을 해당하는 래퍼 클래스로 변환하는 과정
 - 언박싱(Unboxing): 래퍼 클래스를 다시 기본 자료형으로 변환하는 과정

- > 자바 5부터는 오토 박싱(Auto Boxing)과 오토 언박싱(Auto Unboxing)을 지원하여, 개발자가 명시적으로 변환할 필요 없이 자바가 자동으로 변환해준다.



---


***3. 옵셔널***

- optional은 자바 8에 도입된 기능으로, 값이 존재할 수도 있고 존재하지 않을 수도 있는 상황을 표현하는 데 사용된다. 이를 통해 null 값 처리의 복잡함을 줄이고, NullPointerException의 발생을 예방할 수 있다.

Optional<String> optionalString = Optional.ofNullable(null);
optionalString.ifPresent(System.out::println); // 값이 있을 경우 출력

 - > Optional.of()는 값이 절대 null이 아닌 경우 사용되고, Optional.ofNullable()은 값이 null일 수도 있을 때 사용된다. 이 방법을 사용하면 null 체크를 명시적으로 처리할 수 있다.



---


***4. 면접 질문 만들어보기***

- 제네릭의 장점은 무엇인가요?

컴파일 시 타입을 체크하므로 런타임 오류가 줄어들 수 있다.