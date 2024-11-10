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
>
> 

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


