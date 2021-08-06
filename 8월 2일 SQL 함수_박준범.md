## 4. 함수

**1) 실행 구분**

- 단일함수
- 그룹함수

**2) 카테고리별 구분**

- 문자열 관련함수
- 숫자함수
- 변환함수
- 날짜함수

**3) 개요**

- where절에 그룹함수를 사용 할 수 없다.
- select절에 일반 컬럼과 그룹함수를 같이 사용할 수 없다.

**4) 문자열 함수**

```sql
lower(문자열) - 문자열을 소문자로 변환
upper(문자열 or 컬럼) - 문자열을 대문자로 변환
concat(문자열 or 컬럼) - || 연산자와 동일하게 실행
substr(문자열,숫자1,숫자2) - 전체 문자열에서 원하는 문자열만 추출
instr(문자열1,문자,숫자1,숫자2) - 특정 문자의 위치를 추출
·문자열1 : 문자를 찾을 문자열이나 컬럼
·문자 : 찾을 문자
·숫자1 : 찾기 시작할 위치
·숫자2 : n번째 만나는 숫자
iength(문자열or컬럼명) - 문자열의 길이를 리턴
initcap(문자열or컬럼) - 문자열의 첫글자를 대문자로
lpad(문자열1, 숫자, 문자) - 문자열1을 지정된 숫자의 길이만큼 표시, 남는 자리는 문자로 채움
·문자열 1 : 원본문자
·숫자 : 표시할 문자열의 길이
·문자 : 채울 문자
ltrim(문자열or컬럼,삭제할 문자) - 지정된 컬럼이나 문자열에서 연속으로 나오는 삭제할 문자를 지움
```

**5) 숫자 함수**

- round

```sql
round(반올림할 숫자나 컬럼, 숫자)
```

· 지정한 자리로 반올림

· 숫자에 양수, 0, 음수

- trunc

```sql
trunc(버림을 수행할 숫자나 컬럼, 숫자)
```

· 지정한 자리로 버림

- ceil

```sql
ceil(숫자나 컬럼)
```

· 주어진 값보다 큰 가장 최소값을 구하는 함수

- mod

```sql
mod(숫자1,숫자2)
```

· 숫자1을 숫자2로 나눈 나머지

- abs

```sql
abs(숫자나 컬럼)
```

· 절대값

**6) 날짜관련 함수**

- months_between

```sql
months_between(날짜1, 날짜2)
```

· 날짜1과 날짜2 사이의 경과한 개월의 수, 날짜1이 최근 날짜

- add_months

```sql
add_months(날짜1, 숫자)
```

· 날짜에 숫자개월을 더함

- last_day(날짜)

- next_day(날짜,요일번호)

  · 정의된 날짜 이후의 날짜 데이터에서 지정한 요일에 해당

**7) 변환 함수**

· 데이터 타입을 변환할때 사용하는 함수

- 숫자 → 문자

```sql
to_char
```

- 문자 → 숫자

```sql
to_number
```

- 문자 → 날짜

```sql
to_date
```

· 숫자 → 문자                                                날짜 → 문자

```
to_ char                                                         to_char

              숫자 format을 나타낼때 사용 (특수문자를 사용해서 표시)

 9,0 : 숫자                                                 YYYY,YY : 년도 데이터

   ,   : 콤마                                                 MM   : 월

   .   : 소수점                                               DD    : 일

  $,L: 통화기호                                                DY    : 요일
```

**8) null 관련 함수**

- nvl

```sql
nvl(null이 있는 컬럼, 변경할 값)
```

· null 값을 특정 값으로 변환

· null 인 경우 치환할 값은 반드시 컬럼의 데이터 타입과 일치 만약 다른 값을 넣고 싶으면 컬럼의 타입을 변경

- nvl2(컬럼, 값1, 값2)

null을 평가해서 null이면 값2를 null아니면 값1을 리턴

## 5. 그룹(데이터집계)

- 여러 레코드의 데이터를 묶어서 처리하는 방식
- 통계작업을 위해 사용
- 그룹함수를 이용해서 처리
- 여러 레코드를 묶어서 처리 즉, 그룹당 하나의 결과를 리턴하는 함수

##### [구문]

select 컬럼(group by에 정의한 컬럼명만), 그룹함수....

```sql
from 테이블명1, 테이블2....

where 조건

group by 컬럼명(그룹화 하고 싶은 기준)

having 그룹화된 데이터에 명시할 조건

order by 컬럼
```

##### [개요]

- select문에는 일반 데이터조회 처럼 아무 컬럼이나 정의할 수 없다.

group by에 명시한 컬럼만 정의할 수 있다.

- where절을 group by보다 먼저 실행하여 레코드를 그룹으로 나누기 전에

제외한다.

- group by전에 조건을 적용해야 한다면 where절에 명시
- group by한 후에 만들어지는 결과에 조건을 적용해야 하면 where절에 명시하지

않고 having절에 추가해야 한다.

##### [실습]

직업별 인원수와 평균sal를 출력하세요

```sql
select job, count(empno), avg(sal)
from emp
group by job;
```

##### [실습]

입사월별로 인원수를 출력하세요. 단,sal이 5000이상인 사원은 제외

입사자 수가 1명인 월은 출력 제외

hint) 그룹화 : 입사월

to_char

```sql
select to_char(hiredate, 'MM') , count(empno)
from emp
where sal<5000
group by to_char(hiredate, 'MM')
having count(empno)>1
order by to_char(hiredate, 'MM');
```

##### [실습]

부서별로 최대급여, 최소급여를 출력하세요.

job이 PRESIDENT인 데이터는 제외하고 최대급여가 3000이상인 부서만 출력하세요

부서별로 오름차순정렬하기

```sql
select deptno, max(sal) 최대급여 , min(sal) 최소급여
from emp
where job<>'PRESIDENT'
group by deptno
having max(sal)>=3000
order by deptno;
```

##### [select문의 정의]

select -> from -> where -> group by -> having -> order by

##### [select문의 실행 순서]

from -> where -> group by -> having -> select -> order by

ORA-00911: invalid character







 

















