<h1 id="rest-api">Rest API</h1>
<blockquote>
<p>REST : Representational State Transfer의 약자로 소프트웨어 프로그램 아키텍처
의 한 형식이다.</p>
</blockquote>
<ul>
<li><p>자원의 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.</p>
</li>
<li><p>기본적으로 웹의 기존 기술과 HTTP 프로토콜에 한에서 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일이다.</p>
</li>
</ul>
<p>좀 더 자세하게 알아보자면</p>
<ul>
<li>웹의 모든 자원에 고유한 <code>ID</code> 인 HTTP <code>URL</code> 을 부여하고, HTTP Method (GET, POST, PUT, DELETE)를 통해 해당 자원에 대한 CRUD 연산을 적용한다. </li>
</ul>
<blockquote>
<p>즉, REST는 <strong>자원 기반 구조</strong> (ROA : Resource Oriented Architecture) 설계의 중심에 Resource가있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍처를 의미한다.</p>
</blockquote>
<p>프론트가 없을 때, </p>
<hr />
<h2 id="구성">구성</h2>
<ol>
<li>자원(Resource) : <strong>URL</strong><ul>
<li>모든 자원에 고유한 ID 가 존재하고, 이 자원은 Server 에 존재한다.</li>
<li>자원을 구분하는 ID 는 <code>/orders/order-id/1</code> 과 같은 URL이다.</li>
</ul>
</li>
</ol>
<blockquote>
<p><strong>자원을 식별할 수 있어야 한다.</strong></p>
<p>URL (Uniform Resource Locator) 만으로 내가 어떤 자원을 제어하려고 하는지 알 수 있어야
한다.
Server가 제공하는 정보는 JSON 이나 XML 형태로 HTTP body에 포함되어 전송 시킨다.</p>
</blockquote>
<ol start="2">
<li>행위(Verv) : <strong>HTTP Method</strong></li>
</ol>
<table>
<thead>
<tr>
<th><strong>Method</strong></th>
<th><strong>역할</strong></th>
</tr>
</thead>
<tbody><tr>
<td>GET</td>
<td>해당 리소스를 조회한다.</td>
</tr>
<tr>
<td>POST</td>
<td>해당 URI를 요청하면 리소스를 생성한다.</td>
</tr>
<tr>
<td>PUT</td>
<td>해당 리소스를 수정한다.</td>
</tr>
<tr>
<td>DELETE</td>
<td>해당 리소스를 삭제한다.</td>
</tr>
</tbody></table>
<blockquote>
<p><strong>행위는 명시적이어야 한다.</strong></p>
<p>REST는 아키텍쳐 혹은 방법론 과 비슷하다. 따라서 이런 방식을 사용해야 한다고 강제적이지
않다. 기존의 웹 서비스 처럼, GET을 이용해서 UPDATE와 DELETE를 해도 된다.
다만 REST 아키텍쳐에는 부합하지 않으므로 REST를 따른다고 할 수는 없다.</p>
</blockquote>
<ol start="3">
<li>표현(Representations)<ul>
<li>REST에서 하나의 자원은 JSON, XML, TEXT, RSS등 여러 형태의 Representation으로 나타낼 수 있다.</li>
<li>최근에는 JSON 으로 주고 받는 것이 대부분이다.</li>
</ul>
</li>
</ol>
<blockquote>
<p><strong>자기 서술적이어야 한다.</strong></p>
<p>데이터에 대한 메타정보만 가지고도 어떤 종류의 데이터인지, 데이터를 위해서 어떤 어플리케
이션을 실행 해야 하는지를 알 수 있어야 한다.
즉, 데이터 처리를 위한 정보를 얻기 위해서, 데이터 원본을 읽어야 한다면 자기 서술적이지 못
하다.</p>
</blockquote>
<hr />
<h2 id="특징">특징</h2>
<ol>
<li>클라이언트 / 서버 구조</li>
</ol>
<p>클라이언트와 서버의 역할이 명확하게 구분된다.</p>
<table>
<thead>
<tr>
<th>구분</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>client</td>
<td>사용자 인증이나 로그인 정보 등을 직접 관리하고 책임진다.</td>
</tr>
<tr>
<td>server</td>
<td>API를 제공하고 비지니스 로직 처리 및 저장을 책임진다.</td>
</tr>
</tbody></table>
<ol start="2">
<li>무상태성 (Stateless)</li>
</ol>
<ul>
<li>REST는 HTTP의 특성을 이용하기 때문에 무상태성 을 가진다.</li>
<li>서버에서는 상태정보를 기억할 필요가 없고 들어온 요청에 대해 처리만 해주면 되기 때문에 구현이 단순해진다.</li>
</ul>
<ol start="3">
<li>캐시 처리 가능 (Cacheable)</li>
</ol>
<ul>
<li>HTTP의 명세를 따르는 REST의 특징 덕분에 캐시 사용이 가능하다.</li>
<li>캐시 사용을 통해 응답 시간이 빨라지고 REST Server 트랜젝션이 발생하지 않기 때문에 전체 응답 시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.</li>
</ul>
<ol start="4">
<li>자체 표현 구조 (Self - Descriptiveness)</li>
</ol>
<ul>
<li>REST API의 메시지만으로 그 요청이 어떤 행위를 하는지 손쉽게 이해할 수 있다.</li>
</ul>
<ol start="5">
<li>계층화 (Layered Systgem)</li>
</ol>
<ul>
<li>클라이언트와 서버가 분리되어 있기 때문에 중간에 프록시 서버, 암호화 계층 등 중간 매체를 사용할 수 있어서 자유도가 높다.</li>
</ul>
<ol start="6">
<li>유니폼 인터페이스 (Uniform Interface)</li>
</ol>
<ul>
<li>HTTP 표준만 따르면 모든 플랫폼에서 사용이 가능하며, URL로 지정한 리소스에 대한 조작이
가능하게 하는 아키텍처 스타일을 말한다.</li>
<li>즉, 특정 언어나 기술에 종속되지 않는다.</li>
</ul>
<hr />
<h2 id="설계-규칙">설계 규칙</h2>
<h3 id="중심-규칙">중심 규칙</h3>
<ul>
<li>URI는 정보의 자원을 표현해야 한다.</li>
<li>자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE 등)으로 표현한다.</li>
</ul>
<hr />
<h3 id="세부-규칙">세부 규칙</h3>
<ol>
<li>슬래시 구분자 ( / )는 계층 관계를 나타내는데 사용한다.</li>
<li>URI 마지막 문자로 슬래시 ( / )를 포함하지 않는다.<ul>
<li>URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은
리소스가 다르다는 것을 의미한다.</li>
</ul>
</li>
<li>하이픈 ( - )은 URI 가독성을 높이는데 사용한다. 하지만 같은 목적으로 밑줄 ( _ )은 URI에 사용하지 않는다.</li>
<li>URI 경로에는 소문자가 적합하다. 가급적이면 URI 경로에 대문자 사용은 피하도록 한다.</li>
<li>파일확장자는 URI에 포함하지 않는다.<ul>
<li>REST API 에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다. (Accept Header 사용)</li>
<li>ex) <code>GET: http://restapi.exam.com/orders/2</code>
{Accept: image/jpg}</li>
</ul>
</li>
<li>리소스 간에 연관 관계가 있는 경우<ul>
<li>/리소스명/리소스ID/관계가 있는 다른 리소스 명</li>
<li>ex) <code>GET: /users/2/orders</code> (일반적으로 소유의 관계를 표현할 때 사용)</li>
</ul>
</li>
</ol>
<hr />
<h2 id="추가-개념">추가 개념</h2>
<p><strong>HATEOAS</strong> (Hypermedia as the Engine of Application State)</p>
<ul>
<li>클라이언트 요청에 대해 <strong>응답을 할 때, 추가적인 정보를 제공하는 링크</strong>를 포함할 수 있어야 한다.</li>
<li>REST는 <strong>독립적</strong>으로 컴포넌트들을 손쉽게 연결하기 위한 목적으로도 사용된다. 따라서 서로
다른 컴포넌트들을 유연하게 연결하기 위해선, <strong>느슨한 연결</strong>을 만들어줄 것이 필요하다.</li>
<li>이때 사용되는 것이 바로 링크 이다.</li>
<li><code>HATEOAS</code> 는 서버가 독립적으로 진화할 수 있도록 서버와 서버, 서버와 클라이언트를 분리 할
수 있게 한다.</li>
</ul>
<hr />
<h2 id="rest의-단점">REST의 단점</h2>
<ul>
<li><p>REST는 <code>point-to-point</code> 통신모델을 기본으로 한다. 따라서 서버와 클라이언트가 연결을 맺고 상호작용해야하는 어플리케이션의 개발에는 적당하지 않다.</p>
</li>
<li><p>REST는 URI, HTTP 이용한 아키텍처링 방법에 대한 내용만을 담고 있다. <strong>보안과 통신규약 정책</strong> 같은 것은 전혀 다루지 않는다. </p>
<ul>
<li>따라서 개발자는 통신과 정책에 대한 설계와 구현을 도맡아서 진행해야 한다.</li>
</ul>
</li>
<li><p>HTTP에 상당히 의존적 이다. REST는 설계 원리이기 때문에 HTTP와는 상관없이 다른 프로토콜에서도 구현할 수 있기는 하지만 자연스러운 개발이 힘들다.</p>
<ul>
<li>다만 REST를 사용하는 이유가 대부분의 서비스가 웹으로 통합되는 상황이기에 큰 단점이 아니게 되었다.</li>
</ul>
</li>
<li><p>CRUD 4가지 메소드만 제공한다. </p>
<ul>
<li>대부분의 일들을 처리할 수 있지만, 4가지 메소드 만으로 처리하기엔 모호한 표현 이 있다.</li>
</ul>
</li>
</ul>
<p><a href="https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80">출처 : https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80</a></p>