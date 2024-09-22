# 강의 내용 정리
## 데이터 타입과 데이터 변환
  * 데이터 타입: 숫자,문자, 시간, bool 등...
  * 자료 타입을 변경하는 함수 : `CAST(1 AS STRING)`
  * `SAFE_CAST`: 변환이 실패할 경우 null 반환
  * 나누기를 할 경우 `SAFE_DIVIDE(x,y)` 함수 사용하기
## 문자열 함수
  * `CONCAT (문자열, 문자열)` : 여러 문자열을 붙여서 출력한다
  * `SPLIT(문자열, 기준인자)` : 문자열을 특정 인자를 기준으로 분할한다
  * `REPLACE(문자열, 찾을단어, 바꿀단어)` : 문자열의 특정 단어를 다른 단어로 교체한다.
  * `TRIM(문자열, 자를단어)` : 문자열에서 특정 단어를 삭제한다.
  * `UPPER(문자열)` : 문자열을 대문자로 변환
## 시계열 데이터, 함수
  * DATE : 날짜만 표시하는 데이터
  * TIME : 시간만 표시하는 데이터
  * DATETIME : 날짜와 시간까지 표시하는 데이터
  * TIMESTAMP : UTC부터 경과한 시간을 나타내는 값, timezone 정보 있음
  * millisecond(ms): 1000ms=1s
  * microseconds(us): 1000us=1ms
  * `EXTRACT(part FROM datetime_expression)` : DATETIME에서 특정 부분만 추출하고 싶은 경우. 요일은 DAYOFWEEK 사용
  * `DATETIME_TRUNC(datetime_col, part)` : 특정 파트 이후의 시계열만 남긴다 (버림)
  * `PARSE_DATETIME(문자열의 format, DATETIME문자열)` : 문자열로 저장된 datetime을 datetime타입으로 변환
  * `FORMAT_DATETIME(문자열 format, datetime)` : 파싱의 반대. datetime을 문자열로 변환
  * `LAST_DAY(DATETIME)` : 월의 마지막 값을 반환
  * `DATETIME_DIFF(first_datetime, second_datetime, part)` :  두 datetime 사이의 차이를 반환

# 문제풀이
## 1. [우리 플랫폼에 정착한 판매자](https://solvesql.com/problems/settled-sellers-1/)
 ![image](https://github.com/user-attachments/assets/b6fcfde5-6521-4e3b-bb23-0bbb2b7b9bee)
 
```sql
SELECT seller_id, count(DISTINCT order_id) AS orders
FROM olist_order_items_dataset
GROUP BY seller_id
HAVING count(DISTINCT order_id) >= 100;
```
 * 그룹바이를 하고 필터링을 할 때에는 WHERE 절이 아닌 HAVING 절을 사용한다.
 * conut를 할 때에는 잊지 말고 DISTINCT를 걸어주기 (상황에 따라)
 
## 2. [최고의 근무일을 찾아라](https://solvesql.com/problems/best-working-day/)
![image](https://github.com/user-attachments/assets/5b767d48-dfcd-44ce-aba6-ef96780c4da5)

```sql
SELECT day, ROUND(SUM(tip), 3) AS tip_daily
FROM tips
GROUP BY day
ORDER BY SUM(tip) DESC
LIMIT 1;
```
* 가장 팁이 많았던 날만 골라서 출력해야 하는데, 가장 먼저 MAX 함수가 생각이 나서 써봤는데 오류가 났다. `MAX(SUM())` 처럼 집계함수를 이중으로 사용할 수 없다는 것.
* 조금 더 쉽게 생각해서 내림차순으로 정렬하고 LIMIT 1을 걸어 해결했다.

## 3. [몇 분이서 오셨어요?](https://solvesql.com/problems/size-of-table/)
![image](https://github.com/user-attachments/assets/10c38666-de94-42f2-ae4b-13ba62e5730b)

```sql
SELECT *
FROM tips
WHERE size % 2 == 1;
```
* 나눗셈에서 몫을 구하는 연산자 "/"
* 나눗셈에서 나머지를 구하는 연산자 "%"


