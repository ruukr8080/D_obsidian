# 테이블 생성

```sql
CREATE TABLE db명.테이블명 (
  컬럼명1 INT PRIMARY KEY AUTO_INCREMENT, -- 기본키 숫자 자동 증가 설정
  컬럼명2 CHAR(15) NOT NULL,
  컬럼명3 INT,

  PRIMARY KEY(컬럼명1),
  FOREIGN KEY(컬럼명2) REFERENCES 테이블명(컬럼명) -- 자기자신 외래키 참조
  FOREIGN KEY(컬럼명3) REFERENCES 다른테이블명(컬럼명a) -- 다른테이블 외래키 참조
);
```

```sql
테이블 이름을 띄어쓰기 하고싶다면 `` 를 넣어라.
create table `TEST TBL`(id INT);
```

## 테이블 제약 조건

- NOT NULL : NULL 값 저장 안됨.
- UNIQUE : 서로 다른 값을 가져야만함.
- PRIMARY KEY : `not null`과 `unique` 특징을 다 적용.
- FOREIGN KEY :  하나의 테이블을 다른 테이블에 의존시킴.
- DEFAULT : 기본값을 설정.
- AUTO_INCREMENT : 값을 1부터 시작. 레코드가 추가될 때 마다 1씩 자동 증가

```sql
create table userTbl
(	
    userID char(8) primary key,
    name varchar(10) not null unique, -- 만일 기본기지정을 안했으면 not null 유니크가 기본키가 된다.
    birthYear int not null,
    addr char(2) not null,
    mobile char(3),
    mdate date
);
```

```sql
create table buyTbl
(	
    num int auto_increment not null primary key, -- 자동으로 숫자순서대로 증가
    userID char(8) not null,
    price int not null default 80, -- 값이 입력되지 않을 때 기본값
    
    constraint 외래키이름 foreign key (userID) references userTbl(userID) on update cascade 
    -- 외래키 지정. constraint쓰면 외래키이름 지정, 안쓰면 필드값 자체가 이름이 됨.
    -- 만일 부모키가 변경할 경우 같이 외래키값도 변경
);
```

### 결과 

![[Pasted image 20240704152022.png]]


# 테이블 조회

```sql
show tables

show table STATUS -- 더 자세히 
```



# 연산자

>[!info] info
>` = ` : 같다.
>` != `, ` ^= ` , ` <> ` : 같지 않다.
>` >= ` , ` <= ` ,` < `  ,` > ` : 크거나 같음, 작거나 같음, 작음, 큼  
>
>`and` , `or` , `between and` , `in` , `like` , `is not null` 


# 정렬
### order by : 정렬
 - asc : 오름차순 
 - desc : 내림차순
	 - _컬럼명, 컬럼 번호으로 가능_




