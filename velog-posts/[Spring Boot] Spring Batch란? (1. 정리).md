<p>프로젝트를 진행하며 연관관계가 완벽하지 않을 것이라 생각하고 연관관계에 대한 성능 개선을 위해 어떻게 데이터를 임시로 만들어서 사용할 수 있을까에 대해 고민을 해보았습니다.</p>
<ol>
<li>반복문을 활용해 멀티쓰레드로 데이터를 만들어서 사용하기</li>
<li>Spring Batch 를 간단하게 적용해보기</li>
</ol>
<p>2가지 방법에서 고민을 했고, 당장엔 반복문도 좋지만 Spring Batch 에 대해 깊게는 아니어도 공부해 나중에 같은 고민을 하게 될 때 활용하기 위해 이 글을 작성하게 됐습니다.</p>
<hr />
<blockquote>
<p><strong>목차</strong>
    - Spring Batch 개념
    - 특징
    - 용어</p>
</blockquote>
<hr />
<h1 id="spring-batch란">Spring Batch란?</h1>
<p>우선 Batch 는 '일괄 처리'라는 뜻을 지니고 있습니다.</p>
<p>주기적으로 임의의 데이터로 처리해야 하는 기능이 있다고 했을 때, 그 시간에 데이터를 가져와 처리해야 할텐데 이것을 어떻게 구현할 수 있을까요?</p>
<p>많은 분들이 Spring Schedule 을 사용해서 구현하고 있을 것이라고 생각합니다.</p>
<pre><code class="language-java">import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ExamScheduler {

    @Scheduled(cron = &quot;0 0 1 1 * *&quot;) // 매월 1일 새벽 1시에 실행
    public void exam() {

    }
}
</code></pre>
<pre><code>위와 같은 코드처럼 말이죠. (Schedule에 대한 글도 추후에 작성하겠습니다.)</code></pre><p>그런데 이렇게 하면 서버는 exam() 에 대한 기능에 리소스를 다 사용하느라 사용자의 요청은 처리 못할 수 있다고 합니다.
또한 스케쥴링 일정에 변경이 생긴다면 서비스를 재시작해야 하기도 합니다.</p>
<p>하지만 Spring Batch를 활용한다면 위와 같은 문제점을 없앨 수 있습니다.</p>
<hr />
<h2 id="spring-batch-특징">Spring Batch 특징</h2>
<ul>
<li>로깅, 추적, 통계 처리, 트랜잭션 관리, 작업 재시작, 건너뛰기 등 재사용 가능한 필수 기능 지원</li>
<li>대용량 데이터 처리에 최적화되어 고성능</li>
<li>수동으로 처리하지 않도록 자동화되어 있음</li>
<li>작업 프로세스 구조만 이해하면 비즈니스 로직에만 집중 가능</li>
<li>예외 사항과 비정상적인 동작에 대한 방어 기능 존재<ul>
<li>Batch가 실패해 작업 재시작 시 <strong>실패한 지점부터 실행</strong></li>
<li>중복 실행을 막기 위해 성공한 이력이 있는 Batch는 <strong>동일한 Parameters로 실행 시 Exception이 발생</strong></li>
</ul>
</li>
</ul>
<hr />
<p>우선 알아둬야 할 점으로는 스케쥴링 일정에 변경이 생기면 Batch를 사용해 해결할 수 있다는 것에서 드러납니다.</p>
<blockquote>
<p><strong>Batch 와 Schedule은 비교대상이 아닙니다. (스케줄러가 아니기 때문)</strong></p>
</blockquote>
<p>왜냐하면 Spring Batch 에서 'Job' 이라고 하는 Batch 처리 과정을 객체로 만든 것을 관리를 하지만, Job을 실행시키거나, 활용하는 기능을 하지는 않습니다.
Job을 실행시키려면 Schedule을 사용해야 합니다.</p>
<pre><code>Schedule 에는 Quartz, Scheduler, Jenkins 와 같은 스케줄이 있습니다.</code></pre><p>위에서 말한 Job 부터 개념을 정리해보겠습니다.
용어에 대한 개념을 익힐 때 정말 많은 도움을 받은 사이트를 출처로 남기겠습니다.</p>
<p><a href="https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0">히진쓰의 서버사이드 기술 블로그 - Spring Batch란? 이해하고 사용하기(예제소스 포함)</a></p>
<hr />
<h2 id="spring-batch의-용어들">Spring Batch의 용어들</h2>
<p><strong>1. Job</strong>
배치처리 과정을 하나의 단위로 만들어 놓은 <strong>객체</strong>입니다. 배치처리 과정에 있어 <strong>전체 계층 최상단</strong>에 위치합니다.</p>
<p><strong>2. JobInstance</strong>
<code>JobInstance</code> 는 <strong>Job의 실행의 단위</strong>를 나타냅니다. Job을 스케줄러로 실행시키게 되면 하나의 JobInstance가 생성되게 됩니다. 예를 들어 1월 1일 실행, 1월 2일 실행을 하게 되면 각각의 JobInstance가 생성되며 1월 1일 실행한 JobInstance가 실패하여 다시 실행을 시키더라도 이 JobInstance는 1월 1일에 대한 데이터만 처리하게 됩니다.</p>
<p><strong>3. JobParameters</strong>
JobInstance는 Job의 실행 단위라고 했습니다. 그렇다면 JonInstance는 어떻게 구별할까요?
이는 바로 <code>JobParameters</code> 객체로 구분하게 됩니다. <code>JobParameters</code>는 <strong>JobInstance 구별</strong> 외에도 <strong>개발자 JobInstance에 전달되는 매개변수 역할</strong>도 하고 있습니다.</p>
<pre><code>또한 JobParameters는 String, Double, Long, Date 4가지 형식만을 지원하고 있습니다.</code></pre><p><strong>4. JobExecution</strong>
<code>JobExecution</code>은 JobInstance에 대한 실행 시도에 대한 객체입니다. 1월 1일에 실행한 JobInstacne가 실패하여 재실행을 하여도 동일한 JobInstance를 실행시키지만 이 2번에 실행에 대한 JobExecution은 개별로 생기게 됩니다. JobExecution는 이러한 <strong>JobInstance 실행에 대한 상태, 시작시간, 종료시간, 생성시간 등의 정보를 담고 있습니다.</strong></p>
<p><strong>5. Step</strong>
<code>Step</code>은 Job의 배치처리를 정의하고 순차적인 단계를 캡슐화 합니다. <strong>Job은 최소한 1개 이상의 Step을 가져야 하며</strong> Job의 실제 일괄 처리를 제어하는 모든 정보가 들어있습니다.</p>
<p><strong>6. StepExecution</strong>
<code>StepExecution</code>은 JobExecution과 동일하게 <strong>Step 실행 시도에 대한 객체</strong>를 나타냅니다. 하지만 Job이 여러 개의 Step으로 구성되어 있을 경우 이전 단계의 Step이 실패하게 되면 다음 단계가 실행되지 않음으로 실패 이후 StepExecution은 생성되지 않습니다. 
<strong>StepExecution 또한 JobExecution과 동일하게 실제 시작이 될 때만 생성</strong>됩니다. 
StepExecution에는 JobExecution에 저장되는 정보 외에 read 수, write 수, commit 수, skip 수 등의 정보들도 저장이 됩니다.</p>
<p><strong>7. ExecutionContext</strong>
<code>ExecutionContext</code>란 Job에서 데이터를 공유할 수 있는 데이터 저장소입니다.
Spring Batch에서 제공하는 <code>ExecutionContext</code>는 1. <code>JobExecutionContext</code>, 2. <code>StepExecutionContext</code> 2가지 종류가 있으나 <strong>이 2가지는 지정되는 범위가 다릅니다.</strong>
<code>JobExecutionContext</code>의 경우 Commit 시점에 저장되는 반면, <code>StepExecutionContext</code>는 실행 사이에 저장이 되게 됩니다.
ExecutionContext를 통해 Step간 Data 공유가 가능하며 Job 실패시 ExecutionContext를 통한 마지막 실행 값을 재구성 할 수 있습니다.</p>
<p><strong>8. JobRepository</strong>
<code>JobRepository</code>는 <strong>위에서 말한 모든 배치 처리 정보를 담고있는 매커니즘</strong>입니다. Job이 실행되게 되면 JobRepository에 JobExecution과 StepExecution을 생성하게 되며 JobRepository에서 Execution 정보들을 저장하고 조회하며 사용하게 됩니다.</p>
<p><strong>9. JobLauncher</strong>
<code>JobLauncher</code>는 <strong>Job과 JobParameters를 사용하여 Job을 실행하는 객체</strong>입니다.</p>
<blockquote>
<p>Schedule을 활용해서 JobLauncher 를 사용할 수 있는 것이지 Batch 에서 가능한 것이 아닙니다.</p>
</blockquote>
<p><strong>10. ItemReader</strong>
<code>ItemReader</code>는 <strong>Step에서 Item을 읽어오는 인터페이스</strong>입니다. ItemReader에 대한 다양한 인터페이스가 존재하며 다양한 방법으로 Item을 읽어 올 수 있습니다.</p>
<p><strong>11. ItemWriter</strong>
<code>ItemWriter</code>는 <strong>처리된 Data를 Writer 할 때 사용한다.</strong>
Writer는 처리 결과물에 따라 Insert가 될 수도 Update가 될 수도 Queue를 사용한다면 Send가 될 수도 있습니다. Writer 또한 Read와 동일하게 다양한 인터페이스가 존재합니다. 
Writer는 기본적으로 Item을 Chunk로 묶어 처리하고 있습니다.</p>
<p><strong>12. ItemProcessor</strong>
<code>ItemProcessor</code>는 Reader에서 읽어온 Item을 데이터를 처리하는 역할을 하고 있습니다. Processor는 배치를 처리하는데 필수 요소는 아니며 Reader, Writer, Processor 처리를 분리하여 각각의 역할을 명확하게 구분하고 있습니다.</p>