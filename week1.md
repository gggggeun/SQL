>select/orderby/select distinct/where/In/Between/Like/Isnull/Limit/Fetch

### 1. SELECT/FROM
: select for specific columns of data from a table

```
SELECT 열이름1,열이름2 FROM 테이블이름;

ex) SELECT * FROM MOVIES (영화테이블에서 전체 열 선택)
```

 
### 2. WHERE
: In order to filter certain results from being returned, we need to use a WHERE clause in the query.

```
SELECT * FROM movies
WHERE 조건1 AND/OR 조건2;

ex) SELECT * FROM MOVIES 영화테이블 전체 열 선택
WHERE Year BETWEEN 2000 and 2010 AND Titles LIKE "%Toy%"
2000~2010년 개봉과 Toy 글자가 들어가는 영화(행) 선택
```
![image](https://user-images.githubusercontent.com/74692845/128552732-9414811b-b884-4e29-a2ae-52a9636ae6b0.png)
![image](https://user-images.githubusercontent.com/74692845/128552737-d58d980c-816e-406c-a7f0-ae371c351bad.png)

* WHERE IS NULL / IS NOT NULL
: 데이터베이스에서의 NULL의 대안은 0이나 -와 같은 적절한 기본값을 사용하는 것이지만, 이것은 자료를 왜곡할 수도 있다. 때론 NULL을 피할 수 없는 경우(비대칭 자료)도 있다. 이럴땐 WHERE IS/IS NOT NULL로 컬럼을 확인해볼 수 있다.

 ```
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

### 3. DISTINCT
: 선택한 열의 중복제거

```
SELECT DISTINCT 열1,열2 FROM 테이블이름
WHERE condition(s);
```


### 4. ORDER BY
: 정렬 (ASC:오름차순, DESC: 내림차순)

```
SELECT 열1,열2 FROM mytable
WHERE condition(s)
ORDER BY column ASC(Defalt)/DESC;
```


### 5. LIMIT(=FETCH))/OFFSET
: 제한된 행이있는 쿼리 선택

- LIMIT :반환 할 행 수를 줄임
- OFFSET(선택): 시작할 행 숫자 지정(그 다음 행 부터 출력)

```
SELECT column, another_column, …
FROM mytableWHERE condition(s)
ORDER BY column ASC/DESCLIMIT num_limit 
OFFSET num_offset;

ex)
SELECT Title FROM Movies
ORDER BY Title
LIMIT 5 OFFSET 5 영화제목 알파벳 순서 6번째부터 5개
```

### 6. CONCAT, ||
: 문자열 이어 붙이기, 합치기

- concat(문자열1, 문자열2, 문자열3)

```
select concat(first_name,' ',last_name) from actor ;
```

- 문자열1||문자열2||문자열3

```
select distinct upper(first_name||' '||last_name) from actor ;
```

<주의>
- MySQL에서 ||는 문자열 합치기가 아닌 OR(또는)을 뜻한다.
- Oracle에서 concat 사용시 매개변수를 두 개만 허용하기 때문에 concat(concat(문자열1, 문자열2), 문자열3) 와 같이 사용할 수 있다. 


### 7. '_1%', SUBSTRING
: 두번째 글자가 1인 문자열 추출

-  WHERE 컬럼 LIKE  '_문자열%'

```
select * from address
where postal_code like '_1%' ;
```
 ex) 세번째 문자열이 1인 문자열 :  '__1%'
 
- SUBSTRING(컬럼,*번째문자열,문자열개수)

```
select address_id , address, district, postal_code from address
where substring(postal_code,2,1) = '1';
```
 ex) 세번째 문자열이 1인 문자열 :  substring(postal_code,3,1) = '1'
 
- 그 외 substring 사용하는 법
  - 앞의 4자리(년도)만 추출 : substring('20210807', 1, 4) -> '2021'
  - 5번째부터 2자리(월)만 추출 : substring('20210807', 5, 2) -> '08'
 
 
 ### 8. EXTRACT(A FROM B)
 : 년도,월,일,시간 등 추출하기
 
 - A (Field 값) : year, month, day 와 같은 추출할 날짜/시간
 ![image](https://user-images.githubusercontent.com/74692845/128629614-2090b551-5f4d-4e34-998a-fe276aa83e25.png)


- B (Source 값) : timestamp 데이터(예. '2021-08-08 00:00:00')

<CODE 예시>

```
select empbirthdate, extract(year from empbirthdate) as birth_year from employees;
```
![image](https://user-images.githubusercontent.com/74692845/128629656-4f806246-60dd-4256-adee-1ccdd55267bd.png)


### 9. REPLACE(컬럼, '문자열', '바꿀문자')
: 문자열 한개 치환

```
select customerid, custstate, replace(custstate,'A','000')
from customers;
```

### 10. REGEXP_REPLACE(컬럼, '문자열1|문자열2|문자열3', '바꿀문자')
: 다중 문자열 치환

```
--custstate 지역 중 WA 지역에 사는 사람과 WA 가 아닌 지역에 사는 사람을 구분해서 보여주세요.--
select customerid, custstate, regexp_replace(custstate,'TX|OR|CA','Others') as newstate_flag
from customers;
```
