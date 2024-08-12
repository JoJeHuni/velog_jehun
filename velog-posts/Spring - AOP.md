<p>OOP (Object Oriented Programming) : 객체 지향 프로그래핑
AOP (Aspect Oriented Programming) : 관점 지향 프로그래밍</p>
<p>2가지에 대해서 알아야 한다.</p>
<p>Java와 같은 OOP의 특징과 Spring의 AOP의 특징을 함께 알아야 한다.</p>
<p>AOP는 OOP의 확장 개념이다.</p>
<hr />
<h1 id="aop">AOP</h1>
<blockquote>
<p><strong>AOP</strong> : 중복되는 공통 코드를 분리하고 코드 실행 전이나 후의 시점에 해당 코드를 삽입함으로써 소스 코드의 중복을 줄이고, 필요할 때마다 가져다 쓸 수 있게 객체화하는 기술</p>
</blockquote>
<h2 id="핵심-용어">핵심 용어</h2>
<table>
<thead>
<tr>
<th>용어</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Aspect</td>
<td>핵심 비즈니스 로직과는 별도로 수행되는 횡단 관심사를 말한다.</td>
</tr>
<tr>
<td>Advice</td>
<td>Aspect의 기능 자체를 말한다.</td>
</tr>
<tr>
<td>Join point</td>
<td>Advice가 적용될 수 있는 위치를 말한다.</td>
</tr>
<tr>
<td>Point cut</td>
<td>Join point 중에서 Advice가 적용될 가능성이 있는 부분을 선별한 것을 말한다.</td>
</tr>
<tr>
<td>Weaving</td>
<td>Advice를 핵심 비즈니스 로직에 적용하는 것을 말한다.</td>
</tr>
</tbody></table>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/19cfe3cb-b544-425b-b67c-0dc0fda59140/image.png" /></p>
<p>Aspect : <strong>어느 지점</strong>에 <strong>어떤 기능을 사용</strong>할 것인가</p>
<blockquote>
<p><strong>어느 지점</strong> = <strong>Join point</strong>
Join point에 <strong>사용 할 기능을 선정하는 것</strong> = <strong>Point cut</strong></p>
</blockquote>
<p>위 2가지 과정에 속하는 것이 <strong>Aspect</strong> 이고, 만든 것을 <strong>Advice</strong>, 그것을 실제 코드에 적용하는 것을 <strong>Weaving</strong>이라고 한다.</p>
<hr />
<h2 id="advice-종류">Advice 종류</h2>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Before</td>
<td>대상 메소드가 실행되기 이전에 실행되는 어드바이스</td>
</tr>
<tr>
<td>After-returning</td>
<td>대상 메소드가 정상적으로 실행된 이후에 실행되는 어드바이스</td>
</tr>
<tr>
<td>After-throwing</td>
<td>예외가 발생했을 때 실행되는 어드바이스</td>
</tr>
<tr>
<td>After</td>
<td>대상 메소드가 실행된 이후에(정상, 예외 관계없이) 실행되는 어드바이스</td>
</tr>
<tr>
<td>Around</td>
<td>대상 메소드 실행 전/후에 적용되는 어드바이스</td>
</tr>
</tbody></table>
<hr />
<h2 id="spring-aop">Spring AOP</h2>
<ul>
<li><p><strong>Proxy 기반의 AOP 구현체</strong> : 대상 객체(Target Object)에 대한 프록시를 만들어 제공하며, 타겟을 감싸는 프록시는 서버 Runtime 시에 생성된다.</p>
</li>
<li><p><strong>메서드 조인 포인트만 제공</strong> : 핵심기능(대상 객체)의 메소드가 호출되는 런타임 시점에만 부가기능(어드바이스)을 적용할 수 있다.</p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bc6fed47-babf-46b9-9b2c-358ef48fd75e/image.png" /></p>
<p><strong>Proxy</strong> : '대리자' 라는 개념으로 알고 있겠지만, <strong>원래 코드를 건드리지 않고 부가 코드를 덧씌우는 기술</strong>로 알고 있는게 좋다.</p>
<hr />
<h1 id="aop-구현해보기">AOP 구현해보기</h1>
<p>MemberDTO, MemberDAO, MemberService 를 활용해서 출력해보자.</p>
<p><strong>MemberDTO</strong></p>
<pre><code class="language-java">public class MemberDTO {
    private Long id;
    private String name;

    public MemberDTO(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return &quot;MemberDTO{&quot; +
                &quot;id=&quot; + id +
                &quot;, name='&quot; + name + '\'' +
                '}';
    }
}</code></pre>
<blockquote>
<p><strong>Lombok</strong> 을 활용해서 <code>@AllArgsConstructor</code>, <code>@ToString</code> 도 가능하다.</p>
</blockquote>
<p><strong>MemberService</strong></p>
<pre><code class="language-java">import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class MemberService {

    private final MemberDAO memberDAO;

    @Autowired
    public MemberService(MemberDAO memberDAO) {
        this.memberDAO = memberDAO;
    }

    public List&lt;MemberDTO&gt; findAllMembers() {
        System.out.println(&quot;Target -&gt; findAllMembers()&quot;);

        return memberDAO.selectAllMembers();
    }
}</code></pre>
<p><strong>MemberDAO</strong></p>
<pre><code class="language-java">import org.springframework.stereotype.Repository;

import java.util.ArrayList;
import java.util.List;

@Repository
public class MemberDAO {

    private final List&lt;MemberDTO&gt; memberList;

    public MemberDAO() {
        memberList = new ArrayList&lt;&gt;();
        memberList.add(new MemberDTO(1L, &quot;홍길동&quot;));
        memberList.add(new MemberDTO(2L, &quot;유관순&quot;));
    }

    public List&lt;MemberDTO&gt; selectAllMembers() {
        return memberList;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.List;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.ohgiraffers.section01.aop&quot;);

        MemberService memberService = context.getBean(&quot;memberService&quot;, MemberService.class);
        System.out.println(&quot;===== Select All Members =====&quot;);

        List&lt;MemberDTO&gt; members = memberService.findAllMembers();
        members.forEach(System.out::println);
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/636b771a-50f3-43f0-b351-8fbea6144172/image.png" /></p>
<p>이제 AOP 기능을 동작하기 위해서 build.gradle에 추가할 라이브러리가 있다.</p>
<p><strong>build.gradle</strong></p>
<pre><code>    // https://mvnrepository.com/artifact/one.gfw/aspectjweaver
    implementation 'one.gfw:aspectjweaver:1.9.19'

    // https://mvnrepository.com/artifact/org.aspectj/aspectjrt
    implementation(&quot;org.aspectj:aspectjrt:1.9.19&quot;)</code></pre><p>dependency 에 추가하자.</p>
<hr />
<h1 id="aspect-생성">Aspect 생성</h1>
<h2 id="회원-전체-조회-기능">회원 전체 조회 기능</h2>
<h3 id="before-after-어드바이스-사용">Before, After 어드바이스 사용</h3>
<p><code>LoggingAspect</code> 클래스를 생성하고 빈 스캐닝을 통해 빈 등록을 한다.</p>
<p>우선 <code>Before</code> 어드바이스를 간단하게 알아보자.</p>
<p><code>Before</code> 어드바이스는 대상 메소드가 실행되기 이전에 실행되는 어드바이스이다. 미리 작성한 포인트 컷을 설정한다. </p>
<p><strong>LoggingAspect</strong></p>
<pre><code class="language-java">package com.jehun.section01.aop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {
    @Before(&quot;execution(* com.jehun.section01.aop.*Service.*(..))&quot;)
    public void logBefore (JoinPoint joinPoint) {
        System.out.println(&quot;Before joinPoint.getTarget() : &quot; + joinPoint.getTarget());
        System.out.println(&quot;Before joinPoint.getSignature() : &quot; + joinPoint.getSignature());

        if (joinPoint.getArgs().length &gt; 0) { // 타겟으로 하는 매개변수가 하나라도 있다면
            System.out.println(&quot;Before joinPoint.getArgs()[0] : &quot; + joinPoint.getArgs()[0]); // 배열 형태로도 받아올 수 있다.
        }
    }
}</code></pre>
<p><code>@Aspect</code> : pointcut과 advice를 하나의 클래스 단위로 정의하기 위한 어노테이션이다.</p>
<p>위의 <strong>LoggingAspect</strong> 를 적용하기 위해서는 빈 설정파일이 필요하다.</p>
<p><code>aspectj</code>의 <code>autoProxy</code> 사용에 관한 설정을 해 주어야 <code>advice</code>가 동작한다.</p>
<p><strong>ContextConfiguration</strong></p>
<pre><code class="language-java">package com.jehun.section01.aop;

import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class ContextConfiguration {
}</code></pre>
<p><code>proxyTargetClass=true</code> 설정은 cglib를 이용한 프록시를 생성하는 방식으로, Spring 3.2부터 스프링 프레임워크에 포함되어 별도 라이브러리 설정을 하지 않고 사용할 수 있다. 
성능 면에서 더 우수하다.</p>
<p>다시 Application 클래스의 main을 실행하면</p>
<pre><code>Before joinPoint.getTarget() : com.jehun.section01.aop.MemberService@c667f46
Before joinPoint.getSignature() : List com.jehun.section01.aop.MemberService.findAllMembers()
Target -&gt; findAllMembers()
MemberDTO{id=1, name='홍길동'}
MemberDTO{id=2, name='유관순'}</code></pre><p>위와 같이 출력된다. 즉, <code>MemberService</code> 부분을 호출할 때 <code>LoggingAspect</code> 클래스에서 중간에 채가서 <code>@Before</code> 어노테이션에서 정의한 포인트 컷 속 <code>jointPoint.getTarget()</code> 을 실행해준다.</p>
<ul>
<li><strong>타켓 클래스의 메소드에서 어드바이스를 적용할 수 있는 지점들</strong>을 <code>조인포인트(Join Point)</code>라고 한다.</li>
<li><code>포인트컷(Point Cut)</code>은 <strong>여러 조인포인트들에 어드바이스를 적용할 곳을 지정한 것</strong>이다.</li>
<li>해당 조인포인트에서 어드바이스가 동작한다.</li>
</ul>
<blockquote>
<p><strong>&lt;포인트컷 표현식&gt;</strong>
execution([수식어] 리턴타입 [클래스이름], 이름(파라미터))</p>
<ol>
<li>수식어 : public, private 등 수식어를 명시 (생략 가능하다)</li>
<li>리턴 타입 : 리턴 타입을 명시</li>
<li>클래스 이름(패키지명 포함) 및 메소드 이름 : 클래스 이름과 메소드 이름을 명시</li>
<li>파마리터(매개변수) : 메소드의 파라미터를 명시</li>
<li>&quot; * &quot; : 1개이면서 모든 값이 올 수 있음</li>
<li>&quot; .. &quot; : 0개 이상의 모든 값이 올 수 있음</li>
</ol>
</blockquote>
<blockquote>
<p><strong>포인트컷의 예시</strong>
<code>excution(public Integer com.jehun.section01.advice.*.*(*))</code>
=&gt; <code>com.jehun.section01.advice</code> 패키지에 속해 있는 <strong>바로 다음 하위 클래스에 파라미터가 1개인 모든 메소드</strong>이자 <strong>접근 제어자가 public</strong> 이고 <strong>반환형이 Integer인 경우</strong></p>
<p><code>excution(* com.jehun.section01.advice.annotation..stu*(..))</code>
=&gt; <code>com.jehun.section01.advice</code> 패키지 및 하위 패키지에 속해 있고 이름이 stu로 시작하는 파라미터가 0개 이상인 모든 메소드이며 접근제어자와 반환형은 상관없음</p>
</blockquote>
<p>LoggingAspect에 <code>logPointcut()</code> 메소드 추가하기</p>
<pre><code class="language-java">    @Pointcut(&quot;execution(* com.jehun.section01.aop.*Service.*(..))&quot;)
    public void logPointcut(){
    }</code></pre>
<p>그럼 이제 Before advice에서도 바꿔줄 수 있다.</p>
<pre><code class="language-java">@Before(&quot;LoggingAspect.logPointcut()&quot;)
    public void logBefore (JoinPoint joinPoint) {
        System.out.println(&quot;Before joinPoint.getTarget() : &quot; + joinPoint.getTarget());
        System.out.println(&quot;Before joinPoint.getSignature() : &quot; + joinPoint.getSignature());

        if (joinPoint.getArgs().length &gt; 0) { // 타겟으로 하는 매개변수가 하나라도 있다면
            System.out.println(&quot;Before joinPoint.getArgs()[0] : &quot; + joinPoint.getArgs()[0]); // 배열 형태로도 받아올 수 있다.
        }
    }</code></pre>
<p>이번엔 <code>After</code> 어드바이스에 대해 알아보자.</p>
<p><code>After</code> 어드바이스는 대상 메소드가 실행된 이후에(정상, 예외 관계없이) 실행되는 어드바이스이다. 미리 작성한 포인트 컷을 설정한다. </p>
<p>포인트 컷을 동일한 클래스 내에서 사용한다면, 클래스명 생략 가능</p>
<p>패키지가 다르다면, 패키지를 포함한 클래스명을 기술해야한다.</p>
<blockquote>
<p>Before 어드바이스와 동일하게 매개변수로 JoinPoint 객체 전달 가능</p>
</blockquote>
<pre><code class="language-java">@After(&quot;logPointcut()&quot;)
    public void logAfter(JoinPoint joinPoint) {
        System.out.println(&quot;After joinPoint.getTarget() &quot; + joinPoint.getTarget());
        System.out.println(&quot;After joinPoint.getSignature() &quot; + joinPoint.getSignature());
        if(joinPoint.getArgs().length &gt; 0){
            System.out.println(&quot;After joinPoint.getArgs()[0] &quot; + joinPoint.getArgs()[0]);
        }
    }</code></pre>
<p><strong>위의 코드 추가 후 실행 결과</strong></p>
<pre><code>===== Select All Members =====
Before joinPoint.getTarget() : com.jehun.section01.aop.MemberService@26adfd2d
Before joinPoint.getSignature() : List com.jehun.section01.aop.MemberService.findAllMembers()
Target -&gt; findAllMembers()
After joinPoint.getTarget() com.jehun.section01.aop.MemberService@26adfd2d
After joinPoint.getSignature() List com.jehun.section01.aop.MemberService.findAllMembers()
MemberDTO{id=1, name='홍길동'}
MemberDTO{id=2, name='유관순'}</code></pre><hr />
<h2 id="회원-1명-조회">회원 1명 조회</h2>
<p><strong>Application 에 추가</strong></p>
<pre><code class="language-java">        System.out.println(&quot;===== Select One Members =====&quot;);
        System.out.println(memberService.findByMember(1));</code></pre>
<p><strong>MemberService</strong> 에 추가</p>
<pre><code class="language-java">    public MemberDTO findByMember(int index) {
        System.out.println(&quot;target =&gt; findByMember 실행&quot;);
        return memberDAO.selectByMember(index);
    }</code></pre>
<p><strong>MemberDAO에 추가</strong></p>
<pre><code class="language-java">    public MemberDTO selectByMember(int index) {
        return memberList.get(index);
    }</code></pre>
<p><strong>실행 결과</strong></p>
<pre><code>===== Select All Members =====
Before joinPoint.getTarget() : com.jehun.section01.aop.MemberService@26adfd2d
Before joinPoint.getSignature() : List com.jehun.section01.aop.MemberService.findAllMembers()
Target -&gt; findAllMembers()
After joinPoint.getTarget() com.jehun.section01.aop.MemberService@26adfd2d
After joinPoint.getSignature() List com.jehun.section01.aop.MemberService.findAllMembers()
MemberDTO{id=1, name='홍길동'}
MemberDTO{id=2, name='유관순'}
===== Select One Members =====
Before joinPoint.getTarget() : com.jehun.section01.aop.MemberService@26adfd2d
Before joinPoint.getSignature() : MemberDTO com.jehun.section01.aop.MemberService.findByMember(int)
Before joinPoint.getArgs()[0] : 1
target =&gt; findByMember 실행
After joinPoint.getTarget() com.jehun.section01.aop.MemberService@26adfd2d
After joinPoint.getSignature() MemberDTO com.jehun.section01.aop.MemberService.findByMember(int)
After joinPoint.getArgs()[0] 1
MemberDTO{id=2, name='유관순'}</code></pre><p>잘 보면 이전에 <code>jointPoint.getArgs()[0]</code> 부분에서 출력이 없다가, 회원 1명 조회를 하니 생긴 것이 보인다. </p>
<p>다시 해당 코드가 작성된 곳으로 가면 주석으로 <strong>매개변수가 1개 이상이 있는 경우</strong> 라고 하여 회원 1명을 조회하기 위해 인덱스에 1을 넣었기 때문에 그 때 출력된 것이다.</p>
<hr />
<h3 id="afterreturning-어드바이스">AfterReturning 어드바이스</h3>
<p>이번엔 <code>@AfterReturning</code> 어노테이션에 대해서 알아보자.</p>
<p><strong>LoggingAspect에 추가</strong></p>
<pre><code class="language-java">    @AfterReturning(pointcut = &quot;logPointcut()&quot;, returning = &quot;result&quot;)
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        System.out.println(&quot;After Returning result: &quot; + result);

        if (result != null &amp;&amp; result instanceof List) {
            ((List&lt;MemberDTO&gt;) result).add(new MemberDTO(3L, &quot;반환 값 가공&quot;));
        }
    }</code></pre>
<p><code>AfterReturning</code> 어드바이스는 대상 메소드가 <strong>정상적으로 실행된 이후에 실행되는 어드바이스</strong>이다. 미리 작성한 포인트 컷을 설정한다. </p>
<p><code>returning</code> 속성은 <strong>리턴값으로 받아올 오브젝트의 매개변수 이름과 동일해야 한다.</strong> 또한 joinPoint는 반드시 첫 번째 매개변수로 선언해야 한다. </p>
<p>이 어드바이스에서는 <strong>반환 값을 가공할 수도 있다.</strong></p>
<p><strong>AfterReturning 내용의 실행결과</strong></p>
<pre><code>Target -&gt; findAllMembers()
After Returning result: [MemberDTO{id=1, name='홍길동'}, MemberDTO{id=2, name='유관순'}]
After joinPoint.getTarget() com.jehun.section01.aop.MemberService@72ade7e3
After joinPoint.getSignature() List com.jehun.section01.aop.MemberService.findAllMembers()
MemberDTO{id=1, name='홍길동'}
MemberDTO{id=2, name='유관순'}
MemberDTO{id=3, name='반환 값 가공'}</code></pre><p>MemberService 클래스의 findAllMembers 메소드가 실행된 후에 AfterReturning 어드바이스의 실행 내용이 삽입돼 동작하는 것을 확인할 수 있다.</p>
<hr />
<h3 id="afterthrowing-어드바이스">AfterThrowing 어드바이스</h3>
<p>이번에는 <code>@AfterThrowing</code> 에 대해 알아보자.</p>
<p><strong>AfterThrowing</strong> 어드바이스는 예외가 발생했을 때 실행되는 어드바이스이다. 미리 작성한 포인트 컷을 설정한다. </p>
<p>throwing 속성의 이름과 매개변수의 이름이 동일해야 한다. 이 어드바이스에서는 <code>Exception</code> 에 따른 처리를 작성할 수 있다.</p>
<p><strong>LoggingAspect에 추가</strong></p>
<pre><code class="language-java">    @AfterThrowing(pointcut = &quot;logPointcut()&quot;, throwing = &quot;exception&quot;)
    public void logAfterThrowing(Throwable exception) {
        System.out.println(&quot;AfterThrowing exception = &quot; + exception);
    }</code></pre>
<p><strong>Application 에 추가</strong></p>
<pre><code class="language-java">        System.out.println(&quot;===== Select One Members (예외 발생시키기) =====&quot;);
        System.out.println(memberService.findByMember(3));</code></pre>
<p>3을 넣었을 때 IndexOutOfBounds가 발생하게 될 것이고, <code>exception</code>을 통해 출력되는 것을 볼 수 있을 것이다.</p>
<p><strong>실행 결과</strong></p>
<pre><code>===== Select One Members =====
Before joinPoint.getTarget() : com.jehun.section01.aop.MemberService@72ade7e3
Before joinPoint.getSignature() : MemberDTO com.jehun.section01.aop.MemberService.findByMember(int)
Before joinPoint.getArgs()[0] : 3
target =&gt; findByMember 실행
AfterThrowing exception = java.lang.IndexOutOfBoundsException: Index 3 out of bounds for length 3
After joinPoint.getTarget() com.jehun.section01.aop.MemberService@72ade7e3
After joinPoint.getSignature() MemberDTO com.jehun.section01.aop.MemberService.findByMember(int)
After joinPoint.getArgs()[0] 3</code></pre><hr />
<h3 id="around-어드바이스">Around 어드바이스</h3>
<p><code>Around</code> 어드바이스 알아보기</p>
<p><code>Around</code> 어드바이스는 대상 메소드 실행 전/후에 적용되는 어드바이스이다. 미리 작성한 포인트 컷을 설정한다. </p>
<p>Around Advice는 가장 강력한 어드바이스이다. 이 어드바이스는 조인포인트를 완전히 장악하기 때문에 앞에 살펴 본 어드바이스 모두 Around 어드바이스로 조합할 수 있다. </p>
<p>AroundAdvice의 조인포인트 매개변수는 <code>ProceedingJoinPoint</code>로 고정되어 있다. JoinPoint의 하위 인터페이스로 원본 조인포인트의 진행 시점을 제어할 수 있다. </p>
<p>조인포인트 진행하는 호출을 잊는 경우가 자주 발생하기 때문에 주의해야 하며 <strong>최소한의 요건을 충족하면서도 가장 기능이 약한 어드바이스를 쓰는게 바람직하다.</strong></p>
<p><strong>LoggingAspect 에 추가</strong></p>
<pre><code class="language-java">    @Around(&quot;logPointcout()&quot;)
    public Object logArount(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println(&quot;Around Before : &quot; + joinPoint.getSignature().getName());
        Object result = joinPoint.proceed();
        System.out.println(&quot;Around After : &quot; + joinPoint.getSignature().getName());

        return result;
    }</code></pre>
<p><strong>실행 결과</strong> (AfterThrowing과 관련된 인덱스 3을 넣는 코드는 주석처리했다.)</p>
<pre><code>===== Select All Members =====
Around Before : findAllMembers
(중략)
Around After : findAllMembers
MemberDTO{id=1, name='홍길동'}
MemberDTO{id=2, name='유관순'}
MemberDTO{id=3, name='반환 값 가공'}
===== Select One Members =====
Around Before : findByMember
(중략)
Around After : findByMember
MemberDTO{id=2, name='유관순'}</code></pre><hr />
<h1 id="reflection">Reflection</h1>
<blockquote>
<p>Reflection : 컴파일 된 자바 코드에서 필드 및 메소드의 정보를 구해오는 방법이다.
이를 통해 프로그램의 동적인 특성 구현 가능</p>
</blockquote>
<p>예를 들어, 리플렉션을 이용하면 실행 중인 객체의 클래스 정보를 얻어오거나, 클래스 내부의 필드나 메소드에 접근할 수 있다. </p>
<p>스프링에서는 런타임 시 개발자가 등록한 빈을 애플리케이션 내부에서 다루기 위한 기술이기도 한다.</p>
<blockquote>
<p>쓰이는 곳 : 스프링 프레임워크, 마이바티스, 하이버네이트, jackson 등의 라이브러리</p>
</blockquote>
<h2 id="로직을-포함하는-코드-작성">로직을 포함하는 코드 작성</h2>
<p>리플렉션 테스트의 대상이 될 Account 클래스를 생성한다.</p>
<p><strong>Account</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section02.reflection;

public class Account {

    private String backCode;
    private String accNo;
    private String accPwd;
    private int balance;

    public Account() {}

    public Account(String bankCode, String accNo, String accPwd) {
        this.backCode = bankCode;
        this.accNo = accNo;
        this.accPwd = accPwd;
    }

    public Account(String bankCode, String accNo, String accPwd, int balance) {
        this(bankCode, accNo, accPwd);
        this.balance = balance;
    }

    public String getBalance() {

        return this.accNo + &quot; 계좌의 현재 잔액은 &quot; + this.balance + &quot;원 입니다.&quot;;
    }

    public String deposit(int money) {

        String str = &quot;&quot;;

        if(money &gt;= 0) {
            this.balance += money;
            str = money + &quot;원이 입급되었습니다.&quot;;
        }else {
            str = &quot;금액을 잘못 입력하셨습니다.&quot;;
        }

        return str;
    }

    public String withDraw(int money) {

        String str = &quot;&quot;;

        if(this.balance &gt;= money) {
            this.balance -= money;
            str = money + &quot;원이 출금되었습니다.&quot;;
        }else {
            str = &quot;잔액이 부족합니다. 잔액을 확인해주세요.&quot;;
        }

        return str;
    }
}</code></pre>
<h2 id="리플렉션-테스트">리플렉션 테스트</h2>
<h3 id="class">Class</h3>
<p>Class 타입의 인스턴스 -&gt; 해당 클래스의 메타 정보를 가지고 있는 클래스</p>
<pre><code class="language-java">        /* .class 문법을 이용하여 Class 타입의 인스턴스를 생성할 수 있다. */
        Class class1 = Account.class;
        System.out.println(&quot;class1 = &quot; + class1);

        Class class2 = new Account().getClass();
        System.out.println(&quot;class2 = &quot; + class2);

        try {
            Class class3 = Class.forName(&quot;com.ohgiraffers.section02.reflection.Account&quot;);
            System.out.println(&quot;class3 = &quot; + class3);

            Class class4 = Class.forName(&quot;[D&quot;);
            Class class5 = double[].class;
            System.out.println(&quot;class4 = &quot; + class4);
            System.out.println(&quot;class5 = &quot; + class5);

            Class class6 = Class.forName(&quot;[Ljava.lang.String;&quot;);
            Class class7 = String[].class;

            System.out.println(&quot;class6 = &quot; + class6);
            System.out.println(&quot;class7 = &quot; + class7);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }</code></pre>
<p><strong>실행 결과</strong></p>
<pre><code>class1 = class com.jehun.section02.reflection.Account
class2 = class com.jehun.section02.reflection.Account
class3 = class com.jehun.section02.reflection.Account
class4 = class [D
class5 = class [D
class6 = class [Ljava.lang.String;
class7 = class [Ljava.lang.String;</code></pre><hr />
<h3 id="field">Field</h3>
<p>field 정보에 접근 가능하다.</p>
<p><strong>Application에 추가</strong></p>
<pre><code class="language-java">        Field[] fields = Account.class.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(&quot;modifiers = &quot; + Modifier.toString(field.getModifiers())
                    + &quot;, type = &quot; + field.getType()
                    + &quot;, name = &quot; + field.getName());
        }</code></pre>
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/726be4ae-c929-4618-b281-fe45a4cc47da/image.png" /></p>
<hr />
<h3 id="생성자">생성자</h3>
<p>생성자 정보에 접근 가능하다.</p>
<p><strong>Application 에 추가</strong></p>
<pre><code class="language-java">        Constructor[] constructors = Account.class.getConstructors();
        for (Constructor constructor : constructors) {
            System.out.println(&quot;name : &quot; + constructor.getName());

            Class[] params = constructor.getParameterTypes();
            for (Class param : params) {
                System.out.println(&quot;paramType : &quot; + param.getTypeName());
            }
        }</code></pre>
<p><strong>실행 결과</strong></p>
<pre><code>name : com.jehun.section02.reflection.Account
paramType : java.lang.String
paramType : java.lang.String
paramType : java.lang.String
paramType : int
name : com.jehun.section02.reflection.Account
paramType : java.lang.String
paramType : java.lang.String
paramType : java.lang.String
name : com.jehun.section02.reflection.Account</code></pre><p>인스턴스도 생성 가능하다.</p>
<pre><code class="language-java">        try {
            Account acc = (Account) constructors[0].newInstance(&quot;20&quot;, &quot;110-223-123456&quot;, &quot;1234&quot;, 10000);
            System.out.println(acc.getBalance());
        } catch (InstantiationException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        }</code></pre>
<p><strong>실행 결과</strong></p>
<pre><code>110-223-123456 계좌의 현재 잔액은 10000원 입니다.</code></pre><hr />
<h2 id="메소드">메소드</h2>
<p>메소드 정보에 접근 가능하다.</p>
<p>Application에 추가</p>
<pre><code class="language-java">        Method[] methods = Account.class.getMethods();
        Method getBalanceMethod = null;
        for (Method method : methods) {
            System.out.println(Modifier.toString(method.getModifiers()) + &quot; &quot;
                                + method.getReturnType().getSimpleName() + &quot; &quot;
                                + method.getName());
            if (&quot;getBalance&quot;.equals(method.getName())) {
                getBalanceMethod = method;
            }
        }</code></pre>
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8a4fe926-a2d4-4065-a49c-e36a1431eef6/image.png" /></p>
<p>invoke 메소드로 메소드 호출이 가능하다.</p>
<pre><code class="language-java">        Method[] methods = Account.class.getMethods();
        Method getBalanceMethod = null;
        for (Method method : methods) {
            System.out.println(Modifier.toString(method.getModifiers()) + &quot; &quot;
                                + method.getReturnType().getSimpleName() + &quot; &quot;
                                + method.getName());
            if (&quot;getBalance&quot;.equals(method.getName())) {
                getBalanceMethod = method;
            }
        }

        try {
            System.out.println(getBalanceMethod.invoke(((Account)constructors[2].newInstance())));
        } catch (InvocationTargetException e) {
            throw new RuntimeException(e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException(e);
        } catch (InstantiationException e) {
            throw new RuntimeException(e);
        }</code></pre>
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ac6e07af-fe6b-4526-b0ac-3f6880381f70/image.png" /></p>