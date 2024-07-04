<p>시작하기 전 디렉토리 구조를 '트리' 형태로 볼 수 있는 패키지를 설치하자</p>
<pre><code class="language-shell">sudo apt install tree</code></pre>
<hr />
<h1 id="파일-시스템">파일 시스템</h1>
<h2 id="파일-구조-및-대표-디렉토리">파일 구조 및 대표 디렉토리</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/041ffa9f-318c-4722-b8db-ea4f3f9b7216/image.png" /></p>
<p>리눅스는 모든 정보(데이터)가 파일로 보존된다.
문서, 이미지, 영상, 프로그램들이 이에 해당한다. 또한, 사용자의 데이터 뿐 아니라 시스템을 구성하는 장치 조차도 파일로 다룬다.</p>
<p>리눅스 커널 뿐 아니라 시스템 설정 마저도 파일로 기록되고, 최상위에 /디렉토리(루트 디렉토리)가 있고 아래에 디렉토리와 파일이 있는 계층 구조를 가지고 있다.(트리 혹은 디렉토리 트리로 표현)</p>
<blockquote>
<p>즉, 리눅스는 시스템 전체에 단 하나의 트리만 가지게 된다.</p>
</blockquote>
<ul>
<li><strong>/</strong><ul>
<li>리눅스 파일 체제의 최상의 디렉토리</li>
<li>모든 디렉토리들의 시작점으로 일반적인 데이터를 저장하지 않는다.</li>
</ul>
</li>
<li><strong>/bin</strong><ul>
<li>시스템을 부팅하거나 복구 모드로 운영할 때 필요한 필수적인 바이너리 실행 파일들이 들어있다.</li>
<li>ls, cp, mv, cat과 같은 기본적인 명령어들이 여기 포함된다.</li>
</ul>
</li>
<li><strong>/dev</strong><ul>
<li>‘device’의 약자로, 시스템의 장치 파일이 위치하는 곳이다.</li>
<li>하드 드라이브, USB 드라이브, 터미널, 키보드 등 다양한 하드웨어 장치를 나타내는 파일들이 포함된다.</li>
</ul>
</li>
<li><strong>/etc</strong><ul>
<li>시스템 전반에 걸쳐 사용되는 설정 파일들이 저장되는 위치이다.</li>
<li>부팅 스크립트, 네트워크 설정 파일, 사용자 계정 정보 등이 포함된다.</li>
</ul>
</li>
<li><strong>/home</strong><ul>
<li>사용자의 개인 데이터와 설정 파일이 저장되는 디렉토리이다.</li>
<li>시스템에 있는 각 사용자 계정은 여기에 자신의 홈 디렉토리를 가지며, 사용자의 문서, 사진, 설정 파일 등이 이곳에 저장된다.</li>
</ul>
</li>
<li><strong>/sbin</strong><ul>
<li>‘system binary’의 약자로, 시스템 관리와 관련된 실행 파일들이 들어있는 디렉토리이다.</li>
<li>주로 시스템 관리자(root)에 의해 사용되며, 시스템 부팅, 복구, 관리 등에 필요한 명령어들이 포함된다.</li>
</ul>
</li>
<li><strong>/tmp</strong><ul>
<li>임시 파일을 저장하는 곳으로, 시스템이나 사용자가 일시적으로 사용하는 파일들을 위한 공간이다.</li>
<li>이 디렉토리의 내용은 보통 재부팅 시에 지워질 수 있다.</li>
</ul>
</li>
<li><strong>/usr</strong><ul>
<li>‘Unix System Resources’의 약자로, 사용자들에 의해 사용되는 응용 프로그램과 파일들이 위치하는 디렉토리이다.</li>
<li>/usr/bin, /usr/lib, /usr/local등과 같은 하위 디렉토리들을 포함하고 있다.</li>
</ul>
</li>
<li><strong>/var</strong><ul>
<li>‘variable’의 약자로, 자주 변하는 데이터를 저장하는 곳이다.</li>
<li>이곳에는 로그 파일, 메일 박스, 임시 파일 등이 저장되며 시스템 운영 중에 지속적으로 변할 수 있다.</li>
</ul>
</li>
</ul>
<hr />
<h1 id="파일-관련-간단-명령어">파일 관련 간단 명령어</h1>
<h2 id="절대-경로-상대-경로">절대 경로, 상대 경로</h2>
<p>일단 명령어를 바로 들어가지 않고 경로에 대한 개념을 먼저 이야기 한다.</p>
<h3 id="절대-경로">절대 경로</h3>
<p><strong>루트 디렉토리부터 해당 파일에 이르는 경로를</strong> 표시하는 것을 <strong>절대 경로</strong>라고 한다.</p>
<h3 id="상대-경로">상대 경로</h3>
<p><strong>현재 디렉토리의 위치를 기준으로 표기</strong>하는 경로를 <strong>상대 경로</strong>라고 한다.</p>
<h4 id="pwd--현재-위치의-디렉토리를-절대경로로-보여주는-명령어">pwd : 현재 위치의 디렉토리를 절대경로로 보여주는 명령어</h4>
<p>절대 경로를 보는 명령어 : pwd</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7571e277-4ede-4817-9534-b1aa55ecbbb6/image.png" /></p>
<hr />
<h2 id="ls-명령어">ls 명령어</h2>
<blockquote>
<p>💡 ls 명령어를 통해 디렉토리 내의 파일 및 디렉토리 목록을 출력해 준다.</p>
</blockquote>
<pre><code class="language-bash">## 시간순으로 나열하기
ls -lt

## 큰 사이즈 우선
ls -lSh

## 작은 사이즈 우선
ls -lSrh

## 인간이 보기 쉬운 용량
ls -lh</code></pre>
<table>
<thead>
<tr>
<th>옵션</th>
<th>단어</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>-r</td>
<td>reverse</td>
<td>거꾸로 나열한다.</td>
</tr>
<tr>
<td>-R</td>
<td>recursive</td>
<td>하위 디렉토리도 검색한다.</td>
</tr>
<tr>
<td>-h</td>
<td>human</td>
<td>사이즈를 인간이 보기 쉽게 K, M, G 단위로 표시한다.</td>
</tr>
<tr>
<td>-t</td>
<td>time</td>
<td>시간 순서로 나열한다.</td>
</tr>
<tr>
<td>-a</td>
<td>all</td>
<td>숨겨진 파일이나 디렉토리도 전부 표시한다.</td>
</tr>
<tr>
<td>-l</td>
<td>long</td>
<td>자세한 내용을 출력한다.</td>
</tr>
<tr>
<td>권한: 포함된파일수 : 소유자 : 그룹 : 파일크기 : 수정일자 : 파일이름</td>
<td></td>
<td></td>
</tr>
<tr>
<td>-S</td>
<td>size</td>
<td>파일의 크기 순으로 표시한다.</td>
</tr>
</tbody></table>
<blockquote>
<p>모든 명령어의 더 많은 옵션들은<code>--help</code> 를 더 적어서 볼 수 있다.</p>
</blockquote>
<pre><code class="language-shell">ls --help</code></pre>
<hr />
<h1 id="파일-조작-명령어">파일 조작 명령어</h1>
<h2 id="mkdir-명령어">mkdir 명령어</h2>
<p>디렉토리 만드는 명령어</p>
<pre><code class="language-shell">## 현재 디렉토리 확인
$ pwd

## testDir 디렉토리 만들기
$ mkdir testDir

## 디렉토리 확인
$ ls

## 만들어진 testDir로 이동
$ cd testDir</code></pre>
<p>중간 경로가 없을 시에는 -p 옵션을 준다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fe85f5c2-5d07-4f17-b327-97310b1415ea/image.png" /></p>
<p>apt install tree 로 설치한 tree를 적어보면
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b7b4865f-d009-4520-a234-4c578bbacd0c/image.png" /></p>
<p>위 사진처럼 결과가 나온다.</p>
<hr />
<h2 id="touch-명령어">touch 명령어</h2>
<p>파일을 만드는 명령어</p>
<pre><code class="language-shell">$ touch &lt;생성할 파일1&gt; &lt;생성할 파일2&gt; ...</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4a0644ad-d5f6-44d9-b827-e3164eb0cbe4/image.png" /></p>
<hr />
<h2 id="rm-명령어">rm 명령어</h2>
<p>파일 및 디렉토리를 지우는 명령어</p>
<pre><code class="language-shell">## 파일 삭제
$ rm [옵션] &lt;삭제할 파일1&gt; &lt;삭제할 파일2&gt; ...</code></pre>
<h3 id="파일-삭제">파일 삭제</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c41a9ac6-8a3c-42f8-828d-1a49028f31a7/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a9088c8f-b487-40d2-8533-cec6dfc53520/image.png" /></p>
<h3 id="디렉토리-삭제">디렉토리 삭제</h3>
<pre><code class="language-shell">## -r 옵션을 주어 디렉토리를 삭제할 수 있다.
$ rm -r &lt;삭제할 디렉토리1&gt; &lt;삭제할 디렉토리2&gt; ...

## 추가 질문 없이 내용물이 삭제되므로 확인 문구를 띄우고 싶으면 -i 옵션을 준다.
$ rm -r -i &lt;삭제할 디렉토리1&gt; &lt;삭제할 디렉토리2&gt; ...

## 비어있는 디렉토리는 rmdir로도 지울 수 있다.
$ rmdir &lt;삭제할 디렉토리1&gt; &lt;삭제할 디렉토리2&gt; ...</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/458e5cca-4f80-4a3d-8239-923b8683c66b/image.png" /></p>
<hr />
<h2 id="cat-명령어">cat 명령어</h2>
<p>파일 내용을 출력할 수 있다.</p>
<pre><code class="language-shell">$ cat [옵션] &lt;파일 이름1&gt; &lt;파일 이름2&gt; ...</code></pre>
<ul>
<li>/etc/hostname 파일 출력하기<pre><code class="language-shell">## 파일 하나 출력하기
$ cat /etc/hostname
</code></pre>
</li>
</ul>
<h2 id="파일-여러개-한번에-출력하기">파일 여러개 한번에 출력하기</h2>
<p>$ cat /etc/hostname /etc/crontab</p>
<h2 id="행번호-붙여-출력하기">행번호 붙여 출력하기</h2>
<p>$ cat -n /etc/crontab</p>
<pre><code>
---
## cp 명령어
파일 복사 명령어, 디렉토리도 가능하다
```shell
$ cp [옵션] &lt;복사할 파일&gt; ... &lt;복사할 위치&gt;</code></pre><ul>
<li><strong>파일을 다른 이름으로 복사하기 (확인 질문을 받으려면 <code>-i</code> 활용)</strong><pre><code class="language-shell">## test1 파일 만들기
$ touch test1.txt
</code></pre>
</li>
</ul>
<h2 id="test1txt을-test2txt로-복사하기">test1.txt을 test2.txt로 복사하기</h2>
<p>$ cp test1.txt test2.txt</p>
<h2 id="test1txt를-기존-파일에-덮어쓸-때-확인하기">test1.txt를 기존 파일에 덮어쓸 때 확인하기</h2>
<p>$ cp -i test1.txt test2.txt</p>
<h2 id="복사된-파일-확인">복사된 파일 확인</h2>
<p>$ ls</p>
<pre><code>
-** 파일을 특정 디렉토리 안에 복사하기**
```shell
## dir1 디렉토리 생성하기
$ mkdir dir1

## test1.txt를 dir1 디렉토리로 복사하기
$ cp test1.txt dir1

## dir1 폴더에 복사된 것을 확인
$ ls dir1 </code></pre><p><strong>- 디렉토리 복사 (재귀 옵션인 <code>-r</code>이 필요)</strong></p>
<pre><code class="language-shell">$ cp -r dir1 dir2</code></pre>
<hr />
<h2 id="mv-명령어">mv 명령어</h2>
<p>파일 위치 옮기기 가능</p>
<pre><code class="language-shell">$ mv [옵션] &lt;이동할 파일&gt; ... &lt;이동할 위치&gt;</code></pre>
<p>같은 파일 간 이동이면 이름이 바뀌게 되고, 새로운 디렉토리를 생성하고 이동하면 해당 디렉토리로 이동한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8bbf7cae-c33c-4526-8b20-5e9ab4ecb320/image.png" /></p>
<ul>
<li>여러 파일 옮기기<pre><code class="language-shell">## 파일 두개 더 생성
$ touch test3.txt test4.txt
</code></pre>
</li>
</ul>
<h2 id="testdir2-디렉토리로-한번에-이동">testDir2 디렉토리로 한번에 이동</h2>
<p>$ mv test3.txt test4.txt testDir2</p>
<h2 id="이동된-파일들-확인">이동된 파일들 확인</h2>
<p>$ ls testDir2</p>
<pre><code>![](https://velog.velcdn.com/images/jojehuni_9759/post/e2bb0083-cce1-47aa-9ade-40a8fd015e18/image.png)</code></pre>