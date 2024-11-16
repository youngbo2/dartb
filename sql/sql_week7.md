(https://school.programmers.co.kr/learn/courses/30/lessons/59410)

**동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성합니다.**

이때, 이름이 없는 동물의 이름은 ‘No name’으로 합니다.

**문제1. IFNULL()으로 해결**

```sql
// IfNULL() 함수 문법을 익히고 사용하여 해결해주세요.
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name") AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

같은 문제를, CASE WHEN 문법을 사용하여 해결해주세요

**문제2. CASE WHEN으로 해결**

```sql
//CASE WHEN 문법을 익히고 사용하여 해결해주세요.
SELECT
    ANIMAL_TYPE,
    CASE
        WHEN NAME IS NULL THEN "No name"
        ELSE NAME
    END AS NAME,
    SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

**-중성화 여부 파악하기**

(https://school.programmers.co.kr/learn/courses/30/lessons/59409#qna)

**문제 3. 문제를 풀어주세요 (힌트: IF, LIKE를 사용할 수 있습니다)**

```sql
SELECT
    ANIMAL_ID,
    NAME,
    CASE
        WHEN SEX_UPON_INTAKE LIKE "%Neutered%" OR SEX_UPON_INTAKE LIKE "%Spayed%" THEN "O"
        ELSE "X"
    END AS 중성화
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

**문제 4. 아래는 QnA에 올라온 질문입니다. 왜 풀이가 틀렸는지 답해주세요.**

(https://school.programmers.co.kr/questions/80270)

오류코드
```sql
SELECT animal_id, name,
       if (sex_upon_intake like '%Neutered%' or '%Spayed%', 'O' , 'X') as '중성화'
from animal_ins
order by 1;
```

> `LIKE`연산자는 특정 문자열을 비교할 때만 사용할수 있으므로 쿼리의`or '%Spayed%'` 부분이 항상 참으로 표시된다.
>
> `IF (sex_upon_intake LIKE '%Neutered%' OR sex_upon_intake LIKE '%Spayed%', 'O' , 'X') AS '중성화'` 와 같이 LIKE 연산자를 다시 사용해줘야 한다.
>
> 또한 해당 쿼리는 가독성이 매우 떨어지므로 이를 신경쓰는 것이 좋다.


수정코드
```sql
SELECT
    ANIMAL_ID,
    NAME,
    IF (SEX_UPON_INTAKE LIKE '%Neutered%' OR SEX_UPON_INTAKE LIKE '%Spayed%', 'O' , 'X') AS '중성화'
FROM
    ANIMAL_INS
ORDER BY
    ANIMAL_ID;
```
