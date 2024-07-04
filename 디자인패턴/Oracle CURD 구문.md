# 테이블 생성

`USER` 테이블을 만들어보자

```sql
CREATE TABLE TBL_USER(
user_idx number primary key,
id varchar2(20) not null,
name varchar2(20) not null,
password varchar2(20) not null,
email varchar2(50),
address varchar2(200),
);

테이블이 생성되었습니다.
```

user_idx는 PK(primary key)로 사용할 거니까 시퀀스를 생성해주자.

```sql
CREATE SEQUENCE USER_SEQ;

시퀀스가 생성되었습니다.
```

생성한 테이블 확인 해보자.

```sql
DESC TBL_USER
```

# INSERT문

생성한 테이블에 데이터를 집어넣어보자.

```sql
INSERT INTO TBL_USER(
user_idx,
id,
name,
password,
email,
address,
)
VALUES(
USER_SEQ,NEXTVAL,
'ruukr' --'id칼럼'
'han' --name칼럼'
'0000' --'password칼럼'
'ruukr@email.com' --'email칼럼'
'파주시 평양' --'address칼럼'
)
```

# SELECT문 - 조회

전체 조회해보자.

```sql
select *
from TBL_USER;
```

 - ### 조회 기본 포맷

	SELECT `[컬럼 명]` `가져올 칼럼명`
	FROM `[테이블 명]` `어디 테이블에서?`
	WHERE `[조건 명]` `~~이 포함된,,,`
	ORDER BY `[컬럼]` `오름차순 or 내림차순`

> `원하는 데이터만 잘 뽑으려면 WHERE절의 조건을 잘 활용해야한다.`

---


#### EX 1
 `name`과 `password` 뽑기
`TBL_USER` 에서
 포함될 조건 `ID`칼럼에서 `'ruukr'` 이 포함된.
 `USER_DIX`의 `ASC` 순서로.
```sql
SELECT name, password 
FROM TBL_USER
WHERE ID = 'ruukr'
ORDER BY USER_DIX ASC;
```


---
#### EX 2
	 *+ LIKE 절* 
```sql
SELECT *
FROM TBL_USER
WHERE NAME
LIKE '%han%';
```

위 LIKE절 `'%han%'` 은 `name`에서 han이라는 문자열이 포함돼있는 데이터를 가져와라임.

%의 위치에 따라 
han이 문자열의 앞에 위치하느냐`'han%'`
han이 문자열의 뒤에 위치하느냐. `'%han'`
조건을 걸 수 있음.



# UPDATE문 - 수정

데이터를 수정해보자.
	> update,set은 select,from 처럼 세트로 씀



```sql
--기본형식
UPDATE '테이블명'
SET
 칼럼1='값',
 칼럼2='값',
WHERE '조건'
```
- `UPDATE [테이블] SET [열] = '변경할값' WHERE [조건]`

update문에 where절 조건을 안주면 테이블의 모든 행의 데이터가 다 바뀜.
대참사가 일어날 수 있으니 UPDATE문을 사용할 땐
WHERE 조건을 꼭 걸어주자.

---


### EX 3

	 `ID`가 `ruukr` 이고 `name`이 `han`인 유저가 `파주시 평양`에서 살다가 `서울시 판교`
	로 이사했다.
	update문으로 수정하자.

```sql
UPDATE TBL_USER  --update(수정)할거임 TBL_USER를.
set address = '서울시 판교' -- address를 '서울시 판교'로. 저장할거임
WHERE
 USER_IDX = (
 SELECT USER_IDX
 FROM USER TBL_USER
 WHERE
  NAME = 'han' AND
  ID = 'ruukr'
 )
```

> whwere절 ( ) 안에 SELECT문을 서브쿼리라고 부르는데,
> 조건절에서 SELECT문을 넣어서 값을 가져오는 거임.



**COMMIT하고  확인해보자!**
```sql
COMMIT;
```
```sql
SELECT * FROM TBL_USER;
```


---



	 트랜잭션 내에 DML 명령문들은 실행되어 반영이 되더라도 커밋 전까지는 전부 임시적인 상태임. 

>[!tip] Tip
>트랜잭션을 제어하는 명령어로써, 아래 3가지가 있음.
>
 COMMIT : 모든 변경사항 영구적용 후 현재 트랜잭션 종료.
> ```sql
> commit;
> ```
> ROLLBACK :  마지막 COMMIT 시점으로 회귀. 
> ```sql
> rollback;
> ```
> SAVEPOINT : ROLLBACK 할 포인트(세이브포인트)를 지정.
> 
>





---





# DELETE문 - 삭제

기본형
```sql
DELETE FROM '테이블명' WHERE 조건
```

위에 만들었던 데이터를 과감히 삭제해보자

```sql
DELETE
FROM TBL_USER
WHERE 
 NAME = 'han';
 
COMMIT;
```


**확인해보자!**
```sql
SELECT * FROM TBL_USER;
```





