<h2 id="stash">stash</h2>
<p>git stash는 현재 작업 진행 상황을 일시정지(다른 곳에 저장)하고 다시 가져올 수 있도록 합니다.</p>
<p>저 같은 경우는 메인 브랜치랑 상관 없는 내용을 코딩 중인데, pr을 작게작게 올리기로 해서 커밋을 하나도 안 하고 있었습니다. 그러다 보니까 점점 커밋할 것이 쌓여갔고... 그 와중에! 원격 브랜치에 최근에 머지된 것을 가져올 필요가 있게 됐습니다! </p>
<p>기존에 알고 있던 fetch-merge나 rebase는 커밋을 모두 해야 사용 가능했다! 그래서 새로운 방법을 찾은 것이 바로 stash를 사용하는 방법이다.</p>
<h3 id="사용-방법">사용 방법</h3>
<h4 id="stash로-임시-저장">stash로 임시 저장</h4>
<pre><code>git stash </code></pre><p>현재 적용된 커밋 이후의 변경 사항이 stash로 이동한다.</p>
<pre><code>git stash push -m &quot;메시지&quot;</code></pre><p>이러면 메시지와 함께 저장할 수 있다.</p>
<h4 id="로컬-메인-브랜치에-pull-받기">로컬 메인 브랜치에 pull 받기</h4>
<pre><code>git checkout 풀 받을 브랜치
git pull origin 풀 받을 브랜치</code></pre><h4 id="작업-브랜치로-돌아가서-머지하기">작업 브랜치로 돌아가서 머지하기</h4>
<pre><code>git checkout 작업 브랜치
git merge --ff-only 풀 받은 브랜치
</code></pre><p>근데, 나 같은 경우는 저 머지가 충돌 때문인지 통하질 않아서</p>
<pre><code>git checkout 작업 브랜치
git reset --hard 풀 받은 브랜치
</code></pre><p>이렇게 해서 작업 브랜치를 풀 받은 브랜치와 완전히 같은 상태로 만들어버렸다.
참고로 이 두 방법 다 커밋 내역이 더러워지지 않는다고 한다!</p>
<h4 id="stash에-있는-변경-사항-재적용">stash에 있는 변경 사항 재적용</h4>
<pre><code>git stash pop</code></pre><p>진짜 전혀 상관 없는 내용이 아닌 이상 무조건 충돌이 생길 텐데, 인텔리제이면 Commit  탭에 뜨는 Resolve Conflicts에 들어가서 해결해주자!
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/9aaca98b-0baa-4867-abaf-24038279a680/image.png" /></p>
<p><strong>해결~~</strong></p>