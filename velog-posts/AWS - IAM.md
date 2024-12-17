<p>AWS 에서 회원가입을 하게 되면 Root 계정으로 생성이 된다.</p>
<p>이 Root 계정은 모든 권한을 갖고 있는데, 보안적으로 문제가 발생할 수도 있기 때문에 IAM 에서 <code>사용자 계정 및 그룹</code> 을 생성해 권한을 제어하는 기능을 제공을 해준다.</p>
<hr />
<h1 id="iamidentity-and-access-management">IAM(Identity and Access Management)</h1>
<p>AWS의 모든 서비스와 리소스에 대한 접근을 관리하는 보안의 핵심으로</p>
<ul>
<li>사용자, 그룹, 역할을 생성하고 관리할 수 있다.<ul>
<li>개발팀은 개발 서버에만 접근</li>
<li>운영팀은 운영 서버에만 접근</li>
</ul>
</li>
</ul>
<p>위와 같이 최소 권한 원칙 (Principle of Least Priviledge) 을 적용해 보안을 강화하고, 비밀번호 외 2차 인증 (MFA - Multi-Factor Authentication)을 추가해 더욱 강화할 수 있다.</p>
<hr />
<h2 id="사용자-생성">사용자 생성</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/400341fd-4433-49b2-b9e9-1292d7f12ba3/image.jpg" /></p>
<p>이름과 사용자 지정 암호를 입력한 뒤, 새 암호를 생성하는 것도 권장하기는 하지만 반복적이지 않게끔 해제를 해줘도 괜찮다.</p>
<p>위와 같이 해서 본인의 사용자를 생성한 뒤, 권한을 추가해주자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fa7aea8e-c64d-482a-ba1a-678138ee0c8b/image.png" /></p>
<p>위와 같이 권한을 추가해준 뒤 생성을 할 수 있다.</p>
<hr />
<h2 id="mfa-설정">MFA 설정</h2>
<p>생성된 사용자를 들어가보면 아래와 같은 경고가 떠 있다.</p>
<ul>
<li><code>콘솔 액세스</code> : MFA 없이 활성화됨</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/578c9539-0f4d-4559-816e-80db6c0cde42/image.png" /></p>
<p><code>보안 자격 증명</code> 을 통해 <code>MFA 디바이스 할당</code>을 눌러</p>
<ol>
<li>디바이스 이름 입력</li>
<li>디바이스 옵션 - 인증 관리자 앱 - Authenticator app - QR 코드 표시</li>
<li>Google Authenticator 설치 후 QR을 통해 입력 후 30초 간격으로 2번 입력</li>
</ol>
<p>위 세 가지 순서대로 진행하면 MFA 설정이 가능해져서 2차 인증할 때 사용할 수 있다.</p>
<hr />
<h2 id="액세스-키-설정">액세스 키 설정</h2>
<p>액세스 키도 사용자에 맞춰 생성해줘야 한다.</p>
<ul>
<li>액세스 키 만들기</li>
<li>AWS 외부에서 실행되는 애플리케이션</li>
<li>태그 (선택)</li>
<li>액세스 키, 비밀 액세스 키 확인 <strong>(반드시 저장해둘 것)</strong></li>
</ul>
<p>이제 루트 계정으로 로그인하지 않고 IAM 계정으로도 가능하다.</p>