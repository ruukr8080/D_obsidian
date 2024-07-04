## builde.gradle dependencies 추가해야 할 사항



```
   implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
   implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect'
```

## 이번 프로젝트에서 Layout 적용을 위한 html 폴더 구조
---

## 구조

![[Pasted image 20240620205329.png]]

## 설명

## layout

> default_layout.html은 하나의 기본 판이 되어서 특정 부품들이 붙어야 할 곳들을 지정해둔다.
## fragment

 -  layout 이 지정한 위치에 붙어야 할 부품들이다.
 - 특별히 config.html 은 전체 프로젝트에서 사용될 head 태그에 해당하는 내용들을 담고 있다.
 - 그 외 나머지 fragment 들은 html 태그에 해당하는 내용들이다.

---

## layout 적용방법

- default_layout.html 상단에 타임리프 문법에 따라 레이아웃 적용을 선언함
- 해당하는 위치에 맞게 replace를 사용하여 fragment 들을 배치
- content와 content 에 담긴 js 의 경우 replace를 사용하지 않음 왜냐하면 해당 내용들은 특정 화면에 따라 달라져야 하기 때문에

```
<!DOCTYPE html>
<html lang="ko"
      xmlns:th="http://www.thymeleaf.org" // 타임리프 적용
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"> // 레이아웃 적용
    <!-- config -->
    <th:block th:replace="fragment/config :: configFragment"></th:block>
    <body>
        <!-- header -->
        <th:block th:replace="fragment/header :: headerFragment"></th:block>
        <!-- nav -->
        <th:block th:replace="fragment/nav :: navFragment"></th:block>
        <!-- rows -->
        <th:block th:replace="fragment/rows :: rowsFragment"></th:block>
        <!-- content -->
        <th:block layout:fragment="content"></th:block> // replace 미사용
        <!-- zoom -->
        <th:block th:replace="fragment/zoom :: zoomFragment"></th:block>
        <!-- footer -->
        <th:block th:replace="fragment/footer :: footerFragment"></th:block>
    </body>
    <!-- 컨텐츠 페이지의 js -->
    <th:block layout:fragment="script"></th:block> // replace 미사용
</html>
```

## replace가 적용된 fragment 예시
---
### header.html

- 최상단에 타임리프 적용을 명시
- fragment에 default-layout.html에서 사용한 headerFragment를 명시함
- 이와 마찬가지로 나머지 fragment에도 해당 내용을 기입함.
- 작업이 완료되면 해당 fragment들은 default-layout.html에 붙어서 페이지 전환에 따른 변동 없이 하나의 html처럼 브라우저에서 인식함.

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<header th:fragment="headerFragment">
    <p>
        my   -   Blog.
    </p>
</header>
```

## replace를 사용하지 않은 경우
---
### post.html

- 최상단에 타임리프 적용 선언 및 레이아웃 적용선언
- layout 형식을 default_layout으로 지정
- default_layout.html에서 content 에 해당하는 fragment 명시
- 마찬가지로 default_layout.html 하단부에 지정된 script fragment 명시

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{layout/default_layout}"
>

<th:block layout:fragment="script">
    <script type="text/javascript" th:src="@{/js/post.js}"></script>
</th:block>

<main layout:fragment="content">
    <div class="container">
        <th:block th:each="post, status: ${postList.getContent()}">
            <a th:href="'/post/'+${post.id}" >
                <div class="row">
                    <div>[[${post.title}]]</div>
                    <div>[[${post.count}]]</div>
                </div>
            </a>
        </th:block>
    </div>
</main>
</html>
```

## layout 적용 전후 비교

### layout 적용 전 index.html


```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://kit.fontawesome.com/1e99b557a8.js" crossorigin="anonymous"></script>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" />
    <link rel="stylesheet" href="/css/index.css">
    <link rel="stylesheet" href="/css/common.css">

    <title>blog</title>
</head>
<body>
<header>  <p>my   -   Blog. </p> </header>
<nav>
    <button class="index" onclick="location.href='index'"><span>Home</span></button>
    <button class="post" onclick="location.href='post'"><span>Post</span></button>
    <button class="portfolio" onclick="location.href='#'"><span>Portfolio</span></button>
            <!--<button class="kit" onclick="location.href='#'"><span>DevKit</span></button>-->
             <!-- <button class="lab" onclick="location.href='#.html'"><span>#</span></button> -->
             <!-- <button class="#" onclick="location.href='#.html'">#</button> -->
             <!-- <button class="#" onclick="location.href='#.html'">#</button> -->
             <button class="boo" onclick="location.href='#'"><span class="material-symbols-outlined">login</span></button>
</nav>    

      
          <!-- //임시 -->
          <div class="rows">
            <p> 1<br>2 <br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10
              <br>11<br>12<br>13<br>14<br>15<br>16<br><br>17<br><br>18<br>19<br>20
              <br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30 </p>
              <div class="line_container" id="line_container"></div>
          </div> 
          <!-- //임시 -->
            
            

<main>
 
      <div class="stat">
        <div class="box1"><input type="text" value=" 4 "><span> 총 방문자</span></div>
        <div class="box2"><input type="text" value=" 2 "><span> 총 포스팅</span></div>
        <div class="box3"><input type="text" value=" 0 "><span> 오늘 방문자</span></div>
        <div class="box4"><input type="text" value=" 2 "><span> 블로 운영</span></div>
      </div>
      <div class="welcome">
        <p>WELCOME</p>
      </div>
     

      <div class="slide">
        <h1>my folow list</h1>
        <p>its a slide zone for show 'folow list'</p>
      </div>


      <div class="posts">
        <h1>lately post</h1>
        <p>Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!</p>
      </div>
      
    

</main>

<div class="zoom">
    <span>tag</span>
    <span>tag</span>
    <span>tag</span>
    <span>tag</span>

                    <!-- <h1>내용 미리보기</h1>

                    <h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1><h1>내용 미리보기</h1> -->
  </div>
<div class="foot"><div class="footer">user flow / session history</div></div>
   
   

   
   
   
   
   
   
   
   
   
   <script>
        const container = document.getElementById('line_container');
        const Lines = 9;
        var l= 10;
        var n= 2;
          

        for (let i = 1; i < Lines; i++) {
            const line = document.createElement('div');
            line.style.height = `calc(86vh - ${i*10}vh`; // height 
            line.style.marginTop = `calc(10px + ${4.9*10*i}px)`; // margin-top
            line.style.marginLeft = `calc(0px + ${1}vw)`; // fallfrom left
            line.style.width = '1px';
            container.appendChild(line);
        }
        </script>

</body>
</html>
```

### layout 적용 후 index.html


```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{layout/default_layout}">
<main layout:fragment="content">
      <div class="stat">
        <div class="box1"><input type="text" value=" 4 "><span> 총 방문자</span></div>
        <div class="box2"><input type="text" value=" 2 "><span> 총 포스팅</span></div>
        <div class="box3"><input type="text" value=" 0 "><span> 오늘 방문자</span></div>
        <div class="box4"><input type="text" value=" 2 "><span> 블로 운영</span></div>
      </div>
      <div class="welcome">
        <p>WELCOME</p>
      </div>
      <div class="slide">
        <h1>my folow list</h1>
        <p>its a slide zone for show 'folow list'</p>
      </div>
      <div class="posts">
        <h1>lately post</h1>
        <p>Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!Lorem ipsum dolor sit voluptate consequatur repudiandae accusamus, cupiditate eaque adipisci nulla alias ad voluptate consequatur repudiandae cum!tate consequatur repudiandae cum!</p>
      </div>
</main>
</html>
```

## thymeleaf layout 적용의 장단점 정리


### 장점
- 공통적인 코드 작성을 하지 않아도 되기에 필요한 코드만 작성할 수 있어서 좋다.
- 관리해야 할 html 파일이 많아지는 것 같아서 부담스럽게 느껴질 수 있지만 막상 실제 코드 라인이 획기적으로 줄기 때문에 실제적 관리요소가 적다

### 단점
- layout 적용에 있어서 배우는 것이 조금 까다롭다? (하지만 이것도 조금만 공부하면 누구나 적용하기 편하긴 함)

## 결론
- 타임리프와 레이아웃 사용은 몸에 좋다