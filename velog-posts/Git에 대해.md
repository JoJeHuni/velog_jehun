<p>나는 Git이 버전 관리 프로그램으로 말그대로 프로젝트 시작, 기능 추가, 기능 개선을 포함한 마무리 할 때까지의 변경점들을 업데이트해가며 버전 관리할 수 있다라는 간단한 것만 알고 있다.</p>
<p>사용할 때에는 크게 문제를 못 느꼈지만, 더욱 자세하게 알아보고 정리를 하는 것은 항상 중요하다고 생각한다.</p>
<hr />
<p><strong>Git을 왜 쓰는가?</strong> -&gt; 역사에 대해 알아보면 알 수 있다.</p>
<p>만약 노트북 or 컴퓨터로 코드나 메모장에 무언가를 적었을 때, 사라지면 안 되기 때문에 내 컴퓨터에 저장을 해야 한다.</p>
<p>하지만 이 때의 문제점? = 고장나면 잃게 된다. -&gt; 이걸 해결하기 위해 usb나 외장 하드에 데이터를 옮기면 된다.</p>
<p>그럼에도 백업한 것(usb, 외장 하드) 또한 고장나거나 분실하면 데이터를 잃게 된다.</p>
<p>그래서 옛날부터 SVN을 사용하거나 각각의 데이터 센터를 이용해 관리했었다.</p>
<p>데이터 센터 또한 화재가 나면 데이터를 잃을 수 있기에 결국 원격 저장소(ex. GitHub, 클라우드) 에 저장하는 것을 지향하기 시작하면서 쓰게 됐다.</p>
<hr />
<h1 id="git">Git</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ea97a553-b874-4a63-b990-52f14d33d862/image.png" /></p>
<h2 id="git-용어">Git 용어</h2>
<blockquote>
<p><strong>Git 및 Github의 전반적인 용어</strong></p>
</blockquote>
<ul>
<li><strong>Project Source Code</strong> : 소스코드가 있는 프로그램 작성 파일<ul>
<li><strong>Staging Area</strong> : commit할 것들을 담아놓는 임시 공간</li>
</ul>
</li>
<li><strong>Local Repo</strong> : 개발자가 Project Source Code를 개발 중인 컴퓨터에 저장하는 곳</li>
<li><strong>Remote Repo (=Github)</strong> : 작업 중인 컴퓨터가 아닌, github와 같은 공유 저장소</li>
</ul>
<blockquote>
<p><strong>Git에서 하는 작업 용어</strong></p>
</blockquote>
<ul>
<li>add : 프로젝트 파일 속 변경된 것을 Staging Area에 올리는 작업</li>
<li>commit : Staging Area에 있는 것은 Local Repo로 저장하는 작업</li>
<li>push : 로컬에 있던 것을 Remote Repo(원격 저장소)로 올리는 작업</li>
<li>fetch : 원격 저장소로부터 필요한 파일을 로컬로 받아오는 것</li>
<li>merge : 로컬에 변경 사항을 Project에 합치는 작업
  (Remote Repo에서 받은 것과 내 프로젝트 간에 충돌 발생할 수 있음)</li>
<li>pull : fetch + merge (merge를 포함하므로 충돌 발생할 수 있음)</li>
</ul>
<hr />
<h3 id="충돌에-대해">충돌에 대해</h3>
<p>충돌에 대해 예를 들어 설명을 해보려고 한다.</p>
<p>2명의 개발자(p1, p2)가 같이 협업을 한다고 하자.
원격저장소에 A, B 라는 text 파일이 저장돼 있는 상태이다.</p>
<ol>
<li>Merge 시  에러가 나는 경우 - 개발자가 각각의 파일을 변경한 상태일 때<blockquote>
<p>p2가 B파일을 변경하고 commit -&gt; push를 해서 원격저장소에 올린 시점에서</p>
</blockquote>
</li>
</ol>
<p><strong>p1이 A파일을 변경한 상태로 원격저장소에 있는 것을 merge하면?</strong></p>
<p>-&gt; <strong>에러가 뜬다.</strong></p>
<p>그 이유는 원격저장소에는 지금 p2가 B파일을 업데이트해 B' 파일이 저장돼 있다.</p>
<blockquote>
<p><strong>현재 상태</strong>
원격 저장소 - A, B'
p1가 갖고 있는 소스코드 - A', B
p2가 갖고 있는 소스코드 - A, B'</p>
</blockquote>
<p>원격저장소에 있는 A, B' 파일을 merge 하려고 하니 p1이 변경한 A' 파일 때문에 에러가 생기는 것이다.</p>
<blockquote>
<p><strong>에러를 해결하는 방법</strong></p>
</blockquote>
<ol>
<li>fetch로 B'파일을 local Repo로 가져오고, A'파일을 commit하여 local Repo를 A', B'로 만든 뒤 merge를 하면 된다.<ul>
<li>그럼 pull을 하면 되는거 아닌가? -&gt; <strong>그렇지 않다. pull은 자동병합이어서 수동병합으로 각각 해야 한다.</strong> </li>
</ul>
</li>
<li>그러면 p1은 A', B'을 갖게 되므로 push 또한 가능해진다.</li>
</ol>
<hr />
<ol start="2">
<li>conflict가 나는 경우 - 2명의 개발자가 같은 파일을 각각 변경했을 때<blockquote>
<p>p1도 A파일, p2도 A파일을 각각 다르게 작성하고 p2가 먼저 A파일을 원격저장소에 commit -&gt; push 한 시점</p>
</blockquote>
</li>
</ol>
<p><strong>이 때, p1이 merge를 하려고 하면?</strong></p>
<p><strong>-&gt; 이 때는 conflict가 난다.</strong></p>
<p>그 이유는 원격저장소에 이미 p2가 업데이트 한 A' 파일이 저장돼 있다.</p>
<blockquote>
<p>현재 상태
원격 저장소 - A', B
p1가 갖고 있는 소스코드 - A'', B
p2가 갖고 있는 소스코드 - A', B</p>
</blockquote>
<p>이 상태에서 p1이 merge하려고 하면 원격저장소에서는 이미 최신화된 파일을 갖고 있다.
그 때 merge하면 어떤 A 파일이 최신화된 파일인지 헷갈리게 되면서 conflict를 발생시키는 것이다.</p>
<blockquote>
<p><strong>에러를 해결시키는 방법</strong></p>
</blockquote>
<ol>
<li>이미 p2가 선착순으로 올렸기 때문에 어쩔 수 없이 p1은 reset을 통해 p2가 올린 것을 받아온 뒤, 자신이 수정한 코드를 추가 or 업데이트하는 방법이 있다.</li>
<li>p1은 commit 후 push or pull을 하면 에러가 뜨므로, fetch 후 merge 상태에서 뜨는 아래 코드에서 변경하고 싶은 점을 바꿔줘도 된다.<pre><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; head
변경점
=======
변경점
&gt;&gt;&gt;&gt;&gt;&gt;&gt; origin</code></pre>2-1. 위를 수정하면 push 또한 가능해진다.</li>
</ol>
<hr />
<h2 id="git이란">Git이란</h2>
<blockquote>
<p>분산/버전 관리 시스템 -&gt; 소스 코드의 변경점을 추적하고 여러 사용자 간 작업 조율에 사용됨</p>
</blockquote>
<hr />
<h2 id="git의-기능">Git의 기능</h2>
<ol>
<li><strong>버전관리</strong> : 파일의 변경점을 기록하고, 이전 버전으로도 돌아갈 수 있다.</li>
<li><strong>Branch</strong> : main, develop, feature 등 기능마다 분리해 관리할 수 있다. 기능을 나누기 때문에 여러 기능을 동시에 개발하고 통합할 수 있다.</li>
<li><strong>협업</strong> : 여러 개발자가 같은 프로젝트를 작업할 수 있다.</li>
</ol>