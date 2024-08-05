<h1 id="ddd">DDD</h1>
<blockquote>
<p>DDD : Domain Driven Design (Development)</p>
</blockquote>
<p>도메인 주도 설계로, 서비스의 &quot;기능&quot;을 기준으로 코드 구분을 하는 것이 아닌, <strong>도메인이라는 비즈니스 영역을 기준으로 코드를 구분하는 것</strong></p>
<p>코드의 역할과 책임이 명확해지고, 비즈니스 로직을 이해하고 관리하는 것이 한결 더 쉬워진다.</p>
<hr />
<h2 id="이벤트-스토밍">이벤트 스토밍</h2>
<blockquote>
<p>서비스와 관계 있는 모든 이해관계자들이 서로가 가지고 있는 생각을 공유하며 서비스에서 발생하는 <strong>이벤트를 중심(Event-First)</strong>으로 분석하는 기법</p>
</blockquote>
<p>이벤트 스토밍을 통해 영역을 나눠서 서브 도메인을 만들고, 이는 곧 bounded context가 된다.</p>
<p>이것이 더 나아가서 MSA의 기반이 되기도 하는데 한 번 간단하게 알아보자.</p>
<ol>
<li>서비스에서 <strong>어떤 주체</strong>가 <strong>어떤 행동</strong>을 취하는가?</li>
<li><strong>그 행동의 결과</strong>로 <strong>어떤 이벤트가 발생</strong>하는가?</li>
<li><strong>이벤트가 발생</strong>하면 시스템에서는 <strong>어떤 변화</strong>가 일어나는가?</li>
<li><strong>이 이벤트</strong>가** 다른 이벤트에 어떤 영향**을 미치는가?</li>
</ol>
<p>위 4가지 질문에 답을 얻어가며 서비스의 핵심 프로세스와 비즈니스 로직을 명확히 이해하고 모델링할 수 있을 것이다.</p>
<hr />
<h3 id="domain-event">Domain Event</h3>
<p>도메인에서 실제로 발생하는 결과를 뜻한다.
서비스에서 발생한 사실, 결과에 대해 Output을 표현하기 위해 사용한다.</p>
<h3 id="command">Command</h3>
<p>Command는 간단하게 비즈니스 로직, 하나의 트랜잭션이라고 생각하면 된다.
이벤트가 발생되기 전에 command가 붙는데, 말 그대로 사용자가 결제를 하겠다는 요청.
이 요청 자체가 Command에 속하게 되는 것이다.</p>
<h3 id="hotspot">Hotspot</h3>
<p>수정하거나 궁금하거나 정해지지 않은 정책에 대해 표현하는 것. 다 정해지면 바로 떼면서 최종적으로는 붙어 있어서는 안 된다.</p>
<h3 id="actor">Actor</h3>
<p>Command를 발생시키는 '주체'를 뜻한다.
ex) 고객이 상품을 '주문' -&gt; 여기서는 <strong>주문자인 고객이 Actor</strong></p>
<h3 id="external-system-외부-시스템">External System (외부 시스템)</h3>
<p><strong>Domain event를 호출하거나 관계가 있는 것을 표현할 때 사용</strong>한다.
ex) 결제 요청을 처리하기 위해서는 “결제 모듈”이라는 외부 시스템이 필요하며, 고객에게 알림을 전달하기 위해서는 “알림 모듈”이라는 외부 시스템이 필요할 것이다. </p>
<p>위 예제와 같이 서비스에 필요하지만, 외부 의존성을 가지는 시스템을 뜻한다.</p>
<h3 id="aggregate">Aggregate</h3>
<p>상태가 변경되는 <strong>데이터 묶음</strong>으로 (연관되는 Entity와 값 객체의 묶음)
-&gt; vo, enum, entity</p>
<p>ex) 고객이 자동차를 대여하는 요청을 하면, 자동차 대여점에서는 해당 요청을 처리하기 위해 필요한 자동차의 상세한 정보(차종, 연식, 주행거리 등)가 필요하게 된다.</p>
<h3 id="policy">Policy</h3>
<p>event 조건에 따라 발생하는 <strong>추가적인(or 새로운) Command</strong>를 나타낸다.
비즈니스 로직들은 1개 행위는 1개의 결과만 발생할까? -&gt; <strong>아니다.</strong></p>
<p>주문 <strong>요청</strong> -&gt; 주문 <strong>완료</strong> -&gt; 알림 생성 <strong>요청</strong> -&gt; 알림 <strong>발송</strong> 처럼 이벤트로 인해 발생하는 것들을 &quot;<strong>정책</strong>&quot;이라고 한다.</p>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2dc0fd38-29cb-4d06-9596-c3a644a2322a/image.png" /></p>
<p><a href="https://custom-li.tistory.com/207">참고한 블로그의 게시글</a></p>
<hr />
<p>간단하게 개념을 알아보고 한 번 이번에 하는 프로젝트를 DDD를 활용해서 나타내볼까 한다.</p>