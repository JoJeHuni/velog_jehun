<h1 id="http">HTTP</h1>
<p>HTTP는 애플리케이션 계층으로서 웹 서비스 통신에 사용된다.</p>
<p>지금은 3버전이며 1.0버전부터 차례대로 알아보자.</p>
<hr />
<h2 id="http10">HTTP/1.0</h2>
<p>설계할 때부터 <strong>한 연결 당 1개의 요청을 처리</strong>하도록 만들어졌는데</p>
<p>이는 곧 RTT 증가를 불러왔다.</p>
<h3 id="rtt-증가">RTT 증가</h3>
<blockquote>
<p>RTT : 패킷이 목적지에 도달한 뒤 다시 출발지로 돌아올 때까지의 시간
즉, 패킷 왕복 시간</p>
</blockquote>
<p>TCP의 3-way handshake 방식으로는 RTT가 계속 일어나게 된다.</p>
<ol>
<li>클라이언트 - TCP 연결 초기화</li>
<li>클라이언트 - 파일 요청<ul>
<li>서버 - 파일 전송</li>
</ul>
</li>
<li>전체 파일 수신</li>
</ol>
<p>위 3가지 과정 동안 2번의 RTT가 일어나게 된다.</p>
<hr />
<h3 id="rtt-증가를-해결하는-방법">RTT 증가를 해결하는 방법</h3>
<p>HTTP/1.0 때는 매번 연결할 때마다 RTT가 증가하기 때문에</p>
<ul>
<li>서버의 부담</li>
<li>사용자 응답시간 증가</li>
</ul>
<p>위 2가지 단점이 있었기에 해결하기 위해</p>
<ul>
<li>이미지 스플리팅</li>
<li>코드 압축</li>
<li>이미지 Base64 인코딩</li>
</ul>
<p>3가지 방식을 사용하곤 했다.</p>
<hr />
<h4 id="이미지-스플리팅">이미지 스플리팅</h4>
<p>많은 이미지를 다운로드 받는 방식 -&gt; 과부하를 야기</p>
<p>위 문제로 인해 <strong>많은 이미지가 합쳐져 있는 하나의 이미지를 다운로드하는 방식</strong>으로, 이를 통해 <code>background-image</code> 의 <code>position</code>을 이용해 이미지를 표기하는 방법이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6c03baff-1ff2-4cd4-a4d3-b706a6a580dc/image.png" /></p>
<p><strong>장점</strong></p>
<ul>
<li>서버의 요청 수를 줄여 로딩 시간을 단축할 수 있다.</li>
<li>스프라이트 이미지 파일만을 관리하기에 관리에 용이하다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>스프라이트의 이미지에서 사용하려는 이미지의 정확한 position 값을 알아야 한다.</li>
<li>스프라이트 이미지 내에서 특정 이미지를 변경해야 할 때, 단일 파일보다 수정이 번거롭다.</li>
</ul>
<hr />
<h4 id="코드-압축">코드 압축</h4>
<p>코드를 압축해 개행 문자, 빈칸을 없애서 코드의 크기를 최소화하는 방법이다.</p>
<p>가독성을 위해서는 좋은 방식인지 잘 모르겠다.</p>
<hr />
<h4 id="이미지-base64-인코딩">이미지 Base64 인코딩</h4>
<blockquote>
<p>이미지 파일을 64진법으로 이루어진 문자열로 인코딩하는 방법</p>
</blockquote>
<p><strong>장점</strong></p>
<ul>
<li>서버와의 연결을 열고 이미지에 대해 서버에 HTTP 요청을 할 필요 없다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>Base64 문자열로 변환할 경우 37% 정도 크기가 더 커진다..</li>
</ul>
<blockquote>
<p>인코딩 : 정보의 형태나 형식을 표준화, 보안, 처리 속도 향상, 저장 공간 절약을 위해 다른 형태로 변환하는 처리 방식</p>
</blockquote>
<hr />
<h2 id="http11">HTTP/1.1</h2>
<p>1.0에서 발전한 것이 1.1이다.</p>
<p>매번 TCP 연결을 하는 것이 아닌, TCP 초기화 이후 <code>keep-alive</code> 옵션으로 여러 개의 파일을 송수신할 수 있게 바뀌었다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/076340d5-c175-482a-9344-21e153cc87e1/image.png" /></p>
<p>HTTP/1.0과 HTTP/1.1의 비교 사진으로 왼쪽이 1.0, 오른쪽이 1.1이다.</p>
<p>1.1부터는 1번 TCP 3-way handshake가 발생하면 다음부터 발생하지 않는 것을 볼 수 있다.</p>
<p>하지만 이 또한 아래와 같이 단점들이 있다.</p>
<blockquote>
<ul>
<li>다수의 리소스를 처리하려면 요청할 리소스 개수에 비례해 대기 시간이 길어진다.</li>
<li>HOL Blocking </li>
<li>무거운 헤더 구조 : HTTP/1.1의 헤더에는 쿠키 등 많은 메타데이터가 들어 있고 압축이 되지 않아 무겁다.</li>
</ul>
</blockquote>
<hr />
<h3 id="hol-blocking">HOL Blocking</h3>
<blockquote>
<p>HOL Blocking (Head Of Line Blocking) : 네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상</p>
</blockquote>
<p>만약 image.jpg, styles.css, data.xml 이라는 리소스를 다운로드할 때 순차적으로 받아지긴 하지만, <strong>첫 번째 패킷이 다운로드 시간이 길어지게 된다면 다운로드 시간이 지연되는 상태가 되는데</strong> 이 때를 HOL Blocking 현상이라고 한다.</p>
<hr />
<h2 id="http2">HTTP/2</h2>
<p>1.x 보다 지연 시간을 줄이고 응답 시간을 더 빠르게 하여</p>
<ul>
<li>멀티플렉싱</li>
<li>헤더 압축</li>
<li>서버 푸시</li>
<li>요청의 우선순위 처리</li>
</ul>
<p>위 4가지를 지원하는 프로토콜이다.</p>
<h3 id="멀티플렉싱">멀티플렉싱</h3>
<blockquote>
<p>여러 개의 스트림을 사용하여 송수신하는 것으로, 하나의 통신 채널을 통해서 둘 이상의 데이터를 전송하는데 사용되는 기술</p>
<p>특정 스트림의 패킷이 손실되었어도 해당 스트림에만 영향을 미치고 나머지 스트림은 정상적으로 동작할 수 있다.</p>
</blockquote>
<ul>
<li><strong>스트림</strong> : 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7f73f3e4-91f2-4f73-9b95-ae94a22bed78/image.png" /></p>
<hr />
<h3 id="헤더-압축">헤더 압축</h3>
<p>HTTP/2에서는 헤더 압축을 써서 HTTP/1.x에서의 무거운 헤더 구조의 단점을 해결했는데,
허프만 코딩 압축 알고리즘을 사용하는 HPACK 압축 형식을 활용했다.</p>
<ul>
<li><strong>허프만 코딩</strong> : 문자열을 문자 단위로 쪼개 빈도수를 세어 <strong>빈도가 높은 정보는 적은 비트 수</strong>를 사용하여 표현하고, <strong>빈도가 낮은 정보는 비트 수를 많이 사용</strong>하여 표현해서 전체 데이터의 표현에 <strong>필요한 비트양을 줄이는 원리</strong></li>
</ul>
<hr />
<h3 id="서버-푸시">서버 푸시</h3>
<p>HTTP/1.1에서는 파일을 다운로드하려면 클라이언트가 서버에 요청을 해야 한다.</p>
<p>HTTP/2는 클라이언트 요청 없이 서버가 바로 리소스를 푸시할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/85205b29-d5dd-42b9-bdec-0a476090a855/image.png" /></p>
<p>html에는 css나 js 파일이 포함되기 마련인데, html을 읽으면서 안에 있는 css 파일을 서버에 푸시해 클라이언트에 먼저 줄 수 있다.</p>
<hr />
<h2 id="https">HTTPS</h2>
<p>HTTP/2는 HTTPS 위에서 동작한다.</p>
<blockquote>
<p><strong>HTTPS</strong> : 애플리케이션 계층과 전송 계층 사이에 <strong>신뢰 계층인 SSL/TLS 계층</strong>을 넣어서 신뢰할 수 있는 HTTP 요청을 말한다. -&gt; <strong>통신을 암호화</strong>한다.</p>
</blockquote>
<h3 id="ssltls">SSL/TLS</h3>
<blockquote>
<p>전송 계층에서 보안을 제공하는 프로토콜
클라이언트와 서버가 통신을 할 때 SSL/TLS를 통해 제3자가 메시지를 도청하거나 변조하지 못하도록 한다.</p>
</blockquote>
<p>SSL(Secure Socket Layer)은 SSL 1.0부터 시작해서 TLS(Transport Layer Security Protocol)로 명칭이 변경되었으나 보통 <strong>SSL/TLS</strong>이라고 한다.</p>
<p>보안 세션을 기반으로 데이터를 암호화하며 <strong>보안 세션이 만들어질 때 인증 메커니즘, 키 교환 암호화 알고리즘, 해싱 알고리즘이 사용</strong>된다.</p>
<ul>
<li><strong>세션</strong> : 운영체제가 어떤 사용자로부터 자신의 자산 이용을 허락하는 일정한 기간<ul>
<li>즉, 사용자는 일정 기간 동안 응용 프로그램, 자원 등을 사용할 수 있다.</li>
</ul>
</li>
</ul>
<hr />
<h2 id="http3">HTTP/3</h2>
<p>HTTP/3 는 TCP 위에서 돌아갔던 HTTP/2와 다르게 <strong>QUIC 라는 계층 위에서 돌아간다.</strong></p>
<ul>
<li>추가로 <strong>UDP 기반으로 돌아간다.</strong></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b69f91e2-5fb1-4c49-8f3f-a645464d2c8c/image.png" /></p>
<p><a href="https://www.keysight.com/blogs/en/tech/nwvs/2022/07/08/http3-and-quic-prepare-your-network-for-the-most-important-transport-change-in-decades">참고 링크</a></p>
<p><strong>장점</strong></p>
<ul>
<li>HTTP/2에서 장점이었던 멀티플렉싱</li>
<li>초기 연결 설정 시 지연 시간 감소</li>
</ul>
<hr />
<h3 id="초기-연결-설정-시-지연-시간-감소">초기 연결 설정 시 지연 시간 감소</h3>
<p>QUIC는 TCP를 사용하지 않기 때문에 통신을 시작할 때 번거로운 3-way handshake 과정을 거치지 않아도 된다.</p>
<p>QUIC는 첫 연결 설정에 1-RTT(Round Trip Time, 왕복 시간)만 소요되고, 클라이언트가 보낸 어떠한 신호에 서버가 응답한다면 바로 통신을 할 수 있다.</p>
<hr />
<p>Reference</p>
<ul>
<li><a href="https://www.informit.com/">https://www.informit.com/</a></li>
<li><a href="https://babbab2.tistory.com/7">https://babbab2.tistory.com/7</a></li>
</ul>