<p>웹프로그래밍2 실습하다가 겪은 문제에 대해서 정리해보겠다.</p>
<p>일단 조건</p>
<ul>
<li>JDK 설치!</li>
<li>STS4 설치!</li>
<li>lombok 설치!</li>
</ul>
<p>내가 겪었던 문제는 <code>@Getter</code>, <code>@Setter</code> 어노테이션이 import는 되지만 관련 메소드를 사용 못하는 문제였다.
이는 lombok이 제대로 안 깔려서 일어나는 문제였다!</p>
<h3 id="해결방법">해결방법</h3>
<ol>
<li>cmd를 <strong>관리자 모드</strong>로 실행
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/d336ef31-7d8f-4ee7-a3a8-8b05c7c4146e/image.png" /></li>
<li>lombok이 다운로드된 경로로 이동</li>
<li><code>java -jar lombok.jar</code> 명령어 실행</li>
<li>STS4 재실행</li>
</ol>
<p>의존성 추가 등등 다른 방법들도 다 해봤는데 최종적으로 이게 통했다.
왜 제대로 안 깔렸는지는 의문</p>
<p>근데 이렇게 하고 나니까 갑자기 다른데서 오류가 생겨서</p>
<pre><code>implementation 'org.springframework.boot:spring-boot-starter-web'</code></pre><p>요걸 build.gradled 의존성 부분에 추가했더니 괜찮아지더라</p>