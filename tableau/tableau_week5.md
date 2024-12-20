# Fifth Study Week

- 39강: [LOD](#39강-lod)

- 40강: [EXCLUDE](#40-lod-exclude)

- 41강: [INCLUDE](#41-lod-include)

- 42강 : [매개변수](#42-매개변수-필터)

- 43강 : [매개변수 실습](#43-매개변수-계산식) 
[링크](https://youtu.be/GJvB8hBqeE8?si=3jIj1iymZHZ7mBam)

- 44강: [매개변수 실습](#44-매개변수-참조선)

- 45강: [마크카드](#45-워크시트-마크카드)

- 46강: [서식계층](#46-서식-계층)

- 47강: [워크시트](#47-워크시트-서식)

- [문제1](#문제-1)

- [문제2](#문제-2)

## Study Schedule

| 강의 범위     | 강의 이수 여부 | 링크                                                                                                        |
|--------------|---------|-----------------------------------------------------------------------------------------------------------|
| 1~9강        |  ✅      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=84)       |
| 10~19강      | ✅      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=75)       |
| 20~29강      | ✅      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=65)       |
| 30~38강      | ✅      | [링크](https://www.youtube.com/watch?v=e6J0Ljd6h44&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=55)       |
| 39~47강      | ✅      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=45)       |
| 48~59강      | 🍽️      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=35)       |
| 60~69강      | 🍽️      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=25)       |
| 70~79강      | 🍽️      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=15)       |
| 80~89강      | 🍽️      | [링크](https://www.youtube.com/watch?v=AXkaUrJs-Ko&list=PL87tgIIryGsa5vdz6MsaOEF8PK-YqK3fz&index=5)        |


<!-- 여기까진 그대로 둬 주세요-->

> **🧞‍♀️ 오늘의 스터디는 지니와 함께합니다.**


## 39강. LOD

<!-- INCLUDE, EXCLUDE, FIXED 등 본 강의에서 알게 된 LOD 표현식에 대해 알게 된 점을 적어주세요. -->
> LOD란? level of detail. 현제 뷰의 영향을 받지 않고 본인이 원하는 세부 수준에서 계산을 수행
> fixed에서 설정한 차원이 뷰에 포함되어 있을 때 `{FIXED [지역] : SUM([매출])}`
> " 아닐 때 `{FIXED [범주] : SUM([매출])}`

## 40. LOD EXCLUDE

<!-- INCLUDE, EXCLUDE, FIXED 등 본 강의에서 알게 된 LOD 표현식에 대해 알게 된 점을 적고, 아래 두 질문에 답해보세요 :) -->

> **🧞‍♀️ FIXED와 EXCLUDE을 사용하는 경우의 차이가 무엇인가요?**

```
exclude lod는 현재 뷰에서 해당 차원을 무시하고 값을 반환한다. fixed는 특정 차원을 고정하여 계산한다. (예: 범주-하위범주:매출 나타내었을 때, 범주별 매출도 같이 나타내고 싶은 경우) 
첫번째 차이는 설정값보다 더 하위 범주가 추가되었을 때, fixed는 고정된 차원이 그대로 나타나지만, exclude는 해당 차원이 제외되었기 때문에 제외되지 않은 다른 차원(더 하위 범주)들이 나타난다.
두 번째 차이는 필터 설정 시 fixed값은 고정되지만 exclude 값은 변한다.
Fixed는 뷰에 영향을 받지 않고 정해진 차원에 따라 계산하지만 exclude는 뷰에서 계산하기 때문에 관련 차원의 영향을 받음
```

> **🧞‍♀️ 왜 ATTR 함수를 사용하나요?**

```
계산된 필드에서 그룹의 행이 단일 값을 가지지 않을 경우 계산이 불가능하기 때문에 집계함수 ATTR을 사용해 식이 유효하게 만들어준다.
```


## 41. LOD INCLUDE

<!-- INCLUDE, EXCLUDE, FIXED 등 본 강의에서 알게 된 LOD 표현식에 대해 알게 된 점을 적고, 아래 두 질문에 답해보세요 :) -->


> **🧞‍♀️ 그렇다면 어떤 경우에 각 표현식을 사용하나요? 예시와 함께 적어보아요**


```
include lod 표현식은 현재 뷰에서 특정 차원을 추가하여 계산한다. 특정 지역별 유저들의 평균 구매량을 필드로 만들고 싶을 때 유저id를 표시하지 않으면서 나타낼 수 있다.
```

## 42. 매개변수 - 필터 

<!-- 매개변수에 대해 알게 된 점을 적어주세요 -->

> 매개변수는 반드시 계산식/필터/참조선과 함께 사용한다.
> 상위 n개의 결과를 표시하고 싶은데 n을 바꿀 때 마다 필터에 들어가서 바꾸기 번거롭기 때문에 매개변수를 사용한다.

> **🧞‍♀️ 집합에도 매개변수를 적용할 수 있나요? 시도해봅시다**
```
네
```

## 43. 매개변수 - 계산식

<!-- 영상 묶음에 포함되지 않아 찾기 어려우실까 링크를 아래에 첨부하겠습니다. 수강 후 삭제해주세요-->


## 44. 매개변수 - 참조선

<!-- 매개변수에 대해 알게 된 점을 적어주세요 -->
> 분석 -> 참조선 -> 매개변수만들기 / 이후 계산된 필드에서 조건 설정 가능


## 45. 워크시트 마크카드

<!-- 마크카드에 대해 알게 된 점을 적어주세요 -->
> 기존 동영상에 조금씩 나왔던 내용의 정리


## 46. 서식 계층

<!-- 서식계층에 대해 알게 된 점을 적어주세요 -->

> **🧞‍♀️ 서식계층을 일반적인 것에서 구체적인 것 순서로 기입해보세요**


```
워크시트 서식 > 행/열 서식 > 특정 필드 > 필드 레이블 > 도구설명/제목/마크
하위 계층의 서식이 우선 적용된다.
```


## 47. 워크시트 서식 - 글꼴, 맞춤, 음영

<!-- 워크시트 서식에 대해 알게 된 점을 적어주세요!-->



## 문제 리스트



## 문제 1.

```
가장 많이 주문한 사람들은 물건 배송을 빨리 받았을까요?
조건을 준수하여 아래 이미지를 만들어봅시다.
1) 국가/지역별(이하 '나라'로 통칭), 범주별로 배송일자가 다를 수 있으니 먼저, 나라별/범주별로 평균 배송일자를 설정한 뒤,
2) 각 나라에서 가장 많이 주문한 사람의 이름을 첫 번째 열,
3) 그 사람이 주문한 제품 이름을 2번째 열,
4) 각 상품이 배송까지 걸린 날 수를 표현하고
5) 그리고 만약 배송이 각 나라/범주별 평균보다 빨랐다면 '빠름', 같다면 '평균', 느리다면 '느림' 으로 print 해주세요. 
```

![이미지주소](https://github.com/yousrchive/BUSINESS-INTELLIGENCE-TABLEAU/blob/main/study/img/2nd%20study/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202024-08-13%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.12.36.png?raw=true)
___
<!-- 여기까지 오는 과정 중 알게 된 점을 기입하고, 결과는 시트 명을 본인 이름으로 바꾸어 표시해주세요.-->
![image](https://github.com/user-attachments/assets/26b2c849-c974-4576-b94b-18e0b893d837)

```
먼저 각 fixed lod를 통해 각 국가별 평균 배송일자를 구하는 필드를 만든다.
배송일자를 구하는 필드를 만들고, 실제 배송일자를 평균 배송일자에서 빼서 배송속도수준을 구하는 필드를 만든다.
고객 이름 행에서 필터를 주문수 기준 내림차순 해준다.
국가마다 가장 주문이 많은 사람만 표시하는 방법은 잘 모르겠다. 계산된 필드에서 top 함수를 사용하면 되는 것 같긴 한데 꽤 복잡해서 더 쉬운 방법이 있는지 모르겠다.
```

## 문제 2.

```
채원이는 태블로를 쓰실 수 없는 상사분께 보고하기 위한 대시보드를 만들고 싶어요. 

제품 중분류별로 구분하되 매개변수로써 수익, 매출, 수량을 입력하면 저절로 각각 지표에 해당하는 그래프로 바뀌도록 설계하고자 해요.

 어떤 값이 각 지표의 평균보다 낮은 값을 갖고 있다면 색깔을 주황색으로, 그것보다 높다면 파란색으로 표시하고 싶어요. 그 평균값은 각 지표별로 달라야 해요.
```

![image](https://github.com/user-attachments/assets/2804133a-f55c-4b0d-9292-c88e14e6f9dd)

<!-- 예시 사진은 지워주세요-->

> 측정값마다 전체평균을 다시 계산해서 각 항목의 평균에서 빼는 방법을 모르겠다. 계속 평균 - 평균 = 0 이 되어서 0보다 작으면 주황색, 0보다 크면 파란색으로 표시되는 것 같다.
