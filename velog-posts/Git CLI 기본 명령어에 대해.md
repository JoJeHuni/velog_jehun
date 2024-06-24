<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1c310953-9e6b-4749-ab0c-e26665719438/image.png" /></p>
<p>Git Bash 기준으로 Git CLI 기본 명령어를 배워 정리하려고 한다.
Command Line Interface 의 줄임말로 Window, GUI 환경과는 다르게 마우스 없이도 사용가능하게끔 만들었다.</p>
<p>Git Bash에서는 shell 명령어를 활용해 OS와 대화할 수 있다.
(Window는 터미널 창이 같은 역할을 한다. + linux 또한 사용 가능하게끔 해뒀다. )</p>
<hr />
<h1 id="git-cli-기본-명령어">Git CLI 기본 명령어</h1>
<h2 id="git-설치">Git 설치</h2>
<pre><code class="language-shell"># 설치된 깃 버전 확인
$ git --version</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f9592fc5-8764-4ae1-991f-dd4057983bd2/image.png" /></p>
<pre><code class="language-shell"># apt-get으로 깃 설치 가능(ubuntu linux 환경일 때)
$ sudo apt-get install git-core</code></pre>
<h2 id="git-초기-설정">Git 초기 설정</h2>
<pre><code class="language-shell"># 깃 초기 설정 ( '' 안에 username, useremail을 적어주면 된다 )
$ git config --global user.name '{username}'
$ git config --global user.email '{user.email}'

# CRLF 문제 해결
$ git config --global core.autocrlf input

# 깃 설정 확인
$ cat ~/.gitconfig</code></pre>
<p><del>진행하면서 CRLF 문제 해결하는 것을 하지 않아서 중간에 했다.</del></p>
<hr />
<h2 id="기본-사용법">기본 사용법</h2>
<h3 id="1-새로운-디렉토리-생성-mkdir">1. 새로운 디렉토리 생성 (mkdir)</h3>
<pre><code class="language-shell">$ mkdir -p c:/gitwork/testproject</code></pre>
<p>윈도우 환경에서 실제로 만들어졌음을 알 수 있다.<br /><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/df2ddd4d-34e8-4c10-83ae-dbc6961269a8/image.png" /></p>
<hr />
<h3 id="2-directory에서-git-초기화">2. directory에서 Git 초기화</h3>
<pre><code class="language-shell"># 생성한 디렉토리로 이동
$ cd c:/gitwork/testproject

# 현재 git의 디폴트 브랜치 확인
$ git config --get init.defaultBranch

# 기본 브랜치 main으로 변경
$ git config --global init.defaultBranch main
$ git config --get init.defaultBranch

#.git 디렉토리 생성 및 확인
$ git init
$ ls -al</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/81864575-9847-4700-847a-8f25af837841/image.png" /><br /><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e914e915-ad03-4fb6-afc4-86c2e6d1edad/image.png" /></p>
<hr />
<h3 id="3-repository에-파일-추가하기-touch">3. Repository에 파일 추가하기 (touch)</h3>
<pre><code class="language-shell"># 파일 추가하기
$ touch test.txt

# 코드 작성 (수정 후 :wq) -&gt; test.txt 에는 &quot;Hello world&quot; 라는 문구를 적어줬다.
$ vim test.txt</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/88aff9a8-0884-4184-8bfb-f84c7ee15905/image.png" />
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2895c99d-a821-4451-a85c-0bb9bcf50836/image.png" /></p>
<p>아직은 vim 단축키에 관해 삽입 (i키), 저장 (w키), 나가기(q키) 등 몇 가지 아는 것이 없어서 방향키 (HJKL키)와 같이 몇 가지 단축키에 관해서도 다뤄봐야겠다는 생각을 했다.</p>
<hr />
<h3 id="4-source-code를-staging-area에-올리기-add">4. Source code를 Staging Area에 올리기 (add)</h3>
<pre><code class="language-shell">$ git add test.txt</code></pre>
<h3 id="5-repository에-commit-하기">5. Repository에 commit 하기</h3>
<pre><code class="language-shell">$ git commit -m 'first commit'</code></pre>
<ul>
<li>추가 내용<pre><code class="language-shell">$ git commit -m 'first commit
&gt;
&gt; 커밋 내용'</code></pre>
제목과 커밋 내용을 작성하는 방법은 '(콜론)을 닫지 않고 Enter를 눌러서 다음 줄로 넘어가면 된다.</li>
</ul>
<hr />
<h3 id="6-git의-흐름-확인하기-log">6. Git의 흐름 확인하기 (log)</h3>
<pre><code class="language-shell"># 변경 이력 확인
$ git log

# 변경 이력과 함께 차이점 표시(끌 때는 q)
$ git log -p</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bf7cdf57-d460-4187-8e4a-061b18ab42c0/image.png" /></p>
<hr />
<h3 id="7-특정-커밋과-수정한-후의-차이-표시-diff">7. 특정 커밋과 수정한 후의 차이 표시 (diff)</h3>
<p>여기서 test.txt를 약간 수정해보고 git status를 해보았다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c68bb278-717b-4c96-ae1c-4f8397cb2218/image.png" /></p>
<p>이 때 diff를 활용하면?</p>
<pre><code>$ git diff [7자리 커밋 오브젝트명]</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e7b9bae7-f45f-47d3-9a01-823ce0778865/image.png" /></p>
<hr />
<h2 id="기본-명령어-정리">기본 명령어 정리</h2>
<table>
<thead>
<tr>
<th>명령어</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>git init</td>
<td>레포지토리 생성</td>
</tr>
<tr>
<td>git add</td>
<td>commit 대상으로 등록</td>
</tr>
<tr>
<td>git commit</td>
<td>commit 수행</td>
</tr>
<tr>
<td>git status</td>
<td>작업 트리의 상태 출력</td>
</tr>
<tr>
<td>git diff</td>
<td>차이 표시</td>
</tr>
<tr>
<td>git log</td>
<td>이력 표시</td>
</tr>
</tbody></table>
<hr />
<p>현재는 first commit 이후에 한 가지 커밋을 더 한 상태이다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9dec4edf-9bf2-4ffa-8968-5db5b9e73752/image.png" /></p>
<h3 id="8-이전-커밋으로-돌아가기-revert">8. 이전 커밋으로 돌아가기 (revert)</h3>
<pre><code class="language-shell">$ git revert [취소하고 싶은 커밋의 오브젝트명]</code></pre>
<hr />
<h3 id="9-branch-관련-명령어">9. Branch 관련 명령어</h3>
<ul>
<li>Branch 목록 출력<pre><code class="language-shell">$ git branch</code></pre>
</li>
</ul>
<hr />
<ul>
<li>새로운 Branch 생성 및 확인<pre><code class="language-shell">$ git branch [브랜치명]
</code></pre>
</li>
</ul>
<h1 id="생성된-브랜치-확인">생성된 브랜치 확인</h1>
<p>$ git branch</p>
<pre><code>---
- 해당 Branch로 전환
```shell
$ git checkout [전환할 브랜치]

# 브랜치 전환된 것 확인
$ git branch</code></pre><hr />
<ul>
<li>main Branch로 전환<pre><code class="language-shell">$ git checkout main</code></pre>
</li>
</ul>
<hr />
<ul>
<li>지정한 Branch의 내용을 현재 Branch에 merge<pre><code class="language-shell">$ git merge [merge할 브랜치명]
</code></pre>
</li>
</ul>
<h1 id="merge-메시지-뜨면-작성할-것">merge 메시지 뜨면 작성할 것</h1>
<pre><code>
내가 현재 main 에서 feature1 branch를 merge 하고 싶다면?
main 을 체크아웃한 뒤
```shell
$ git merge feature1</code></pre><hr />
<ul>
<li>Branch 삭제</li>
</ul>
<p>이 때는 내가 삭제할 브렌치에서 삭제하는 것이 아닌, 다른 브렌치로 체크아웃으로 옮겨간 뒤에 삭제할 수 있다.</p>
<pre><code class="language-shell">$ git branch -d [브랜치명]</code></pre>
<hr />
<h3 id="10-repository-복제하기-clone">10. Repository 복제하기 (clone)</h3>
<pre><code class="language-shell">$ mkdir -p c:/gitwork/testproject2
$ cd c:/gitwork/testproject2

$ git clone [복제할 레포지토리]

# 복제 된 브랜치 확인</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1d6769fb-8371-4fe0-b86e-80676f9d5edd/image.png" /></p>
<hr />
<h3 id="11-원격-저장소에-올리기-push">11. 원격 저장소에 올리기 (push)</h3>
<pre><code class="language-shell">$ git branch -M main
$ git remote add origin [원격지 https url 주소]
$ git push -u origin main</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0e38bdb2-c603-48d1-b527-1bc322c626eb/image.png" /></p>
<p>board.txt 파일을 만들어서 올려보았다.</p>
<hr />
<p><a href="https://git-scm.com/docs">Git 레퍼런스 공식사이트 가서 보기</a></p>