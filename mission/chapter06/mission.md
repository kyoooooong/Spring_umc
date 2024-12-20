## 🔥 미션

---

- 2주차 미션 때 했던 해당 화면들에 대해 작성했던 쿼리를 QueryDSL로 작성하여 리팩토링하기


***1.  내가 진행 중, 진행 완료한 미션을 모아서 보는 쿼리***

Q회원 h = Q회원.회원;
Q선호음식카테고리 c = Q선호음식카테고리.선호음식카테고리;
Q미션 m = Q미션.미션;

List<Tuple> results = queryFactory
    .select(h.name, m.mission_num, m.rest_name, m.request)
    .from(h)
    .join(c).on(h.email.eq(c.email))
    .leftJoin(m).on(c.email.eq(m.email))
    .where(h.email.eq("회원이메일"))
    .orderBy(m.datetime.desc())
    .fetch();

---


***2. 리뷰 작성하는 쿼리***

Q리뷰 review = Q리뷰.리뷰;

long insertedCount = queryFactory.insert(review)
    .set(review.mission_num, 1234)
    .set(review.email, "회원이메일")
    .set(review.star, 4.5)
    .set(review.content, "좋은음식이었어요~:)")
    .set(review.datetime, LocalDateTime.now())
    .execute();

---


***3. 홈 화면 쿼리 ***

- 지역내에서 성공한 미션들
Q미션 m = Q미션.미션;
Q점표 j = Q점표.점표;
Q지역 r = Q지역.지역;

long successCount = queryFactory
    .select(m.count())
    .from(m)
    .join(j).on(m.mission_num.eq(j.mission_num))
    .join(r).on(j.지역_id.eq(r.지역_id))
    .where(
        j.request.eq(1),
        j.email.eq("회원이메일"),
        j.지역_id.eq("선택지역id")
    )
    .orderBy(m.datetime.desc())
    .limit(10)
    .offset(0)
    .fetchCount();

- 지역내에서 도전가능 미션들
List<Tuple> availableMissions = queryFactory
    .select(m.rest_name, m.price, m.datetime)
    .from(m)
    .join(j).on(m.mission_num.eq(j.mission_num))
    .join(r).on(j.지역_id.eq(r.지역_id))
    .where(
        j.request.eq(0),
        j.email.eq("회원이메일"),
        j.지역_id.eq("선택지역id")
    )
    .orderBy(m.datetime.desc())
    .fetch();
  
---


***4. 마이 페이지 화면 쿼리***

Q회원 h = Q회원.회원;

Tuple myPageInfo = queryFactory
    .select(h.name, h.email, h.address)
    .from(h)
    .where(h.email.eq("회원이메일"))
    .fetchOne();


  
