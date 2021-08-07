##  서브쿼리





### 1. 개요

> 쿼리 안에 포함된 쿼리를 의미
>
> >  select 문에 삽입된 select 문을 의미





[구문]

```
select 컬럼명
from 테이블명
where 컬럼명 연산자 (select 컬럼명
                    from  테이블
                    where XXX)
                    
 
```

* 서브쿼리는 ( )를 삽입하여 작업하기
* 서브쿼리도 일반 쿼리 처럼 group by , join 모두 사용이 가능
* 서브쿼리는 메인쿼리(기본쿼리) 실행 전에 한 번 만 실행된다.
* where 절에 서브쿼리를 삽입하는 경우 비교조건의 오른쪽에 쿼리를 삽입
* 보통 서브쿼리의 결과가 행 하나 컬럼 하나인 경우가 대부분
* select , from, where, having, group by절에 서브퀴를 사용할 수 있다.
* insert문, delete문, update문, select문 모두 서브쿼리를 사용할 수 있다.





**[실습]** scott 계정



1. 10번 부서의 평균 급여 보다 급여가 많은 사원의 목록을 조회

```sql
select *
from emp
where sal > (select avg(sal) from emp where deptno = 10);
```



2. SMITH 와 같은 부서의 사원의 목록을 조회

```sql
select *
from emp
where deptno = (select deptno from emp where ename = 'SMITH');
```



3. 7566번 사원보다 급여를 많이 받는 사원의 이름과 급여를 조회

```sql
select ename, sal
from emp
where sal > (select sal from emp where empno = '7566');
```



4. 이름에 'T'를 포함하고 있는 사원과 같은 부서에서 근무하는 사원의 이름과 사원번호 출력

```sql
 select ename, empno
 from emp
 where deptno in (select deptno from emp where ename like '%T%');
```



5. 20번 부서의 최고 급여보다 급여가 많은 사원들의 사원명, 부서코드, 급여 목록 출력하기

```sql
select ename, deptno, sal
from emp
where sal > (select max(sal) from emp where deptno='20');
```



6. 1982년에 입사한 직원의 평균 급여보다 급여가 높은 사원들의 사원명, 입사일, 급여의 목록을  출력하기

```sql
select ename, hiredate, sal
from emp
where sal > (select avg(sal) from emp where hiredate like '82%');
```





### 2) 서브쿼리의 종류

##### 1) 단일행 서브쿼리

> 서브쿼리의 결과가 1행 1열인 쿼리



##### 2) 다중행 서브쿼리

> 서브쿼리의 결과가 다중 행인 경우

* 비교 연산자를 사용할 수 없다. (비교 연산자도 any나 all과 함께 사용)

* in, any, all 사용

  * in : 괄화안의 값과 일치하는 결과가 반영

    ```sql
    select ename, empno
     from emp
     where deptno in (select deptno from emp where ename like '%T%');
     
     =
     
     select ename, empno
     from emp
     where deptno in (20,30);
    ```

    ​	

  * any   (or)

     < any   :  최대값보다 작은 값을 조회

    ```
     select ename, sal
     from emp
     where sal <any(800, 1250, 1600);
    ```

    

    \> any    :  최소값보다 큰 값을 조회 

    ```
     select ename, sal
     from emp
     where sal >any(800, 1250, 1600);
    ```

    

  * all  (and)

    < all    :   최소값보다 작은 값을 조회

    ```
     select ename, sal
     from emp
     where sal <all(950, 1250, 1600);
    ```

    

    \> all   :   최대값보다 큰 값을 조회

    ```
     select ename, sal
     from emp
     where sal >all(950, 1250, 1600);
    ```



**[실습]** 

각 부서의 최소급여를 받는 사원의 부서번호, 사원이름, 급여를 조회

```sql
select ename, deptno, sal
from emp
where sal in (select min(sal) from emp group by deptno);
```



​					

`

​	

##### 3) 다중 컬럼 서브쿼리

> 두 개 이상의 컬럼을 리턴하는 서브쿼리를 의미

`where (컬럼1, 컬럼2...) in (비교할값1, 비교할값2...)`







##### 4) 상호연관 서브쿼리

> 서브쿼리를 실행할 때 메인쿼리 테이블의 컬럼을 참조해서 작업해야 하는 서브쿼리

* 서브쿼리의 비교 값이 계속 바뀌는 경우 메인의 값을 참조해서 사용

  * 각각의 행에 입력된 값에 따라 결과가 달라지는 경우

* 메인쿼리의 테이블에 alias를 추가하고 참조해서 사용한다.

* 메인쿼리의 한 row에 대해서 서브쿼리가 한번씩 실행된다.

* 서브쿼리에서는 메인쿼리의 컬럼을 사용할 수 있으나 메인쿼리에서는 서브쿼리의 컬럼을 사용할 수 없다.

  

  

  

**[실행 흐름]**

1. 메인쿼리에서 참조할 값을 가져온다.
2. 메인쿼리에서 참조할 값을 이용해서 서브쿼리를 실행한다.
3. 서브쿼리의 결과를 이용해서 메인쿼리를 실행
4. 1번부터 3번까지를 전체 레코드에 반복해서 실행한다.





**[실습]**    hr 계정

sal 가 본인 부서의 평균 급여보다 많은 사원의 사원번호, 사원명, 부서번호, sal

```sql
select empno, ename, deptno, sal
from emp outer
where sal > (select avg(sal) from emp where deptno = outer.deptno);

```





Employees 테이블에서 입사일자(hire_date)에 따라 2005년 입사한 직원들 가운데 first_name이 'Lisa'인 직원보다 빨리 입사한 직원의 사번(employee_id), 이름(first_name), 성(last_name), 입사일자(hire_date)를 조회하는 SQL 문장을 작성하시오.

단, <실행 결과>처럼 first_name과 하나의 공백(space bar), last_name 은 name 이라는 컬럼명으로 출력되도록 합니다.

```sql
select employee_id, first_name || ' ' || last_name Name , hire_date
from employees
where hire_date like '05%' and hire_date< (select hire_date from employees where first_name = 'Lisa') ;
```





Sales’부서에 속한 직원의 이름(first_name), 급여(salary), 부서이름(department_name)을 조회하시오. 단, 급여는 100번 부서의 평균보다 적게 받는 직원 정보만 출력되어야 합니다.

```sql
select e.first_name, e.salary, d.department_name
from employees e , departments d
where e.department_id = d.department_id
  and salary < (select avg(salary) from employees where Department_name = 'Sales' and department_id = '100');
```





De Haan 사원의 관리자 사원번호, 이름(last_name), 입사일 및 급여를 표시하는 SQL문을 작성하십시오.alias를 포함하여 실행결과와 동일하게 출력되어야 합니다.

```sql
select employee_id , last_name, hire_date, salary
from employees
where employee_id = (select manager_id from employees where last_name = 'De Haan');
```



부서 이름(department_name) 별 직원들의 평균연봉(salary) 을 조회하시오.단,'30번’ 부서의 직원 평균 연봉보다 평균 연봉이 이하인 부서 정보만 출력되어야 합니다. 

```sql
select d.department_name, avg(e.salary)
from employees e, departments d
where e.department_id = d.department_id
group by d.department_name
having avg(e.salary) <= (select avg(salary) from employees where department_id = 30);

```



각 부서(department_id)별로 최고 연봉(salary)를 받는 사원의 사번(employee_id), 성(last_name)과 연봉(salary)을 조회하시오. 단 조회결과는 연봉의 내림차순으로 정렬되어 나타나야 합니다.

```sql
select employee_id, last_name, department_id
from employees
where (department_id,salary) in (select department_id, max(salary) from employees group by department_id)
order by salary desc;
```

 

업무명(job_title)이 ‘Sales Representative’인 직원 중에서 연봉(salary)이 9,000 이상, 10,000 이하인 직원들의 이름(first_name), 성(last_name)과 연봉(salary)을 출력하시오

```sql
select e.first_name, e.last_name, e.salary
from employees e, jobs j
where e.job_id = j.job_id
and j.job_title = 'Sales Representative'
and e.salary between '9000' and '10000';
```

