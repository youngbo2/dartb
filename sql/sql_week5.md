## 1. [성분으로 구분한 아이스크림 총 주문량](https://school.programmers.co.kr/learn/courses/30/lessons/133026)

![image](https://github.com/user-attachments/assets/d68e7a28-c215-4dd6-8a13-f3020bd4562c)


```SQL
SELECT I.INGREDIENT_TYPE, SUM(H.TOTAL_ORDER) AS TOTAL_ORDER
FROM ICECREAM_INFO I
LEFT JOIN FIRST_HALF H ON I.FLAVOR = H.FLAVOR
GROUP BY INGREDIENT_TYPE
ORDER BY TOTAL_ORDER ASC;
```
전에 풀었던 기록이 남아있는 문제다. 그래도 다시 한번 풀어봤다.

별로 어려운 부분은 없는 것 같다.


## 2. [즐겨찾기가 가장 많은 식당 정보 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131123)

![image](https://github.com/user-attachments/assets/b2782ca0-80c1-4c9b-a769-12615b316609)


```SQL
SELECT food_type, rest_id, rest_name, favorites
FROM rest_info
WHERE (food_type, favorites) IN (
    SELECT food_type, MAX(favorites)
    FROM rest_info
    GROUP BY food_type
    )
ORDER BY food_type DESC;
```

SQL에서는 집계함수를 이용해 필터링할 때, 그와 일치하는 다른 필드를 바로 가져올 수 없기 때문에 원하는 조건을 찾는 서브쿼리를 작성한다.

서브쿼리에서 두 개의 필드 `food_type, favorites`를 선택하는 이유는 각 food_type별로 가장 높은 favorites 값을 찾아야 하기 때문이다.
다시 메인 쿼리의 `WHERE`절에서 `IN`키워드를 통해 서브쿼리에서 선택된 레코드만 가져오는 필터를 걸어준다.
만약 한 `food_type`내에서 가장 높은 `favorites`값이 중복 될 경우 값이 일치하는 모든 레코드를 반환한다.

윈도우 함수나 조인을 통해서도 문제 해결이 가능한 것 같다. 조인을 하면 서브쿼리 없이 구할 수 있고, 윈도우 함수를 이용하면 상위 1개가 아닌 원하는 만큼 구할 수 있다.


## 3. [조건에 맞는 사원 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/284527)

![image](https://github.com/user-attachments/assets/113fa46c-26f6-4e6c-b5ec-93f4a03800ee)


```SQL
SELECT SUM(G.SCORE) AS SCORE, E.EMP_NO, E.EMP_NAME, E.POSITION, E.EMAIL
FROM HR_EMPLOYEES E
JOIN HR_GRADE G ON E.EMP_NO = G.EMP_NO AND YEAR = 2022
GROUP BY EMP_NO
ORDER BY SCORE DESC
LIMIT 1;
```

특별히 어려운 것은 없는 문제였다. 원하는 연도로 필터링해서 조인해주고 SUM은 SELECT절에서 해주면 쉽게 풀 수 있다.

그런데 만약 부서별로 한 명씩 찾아야 한다면 어떨까? 혹은 연도별로 한 명씩 찾아야 한다면?

2번 문제처럼 각 부서별/연도별로 원하는 조건을 찾는 서브쿼리를 먼저 작성하고, 해당 서브쿼리에서 선택된 레코드를 가져오는 메인쿼리를 작성하면 될 것 같다.
