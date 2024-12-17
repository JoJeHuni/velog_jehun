<p>이전 게시글에서 사용자를 만들어봤으니</p>
<p>VPC에 대해서도 알아보자.</p>
<h1 id="vpc-virtual-private-cloud">VPC (Virtual Private Cloud)</h1>
<p><strong>VPC</strong> : 클라우드 상에서 만드는 <strong>사용자 전용 네트워크</strong></p>
<p>사용자 전용 네트워크라는 것이 중요하다.</p>
<p>회사 건물을 임대해 내부 구조를 마음대로 설계하는 것처럼 AWS에서 제공하는 가상의 데이터 센터를 원하는 대로 구성할 수 있다.</p>
<p>기본적으로 Region 별로 VPC를 구성한다.
사용자가 직접 IP 주소 범위를 설정할 수 있으며, <code>서브넷</code> 구성과 <code>라우팅 테이블</code>, <code>네트워크 게이트웨이</code> 등을 통해 완벽한 네트워크 제어가 가능하다. 
네트워크 ACL을 통해 네트워크 접근을 세밀하게 통제할 수 있다.</p>
<blockquote>
<p>그래서 다른 Region에 VPC를 만들고 까먹어버리면 해당 VPC에서 계속 결제가 된다.</p>
</blockquote>
<hr />
<h2 id="nacl과-보안그룹">NACL과 보안그룹</h2>
<h3 id="naclnetwork-access-control-list">NACL(Network Access Control List)</h3>
<p><strong>서브넷</strong> 수준에서 작동하는 방화벽 역할을 하는 보안 계층</p>
<ul>
<li>인바운드 (접속하는 것)와 아웃바운드 (나가는 것) 규칙을 각각 따로 설정하는 것<ul>
<li>'허용', '거부' 모두 명시적으로 설정 가능</li>
</ul>
</li>
</ul>
<h3 id="보안그룹security-group">보안그룹(Security Group)</h3>
<p><strong>인스턴스</strong> 수준에서 작동하는 방화벽 역할을 하는 보안 계층</p>
<ul>
<li><strong>인바운드 규칙을 허용하면</strong> 관련된 아웃바운드 트래픽은 <strong>자동 허용</strong><ul>
<li>‘허용’ 규칙만 설정 가능(명시적으로 설정되지 않은 것은 모두 거부)</li>
</ul>
</li>
</ul>
<blockquote>
<p><strong>VPC 내 트래픽 흐름 패턴</strong></p>
<p><strong>1. Public Subnet으로의 접근 흐름:</strong>
인터넷 → IGW → VPC NACL → Public Subnet NACL → Public Subnet 보안그룹 → EC2 인스턴스</p>
<p><strong>2. Private Subnet으로의 접근 흐름(인터넷으로부터):</strong>
인터넷 → IGW → VPC NACL → Public Subnet NACL → NAT Gateway 보안그룹 → NAT Gateway → Private Subnet NACL → Private Subnet 보안그룹 → EC2 인스턴스</p>
<p><strong>3. Bastion Host를 통한 Private Subnet 접근 흐름:</strong>
인터넷 → IGW → VPC NACL → Public Subnet NACL → Bastion Host 보안그룹 → Bastion Host → Private Subnet NACL → Private Subnet 보안그룹 → EC2 인스턴스</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4736c91b-2071-439c-bb32-43e4bec3a10a/image.png" /></p>
<hr />
<p>위 사진과 같고 아래 표와 같은 설정으로 만들어보면</p>
<table>
<thead>
<tr>
<th><strong>항목</strong></th>
<th><strong>값</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td>이름 태그</td>
<td>{본인 원하는 VPC 명}</td>
<td>VPC를 식별하는 이름</td>
</tr>
<tr>
<td>IPv4 CIDR 블록</td>
<td>10.0.0.0/16</td>
<td>VPC에서 이용하는 프라이빗 네트워크의 IPv4주소 범위</td>
</tr>
<tr>
<td>IPv6 CIDR 블록</td>
<td>IPv6 CIDR 블록 없음</td>
<td>VPC에서 이용하는 프라이빗 네트워크의 IPv6주소 범위</td>
</tr>
<tr>
<td>테넌시(tenancy)</td>
<td>기본값</td>
<td>VPC 리소스의 전용 하드웨어에서의 실행 여부</td>
</tr>
</tbody></table>
<hr />
<h3 id="vpc-생성">VPC 생성</h3>
<p>실제로 만들지는 않았지만 이미 이전에 진행했던 방식대로 정리해보려고 한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2ba362f2-1ac5-48c5-83ed-99ad0d099b41/image.png" /></p>
<hr />
<h3 id="internet-gateway-생성">Internet Gateway 생성</h3>
<ol>
<li>인터넷 게이트웨이 생성</li>
<li>이름 태그 설정</li>
<li>생성</li>
</ol>
<p>상단에 생성되었음을 알리며 VPC에 연결을 할 수 있다.
위에서 만든 VPC에 연결하자.</p>