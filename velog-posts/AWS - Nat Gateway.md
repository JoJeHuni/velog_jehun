<h1 id="nat-gateway">Nat Gateway</h1>
<blockquote>
<p>Private Subnet에 위치한 서버가 외부 인터넷과 통신해야 할 때 사용하는 중계 서비스</p>
</blockquote>
<ul>
<li><strong>내부 -&gt; 외부</strong>로 나가는 것은 <strong>허용</strong></li>
<li><strong>외부 -&gt; 내부</strong>로 들어오는 것은 <strong>차단</strong></li>
</ul>
<p>Private Subnet의 서버가 보안 업데이트를 다운로드 해야하거나 외부 API를 호출해야 할 때 NAT Gateway를 통해 안전하게 통신할 수 있다. 또한 Nat Gateway는 public subnet에 두게 된다.</p>
<p>고가용성을 위해 각 AZ마다 구성하는 것이 권장, 트래픽 양에 따라 자동으로 확장되어 별도의 관리가 필요하지 않다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/00abfa0f-4096-4535-bb18-6f14295e49fd/image.png" /></p>
<p>이전에 만들었던 것을 포함해 public subnet이 2개이므로 nat gateway도 2개를 만들어야 한다.</p>
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
<td><strong>NAT 게이트웨이 1</strong></td>
<td>이름</td>
<td>intbyte-ngw-01</td>
<td>NAT 게이트웨이 이름</td>
</tr>
<tr>
<td></td>
<td>서브넷</td>
<td>intbyte-subnet-public01</td>
<td>NAT 게이트웨이를 생성할 서브넷</td>
</tr>
<tr>
<td></td>
<td>탄력적 IP 할당 ID</td>
<td>(자동 생성)</td>
<td>NAT 게이트웨이에 할당할 탄력적 IP</td>
</tr>
<tr>
<td><strong>NAT 게이트웨이2</strong></td>
<td>이름</td>
<td>intbyte-ngw-02</td>
<td>NAT 게이트웨이 이름</td>
</tr>
<tr>
<td></td>
<td>서브넷</td>
<td>intbyte-subnet-public02</td>
<td>NAT 게이트웨이를 생성할 서브넷</td>
</tr>
<tr>
<td></td>
<td>탄력적 IP 할당 ID</td>
<td>(자동생성)</td>
<td>NAT 게이트웨이에 할당할 탄력적 IP</td>
</tr>
</tbody></table>
<hr />
<p>VPC로 가게 되면 왼쪽 탭 중에 <code>NAT 게이트웨이</code> 를 확인할 수 있다.</p>
<ol>
<li>NAT 게이트웨이 생성</li>
<li>각각 맞춰서 이름, 서브넷 선택</li>
<li>탄력적 IP 할당 ID 에서 <code>탄력적 IP 할당</code> 버튼 클릭</li>
<li>생성</li>
</ol>
<p>위 과정을 2번 해서 public subnet에 맞게 만들어주면 끝이다.</p>