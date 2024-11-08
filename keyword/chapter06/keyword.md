## 🎯핵심 키워드

---

***1.  지연로딩과 즉시로딩의 차이***

- 지연 로딩(Lazy Loading)
 - 지연 로딩은 데이터가 실제로 필요할 때 비로소 데이터를 불러오는 전략이다. 
   - 처음에 엔티티가 로드될 때는 프록시 객체로 로딩된다.
   - 예를 들어, Member 엔티티가 Team과 연관되어 있을 때, Member를 조회하면 Team의 실제 데이터는 로딩되지 않고 프록시 객체가 할당된다. Team의 데이터가 실제로 필요해지는 시점, 즉 해당 필드를 접근할 때 데이터베이스 쿼리가 실행되어 Team의 데이터를 가져오는 방식이다. 

 - 이 방법은 대량의 데이터 처리가 이루어질 때 성능을 최적화할 수 있는 장점이 있다.

 - 다만, 지연 로딩은 추가 쿼리를 발생시키므로 연관된 데이터가 자주 호출될 경우 비효율적일 수 있다. 
  - 예를 들어, N+1 문제라는 성능 저하 이슈가 발생할 수 있다. N+1 문제는 5주차에 정리한, 처음 조회한 엔티티와 연관된 엔티티들이 하나씩 로딩되면서 추가 쿼리가 발생하는 현상을 의미한다. 

 - 이러한 상황을 해결하기 위해, JPA는 Fetch Join과 같은 전략을 제공한다.


- 즉시 로딩(Eager Loading)
 - 즉시 로딩은 엔티티를 조회할 때, 연관된 엔티티도 함께 조회하는 방식이다. 
  - 즉, Member 엔티티를 조회할 때, 연관된 Group도 동시에 조회하는 것이다. 
  - 이렇게 하면 데이터가 필요할 때마다 추가적인 쿼리를 실행할 필요가 없어 성능이 향상될 수 있다. 
  - 특히 @ManyToOne, @OneToOne과 같은 연관 관계에서 많이 사용된다.

 - 즉시 로딩은 불필요한 데이터가 로드되는 문제를 야기할 수 있다. 
  - 필요하지 않은 연관 데이터를 함께 조회하기 때문에 메모리 부담이 증가할 수 있으며, JOIN 쿼리가 과도하게 복잡해질 경우 오히려 성능 저하로 이어질 수도 있다. 
  

- 따라서 즉시 로딩과 지연 로딩 중 어느 로딩 방식을 사용할지는 애플리케이션의 요구사항과 데이터의 특성을 고려하여 결정하는 것이 중요하다.


---


***2. Fetch Join***

- Fetch Join은 JPA에서 제공하는 최적화 기법 중 하나로, 지연 로딩으로 설정된 연관 데이터를 한 번의 쿼리로 함께 가져오고자 할 때 사용한다. 
 - 예를 들어, SELECT m FROM Member m JOIN FETCH m.reviews라는 쿼리는 Member 엔티티와 함께 연관된 reviews 컬렉션을 한 번에 조회하여 N+1 문제를 예방한다.

- 이 방식은 성능 최적화에 강력한 수단이지만 주의가 필요하다. 
 - Fetch Join은 특정 상황에서 과도한 조인을 발생시켜 오히려 성능 저하를 초래할 수 있다. 특히 여러 컬렉션이 동시에 조인되면 데이터 중복이 발생하거나 메모리 과부하가 생길 수 있다. 
 - 이러한 이유로 Fetch Join은 단순한 연관 데이터 조회에서는 매우 유용하지만, 대량의 데이터를 동반한 복잡한 연관 관계에서는 주의 깊게 사용해야 한다.
 

---


***3. @EntityGraph***

- @EntityGraph는 엔티티에서 특정 연관 데이터를 즉시 로딩으로 가져와야 할 때 사용되는 어노테이션이다. 
 - 이는 JPQL과 함께 사용하여 복잡한 쿼리 없이 필요한 연관 데이터를 로딩할 수 있게 한다. 
 - 예를 들어, @EntityGraph(attributePaths = {"member", "reviews"})와 같이 설정하면 member와 reviews 필드가 즉시 로딩된다.

- 이는 지연 로딩의 장점과 즉시 로딩의 효율성을 결합한 접근법으로, 특정 엔티티의 연관 관계를 효율적으로 관리할 수 있다. 
 - 특히 복잡한 조회 쿼리를 작성하는 것을 피하면서 연관 데이터를 쉽게 가져올 수 있어 코드의 가독성도 높아진다.
- @EntityGraph는 엔티티 조회 시 추가적인 조인 쿼리를 필요로 하지 않는 간편한 방식으로, 성능 최적화에 적합하다.


---


***4. JPQL***

- JPQL(Java Persistence Query Language)
JPQL은 JPA에서 제공하는 표준 쿼리 언어로, 엔티티 객체를 기준으로 데이터베이스에 질의할 수 있는 기능을 제공한다. 
 - 일반 SQL과 유사한 문법을 사용하지만, 데이터베이스 테이블이 아닌 JPA 엔티티를 대상으로 동작한다. 
 - JPQL은 SELECT r FROM Review r WHERE r.content = :content과 같은 형식으로 쿼리를 작성할 수 있다.

- JPQL의 가장 큰 장점은 SQL에 비해 객체 지향적 접근을 지원한다는 점이다. 
 - 테이블 간의 관계가 아닌 엔티티 간의 연관성을 기반으로 질의할 수 있어, 개발자에게 보다 직관적인 쿼리 작성 환경을 제공한다. 
 - 하지만 동적 쿼리 작성이 어렵고, 컴파일 시점에 쿼리 오류를 감지할 수 없는 단점이 있다. 주로 간단한 조회나 정적인 쿼리에 많이 사용되며, 복잡한 조건을 갖춘 동적 쿼리에는 적합하지 않다.


---


***5. QueryDSL***

- QueryDSL은 동적 쿼리를 보다 안전하고 효율적으로 작성할 수 있도록 지원하는 라이브러리다. QueryDSL은 코드 기반으로 쿼리를 작성하므로, 컴파일 시점에 오류를 확인할 수 있으며, 타입 안정성을 제공한다. 
 - 예를 들어, QReview review = QReview.review; queryFactory.selectFrom(review).where(review.content.eq("JPA"));와 같이 QueryDSL을 사용하면, 코드를 통해 쿼리를 구성할 수 있다.

- QueryDSL은 JPQL의 단점을 보완하며, 동적 쿼리가 필요한 상황에서 특히 강력한 도구다. 
 - QueryDSL을 활용하면 복잡한 조건을 갖춘 동적 쿼리도 쉽게 작성할 수 있으며, 코드의 가독성 또한 높아진다. 
 - Q 클래스를 통해 필드에 접근하여 조건을 설정할 수 있으므로, 복잡한 조건과 필터링이 필요한 경우 유용하게 사용할 수 있다. 
 - QueryDSL은 실무에서 많이 사용되는 라이브러리로, JPQL로 작성된 쿼리를 더욱 간결하고 안전하게 작성하고자 할 때 활용된다.


---