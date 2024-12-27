<h1 id="프록시-패턴">프록시 패턴</h1>
<blockquote>
<p>객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴</p>
</blockquote>
<p> 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용한다.</p>
<p> 이 패턴은 프록시 객체, 프록시 서버로 활용된다.</p>
<p> <strong>프록시 서버에서의 캐싱</strong>
캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 서버에 요청하지 않고 캐시 안에 있는 데이터를 활용하는 것
-&gt; 이는 곧 트래픽을 줄일 수 있다는 장점으로 이어진다.</p>
<hr />
<h2 id="프록시-서버">프록시 서버</h2>
<blockquote>
<p>서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램</p>
</blockquote>
<h3 id="nginx">nginx</h3>
<p>첫 번째 예시로는 nginx 가 있다.
<code>nginx</code> : 비동기 이벤트 기반의 구조와 다수의 연결을 처리 가능한 웹 서버</p>
<p>주로 Node.js 서버의 앞단 프록시 서버로 활용된다. -&gt; 버퍼 오버플로우 취약점 예방을 위해</p>
<p>익명 사용자의 직접적인 서버로의 접근을 차단하고 간접적으로 한 단계를 더 거침으로써 보안성을 더욱 강화할 수 있어서 사용한다.</p>
<ul>
<li>프록시 서버를 nginx로 둬서 실제 포트를 숨기기</li>
<li>정적 자원을 gzip 압축하기</li>
<li>메인 서버 앞단에서의 로깅</li>
</ul>
<p>위와 같은 방식들이 있다.</p>
<blockquote>
<p>버퍼 오버플로우 : <code>버퍼</code>는 보통 데이터가 저장되는 메모리 공간으로 이 공간을 벗어나는 경우를 뜻하는데 사용되지 않아야 할 영역에 데이터가 덮어씌워져 주소, 값을 바꾸는 공격이 발생하게 된다.</p>
</blockquote>
<hr />
<h3 id="cloudflare">CloudFlare</h3>
<p>전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스으로 추가적인 장점들도 있다.</p>
<blockquote>
<p>CDN(Content Delivery Network)
각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크
사용자가 웹 서버로부터 콘텐츠를 다운로드하는 시간을 줄일 수 있다.</p>
</blockquote>
<ul>
<li>HTTPS 구축</li>
<li>DDOS 방어</li>
</ul>
<p>사용자, 크롤러, 공격자가 웹 사이트에 접속하게 됐을 때 CloudFlare를 통해 공격자로부터 보호가 가능하다.</p>
<h4 id="https-구축">HTTPS 구축</h4>
<p>서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축 가능한데
CloudFlare를 사용하면 인증서 설치 없이 좀 더 쉽게 가능하다.</p>
<h4 id="ddos-공격-방어">DDOS 공격 방어</h4>
<p>DDOS : 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형</p>
<p>CloudFlare는 의심스러운 트래픽, 특히 사용자가 접속하는 것이 아닌 시스템을 통해 오는 트래픽을 자동으로 차단해서 DDOS 공격으로부터 보호한다.</p>
<hr />
<h3 id="cors---프론트엔드의-프록시-서버">CORS - 프론트엔드의 프록시 서버</h3>
<blockquote>
<p>CORS(Cross-Origin Resource Sharing) :
서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘</p>
<ul>
<li>오리진 : 프로토콜과 호스트 이름, 포트의 조합</li>
</ul>
</blockquote>
<p>프론트엔드의 오리진과 백엔드 서버의 오리진이 다르면 (거의 포트번호가 다른 경우로 받아들이면 된다.) CORS 에러가 발생한다.</p>
<p>이 때는 프론트엔드 서버 앞단에 프록시 서버를 둬서 문제를 해결할 수 있다.</p>
<p>요청이 들어왔을 때 해당 API 서버 통신을 매끄럽게 해주기 때문이다.</p>
<hr />
<h1 id="이터레이터-패턴">이터레이터 패턴</h1>
<blockquote>
<p>이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴</p>
</blockquote>
<p>순회할 수 있는 여러 가지 자료형의 구조와 상관없이 (= 다른 객체여도) 이터레이터라는 하나의 인터페이스로 순회가 가능하다.</p>
<pre><code class="language-javascript">const map = new Map()
map.set('a', 1)
map.set('b', 2)
map.set('c', 3)

const set = new Set()
set.add(1)
set.add(2)
set.add(3)

for (let a of map) console.log(a)
for (let a of set) console.log(a)

/*
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/</code></pre>
<p>다른 자료 구조인 <code>set</code>과 <code>map</code>임에도 똑같은 <code>for a of b</code>라는 이터레이터 프로토콜을 통해 순회한다.</p>
<blockquote>
<p>이터레이터 프로토콜 : iterable 한 객체들을 순회할 때 쓰이는 규칙</p>
</blockquote>
<blockquote>
<p>itreable 한 객체 : 반복 가능한 객체로, 배열을 일반화한 객체를 뜻한다.</p>
</blockquote>
<hr />
<h1 id="노출모듈-패턴">노출모듈 패턴</h1>
<blockquote>
<p>즉시 실행 함수를 통해 <code>private</code>, <code>public</code> 같은 접근 제어자를 만드는 패턴</p>
</blockquote>
<p>JavaScript는 private나 public 같은 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행된다. </p>
<p>그래서 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 한다.</p>
<pre><code class="language-javascript">const revealing = (() =&gt; {
    const a = 1
    const b = () =&gt; 2
    const public = {
        c : 2, 
        d : () =&gt; 3
    }
    return public
})()
console.log(revealing)
console.log(revealing.a)
// { c: 2, d: [Function: d] }
// undefined</code></pre>
<p>a와 b는 다른 모듈에서 사용할 수 있는 변수나 함수인 private 범위여서 다른 모듈에서 접근할 수 없다.</p>
<p>c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수인 public 범위여서 접근 가능하다.</p>
<blockquote>
<p>즉시 실행 함수 :
함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용한다.</p>
</blockquote>
<hr />
<h1 id="mvc-패턴">MVC 패턴</h1>
<p><a href="https://velog.io/@jojehuni_9759/Spring-Spring-MVC-%ED%8C%A8%ED%84%B4%EC%9D%98-%EC%9A%94%EC%B2%AD-%EC%B2%98%EB%A6%AC-%EA%B3%BC%EC%A0%95">Spring MVC 패턴의 요청 처리 과정</a></p>
<p>이전에 MVC 패턴에 배우면서 간단하게 처리 과정에 대해서 정리한 적이 있어 그 글도 함께 보면 좋을 것 같다.</p>
<blockquote>
<p>MVC 패턴 : 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴</p>
</blockquote>
<p>애플리케이션의 구성 요소를 세 가지 역할로 구분해 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있다.</p>
<ul>
<li>재사용성과 확장성이 용이하다는 <strong>장점</strong></li>
<li>애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 <strong>단점</strong></li>
</ul>
<h2 id="용어">용어</h2>
<ol>
<li>Model : 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻한다.</li>
</ol>
<p>사각형 모양의 박스 안에 글자가 들어 있다면 그 사각형 모양의 박스 위치 정보, 글자 내용, 글자 위치, 글자 포맷(utf-8 등)에 관한 정보를 다 가지고 있어야 한다.</p>
<p>View 에서 데이터를 생성 or 수정하면 Controller 를 통해 Model을 생성하거나 갱신한다.</p>
<ol start="2">
<li>View : inputbox, checkbox, textarea 등 사용자 인터페이스 요소로 Model을 기반으로 사용자가 볼 수 있는 화면을 뜻한다.</li>
</ol>
<p>화면에 표시하는 정보만 가지고 있어야 하고, 변경이 생기면 Controller에 전달해줘야 한다.</p>
<ol start="3">
<li>Controller : 하나 이상의 Model과 View를 잇는 다리 역할. 이벤트와 같은 메인 로직을 담당한다.</li>
</ol>
<p>Model과 View의 생명주기 관리도 하고 변경이 되는 경우에 각각의 구성 요소에 해석한 내용을 알려준다.</p>
<hr />
<h1 id="mvp-패턴">MVP 패턴</h1>
<blockquote>
<p>MVC 패턴으로부터 파생된 패턴으로, Controller -&gt; Presenter로 교체된 패턴이다.</p>
<p>View 와 Presenter는 1:1 관계로 MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴이다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e7ce270d-aa7b-40d7-afa3-b2acd2f38a8d/image.png" /></p>
<hr />
<h1 id="mvvm-패턴">MVVM 패턴</h1>
<blockquote>
<p> MVC의 C에 해당하는 컨트롤러가 뷰모델(view model)로 바뀐 패턴</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8e53d228-c5d1-4b2f-9384-320304f2b77b/image.png" /></p>
<p>View Model은 View를 더 추상화한 계층이며 이 패턴은 커맨드와 데이터 바인딩을 가지는 것이 특징이다.</p>
<p>뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원하며 UI를 별도의 코드 수정 없이 재사용할 수 있고 단위 테스팅하기 쉽다는 장점이 있다.</p>
<p>예시로는 Vue.js가 있다.</p>
<blockquote>
<p>커맨드 :
여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법이다.</p>
</blockquote>
<blockquote>
<p>데이터 바인딩 :
화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법으로, 뷰모델을 변경하면 뷰가 변경된다.</p>
</blockquote>
<h2 id="vuejs">Vue.js</h2>
<p>Vue.js는 반응형이 특징인 프런트엔드 프레임워크로, <code>watch</code>와 <code>computed</code> 등으로 쉽게 반응형적인 값들을 구축할 수 있다.</p>
<p>함수를 사용하지 않고 값 대입만으로도 변수가 변경되며 <code>양방향 바인딩</code>, <code>html</code>을 토대로 컴포넌트를 구축할 수 있다는 점이 특징이다.</p>
<p>재사용 가능한 컴포넌트도 정의할 수 있다. 이것을 기반으로 UI를 구축할 수 있다.</p>