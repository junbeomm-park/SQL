# 08.03 SQL 조인





## 1. 관계형 데이터 베이스

- 2차원 테이블로 데이터를 저장

- 데이터의 무결성을 보장

- 데이터 처리를 위해서 SQL문을 사용

- 테이블 설계시 정의할 수 있는 컬럼

  · 기본키 (primary key) : 테이블에서 레코드를 구분하기 위해서 사용

  ​                                   중복과 공백을 허용하지 않는다.

  · 외래키 (foreign key) : 기본키를 참조하는 키

  ​                                        테이블과 테이블의 관계를 나타내기 위해  사용

  ​                                        기본키에 정의 되지 않은 값을 사용 할 수 없다.

- 참조 무결성이 강화

- 테이블을 분리해서 설계 (테이블 정규화)

  · 테이블 정규화

  하나의 테이블로 포현하기에 중복이 많이 발생하는 경우 두개 이상의 테이블로 정의하여 관리

- 관계형 데이터 베이스에서 테이블은 성격별로 각각 정의해서 관리

## 2. 조인

[구문]

```sql
select 컬럼명1, 컬럼명2....
from 테이블명1 e, 테이블명2 d...
where e.테이블명 = d.테이블명
```

- 조인 방법

  · from절에 조회하고 싶은 테이블을 모두 정의

  · 컬럼명을 정의할 때, 컬럼명의 모호성을 피하기 위해 테이블 표시

  · 조인을 하는 경우 반드시 어떤 컬럼끼리 관계가 있는지 명시 해야한다.

  즉, 어떤 컬럼의 값을 기준으로 원하는 값을 찾아와야 하는지 알아야 적절한 데이터를 가져옴

  · 비교해야 하는 컬럼을 명시하는 것을 조인조건 이라 한다.

  · 조인조건은 where 절에 정의

  · 일반적으로 조인조건은  pk테이블의 pk와 fk 테이블의 fk 키가 = 연산자로 처리

  · 조인조건은 테이블이 N개 이면 N-1개의 조건이 필요하다.

  · 조인조건을 정의하지 않거나 잘못 정의하는경우 어떻게 비교해야 할지 애매하므로 from절에 명시된 모든 테이블의 레코드를 조회해서 출력. 이를 카티션 프로덕트라 한다.

  · 조건이 1개 있다면 조인조건과 무조건 and 조건으로 연결

  · select절에 두개이상의 테이블의 컬럼을 표시하지 않는다고 하더라도 조건에 사용되면 조인조건을 명시해야 한다.

  이때, 직접적으로 사용되지 않은 테이블도 조건을 위해 필요하다면 명시.

[실습]    scott 계정

SALESMAN의사원번호, 이름, 급여, 부서명, 근무지코드를조회

```sql
select e.empno,e.ename,e.sal,d.dname,d.loc_code
from emp e , dept d
where job = 'SALESMAN'
          and e.deptno = d.deptno;
```

[실습2]   hr 계정

IT 부서에서 일하는 직원의 first_name, last_name, salary 를 출력하시요. 출력결과는 salary 가 낮은 사람부터 출력하시요. 전체 결과는 아래 수행결과처럼 하나의 문자열로 결합되어서 나와야 하며 전체 결과 하나의 컬럼으로 출력되도록 작성하세요.

```sql
select e.first_name || e.last_name  || '의 연봉은' || e.salary || '입니다' as 결과
from employees e , departments d
where e.department_id = d.department_id
         and department_name = 'IT'
order by salary asc;
```

[실습3]

각 사원(employee)에 대해서 사번(employee_id), 이름(first_name), 업무명(job_title), 부서명(department_name)를 조회하시오. 단 도시명(city)이 ‘Seattle’인 지역(location)의 부서(department)에 근무하는 직원만 출력하시오.

```sql
select e.employee_id, e.first_name , j.job_title , d.department_name
from employees e , departments d, locations l , jobs j
where e.job_id = j.job_id and
e.department_id = d.department_id
and d.location_id = l.location_id and
l.city = 'Seattle';
```

[실습4]

각 직책 별(job_title)로 급여의 총합을 구하되 직책이 Representative 인 사람은 제외하십시오. 단, 급여 총합이 30000 초과인 직책만 나타내며, 급여 총합에 대한 오름차순으로 정렬하십시오. 출력 결과의 컬럼명은 아래 결과와 동일하게 주십시오.

```sql
select j.job_title JOB , sum(e.salary) 급여
from jobs j , employees e
where j.job_id = e.job_id
      and j.job_title <> 'Sales Representative'
having sum(e.salary)>30000
group by j.job_title
order by sum(e.salary) asc;
```

[실습5]

각 부서 이름 별로 2005년 이전에 입사한 직원들의 인원수를 조회하시오

```sql
select d.department_name 부서명, count(e.employee_id) 인원수
  from departments d , employees e
  where e.department_id = d.department_id
                     and hire_date<'05/01/01'
   group by d.department_name;
```

[실습6]

사원수가 3명 이상의 사원을 포함하고 있는 부서의 부서번호(department_id), 부서이름(department_name), 사원 수, 최고급여, 최저급여, 평균급여, 급여총액을 조회하여 출력하십시오. 출력 결과는 부서에 속한 사원의 수가 많은 순서로 출력하고, 컬럼명은 아래 결과와 동일하게 출력하십시오. (평균급여 계산시 소수점 이하는 버리시오)

```sql
select d.department_id 부서번호, d.department_name 부서명, count(*) 인원수, max(e.salary) 최고급여, min(e.salary) 최저급여, round(avg(e.salary),0) 평균급여, sum(e.salary) 급여총액
from departments d , employees e
where d.department_id = e.department_id
group by d.department_id, d.department_name
having count(*)>=3
order by count(e.employee_id) desc;
```





 

















