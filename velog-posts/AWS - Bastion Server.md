<h1 id="bastion-server">Bastion Server</h1>
<p>Bastion Server 라는 개념이 있다.</p>
<blockquote>
<p>Private Subnet에 있는 서버들에 접근하기 위한 중계 서버
예시를 들면 경비실과 같이 모든 관리자는 Bastion Server를 거쳐 내부 서버에 접근해야 한다.</p>
</blockquote>
<p>SSH를 통해 안전한 접근을 제공하며, Public Subnet에 위치한다.</p>
<p>Private Subnet에 접근하기 위해서는 Bastion Server(= Bastion Host, Jump Server)가 필요하다. Public Subnet에 위치하면서 접근이 허용된 관리자들은 접근할 수 있게끔 해준다.</p>
<p>보안 그룹을 활용해서 접근을 제한하고, 내부 리소스에 대한 안전한 관리 채널을 제공해준다.</p>
<p>이 Bastion Server를 통해 접근 기록을 남기고 보안을 강화할 수 있다.</p>
<p>바로 이전 게시글에서 이미 Bastion Server 용 보안그룹을 만들었으니 참고하는 것도 좋다.</p>
<blockquote>
<p><a href="https://velog.io/@jojehuni_9759/AWS-Route-Table">Bastion Server 용 보안그룹 참고</a></p>
</blockquote>
<hr />
<h2 id="bastion-server-생성">Bastion Server 생성</h2>
<p>위에서 참고용 게시글을 보면 EC2를 만드는 것도 포함돼 있는데 이걸 활용해서 Bastion Server용 EC2를 만들 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1ab373e1-5600-4211-8cc0-de8a84b5708c/image.png" /></p>
<p>점프 서버 하나를 하나의 public subnet에 만들면 해당 public subnet과 매핑 되는 private subnet 뿐 아니라 또 다른 public subnet과 매핑 되는 private subnet까지 연동 (경유하게)해 준다. (내부 인터넷 망을 사용하기 때문)</p>
<p>위의 사진에는 해당 Bastion Server를 통해 Public Subnet 1번을 통해 1번 Private Subnet만 가는 것처럼 보이더라도 2번 Private Subnet에도 경유가 가능하다는 의미다.</p>
<p>한 번 만들어보자.</p>
<ol>
<li>AWS EC2 - <code>인스턴스 생성</code></li>
<li>이름 : <code>bastion</code> (편한대로 작성해도 된다. 알아볼 수 있어야 한다.)</li>
<li>이전 게시글과 마찬가지로 <code>Amazon Linux 2023 AMI</code></li>
<li>키 페어 : <code>만든 키 페어</code>로 설정</li>
<li>VPC : <code>만든 VPC</code>로</li>
<li>Subnet : <code>public01</code> 서브넷으로 설정 (내가 만들었다면 intbyte-subnet-public01 을 선택했을 것)</li>
<li><code>퍼블릭 IP 자동 할당</code> : <strong>활성화</strong></li>
<li><code>기존 보안 그룹 선택</code> -&gt; <code>intbyte-sg-bastion</code> 선택 후 생성</li>
</ol>
<p>이것 또한 저번 게시글에 이어서 mobaXterm 으로 확인할 수 있는데 필요하다면 추가로 하나의 게시글로 작성하겠다.</p>