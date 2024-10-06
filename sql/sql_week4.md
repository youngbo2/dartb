## 1. [ROOT 아이템 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/273710)

![image](https://github.com/user-attachments/assets/63299b6c-4d8a-4bb0-be6b-ecf0b683e7b4)

```SQL
SELECT I.ITEM_ID, I.ITEM_NAME
FROM ITEM_INFO I
LEFT JOIN ITEM_TREE T ON I.ITEM_ID = T.ITEM_ID 
WHERE T.PARENT_ITEM_ID IS NULL
ORDER BY I.ITEM_ID ASC;
```
> 별로 어려운 문제는 아니었다...그런데 만약 ROOT 아이템만 출력하는게 아니라, 모든 아이템의 ROOT 아이템을 함께 출력하는 문제라면 어떻게 풀어야 할까?
> 챗GPT에게 질문해보니 다음과 같은 답을 얻었다. 파이썬으로 하면 정말 간단한 문제인데 SQL로 하니 굉장히 복잡하고 어려워진다.
```SQL
WITH RECURSIVE ItemHierarchy AS (
    -- 1. 초기 값: 각 아이템과 그 부모 아이템을 선택
    SELECT
        IT.ITEM_ID,
        IT.ITEM_NAME,
        IT.ITEM_ID AS ROOT_ITEM_ID -- 처음에는 자기 자신을 ROOT 아이템으로 설정
    FROM
        ITEM_INFO IT
    LEFT JOIN
        ITEM_TREE TREE ON IT.ITEM_ID = TREE.ITEM_ID
    WHERE
        TREE.PARENT_ITEM_ID IS NULL -- PARENT_ITEM_ID가 NULL인 경우 ROOT 아이템

    UNION ALL

    -- 2. 재귀 부분: 부모 아이템을 찾아 계속해서 위로 올라감
    SELECT
        IT.ITEM_ID,
        IT.ITEM_NAME,
        IH.ROOT_ITEM_ID -- 상위 CTE에서 계산된 ROOT 아이템 ID 사용
    FROM
        ITEM_INFO IT
    JOIN
        ITEM_TREE TREE ON IT.ITEM_ID = TREE.ITEM_ID
    JOIN
        ItemHierarchy IH ON TREE.PARENT_ITEM_ID = IH.ITEM_ID -- 부모 아이템을 찾음
)

-- 최종 결과를 출력
SELECT
    IH.ITEM_ID,
    IH.ITEM_NAME,
    RI.ITEM_NAME AS ROOT_ITEM_NAME -- ROOT 아이템 명을 함께 출력
FROM
    ItemHierarchy IH
JOIN
    ITEM_INFO RI ON IH.ROOT_ITEM_ID = RI.ITEM_ID -- ROOT 아이템 이름을 구하기 위해 조인
ORDER BY
    IH.ITEM_ID;

```

## 2. [노선별 평균 역 사이 거리 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/284531)

![image](https://github.com/user-attachments/assets/e8917e1c-f5a1-434e-9d23-b2b21377cad0)

```SQL
SELECT
    ROUTE,
    CONCAT(ROUND(SUM(D_BETWEEN_DIST), 1), "km") AS TOTAL_DISTANCE,
    CONCAT(ROUND(AVG(D_BETWEEN_DIST), 2), "km") AS AVERAGE_DISTANCE
FROM
    SUBWAY_DISTANCE
GROUP BY
    ROUTE
ORDER BY
    SUM(D_BETWEEN_DIST) DESC;
```
> 간단한 줄 알았는데 생각치 못한 부분에서 애먹었다. 첫째로 문자열을 더해서 같이 출력해야 한다는 점이고, 둘째로는 문자열 형태로 변환된 컬럼을 숫자 기준으로 `ORDER BY` 할 수 없다는 것이다.
* `CONCAT` 함수: 지정한 문자열을 합쳐서 반환한다.
* `ROUND` 함수: MYSQL에서는 원하는 자릿수만큼 지정한다. (ex: 소수점 둘째 자리에서 반올림 = 1)

## 3. [헤비 유저가 소유한 장소](https://school.programmers.co.kr/learn/courses/30/lessons/77487)

![image](https://github.com/user-attachments/assets/86411774-457d-42bb-abe8-bdc7c6e9f8a2)

```SQL
SELECT *
FROM PLACES
WHERE HOST_ID IN (
    SELECT HOST_ID
    FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(ID) >= 2
)
ORDER BY ID;
```
> 메인 쿼리에서 바로 `GROUP BY`, `HAVING`절을 사용하면 그룹화된 하나의 행씩만 출력되므로 서브쿼리를 이용해 원하는 `HOST_ID`만 가져온 후 `WHERE`절을 이용해 필터링한다.
* `IN` 절에 대해 :
  > SQL에서 IN 절은 하나 이상의 값을 조건으로 비교할 때 사용됩니다. 기본적으로 IN 절은 지정된 값 목록 또는 서브쿼리의 결과와 비교하여, 해당 값이 그 목록에 포함되는지 여부를 확인합니다. 이를 통해 복수의 값을 비교할 때, OR 조건을 여러 번 사용해야 하는 번거로움을 줄일 수 있습니다
