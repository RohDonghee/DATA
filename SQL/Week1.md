# SQL_ADVANCED 1주차 정규 과제 

## Week 1 : 서브쿼리 & CTE

📌**SQL_ADVANCED 정규과제**는 매주 정해진 주제에 따라 **MySQL 공식 문서 또는 한글 블로그 자료를 참고해 개념을 정리한 후, 프로그래머스 SQL 3문제**와 **추가 확인문제**를 직접 풀어보며 학습하는 과제입니다. 

이번 주는 아래의 **SQL_ADVANCED_0th_TIL**에 나열된 주제를 중심으로 개념을 학습하고, 주차별 **학습 목표**에 맞게 정리해주세요. 정리한 내용은 GitHub에 업로드한 후, **스프레드시트의 'SQL' 시트에 링크를 제출**해주세요. 



**👀 (수행 인증샷은 필수입니다.)** 

> 프로그래머스 문제를 풀고 '정답입니다' 문구를 캡쳐해서 올려주시면 됩니다. 



## SQL_ADVANCED_1st_TIL 

### 15.2.15. SubQueries

#### 특히 15.2.15.1 ~ 15.2.15.7 (Scalar, EXISTS, Correlated, Derived 등) 

### 15.2.20 WITH (Common Table Expressions)

- `WITH RECURSIVE`에 대한 내용은 추후에 공부합니다. 해당 링크에서 `WITH`에 해당하는 부분만 정리해보세요. 




## 🏁 주차별 학습 (Study Schedule)

| 주차  | 공부 범위               | 완료 여부 |
| ----- | ----------------------- | --------- |
| 1주차 | 서브쿼리 & CTE          | ✅         |
| 2주차 | 집합 연산자 & 그룹 함수 | 🍽️         |
| 3주차 | 윈도우 함수             | 🍽️         |
| 4주차 | Top N 쿼리              | 🍽️         |
| 5주차 | 계층형 질의와 셀프 조인 | 🍽️         |
| 6주차 | PIVOT / UNPIVOT         | 🍽️         |
| 7주차 | 정규 표현식             | 🍽️         |

<br>


### 공식 문서 활용 팁

>  **MySQL 공식 문서는 영어로 제공되지만, 크롬 브라우저에서 공식 문서를 열고 이 페이지 번역하기에서 한국어를 선택하면 번역된 버전으로 확인할 수 있습니다. 다만, 번역본은 문맥이 어색한 부분이 종종 있으니 영어 원문과 한국어 번역본을 왔다 갔다 하며 확인하거나, 교육팀장의 정리 예시를 참고하셔도 괜찮습니다.**



# 1️⃣ 학습 내용 

> 아래의 링크를 통해 *MySQL 공식문서*로 이동하실 수 있습니다.
>
> - SubQueries : MySQL 공식문서 
>
> https://dev.mysql.com/doc/refman/8.0/en/subqueries.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-subqueries/


> - CTE(공통 테이블 표현식) : MySQL 공식문서
>
> https://dev.mysql.com/doc/refman/8.0/en/with.html
>
> (한국어 버전)
> https://dart-b-official.github.io/posts/mysql-cte/

<br>
<br>
<!-- 여기까진 그대로 둬 주세요-->





# 2️⃣ 학습 내용 정리하기

---

 # 1. 서브쿼리 (SubQuery)
- 메인 쿼리(Outer Query)의 실행에 필요한 데이터를 제공하기 위해 사용되는 내부 쿼리(Inner Query)
- 하나의 SQL 문 안에 포함된 SELECT 문을 표현 (서브쿼리는 외부쿼리 안에 중첩됨)
- 서브쿼리는 괄호로 감싸져야 함.
- ```GROUP BY```를 제외한 모든 부분에서 사용이 가능함
  
## 서브쿼리 사용 이유
- 테이블 : 영속적인 데이터를 저장
- 뷰 : 영속적이지만 데이터는 저장하지 않음. 따라서 접근할 때마다 SELECT문이 실행됨
- 서브쿼리 : 비영속적인 생존기간(스코프)이 SQL구문 실행 중으로 한정됨

## 서브쿼리 사용 가능한 외부 구문 
### SELECT (스칼라 서브쿼리, Scalar Subquery)
- 다른 테이블에서 값을 가져올 때 사용
- 하나의 행과 하나의 컬럼만을 반환
```MySQL
SELECT [컬럼], (SELECT [컬럼] FROM [테이블] WHERE 조건) 
FROM [테이블]
WHERE 조건;

```

### FROM (인라인 뷰, Inline View)
- 동적 테이블이기 때문에, DB에 저장되지 않음
- 메인 쿼리가 데이터를 조회하기 위한 임시 테이블 또는 데이터 집합 역할을 수행
- FROM절에 쓰이는 서브쿼리는 반드시 AS로 별칭을 지정해야 함
```MySQL
SELECT [컬럼]
FROM (SELECT [컬럼] FROM [테이블] WHERE [조건]) AS [별명] 
WHERE 조건;

```

### WHERE (중첩문 서브쿼리, Nested Subquery)
```MySQL
SELECT [컬럼]
FROM [테이블]
WHERE [컬럼][연산자](SELECT [컬럼] FROM [테이블] WHERE 조건);

```


### HAVING 
- GROUP BY한 조건을 이후에 서브쿼리 기준으로 분류
```MySQL
SELECT T1.C1, T2.C1, T2.C2
FROM TEMP1 T1, TEMP2 T2
WHERE T1.C1 = T2.C1
GROUP BY T1.C1, T2.C1, T2.C2
HAVING AVG(T1.C1) < (SELECT AVG(C1) FROM T2 );

```

#### ORDER BY
```MySQL
SELECT *
FROM TEMP1 T1
ORDER BY C1;
 
SELECT *
FROM TEMP2 T2  
ORDER BY (SELECT C1 FROM T1 WHERE T1.C2 = T2.C2);
```
#### INSERT - VALUES
#### UPDATE - SET




# 2. 공통 테이블 표현식(CTE)
~~~
✅ 학습 목표 :
* CTE에 대한 문법을 이해하고 활용할 수 있다. 
~~~
- CTE는 하나의 SQL문 내에서만 존재하는 이름 있는 임시 결과 집합
```MySQL
WITH 테이블 이름 AS (테이블 만들 쿼리문)
``` 

```MySQL
WITH avg_tip AS (
  SELECT day, AVG(tip) AS avg_tip
  FROM tips
  GROUP BY day
)
SELECT t.day, t.time, t.total_bill, t.tip
FROM tips t
JOIN avg_tip a ON a.day = t.day
WHERE t.tip > a.avg_tip
ORDER BY t.day, t.tip DESC;
```


---

# 3️⃣ 실습 문제

**두 문제 중에서 한 문제는 SubQuery와 CTE를 사용한 방법을 각각 활용해서 2개의 답변을 제시해주세요**

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131123

> 즐겨찾기가 가장 많은 식당 정보 출력하기 (GROUP BY, SubQuery) : Lev 3

https://school.programmers.co.kr/learn/courses/30/lessons/131115

> 가격이 제일 비싼 식품의 정보 출력하기 (SUM, MAX, MIN, SubQuery) : Lev 2



---

## 문제 인증란

<!-- 이 주석을 지우고 여기에 문제 푼 인증사진을 올려주세요. -->



---


## 문제 1

> **🧚예린이는 최근 여러 주문 데이터를 분석하는 업무를 맡게 되었습니다. 특정 고객의 주문 이력을 분석하기 위해, 다음과 같이 최근 30일간 주문만 필터링한 CTE를 사용해 쿼리를 작성했습니다.**

~~~sql
WITH RecentOrders AS (
  SELECT *
  FROM Orders
  WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
)
SELECT customer_id, COUNT(*) AS recent_order_count
FROM RecentOrders
GROUP BY customer_id;
~~~

> **그런데 예린이는 "이 쿼리를 WITH 없이, 서브쿼리 방식으로 바꿔서 실행해보라" 는 피드백을 받았고, 서브쿼리로 작성해보려 했지만 익숙하지 않아 SQL_ADVANCED를 듣는 학회원분들에게 도움을 요청하고 있습니다. 예린이의 쿼리를 WITH 없이 서브쿼리로 변환해보세요. 그리고 두 방식의 차이점을 설명해보고, 각각의 장단점을 정리해보세요**



~~~
여기에 답을 작성해주세요!
~~~



## 참고자료

서브쿼리를 사용하는 이유가 너무 어려우신 분들을 위해 참고자료를 첨부합니다. 아래 블로그를 통해서 더욱 쉽게 공부해보시고 문제를 풀어보세요.

1. [SQL] 서브쿼리는 언제 쓰는걸까? 
   https://project-notwork.tistory.com/38

2. [SQLD] 서브 쿼리 (SubQeury) 개념 및 종류
   https://bommbom.tistory.com/entry/%EC%84%9C%EB%B8%8C-%EC%BF%BC%EB%A6%ACSub-Query-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%A2%85%EB%A5%98


### 🎉 수고하셨습니다.
