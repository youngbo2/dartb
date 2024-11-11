## 1. 틀린 코드 이유 분석
[https://school.programmers.co.kr/learn/courses/30/lessons/131123]


```SQL
SELECT *
FROM (SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE
ORDER BY FOOD_TYPE DESC
```
> 이 코드가 틀린 이유를 찾아내고 정답 코드와 비교

쿼리 자체가 뭔가 이상한데 원래 의도한건 아래 코드인 것 같다.
```SQL
SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE
ORDER BY FOOD_TYPE DESC
```
아무튼 위 코드가 틀린 이유는 SQL에서는 집계함수를 이용해 필터링할 때, 그와 일치하는 다른 필드를 바로 가져올 수 없기 때문이다.

### 정답코드
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

서브쿼리에서 두 개의 필드 food_type, favorites를 선택하는 이유는 각 food_type별로 가장 높은 favorites 값을 찾아야 하기 때문이다. 다시 메인 쿼리의 WHERE절에서 IN키워드를 통해 서브쿼리에서 선택된 레코드만 가져오는 필터를 걸어준다. 만약 한 food_type내에서 가장 높은 favorites값이 중복 될 경우 값이 일치하는 모든 레코드를 반환한다.
___
## 2. 개선된 쿼리


```SQL
WITH RankedRest AS (
    SELECT 
        FOOD_TYPE, REST_ID, REST_NAME, FAVORITES,
        ROW_NUMBER() OVER (PARTITION BY FOOD_TYPE ORDER BY FAVORITES DESC, REST_ID) AS rnk
    FROM REST_INFO
)
SELECT 
    FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM RankedRest
WHERE rnk = 1
ORDER BY FOOD_TYPE DESC;
```
> 이 코드를 단계별로 해석하고 위 코드와 비교했을때 이점 설명

우선 CTE를 사용한 것이 눈에 띈다. `WITH`절로 쿼리를 미리 정의하면 해당 쿼리의 결과를 어디서나 가져올 수 있다. 쿼리문 자체는 길어지기 때문에 문제를 풀 때 선호하는 방식은 아니지만, 현업에서는 쓸 일이 많을 것 같다.

CTE를 통해 `FAVORITES`를 기준으로 정렬한 row number를 `rnk`라는 필드로 지정했다. 메인 쿼리에서는 해당 `rnk`필드로 필터를 하면 원하는 조건을 찾을 수 있다.

해당 방식의 장점은, 오직 최대값만을 반환하는 MAX함수와 달리 `WHERE rnk < 5 AND rnk > 1`와 같은 방식으로 원하는 값을 원하는 만큼 가져올 수 있다.

해당 방식의 단점은, 만약 `FAVORITES`값이 중복되어도 공동순위가 인정되지 않는다. 1등이 2개여도 항상 한개의 값만 출력한다.

"개선된"이라는 표현은 별로 적절하지 않다고 생각한다. 

___

## 3. 조건에 맞는 사원 정보 조회하기
[https://school.programmers.co.kr/learn/courses/30/lessons/284527]


```SQL
SELECT 
    EMP_NO, 
    EMP_NAME, 
    SAL,
    RANK() OVER (ORDER BY SAL DESC) AS rnk
FROM 
    HR_EMPLOYEES;
```
> 이때, RANK(), DENSE_RANK(), ROW_NUMBER() 함수를 사용하며 결과를 비교하고 해당 함수를 사용하는 경우를 서술해주세요. (함수 사용 예제는 직접 찾아보기)

1. `RANK()` : 일반적인 순위
![image](https://github.com/user-attachments/assets/db52e7e3-50b5-4594-bebd-3e6c76a3bf31)


2. `DENSE_RANK()` : 동점자는 무시하고 단순 숫자 크기의 순위
![image](https://github.com/user-attachments/assets/892c72d1-f631-40c8-b776-e35f4e9613b6)


3. `ROW_NUMBER()` : 행 번호
![image](https://github.com/user-attachments/assets/70292686-1bbb-4020-94c4-c3cc4f3647b8)

