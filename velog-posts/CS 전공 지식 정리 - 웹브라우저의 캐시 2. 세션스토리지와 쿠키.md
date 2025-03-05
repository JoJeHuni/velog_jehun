<h1 id="세션-스토리지">세션 스토리지</h1>
<p>세션 스토리지는 로컬 스토리지와 굉장히 유사하다.</p>
<blockquote>
<p>웹 스토리지 객체로 브라우저 내에 { key : value } 형태로 오리진에
종속되어 저장되는 데이터</p>
</blockquote>
<ul>
<li>하나의 키, 하나의 값 저장</li>
<li>최대 저장 용량은 5MB</li>
</ul>
<h2 id="로컬스토리지와-차이점">로컬스토리지와 차이점</h2>
<ul>
<li>동일한 오리진이어도 브라우저의 각 탭마다 독립적으로 저장된다.</li>
</ul>
<blockquote>
<p>즉, 다른 탭에서 세션 스토리지에 저장된 데이터에 접근할 수 없다.</p>
</blockquote>
<ul>
<li>사용자가 브라우저에서 탭을 닫으면 데이터가 만료된다.</li>
</ul>
<hr />
<h2 id="사용법">사용법</h2>
<ul>
<li>설정 : sessionStorage.setItem(key, value);</li>
<li>탐색 : sessionStorage.getItem(key);</li>
<li>제거 : sessionStorage.removeItem(key);</li>
<li>전체제거 : sessionStorage.clear()</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/71b75328-ae54-4a74-bfaa-2eefa805dfd7/image.png" /></p>
<p>위와 같이 Console 창에 했을 때</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8a9a358b-1957-4a6d-9855-c4956abbdbd8/image.png" /></p>
<p>해당 탭의 Session storage 에 저장된 것을 볼 수 있지만</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2a045f38-7490-40f1-b0ab-659985a66877/image.png" /></p>
<p>다른 탭에서는 없는걸 볼 수 있다.</p>
<p>그런 차이가 있다.</p>
<blockquote>
<p>보통 세션스토리지보다 로컬스토리지를 많이 쓴다.</p>
</blockquote>
<hr />
<h1 id="쿠키">쿠키</h1>
<blockquote>
<p>브라우저에 저장된 데이터 조각</p>
</blockquote>
<p>클라이언트에서 먼저 설정할 수도 있고, 서버에서 먼저 설정할 수도 있다.</p>
<p>거의 보통은 서버에서 먼저 설정해서 쿠키를 만드는게 일반적이다.</p>
<ol>
<li>서버에서 응답헤더로 <code>Set-Cookie</code> 로 설정해서 쿠키를 보낸다.</li>
<li>클라이언트에서 요청헤더 <code>Cookie</code> 에 설정돼 자동으로 서버에 전달되게 돼 브라우저에 저장되게 한다.</li>
</ol>
<blockquote>
<p>참고 : HTTP 헤더를 통해 Client or Server 가 HTTP 요청, 응답할 때 추가 정보를 전달할 수 있다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e2eecb8f-3daa-4ba9-a575-44bbc29c7c7b/image.png" /></p>
<p>클라이언트와 서버 둘 다 조작이 가능하지만 <strong>보통 서버에서 만료기한</strong> 등을 설정 및
컨트롤한다.</p>
<p>저장용량은 <strong>최대 4KB</strong></p>
<ul>
<li>로그인</li>
<li>장바구니</li>
<li>사용자 커스터마이징</li>
<li>사용자 행동분석 (주로 개인화된 광고에 활용되는 것)</li>
</ul>
<p>위와 같은 것들에 사용된다.</p>
<h2 id="클라이언트에서도-설정가능한-쿠키-권장-x">클라이언트에서도 설정가능한 쿠키 (권장 X)</h2>
<p>클라이언트에서 자바스크립트 - <code>document.cookie</code>를 통해 쿠키를 설정할 수 있고 보낼 때도 이런 식으로 <code>header</code> - <code>Cookie</code>에 값을 정해서 보낼 수도 있다. </p>
<p>하지만 이를 권장 X</p>
<pre><code class="language-javascript">axios.get(url, {
    headers: {
        Cookie: &quot;cookie1=value; cookie2=value; cookie3=value;&quot;
    }
}).then</code></pre>
<p>이렇게 되면 쿠키에 대한 제어권을 클라이언트에게 두게 되는데, 쿠키에는 보통 민감한 정보들이 담길 수도 있기 때문에 이 제어권에 관한 것을 <strong>클라이언트가 아닌 서버</strong>가 가지고 있게 해야 한다.</p>
<hr />
<h2 id="세션-쿠키">세션 쿠키</h2>
<p><code>Expires</code> 또는 <code>Max-Age</code> 속성을 지정하지 않은 것을 말한다.</p>
<p>브라우저가 종료되면 쿠키도 사라진다.</p>
<hr />
<h2 id="영구-쿠키">영구 쿠키</h2>
<p><code>Expires</code> 또는 <code>Max-Age</code> 속성을 지정해서 특정 날짜 또는 일정 기간이 지나면 삭제되게 만든 쿠키로, 브라우저를 닫을 때 만료되지 않는다.</p>
<hr />
<h2 id="문법">문법</h2>
<pre><code>Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; Expires=&lt;date&gt;
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; Max-Age=&lt;non-zero-digit&gt;
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; Domain=&lt;domain-value&gt;
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; Path=&lt;path-value&gt;
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; Secure
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; HttpOnly
Set-Cookie: &lt;cookie-name&gt;=&lt;cookie-value&gt;; SameSite=Strict</code></pre><h3 id="secure">secure</h3>
<p>쿠키에 이 옵션을 추가하면 https로만 쿠키를 주고받을 수 있게 하는 옵션이다.</p>
<p>하지만 Chrome v89 및 Firefox v75이상부터 localhost에서는 이 사양을 무시합니다.</p>
<blockquote>
<p>즉, Chrome v89 및 Firefox v75이상의 <code>localhost</code>에서는 <code>secure</code> 옵션을 걸어도 <code>http</code>로 쿠키를 주고받을 수 있어 쉽게 테스팅이 가능</p>
</blockquote>
<h3 id="httponly">httponly</h3>
<p>공격자가 쿠키를 자바스크립트로 빼낼 수 없게 만든다. (document.cookie로 접근 불가)</p>
<h3 id="samesite">samesite</h3>
<p>요청이 동일한 도메인에서 시작된 경우에만 쿠키가 애플리케이션으로 전송되도록 허용</p>
<h3 id="실습">실습</h3>
<pre><code>const http = require('http');
const hostname = '127.0.0.1';
const port = 3000;
const server = http.createServer((req, res) =&gt; {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8');
    res.setHeader('Set-Cookie', ['cookie1 = httponlycookie; httponly', 'cookie2 = securecookie; Secure']);
    res.end('end\n');
});
server.listen(port, hostname, () =&gt; {
    console.log(`Server running at http://${hostname}:${port}/`);
});</code></pre><p>http 서버, 호스트명은 localhost, 포트는 3000</p>
<p><code>res.setHeader('Content-Type', 'text/plain; charset=utf-8');</code> 라고 하는 형태로 보내지만</p>
<p>쿠키는
<code>res.setHeader('Set-Cookie', ['cookie1 = httponlycookie; httponly', 'cookie2 = securecookie; Secure']);</code>
이렇게 만든다.</p>
<p>js 파일에서 만든 뒤 <code>node script.js</code> 로 실행하면 3000번 포트에 서버가 구동된걸 볼 수 있고,</p>
<p>개발자 도구의 Application -&gt; Cookies 를 보면 들어간 것도 볼 수 있을 것이다.</p>
<ol>
<li>httponly와 secure 에도 차이가 있는걸 볼 수 있을 것이다.</li>
</ol>
<blockquote>
<p>내가 https가 아닌 http인데 secure가 보이는 이유? -&gt; 크롬 89 이상과 firefox 75 이상이기 때문일 것이다.</p>
</blockquote>
<ul>
<li>테스트할 때만 로컬에서 볼 수 있게 한시적으로 허용해준다.</li>
</ul>
<ol start="2">
<li><code>document.cookie</code> 를 검색해보면 <code>'cookie2 : securecookie</code> 만 나오는 것을 볼 수 있을 것이다.</li>
</ol>
<p>httponly는 나오지 않기 때문이다. (조금 더 안전해진다.)</p>
<hr />
<p>즉, 쿠키의 시큐어코딩에 대해 알아보자.</p>
<p><strong>쿠키 - 세션으로 로그인을 처리한다면</strong> 다음과 같은 시큐어 코딩을 해야 한다.</p>
<ol>
<li><code>cookie</code>에 <code>세션ID</code>를 담을 때 이 <code>세션ID</code>기반으로 클라이언트의 개인정보를 유추할 수
없게 해야 한다.</li>
<li>자바스크립트로는 파악할 수 없게 <code>http only</code> 옵션을 걸고, <code>https</code>로만 쿠키를 주고받을
수 있게 <code>secure</code> 옵션을 걸어야 한다.</li>
<li>일정시간의 <strong>세션 타임아웃</strong>을 걸어야 한다.</li>
</ol>
<hr />
<h3 id="사례">사례</h3>
<blockquote>
<p><strong>세션 타임아웃 사례</strong> : upbit의 세션타임아웃으로 공격자의 접근을 차단할 수 있다.</p>
</blockquote>
<p>-&gt; 로그인 해두고 뭐 자리를 비웠을 때 타임아웃이 된 후에는 공격자가 접근할 수 없게끔 하는 것이다.</p>
<hr />
<blockquote>
<p><strong>쿠키 허용 관련 알림창 사례</strong></p>
</blockquote>
<p>서비스 운용시 쿠키를 사용한다면 쿠키허용관련 알림창을 만들어야 한다. 
방문 기록을 추적할 때 쿠키가 사용되기 때문이다. 이는 사용자의 데이터 간접수집에 해당하며 거기에 해당하는 KISA 지침을 준수해야 하기 때문.</p>
<hr />
<h1 id="로컬스토리지-세션스토리지-쿠키의-공통점-차이점">로컬스토리지, 세션스토리지, 쿠키의 공통점 차이점</h1>
<p>이전 로컬스토리지에 이어서 봤던 웹브라우저의 캐시들의 공통점, 차이점을 알아보자.</p>
<p><strong>공통점</strong></p>
<ol>
<li>브라우저에 캐싱을 함으로써 <strong>서버에 대한 요청을 줄여 서버부하를 방지</strong>할 수 있다.</li>
<li>캐싱으로 인해 다운로드 하는 컨텐츠가 줄어들어 <strong>웹사이트의 컨텐츠를 더 빨리 다운로드</strong>가 가능합니다.</li>
<li>사이트 <strong>기본 설정 커스터마이징(색상, 글꼴 크기 등)을 저장</strong>하거나 <strong>로그인 상태를 유지</strong>할 때 사용될 수 있습니다</li>
</ol>
<p><strong>차이점</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/59a4f208-33ad-4f64-8bbf-cc989c4cd60c/image.png" /></p>
<p>-&gt; 쿠키, 로컬스토리지는 오리진이 같은 여러 탭을 닫아도 유지가 된다.</p>