<h1 id="운영체제">운영체제</h1>
<p>운영체제 (OS, Operating System) 은 간단하게 사용자가 컴퓨터를 쉽게 다루게 해주는 인터페이스로 한정된 메모리, 자원을 효율적으로 분배하는 것이다.</p>
<p>비슷한 것으로 <strong>펌웨어</strong> (firmware) 라는 것이 있는데 펌웨어는 소프트웨어를 추가로 설치할 수 없다는 차이가 있다.</p>
<hr />
<h1 id="운영체제와-컴퓨터">운영체제와 컴퓨터</h1>
<ul>
<li>하드웨어와 소프트웨어를 관리하는 운영체제</li>
<li>CPU, 메모리 등으로 이루어진 컴퓨터</li>
</ul>
<p>위 2가지를 알아보자.</p>
<h2 id="운영체제-역할과-구조">운영체제 역할과 구조</h2>
<h3 id="운영체제의-역할">운영체제의 역할</h3>
<p>운영체제의 역할을 알아보자.</p>
<ol>
<li>CPU 스케줄링, 프로세스 관리 : <strong>CPU 소유권을 어떤 프로세스에 할당할 지</strong>, 프로세스의 생성과 삭제, 자원 할당 및 반환 관리</li>
<li>메모리 관리 : <strong>한정된 메모리</strong>를 <strong>어떤 프로세스에</strong> <strong>얼만큼 할당</strong>해야 하는지 관리</li>
<li>디스크 파일 관리 : 어떠한 방법으로 디스크 파일을 보관할 지 관리</li>
<li>I/O 디바이스 관리 : I/O 디바이스 (마우스, 키보드와 같은) 와 컴퓨터 간 데이터 주고 받는 것 관리</li>
</ol>
<hr />
<h3 id="운영체제의-구조">운영체제의 구조</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/93f43564-3314-42eb-928e-c8bb39bc06b9/image.png" /></p>
<p>사진과 같이 맨 위에 소프트웨어 ~ 맨 아래에 하드웨어가 있는 구조인데, 중간에 표시한 부분이 운영체제 부분이다.</p>
<p>GUI가 없고 CUI만 있는 리눅스 서버도 있긴 하다.</p>
<blockquote>
<p>GUI (Graphical User Interface) : 아이콘을 마우스로 클릭하는 것 같이 명령어를 입력해서 상호작용하는 것이 아닌 단순한 동작으로 컴퓨터와 상호작용할 수 있는 사용자 인터페이스의 형태</p>
<p>드라이버 : 하드웨어를 제어하기 위한 소프트웨어</p>
<p>CUI : 명령어로 처리하는 인터페이스</p>
</blockquote>
<hr />
<h3 id="시스템-호출-시스템-콜">시스템 호출 (시스템 콜)</h3>
<blockquote>
<p>운영체제가 커널에 접근하기 위한 인터페이스</p>
<p>소프트웨어가 운영체제의 서비스를 받기 위해 커널 함수를 호출할 때 사용한다.</p>
</blockquote>
<p>소프트웨어가 I/O 요청으로 트랙을 발동하면 올바른 I/O 요청인지 확인 후 유저 모드였던 시스템 콜이 커널 모드로 변환돼 요청에 맞춰 파일 시스템에 영향을 준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/629f264b-4af9-4f52-a3be-e5d8ce1135f1/image.png" /></p>
<p><a href="https://c4u-rdav.tistory.com/85">참고 링크</a></p>
<blockquote>
<p>ex) <code>fs.readFile()</code>라는 파일 시스템 파일을 읽는 함수가 있다면 커널 모드로 읽고 유저 모드로 돌아가 소프트웨어의 로직 수행</p>
</blockquote>
<p>이 과정으로 컴퓨터 자원에 직접 접근하는 것을 차단할 수도 있고, 프로그램을 다른 프로그램으로부터 보호할 수 있다.</p>
<p>프로세스 or 스레드에서 OS로 어떠한 요청을 한다면 시스템 콜 인터페이스와 커널을 거쳐 운영체제에 전달된다.</p>
<p>시스템 콜은 하나의 추상화 계층이기 때문에 아래와 같은 장점이 있다.</p>
<p><strong>장점</strong></p>
<ul>
<li>네트워크 통신이나 DB같은 낮은 단계의 영역 처리에 대한 부분을 많이 신경쓰지 않고 프로그램 구현할 수 있다.</li>
</ul>
<hr />
<h4 id="modebit">modebit</h4>
<p>시스템 콜은 작동될 때 <code>modebit</code> 를 참고해서 유저 모드와 커널 모드를 구분한다.</p>
<p>0은 커널 모드, 1은 유저 모드로 무조건 이 플래그 변수로 작동해야 한다.</p>
<p>유저 모드를 기반으로 프로그램이 작동되면 공격자가 악의적인 행위를 할 수 있게 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4124224f-48df-4dc7-95d2-cb93d9ac96ee/image.png" /></p>
<p>ex) 카메라를 키는 프로그램을 예시로 든 사진이다.</p>
<hr />
<h2 id="컴퓨터하드웨어-의-요소">컴퓨터(하드웨어) 의 요소</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/732b30e4-c7f1-466b-b13e-0ea05ffd7fab/image.png" /></p>
<h3 id="cpu">CPU</h3>
<blockquote>
<p>CPU (Central Processing Unit) : 산술논리연산장치, 제어장치, 레지스터로 구성된 장치</p>
</blockquote>
<p>CPU는 인터럽트에 의해 메모리에 존재하는 명령어를 해석해서 실행하는 역할을 한다.</p>
<p>관리자의 역할을 하는 운영체제의 커널이 프로그램을 메모리에 올려 프로세스로 만들면 CPU가 처리한다.</p>
<h4 id="산술논리연산장치">산술논리연산장치</h4>
<blockquote>
<p>산술논리연산장치 (Arithmetic Logic Unit) : 덧셈, 뺄셈 같은 산술 연산과 배타적 논리합, 논리곱 같은 논리 연산을 계산하는 디지털 회로다.</p>
</blockquote>
<h4 id="제어장치-cu">제어장치 (CU)</h4>
<blockquote>
<p>제어장치 (Control Unit) : 프로세스 조작을 지시하는 부품
I/O 장치 간 통신을 제어하고 명령어를 읽고 해석하며 데이터 처리를 위한 순서를 결정</p>
</blockquote>
<h4 id="레지스터">레지스터</h4>
<blockquote>
<p>레지스터 : CPU 안에 있는 매우 빠른 임시 기억 장치
연산 속도가 메모리보다 월등히 빠르며, CPU에 자체적으로 데이터를 저장할 방법이 없어 레지스터를 거쳐 데이터를 전달한다.</p>
</blockquote>
<hr />
<h3 id="cpu-연산-처리">CPU 연산 처리</h3>
<p>위 장치들을 통해 연산하는 예시는 아래와 같다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5f56281b-2b20-4871-afa8-b9b48c8c9aec/image.png" /></p>
<p><a href="https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&amp;blogId=dufvndrnjs&amp;logNo=70152020258">참고 링크</a></p>
<ol>
<li>제어장치가 메모리와 레지스터에 계산할 값을 로드한다.</li>
<li>제어장치가 레지스터에 있는 값을 계산하라고 산술논리연산장치에 명령하면</li>
<li>제어장치가 계산된 값을 다시 레지스터 -&gt; 메모리로 계산한 값을 저장한다.</li>
</ol>
<h4 id="인터럽트">인터럽트</h4>
<p>어떤 신호가 왔을 때 CPU를 잠깐 정지시키는 것을 말한다.</p>
<p>인터럽트가 발생하면 인터럽트 핸들러 함수가 모여있는 인터럽트 벡터로 가서 핸들러 함수를 실행한다.</p>
<p>물론 인터럽트 간에는 우선순위가 있어서 그것에 따라 실행되고, 하드웨어/소프트웨어 인터럽트 2가지가 있따.</p>
<blockquote>
<p>인터럽트 핸들러 함수 : 인터럽트가 발생할 때 이를 핸들링하기 위한 함수</p>
</blockquote>
<hr />
<h3 id="하드웨어-인터럽트">하드웨어 인터럽트</h3>
<blockquote>
<p>키보드를 연결하거나 마우스를 연결하는 것처럼 I/O 디바이스에서 발생하는 인터럽트</p>
</blockquote>
<hr />
<h3 id="소프트웨어-인터럽트">소프트웨어 인터럽트</h3>
<blockquote>
<p><strong>트랩</strong>이라고도 하며 프로세스 오류 등으로 프로세스가 시스템콜을 호출할 때 발동한다.</p>
</blockquote>
<h4 id="dma-컨트롤러">DMA 컨트롤러</h4>
<p>I/O 디바이스가 메모리에 직접 접근할 수 있도록 하는 하드웨어 장치를 뜻한다.</p>
<p>CPU에 너무 많은 인터럽트 요청이 들어오는데 그로 인한 부하를 DMA 컨트롤러가 막아준다.</p>
<ul>
<li>CPU와 DMA 컨트롤러가 동시에 같은 작업을 하는 것도 방지한다.</li>
</ul>
<hr />
<h4 id="메모리">메모리</h4>
<blockquote>
<p>전자회로에서 데이터나 상태, 명령어 등을 기록하는 장치</p>
</blockquote>
<p>보통 RAM을 메모리라고도 한다.</p>
<p>CPU 가 계산을 한다면, 메모리는 기억을 담당한다.</p>
<hr />
<h4 id="타이머">타이머</h4>
<blockquote>
<p>특정 프로그램에 시간 제한을 다는 역할</p>
</blockquote>
<hr />
<h4 id="디바이스-컨트롤러">디바이스 컨트롤러</h4>
<blockquote>
<p>컴퓨터와 연결돼 있는 I/O 디바이스들의 작은 CPU라고 보면 된다.
옆에 붙어 있는 <strong>로컬 버퍼</strong>는 각 디바이스에서 데이터를 임시로 저장하기 위한 작은 메모리 역할이다.</p>
</blockquote>