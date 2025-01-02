<h1 id="tcpip-4계층-모델">TCP/IP 4계층 모델</h1>
<p><code>인터텟 프로토콜 스위트</code> : 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 프로토콜의 집합이다.</p>
<p>TCP/IP 4계층 모델 or OSI 7계층 모델로 설명한다. TCP/IP 4계층을 보자.</p>
<hr />
<h2 id="2---5-계층-구조">2 - 5. 계층 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/753e6288-3576-4751-9843-2b8433bb30ff/image.png" /></p>
<p>위와 같이 비교할 수 있다.</p>
<p>애플리케이션 계층부터 보자.</p>
<hr />
<h3 id="애플리케이션-계층">애플리케이션 계층</h3>
<p>FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계층이다.</p>
<blockquote>
<ul>
<li><strong>FTP</strong> : 장치와 장치 간의 파일을 전송하는 데 사용되는 표준 통신 프로토콜</li>
<li><strong>HTTP</strong> : World Wide Web (www) 을 위한 데이터 통신의 기초이자 웹 사이트를 이용하는 데 쓰는 프로토콜</li>
<li><strong>SSH</strong> : 보안되지 않은 네트워크에서 안전하게 서비스 운영을 하기 위해 사용하는 암호화 네트워크 프로토콜</li>
<li><strong>SMTP</strong> : 이메일 전송과 같이 전자 메일 전송을 위한 인터넷 표준 통신 프로토콜</li>
<li><strong>DNS</strong> : 도메인 이름과 IP 주소를 매핑해주는 서버. IP 주소가 바뀌어도 도메인 주소로 서비스할 수 있다.</li>
</ul>
</blockquote>
<hr />
<h3 id="전송-계층">전송 계층</h3>
<p>송신자와 수신자를 연결하는 통신 서비스를 제공한다.
연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어를 제공할 수 있다.</p>
<ul>
<li>애플리케이션 계층과 인터넷 계층 사이의 데이터가 전달될 때 중계 역할을 한다.</li>
</ul>
<p>TCP, UDP, QUIC 가 있다.</p>
<blockquote>
<ul>
<li>TCP : 패킷 사이의 순서를 보장하고 연결 지향 프로토콜을 사용해 연결하기 때문에 신뢰성을 구축해 수신 여부를 확인할 수 있다. <code>가상회선 패킷 교환 방식</code>을 사용한다.</li>
<li>UDP : 순서를 보장하지 않고 수신 여부도 확인하지 않는다. 단순히 데이터만 주는 <code>데이터그램 패킷 교환 방식</code>을 사용한다.</li>
</ul>
</blockquote>
<p><code>가상회선 패킷 교환 방식</code>과 <code>데이터그램 패킷 교환 방식</code> 을 알아보자.</p>
<h4 id="가상회선-패킷-교환-방식">가상회선 패킷 교환 방식</h4>
<p>각 패킷에는 가상회선 식별자가 포함돼 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 <strong>순서대로</strong> 도착한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b62f09c1-d32b-4180-8431-ffa9f23c1fcd/image.png" /></p>
<hr />
<h4 id="데이터그램-패킷-교환-방식">데이터그램 패킷 교환 방식</h4>
<p>패킷이 독립적으로 이동해 최적의 경로를 선택해서 간다.
하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있어서 보낸 메시지의 <strong>순서가 다르게</strong> 도착할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/87fb2be9-f45f-4096-ad33-e14455e227c3/image.png" /></p>
<hr />
<h4 id="tcp-연결-성립-과정">TCP 연결 성립 과정</h4>
<p>TCP는 신뢰성을 확보할 때 <strong>3-way handshake</strong> 라는 작업을 진행한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e51b3ce1-04ed-4ffa-990b-f64414034712/image.png" /></p>
<ol>
<li><strong>SYN 단계</strong> : 클라이언트는 서버에 클라이언트의 ISN을 담아 보낸다.<ul>
<li>ISN : 새로운 TCP 연결의 첫 번째 패킷에 할당된 임의의 시퀀스 번호</li>
</ul>
</li>
<li><strong>SYN + ACK 단계</strong> : 서버는 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로는 ISN + 1 값을 보낸다.</li>
<li><strong>ACK 단계</strong> : 그 승인번호를 클라이언트가 담아 ACK 서버에 보낸다.</li>
</ol>
<blockquote>
<ul>
<li>SYN : 연결 요청 플래그</li>
<li>ACK : 응답 플래그</li>
<li>ISN : 초기 네트워크 연결할 때 할당된 32비트 고유 시퀀스 번호</li>
</ul>
</blockquote>
<p>TCP는 <strong>3-way handshake</strong> 로 인해 신뢰성이 있는 계층으로 볼 수 있다.</p>
<hr />
<h4 id="tcp-연결-해제-과정">TCP 연결 해제 과정</h4>
<p>연결을 해제할 때는 <strong>4-way handshake</strong> 과정을 진행한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/97d02436-d0f6-4351-b95d-1e7c7fefc978/image.png" /></p>
<ol>
<li><p>클라이언트가 연결을 닫으려고 할 때 <code>FIN</code> 으로 설정된 세그먼트를 보낸다.</p>
<ul>
<li>클라이언트는 <code>FIN_WAIT_1</code> 상태로 들어가 서버의 응답을 기다린다.</li>
</ul>
</li>
<li><p>서버는 클라이언트로 <code>ACK</code>라는 승인 세그먼트를 보낸다.</p>
<ul>
<li>서버도 <code>CLOSE_WAIT</code> 상태로 들어간다.</li>
<li>클라이언트가 세그먼트를 받으면 <code>FIN_WAIT_2</code> 상태로 들어간다.</li>
</ul>
</li>
<li><p>서버는 <code>ACK</code>를 보내고 일정 시간이 지난 뒤 클라이언트에 <code>FIN</code>이라는 세그먼트를 보낸다.</p>
</li>
<li><p>클라이언트는 <code>TIME_WAIT</code> 상태가 되고 다시 서버로 <code>ACK</code>를 보내 서버는 <code>CLOSED</code> 상태가 된다.
이후 클라이언트는 <strong>어느 정도의 시간을 대기한 뒤</strong> 연결이 닫히고 모든 자원의 연결이 해제된다.</p>
</li>
</ol>
<blockquote>
<p>어느 정도의 시간을 대기한 뒤에 닫는 이유?</p>
</blockquote>
<ol>
<li>지연 패킷이 발생할 경우를 대비하기 위해<ul>
<li>그냥 연결을 닫앗다가 패킷이 뒤늦게 도착해 처리하지 못하면 데이터 무결성 문제가 발생한다.</li>
</ul>
</li>
<li>두 장치가 연결이 닫혔는지 확인하기 위해<ul>
<li><code>LAST_ACK</code> 상태에서 닫히게 되면 다시 새로운 연결을 하려고 할 때 장치는 줄곧 <code>LAST_ACK</code>로 돼 있어 접속 오류가 생긴다.</li>
</ul>
</li>
</ol>
<blockquote>
<p>데이터 무결성 : 데이터의 정확성과 일관성을 유지하고 보증하는 것</p>
</blockquote>
<hr />
<h3 id="인터넷-계층">인터넷 계층</h3>
<blockquote>
<p>장치로부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용하는 계층</p>
</blockquote>
<p>IP, ARP, ICMP 등이 있고 패킷을 수신해야 할 상대의 주소를 지정해 데이터를 전달한다.</p>
<p>상대방이 제대로 수신했는지에 대해서는 보장하지 않는 <strong>비연결형</strong>이라는 특징을 가지고 있다.</p>
<hr />
<h3 id="링크-계층-네트워크-접근-계층">링크 계층 (네트워크 접근 계층)</h3>
<blockquote>
<p>전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달하며 장치 간 신호를 주고 받는 <strong>규칙</strong>을 정하는 계층</p>
</blockquote>
<p>링크 계층을 물리 계층 / 데이터 링크 계층 으로 나누기도 한다.</p>
<ul>
<li>물리 계층 : 무선 LAN 과 유선 LAN을 통해 0 과 1로 이루어진 데이터를 보내는 계층</li>
<li>데이터 링크 계층 : '이더넷 프레임'을 통해 에러 확인, 흐름 제어, 접근 제어를 담당하는 계층</li>
</ul>
<blockquote>
<p>유선 LAN (IEEE 802.3) : 유선 LAN을 이루는 이더넷은 IEEE802.3 이라는 프로토콜을 따르며 전이중화 통신을 쓴다.</p>
</blockquote>
<p>그럼 전이중화 통신에 대해서도 알아보자.</p>
<h4 id="전이중화-통신">전이중화 통신</h4>
<blockquote>
<p>양쪽 장치가 동시에 송수신할 수 있는 방식</p>
</blockquote>
<p>송신로, 수신로로 나눠서 데이터를 주고 받으며 현대의 고속 이더넷은 이 방식으로 통신한다.</p>
<hr />
<h4 id="반이중화-통신">반이중화 통신</h4>
<blockquote>
<p>양쪽 장치가 통신은 가능하지만, 동시에는 불가능해 한 번에 한 방향만 통신할 수 있는 방식을 말한다.</p>
</blockquote>
<blockquote>
<p>이더넷 프레임 : 데이터 링크 계층은 이더넷 프레임을 통해 데이터의 에러를 검출하고 캡슐화해 아래 구조를 갖는다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/930528da-2c45-42b3-a0f6-736c1e68545d/image.png" /></p>
<ul>
<li>Preamble : 이더넷 프레임이 시작임을 알리는 것</li>
<li>SFD : 다음 바이트부터 MAC 주소 필드가 시작됨을 알리는 것</li>
<li>DMAC, SMAC : 수신, 송신 MAC 주소를 말하는 것</li>
<li>EtherType : 데이터 계층 위의 계층인 IP 프로토콜을 정의한다.<ul>
<li>IPv4 or IPv6</li>
</ul>
</li>
<li>Payload : 전달받은 데이터</li>
<li>CRC : 에러 확인 비트</li>
</ul>
</blockquote>
<blockquote>
<p>MAC 주소 : 컴퓨터, 노트북 등 각 장치에는 네트워크에 연결하기 위한 장치 (LAN 카드)가 있는데 구별하기 위한 식별번호를 말한다.</p>
</blockquote>