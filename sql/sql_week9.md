# 1. PIVOT과 UNPIVOT
  * pivot: 행을 열로 바꾼다. 즉, 레코드를 오른쪽으로 펼치고 컬럼에 따라서 나타냄. 보기에 편하다는 장점이 있음.
    ```sql
    SELECT *
    FROM (
        SELECT E.JOB, D.DNAME
        FROM EMP E, DEPT D
        WHERE E.DEPTNO = D.DEPTNO
    )
    PIVOT (
        COUNT(*) FOR DNAME IN (
            'ACCOUNTING' AS ACCOUNTING,
            'RESEARCH' AS RESEARCH,
            'SALES' AS SALES
        )
    );
    ```
  * unpivot: 피봇의 반대. 열을 행으로 바꾼다. 그룹바이나 집계연산을 할 수 있도록 각 행이 하나의 레코드가 되도록 한다.
    ```sql
    SELECT 계절, 연도, 기온
    FROM (SELECT * FROM 평균기온)
    UNPIVOT (
        기온 FOR 연도 IN (
            Y2018 AS '2018년',
            Y2019 AS '2019년',
            Y2020 AS '2020년',
            Y2021 AS '2021년',
            Y2022 AS '2022년'
        )
    );
    ```
  ### 문제
  (https://www.hackerrank.com/challenges/occupations/problem)
  ```sql
  SELECT
      MAX(CASE WHEN Occupation = 'Doctor' THEN Name END) AS Doctor,
      MAX(CASE WHEN Occupation = 'Professor' THEN Name END) AS Professor,
      MAX(CASE WHEN Occupation = 'Singer' THEN Name END) AS Singer,
      MAX(CASE WHEN Occupation = 'Actor' THEN Name END) AS Actor
  FROM (
      SELECT
          Name,
          Occupation,
          ROW_NUMBER() OVER (PARTITION BY Occupation ORDER BY Name) AS RowNum
      FROM OCCUPATIONS
  ) AS Ranked
  GROUP BY RowNum
  ORDER BY RowNum;
  ```

# 2. 성능 최적화 기법
  https://community.heartcount.io/ko/query-optimization-tips/
  1. 데이터를 변형하는 연산은 가급적 피하고, 원본 데이터를 직접 비교하는 조건을 사용하세요. 이는 인덱스 활용도를 높여 쿼리 속도를 향상시킵니다.
  2. OR 연산자 대신 UNION을 활용하면 각 조건을 독립적으로 최적화하고 인덱스를 효과적으로 사용할 수 있습니다.
  3. 불필요한 Row와 Column을 제외하고 꼭 필요한 데이터만 조회하세요. 이는 데이터 처리량을 최소화하여 쿼리 성능을 높입니다.
  4. ROW_NUMBER(), RANK(), LEAD(), LAG() 등의 분석 함수를 적극 활용하면 복잡한 데이터 분석을 유연하고 효율적으로 수행할 수 있습니다.
  5. LIKE 연산자와 와일드카드(%)를 사용할 때는 문자열 끝에 와일드카드를 두는 것이 인덱스 활용에 유리합니다.
  6. 복잡한 계산은 실시간으로 처리하기보다는 미리 계산해서 저장해두고 주기적으로 업데이트하는 것이 효율적입니다.


## 문제
  여러분은 `customer_orders`라는 테이블을 관리하는 데이터베이스 관리자로 일하고 있습니다. 이 테이블에는 고객의 주문 정보가 저장되어 있으며, 각 고객이 주문한 제품과 수량, 가격 정보가 포함되어 있습니다. 또한, 고객들이 특정 제품을 재구매한 비율을 계산하려고 합니다.

  ### 테이블 구조:
  
  1. **customers** 테이블
      - `customer_id` (고객 ID, PRIMARY KEY)
      - `name` (고객 이름)
  2. **orders** 테이블
      - `order_id` (주문 ID, PRIMARY KEY)
      - `customer_id` (고객 ID, FOREIGN KEY)
      - `order_date` (주문 날짜)
  3. **order_details** 테이블
      - `order_id` (주문 ID, FOREIGN KEY)
      - `product_id` (제품 ID)
      - `quantity` (수량)
      - `unit_price` (단가)
  
  ---
  
  ### 요구 사항:
  
  1. **`avg_order_value`**: 고객별 평균 주문 금액을 계산하여 `customers` 테이블에 업데이트하세요.
      - `avg_order_value`는 각 고객이 한 번의 주문에서 지출한 평균 금액입니다.
  2. **`total_spent`**: 고객별 총 지출 금액을 계산하여 `customers` 테이블에 업데이트하세요.
      - `total_spent`는 고객이 지금까지 지출한 총 금액입니다.
  3. **`num_orders`**: 고객이 총 몇 번의 주문을 했는지 계산하여 `customers` 테이블에 업데이트하세요.
      - `num_orders`는 고객이 주문한 총 개수입니다.
  4. **`repurchase_rate`**: 고객의 재구매 비율을 계산하여 `customers` 테이블에 업데이트하세요.
      - `repurchase_rate`는 각 고객이 2번 이상 주문한 제품 비율을 의미합니다. (즉, 재구매한 제품이 전체 구매 제품 중 몇 퍼센트를 차지하는지)
  
  ### 예시:
  
  - 고객 A는 3번 주문을 했고, 그 중 2개의 제품을 재구매했습니다.
      - 평균 주문 금액: 100,000원
      - 총 지출 금액: 300,000원
      - 주문 횟수: 3번
      - 재구매 비율: 66.67%
   
  ## 정답
```sql
      UPDATE customers c
      SET
      avg_order_value = (
          SELECT AVG(od.quantity * od.unit_price)
          FROM order_details od
          JOIN orders o ON od.order_id = o.order_id
          WHERE o.customer_id = c.customer_id
      ),
      total_spent = (
          SELECT SUM(od.quantity * od.unit_price)
          FROM order_details od
          JOIN orders o ON od.order_id = o.order_id
          WHERE o.customer_id = c.customer_id
      ),
      num_orders = (
          SELECT COUNT(DISTINCT o.order_id)
          FROM orders o
          WHERE o.customer_id = c.customer_id
      ),
      repurchase_rate = (
          SELECT
              COUNT(DISTINCT CASE WHEN od.product_id IN (
                  SELECT product_id
                  FROM order_details
                  JOIN orders o ON order_details.order_id = o.order_id
                  WHERE o.customer_id = c.customer_id
                  GROUP BY order_details.product_id
                  HAVING COUNT(order_details.product_id) > 1
              ) THEN od.product_id END) * 1.0 / COUNT(DISTINCT od.product_id)
          FROM order_details od
          JOIN orders o ON od.order_id = o.order_id
          WHERE o.customer_id = c.customer_id
      );
```
