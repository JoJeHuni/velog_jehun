<h1 id="route-table">Route Table</h1>
<blockquote>
<p>네트워크 통행의 이정표 역할을 하는 구성요소</p>
</blockquote>
<p>어느 목적지가 있을 때 어떤 경로로 가야 하는지 정의를 해주는 구성요소로 각 서브넷 별로 다른 라우팅 규칙을 적용할 수 있으며 인터넷 게이트웨이, NAT 게이트웨이 등과의 연결을 정의한다.</p>
<p>VPC 내부 통신과 외부 통신의 경로를 결정하는 핵심 구성 요소로 효율적인 네트워크 운영의 기반이 된다.</p>
<blockquote>
<p><strong>Nat Gateway가 생성 될 때 별도로 private subnet을 선택하는 이유</strong> :
NAT Gateway는 통로를 만드는 것이고, 라우팅 테이블은 그 통로로 가는 길을 알려주는 지도와 같은 역할을 한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ba71a3a1-979d-4e05-b2b7-56abc0974319/image.png" /></p>
<p>라우팅 테이블을 만들어 볼 것이다.</p>
<hr />
<p>public subnet 전용 Route Table 과 private subnet 각각의 Route Table을 만들어줄 것이다.</p>
<p>총 3개를 만드는 것이다.</p>
<ol>
<li><strong>intbyte-rt-public</strong></li>
</ol>
<table>
<thead>
<tr>
<th><strong>서브 카테고리</strong></th>
<th><strong>대상</strong></th>
<th><strong>항목</strong></th>
<th><strong>이름</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>라우팅</strong></td>
<td>로컬</td>
<td>송신 대상자</td>
<td>10.0.0.0/16</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>Local</td>
</tr>
<tr>
<td></td>
<td>외부</td>
<td>송신 대상자</td>
<td>0.0.0.0/0</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>intbyte-igw</td>
</tr>
<tr>
<td><strong>서브넷</strong></td>
<td>퍼블릭 서브넷</td>
<td>서브넷 ID</td>
<td>intbyte-subnet-public01</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>intbyte-subnet-public02</td>
</tr>
</tbody></table>
<ol start="2">
<li><strong>intbyte-rt-private01</strong></li>
</ol>
<table>
<thead>
<tr>
<th><strong>서브 카테고리</strong></th>
<th><strong>대상</strong></th>
<th><strong>항목</strong></th>
<th><strong>이름</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>라우팅</strong></td>
<td>로컬</td>
<td>송신 대상자</td>
<td>10.0.0.0/16</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>Local</td>
</tr>
<tr>
<td></td>
<td>외부</td>
<td>송신 대상자</td>
<td>0.0.0.0/0</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>intbyte-ngw-01</td>
</tr>
<tr>
<td><strong>서브넷</strong></td>
<td>퍼블릭 서브넷</td>
<td>서브넷 ID</td>
<td>intbyte-subnet-private01</td>
</tr>
</tbody></table>
<ol start="3">
<li><strong>intbyte-rt-private02</strong></li>
</ol>
<table>
<thead>
<tr>
<th><strong>서브 카테고리</strong></th>
<th><strong>대상</strong></th>
<th><strong>항목</strong></th>
<th><strong>이름</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>라우팅</strong></td>
<td>로컬</td>
<td>송신 대상자</td>
<td>10.0.0.0/16</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>Local</td>
</tr>
<tr>
<td></td>
<td>외부</td>
<td>송신 대상자</td>
<td>0.0.0.0/0</td>
</tr>
<tr>
<td></td>
<td></td>
<td>타깃</td>
<td>intbyte-ngw-02</td>
</tr>
<tr>
<td><strong>서브넷</strong></td>
<td>퍼블릭 서브넷</td>
<td>서브넷 ID</td>
<td>intbyte-subnet-private02</td>
</tr>
</tbody></table>
<hr />
<h2 id="라우팅-테이블-생성">라우팅 테이블 생성</h2>
<ol>
<li><p>AWS VPC 왼쪽 탭 <code>라우팅 테이블</code> 에서 <code>라우팅 테이블 생성</code></p>
</li>
<li><p>위의 표대로 이름 설정, VPC 선택 후 생성한 뒤</p>
</li>
</ol>
<p>3-1. Internet Gateway의 경우 해당 라우팅 테이블을 클릭한 뒤 <code>라우팅 편집</code>
3-2. Nat Gateway의 경우 인터넷 게이트웨이와 같이 하되 NAT 게이트웨이로 선택해주면 된다.</p>
<ol start="4">
<li><p><code>라우팅 추가</code> 클릭
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fbb639f2-914b-4395-ac24-88b0a2a079f7/image.png" /></p>
</li>
<li><p>서브넷 연결 편집
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2c8b72bf-41f3-4dcc-8402-1a4f2d8f8b5c/image.png" /></p>
</li>
<li><p>public 라우팅 테이블이면 public subnet 2개, private이면 각각 맞는 subnet 선택</p>
</li>
</ol>
<hr />
<h2 id="추가-보안그룹-설정-및-ec2-만드는-과정">추가 보안그룹 설정 및 EC2 만드는 과정</h2>
<h3 id="보안그룹-설정">보안그룹 설정</h3>
<p>다음 게시글은 <code>Bastion Server</code>에 대한 게시글인데 해당 서버를 위해 보안 그룹을 설정해줘야 한다.</p>
<ol>
<li><code>AWS EC2</code>로 이동</li>
<li>왼쪽 탭의 <code>보안 그룹</code> 선택</li>
<li><code>보안 그룹 생성</code> 버튼 클릭</li>
<li>본인이 원하는 이름 지정, 이전에 만들었던 VPC로 선택<ul>
<li>나는 <code>intbyte-sg-bastion</code> 으로 만들었다.</li>
</ul>
</li>
<li>인바운드 규칙<ul>
<li><code>SSH</code> 유형, 소스는 <code>Anywhere-IPv4</code></li>
<li><code>모든 ICMP - IPv4</code> 유형, 소스는 <code>Anywhere-Ipv4</code></li>
</ul>
</li>
</ol>
<hr />
<p>추가로 <code>Load Balancer</code> 용 보안 그룹도 만들어보자.</p>
<ol>
<li><code>AWS EC2</code>로 이동</li>
<li>왼쪽 탭의 <code>보안 그룹</code> 선택</li>
<li><code>보안 그룹 생성</code> 버튼 클릭</li>
<li>본인이 원하는 이름 지정, 이전에 만들었던 VPC로 선택<ul>
<li>나는 <code>intbyte-sg-elb</code> 이름으로 만들었다.</li>
</ul>
</li>
<li>인바운드 규칙<ul>
<li><code>SSH</code> 유형, 소스는 <code>Anywhere-IPv4</code></li>
<li><code>모든 ICMP - IPv4</code> 유형, 소스는 <code>Anywhere-Ipv4</code></li>
<li><code>HTTP</code> 유형, 소스는 <code>Anywhere-IPv4</code> (포트는 80)</li>
<li><code>HTTPS</code> 유형, 소스는 <code>Anywhere-IPv4</code> (포트는 443)</li>
</ul>
</li>
</ol>
<hr />
<p><code>키 페어</code>도 생성해보자.</p>
<ul>
<li>EC2 -&gt; 키 페어 -&gt; 키 페어 생성</li>
<li>키 페어 유형은 <code>RSA</code></li>
<li>프라이빗 키 파일 형식은 <code>.pem</code></li>
</ul>
<p>형태로 해서 사용했었다.</p>
<blockquote>
<p>만들면 파일이 다운로드 되는데 해당 파일이 키 페어 파일이므로 반드시 저장해두자.</p>
</blockquote>
<hr />
<p>Elastic Beanstalk에서 환경을 만들면 자동으로 담당 인스턴스가 생성이 된다.
그걸 만들어준다고 해서 모르고 넘어가는 것보다 EC2 또한 만드는 과정을 알아두는 것도 좋다고 생각해서 추가했다.</p>
<p>이전 게시글부터 만들어온 VPC, Subnet, 키 페어, 보안 그룹을 이용해서 만들어본다.</p>
<h3 id="ec2">EC2</h3>
<ol>
<li>AWS EC2에서 <code>인스턴스 시작</code></li>
<li>이름 설정</li>
<li><code>애플리케이션 및 OS 이미지 (Amazon Machine Image)</code> - <code>Amazon Linux</code> 를 선택<ul>
<li><code>2023 AMI</code></li>
</ul>
</li>
<li><code>인스턴스 유형</code>은 원하는 유형 설정 (프리 티어가 붙은 것들은 사용해보는 것도 비용이 많이 나오지 않아 좋다.)</li>
<li><code>키 페어</code> - 위에서 만든 키 페어 설정</li>
<li><code>네트워크 설정</code>은 설정된 것 말고 <code>편집</code><ul>
<li><code>본인의 VPC</code> 설정</li>
<li>서브넷은 <code>public01</code></li>
<li><code>퍼블릭 IP 자동 할당</code> : <code>활성화</code>로 변경</li>
<li><code>기존 보안 그룹 선택</code> : EC2 이므로 로드밸런서용 보안그룹 선택</li>
</ul>
</li>
</ol>
<p>위 과정을 하면 어느 정도 시간이 지난 뒤 생성이 되는데
들어가서 <code>퍼블릭 Ipv4 주소</code> 를 복사해준 뒤</p>
<p>윈도우라면 <code>윈도우 키</code> + R 을 눌러서 <code>cmd</code>로 명령 프롬프트 창을 열어</p>
<blockquote>
<p>ping {복사한 퍼블릭 Ipv4 주소}</p>
</blockquote>
<p>위 명령어를 입력하면 </p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bd9f717a-109b-4a61-b2a1-01e3d37adea2/image.png" /></p>
<p>위와 같이 잘 생성된 것을 볼 수 있을 것이다.</p>
<p>윈도우라면 mobaXterm을 활용해서 할 수도 있는데 필요하게 된다면 public subnet에 생성된 EC2를 확인했다면 삭제해야 하는 이유들도 함께 올려보도록 하겠다..</p>