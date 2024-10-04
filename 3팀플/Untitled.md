# 1. 회원 관리 프로그램

_JDBC 로 JAVA 와 h2 DB를 직접 연동해보면서 이해해보자!  
_~~_부족한 코드지만,,, 며칠간 열심히 한 프로젝트이다,,, 놀라지말자,, 나중에 디벨롭 하러 다시 오겠다..!_~~

![몽총몽총냥이](https://velog.velcdn.com/images/yeppi/post/01757624-cdb8-42bf-87f4-adfac0790f55/image.gif)

---

# 2. 프로그램 설명

원래 대부분의 애플리케이션은 Oracle 같은 DBMS를 연동하여 사용자가 입력한 데이터를 저장하고 관리한다. 이번 토이 프로젝트에서는 사용자가 입력한 회원 정보를 데이터베이스에 저장하고 관리하는 회원관리 프로그램을 개발할 것이다. 이 토이 프로젝트를 통해 클래스에 대한 설계와 String 클래스가 제공하는 API, 입출력, 그리고 기본적인 SQL을 비롯한 JDBC 프로그램을 점검할 수 있을 것이다.

비용 문제로 인해 Oracle 대신 h2 Database 를 선택하여 프로젝트를 진행했다.

## 구현 가이드

### 1) 테이블 설계

다음을 참조하여 H2 데이터베이스에 회원(`MEMBER`) 테이블을 생성

- `MEMBER` 테이블에 저장할 정보는 회원의 아이디`(MEMBER_ID`), 회원 이름(`NAME)`, 그리고 전화번호(`PHONE_NUMBER`)다.
    
- 각 컬럼에 적절한 제약 조건을 설정한다.  
      
    

### 2) Member 클래스 구현

- 사용자가 입력한 회원 정보를 저장할 수 있도록 `private` 멤버변수(필드)를 선언한다.
    
- `private` 멤버 필드에 접근하기 위한 `Getter` 메소드와 `Setter` 메소드를 선언한다.
    
- `Member` 객체의 상태를 문자열로 리턴하는 `toString()` 메소드를 재정의(Overriding)한다.  
      
    

### 3) MemberDAO 클래스 구현

- `Connection` 객체를 획득하고, 사용한 `Connection` 객체를 종료(close)하는 Utility 클래스를 작성한다.
    
- `MemberDAO` 클래스를 작성하고 회원등록, 회원 수정, 회원 삭제, 회원 상세 조회, 회원 목록 검색 기능의 메소드를 각각 구현한다.  
      
    

### 4) MemberManager 클래스 구현

- `MemberDAO` 와 사용자가 키보드를 통해 입력한 데이터를 읽어들이는 입력 스트림을 멤버변수(필드)로 선언하고, 멤버번수를 초기화하는 생성자를 작성한다.
    
- 무한루프를 돌면서 사용자가 입력한 메뉴 정보를 읽어들이는 `readMenu()` 메소드를 구현한다. `readMenu()` 메소드에서는 사용자가 입력한 숫자에 따라 다음과 같이 분기처리한다.  
      
    

### 실행 기능

- 선택한 번호마다 실행할 기능(호출할 메소드)  
    1 회원 목록 (`getMemberList()`)  
    2 회원 등록 (`insertMember()`)  
    3 회원 수정 (`updateMember()`)  
    4 회원 삭제 (`deleteMember()`)  
    0 프로그램 종료

- 사용자가 1번 메뉴를 입력했을 때, 회원목록을 출력할 `getMemberList()` 메소드를 구현한다.
- 사용자가 2번 메뉴를 입력했을 때, 회원가입을 처리할 `insertMember()` 메소드를 구현한다.
- 사용자가 3번 메뉴를 입력했을 때, 회원수정을 처리할 `updateMember()` 메소드를 구현한다.
- 사용자가 4번 메뉴를 입력했을 때, 회원삭제를 처리할 `deleteMember()` 메소드를 구현한다.

---

# 3. 구현하기

_해당 토이프로젝트 소스코드가 `push` 되어 있는 [yeppi 의 github](https://github.com/Johyejin-119/JDBC-h2-member-ToyProject.git)  
_

## 1) DB

```sql
CREATE TABLE MEMBER (
	MEMBER_ID VARCHAR(7) PRIMARY KEY,
    NAME VARCHAR(3),
	PHONE_NUMBER VARCHAR(13)
);
```

  
  

## 2) JDBC

- h2 DB를 연결
    
- JDBCUtil.java
    

```java
package toyproject;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JDBCUtil { // h2 DB 연결

	public static Connection getConnection() { 
		Connection conn = null;
		try {
			DriverManager.registerDriver(new org.h2.Driver());
			conn = DriverManager.getConnection("jdbc:h2:tcp://localhost/~/test", "sa", "");

		} catch (SQLException e) {
			e.printStackTrace();
		}
		return conn;
	}

	public static void close(PreparedStatement stmt, Connection conn) {
		try {
			stmt.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		try {
			conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	public static void close(ResultSet rs, PreparedStatement stmt, Connection conn) {
		try {
			rs.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		try {
			stmt.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		try {
			conn.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}
```

  
  

## 3) 회원 정보 클래스

- Member.java

```java
package toyproject;

import java.util.Objects;

public class Member { // 회원 정보

	private String memberId;
	private String name;
	private String phoneNumber;

	public Member(String memberId, String name, String phoneNumber) {
		this.memberId = memberId;
		this.name = name;
		this.phoneNumber = phoneNumber;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPhoneNumber() {
		return phoneNumber;
	}

	public void setPhoneNumber(String phoneNumber) {
		this.phoneNumber = phoneNumber;
	}

	
	@Override
	public int hashCode() {
		return Objects.hash(memberId, name, phoneNumber);
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Member other = (Member) obj;
		return Objects.equals(memberId, other.memberId) && Objects.equals(name, other.name)
				&& Objects.equals(phoneNumber, other.phoneNumber);
	}

	@Override
	public String toString() {
		return "---> Member [memberId=" + memberId + ", name=" + name + ", phoneNumber=" + phoneNumber + "]";
	}
	
	

}
```

  
  

## 4) 회원 관리 기능 DAO

- MemberDAO.java

```java
package toyproject;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Scanner;

public class MemberDAO {
	private Connection conn = null;
	private PreparedStatement stmt = null;
	private ResultSet rs = null;

	private final String MEMBER_LIST = "select * from member";
	private final String MEMBER_INSERT = "insert into member values(?, ?, ?)";
	private final String MEMBER_UPDATE = "update member set phone_number = ? where member_id = ?";
	private String MEMBER_DELETE = "delete member where member_id = ?";

	Scanner scan = new Scanner(System.in);

	// 회원 삭제
	public void deleteMember() {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_DELETE);

			System.out.print("삭제할 아이디를 입력하세요 (형식 M-00001): ");
			String member_id = scan.next();

			stmt.setString(1, member_id);
			stmt.executeUpdate();

			System.out.println("---> " + member_id + "회원 삭제에 성공하셨습니다.");

		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}

	// 회원 수정
	public void updateMember() {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_UPDATE);

			System.out.print("수정할 아이디를 입력하세요 (형식 M-00001): ");
			String member_id = scan.next();

			System.out.print("수정할 전화번호를 입력하세요 : ");
			String phone_number = scan.next();

			stmt.setString(1, phone_number);
			stmt.setString(2, member_id);
			stmt.executeUpdate();

			System.out.println("---> 회원 수정에 성공하셨습니다.");

		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}

	// 회원 삽입
	public void insertMember() {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_INSERT);

			System.out.print("아이디를 입력하세요 (형식 M-00001): ");
			String member_id = scan.next();

			System.out.print("이름을 입력하세요 : ");
			String name = scan.next();

			System.out.print("전화번호를 입력하세요 : ");
			String phone_number = scan.next();

			stmt.setString(1, member_id);
			stmt.setString(2, name);
			stmt.setString(3, phone_number);
			stmt.executeUpdate();

			System.out.println("---> 회원가입에 성공하셨습니다.");
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}

	// 회원 목록
	public void getMemberList() {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_LIST);
			rs = stmt.executeQuery();

			if (rs.next()) {
				System.out.println("현재 등록된 회원 목록입니다.");
				do {
					String member_id = rs.getString("MEMBER_ID");
					String name = rs.getString("NAME");
					String phone_number = rs.getString("PHONE_NUMBER");

					Member member = new Member(member_id, name, phone_number);
					System.out.println(member.toString());
				} while (rs.next());
			} else {
				System.out.println("등록된 회원이 없습니다.");
			}

		} catch (SQLException e) {
			e.printStackTrace();

		} finally {
			JDBCUtil.close(rs, stmt, conn);
		}
	}

	// 회원 상세 목록
	public void getMemberListDetail() {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_LIST);
			rs = stmt.executeQuery();

			String member_id_input = scan.next();

			while (rs.next()) {
				String member_id_origin = rs.getString("MEMBER_ID");

				// 입력한 member_id_input과 기존 db에 저장된 member_id_origin이 동일할 때
				if (member_id_origin.equals(member_id_input)) {
					System.out.println(member_id_input + "가 이미 존재합니다.");
				} else {
					insertMember();
				}
			}

		} catch (SQLException e) {
			e.printStackTrace();

		} finally {
			JDBCUtil.close(rs, stmt, conn);
		}
	}
}
```

  
  

## 5) 회원 프로그램 관리 컨트롤

- MemberManager.java

```java
package toyproject;

import java.util.Scanner;

public class MemberManager {
	MemberDAO dao = new MemberDAO();
	Scanner sc = new Scanner(System.in);
	
	public void readMenu() {
		while (true) {
			System.out.println("목록을 원하시면 1번을 입력하세요.");
			System.out.println("등록을 원하시면 2번을 입력하세요.");
			System.out.println("수정을 원하시면 3번을 입력하세요.");
			System.out.println("삭제를 원하시면 4번을 입력하세요..");
			System.out.println("종료를 원하시면 5번을 입력하세요..");

			int number = sc.nextInt();
			
			if (number == 1) {
				dao.getMemberList();
			}
			else if (number == 2) {
				dao.insertMember();
			}
			else if (number == 3) {
				dao.updateMember();
			}
			else if (number == 4) {
				dao.deleteMember();
			}
			else {
				sc.close();
				break;
			}
		}
	}
}
```

  
  

## 6) Client

- 프로그램 실행하는 MainClass.java

```java
package toyproject;

import java.io.IOException;

public class MainClass {

	public static void main(String[] args) throws IOException{
 
		System.out.println("############################");
		System.out.println("### 회원 관리 프로그램 START ###");
		System.out.println("############################");

		MemberManager manager = new MemberManager();
		manager.readMenu();
	
		System.out.println("############################");
		System.out.println("### GOOD-BYE 프로그램 종료 ###");
		System.out.println("############################");
	
	}

}
```

  

---

# 🧐회고🧐

`MemberDAO` 클래스와 `MemberManager` 클래스의 각 기능들을 잘 분리해서 개발해야 되겠다고 느꼈다.  
이런 프로그램을 효율적으로 관리하기 위해서는 유지보수가 잘 되어야한다고 느꼈고, 메서드마다 기능을 독립적으로 적절히 분배해야한다. . . ~~(이게 바로 객체 지향 개발의 핵심이라고 느꼈다)~~

본인은 `MemberManager` 클래스에서 TV 리모컨처럼 조절하는 기능만 간략하게 넣었고,  
`MemberDAO` 클래스의 각 메서드에 해당하는 기능을 모조리 넣었다.

`System.out` 으로 `Clinet` 에게 보여지는 출력문은 `MemberManager` 클래스로 빼는 게 훨씬 객체간 결합도를 낮추는 좋은 코딩 방법이 있었다. 그리고 `MemberDAO` 클래스의 각 메서드(`insert`, `delete` 등) 들은 `Member` 객체를 매개변수로 전달받아서 `Member` 클래스를 잘 활용했어야 했는데. . . ~~그렇지 못했다...~~

코드에 정답은 없다지만, 기능이 돌아가는 것에만 초점을 두면서 개발했던 것 같다.  
더 효율적인 코드, 가독성이 좋고, 객체지향의 요소들을 더 적절히 반영하기위해 앞으로 많이 배워야할 것 같다!

어떻게 보면 간단한 토이 프로젝트였지만, 많은 프로그램 개발의 기초라고 할 수 있는 데이터베이스 연동을 많이 이해할 수 있어서 좋은 계기가 되었다. 앞으로 화이티이이ㅇ화이팅. . .!!!  
  

---

## 효율적인 코드

- MemberManager.java

- 유효성 체크를 추가
    - 아이디 검증
    - 이름 검증
    - 전화 번호 null 체크
    - 회원 수정 및 삭제 시 회원 존재 여부 확인

```java
public class MemberManager {
	private MemberDAO memberDAO;
	private Scanner keyboard;
	
	public MemberManager() {
		memberDAO = new MemberDAO();	
		keyboard = new Scanner(System.in);
	}
	
	public void startProgram() throws IOException {	
		
		while (true) {			
			printMenu();
			String buttonNumber = keyboard.nextLine();
			if(buttonNumber.equals("0")) {
				break;
			} else {
				if(buttonNumber.equals("1")) {
					getMemberList();
				} else if(buttonNumber.equals("2")) {
					insertMember();
				} else if(buttonNumber.equals("3")) {
					updateMember();
				} else if(buttonNumber.equals("4")) {
					deleteMember();
				} else {
					System.out.println(buttonNumber + "는 유효하지 않은 번호입니다.");
				}
			}
			
		}

		keyboard.close();
	}

	private void insertMember() {
		System.out.print("아이디를 입력하세요 (형식 M-00001): " );		
		String memberId = keyboard.nextLine();
		if(memberDAO.getMember(memberId)) {
			System.out.println(memberId + "가 이미 존재합니다.");
			return;
		}
		if(idVarify(memberId)) {
			return;
		}
		
		System.out.print("이름을 입력하세요 : " );	
		String name = keyboard.nextLine();
		if(nameVarify(name)) {
			return;
		}
		
		System.out.print("전화번호를 입력하세요 : " );
		String phoneNumber = keyboard.nextLine();
		if(phoneNumberVarify(phoneNumber)) {
			return;
		}

		memberDAO.insertMember(memberId, name, phoneNumber);
		System.out.println("회원가입에 성공하셨습니다.");
		memberDAO.getMemberList();
	}

	private void updateMember() {
		System.out.print("수정할 아이디를 입력하세요 (형식 M-00001): " );		
		String memberId = keyboard.nextLine();
		if(idVarify(memberId)) {
			return;
		}
		if(!memberDAO.getMember(memberId)) {
			System.out.println("수정할" + memberId + "회원 정보가 존재하지 않습니다.");
			return;
		}
		System.out.print("수정할 전화번호를 입력하세요 : " );
		String phoneNumber = keyboard.nextLine();
		if(phoneNumberVarify(phoneNumber)) {
			return;
		}

		memberDAO.updateMember(phoneNumber, memberId);
		System.out.println("회원수정에 성공하셨습니다.");		
	}

	private void deleteMember() {
		System.out.print("삭제할 아이디를 입력하세요 : " );
		String memberId = keyboard.nextLine();
		if(idVarify(memberId)) {
			return;
		}
		if(!memberDAO.getMember(memberId)) {
			System.out.println("삭제할" + memberId + "회원 정보가 존재하지 않습니다.");
			return;
		}
		memberDAO.deleteMember(memberId);
		System.out.println(memberId + "회원 삭제에 성공하셨습니다.");
		
	}
	
	private void getMemberList() {
		List<Member> memberList = memberDAO.getMemberList();
		if(memberList.size() == 0) {
			System.out.println("등록된 회원이 없습니다.");
		} else {
			System.out.println("현재 등록된 회원 목록입니다.");					
			for (Member member : memberList) {
				System.out.println("---> " + member.toString());
			}
		}
	}

	private void printMenu() {
		System.out.println("목록을 원하시면 1번을 입력하세요.");
		System.out.println("등록을 원하시면 2번을 입력하세요.");
		System.out.println("수정을 원하시면 3번을 입력하세요.");
		System.out.println("삭제를 원하시면 4번을 입력하세요.");
		System.out.println("종료를 원하시면 0번을 입력하세요.");
	}
	
	private boolean idVarify(String memberId) {
		if(memberId == null || memberId.length() == 0) {
			System.out.println("아이디는 필수 입력항목입니다.");
			return true;
		}
		if(!memberId.startsWith("M-") || !(memberId.length() == 7)) {
			System.out.println("아이디는 'M-'로 시작해야 하며, M-를 포함하여 7개의 문자로 구성해야 합니다.");
			return true;
		}		
		return false;
	}
	
	private boolean nameVarify(String name) {
		if(name == null || name.length() == 0) {
			System.out.println("이름은 필수 입력항목입니다.");
			return true;
		}
		return false;
	}
	
	private boolean phoneNumberVarify(String phoneNumber) {
		if(phoneNumber == null || phoneNumber.length() == 0) {
			System.out.println("전화번호는 필수 입력항목입니다.");
			return true;
		}
		if(phoneNumber.charAt(3) != '-'  || !(phoneNumber.length() == 13)) {
			System.out.println("전화번호는 두 개의 '-'를 포함하여 총 13개의 문자로 구성해야 합니다.");
			return true;
		}	
		return false;
	}

}
```

  

- MemberDAO.java
    - DB 에 정보를 출력, 삽입, 수정, 삭제하는 순수한 기능들로만 채우기
    - 출력문이나 유효성 체크는 위 MemberManager.java 로 분리시키기

```java
public class MemberDAO {
	private Connection conn;
	private PreparedStatement stmt;
	private ResultSet rs;
	
	private static String MEMBER_INSERT = "insert into member(member_id, name, phone_number) values (?, ?, ?)";
	private static String MEMBER_UPDATE = "update member set phone_number = ? where member_id = ?";
	private static String MEMBER_DELETE = "delete member where member_id = ?";
	private static String MEMBER_GET    = "select * from member where member_id = ?";
	private static String MEMBER_LIST   = "select * from member order by member_id desc";
	
	public void insertMember(String memberId, String name, String phoneNumber) {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_INSERT);
			stmt.setString(1, memberId);
			stmt.setString(2, name);
			stmt.setString(3, phoneNumber);
			stmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}

	public void updateMember(String phoneNumber, String memberId) {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_UPDATE);
			stmt.setString(1, phoneNumber);
			stmt.setString(2, memberId);
			stmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}

	public void deleteMember(String memberId) {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_DELETE);
			stmt.setString(1, memberId);
			stmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(stmt, conn);
		}
	}
	
	public boolean getMember(String memberId) {
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_GET);
			stmt.setString(1, memberId);
			rs = stmt.executeQuery();
			if(rs.next()) {
			    return true;			
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(rs, stmt, conn);
		}
		return false;
	}
	
	public List<Member> getMemberList() {
		List<Member> boardList = new ArrayList<Member>();
		try {
			conn = JDBCUtil.getConnection();
			stmt = conn.prepareStatement(MEMBER_LIST);
			rs = stmt.executeQuery();
			while(rs.next()) {
				Member member = new Member();
			    member.setMemberId(rs.getString("MEMBER_ID"));
			    member.setName(rs.getString("NAME"));
				member.setPhoneNumber(rs.getString("PHONE_NUMBER"));
				boardList.add(member);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JDBCUtil.close(rs, stmt, conn);
		}
		return boardList;
	}
}
```

  

## 참고문서

안드로이드는 전자양의 꿈을 꾸는가의 [JDBC 문제: 자바에서 데이터 입력,수정,삭제,조회](https://mainichibenkyo.tistory.com/53)  
jaechun's story의 [ResultSet이 null일 때를 피하는 방법](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hjc426&logNo=130023680300)

[![profile](https://media.vlpt.us/images/yeppi/profile/681653b9-42a4-4082-b945-7a2ab3ad64bc/%EB%B8%94%EB%A1%9C%EA%B7%B8%ED%94%84%EB%A1%9C%ED%95%84_%EB%B9%85%EC%8A%A4%EB%B9%84AR%EC%82%AC%EC%A7%84.png)](https://velog.io/@yeppi/posts)

[Yeppi's 개발 일기](https://velog.io/@yeppi/posts)