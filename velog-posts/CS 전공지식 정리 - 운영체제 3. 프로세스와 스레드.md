<p><strong>프로세스</strong> : 컴퓨터에서 실행되고 있는 프로그램
CPU 스케줄링의 대상이 되는 작업이라는 용어와 거의 같은 의미로 쓰인다.</p>
<p><strong>스레드</strong> : 프로세스 내 작업의 흐름</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c6381944-ed1a-492d-a659-b4ec4508793b/image.png" />
프로그램이 메모리에 올라가면 인스턴스화된 것이 프로세스이다.
운영체제의 CPU 스케줄러에 따라 CPU가 프로세스를 실행한다.</p>
<hr />
<h2 id="프로세스와-컴파일-과정">프로세스와 컴파일 과정</h2>
<blockquote>
<p><strong>컴파일(Compile)</strong> : 소스코드를 컴퓨터가 이해할 수 있는 기계어로 번역하여 실행할 수 있는 파일을 만드는 작업</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/96c1a806-27f9-4a80-bf3f-c9fd5b304947/image.png" /></p>
<p>위는 프로그램 컴파일 과정 사진이다.</p>
<ul>
<li><p>전처리</p>
<ul>
<li>소스 코드의 주석 제거, #include 등 헤더 파일을 병합하여 매크로를 치환</li>
</ul>
</li>
<li><p>컴파일러</p>
<ul>
<li>오류 처리, 코드 최적화 작업, 어셈블리어로 변환</li>
</ul>
</li>
<li><p>어셈블러</p>
<ul>
<li>어셈블리어를 목적 코드로 변환</li>
</ul>
</li>
<li><p>링커</p>
<ul>
<li>프로그램 내에 있는 라이브러리 함수 또는 다른 파일들과 목적 코드로 결합하여 실행 파일을 만듦</li>
<li>실행 파일의 확장자는 .exe또는 .out</li>
</ul>
</li>
</ul>
<hr />
<h2 id="프로세스의-상태">프로세스의 상태</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/221d3c82-57c3-47e6-a7e9-e2132ea25568/image.png" /></p>
<h3 id="생성create">생성(create)</h3>
<p>프로세스가 생성된 상태를 의미하며, <code>fork()</code> 또는 <code>exec()</code> 함수를 통해 생성 -&gt; 이 때 PCB가 할당된다.</p>
<blockquote>
<p><strong>PCB(Process Control Block)</strong>?</p>
<ul>
<li>프로세스의 메타데이터들이 저장되어 관리되는 영역</li>
<li>프로세스의 중요한 정보를 포함하고 있기 때문에 일반 사용자가 접근하지 못하도록 커널 스택의 가장 앞부분에서 관리됨</li>
<li>프로세스 스케줄링 상태, 프로세스 ID 정보등을 관리</li>
</ul>
</blockquote>
<p>PCB에 대해서는 아래에서 더 다룰 것이다.</p>
<h3 id="대기ready">대기(ready)</h3>
<p>메모리 공간이 충분하면 메모리를 할당 받고, 아니면 아닌 상태로 대기하고 있으며 CPU 스케줄러부터 CPU 소유권이 넘어오기를 기다리는 상태</p>
<h3 id="대기-중단ready-suspended">대기 중단(ready suspended)</h3>
<p>메모리 부족으로 일시 중단된 상태</p>
<h3 id="실행running">실행(running)</h3>
<p>CPU 소유권과 메모리를 할당받고 인스트럭션을 수행 중인 상태</p>
<h3 id="중단blocked">중단(blocked)</h3>
<p>어떤 이벤트가 발생한 이후 기다리며 프로세스가 차단된 상태
ex) 프린트 인쇄 버튼을 눌렀을 때 프로세스가 잠깐 멈추는 현상</p>
<h3 id="일시중단blocked-suspended">일시중단(blocked suspended)</h3>
<p>중단된 상태에서 프로세스가 실행되려고 했지만 메모리 부족으로 일시 중단된 상태</p>
<h3 id="종료terminated">종료(terminated)</h3>
<p>메모리와 CPU 소유권을 모두 놓고 종료되는 상태</p>
<hr />
<h2 id="프로세스의-메모리-구조">프로세스의 메모리 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/641e77eb-b818-4f46-b66f-1e5486f1003f/image.png" /></p>
<p>프로세스의 메모리 구조 사진이다.</p>
<h3 id="스택-힙">스택, 힙</h3>
<p>스택과 힙은 동적 할당이 되며, <code>동적 할당</code>이란 <strong>런타임 단계에서 메모리를 할당받는 것</strong>을 말한다.</p>
<ul>
<li><p><strong>스택</strong> : 지역 변수, 매개 변수, 실행되는 함수에 의해 늘거나 줄어드는 메모리 영역이다. 함수가 호출될 때마다 호출될 때의 환경 등 특정 정보가 스택에 게속해서 저장된다.</p>
</li>
<li><p><strong>힙</strong> : 동적으로 할당되는 변수들을 담는다. 동적으로 관리되는 자료 구조의 경우 힙 영역을 사용한다. 예를 들어 vector는 내부적으로 힙 영역을 사용한다.</p>
</li>
</ul>
<h3 id="데이터-영역과-코드-영역">데이터 영역과 코드 영역</h3>
<p>데이터 영역과 코드 영역은 정적 할당되는 영역이다.
<code>정적 할당</code> 은 컴파일 단계에서 메모리를 할당하는 것을 말한다. 여기서 데이터 영역은 BBS segment와 Data segment, code/text segment로 나뉘어서 저장하게 된다.</p>
<ul>
<li><p>BBS segment 는 전역 변수 또는 static, const로 선언되어 있고, 0으로 초기화 또는 초기화가 어떤 값으로도 되어 있지 않은 변수들이 이 메모리 영역에 할당된다.</p>
</li>
<li><p>Data segment 는 전역 변수 또는 static, const로 선언되어 있고 0이 아닌 값으로 초기화된 변수를 이 메모리에 할당한다.</p>
</li>
<li><p>code segment 는 프로그램의 코드가 들어간다.</p>
</li>
</ul>
<hr />
<h2 id="pcb">PCB</h2>
<p>위에서 일부 작성했던 PCB에 대해서 더 다뤄보자.</p>
<blockquote>
<p><strong>PCB(Process Control Block)</strong>?</p>
<ul>
<li>프로세스의 메타데이터들이 저장되어 관리되는 영역</li>
<li>프로세스의 중요한 정보를 포함하고 있기 때문에 일반 사용자가 접근하지 못하도록 커널 스택의 가장 앞부분에서 관리됨</li>
<li>프로세스 스케줄링 상태, 프로세스 ID 정보등을 관리</li>
</ul>
</blockquote>
<h3 id="pcb의-구조">PCB의 구조</h3>
<ul>
<li>프로세스 스케줄링 상태: '준비','일시중단'등 프로세스가 CPU에 대한 소유권을 얻은 이후의 상태</li>
<li>프로세스 ID: 프로세스 ID, 해당 프로세스의 자식 프로세스 ID</li>
<li>프로세스 권한: 컴퓨터 자원 또는 I/O 디바이스에 대한 권한 정보</li>
<li>프로그램 카운터: 프로세스에서 실행해야 할 다음 명령어의 주소에 대한 포인터</li>
<li>CPU 레지스터: 프로세스를 실행하기 위해 저장해야 할 레지스터에 대한 정보</li>
<li>CPU 스케줄링 정보: CPU 스케줄러에 의해 중단된 시간 등에 대한 정보</li>
<li>계정 정보: 프로세스 실행에 사용된 CPU 사용량, 실행한 유저의 정보</li>
<li>I/O 상태 정보: 프로세스에 할당된 I/O 디바이스 목록</li>
</ul>
<hr />
<h3 id="컨텍스트-스위칭">컨텍스트 스위칭</h3>
<p>PCB를 기반으로 프로세스의 상태를 저장하고 로드시키는 과정으로, PCB를 교환하는 과정이라고 보면 된다.</p>
<p>한 프로세스에 할당된 시간이 끝나거나 인터럽트에 의해 발생한다.</p>
<p>컴퓨터는 여러 프로그램을 동시에 실행하는 것이 아니라 단 한 개만을 실행하고 있다.</p>
<ul>
<li>이 또한 현대는 멀티코어의 CPU기 때문에 지금이랑은 다르다.</li>
</ul>
<p>싱글코어라고 생각하고 보자.</p>
<p>싱글코어일 때도 우리가 여러 개가 동시에 돌아간다고 생각하고 있는 이유는 이 컨텍스트 스위칭이 매우 빠르게 실행되고 있기 때문이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5f0919a6-5700-44cd-b90d-4abb5d2fdaae/image.png" /></p>
<p>A프로세스가 실행하다 멈추고 A의 PCB를 저장하고 B프로세스를 로드하여 실행한다.</p>
<p>그리고 B PCB에 저장하고 A PCB를 다시 불러오는 방식이다.
이때 컨텍스트 스위칭이 일어날 때 대기하는 유효기간이 발생한다.</p>
<p>이뿐만 아니라 더 드는 비용이 있는데, 바로 <code>캐시미스</code>이다.
-&gt; 프로세스가 가지고 있는 메모리 주소가 그대로 있으면 잘못된 주소 변환이 생기므로 캐시클리어 과정을 겪게 되는데, 이때 <code>캐시미스</code> 가 발생한다.</p>
<hr />
<h2 id="멀티프로세싱">멀티프로세싱</h2>
<blockquote>
<p>프로세스를 여러 개 수행하는 것</p>
</blockquote>
<p><strong>장점</strong></p>
<ul>
<li>하나 이상의 일을 병렬로 처리할 수 있다.</li>
<li>특정 프로세스의 메모리, 프로세스 중 문제가 생겨도 다른 프로세스로 처리 가능해 신뢰성이 높다.</li>
</ul>
<h3 id="웹-브라우저">웹 브라우저</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ddb28ea7-0a22-46a6-864a-d35d11ab59cc/image.png" /></p>
<ol>
<li>브라우저 프로세스: 주소 표시줄, 북마크 막대, 뒤로 가기 버튼, 앞으로 가기 버튼 등을 담당하며 네트워크 요청이나 파일 접근 같은 권한을 담당한다.</li>
<li>렌더러 프로세스: 웹 사이트가 '보이는' 부분의 모든 것을 제어한다.</li>
<li>플러그인 프로세스: 웹 사이트에서 사용하는 플러그인을 제어한다.</li>
<li>GPU 프로세스: GPU를 이용해서 화면을 그리는 부분을 제어한다.</li>
</ol>
<hr />
<h3 id="ipc">IPC</h3>
<blockquote>
<p>프로세스끼리 데이터를 주고 받고, 공유 데이터를 관리하는 매커니즘</p>
</blockquote>
<p>멀티프로세스는 IPC가 가능하다.</p>
<p>ex) 클라이언트와 서버
클라이언트 : 데이터를 요청
서버 : 클라이언트 요청에 응답</p>
<p>종류들에 대해서도 알아보자.</p>
<ul>
<li>공유 메모리</li>
<li>파일</li>
<li>소켓</li>
<li>익명 파이프</li>
<li>명명 파이프</li>
<li>메시지 큐</li>
</ul>
<p>메모리를 완전히 공유하는 스레드보다는 속도가 떨어지는데 스레드에 대해서도 이후에 알아보자.</p>
<hr />
<h4 id="공유-메모리">공유 메모리</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cbddfc68-64fd-40e5-b2c7-f7c1cfa50966/image.png" /></p>
<blockquote>
<p>여러 프로세스가 동일한 메모리 블록에 대한 접근 권한이 부여돼 프로세스가 공유된 메모리에서 서로 통신하는 방식</p>
</blockquote>
<p><strong>장점</strong>
메모리 자체를 공유하기 때문에 불필요한 데이터 복사의 오버헤드가 발생하지 않아 가장 빠르다. </p>
<p><strong>장점에 의한 고려사항</strong>
같은 메모리 영역을 공유하기 때문에 동기화 작업이 필요하다.</p>
<hr />
<h4 id="파일">파일</h4>
<blockquote>
<p>디스크에 저장된 데이터 또는 파일 서버에서 제공한 데이터를 말한다.</p>
</blockquote>
<hr />
<h4 id="소켓">소켓</h4>
<blockquote>
<p>동일한 컴퓨터의 다른 프로세스나 네트워크의 다른 컴퓨터로 네트워크 인터페이스를 통해 전송하는 데이터를 의미한다.</p>
</blockquote>
<p>예시로는 TCP, UDP가 있다.</p>
<hr />
<h4 id="익명-파이프">익명 파이프</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8c80baf8-02e6-4aa3-a5c9-38772f4cd739/image.png" /></p>
<blockquote>
<p>프로세스 간에 FIFO 방식으로 읽히는 임시 공간인 파이프를 기반으로 데이터를 주고 받으며, <strong>단방향 방식</strong> 의 읽기 전용, 쓰기 전용 파이프를 만들어서 작동하는 방식</p>
</blockquote>
<hr />
<h4 id="명명된-파이프">명명된 파이프</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d3af1ede-f94e-4073-8687-15abce7e65d8/image.png" /></p>
<blockquote>
<p>파이프 서버와 하나 이상의 파이프 클라이언트 간의 통신을 위한 <strong>명명된 단방향 or 양방향 파이프</strong></p>
</blockquote>
<p>클라이언트/서버 통신을 위한 별도의 파이프를 제공하며, 여러 파이프를 동시에 사용할 수 있다.</p>
<p>이를 통해 컴퓨터의 프로세스끼리 또는 다른 네트워크 상의 컴퓨터와도 통신을 할 수 있다.</p>
<hr />
<h4 id="메시지-큐">메시지 큐</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3cf8a076-e560-4faf-bcce-6f035eb7bb7f/image.png" /></p>
<blockquote>
<p>메시지를 큐에 형태로 관리하는 것을 의미한다.</p>
</blockquote>
<p>커널의 전역변수 형태 등 커널에서 전역적으로 관리된다.</p>
<p><strong>장점</strong></p>
<ul>
<li>매우 직관적이며, 간단하다.</li>
<li>다른 코드의 수정 없이 단지 몇 줄의 코드를 추가시켜 간단하게 접근할 수 있다</li>
</ul>
<hr />
<h2 id="스레드와-멀티스레딩">스레드와 멀티스레딩</h2>
<h3 id="스레드">스레드</h3>
<blockquote>
<p>프로세스의 실행 가능한 가장 작은 단위</p>
</blockquote>
<p>프로세스는 여러 스레드를 가질 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/db9d7a62-9571-48f2-b1ca-9f58f648963c/image.png" /></p>
<p><strong>프로세스와 스레드의 차이점</strong>
프로세스는 코드, 데이터, 스택, 힙 영역을 각각 생성
스레드는 코드, 데이터 힙 영역을 서로 공유한다. 그 외의 영역은 각각 생성된다.</p>
<hr />
<h3 id="멀티스레딩">멀티스레딩</h3>
<blockquote>
<p>이는 프로세스 내 작업을 여러 개의 스레드, 멀티 스레드로 처리하는 기법을 말한다.</p>
</blockquote>
<p><strong>장점</strong></p>
<ul>
<li>스레드끼리 자원을 서로 공유하기 때문에 효율성이 높다.</li>
<li>동시성도 큰 장점이 있다.</li>
</ul>
<p><strong>단점</strong>
한 스레드가 문제가 생기면 다른 스레드도 영향을 끼친다는 단점이 있다.</p>
<blockquote>
<p><strong>동시성</strong>이란? : 서로 독립적인 작업들을 작은 단위로 나누고 동시에 실행되는 것처럼 보여주는 것</p>
</blockquote>
<hr />
<h2 id="공유-자원과-임계-영역">공유 자원과 임계 영역</h2>
<h3 id="공유-자원">공유 자원</h3>
<blockquote>
<p>시스템 안에서 각 프로세스, 스레드가 함께 접근할 수 있는 모니터, 프린터, 메모리, 파일, 데이터 등의 자원이나 변수 등을 의미</p>
</blockquote>
<p>이 공유 자원을 두 개 이상의 프로세스가 동시에 읽거나 쓰이는 상황을 <code>경쟁 상태</code> 라고 한다.</p>
<p>이 때문에 접근의 타이밍이나 순서 등이 결과값에 영향을 준다.</p>
<p>예시 사진
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/eb6d054b-1dc3-4917-9e27-4d67eb8f8a2e/image.png" /></p>
<hr />
<h3 id="임계-영역">임계 영역</h3>
<blockquote>
<p>둘 이상의 프로세스, 스레드가 공유 자원에 접근할 때 순서 등의 이유로 결과가 달라지는 코드 영역</p>
</blockquote>
<p>이를 해결하기 위해선 크게 뮤텍스, 세마포어, 모니터 세 가지가 있다.</p>
<p>이 방법 모두 <code>상호 배제</code>, <code>한정 대기</code>, <code>융통성</code>이라는 조건을 만족하며</p>
<p>위 3가지 방법에 토대가 되는 메커니즘은 <strong>잠금(lock)</strong> 이다.</p>
<p>ex) 화장실 사용 시 문 잠그고 사용한 뒤 나오면 다음 사람이 사용하는 것</p>
<ul>
<li>상호배제 : 한 프로세스가 임계 영역에 들어갔을 때 다른 프로세스는 들어갈 수 없다.</li>
<li>한정 대기 : 특정 프로세스가 영원히 임계 영역에 들어가지 못하면 안 된다.</li>
<li>융통성 : 한 프로세스가 다른 프로세스의 일을 방해해서는 안 된다.</li>
</ul>
<hr />
<h4 id="뮤텍스">뮤텍스</h4>
<p>프로세스나 스레드가 공유 자원을 <code>lock()</code>을 통해 잠금 설정하고 사용한 후에는 <code>unlock()</code>을 통해 잠금 해제하는 방식</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/92951ca8-478d-442f-83ed-dcc7c6c46e60/image.png" /></p>
<hr />
<h4 id="세마포어">세마포어</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/116f9d61-ee7e-4086-9c72-0dc5c960a5eb/image.png" /></p>
<p>일반화된 뮤텍스로, 간단한 정수 값과 두 가지 함수 <code>wait</code> 함수, <code>signal</code> 함수로 공유 자원에 대한 접근을 처리한다.</p>
<p><code>wait()</code>는 자신의 차례가 올때까지 기다리는 함수이며, <code>signal()</code>은 다음 프로세스 순서로 넘겨주는 함수를 말한다.</p>
<p>공유 자원에 접근하려면, 세마포어에서 wait()를 수행하고 공유 자원을 해제하면 signal() 작업을 수행한다.</p>
<p><strong>세마포어의 종류</strong></p>
<ul>
<li><p><strong>바이너리 세미포어</strong>
0과 1의 두가지 값만 가질 수 있는 세마포어이다. 이는 신호 메커니즘으로 신호를 기반으로 일어난다.</p>
</li>
<li><p><strong>카운팅 세마포어</strong>
이는 여러 개의 값을 가질 수 있는 세마포어이다. 여러 자원에 대한 접근을 제어하는데 사용된다.</p>
</li>
</ul>
<hr />
<h4 id="모니터">모니터</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6ad67e6b-6e7b-4cf7-afc3-35df905576bf/image.png" /></p>
<p>둘 이상의 스레드나 프로세스가 공유 자원에 안전하게 접근할 수 있도록 공유자원을 숨기고, 해당 접근에 대해 인터페이스만을 제공한다.</p>
<p><strong>모니터와 세마포어의 차이점</strong></p>
<ul>
<li>모니터는 세마포어보다 구현하기 쉽고, 상호 배제는 자동</li>
<li>세마포어는 상호 배제를 명시적으로 구현해야 한다.</li>
</ul>
<hr />
<h2 id="교착-상태">교착 상태</h2>
<blockquote>
<p><strong>교착 상태</strong> : 두 개 이상의 프로세스들이 서로가 가진 자원을 기다리며 중단된 상태</p>
</blockquote>
<h3 id="원인">원인</h3>
<ul>
<li><strong>상호 배제</strong> : 한 프로세스가 자원을 독점하고 있으며 다른 프로세스는 접근이 불가능하다.</li>
<li><strong>점유 대기</strong> : 특정 프로세스가 점유한 자원을 다른 프로세스가 요청하는 상태</li>
<li><strong>비선점</strong> : 다른 프로세스의 자원을 강제적으로 가져올 수 없다.</li>
<li><strong>환형 대기</strong> : 프로세스 A는 프로세스 B의 자원을 요구하고, 프로세스 B는 프로세스 A의 자원을 요구하는 등 서로가 서로의 자원을 요구하는 상황</li>
</ul>
<h3 id="해결방법">해결방법</h3>
<ol>
<li>자원을 할당할 때 애초에 조건이 성립되지 않도록 설계한다.</li>
<li>교착 상태 가능성이 없을 때만 자원 할당되며, 프로세스당 요청할 자원들의 최대치를 통해 자원 할당 가능 여부를 파악하는 <code>은행원 알고리즘</code> 을 쓴다.</li>
</ol>
<blockquote>
<p>은행원 알고리즘
총 자원의 양과 현재 할당한 양을 기준으로 안정 또는 불안정 상태로 나누고 안정 상태로 가도록 자원을 할당하는 알고리즘</p>
</blockquote>
<ol start="3">
<li>교착 상태가 발생하면 사이클이 있는지 찾아보고 이에 관련된 프로세스를 한 개씩 지운다.</li>
<li>교착 상태는 매우 드물게 일어나고 비용이 크기 때문에 현대 운영체제에서는 사용자가 작업을 종료하는 방법을 채택했다.</li>
</ol>