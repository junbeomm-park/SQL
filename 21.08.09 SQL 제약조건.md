# 제약조건



### 제약조건

> 데이터가 테이블에 저장될 때 데이터의 무결성을 위하여 규칙을 정의 할 수 있다. 제약조건은 테이블을 생성하며 정의하거나 생성 후에 alter table문을 이용하여 추가할 수 있다.





### 제약조건의 종류

* NOT NULL
* UNIQUE
* PRIMARY KEY
* FOREIN KEY
* CHECK



### 제약조건 추가

> 테이블 정의할 때 칼럼을 정의하며 추가하거나 테이블의 마지막에서 제약조건을 추가할 수 있다.



####  테이블이 모두 정의된 후 추가되는 경우

```sql
alter table 테이블명
add constraint 제약조건이름 추가할제약조건
```





























### FOREIGN KEY

``` 
alter table 테이블명
add constraint 제약조건명 foreign key(foreign key 제약조건을 적용할 컬럼)
references 테이블명(기본키-foreign key에서 참조할 기본키)
```

