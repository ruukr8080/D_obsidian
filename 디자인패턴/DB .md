# 학습용 계정 SCOTT 계정 사용하기.




### 1. SCOTT 계정 잠금 풀기.
  `ALTER USER SCOTT IDENTIFIED BY TIGER ACCOUNT UNLOCK;`
  
```oracle
	ALTER USER SCOTT //scott계정을 변경하겠다.
	IDENTIFIED BY tiger // 접속 비밀번호를 소문자 tiger로 지정하겠다.
	ACCOUNT UNLOCK //계정을 사용 가능 상태로 전환하겠다.
```


### 2. 결과 화면
```cmd
다음에 접속됨 :
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing optinos
 
SQL> ALTER USER SCOTT
 	2 IDENTIFIED BY tiger
 	3 ACCOUNT UNLOCK
```
##### <span style='color:var(--mk-color-red)'> 🖕 ERROR  </span>

> [!warning] ERROR
> 1. RA-01918: user 'SCOTT' does not exist
> 2. ORA-01017: invalid credential or not authorized; logon denied

1. scott 계정이 비활성화 돼있는거임.
2. soctt 파일 경로 못찾는거임 경로 설정해줘야됨


실행 :  
```db
ALTER USER SCOTT IDENTIFIED BY TIGER ACCOUNT UNLOCK;
```

이거 했는데 안되면 직접  오라클 설치 경로 가서 scott.sql 파일 찾아야됨.
```db
conn SYS/`비번` as SYSDBA
```
SYS 계정으로 로그인-> 비밀번호는 오라클을 설치했을 때 지정한 것으로 입력.
```
C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql
```
여기 있었음. 해당경로 맨앞에 @붙혀서 cmd에 입력. 
```
@C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql
```


### 3. 접속계정 생성 & 접속

실행 : 
```db
conn scott/tiger
```


😃