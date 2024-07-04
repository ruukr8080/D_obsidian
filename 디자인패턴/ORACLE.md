##### 테이블 구성 확인  
```sql
desc '테이블명'
```

##### SQL 기본 명령어

1. 데이터 조작 (DML : Data Manipulation Language)
	 `insert` `update` `delete` `merge`
2. 데이터 정의 (DDL : Data Definition Language)
	 `create` `alter` `drop` `rename` `truncate`
3. 데이터 검색
	`select`
4. 트랜잭션
	`commit` `rollback` `savepoint`
5. 데이터 제어(DCL : Data Control Language)
	`grant` `revoke`


# select

>[!info] SELECT 기본 형식
>```sql
>select 'distinct' '칼럼1,칼럼2,....' as '별명' ' ||연산자' ' * '
>from '테이블명'
>where 
> '조건'
>```


### ex 1 
> emp 테이블에서 모든 사워번호`empno`, 이름`ename`,급여`sal`를 조회해보자


```sql
select empno 사원번호, ename as 이름, sal as "급 여"
from emp;
```
	 `as`는 생략 가능.
	 칼럼명에 공백을 넣고싶다면 "" 쌍따옴표 이용.


결과 
![[Pasted image 20240704173149.png]]


---

### ex 2

> `emp` 테이블에서 `empno`, `ename`, `연봉`을 구해보자

```sql
select empno, ename, sal*12 as 연봉
from emp
```

결과
![[Pasted image 20240704173323.png]]

---

### ex 3 
> `job`과 `sal`을 연결하여 `업무 / 급여` 칼럼으로 출력해보자

```sql
select job || "  " ||sal as "업무 / 급여"
from emp
```
	`연결연산자 (||)`를 사용.

결과
![[Pasted image 20240704173734.png]]

# DISTINCT


>[!info] DISTINCT : 중복 제거
>```sql
> select distinct '칼럼' 
> from '테이블' 
> where  'and' 'or' 'like' 'in' 'between and' 'is null' 'is not null'
>```

### ex 4
>`emp` 테이블에서 `deptno` (`부서 번호`)를 각각 하나씩만 출력해보자.

```sql
select distinct deptno as "부서 번호"
from emp
```


![[Pasted image 20240704185528.png]]

### ex 5
> `30` 부서 또는 `10` 부서 사원들의 `이름`, `급여`, `부서 번호` 를 출력해보자

```sql
select ename 이름, sal 급여,DEPTNO "부서 번호"  
from EMP  
where DEPTNO=30 or DEPTNO=10;
```

### ex 6
> 회사의 재정이 어려운 상황이다. 명예퇴직 후보를 뽑아보자.
> 
 `급여:sal`가 3000이상 5000미만인 사원의 `이름:ename`과 `heredate`: `입사일`을 검색해보자

```sql
select ename 후보|| hiredate 
from emp
where sal >=3000 and sal <5000;
```
