<h3 id="1-스프링-부트-프로젝트의-구조-이해하기">1. 스프링 부트 프로젝트의 구조 이해하기</h3>
<h4 id="스프링-부트-프로젝트의-구조에-대한-간단-설명">스프링 부트 프로젝트의 구조에 대한 간단 설명</h4>
<ul>
<li>컨트롤러(Controller): entry point. 클라이언트의 요청에 따라 어떠한 메소드를 실행할지 결정하는 클래스이다. 클라이언트에게 View(또는 서버에서 이미 처리된 데이터를 표현하는 View)를 응답으로 보내준다. <code>@Controller</code></li>
<li>폼(Form): 페이지에서 컨트롤러로 데이터를 넘기기 전 데이터를 입력받고 입력받은 데이터를 저장하고 있는 객체이다. form 태그 내부의 input 태그와 같은 태그로 컨트롤러로 넘길 데이터를 입력받을 수 있다.</li>
<li>DTO(Data Transfer Object): 데이터 전송 객체, 프로세스 간 데이터를 전달하는 객체로, 비즈니스 로직은 가지지 않고 전달하고 싶은 데이터만이 담겨 있다. getter&amp;setter만을 가진다. <code>@Request</code>, <code>@Response</code></li>
<li>엔티티(Entity): 실제 DB 테이블과 매핑이 되는 클래스로, 데이터를 전달하는 클래스로 사용하면 안 된다. 이를 기준으로 테이블이 형성된다. 비즈니스 로직을 포함할 수 있고 setter()을 포함할 수 있다. <code>@Entity</code></li>
<li>리포지토리(Repository): DB와 상호작용 하기 위한 매개 모듈이다. DB 테이블에 접근하는 메소드를 사용하기 위한 인터페이스이다. 외부I/O 처리를 담당한다. <code>@Repository</code></li>
<li>서비스(Service): 컨트롤러에서 리포지토리를 직접 호출하지 않기 위해 중간에 위치한다. 주로 자바 로직을 처리한다. <code>@Service</code></li>
</ul>
<h4 id="srcmainjava-디렉토리">src/main/java 디렉토리</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/e65dd6fb-ac6d-49ce-9846-cabf03f6fff6/image.png" /></p>
<ul>
<li>com.mysite.sbb 패키지<ul>
<li>이 패키지는 sbb의 자바 파일을 저장하는 공간이다.</li>
<li>HelloController.java와 같은 스프링 부트의 컨트롤러, 폼과 DTO, 데이터베이스 처리를 위한 엔티티, 서비스 등의 자바 파일이 이곳에 위치한다.</li>
</ul>
</li>
<li>SbbApplication.java 파일<ul>
<li>모든 프로그램에는 프로그램의 시작을 담당하는 파일이 있다. 스프링 부트로 만든 프로그램에도 시작을 담당하는 파일이 있는데 그 파일이 바로 '프로젝트명+Application.java' 파일이다.</li>
<li>스프링 부트 프로젝트를 생성할 때 프로젝트명으로 'sbb'라는 이름을 입력하면 다음과 같이 SbbApplication.java 파일이 자동으로 생성된다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/dd5f5915-b826-4d7a-9c06-74957d78f023/image.png" /></li>
<li>SbbApplication 클래스에는 반드시 @SpringBootApplication 어노테이션이 적용되어 있어야 한다. 이 어노테이션을 통해서 스프링 부트 애플리케이션을 시작할 수 있기 때문이다.</li>
</ul>
</li>
</ul>
<h4 id="srcmainresources-디렉토리">src/main/resources 디렉토리</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/2a4db0b8-6726-4042-b643-bb70eef7eca4/image.png" /></p>
<ul>
<li>templates 디렉토리<ul>
<li>템플릿 파일을 저장한다. 템플릿 파일은 자바 코드를 삽입 가능한 HTML 형식의 파일로, 스프링 부트에서 생성한 자바 객체를 HTML 형태로 출력 가능하다.</li>
<li>Templates에는 SBB 게시판 서비스에 필요한 '질문 목록', '질문 상세' 등의 웹페이지를 구성하는 HTML 파일을 저장한다. 이는 MVC 모델에서 View에 해당한다.</li>
</ul>
</li>
<li>static 디렉토리<ul>
<li>sbb 프로젝트의 스타일 시트(css 파일), 자바 스크립트(js 파일), 그리고 이미지 파일(jpg 파일, png 파일 등) 등을 저장한다.</li>
</ul>
</li>
<li>application.properties 파일<ul>
<li>application.properties 파일은 sbb 프로젝트의 환경을 설정한다.</li>
<li>sbb 프로젝트의 환경변수, 데이터베이스 등의 설정을 이 파일에 저장한다. </li>
</ul>
</li>
</ul>
<h4 id="srctestjava-디렉토리">src/test/java 디렉토리</h4>
<ul>
<li>sbb 프로젝트에서 작성한 파일을 테스트하는 코드를 저장하는 공간이다. </li>
<li>JUnit과 스프링 부트의 테스트 도구를 사용하여 서버를 실행하지 않은 상태에서 src/main/java 디렉토리에 작성한 코드를 테스트할 수 있다.</li>
</ul>
<h4 id="buildgradle-파일">build.gradle 파일</h4>
<ul>
<li>build.gradle은 gradle이 사용하는 환경 파일</li>
<li>gradle은 groovy를 기반으로 한 빌드 도구로, Ant나 Maven과 같은 이전 세대의 단점을 보완하고 장점을 취합하여 만들어졌다.</li>
<li>build.gradle 파일에는 프로젝트에 필요한 플러그인과 라이브러리를 설치하기 위한 내용을 작성한다.(외부 의존성 주입)</li>
</ul>
<h3 id="간단한-웹-프로그램-만들기">간단한 웹 프로그램 만들기</h3>
<p>브라우저에 URL &quot;<a href="http://localhost:8080/sbb&quot;%EB%A5%BC">http://localhost:8080/sbb&quot;를</a> 입력하면 다음과 같은 화면이 등장한다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/d45635f9-55e9-4fad-be93-7c71ca1d362b/image.png" />
그 유명한 404 Not Found Error이다. 이 404는 HTTP 오류 코드 중 하나로, 브라우저가 요청한 페이지를 찾을 수 없다는 의미이다.</p>
<p>이러한 오류를 해결하기 위해서는 컨트롤러를 작성하여 /sbb URL에 대한 <strong>매핑</strong>을 추가하면 된다.
클라이언트(브라우저 등)의 페이지 요청이 발생하면 스프링 부트는 가장 먼저 이 컨트롤러에 등록된 URL 매핑을 찾고, 해당 URL 매핑을 발견하면 URL 매핑과 연결된 메서드를 실행한다.</p>
<p>웹 브라우저와 같은 클라이언트의 요청이 발생하면 서버 역할을 하는 스프링 부트가 응답해야 한다. 따라서 그러기 위해서는 URL이 스프링 부트에 매핑되어 있어야 하고 이를 위해서는 컨트롤러를 먼저 만들어야 한다.</p>
<p>URL 매핑을 추가하기 위해서 MainController.java 파일을 작성해보자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/20b3d7af-c488-4afc-a1a3-4299f96082b0/image.png" /></p>
<ol>
<li><p>MainController 클래스에 @Controller 어노테이션을 적용하면 MainController 클래스는 스프링 부트의 컨트롤러가 된다. 그리고 index 메소드의 @GetMapping 어노테이션은 요청된 URL(/sbb)과의 매핑을 담당한다. 브라우저가 URL을 요청하면 스프링 부트는 요청 페이지와 매핑되는 메소드를 찾아 실행한다. 정리하자면, 스프링 부트는 웹 브라우저로부터 <a href="http://localhost:8080.sbb">http://localhost:8080.sbb</a> 요청이 발생하면 /sbb URL과 매핑되는 index 메소드를 MainController 클래스에서 찾아 실행한다.</p>
</li>
<li><p>다시 <a href="http://localhost:8080/sbb">http://localhost:8080/sbb</a> URL을 호출해보자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/0035d485-bce4-4e2f-96e3-ee8b76d29008/image.png" />
404 에러에서 500 에러로 바뀌었다!
index 메소드를 호출하긴 했는데 오류가 발생한 것이다. 이 index 메소드가 아무 값도 리턴하지 않았기 때문이다. 오류를 해결하기 위해서는 클라이언트로 리턴할 응답을 생성해야 한다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/0b07196c-d956-423e-9faa-84658029b408/image.png" /></p>
</li>
<li><p>컨트롤러를 수정해보자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/fa891036-1d36-4b63-b7ff-cc136d2a2ad5/image.png" />
이렇게 index 메소드의 반환 자료형을 String으로 수정하고, 그에 맞게 String인 &quot;index&quot;를 반환하도록 한다. 여기서 @ResponseBody 어노테이션은 URL 요청에 대한 응답으로 문자열을 리턴하라는 의미로 쓰인다.</p>
</li>
<li><p>재실행하고 URL을 다시 호출해보자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/70f80e26-679c-4895-921f-b32de08201fa/image.png" />
정상적으로 문자열 &quot;index&quot;가 웹페이지에 출력되는 것을 확인할 수 있다.</p>
</li>
<li><p>출력되는 문자열을 수정해보자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/8aa51cfa-7f5a-47b3-af57-857aa8908d14/image.png" />
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/d8991bb4-37c0-4e68-b0be-06c8735cdc85/image.png" />
문자열을 수정해도 정상적으로 출력된다!</p>
</li>
</ol>
<h3 id="jpa로-데이터베이스-사용하기">JPA로 데이터베이스 사용하기</h3>
<p>만들어 볼 'SBB' 웹 페이지는 방문자들이 질문과 답변을 남길 수 있는 게시판 서비스이다.
SBB 게시판의 사용자가 질문이나 답변을 작성하면 데이터가 생성되는데, 이러한 데이터를 관리하려면 저장, 조회, 수정하는 기능을 구현해야 한다. </p>
<p>대부분의 웹 서비스들은 생성되는 데이터를 관리하고 처리하기 위해 데이터베이스를 사용한다.
데이터베이스(DB)는 데이터를 모으고 관리하는 저장소라고 할 수 있다.</p>
<p>여기서 문제는, DB를 관리하기 위해서는 SQL을 사용해야 한다. 스프링 부트와 달리 DB는 자바를 이해하지 못하기 때문이다. 
여기서!!! <strong>ORM(Object Relational Mapping)</strong>이라는 도구를 사용하면 자바 문법으로도 DB를 다룰 수 있다. 즉, ORM을 이용하면 개발자는 SQL을 직접 작성하지 않아도 DB의 데이터를 처리할 수 있다.</p>
<h4 id="orm과-jpa-이해하기">ORM과 JPA 이해하기</h4>
<ul>
<li><p>ORM이란?</p>
<ul>
<li>ORM은 DB의 테이블을 자바 클래스로 만들어서 관리할 수 있다.</li>
<li>SQL의 쿼리(Query)문과 ORM 코드(즉, 자바로 작성된 코드)를 비교하며 ORM을 이해해보자.</li>
<li>'post'라는 테이블에 데이터를 입력한다고 가정하고, 여기엔 id, title, content라는 열이 있다고 가정해보자.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/03f4d59c-9c2a-4fd5-8256-89a16c5299d4/image.png" /></li>
<li>이 테이블에 데이터를 저장하기 위해서는 이런 SQL 쿼리문을 아래와 같이 직접 작성해야 한다.<pre><code>insert into post (id, title, content) values (1, '질문 있습니다', '저녁 뭐 먹을까요?');
insert into post (id, title, content) values (2, '할 말 있습니다', '메롱 입니다');</code></pre></li>
<li>하지만! ORM을 사용하면 이런 쿼리문 대신 자바 코드로 다음처럼 작성할 수 있다.<pre><code>Post p1 = new Post();
p1.setId(1);
p1.setTitle(&quot;질문 있습니다&quot;);
p1.setContent(&quot;저녁 뭐 먹을까요?&quot;);
this.postRepository.save(p1);
</code></pre></li>
</ul>
<p>Post p2 = new Post();
p2.setId(2);
p2.setTitle(&quot;할 말 있습니다&quot;);
p2.setContent(&quot;메롱 입니다&quot;);
this.postRepository.save(p2);</p>
<pre><code>- 이와 같이 SQL의 쿼리문과 ORM 코드를 단순히 비교해보면 ORM 코드의 양이 더 많아 보이지만 별도의 SQL 문법을 배우지 않아도 DB를 사용할 수 있기 때문에 매우 편리하다.
- ORM 코드를 간단히 살펴보면 Post는 자바 클래스이며, 이처럼 데이터를 관리하는데 사용하는 ORM의 자바 클래스를 엔티티(entity)라고 한다.
- 엔티티는 데이터베이스의 테이블과 매핑되는 자바 클래스를 말한다.


</code></pre></li>
</ul>