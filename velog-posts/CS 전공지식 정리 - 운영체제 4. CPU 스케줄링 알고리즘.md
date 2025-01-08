<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e3142c4c-0f35-4e8b-afc1-88ad13d1e124/image.png" /></p>
<p>CPU 스케줄러는 <strong>CPU 스케줄링 알고리즘에 따라 프로세스에서 해야 하는 일을 스레드 단위로 CPU에 할당</strong>한다.</p>
<p>CPU 스케줄링 알고리즘이 어떤 프로그램에 CPU 소유권을 줄지 결정하는데</p>
<blockquote>
<ul>
<li>CPU 이용률은 높게</li>
</ul>
</blockquote>
<ul>
<li>주어진 시간에 많은 일을 하게</li>
<li>준비 큐에 있는 프로세스는 적게</li>
<li>응답 시간은 짧게</li>
</ul>
<hr />
<h2 id="비선점형-방식">비선점형 방식</h2>
<blockquote>
<p>프로세스 스스로 CPU 소유권을 포기하는 방식이며, 강제로 프로세스를 중지하지 않는다.
컨텍스트 스위칭으로 인한 부하가 적다.</p>
</blockquote>
<h3 id="fcfs">FCFS</h3>
<p>FCFS는 First Come, First Served 로 <strong>가장 먼저 온 것을 가장 먼저 처리하는 알고리즘</strong></p>
<p><strong>단점</strong></p>
<ul>
<li><strong>길게 수행되는 프로세스 때문에 준비 큐에서 오래 기다리는 현상</strong> (Convoy effect)이 발생하기도 한다.</li>
</ul>
<hr />
<h3 id="sjf">SJF</h3>
<p>SJF는 Shortest Job First로 <strong>실행 시간이 가장 짧은 것을 가장 먼저 실행하는 알고리즘</strong></p>
<p><strong>장점</strong></p>
<ul>
<li>평균 대기 시간이 가장 짧은 알고리즘</li>
</ul>
<p><strong>단점</strong></p>
<ul>
<li><strong>긴 시간을 가진 프로세스가 실행되는 않는 현상</strong> (Starvation)이 발생하기도 한다.</li>
</ul>
<p>위와 같은 장점이 있기도 하지만, 실제로는 실행 시간을 알 수 없어 과거에 실행했던 시간을 토대로 추측해서 사용한다.</p>
<hr />
<h3 id="우선순위">우선순위</h3>
<p>기존 SJF 스케줄링의 경우 긴 시간을 가진 프로세스가 실행되지 않는 현상이 있었다.</p>
<p><strong>오래된 작업일수록 우선 순위를 높이는 방식을 통해 위에 알고리즘에 단점을 보완한 방식</strong></p>
<hr />
<h2 id="선점형-방식">선점형 방식</h2>
<p>이는 현대 운영체제가 쓰는 방식</p>
<blockquote>
<p>지금 사용하고 있는 프로세스를 알고리즘에 의해 중단 시켜 버리고, 강제로 다른 프로세스에 CPU 소유권을 할당하는 방식</p>
</blockquote>
<p>라운드 로빈, SRF, 다단계 큐와 같은 것들이 있다. 알아보자.</p>
<hr />
<h3 id="라운드-로빈">라운드 로빈</h3>
<p>이는 현대 컴퓨터가 쓰고 있는 스케줄링인 우선순위 스케줄링의 일종</p>
<blockquote>
<p>각 프로세스는 동일한 할당 시간을 주고 그 시간 안에 끝나지 못하면 다시 준비 큐 뒤로 가는 알고리즘</p>
</blockquote>
<p>시간 할당량을 q 라고 했을 때 N개의 프로세스가 운영된다고 하면 (N-1) * q 시간이 지났을 때 자신의 차례가 된다.</p>
<p>할당 시간 q가 너무 크면 FCFS가 되고, 짧으면 컨텍스트 스위칭이 잦아지면서 오버헤드 -&gt; 비용이 커진다.</p>
<p>일반적으로 전체 작업 시간은 길어지지만 평균 응답 시간은 짧아진다는 특징이 있다.</p>
<blockquote>
<p>로드밸런서에서 트래픽 분산 알고리즘으로도 쓰인다.</p>
</blockquote>
<hr />
<h3 id="srf">SRF</h3>
<p>SJK는 실행 시간이 더 짧은게 들어와도 기존 작업을 모두 수행하고 그 다음 짧은 작업을 이어간다.</p>
<p>하지만 SRF(Shortest Remaining Time First)는 <strong>중간에 더 짧은 작업이 들어오면 수행하던 프로세스를 중지하고 해당 프로세스를 수행하는 알고리즘</strong></p>
<hr />
<h3 id="다단계-큐">다단계 큐</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f10b113f-7d67-4805-9bc5-b9ca6d38c689/image.png" /></p>
<blockquote>
<p>우선순위에 따른 준비 큐를 여러 개 사용하고, 큐마다 라운드 로빈이나 FCFS 등 다른 스케줄링 알고리즘을 적용한 것</p>
</blockquote>
<p>큐 간의 프로세스 이동이 안되므로 스케줄링 부담이 적지만 유연성이 그만큼 떨어진다.</p>