<h3 id="master-main">master? main?</h3>
<p>처음 깃허브 리포지토리를 로컬과 연결하고 푸시하게 되면 깃허브의 main 브랜치가 아닌 master 브랜치에 푸시됩니다. 왜일까요? </p>
<blockquote>
<p>그냥 Git에서 master 브랜치를 사용하고, Github애서 main 브랜치를 사용하기 때문이다.</p>
</blockquote>
<p>아주 단순한 이유였습니다. </p>
<p><strong>자세히 설명해보자면,</strong> master라는 이름을 Github에서 사용하지 않게 된 이유는 다름 아닌 master라는 이름이 주종관계를 연상케 한다는 것이었다. 2020년 흑인 인권 운동의 영향으로 Github 뿐만이 아닌 다양한 오픈 소스 커뮤니티에서 기본 브랜치명을 main으로 변경하게 된 것입니다. 우와 유익하다!
<span style="color: #C3C3C3;">근데 왜 Git에서는 안 바꾼 거지...? 있는 걸 없애면 위험해서? 궁금하지만 일단 묻어두기로 한다.</span></p>
<p>그런 고로, main을 기본으로 사용하는 것을 권장한다.
아래는 master에서 main으로 기본 브랜치를 변경하는 방법이다!</p>
<h3 id="로컬에서-master---main-기본-브랜치-설정">로컬에서 master -&gt; main 기본 브랜치 설정</h3>
<ol>
<li><p>git 버전 확인</p>
<pre><code># git 버전이 2.28.0 이상이어야 기본 브랜치명 변경 가능
git version</code></pre></li>
<li><p>로컬 리포지토리의 default branch를 main으로 변경</p>
<pre><code># global gitconfig의 init.defaultBranch 값을 main으로 설정한다. 
git config --global init.defaultBranch main</code></pre></li>
<li><p>확인해보기</p>
<pre><code># vscode로 gitconfig 파일을 연다.
code ~/.gitconfig
# 또는 터미널에서 확인
git config --list --global</code></pre><p>이제, 파일을 자세히 살펴보면 defaultBranch = main이라 설정되어 있는 부분이 있을 것입니다.(근데 사실 전 vscode에는 안 나왔어요... 그래서 터미널로 확인!)
확인 끝~</p>
</li>
</ol>
<p>하지만!! 이것만 한다고 브랜치가 바뀌지 않는 걸 볼 수 있습니다. 재시작 해도 안 바뀝니다.
이건 사실 처음에 해줘야 하는 것인데요...
이 글을 보는 사람 대부분은 master에 이미 push하고 찾아보는 사람일 것 같습니다.
그래서~</p>
<h3 id="master---main-로컬-브랜치-변경">master -&gt; main 로컬 브랜치 변경</h3>
<ol>
<li><p>master 브랜치 이름 변경</p>
<pre><code># 현재의 master 브랜치를 main 브랜치로 이름을 변경
git branch -m master main</code></pre></li>
<li><p>local의 변경 사항을 remote에 push</p>
<pre><code># 로컬의 브랜치만 변경된 것이므로, 깃허브의 main 원격 브랜치에 변경 사항 push
git push -u origin main
# master 브랜치를 원격 저장소에서 삭제(위에 것 꼭 하고 하기!! 안 하면 변경 사항 다 날아감)
git push origin --delete master</code></pre></li>
</ol>
<p>요렇게 해주시면 정상적으로 변경된 것을 볼 수 있습니다!</p>
<h2 id="span-stylecolorred그런데span"><span style="color: red;">그런데...</span></h2>
<p>여기서부터는 저 혼자 삽질한 것 기록용입니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/c6294cee-9068-4dc6-89a9-809692a219ac/image.png" />
얘 왜이러는?</p>
<p>remote에 있는 readme가 로컬에 없어서 거부됐네요.
이런 상황도 발생하는구나...
<del>어거지로</del>급한대로
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/1fb03731-bd2a-4994-bc8d-b1087ee609a7/image.png" />
이렇게 cherry pick 해주고 다시 push 했는데?
그래도 거부!!!!!!!!!!!!!!!</p>
<p>그래서 원격 브랜치에 checkout 했다가 이것저것 만지다가</p>
<h4 id="span-stylecolorred파일이-다-날아가버렸습니다span"><span style="color: red;">파일이 다 날아가버렸습니다....</span></h4>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/cb77c160-aaf4-4a04-a113-199c6d710951/image.jpg" />
<span style="color: #C3C3C3;">...</span></p>
<p>사실 전 진짜 기본 설정만 해둬서... spring starter로 생성한 zip 파일을 또 오픈하면 해결되는 문제라 다행이었습니다.</p>
<p>이번엔 마음을 가다듬고...</p>
<pre><code>// 관련되지 않은 원격 브랜치 기록도 가져오기
git pull origin main --allow-unrelated-histories</code></pre><p>이렇게 원격 브랜치의 기록을 가져온 후, push를 해주었더니 정상적으로 동작했습니다.</p>
<p>이 글을 읽는 분들은 꼭 이렇게 해결하시길 바랍니다...
readme 파일만 있다고 간과했다가 이런 일이 생겼네요.
사실 프로젝트에서는 이미 세팅된 리포지토리에서 브랜치 새로 파서 작업하고, 원격 메인 브랜치에 변경 사항 있으면 로컬 메인에 pull 땡겨오고 그걸 작업 브랜치에 merge하는 방식으로만 작업해서 전혀 알지 못하던 부분이었습니다. 이번에 싼 값에 배운 기분이라 오히려 뿌듯하네요!</p>
<p>굿굿!!</p>
<blockquote>
<p><strong>오늘의 교훈</strong>
원격 브랜치의 상태를 모두 아는 상태에서만 push를 시도해야만 한다!</p>
</blockquote>