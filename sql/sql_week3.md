
## 1. [조건에 맞는 사용자와 총 거래금액 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164668)

![image](https://github.com/user-attachments/assets/ac4a9689-cf2f-4972-9d51-2d3e8968c420)

```sql
SELECT U.USER_ID, U.NICKNAME, SUM(B.PRICE) AS TOTAL_SALES
FROM USED_GOODS_USER U
LEFT JOIN USED_GOODS_BOARD B ON U.USER_ID = B.WRITER_ID AND B.STATUS = 'DONE'
GROUP BY U.USER_ID, U.NICKNAME
HAVING SUM(B.PRICE) >= 700000
ORDER BY TOTAL_SALES ASC;
```
* 필터링을 조인할 때 걸어줘야 한다


## 2. [업그레이드 할 수 없는 아이템 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/273712)

![image](https://github.com/user-attachments/assets/822b361b-aa06-403f-a941-b7e3727ec444)

```sql
SELECT I.ITEM_ID, ITEM_NAME, RARITY
FROM ITEM_INFO I
LEFT JOIN ITEM_TREE T ON T.PARENT_ITEM_ID = I.ITEM_ID
WHERE PARENT_ITEM_ID IS NULL
ORDER BY I.ITEM_ID DESC;
```
* 생각보다 어려웠던 문제... 조인은 당연히 같은 컬럼끼리 하는거라고 생각하면 풀 수 없다. 어떤 ITEM_ID가 PARENT_ITEM_ID에 등장하지 않는 경우만 찾아야 한다. 
* left join을 `T.PARENT_ITEM_ID = I.ITEM_ID` 이런 방식으로 하면 item_info 테이블의 ITEM_ID 컬럼의 값이 PARENT_ITEM_ID 컬럼에 없을 경우 null값이 채워진다.
* 원하는 조건을 만들었으므로 출력한다.


## 3. [조건에 맞는 개발자 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/276034)

![image](https://github.com/user-attachments/assets/74836529-313c-4baa-ab5d-b9b6a1cbcfa7)

```sql
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPERS
WHERE SKILL_CODE & (
    SELECT SUM(CODE)
    FROM SKILLCODES
    WHERE NAME IN ("Python", "C#")
    ) != 0
ORDER BY ID ASC;
```
* 지난 학기에 풀었던 문제인데... 풀이 기록이 남아있다.
* 비트 연산자 &는 비트 AND 연산을 수행한다. 예를 들어, 5 & 3 은 비트로 101 AND 011이므로 연산 수행 시 001이 된다. (1의 자리 수만 and 조건을 충족하므로) 
* 이 문제에서는 “Python”에 해당하는 수(2의 n승 형태, 예를들어 이진수로 1000...0)와 “C#”에 해당하는 수를 더하면(SUM), 각 프로그램을 나타내는 자릿수만 1인 이진수의 형태가 된다. (e.g. 10001000, 내가 원하는 조건의 자릿수만 1인 형태) 
해당 값을 SKILL_CODE 값과 & 연산하면, 어떤 개발자에게 우리가 원하는 프로그래밍 능력이 있는 경우 개발자의 SKILL_CODE 값과 내가 원하는 조건의 수가 and 조건을 충족하므로 0이 아닌 값을 반환한다. (반면 겹치는게 없을 경우 0을 반환)




