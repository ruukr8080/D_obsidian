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

# SELECT 

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
#연산자
> 회사의 재정이 어려운 상황이다. 명예퇴직 후보를 뽑아보자.
 `급여:sal`가 3000이상 5000미만인 사원의 `이름:ename`과 `heredate`: `입사일`을 검색해보자

```sql
select ename 후보|| hiredate 
from emp
where sal >=3000 and sal <5000;
```


### ex 7
>'smith' 사원의 모든 칼럼을 표시하셈

```sql
select *  
from EMP  
where lower(ENAME)='smith';
```

### ex 8
> 회사에서 06월에 입사한 인원과`deptno` 신원을 파악하고자한다.
> 6월에 입사한 사원들의 부서번호`deptno`,이름`ename`,입사일`hiredate`를 출력하라

```sql
select deptno,ename,hiredate  
from EMP  
where  
    HIREDATE like'____"06"%';
```
결과:
![[Pasted image 20240705110215.png]]

>[!tip] Tip
>- `'a%'` : a로 시작하는
>- `'%d'` : d로 끝나는
>- `'%c%'` : c가 포함된
>- `'_a%'` : 앞에서 두번째 글자가 a로 시작하는
>- `'%d_'` : 뒤에서 두번쨰 글자가 d 로 끝나는




---

### ex 9
> 나는 대표다. 저번에 복도에서 인사 안하던 놈이 있었는데, 사원증을 얼핏 봤는데 매니저`job`였고 이름`ename`이 J로 시작했던것만 기억난다. 어디 부서의 누구인지 알아내자.

```sql
select deptno,ename,job  
from EMP  
where  
    job = 'MANAGER'  
    and lower(ename) like 'j%'
```

![[Pasted image 20240705114056.png]]

---

### ex 10
> 커미션`comm`을 받는 사원의 이름,급여,커미션 출력

```sql
select ename,sal,comm  
from EMP  
where comm is not null
```



### ex 11
#연산자 

> 직속상관번호 `mgr`가 `7698`이거나 `7839` 인 사원들의 이름,직무,직속상관번호를  출력

 or연산자
```sql
select ename,job,mgr  
from EMP  
where  
    mgr = 7698 or mgr = 7839;
```
in연산자
```sql
select ename,job,mgr  
from EMP  
where  
    mgr in (7698 ,7839);
```


### ex 12
#연산자
>(between,and 연산자)를 활용하여, 
>
>급여가 2000 이상 3000이하인 사원의 이름,급여 출력.
	 *between 연산자는 초과,미만에서는 사용 못함.

```sql
select ename,sal  
from EMP  
where sal between 2000 and 3000;
```

### ex 13
> 업무`job`가 manager 이거나 salesman이면서
> 급여는 2000~3000인 사원의 이름,급여,업무를 출력

```sql
select ename,sal,job  
from EMP  
where lower(job) like '%sale%' or lower(job) like 'manager'  
    and sal between 2000 and 3000;
```

### ex 14
> 사원이름,부서id,입사일을 부서별로 내림차순 정렬하라.

```sql
select ename,deptno,HIREDATE  
from emp  
order by DEPTNO desc;
```


```sql
select ename,deptno,HIREDATE  
from emp  
order by 2 desc  
;
```
- _select로 지정한 요소중 2번째_


### ex 15
>  사원명,부서id,입사일을 부서별로 내림차순 정렬하라,
>   같은 부서끼리는 입사 순으로 정렬하라.


```sql
select ename,DEPTNO,HIREDATE  
from emp  
order by DEPTNO desc , HIREDATE asc  
;
```

```sql
select ename,deptno,HIREDATE  
from emp  
order by 2 desc , 3 asc;
;
```


# FUNCTION



#### 단일행 함수
- 숫자  
  - `mod` : 나머지   
  - `round` :  반올림      
  - `trunc` : 내림   
  - `ceil` : 올림   

😃mod 기본형 
```sql
select mod( [좌항],[우항] )
from dual -- 임시 테이블
```
😃round 기본형
```sql
select round( [함수가 적용될 숫자],[자리수] )
from dual -- 임시 테이블
```
😃trunc 기본형
```sql
select round( [함수가 적용될 숫자],[자리수] )
from dual -- 임시 테이블
```
😃ceil 기본형
```sql
select round( [함수가 적용될 숫자],[자리수] )
from dual -- 임시 테이블
```

_자리수(n)가 양수이면 (n+1)자리에 적용되고, 음수이면 자리수(n) 그대로 적용된다._

---



-  문자  
   - `lower` : 소문자
   - `upper` : 대문자
   - `substr` : 
   - `ltrim` : 좌공백제거  
   - `rtrim` : 우공백제거
   - `trim` :
   - `concat` : 
   - `length` :  문자열의 길이
   - `char` : 고정 문자 길이 ( 최대 2000byte)
   - `varchar2` : 가변 문자 길이 ( 최대 4000byte)
   - `instr` : 지정 문자열의 인덱스 번호를 출력한다.


>[!NOTE] instr기본형
> instr(표현식,찾는 문자, 탐색 시작할 위치)음수이면 뒤에서 부터 탐색한다.
>```sql
>select instr('HelloWorld!', 'W')
>from dual;
>// 결과 : 6
>```
>```sql
>select instr('HelloWorld!', 'll')
>from dual;
>// 결과 : 3
>```
>```sql
>select instr('HelloWorld!', 'o', 1)
>from dual;
>// 결과 : 5
>```
>```sql
>select instr('HelloWorld!', 'o', -1')
>from dual;
>// 결과 : 7
>```

>[!NOTE] substr 기본형
>sybstr('표현식',시작지점,개수)
>```sql
>select substr('Nice to meet you!' , 9, 4) 
>from dual;
>// 결과 : meet
>```
>```sql
>select substr('Nice to meet you!' , 9) 
>from dual;
>// 결과 : meet you!
>```



- 날짜  
  - `sysdate` : 시스템 기반 날짜 
  - `add_month` : 지금부터 몇 달 뒤  
  - `month_between` : 월과 월 사이에 흐른 시간




> [!NOTE] 변환 함수
> 1. 암시적(`implict`) 변환 : <span style='color:var(--mk-color-pink)'>자동</span>
> VARCHAR2 또는 CHAR ------> NUMBER
> VARCHAR2 또는 CHAR ------> DATE
> NUMBER ------> VARCHAR2
> DATE ------> VARCHAR2
> 
> 2. 명시적`(explict)` 변환 : <span style='color:var(--mk-color-red)'>강제</span>
> TO_NUMBER     TO_DATTE
> NUMBER     CHARACTER     DATE
> TO_CHAR     TO_CHAR

## 날짜 형식

	YYYY : 네자리 연도(숫자)
	YEAR : 연도(문자)
	MM : 두자리 값의 month
	MONTH : 달 이름 (november)
	MON : 달 세자리 약어 (Nov)
	DY : 요일 세자리 약어 (Sun)
	DAY : 요일 전체 (Sunday)
	DD : 숫자로 나타낸 날짜 (01,...31)
	HH / HH24 : (24시간제)로 나타낸 시간 
	MI
	SS

## 숫자 형식

	9 : 숫자를 표시
	0 : 0을 강제로 표시
	$ : 부동 $기호 표시
	L : 부동 지역통화기호 표시
	. : 소수점 출력
	, : 천단위 구분자 출력


## 그룹`집합` 함수


	avg , sum , max , min , count 


 

## 기타 함수

	nvl , dcode , case


### ex 16 
> 'JONES' 사원의 이름,id,부서번호를 소문자로 바꿔서 검색하라

```sql
select ename,empno,DEPTNO  
from EMP  
where lower(ename) = 'jones';
```


### ex 17 
>임시 가상테이블인 `dual` 테이블에 8에서 3을 나눈 나머지 값을 구하라

```sql
select mod(8,3)  
from dual;
```

### ex 18 
> 33.5934에서 소수점 2번째 자리수를 반올림 한 값
> 17.34 올림 한 값
> 2155.07에서 100의 자릿수를 내림 한 값 을 차례로 출력해라

```sql
select round(33.594 , 1),  
       ceil(17.34),  
       trunc(2155.07 , -3)  
from dual;
```

### ex 19
> 'Hello '문자열과 'World'문자열을 결합해라

```sql
select concat('Hello ','World')
from dual;
```

### ex 20
> `TEXT`테이블을 생성하고, 
> `str1`에 고정길이 문자(20) `hello`,`안녕` 을 한 항목씩 입력하고,
> `str2`에 가변길이 문자(20) `world`,`하셈` 를 한 항목씩 입력한 뒤, 
> `commit`해라

```sql
create table TEXT(  
    str1 char(20),  
    str2 varchar2(20)  
);  
insert into TEXT(str1, str2) values ('Hello', 'World!');  
insert into TEXT(str1, str2) values ('안녕', '하셈!');  
  
commit;
```




### ex 21 (ex 20번 연장)
>text  테이블에서 str1 , str2 각각의 문자열의 크기(byte)를 구해보자
```sql
select length(str1),length(str2)
from text 
```
![[Pasted image 20240705164847.png]]

### ex 22
> text 테이블에서 str1의 값이 '안녕'인 항목의 길이와
> str2의 값이 '하셈!'인 항목의 길이를 각각 구해보자
```sql
select length(str1),length(str2)  
from text  
where STR1 = '안녕' and STR2 = '하셈!'
```
![[Pasted image 20240705165631.png]]



