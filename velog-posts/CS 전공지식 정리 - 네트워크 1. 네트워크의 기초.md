<blockquote>
<p><strong>네트워크</strong> : 컴퓨터 등 장치들이 통신 기술을 통해 구축하는 연결망을 지칭하는 용어</p>
</blockquote>
<h1 id="2-네트워크의-기초">2. 네트워크의 기초</h1>
<blockquote>
<p><strong>네트워크</strong> : 노드 &lt;-&gt; 링크가 서로 연결돼 있거나, 연결돼 리소스를 공유하는 집합</p>
</blockquote>
<ul>
<li>노드 : 서버, 라우터, 스위치 등 네트워크 장치들</li>
<li>링크 : 유선 or 무선 연결 방식</li>
</ul>
<h2 id="2---1-처리량-지연-시간">2 - 1. 처리량, 지연 시간</h2>
<blockquote>
<p><strong>좋은 네트워크의 개념</strong></p>
</blockquote>
<ul>
<li>많은 처리량을 처리 가능</li>
<li>지연 시간이 짧음</li>
<li>장애 빈도 낮음</li>
<li>좋은 보안을 갖춤</li>
</ul>
<h3 id="처리량">처리량</h3>
<blockquote>
<p>처리량 링크 내에서 성공적으로 전달된 데이터의 양
얼마만큼의 트래픽을 처리했는지 나타낸다.</p>
</blockquote>
<p><strong>처리량의 단위</strong> -&gt; bps(bits per second) : 초당 전송 or 수신되는 bit 수</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/87dc0313-e3fb-4981-af02-f077ae21518c/image.png" /></p>
<h4 id="트래픽-처리량-의미-차이">트래픽, 처리량 의미 차이</h4>
<ul>
<li>트래핑이 많아짐 =&gt; &quot;<strong>흐르는 데이터</strong>가 많아졌다.&quot;</li>
<li>처리량이 많아짐 =&gt; &quot;<strong>처리되는 트래픽</strong>이 많아졌다.&quot;</li>
</ul>
<blockquote>
<p><strong>대역폭</strong> : 주어진 시간 동안 네트워크 연결을 통해 흐를 수 있는 최대 bit 수</p>
</blockquote>
<hr />
<h3 id="지연-시간">지연 시간</h3>
<blockquote>
<p>요청이 처리되는 시간을 의미한다.</p>
</blockquote>
<p>지연 시간에 영향을 주는 것</p>
<ul>
<li>매체 타입 (무선, 유선)</li>
<li>패킷 크기</li>
<li>라우터의 패킷 처리 시간</li>
</ul>
<hr />
<h2 id="2---2-네트워크-토폴로지-병목-현상">2 - 2. 네트워크 토폴로지, 병목 현상</h2>
<blockquote>
<p>네트워크 토폴로지 : 노드와 링크가 어떻게 배치돼 있는지에 대한 방식이자 연결 형태</p>
</blockquote>
<h3 id="트리-토폴로지">트리 토폴로지</h3>
<p>'계층형 토폴로지' 라고도 하며 트리 형태로 배치한 네트워크 구조
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/217dae2a-35b1-4a9e-b0bd-54fffa957630/image.png" /></p>
<ul>
<li>노드의 추가, 삭제가 쉽다</li>
<li>특정 노드에 트래픽이 집중될 때 하위 노드에 영향을 줄 수 있다.</li>
</ul>
<hr />
<h3 id="버스-토폴로지">버스 토폴로지</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/06b3831f-3eae-42a7-92ca-19e1c53fe94e/image.png" /></p>
<p>중앙 통신 회선 하나에 여러 개의 노드가 연결돼 공유하는 네트워크 구성
근거리 통신망 (LAN)에서 사용한다.</p>
<p><strong>장점</strong></p>
<ul>
<li>설치 비용이 적다</li>
<li>신뢰성이 우수하다.</li>
<li>중앙 통신 회선에 노드 추가, 삭제가 쉽다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>스푸핑이 가능한 문제점이 있다.</li>
</ul>
<blockquote>
<p>스푸핑 : LAN 상에서 송신부의 패킷을 송신과 관련 없는 다른 호스트에 가지 않도록 하는 스위칭 기능을 속이거나 마비시켜 특정 노드에 해당 패킷이 오도록 처리하는 것이다.</p>
</blockquote>
<p>중앙 통신 회선 하나에 여러 개의 노드가 연결돼 있기 때문에 스위칭 기능을 속이게 되면 스푸핑이 가능해진다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/891c0401-7899-4925-afad-61066d1a9eee/image.png" /></p>
<hr />
<h3 id="스타-토폴로지">스타 토폴로지</h3>
<p>중앙에 있는 노드에 모두 연결된 네트워크 구성
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cd317b03-62b6-4158-b6de-603307a695eb/image.png" /></p>
<p><strong>장점</strong></p>
<ul>
<li>노드를 추가하기 쉽다</li>
<li>에러를 탐지하기 쉽다</li>
<li>패킷의 충돌 가능성이 적다.</li>
<li>장애 노드가 중앙 노드가 아닌 경우 다른 노드에 영향을 끼치는 것이 적다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>중앙 노드에 장애 발생 시 전체 네트워크를 사용할 수 없다.</li>
<li>설치 비용이 많이 나간다.</li>
</ul>
<hr />
<h3 id="링형-토폴로지">링형 토폴로지</h3>
<p>각각의 노드가 양 옆의 1개씩 노드들과 연결돼 고리 (반지) 형태로 연속돼 통신하는 망 구성 방식
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a467b030-58ac-40bf-939c-492e007c4ce1/image.png" /></p>
<p><strong>장점</strong></p>
<ul>
<li>노드의 개수가 증가해도 네트워크 상 손실이 거의 없다.</li>
<li>충돌이 발생될 가능성이 적다.</li>
<li>노드 고장 발견을 쉽게 찾을 수 있다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>네트워크 구성 변경이 어렵다.</li>
<li>회선에 장애가 발생 시 전체 네트워크에 영향을 크게 끼칠 수 있다.</li>
</ul>
<hr />
<h3 id="메시-토폴로지-망형-토폴로지">메시 토폴로지 (망형 토폴로지)</h3>
<p>메시(mesh)는 그물망처럼 돼 있는걸 의미한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/df547f63-aa5b-4f3c-ac81-dd660abe4031/image.png" /></p>
<p><strong>장점</strong></p>
<ul>
<li>하나의 장치에 장애가 발생해도 여러 개의 경로가 존재해 네트워크를 지속적으로 사용 가능하다.</li>
<li>추가로 트래픽 분산 처리도 가능하다.</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li>노드의 추가가 어렵다.</li>
<li>구축 비용, 운용 비용이 많이 든다.</li>
</ul>
<hr />
<h3 id="병목-현상">병목 현상</h3>
<blockquote>
<p><strong>병목 현상</strong> : 전체 시스템의 성능이나 용량이 하나의 구성 요소로 인해 제한을 받는 현상</p>
<p>예시로는 공간이 엄청 넓더라도 만약 출입문의 개수가 적거나 작다면 유동 유동의 흐름에 문제가 생길 수 있다.
서비스에서 이벤트를 열었을 때 트래픽이 많이 생기고, 트래픽을 잘 관리하지 못 하면 병목 현상이 생겨 사용자는 웹 사이트로 들어가지 못하게 된다.</p>
</blockquote>
<p>위 병목 현상을 찾을 때 중요한 기준이 될 수 있기 때문에 토폴로지가 중요하다.</p>
<hr />
<h2 id="2---3-네트워크-분류">2 - 3. 네트워크 분류</h2>
<ol>
<li>LAN (Local Area Network) : 규모가 작아서 개인적으로 소유 가능한 규모</li>
<li>MAN (Metropolitan Area Network) : 서울시 와 같이 시 규모</li>
<li>WAN (Wide Area Network) : 세계 규모</li>
</ol>
<ul>
<li>규모가 커질 수록 전송 속도는 느려지고, 혼잡스러워진다.</li>
</ul>
<hr />
<h2 id="2---4-네트워크-성능-분석-명령어">2 - 4. 네트워크 성능 분석 명령어</h2>
<p>애플리케이션 코드 상으로 문제가 없는데도 사용자가 서비스로부터 데이터를 못 가져올 때 네트워크 병목 현상일 가능성이 있다.</p>
<p>주된 원인으로는</p>
<ul>
<li>네트워크 대역폭</li>
<li>네트워크 토폴로지</li>
<li>서버 CPU, 메모리 사용량</li>
<li>비효율적인 네트워크 구성</li>
</ul>
<p>이런 원인들이 있으며 성능 분석을 위해 사용하는 명령어들을 알아보자.</p>
<h3 id="ping">ping</h3>
<blockquote>
<p>ping (<strong>P</strong>acket <strong>IN</strong>ternet <strong>G</strong>roper) : 네트워크 상태를 확인하려는 대상 노드를 향해 일정 크기의 패킷을 전송하는 명령어</p>
</blockquote>
<ul>
<li>해당 노드의 패킷 수신 상태</li>
<li>도달하기까지의 시간</li>
<li>네트워크가 잘 연결돼 있는지</li>
</ul>
<p>위와 같은 데이터를 얻을 수 있다.</p>
<p><strong>ping은 TCP/IP 프로토콜 중 ICMP 프로토콜을 통해 동작</strong>한다.
그래서 ICMP 프로토콜을 지원하지 않는 기기를 대상으로는 실행할 수 없거나 네트워크 정책 상 ICMP나 traceroute를 차단하는 경우 테스팅 불가능하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/39711d61-627e-4c82-b1e0-0a3f9c355cdd/image.jpg" /></p>
<p>-n 12 옵션을 넣어 12번의 패킷을 보내고 12번 받는 것을 볼 수 있다.</p>
<p>명령 프롬프트에서 해볼 수 있다.</p>
<hr />
<h3 id="netstat">netstat</h3>
<blockquote>
<p>netstat : 접속돼 있는 서비스들의 네트워크 상태를 표시하는 데 사용되는 명령어</p>
</blockquote>
<p>네트워크 접속, 라우팅 테이블, 네트워크 프로토콜 등 리스트를 보여준다.</p>
<p>주로 서비스의 포트가 열려 있는지 확인할 때 쓴다.</p>
<hr />
<h3 id="nslookup">nslookup</h3>
<blockquote>
<p>nslookup : DNS에 관련된 내용을 확인하기 위해 쓰는 명령어</p>
</blockquote>
<p>특정 도메인에 매핑된 IP를 확인하기 위해 사용한다.</p>
<hr />
<h3 id="tracert-윈도우-traceroute-리눅스">tracert (윈도우), traceroute (리눅스)</h3>
<blockquote>
<p>tracert : 목적지 노드까지 네트워크 경로를 확인할 때 쓰는 명령어</p>
</blockquote>
<p>목적지 노드까지 구간들 중 어느 구간에서 응답 시간이 느려지는지 등을 확인할 수 있다.</p>
<hr />
<h2 id="네트워크-프로토콜-표준화">네트워크 프로토콜 표준화</h2>
<blockquote>
<p>네트워크 프로토콜 : 다른 장치들끼리 데이터를 주고받기 위해 설정된 공통된 인터페이스</p>
<p>IEEE 또는 IETF 라는 표준화 단체가 정한다.</p>
</blockquote>
<p><code>IEEE802.3</code> : 유선 LAN 프로토콜로, 유선으로 LAN을 구축할 때 쓰이는 프로토콜이다. 기업이 다른 장치라도 서로 데이터를 수신할 수 있는 것이다.</p>
<p>예시로는 HTTP가 있는데, HTTP 라는 프로토콜을 통해 노드들은 웹 서비스를 기반으로 데이터를 주고 받을 수 있다.</p>