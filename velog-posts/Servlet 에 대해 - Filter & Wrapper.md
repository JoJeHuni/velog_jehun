<blockquote>
<p>Filter : HTTP 요청과 응답 사이에서 전달되는 데이터를 가로채어, 서비스에 맞게 변경하고 걸러내는 필터링 작업을 수행한다.</p>
</blockquote>
<ul>
<li><p>필터 설정에 따라 해당하는 요청 및 응답 시에 <strong>반드시</strong> 거쳐야 하며, 비밀번호 암호화 처리, 인코딩 설정 등 공통 관리에 해당하는 기능을 수행할 수 있다.</p>
</li>
<li><p>필터는 인증 필터, 압축 필터, 리소스 접근 트리거 이벤트 필터, 로깅 필터, 이미지 변환 필터, 토크나이져 필터 등 다양하게 활용 가능하다.</p>
</li>
</ul>
<h2 id="servlet-filter-처리-내용">Servlet Filter 처리 내용</h2>
<ul>
<li><p>Request에 대한 처리</p>
<ul>
<li>보안 관련 사항</li>
<li>요청 header와 body 형식 지정</li>
<li>요청에 대한 log 기록 유지</li>
</ul>
</li>
<li><p>Response에 대한 처리</p>
<ul>
<li><p>응답 stream 압축</p>
</li>
<li><p>응답 stream 내용 추가 및 수정</p>
</li>
<li><p>새로운 응답 작성</p>
</li>
<li><p>여러 가지 필터를 연결(= chain, 서로 호출)하여 사용할 수 있다.</p>
</li>
</ul>
</li>
</ul>
<h2 id="동작-구조">동작 구조</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e916717-869b-44be-b82c-4e1173a884e0/image.png" /></p>
<p>세션은 서블릿의 실행 전, 후로 동작하기 때문에 서블릿의 service() 메소드 실행 전, 후에 작동한다.</p>
<ul>
<li>필터가 여러 개라면 <code>stack</code> 방식으로 순차적으로 수행된다.</li>
</ul>
<p>사진으로 보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/343bf98c-7d97-44f8-9b9c-c8e0726c5f01/image.png" /></p>
<h3 id="filter-chain-interface">Filter Chain (Interface)</h3>
<p>위에서 말한대로 Filter가 여러 개인 경우에는 Filter Chain으로 작동할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1506ab47-2f0d-416d-94fd-e1ae5b239f70/image.png" /></p>
<ul>
<li>Chain처럼 서로 연결되어 있는 <code>Filter</code>를 <code>doFilter()</code> 메소드를 이용하여 순차적으로 실행시키는 인터페이스이다.</li>
<li><code>doFilter()</code> 메소드는 chain으로 연결되어 있는 다음 필터 또는 서블릿을 실행하는 메소드이다.</li>
</ul>
<p>한 번 활용 예제를 보자.</p>
<hr />
<h2 id="활용-예제">활용 예제</h2>
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;JSP - Hello World&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;Filter&lt;/h1&gt;
&lt;h3&gt;필터의 라이프 사이클&lt;/h3&gt;
&lt;ul&gt;
    &lt;li&gt;&lt;a href=&quot;first/filter&quot;&gt;Filter 사용하기&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<pre><code class="language-java">import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;

import java.io.IOException;

@WebFilter(&quot;/first/*&quot;)
public class FirstFilter implements Filter {

    public FirstFilter() {
        System.out.println(&quot;FirstFilter 인스턴스 생성&quot;);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println(&quot;FirstFilter init 호출됨&quot;);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println(&quot;FirstFilter doFilter 호출됨&quot;);

        /* 설명. filterChain에서 제공하는 doFilter를 활용하여 다음 필터 또는 서블릿을 진행시킬 수 있다. */
        filterChain.doFilter(servletRequest, servletResponse);
        // 위의 doFilter로부터 서블릿 -&gt; 서비스 -&gt; 리포지토리 -&gt; DB 를 다녀온다.

        System.out.println(&quot;서블릿을 다녀온 후&quot;);

    }

    @Override
    public void destroy() {
        System.out.println(&quot;FirstFilter destroy 호출됨&quot;);
    }
}</code></pre>
<p>위의 코드에서 Filter는 아래 코드로 이동해서 <code>doGet()</code> 메소드가 호출된다.</p>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(&quot;/first/filter&quot;)
public class FirstFilterTestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(&quot;서블릿으로 get 요청 확인&quot;);
    }
}</code></pre>
<p><strong>실행 시 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/64319571-f55e-4f71-baac-f774559fb331/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/12ec95ae-6aa2-449f-82b9-186eeda302a6/image.png" /></p>
<hr />
<h3 id="활용-예제-2">활용 예제 2</h3>
<p>위의 예제에 추가를 해보자.</p>
<p>일단 하기 전에 <code>build.gradle</code>에 추가할게 있다.
<code>dependency</code>에 추가하자.</p>
<pre><code>    // https://mvnrepository.com/artifact/org.springframework.security/spring-security-crypto
    implementation 'org.springframework.security:spring-security-crypto:5.7.3'
    // https://mvnrepository.com/artifact/commons-logging/commons-logging
    implementation 'commons-logging:commons-logging:1.2'</code></pre><p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;JSP - Hello World&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;Filter&lt;/h1&gt;
&lt;h3&gt;필터의 라이프 사이클&lt;/h3&gt;
&lt;ul&gt;
    &lt;li&gt;&lt;a href=&quot;first/filter&quot;&gt;Filter 사용하기&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;

&lt;hr&gt;
&lt;!--build.gradle에 단방향 암호화를 위한 bcrypt 관련 라이브러리 추가ㅣ
(2개의 라이브러리 추가)--&gt;
&lt;h3&gt;필터의 활용&lt;/h3&gt;
&lt;form action=&quot;member/regist&quot; method=&quot;post&quot;&gt;
    &lt;label for=&quot;test&quot;&gt;아이디: &lt;/label&gt;
    &lt;input id=&quot;test&quot; type=&quot;text&quot; name=&quot;userId&quot;&gt;
    &lt;br&gt;
    &lt;label&gt;비밀번호: &lt;/label&gt;
    &lt;input type=&quot;password&quot; name=&quot;password&quot;&gt;
    &lt;br&gt;
    &lt;label&gt;이름: &lt;/label&gt;
    &lt;input type=&quot;text&quot; name=&quot;name&quot;&gt;
    &lt;br&gt;
    &lt;button type=&quot;submit&quot;&gt;가입하기&lt;/button&gt;
&lt;/form&gt;

&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><strong>RegisterMemberServlet</strong></p>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

import java.io.IOException;

@WebServlet(&quot;/member/register&quot;)
public class RegisterMemberServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String userId = req.getParameter(&quot;userId&quot;);
        String password = req.getParameter(&quot;password&quot;);
        String name = req.getParameter(&quot;name&quot;);

        System.out.println(&quot;userId = &quot; + userId);
        System.out.println(&quot;password = &quot; + password);
        System.out.println(&quot;name = &quot; + name);

        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        System.out.println(&quot;비밀번호가 pass01인지 확인 : &quot; + passwordEncoder.matches(&quot;pass01&quot;, password));
        System.out.println(&quot;비밀번호가 pass02인지 확인 : &quot; + passwordEncoder.matches(&quot;pass02&quot;, password));
    }
}</code></pre>
<p><strong>PasswordEncryptFilter</strong></p>
<pre><code class="language-java">import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;
import jakarta.servlet.http.HttpServletRequest;

import java.io.IOException;

@WebFilter(&quot;/member/*&quot;)
public class PasswordEncryptFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println(&quot;패스워드 암호화 필터의 doFilter 실행&quot;);

        RequestWrapper requestWrapper = new RequestWrapper((HttpServletRequest) servletRequest);

        filterChain.doFilter(requestWrapper, servletResponse);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}</code></pre>
<p><strong>RequestWrapper</strong></p>
<pre><code class="language-java">import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletRequestWrapper;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

public class RequestWrapper extends HttpServletRequestWrapper {
    public RequestWrapper(HttpServletRequest request) {
        super(request);
    }

    @Override
    public String getParameter(String key) {

        /* 설명. 'password'라는 키 값이 들어오면 암호화를 하는 우리만의 getParameter 메소드 재정의 */
        String value = &quot;&quot;;
        if (&quot;password&quot;.equals(key)) {
            System.out.println(&quot;패스워드 꺼낼 시&quot;);
            BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
            value = passwordEncoder.encode(super.getParameter(&quot;password&quot;));
        } else {
            value = super.getParameter(key);
        }

        return value;
    }
}</code></pre>
<p><strong>EncodingFilter</strong></p>
<pre><code class="language-java">import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;
import jakarta.servlet.http.HttpServletRequest;

import java.io.IOException;

@WebFilter(&quot;/member/*&quot;)
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        /* 설명. 우린 톰캣 10 버전인데 톰캣 10 버전 미만일 경우에는 post 요청에 대해 인코딩 설정을 해줘야 한다. */
        /* 설명. 필터를 활용해 request 객체에 인코딩 설정을 적용하고(전처리) 다음 필터나 서블릿으로 넘겨준다. */
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        if(&quot;POST&quot;.equals(httpServletRequest.getMethod())) {
            httpServletRequest.setCharacterEncoding(&quot;UTF-8&quot;);
        }

        filterChain.doFilter(httpServletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}</code></pre>
<p><strong>실행했을 때의 출력되는 내용들</strong></p>
<pre><code>user01
pass01
홍길동</code></pre><p>위 데이터를 넣어봤다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e778aebe-30c8-42e4-a7e6-71671fb40c2d/image.png" /></p>
<p>필터가 여러 개이므로 index.jsp 에서 가장 먼저 부른건 <code>RegisterMemberServlet</code> 클래스이어도, 잘 보면 <code>@WebFilter</code> 어노테이션들로 이루어진 나머지 클래스들이 stack 형태로 실행되는 것이다.</p>
<blockquote>
<p>순서는? -&gt; 직접 처리해줄 수도 있고 냅두면 자동으로 한다.</p>
</blockquote>
<p>잘 보면 Wrapper에 대해서도 적혀 있는데 자료형에서 나온 Wrapper 클래스와 다른 것이다.</p>
<hr />
<h1 id="servlet-wrapper">Servlet Wrapper</h1>
<ul>
<li><p>관련 클래스(<code>ServletRequest</code>, <code>ServletResponse</code>, <code>HttpServletRequest</code>,
<code>HttpServletResponse</code>)를 내부에 보관하며 해당 인터페이스를 구현한 객체를 참조하여 구현 메소드를 위임한다.</p>
</li>
<li><p>Java Event처리의 Adapter Class와 비슷한 기능을 한다고 볼 수 있다.</p>
</li>
<li><p>사용자가 별도의 request나 response 객체를 생성하여 활용할 때 Wrapper Class를 상속하여 활용하면, 편하게 원하는 Class만 재정의하여 사용할 수 있다.</p>
</li>
</ul>
<h2 id="wrapper-class">Wrapper Class</h2>
<ul>
<li><p><code>HttpServletRequestWrapper</code></p>
<ul>
<li><p>요청한 정보를 변경하는 Wrapper Class로, <code>HttpServletRequest</code> 객체를 매개로 하는 생성자를 가진다.</p>
<pre><code class="language-java">  public SampleWrapper(HttpServletRequest wrapper) {
      super(wrapper);
  }</code></pre>
</li>
</ul>
</li>
<li><p><code>HttpServletResponseWrapper</code></p>
<ul>
<li><p>응답할 정보를 변경하는 Wrapper Class로, HttpServletResponse 객체를 매개로 하는 생성자를 가진다.</p>
<pre><code class="language-java">  public SampleWrapper(HttpServletResponse wrapper) {
      super(wrapper);
  }</code></pre>
</li>
</ul>
</li>
</ul>
<hr />
<h1 id="참고--암호화-및-bcrypt">참고 : 암호화 및 Bcrypt</h1>
<p>예제 2에서 추가한 Bcrypt에 대한 이야기다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/72516168-1d15-43ba-bb05-1fe9882dec07/image.png" /></p>
<p>이 그림은 중간에 패킷 정보를 빼내 가져가는 해킹 기법인 <code>패킷스니핑</code> 기법으로 대비하기 위해 암호화처리가 필요하다.</p>
<p>물론 저런 해킹에 대비해서 DB에도 암호화된 데이터가 필요하다.</p>
<ul>
<li><p><strong>서버는 양방향 암호화 처리를 한다.</strong></p>
<ul>
<li>암호화란 평문을 다이제스트(= 기존 문자열을 변환한 일정 길이의 문자열)로 변경하는 것이고, 복호화는 다이제스트를 다시 평문으로 변경하는 것이다.</li>
<li>이때 암호화는 가능하지만 복호화는 불가한 것이 단방향 암호화이고, 암호화와 복호화 모두 가능한 것이 양방향 암호화이다.</li>
</ul>
</li>
<li><p>저장한 서버도 관리자도 알아서는 안되는 고객의 개인 정보 등이 있을 수 있으므로 데이터베이스는 단방향 암호화 처리를 한다.</p>
</li>
</ul>
<blockquote>
<p>💡 <strong>BCrypt  암호화</strong></p>
</blockquote>
<ol>
<li>비밀번호를 데이터베이스에 저장할 목적으로 설계된 암호화 알고리즘이다.</li>
<li>랜덤 솔팅 기법을 적용한 다이제스트 생성을 지연시킨 단방향 해시 암호화 알고리즘이다.</li>
<li>암호화는 가능하나 복호화가 불가능한 높은 수준의 암호화 알고리즘이다.</li>
</ol>
<ul>
<li><p>해시 알고리즘이란 어떤 메시지를 넣더라도 해시 함수를 통과하면 동일한 길이의 랜덤한 문자열이 반환되는 것으로, 빠른 속도로 다이제스트 생성이 가능하다.</p>
<p>  이러한 해시 알고리즘은 다이제스트 생성 방식이 동일하므로 아래 두 가지 방법으로 패턴만 알아내면 복호화가 가능해진다.</p>
<ol>
<li>무차별 대입  → 모든 경우의 수에 대해 다이제스트를 생성해 맞춰보면서 패턴 탐색</li>
<li>사전식 대입 → 사전 속 단어를 다이제스트로 변경해 다이제스트와 비교해 패턴 탐색</li>
</ol>
</li>
</ul>
<blockquote>
<p>따라서 위와 같은 패턴 탐색을 방지하고자 Bcrypt 암호화에서 적용한 salting 기법은 원문만 암호화하지 않고 지정한 다른 글자(= salt값)를 붙여 암호화하는 것이고, BCrypt는 암호화할 때마다 랜덤한 salt값을 이용한다.</p>
</blockquote>