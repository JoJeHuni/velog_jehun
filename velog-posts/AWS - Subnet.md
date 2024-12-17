<h1 id="subnet">Subnet</h1>
<blockquote>
<p>VPC라는 큰 네트워크를 용도에 맞게 더 작게 나눈 단위</p>
</blockquote>
<p>간단하게 VPC가 아파트라면 Subnet은 각 층, 구역 으로 나누는 것과 비슷하다고 생각하면 된다.</p>
<p><strong>Subnet</strong></p>
<ul>
<li>외부 인터넷과 연결되는 <strong>Public Subnet</strong></li>
<li>외부 인터넷과 연결 되진 않지만 내부 인터넷 망을 통해 접근 가능한 <strong>Private Subnet</strong></li>
</ul>
<p>위 2가지로 나눌 수 있다.</p>
<blockquote>
<p><strong>Public Subnet</strong> : 외부인 출입 가능한 아파트 공용 로비
<strong>Private Subnet</strong> : 입주민만 접근 가능한 사적 공간</p>
</blockquote>
<p>또한 <strong>가용 영역(AZ)</strong> 별로 구성해 고가용성을 확보할 수 있고, 각 서브넷은 독립적인 네트워크 정책을 가질 수 있다.</p>
<blockquote>
<p><strong>가용 영역(AZ, Availability Zone)이란?</strong>
각 AZ는 Region 내에 물리적으로 분리된 데이터 센터이다.
(Seoul Region은 a, b, c, d 네 개의 AZ가 존재한다.)</p>
</blockquote>
<hr />
<h2 id="subnet-구성">Subnet 구성</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c638d354-0c0a-4da1-94fa-d890c9ad14fb/image.jpg" /></p>
<table>
<thead>
<tr>
<th><strong>서브넷</strong></th>
<th><strong>CIDR 블록</strong></th>
</tr>
</thead>
<tbody><tr>
<td>public01</td>
<td>00001010.00000000.0000XXXX.XXXXXXXX (10.0.0.0/20)</td>
</tr>
<tr>
<td>public02</td>
<td>00001010.00000000.0001XXXX.XXXXXXXX (10.0.16.0/20)</td>
</tr>
<tr>
<td>private01</td>
<td>00001010.00000000.0100XXXX.XXXXXXXX (10.0.64.0/20)</td>
</tr>
<tr>
<td>private02</td>
<td>00001010.00000000.0101XXXX.XXXXXXXX (10.0.80.0/20)</td>
</tr>
</tbody></table>
<p>위와 같이 해서 아래에 만들어보자.</p>
<table>
<thead>
<tr>
<th><strong>대상</strong></th>
<th><strong>항목</strong></th>
<th><strong>값</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>외부 서브넷 1</strong></td>
<td>VPC ID</td>
<td>VPC의 ID</td>
<td>서브넷을 생성할 VPC</td>
</tr>
<tr>
<td></td>
<td>서브넷 이름</td>
<td>intbyte-subnet-public01</td>
<td>서브넷별 이름</td>
</tr>
<tr>
<td></td>
<td>가용 영역</td>
<td>ap-northeast-2a</td>
<td>제공되는 가용 영역</td>
</tr>
<tr>
<td></td>
<td>IPv4 CIDR 블록</td>
<td>10.0.0.0/20</td>
<td>VPC의 IP범위에 포함되는 범위</td>
</tr>
<tr>
<td><strong>외부 서브넷 2</strong></td>
<td>VPC ID</td>
<td>VPC의 ID</td>
<td>서브넷을 생성할 VPC</td>
</tr>
<tr>
<td></td>
<td>서브넷 이름</td>
<td>intbyte-subnet-public02</td>
<td>서브넷별 이름</td>
</tr>
<tr>
<td></td>
<td>가용 영역</td>
<td>ap-northeast-2c</td>
<td>제공되는 가용 영역</td>
</tr>
<tr>
<td></td>
<td>IPv4 CIDR 블록</td>
<td>10.0.16.0/20</td>
<td>VPC의 IP범위에 포함되는 범위</td>
</tr>
<tr>
<td><strong>내부 서브넷 1</strong></td>
<td>VPC ID</td>
<td>VPC의 ID</td>
<td>서브넷을 생성할 VPC</td>
</tr>
<tr>
<td></td>
<td>서브넷 이름</td>
<td>intbyte-subnet-private01</td>
<td>서브넷별 이름</td>
</tr>
<tr>
<td></td>
<td>가용 영역</td>
<td>ap-northeast-2a</td>
<td>제공되는 가용 영역</td>
</tr>
<tr>
<td></td>
<td>IPv4 CIDR 블록</td>
<td>10.0.64.0/20</td>
<td>VPC의 IP범위에 포함되는 범위</td>
</tr>
<tr>
<td><strong>내부 서브넷 2</strong></td>
<td>VPC ID</td>
<td>VPC의 ID</td>
<td>서브넷을 생성할 VPC</td>
</tr>
<tr>
<td></td>
<td>서브넷 이름</td>
<td>intbyte-subnet-private02</td>
<td>서브넷별 이름</td>
</tr>
<tr>
<td></td>
<td>가용 영역</td>
<td>ap-northeast-2c</td>
<td>제공되는 가용 영역</td>
</tr>
<tr>
<td></td>
<td>IPv4 CIDR 블록</td>
<td>10.0.80.0/20</td>
<td>VPC의 IP범위에 포함되는 범위</td>
</tr>
</tbody></table>
<blockquote>
<p>난 이전에 만들었던 것들을 삭제해서 참고용으로 작성</p>
</blockquote>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/32a96c71-466f-4f83-b053-26ee91526d3a/image.png" /></p>
<p>VPC ID 에서 이전 게시글에서 만들었던 VPC를 선택해준 뒤</p>
<p>위 표와 같이 서브넷을 추가해가며 만들면 된다.</p>
<blockquote>
<p>주의</p>
</blockquote>
<ul>
<li>가용 영역 a, c 잘 맞춰서 만들기</li>
</ul>
<p>-&gt; 블루/그린 배포를 위해 하는 것이다. 
추가로 로드밸런서 또한 만들기 위해서는 최소 2개의 서브넷이 필요하다.</p>
<p>다 만들면 4개의 서브넷이 생길 것이다.</p>
<p>무엇보다 만들어보는 것은 직접 해보는게 제일 중요하니 프리티어를 활용해서 만들어보고 배포까지 해보는 것도 경험적으로 좋다고 생각한다.</p>