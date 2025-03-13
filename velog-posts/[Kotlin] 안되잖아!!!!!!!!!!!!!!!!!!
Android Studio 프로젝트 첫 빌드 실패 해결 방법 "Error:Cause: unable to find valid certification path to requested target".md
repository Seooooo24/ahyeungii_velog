<p>이번에 &quot;코틀린을 활용한 안드로이드 프로그래밍(한빛아카데미(교재))&quot; 서적을 보면서 코틀린을 공부할 준비 중이었는데, 계속 초기 빌드에서 에러가 났습니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/3a0835c6-54a0-4cdb-9cb3-0e8f341e4100/image.png" /></p>
<blockquote>
<p><span style="color: red;">Error:Cause: unable to find valid certification path to requested target</span></p>
</blockquote>
<p>위 사진과 같이 에러 메시지가 나와서, 무작정 구글링해보았습니다.</p>
<p>아직까지 해결 못 하신 분들 보세요</p>
<h1 id="span-stylecolorgreen이-에러-제가-해결해드리겠습니다span"><span style="color: green;">이 에러, 제가 해결해드리겠습니다!!!!!!!!!!!!!!!!!</span></h1>
<p>지피티한테 물어보신 분들 있나요? 얘는 계속 JAVA_HOME 확인하라고 합니다. <del>짜증나게</del>
<strong>얘 말대로 계속 따라하다 보면 안드로이드 스튜디오님이 돌아가시기 전에 제가 돌아가실 것 같습니다.</strong> </p>
<p>학교/회사 네트워크 보안 때문이라는 검색 결과를 보고 따라해봤지만 안 됐습니다.(학교에서 하고 있었음)
jcenter()를 maven()으로 바꿔보라는 것도 해봤지만 안 됐습니다.
jdk 환경변수 설정 오류도 아니었습니다. </p>
<h3 id="그래서-어떻게-해결했는데">그래서 어떻게 해결했는데?</h3>
<p>stackoverflow에서 뒤져본 결과,
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/c24e6795-06a7-41e4-80a1-e82ddb85cac5/image.png" />
이 build.gradle 파일 속 repositories를</p>
<pre><code>repositories {
    google()
    mavenCentral()
}</code></pre><p>이렇게 바꾸면 
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/7abe9c8a-42df-4413-9324-9244dce03d6b/image.png" />
아름다운 초록색 success 문구를 볼 수 있습니다.</p>
<p>안드로이드 스튜디오가 처음인 분들을 위해 설명드리자면
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/58d6d882-05dd-4103-b3b4-2f4177e03133/image.png" />
이 코끼리에 화살표 있는 버튼 누르면 다시 빌드됩니다.</p>
<p>그런데...
이렇게 했는데도 안됐다?
그럼 저도 모릅니다... 당신의 살 길은 스택 오버플로우에 있습니다 파이팅</p>
<p>[참고한 답변 링크]
<a href="https://stackoverflow.com/questions/46094911/error-in-android-studio-3-0-when-i-sync-gradle-errorcause-unable-to-find-vali">https://stackoverflow.com/questions/46094911/error-in-android-studio-3-0-when-i-sync-gradle-errorcause-unable-to-find-vali</a></p>
<h3 id="이게-왜-발생한-건데">이게 왜 발생한 건데?</h3>
<p>저도 잘 몰라서 찾아봤습니다.
이유는 바로, 저장소 jcenter가 지원을 21년에 중단했기 때문이었습니다.
허무...
그래서 다른 저장소를 사용하여야 한다고 합니다.
사실 제가 찾아본 예시에서는 
maven { &quot;<a href="http://jcenter.bintray.com&quot;">http://jcenter.bintray.com&quot;</a> }
이걸 사용했던데, 이것도 이유 찾아보면서 확인해보니까 안정적인 건 아닌 것 같아서 저는 가장 잘 알려진 mavenCentral을 사용하였습니다.</p>