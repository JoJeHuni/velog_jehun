<h1 id="servlet-listener">Servlet Listener</h1>
<blockquote>
<p><strong>Servlet Listener</strong> : 웹 컨테이너가 관리하는 LifeCycle 사이에 발생하는 <strong>이벤트를 감지</strong>하여, <strong>이벤트 발생 시 그에 대한 일련의 로직을 처리하는 인터페이스</strong></p>
</blockquote>
<h2 id="동작-구조">동작 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5fa8efa8-972b-4a3b-b722-dcf64cc9fdc8/image.png" /></p>
<p>간단하게 사용하는 경우는 이런 것들이 있다.</p>
<ol>
<li><p><strong>Context 변경이 발생하는 경우</strong> (→ 톰캣 컨테이너 자체에 리스너 연결) </p>
<ol>
<li>웹 애플리케이션의 시작, 종료 시점</li>
<li>context에 attribute 추가, 제거, 수정 시점</li>
</ol>
</li>
<li><p><strong>Session 객체에 변경이 발생하는 경우</strong> (→ 세션에서 발생 가능한 이벤트)</p>
<ol>
<li>HttpSession의 시작, 종료 시점</li>
<li>HttpSession에 attribute 추가, 제거 , 수정 시점</li>
</ol>
</li>
<li><p><strong>Request 객체에 변경이 발생하는 경우</strong> (→ request 관련 이벤트)</p>
<ol>
<li>ServletRequest 생성, 소멸 시점</li>
<li>ServletRequest에 attribute 추가, 제거, 수정 시점</li>
</ol>
</li>
</ol>
<hr />
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;JSP - Hello World&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;Listener&lt;/h1&gt;
    &lt;ul&gt;
        &lt;li&gt;&lt;a href=&quot;context&quot;&gt;context listener test&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href=&quot;session&quot;&gt;session listener test&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>이렇게 두고 리스너들에 대해 알아보자.</p>
<h2 id="context-listener">Context Listener</h2>
<p><strong>ContextListener</strong></p>
<pre><code class="language-java">import jakarta.servlet.ServletContextAttributeEvent;
import jakarta.servlet.ServletContextAttributeListener;
import jakarta.servlet.ServletContextListener;
import jakarta.servlet.annotation.WebListener;

@WebListener
public class ContextListener implements ServletContextListener, ServletContextAttributeListener {
    public ContextListener() {
        System.out.println(&quot;context listener 인스턴스 생성&quot;);
    }

    @Override
    public void attributeAdded(ServletContextAttributeEvent event) {
        System.out.println(&quot;context attribute added&quot;);
        System.out.println(&quot;event.getName() = &quot; + event.getName());
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent event) {
        System.out.println(&quot;context attribute replaced&quot;);
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent event) {
        System.out.println(&quot;context attribute removed&quot;);
    }
}</code></pre>
<ul>
<li><p>위 코드에서 인터페이스로 되어 있는 <code>ServletContextListener</code>
웹 애플리케이션의 시작과 종료 시 자동 발생하는 이벤트로, Context 생성/소멸 및 Application 생성/소멸 시점에 로직을 처리한다.</p>
</li>
<li><p><code>ServletContextListener</code> 의 메소드들</p>
<ul>
<li><code>contextInitialized (ServletContextEvent e)</code> : <code>void</code><pre><code>→ 웹 컨테이너가 처음 구동되어 ServletContext가 생성될 때 작동</code></pre></li>
<li><code>contextDestoryed (ServletContextEvent e)</code> : <code>void</code><pre><code>→ 웹 컨테이너가 종료될 때, ServletContext가 소멸될 때 작동</code></pre></li>
</ul>
</li>
</ul>
<p>하지만 ContextListener에서 구현은 하지 않았다. <strong>자동으로 작동하는 것이기 때문</strong></p>
<ul>
<li>구현해둔 메소드들<ul>
<li><code>attributeAdded (ServletContextAttributeEvent e)</code> : <code>void</code>
→ 새로운 속성 값이 추가될 때 실행</li>
<li><code>attributeRemoved (ServletContextAttributeEvent e)</code> : <code>void</code>
→ 속성 값이 제거될 때 실행</li>
<li><code>attributeReplaced (ServletContextAttributeEvent e)</code> : <code>void</code>
→ 속성 값이 변경될 때 실행</li>
</ul>
</li>
</ul>
<p>실행하면 일단은 기본적으로 이렇게 된다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f7e518f1-7ae7-4330-91f9-55b042d36924/image.png" /></p>
<p>파랑 사각형의 Session은 이미 코드는 적어놨기 때문에 나오는 것이니 신경 안 써도 괜찮다.</p>
<p><strong>ContextListenerTestServlet</strong></p>
<pre><code class="language-java">import jakarta.servlet.ServletContext;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(&quot;/context&quot;)
public class ContextListenerTestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(&quot;context listener 확인용 서블릿&quot;);

        // context는 실행하자마자 created되고 그것을 컨텍스트 전용 이벤트가 볼 수 있다. ContextListener는 그것을 위해 만듦
        ServletContext context = req.getServletContext();

        context.setAttribute(&quot;test&quot;, &quot;value&quot;);
        context.setAttribute(&quot;test2&quot;, &quot;value&quot;);
        context.setAttribute(&quot;test2&quot;, &quot;value2&quot;); // test2의 value 수정해보기
        context.removeAttribute(&quot;test&quot;); // 처음 넣었던 것을 test 를 지우는 이벤트 메소드
    }
}</code></pre>
<p>jsp 화면에서 <code>context listener test</code> 링크를 눌렀을 때 실행 결과
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4ea2e3fd-3849-47d3-aced-31233aa77242/image.png" /></p>
<hr />
<h2 id="session-listener">Session Listener</h2>
<p><strong>SessionListener</strong></p>
<pre><code class="language-java">import jakarta.servlet.annotation.WebListener;
import jakarta.servlet.http.HttpSessionAttributeListener;
import jakarta.servlet.http.HttpSessionBindingEvent;
import jakarta.servlet.http.HttpSessionEvent;
import jakarta.servlet.http.HttpSessionListener;

@WebListener
public class SessionListener implements HttpSessionListener, HttpSessionAttributeListener {
    public SessionListener() {
        System.out.println(&quot;SessionListener 인스턴스 생성&quot;);
    }

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println(&quot;session created&quot;);
        System.out.println(&quot;생성된 session id : &quot; + se.getSession().getId());
        System.out.println(&quot;session 객체 타입 : &quot; + se.getSession().getClass().getName());
    }

    @Override
    public void attributeAdded(HttpSessionBindingEvent event) {
        System.out.println(&quot;session attribute added&quot;);
        System.out.println(&quot;session 추가된 attribution &quot; +event.getName() + &quot;, &quot; + event.getValue());
    }
}</code></pre>
<blockquote>
<p><code>HttpSessionListener</code>, <code>HttpSessionAttributeListener</code> 속의 메소드들</p>
<ol>
<li><p><strong>HttpSessionListener</strong></p>
<ul>
<li><p>HTTP session의 생성 및 소멸 시에 작동하는 이벤트로, HttpSession 객체가 생성되거나 소멸되는 시점에 로직을 처리한다.</p>
<ul>
<li><p>Method</p>
<ul>
<li><p><code>sessionCreated (HttpSession e)</code> : void
  → Session 생성 시 실행</p>
</li>
<li><p><code>sessionDestoryed (HttpSession e)</code> : void
  → Session 무효화 될 때 실행</p>
</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ol>
</blockquote>
<p><code>HttpSessionListener</code>의 메소드 2개는 자동으로 실행되기에 
<code>HttpSessionAttributeListener</code>의 메소드들만 구현해봤다.</p>
<blockquote>
<ol start="2">
<li><p><strong>HttpSessionAttributeListener</strong></p>
<ul>
<li><p>Session에 대한 속성의 값이 변경될 경우 발생하는 이벤트로, HttpSession에 대한 속성 값이 변경될 경우 로직을 처리한다.</p>
</li>
<li><p>Method</p>
<ul>
<li><p><code>attributeAdded (HttpSessionBindingEvent e)</code> : void
   → Session에 새로운 속성 값이 추가될 때 실행</p>
<ul>
<li><p><code>attributeRemoved (HttpSessionBindingEvent e)</code> : void
 → Session에 속성 값이 제거될 때 실행</p>
</li>
<li><p><code>attributeReplaced (HttpSessionBindingEvent e)</code> : void
 → Session에 속성 값이 변경될 때 실행</p>
</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ol>
</blockquote>
<p><strong>SessionListenerServlet</strong></p>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

@WebServlet(&quot;/session&quot;)
public class SessionListenerServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        System.out.println(&quot;서블릿에서 session 출력 : &quot; + session.getClass().getName());

        session.setAttribute(&quot;userName&quot;, &quot;hongggildong&quot;);
        session.setAttribute(&quot;age&quot;, 20);
        session.setAttribute(&quot;gender&quot;, &quot;M&quot;);

        session.setAttribute(&quot;userName&quot;, &quot;hong&quot;);
        session.setAttribute(&quot;user&quot;, new UserDTO(&quot;Honggildong&quot;, 20));

        session.removeAttribute(&quot;user&quot;);

        session.invalidate();
    }
}</code></pre>
<p><strong>UserDTO</strong></p>
<pre><code class="language-java">import jakarta.servlet.http.HttpSessionBindingEvent;
import jakarta.servlet.http.HttpSessionBindingListener;

/* 설명. HttpSessionBindingListener는 해당 클래스에 리스너를 추가해야 한다. */
public class UserDTO implements HttpSessionBindingListener {
    private String name;
    private int age;

    public UserDTO() {
    }

    public UserDTO(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return &quot;UserDTO{&quot; +
                &quot;name='&quot; + name + '\'' +
                &quot;, age=&quot; + age +
                '}';
    }

    @Override
    public void valueBound(HttpSessionBindingEvent event) {
        System.out.println(&quot;UserDTO가 담김&quot;);
    }

    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {
        System.out.println(&quot;UserDTO가 제거됨&quot;);
    }
}</code></pre>
<blockquote>
<ol start="3">
<li><strong>HttpSessionBindingListener</strong><ul>
<li>현재 Session에 객체가 추가되거나 해제될 때 발생하는 이벤트로, 사용자의 현재 Session에 바인딩 되거나 해제될 객체가 발생할 경우 로직을 수행한다.</li>
<li>Method<ul>
<li><code>valueBound (HttpSessionBindingEvent e)</code> : void
   → 객체가 Session에 연결될 때 실행</li>
<li><code>valueUnBound (HttpSessionBindingEvnet e)</code> : void
   → 객체가 Session으로부터 연결이 해제될 때 실행</li>
</ul>
</li>
</ul>
</li>
</ol>
</blockquote>
<blockquote>
<p><strong>(참고)</strong> <code>HttpSessionAttributeListener</code> 와 <code>HttpSessionBindingListener</code> 의 차이
        - HttpSessionAttributeListener : <strong>세션에 어떤 속성이 추가/수정/삭제되는 이벤트가 발생한 경우</strong>
        - HttpSessionBindingListener  : <strong>자신이 세션에 속성으로 추가/삭제된 경우</strong></p>
</blockquote>
<p><strong>실행결과</strong></p>
<p>위의 파랑 사각형에서 SessionListener 의 기본생성자가 호출된 것은 봤을 것이다.
일단 실행했을 때는 이런 내용들이 출력된다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0b84d624-3571-4be1-9ebe-971527a6ee0f/image.png" /></p>
<p>jsp 화면에 있는 SessionListener 링크를 누르면 아래 사진처럼 출력된다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/dcc09956-1d2f-4c3e-8dbd-69b4dd381763/image.png" /></p>
<hr />
<p><strong>예제는 없지만 추가 Listener 들</strong></p>
<ol start="4">
<li><p><code>HttpSessionActivationListener</code></p>
<ul>
<li><p>HTTP session이 활성화 또는 비활성화를 감지했을 때 작동하며, HttpSession이 새로 생성되어 활성화될 때 로직을 처리한다.</p>
</li>
<li><p>Method</p>
<ul>
<li><p><code>sessionDidActivate (HttpSessionEvent e)</code> : void</p>
<p>  → Session 활성화 될 때</p>
</li>
<li><p><code>sessionWillPassivate (HttpSessionEvent e)</code> : void</p>
<p>  → Session 비활성화 되려고 할 때</p>
</li>
</ul>
</li>
</ul>
</li>
</ol>
<hr />
<h2 id="request-listener">Request Listener</h2>
<ol>
<li><p><code>ServletRequestListener</code></p>
<ul>
<li><p>클라이언트로부터 서버로 요청 시 자동 발생하는 이벤트로, request 객체가 생성되거나 소멸되는 시점에 로직을 처리한다.</p>
</li>
<li><p>Method</p>
<ul>
<li><p><code>requestInitialized (ServletRequestEvent e)</code> : void</p>
<p>  → request 생성 시 작동</p>
</li>
<li><p><code>requestDestroyed (ServletRequestEvent e)</code> : void</p>
<p>  → request 소멸 시 작동</p>
</li>
</ul>
</li>
</ul>
</li>
</ol>
<ol start="2">
<li><p><code>ServletRequestAttributeListener</code></p>
<ul>
<li><p>request에 대한 속성의 값이 변경될 경우 발생하는 이벤트로, request객체에 대한 속성 값이 변경될 경우 로직을 처리한다.</p>
</li>
<li><p>Method</p>
<ul>
<li><p><code>attributeAdded (ServletRequestAttributeEvent e)</code> : void</p>
<p>  → request에 새로운 속성 값이 추가될 때 실행</p>
</li>
<li><p><code>attributeRemoved (ServletRequestAttributeEvent e)</code> : void</p>
<p>  → request에 속성 값이 제거될 때 실행</p>
</li>
<li><p><code>attributeReplaced (ServletRequestAttributeEvent e)</code> : void</p>
<p>  → request에 속성 값이 변경될 때 실행</p>
</li>
</ul>
</li>
</ul>
</li>
</ol>