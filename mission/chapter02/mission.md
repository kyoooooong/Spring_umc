## 🔥 미션

---

- (1) 1주차 때 설계한 데이터베이스를 토대로 아래의 화면에 대한 쿼리를 작성

***저번주차 설계 DB***

  - ![week1](./1주차미션.png)

***1.  내가 진행 중, 진행 완료한 미션을 모아서 보는 쿼리***

SELECT H.name, M.mission_num, M.rest_name, M.request
FROM 회원 H
JOIN 선호음식카테고리 C  ON H.email = C.email
LEFT JOIN 미션 M ON C.email = M.email
WHERE H.email = '회원이메일' 
ORDER BY M.datetime DESC;

---


***2. 리뷰 작성하는 쿼리***

INSERT INTO 리뷰 (mission_num, email, star, content, datetime)
VALUES (1234, '회원이메일', 4.5, '좋은음식이었어요~:)', NOW());

---


***3. 홈 화면 쿼리 ***

- 지역내에서 성공한 미션들
SELECT count(*)
FROM 미션 M
JOIN 점표 J ON M.mission_num = J.mission_num
JOIN 지역 R ON J.지역 id = R.지역 id
WHERE J.request = 1 //성공미션
AND J.email = '회원이메일' //자신미션
AND J.지역_id = "" //선택지역id
ORDER BY M.datetime DESC
LIMIT 10 OFFSET 0; //10개씩 페이징

- 지역내에서 도전가능 미션들
SELECT M.rest_name, M.price, M.datetime
FROM 미션 M
JOIN 점표 J ON M.mission_num = J.mission_num
JOIN 지역 R ON J.지역 id = R.지역 id
WHERE J.request = 0 //성공미션
AND J.email = '회원이메일' //자신미션
AND J.지역_id = "" //선택지역id
ORDER BY M.datetime DESC
  
---


***4. 마이 페이지 화면 쿼리***

SELECT name, email, address
FROM 회원
WHERE email = '회원이메일'  //자신페이지


  
