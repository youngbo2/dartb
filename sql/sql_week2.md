# 강의 내용 정리
## 데이터 타입과 데이터 변환
  * 데이터 타입: 숫자,문자, 시간, bool 등...
  * 자료 타입을 변경하는 함수 : CAST(1 AS STRING)
  * SAFE_CAST: 변환이 실패할 경우 null 반환
  * 나누기를 할 경우 SAFE_DIVIDE(x,y) 함수 사용하기
## 문자열 함수
  * CONCAT (문자열, 문자열) : 여러 문자열을 붙여서 출력한다
  * SPLIT(문자열, 기준인자) : 문자열을 특정 인자를 기준으로 분할한다
  * REPLACE(문자열, 찾을단어, 바꿀단어) : 문자열의 특정 단어를 다른 단어로 교체한다.
  * TRIM(문자열, 자를단어) : 문자열에서 특정 단어를 삭제한다.
  * UPPER(문자열) : 문자열을 대문자로 변환
## 시계열 데이터, 함수
  * DATE : 날짜만 표시하는 데이터
  * TIMe : 시간만 표시하는 데이터
  * DATETIME : 날짜와 시간까지 표시하는 데이터
  * TIMESTAMP : UTC부터 경과한 시간을 나타내는 값, timezone 정보 있음
  * millisecond(ms): 1000ms=1s
  * microseconds(us): 1000us=1ms
  * EXTRACT(part FROM datetime_expression) : DATETIME에서 특정 부분만 추출하고 싶은 경우. 요일은 DAYOFWEEK 사용
  * DATETIME_TRUNC(datetime_col, part) : 특정 파트 이후의 시계열만 남긴다 (버림)
  * PARSE_DATETIME(문자열의 format, DATETIME문자열) : 문자열로 저장된 datetime을 datetime타입으로 변환
  * FORMAT_DATETIME(문자열 format, datetime) : 파싱의 반대. datetime을 문자열로 변환
  * LAST_DAY(DATETIME) : 월의 마지막 값을 반환
  * DATETIME_DIFF(first_datetime, second_datetime, part) :  두 datetime 사이의 차이를 반환

# 문제풀이
## 1. 우리 플랫폼에 정착한 판매자

