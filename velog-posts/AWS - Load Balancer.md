<h1 id="application-load-balanceralb">Application Load Balancer(ALB)</h1>
<blockquote>
<p>웹 애플리케이션으로 들어오는 트래픽을 여러 서버에 분배하는 서비스이다.
L4(전송 계층) 또는 L7(애플리케이션 계층) 수준에서 작동하여 여러 대상 그룹으로 트래픽을 분산할 수 있다. 또한 SSL/TLS 종료 및 HTTPS 리다이렉션을 지원한다.</p>
</blockquote>
<p>지금 이전 게시글로부터 만들어진 서브넷들을 보면 라우팅 테이블과 bastion server를 통해 내부에서는 접근은 할 수 있게 해놨지만 외부로부터 접근은 할 수가 없는 상태이다.</p>
<p>이것을 로드 밸런서를 통해 사용자들은 특정 public subnet으로 접근이 가능하게 할 수 있는데 그것을 알아볼 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/43d60e40-66d7-4ee3-9f71-65ecd3b43fa0/image.png" /></p>
<blockquote>
<p>여기서부터는 EC2가 있다는 가정으로 적어둔 것으로, 직접 EC2를 만들어서 해보거나 ELB 게시글에서 Elastic Beanstalk을 만드는 것을 보고 해본 뒤 아래 과정을 진행하는 것을 추천한다.</p>
</blockquote>
<p>우선 로드밸런서용 대상 그룹을 만드는 과정을 알아보자.</p>
<h2 id="alb용-대상그룹-생성">ALB용 대상그룹 생성</h2>
<ol>
<li>AWS EC2로 이동</li>
<li>왼쪽 탭의 <code>대상 그룹</code> 선택 -&gt; <code>대상 그룹 생성</code> 버튼 클릭</li>
<li>대상 유형은 <code>인스턴스</code></li>
<li>대상 그룹 이름은 편한대로 작성하면 된다.</li>
<li>프로토콜 : 포트 - HTTP / 80 으로 그대로 진행</li>
<li>VPC는 이전에 만들었던 VPC로 설정한 뒤</li>
<li>대상 등록 -&gt; EC2 인스턴스를 체크</li>
</ol>
<blockquote>
<p>근데 지금은 EC2를 만들지 않았으므로 다음 게시글로 Elastic Beanstalk 으로 백엔드용 환경을 만들 것이니 그 때 설정하자.</p>
<p>나중에 백엔드용 환경 2개를 만들 것이니 대상 등록에서 그 해당 ELB의 EC2 2개를 설정해주면 된다.</p>
</blockquote>
<ol start="8">
<li>체크한 뒤 <code>아래에 보류 중인 것으로 포함</code> 클릭</li>
<li><code>대상 그룹 생성</code></li>
<li>조금 기다리면 Health Check가 된다.</li>
</ol>
<blockquote>
<p>Health Check 관련해서도 백엔드에서 Health Check 용 api 를 만들거나 해야 한다.
이것 관련해서도 ELB 한 뒤 배포할 때 한 번에 추가 설명하겠다.
정상이 아니어도 배포는 됐던 경험이 있다.</p>
</blockquote>
<h2 id="alb-생성">ALB 생성</h2>
<ol>
<li>AWS EC2 - 로드밸런서 - <code>로드밸런서 생성</code></li>
<li>로드밸런서 유형 : <code>Application Load Balancer</code> 선택</li>
</ol>
<blockquote>
<p>https 를 하기 위해 유형은 위와 같이 선택했다.</p>
</blockquote>
<ol start="3">
<li>이름은 편하게 작성</li>
<li>체계 - <code>인터넷 경계</code> -&gt; 외부 노출 여부를 나타내는 것이기 때문에</li>
<li>네트워크 매핑 - VPC : 기존에 만들었던 VPC로 진행</li>
</ol>
<p>5단계를 하면 아래에 가용 영역으로  <code>ap-northeast-2a</code>와 <code>ap-northeast-2c</code>가 보이는데 둘다 public subnet으로 설정해주면 된다.</p>
<ol start="6">
<li><p>보안 그룹 : default로 돼 있는 것에 + 만들었던 <code>intbyte-sg-elb</code> 형태의 로드밸런서용 대상그룹 설정</p>
</li>
<li><p>리스너 및 라우팅 : HTTP : 80 에 대상 그룹은 위에서 로드밸런서용 대상 그룹으로 설정</p>
</li>
<li><p>생성</p>
</li>
</ol>
<p>아직은 <code>Route 53</code> 설정도 해두지 않아서 <code>https</code>도 설정이 돼 있지 않고, 로드밸런서 DNS 이름을 통해서만 작동하는지 확인할 수 있을 것이다.</p>
<blockquote>
<p>물론 백엔드를 연결했을 때의 이야기지만 연결하는 것은 ELB를 만들면서 했던 과정을 포함해서 작성하겠다.</p>
</blockquote>