<p>로그인은 세션기반과 토큰기반의 인증방식이 있다.</p>
<h1 id="세션기반-인증방식">세션기반 인증방식</h1>
<p>HTTP의 특징 중 하나는 상태없음(stateless) 하다라는 것인데</p>
<p>즉, HTTP 요청을 통해 데이터를 주고 받을 때 요청이 끝나면 요청한 사용자의 정보 등을 유지하지 않는 특징이 있다. (이전부터 CS 공부하면서 아는 내용)</p>
<p><strong>그럼 로그인 상태는 어떻게 유지하는걸까?</strong></p>
<ul>
<li><strong>세션</strong> : 서버와 클라이언트의 연결이 활성화된 상태를 의미합니다.</li>
<li><strong>세션ID</strong> : 웹 서버 또는 DB에 저장되는 클라이언트에 대한 유니크한 ID</li>
</ul>
<h2 id="세션기반-로그인-과정">세션기반 로그인 과정</h2>
<ol>
<li>처음 로그인 &gt;&gt; 세션ID가 생성됨 &gt;&gt; 서버에서 세션ID를 쿠키로 설정해서 클라이언트에 전달</li>
<li>클라이언트가 서버에 요청을 보낼 때 해당 세션ID를 쿠키로 담아서 전에 로그인했던 아이디인지 확인</li>
<li>로그인을 유지</li>
</ol>
<p>다른 페이지를 넘어가더라도 쿠키에 있는 세션ID를 보고 해당 ID가 이미 유효하기 때문에 로그인 상태가 유지되는 것이다.</p>
<h3 id="단점">단점</h3>
<ol>
<li>사용자의 상태에 관한 데이터를 서버에 저장했을 때 로그인 중인 유저의 수가 늘어난다면?<ul>
<li>서버의 메모리 과부화가 일어날 수 있다.</li>
</ul>
</li>
<li>DB 중 RDBMS에 저장한다면, <strong>직렬화 및 역직렬화</strong>에 관한 오버헤드가 발생</li>
</ol>
<p>MySQL 같은 곳에 저장한다면 (아마 VARCHAR 형태로 저장될 것)</p>
<p>이 때 직렬화와 역직렬화에 대한 단점을 굳이 꼽아볼 수 있다.</p>
<hr />
<h1 id="토큰기반-인증방식">토큰기반 인증방식</h1>
<p>acceess 토큰, refresh 토큰, JWT 토큰에 대해서는 들어봤을 것이다.</p>
<p>토큰기반 인증방식에 대해 알아보자.</p>
<p>토큰기반 인증방식은 <code>state</code>를 모두 토큰 자체만으로 처리하며 토큰을 처리하는 한 서버를 두고 다른 컨텐츠를 제공하는 서버는 모두 <code>stateless</code>하게 만들자는 이론이 담긴 방식이다.</p>
<p><strong>왜</strong> 토큰을 관리하는 서버는 따로 두어야 할까?</p>
<p>이는 여러 개의 서버를 운용한다라고 했을 때 </p>
<ul>
<li>토큰기반 인증 + A도메인을 처리하는 서버로
구축할 경우</li>
</ul>
<p>A도메인에서 에러가 발생이 되면, 인증에 관한 기능이 마비되고 이는 B, C, D 등의 도메인 기능이 연쇄적으로 마비될 수 있기 때문이다.</p>
<p>토큰은 주로 JWT 토큰이 활용된다.</p>
<ol>
<li>인증로직 &gt;&gt; JWT 토큰 생성(<code>access 토큰</code>, <code>refresh 토큰</code>)</li>
<li>사용자가 이후에 <code>access 토큰</code>을 <code>HTTP Header - Authorization</code> 또는 <code>HTTP Header</code></li>
</ol>
<ul>
<li><code>Cookie</code>에 담아 인증이 필요한 서버에 요청해 원하는 컨텐츠를 가져온다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/72e05fb9-6725-4d2b-a1b5-a9b41bb41b1e/image.png" /></p>
<hr />
<h2 id="jwt">JWT</h2>
<p>JWT는 JSON Web Token을 의미하며 헤더, 페이로드, 서명으로 이루어져 있으며 JSON 객체로 인코딩되며 메시지 인증, 암호화에 사용됩니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4d1bb276-fc2a-4987-96d5-658b6ce55d51/image.png" /></p>
<p><strong>Header</strong></p>
<ul>
<li>토큰 유형과 서명 알고리즘, base64URI로 인코딩 됩니다.</li>
</ul>
<p><strong>Payload</strong></p>
<ul>
<li><strong>데이터</strong>, 토큰 발급자, 토큰 유효기간, base64URI로 인코딩 됩니다.</li>
</ul>
<p><strong>Signature</strong></p>
<ul>
<li>(인코딩된 header + payload) + 비밀키를 기반으로 헤더에 명시된 알고리즘으로 다시
생성한 서명값.</li>
</ul>
<p><a href="https://jwt.io/">https://jwt.io/</a></p>
<p>여기로 가서 보면 Header랑 Payload 를 base64UrlEncode 하고 <img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/87000570-ed5d-4fb8-b4e4-3f8ee60b7b38/image.png" /></p>
<p>비밀키 기반으로 서명 알고리즘을 통해 만들어진다. 그것을 왼쪽에서 확인 가능할 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/52d42b06-c07c-4624-8973-27bbe81b9a44/image.png" /></p>
<h3 id="장점">장점</h3>
<ol>
<li>사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증저장소가 필요 X</li>
<li>다른 유형의 토큰과 비교했을 때 경량화되어있습니다. SAML(Security Assertion Markup Language Tokens)이란 토큰이 있지만 이에 비해 훨씬 경량화되어 있음.</li>
<li>디코딩했을 때 JSON이 나오기 때문에 JSON을 기반으로 쉽게 직렬화, 역직렬화가
가능하다.</li>
</ol>
<h3 id="단점-1">단점</h3>
<ol>
<li>토큰이 비대해질 경우 당연히 서버과부화에 영향을 줄 수 있다.</li>
<li>토큰을 탈취당할 경우 디코딩했을 때 데이터를 볼 수 있다.</li>
</ol>
<hr />
<h2 id="access-토큰-refresh-토큰">Access 토큰, Refresh 토큰</h2>
<p><code>access토큰</code>의 수명은 짧게, <code>refresh토큰</code>의 수명은 길게 한다.</p>
<p><code>refresh</code>토큰은 <code>access토큰</code>이 만료되었을 때 다시 <code>access 토큰</code>을 얻기 위해 사용되는
토큰이다.</p>
<p>이를 통해 <code>access토큰</code>이 만료될 때마다 인증에 관한 비용이 줄어들게 된다.</p>
<hr />
<p>로그인을 하게 되면 access 토큰과 refresh 토큰 두개를 얻는데</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/86787d50-4a42-4511-bee4-d53fab34923c/image.png" /></p>
<p>access 토큰이 만료되거나 사용자가 새로고침을 할 때 refresh 토큰을 기반으로 새로운 access token을 얻는다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b7e8df88-582d-421c-890b-37a699727f36/image.png" /></p>
<p>왜 나눠서 할까?</p>
<p><code>access 토큰</code> : 인증용 토큰이다.</p>
<p>해커가 access 토큰을 탈취해 로그인한다면? -&gt; 안 되기 때문에 만료기한을 적게 해야 한다.</p>
<p>만료기한이 지나면 이후에는 사용 못하기 때문에 해커도 탈취해서 사용하기 힘들다.</p>
<p><code>refresh 토큰</code> : 갱신용 토큰</p>
<p>access 토큰이 짧은 만큼 refresh 토큰은 갱신 기간을 둬서 갱신 가능하게끔 해주는 것이다.</p>
<h3 id="주의할-점">주의할 점</h3>
<p>이렇게 access토큰을 얻었다면 그 이후에 요청을 할 때는 HTTP Header - Authorization 또는 HTTP Header - Cookie에 담아 요청을 하게 되는데 이 때 다음과 같은 규칙을
지키는 것이 좋다.</p>
<ul>
<li><code>Bearer &lt;token&gt;</code> 으로 <code>&quot;Bearer &quot; (띄어쓰기 포함)</code> 을 앞에 둬서 토큰기반인증방식이라는 것을 알려줘야 한다.</li>
<li><code>https</code>를 사용해야 한다.</li>
<li>쿠키에 저장한다면 <code>sameSite: 'Strict'</code>을 써야 한다.</li>
<li>수명이 짧은 <code>access token</code>을 발급해야 한다.</li>
<li>url에 토큰을 전달하지 말아야 한다.</li>
</ul>
<hr />
<h3 id="토큰을-탈취당하는-것을-대비하는-방법">토큰을 탈취당하는 것을 대비하는 방법</h3>
<ol>
<li>먼저 Access Token의 수명을 짧게 설정하여 탈취된 토큰의 유효 기간을 최소화. 짧은 수명의 Access Token을 사용하고, 필요할 때만 Refresh Token을 통해 새로운 Access Token을 발급</li>
<li>Refresh Token을 사용하여 민감한 작업을 수행하려고 할 때 추가적인 사용자 인증
단계를 요구한다.</li>
</ol>
<ul>
<li>예를 들어, IP주소, 디바이스 정보등을 이용하거나 google authentifactor를 이용한 2단계 인증을 사용한다.</li>
</ul>
<ol start="3">
<li>쿠키에 HttpOnly 및 Secure 을 걸어서 관리</li>
</ol>