<h1 id="ip-주소">IP 주소</h1>
<p>인터넷 계층에 있는 IP 주소에 대해 좀 더 자세히 알아보자.</p>
<h2 id="arp">ARP</h2>
<blockquote>
<p>IP 주소로부터 MAC 주소를 구하는 IP와 MAC 주소의 다리 역할을 하는 프로토콜</p>
</blockquote>
<p>ARP를 통해 가상 주소인 IP 주소를 실제 주소인 MAC 주소로 변환한다.</p>
<p>반대로 RARP를 통해 실제 주소인 MAC 주소를 가상 주소인 IP 주소로 변환하기도 한다.</p>
<p>예시를 들어서 어느 하나의 1번 장치가 있다면, </p>
<ol>
<li>ARP Request 브로드캐스트를 보내서 해당 가상 주소 IP주소에 해당하는 MAC 주소를 찾는다.</li>
<li>해당 주소에 맞는 2번 장치에서 ARP Reply 유니캐스트를 통해 MAC 주소를 반환하는 과정을 거져 IP 주소에 맞는 MAC 주소를 찾게 된다.</li>
</ol>
<blockquote>
<p><strong>브로드캐스트</strong> : 송신 호스트가 전송한 데이터가 네트워크에 연결된 모든 호스트에 전송되는 방식
<strong>유니캐스트</strong> : 고유 주소로 식별된 하나의 네트워크 목적에 1:1로 데이터를 전송하는 방식</p>
</blockquote>
<hr />
<h2 id="홉바이홉-통신-hop-by-hop">홉바이홉 통신 (Hop by Hop)</h2>
<blockquote>
<p>IP 주소를 통해 통신하는 과정</p>
<ul>
<li>hop : 건너뛴다는 개념</li>
</ul>
</blockquote>
<p>통신망에서 각 패킷이 여러 개의 라우터를 건너가는 모습을 비유적으로 표현한 것이다.</p>
<p>통신 장치에 있는 <strong>라우팅 테이블</strong>의 IP를 통해 시작 주소부터 시작해 다음 IP로 계속해서 이동하는 <strong>라우팅</strong> 과정을 거쳐 패킷이 최종 목적지까지 도달하는 통신을 뜻한다.</p>
<h3 id="라우팅-테이블">라우팅 테이블</h3>
<blockquote>
<p>송신지에서 수신지까지 도달하기 위해 사용하며 라우터에 들어 있는 목적지 정보들과 해당 목적지로 가기 위한 방법이 들어 있는 리스트</p>
</blockquote>
<p>라우팅 테이블에는 <strong>게이트웨이</strong>와 모든 목적지에 대해 해당 목적지에 도달하기 위해 거쳐야 할 다음 라우터의 정보를 가지고 있다.</p>
<hr />
<h3 id="게이트웨이">게이트웨이</h3>
<blockquote>
<p>서로 다른 네트워크 간 통신을 가능하게 하는 관문</p>
</blockquote>
<p>사용자는 인터넷에 접속하기 위해서 수많은 게이트웨이를 거쳐야 한다.</p>
<p>통신을 가능하게 해준다는 것은 즉, <strong>서로 다른 네트워크 상의 통신 프로토콜을 변환해주는 역할</strong></p>
<blockquote>
<p>게이트웨이 확인 방법
라우팅 테이블을 통해 볼 수 있다.</p>
<ul>
<li>라우팅 테이블은 <code>netstat -r</code> 명령어를 실행해 확인 가능
IPv4 경로 테이블, IPv6 경로 테이블이 있는데 이것이 라우팅 테이블이다.</li>
</ul>
</blockquote>
<hr />
<h2 id="ip-주소-체계">IP 주소 체계</h2>
<ol>
<li><p><strong>IPv4</strong>
32비트를 8비트 단위로 점을 찍어서 표기 -&gt; ex) <code>123.45.67.89</code></p>
</li>
<li><p><strong>IPv6</strong>
64비트를 16비트 단위로 점을 찍어서 표기 -&gt; ex) <code>2001:db8::ff00:12:3456</code></p>
</li>
</ol>
<p>추세는 IPv6로 가고 있긴 하지만 가장 많이 쓰이는건 IPv4이다.</p>
<hr />
<h3 id="클래스-기반-할당-방식">클래스 기반 할당 방식</h3>
<p>네트워크 ID(네트워크 주소)와 호스트 ID(호스트 주소)를 먼저 알아보자.</p>
<p>Network ID, Host ID
하나의 네트워크 주소에는 Network ID와 Host ID
즉, 네트워크 주소와 호스트 주소가 있다.</p>
<blockquote>
<p>네트워크 주소란?
인터넷 상에서 모든 host들을 전부하기 힘들기 때문에 한 네트워크의 범위를 지정하여 관리하기 쉽게 만든 것.</p>
</blockquote>
<blockquote>
<p>호스트 주소란?
호스트를 개별적으로 관리하기 위해 사용하게 된 것.</p>
</blockquote>
<p><strong>클래스 기반 할당 방식</strong>
A, B, C, D, E 이렇게 다섯개의 클래스로 구분하는 할당 방식을 쓴다.</p>
<p>앞의 부분을 네트워크 주소, 그 뒤를 컴퓨터에 부여하는 호스트 주소로 사용한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8cb3f102-9032-41e7-8212-bf89472e7821/image.png" /></p>
<ul>
<li>클래스 A, B, C는 <strong>일대일 통신</strong>으로 사용</li>
<li>클래스 D는 <strong>멀티캐스트 통신</strong></li>
<li>클래스 E는 미래에 사용하기 위한 <strong>예비용 클래스</strong>로 사용</li>
</ul>
<blockquote>
<p>클래스 A
처음 1바이트(8비트)가 네트워크 주소이며, 나머지 3바이트(24비트)는 호스트 주소이다.
맨 왼쪽에 있는 구분 비트는 0으로 시작하고 네트워크 할당은 0~127 이다.</p>
</blockquote>
<blockquote>
<p>클래스 B
처음 2바이트(16비트)가 네트워크 주소이며, 나머지 2바이트(16비트)는 호스트 주소이다.
맨 왼쪽에 있는 구분 비트는 10으로 시작하고, 네트워크 할당은 128~191 이다.</p>
</blockquote>
<blockquote>
<p>클래스 C
처음 3바이트(24비트)가 네트워크 주소이며, 나머지 1바이트(8비트)는 호스트 주소이다.
맨 왼쪽에 있는 구분 비트는 110으로 시작하고, 네트워크 할당은 192~223 이다.</p>
</blockquote>
<hr />
<h4 id="클래스-구분-방법">클래스 구분 방법</h4>
<p>간단하게 첫번째 Octet으로 구분할 수 있으며,
첫번째 Octet에서 0~255 까지의 숫자를 5개로 나눠서 구분하면 된다.</p>
<p><strong>클래스 A</strong> 는 0~255 (총 256개의 절반 128) -&gt; 0부터 시작이므로 <strong>127</strong></p>
<ul>
<li>(0.0.0.0 ~ 127.255.255.255)</li>
</ul>
<p><strong>클래스 B</strong> 는 128~255 (총 128개의 절반 64) -&gt; 0부터 시작이므로 <strong>191</strong></p>
<ul>
<li>(128.0.0.0 ~ 191.255.255.255)</li>
</ul>
<p><strong>클래스 C</strong> 는 192~255 (총 64개의 절반 32) -&gt; 0부터 시작이므로 <strong>223</strong></p>
<ul>
<li>(192.0.0.0 ~ 233.255.255.255)</li>
</ul>
<p><strong>클래스 D</strong> 는 224~255 (총 32개의 절반 16) -&gt; 0부터 시작이므로 <strong>239</strong></p>
<ul>
<li>(234.0.0.0 ~ 239.255.255.255)</li>
</ul>
<p><strong>클래스 E</strong> 는 240~255 (총 16개) -&gt; 0부터 시작이므로 <strong>127</strong></p>
<ul>
<li>(240.0.0.0 ~ 255.255.255.255)</li>
</ul>
<hr />
<h4 id="네트워크-주소">네트워크 주소</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c5ac30a1-2bfa-43d9-94c9-a5ae24f28958/image.png" /></p>
<p><strong>네트워크 구별 주소 (= 네트워크 주소)</strong>
네트워크 구별 주소는 가장 첫번째 주소이며, 네트워크 주소로 사용된다.</p>
<p>⇀ 클래스 A의 120.0.0.0 이라는 네트워크를 부여받은 경우, 120.0.0.0 이 네트워크 구별 주소이다.</p>
<p><strong>호스트 주소</strong>
호스트 주소는 첫번째 주소와 마지막 주소를 제외한 나머지 주소를 말하며, 컴퓨터에 부여할 수 있는 주소이다.</p>
<p>⇀ 120.0.0.1 ~ 120.255.255.254 가 해당된다.</p>
<p><strong>브로드캐스트 주소</strong>
브로드캐스트 주소는 가장 마지막 주소를 말하며, 네트워크에 속해 있는 모든 컴퓨터에 데이터를 보내는 주소이다.</p>
<p>⇀ 120.255.255.255 가 여기에 속한다.</p>
<hr />
<p>클래스 기반 할당 방식은 <strong>사용하는 주소보다 버리는 주소가 많다는 단점</strong>이 있다.</p>
<p>그래서 DHCP, IPv6, NAT 이 나오게 된다.</p>
<hr />
<h3 id="dhcp">DHCP</h3>
<blockquote>
<p>IP 주소 및 기타 통신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜</p>
</blockquote>
<p>이것을 통해 IP 주소를 수동으로 설정할 필요 없이 인터넷에 접속할 때마다 자동으로 IP 주소를 할당할 수 있다.</p>
<p>많은 라우터와 게이트웨이 장비에 DHCP 기능이 있고, 대부분의 가정용 네트워크에서 IP 주소를 할당한다.</p>
<hr />
<h3 id="nat-network-address-translation">NAT (Network Address Translation)</h3>
<blockquote>
<p>패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정해 IP 주소를 다른 주소로 매핑하는 방법</p>
</blockquote>
<p>IPv4 주소 체계만으로는 수많은 주소들을 모두 감당하지 못한다. 그래서 NAT로 공인 IP 와 사설 IP로 나눠서 많은 주소를 처리한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a695ef56-ca66-4b9e-8fae-2b888df5e557/image.png" /></p>
<p><strong>NAT의 단점</strong></p>
<ul>
<li>여러 명이 동시에 인터넷에 접속하기 때문에 실제 접속 호스트 숫자에 따라 접속 속도가 느려질 수 있다.</li>
</ul>