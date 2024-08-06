<p>Servlet 에 대해 개념도 알아보고 활용도 해보자.</p>
<h1 id="servlet">Servlet</h1>
<h2 id="network-통신">Network 통신</h2>
<h3 id="server-client-model">Server-Client Model</h3>
<p>서버 : 특정 서비스를 제공하는 컴퓨터
클라이언트 : 서비스를 이용하는 사용자</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/75d09141-4ea8-44f2-802d-519ed2b6c136/image.png" /></p>
<p>나뉜 이유?</p>
<ul>
<li>책임 전가 + 서버 부담 감소<ul>
<li>실행 파일(.exe)을 클라이언트에게 제공함으로써 서버의 부담이 감소되면서, 클라이언트에게 책임을 전가할 수 있다. (ex. OS 문제로 인해 실행 못 하는 경우)</li>
<li><strong>하지만</strong> 책임을 전가한만큼 업데이트를 하지 않으면 보안에 문제가 생겨서 해킹을 당할 수 있는 위험도 있긴 하다..</li>
</ul>
</li>
</ul>
<hr />
<h3 id="server-종류">Server 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>Web Server</td>
<td>웹 브라우저와 HTTP 프로토콜을 사용하여 사용자의 요구에 따른 특정 서비스를 제공하는 서버</td>
</tr>
<tr>
<td>Mail Server</td>
<td>인터넷을 통해 사용자 간의 전자 우편을 주고 받는 서비스 제공</td>
</tr>
<tr>
<td>FTP Server</td>
<td>서버 내에 파일을 업로드, 다운로드 할 수 있도록 파일 관리 기능 제공</td>
</tr>
<tr>
<td>Telnet Server</td>
<td>Terminal, 텍스트로만 이루어진 창에서 특정 명령어를 통해 원격지 서버를 접속 및 관리</td>
</tr>
<tr>
<td>Database Server</td>
<td>데이터를 저장하고, 원격지에 접속할 경우 권한에 따라 해당 데이터를 열람, 추가, 수정, 삭제하는 기능 처리</td>
</tr>
</tbody></table>
<hr />
<h3 id="개별local-프로그램과-서버-프로그램의-특징">개별(local) 프로그램과 서버 프로그램의 특징</h3>
<ul>
<li><strong>개별(local) 프로그램 특징 및 단점</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ef612b3c-1f3e-4e48-bf10-8288c3b40789/image.png" /></li>
</ul>
<ol>
<li>프로그램 업데이트 발생 시 각각 다시 다운로드 해야 한다.</li>
<li>각 프로그램에서 생성된 데이터 개별 저장되므로 공유 불가하다.</li>
</ol>
<ul>
<li><strong>서버 프로그램 특징</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/96229037-85cc-462a-bfd5-7104c27cf851/image.png" /></li>
</ul>
<ol>
<li>프로그램 업데이트 발생 시 서버가 상관하지 않아도 클라이언트가 서버에서 다운 받아 업데이트를 개별적으로 진행한다.</li>
<li>데이터는 서버에 일괄 저장된다.</li>
</ol>
<hr />
<h2 id="web-통신">Web 통신</h2>
<h3 id="web-통신-구조">Web 통신 구조</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cd493502-6faf-463c-afcf-e8f1bfd9c83a/image.png" /></p>
<hr />
<h3 id="web-server">Web Server</h3>
<p>사용자에게 HTML 페이지 같이 <strong>정적</strong>인 요소들을 화면에 보여주는 역할을 하는 HTTP 프로토콜을 통해 웹 브라우저에 제공하는 서버를 뜻한다.</p>
<ul>
<li>APACHE</li>
<li>Microsoft IIS</li>
<li>NGINX</li>
</ul>
<p>위와 같은 것들이 있다.</p>
<hr />
<h3 id="was란">WAS란</h3>
<p>사용자가 요청한 서비스의 결과를 스크립트 언어 등으로 가공하여 생성한 <strong>동적</strong>인 페이지를 사용자에게 보여주는 역할</p>
<blockquote>
<p>웹 서버가 웹 애플리케이션 서버에 요청하면, 웹 애플리케이션 서버가 해당 프로그램을 실행하는 방식
-&gt; 동일 프로그램에 여러 요청이 있을 시, 한 개의 프로그램을 실행해 다수 요청을 처리한다.</p>
</blockquote>
<ul>
<li>Web Application이란<ul>
<li>정적인 페이지를 제공하는 미리 완성된 클라이언트 프로그램이다.</li>
<li>브라우저만 설치되어 있으면 서버에 요청해 언제든 볼 수 있다.</li>
<li>클라이언트 요청(= url 요청)에 따라 서버가 응답을 html 태그 문법인 텍스트로 전송하면, 브라우저가 해당 텍스트를 해석해 띄워주는 방식이다.</li>
<li>서버는 클라이언트 요청(request)에 반드시 응답(response)해야 한다.<ul>
<li>응답할 내용이 없으면, 응답 내용이 없다는 응답이라도 해야 한다.</li>
</ul>
</li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/70e0ffae-f047-4720-a221-900399727c07/image.png" /></p>
<p>예시)</p>
<ul>
<li>Apache Tomcat</li>
<li>WildFly (or Jboss)</li>
<li>JEUS</li>
</ul>
<p>위와 같은 것들이 있다.</p>
<hr />
<h3 id="web-와-was">Web 와 WAS</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/feaa6bfa-952f-401a-8ac8-8cf2da236eed/image.png" /></p>
<ul>
<li>web(html)은 사전에 작성된 화면으로 정적인 페이지(사전에 작성된 화면)를 의미한다. 서버는 따로 두고 일단 클라이언트 요청에 대해 web에서 응답한 뒤에, 처리할 동적 요청 등 필요에 따라 was에 요청하여 응답한다.</li>
<li>WAS는 Servlet을 보관하다가 서블릿 라이프사이클에 따라 생성, 소멸 등을 주관하는 역할을 하여, 다른 말로 Servlet Container라고도 부른다.</li>
</ul>
<table>
<thead>
<tr>
<th>구분</th>
<th>장점</th>
<th>단점</th>
</tr>
</thead>
<tbody><tr>
<td>Web Server</td>
<td>- 빠른 처리 속도 <br />→ 요청에 대한 결과 페이지만 전송<br />- 쉬운 구현<br />→ HTML같은 단순한 문서만으로 구성</td>
<td>- 한정적인 서비스<br />→ 만들어진 정보만 보여주므로 서비스가 한정적임<br />- 글의 추가, 수정, 삭제가 어려움<br />→ 문서의 내용이 변경될 경우 직접 수정</td>
</tr>
<tr>
<td>WAS</td>
<td>- 서비스의 다양성<br />→ 여러 데이터 활용 가능<br />- 글의 추가, 수정, 삭제가 쉬움<br />→ 문서 내용이 변경되면 직접 수정하지 않음</td>
<td>- 느린 처리 속도<br />→ 데이터를 처리하여 결과 전송<br />- 어려운 구현<br />→ 서비스에 해당하는 소스 직접 작성</td>
</tr>
</tbody></table>
<hr />
<h2 id="servlet-mapping">Servlet mapping</h2>
<h3 id="servlet-mapping-방법">Servlet mapping 방법</h3>
<ol>
<li>web.xml 이용한 방법</li>
<li><code>@annotation</code> 이용한 방법</li>
</ol>
<hr />
<h3 id="servlet-mapping-구조">Servlet mapping 구조</h3>
<p>클라이언트가 서블릿의 url로 요청을 보내면, 톰캣이 이를 해석해 매핑된 서블릿으로 연결하는 구조이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/40bd7dc0-8ed1-48ac-ae7d-50aed33103fa/image.png" /></p>
<p>mapping 경로가 잘못 되면 tomcat 실행 자체가 되지 않는다.</p>
<hr />
<h3 id="webxml-사용해보기">web.xml 사용해보기</h3>
<pre><code class="language-xml">    &lt;servlet&gt;
        &lt;servlet-name&gt;xmlmapping&lt;/servlet-name&gt;
        &lt;servlet-class&gt;실제 클래스 명t&lt;/servlet-class&gt;
    &lt;/servlet&gt;
    &lt;servlet-mapping&gt;
        &lt;servlet-name&gt;xmlmapping&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/xml-lifecycle&lt;/url-pattern&gt;
    &lt;/servlet-mapping&gt;</code></pre>
<pre><code class="language-java">import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.http.HttpServlet;

import java.io.IOException;

public class LifeCycleTestServlet extends HttpServlet {

    /* 설명. 기본 생성자 */
    public LifeCycleTestServlet() {
        System.out.println(&quot;xml 방식 기본 생성자 실행&quot;);
    }

    /* 설명. 서블릿의 요청이 최초인 경우 서블릿 객체를 생성하고 자동으로 호출하게 되는 메소드 */
    @Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println(&quot;xml 매핑 init() 메소드 호출&quot;);
        System.out.println(&quot;실제로는 요청에 따라 doGet() 또는 doPost()가 실행됨&quot;);
    }

    /* 설명.
     *  서블릿 컨테이너에 의해 호출되며 최초 요청 시에만 init() 이후에 동작하고, 2번재부터는 service()만 호출되게 된다.
    * */
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println(&quot;xml 매핑 service() 메소드 호출&quot;);
    }

    /* 설명. 컨테이너가 종료될 때 호출하는 메소드이며 주로 자원을 반납하는 용도로 사용된다. */
    @Override
    public void destroy() {
        System.out.println(&quot;xml 매핑 destroy() 메소드 호출&quot;);
    }
}</code></pre>
<hr />
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;Servlet Life&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;라이프 사이클 테스트&lt;/h1&gt;
&lt;br/&gt;
&lt;a href=&quot;xml-lifecycle&quot;&gt;라이프 사이클 테스트(xml)&lt;/a&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><code>LifeCycleTestServlet</code>를 실행해서 링크도 클릭해보면 잘 출력되는지 확인 가능하다.</p>
<hr />
<h3 id="annotation-사용">@annotation 사용</h3>
<pre><code class="language-java">import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;

import java.io.IOException;

@WebServlet(value=&quot;/annotation-lifecycle&quot;)
public class LifeCycleTestServlet extends HttpServlet {

    public LifeCycleTestServlet() {
        System.out.println(&quot;어노테이션 방식 기본 생성자 실행&quot;);

    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println(&quot;어노테이션 매핑 init() 메소드 호출&quot;);
        System.out.println(&quot;실제로는 요청에 따라 doGet() 또는 doPost()가 실행됨&quot;);
    }

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println(&quot;어노테이션 매핑 service() 메소드 호출&quot;);
    }

    @Override
    public void destroy() {
        System.out.println(&quot;어노테이션 매핑 destroy() 메소드 호출&quot;);

    }
}</code></pre>
<hr />
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;Servlet Life&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;라이프 사이클 테스트&lt;/h1&gt;
&lt;br/&gt;
&lt;a href=&quot;xml-lifecycle&quot;&gt;라이프 사이클 테스트(xml)&lt;/a&gt; &lt;br&gt;
&lt;a href=&quot;annotation-lifecycle&quot;&gt;라이프 사이클 테스트(annotation)&lt;/a&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>또한 잘 되는 것을 알 수 있다.</p>
<ul>
<li>어노테이션만 추가해주면 되는 간편함이 있다.</li>
<li>하지만 별도의 문서 등으로 관리하지 않으면 시스템의 모든 서블릿 매핑 정보를 한눈에 볼 수 없다는 <strong>단점</strong>이 있다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/75c7439b-b77b-497b-9cb0-dfd50703cb2e/image.png" /></p>
<hr />
<h2 id="servlet-구동">Servlet 구동</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a724b476-e83d-4b4a-a570-99221d0eff5f/image.png" /></p>
<hr />
<h1 id="servlet-method">Servlet Method</h1>
<h2 id="http-데이터-전송-방식">HTTP 데이터 전송 방식</h2>
<ol>
<li>브라우저에서 요청 정보를 HTTP 객체에 담아 전송하는 방식</li>
</ol>
<p>요청과 응답을 주고받는 패킷은 요청 정보와 응답 정보를 직렬화한 Byte단위의 문자열 데이터로, 인코딩과 디코딩이 필요하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2028aec6-7b83-420a-976e-8e4e63729b15/image.png" /></p>
<ol start="2">
<li>전달받은 HTTP객체를 서버(=Tomcat)이 해석하여 요청을 처리할 서블릿을 호출한다.</li>
</ol>
<ul>
<li>서블릿의 <code>service()</code> 메소드에서는 <code>request</code>, <code>response</code> 요청 정보를 가지고 처리 로직을 거쳐 응답한다.</li>
</ul>
<p>전송 방식 중에 get 방식, post 방식에 대해 메소드와 함께 알아보자.</p>
<hr />
<h3 id="데이터-전송-방식에-따른-servlet-method">데이터 전송 방식에 따른 Servlet Method</h3>
<p>서블릿이 get/post의 두 방식 중 하나로 요청 정보를 전달 받으면, request와 response를 전달하면서 해당하는 처리 메소드(<code>doGet()</code> 메소드 또는 <code>doPost()</code> 메소드)를 호출한다.</p>
<p>HTML에서 method 속성을 이용해 방식을 결정하며, default는 get 방식이다.</p>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(&quot;/request-service&quot;)
public class ServiceMethodTestServlet extends HttpServlet {
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println(&quot;req = &quot; + req);
        System.out.println(&quot;res = &quot; + res);

        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;

        String httpMethod = request.getMethod();
        System.out.println(&quot;요청 방식 : &quot; + httpMethod);

        if (&quot;GET&quot;.equals(httpMethod)) {
            doGet(request, response);
        } else if(&quot;POST&quot;.equals(httpMethod)) {
            doPost(request, response);
        }
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(&quot;GET 요청을 처리할 메소드 호출 중&quot;);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(&quot;POST 요청을 처리할 메소드 호출 중&quot;);
    }
}</code></pre>
<hr />
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;JSP - Hello World&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;Service Method&lt;/h1&gt;
    &lt;h3&gt;GET 방식의 요청&lt;/h3&gt;
    &lt;h4&gt;a 태그의 href 속성값 변경&lt;/h4&gt;
    &lt;a href=&quot;request-service&quot;&gt;서비스 메소드 요청하기&lt;/a&gt;

    &lt;h3&gt;POST 방식의 요청&lt;/h3&gt;
    &lt;h4&gt;form 태그의 method 속성을 post로 설정&lt;/h4&gt;
    &lt;form action=&quot;request-service&quot; method=&quot;post&quot;&gt;
        &lt;input type=&quot;text&quot; name=&quot;number&quot; value=&quot;123&quot;&gt;
        &lt;input type=&quot;text&quot; name=&quot;name&quot; value=&quot;홍길동&quot;&gt;
        &lt;input type=&quot;submit&quot; value=&quot;POST 방식 요청 전송&quot;&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>위 2가지 코드를 통해 실행해보면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c2ae42ed-514c-49dc-a115-6f14eae74fc8/image.png" /></p>
<p>이런 형태가 되는데 index.jsp 에서의 <code>&lt;form action=&quot;request-service&quot; method=&quot;post&quot;&gt;</code> 이 부분에서 <code>method =&quot;get&quot;</code> 으로 바꿔서 실행할 수도 있다.</p>
<p>post 상태에서 버튼을 눌렀을 때 호출되는 것들은 아래 사진과 같다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a5baab71-7e8b-436d-bcde-6695a4ae2c1e/image.png" /></p>
<hr />
<h1 id="servlet-method-parameter">Servlet Method Parameter</h1>
<h2 id="httpservletrequest">HttpServletRequest</h2>
<h3 id="역할">역할</h3>
<p>HTTP Servlet을 위한 요청 정보(request information)를 제공하는 메소드를 지정</p>
<table>
<thead>
<tr>
<th>method 명</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>getParameter(String)</td>
<td>client가 전송한 값의 명칭이 매개변수와 같은 값 가져옴</td>
</tr>
<tr>
<td>getParameterNames()</td>
<td>client가 전송한 값의 명칭 가져옴</td>
</tr>
<tr>
<td>getParameterValues(String)</td>
<td>client가 전송한 값이 여러 개이면 배열로 가져옴</td>
</tr>
<tr>
<td>getParameterMap()</td>
<td>client가 전송한 값 전체를 Map방식으로 가져옴</td>
</tr>
<tr>
<td>setAttribute(String, object)</td>
<td>request 객체로 전달할 값을 String 이름-Object 값으로 설정</td>
</tr>
<tr>
<td>getAttribute(String)</td>
<td>매개변수와 동일한 객체 속성 값 가져옴</td>
</tr>
<tr>
<td>removeAttribute(String)</td>
<td>request객체에 저장된 매개변수와 동일한 속성 값 삭제</td>
</tr>
<tr>
<td>setCharacterEncoding(String)</td>
<td>전송 받은 request객체 값들의 CharaterSet 설정</td>
</tr>
<tr>
<td>getRequestDispatcher(String)</td>
<td>컨테이너 내에서 request, response객체를 전송하여 처리한 컴포넌트(jsp파일 등)를 가져옴 (forward() method와 같이 사용)</td>
</tr>
</tbody></table>
<hr />
<h3 id="request-header">Request Header</h3>
<p>HttpServletResponse 아래에 있는 활용에는 사용되지 않았지만, 그래도 <strong>HttpServletRequest</strong> 안에 포함되는 내용이기에 추가했다.</p>
<ol>
<li>General header<ul>
<li>요청 및 응답 모두에 적용되지만 최종적으로는 body에 전송되는 것과 관련이 없는 헤더이다.</li>
</ul>
</li>
<li>Request header<ul>
<li>Fetch될 리소스나 클라이언트 자체에 대한 상세 정보를 포함하는 헤더이다.</li>
</ul>
</li>
<li>Response header<ul>
<li>위치나 서버 자체와 같은 응답에 대한 부가적인 정보를 갖는 헤더이다.</li>
</ul>
</li>
<li>Entity header<ul>
<li>컨텐츠 길이나 MIME타입과 같은 엔티티 body에 대한 상세 정보를 포함하는 헤더이다. (요청 응답 모두 사용되며, 메시지 바디의 컨텐츠를 나타내기에 GET요청은 해당하지 않는다.)</li>
</ul>
</li>
<li>getHeader() 메소드로 확인 가능한 값</li>
</ol>
<pre><code>| header 속성 | 값 |
| --- | --- |
| accept | 요청을 보낼 때 서버에게 요청할 응답 타입 |
| accept-encoding | 응답 시 원하는 인코딩 방식 |
| accept-language | 응답 시 원하는 언어 |
| connection | HTTP 통신이 완료된 후에 네트워크 접속을 유지할지 결정 
(기본값: keep-alive = 연결을 열린 상태로 유지) |
| host | 서버의 도메인 네임과 서버가 현재 Listening 중인 TCP포트 지정 
(반드시 하나가 존재. 없거나 둘 이상이면 404) |
| referer | 이 페이지 이전에 대한 주소 |
| sec-fetch-dest | 요청 대상 |
| sec-fetch-mode | 요청 모드 |
| sec-fetch-site | 출처(origin)와 요청된 resource 사이의 관계 |
| sec-fetch-user | 사용자가 시작한 요청일 때만 보내짐 (항상 ?1 값 가짐) |
| cache-control | 캐시 설정 |
| upgrade-insecure-requests | HTTP 메시지 전송 시 보안 적용 |
| user-agent | 현재 사용자가 어떤 클라이언트(OS, browser 포함)을 이용해 보낸 요청인지 명시 |</code></pre><ul>
<li>request header 중 sec-fetch 속성값 및 활용 참고<ul>
<li>sec-fetch-site<ul>
<li><code>cross-site</code> : 요청 개시자와 resource를 호스팅하는 서버가 다른 사이트일 경우</li>
<li><code>same-origin</code> : 요청 개시자와 resource를 호스팅하는 서버가 동일한 출처(origin)를 가질 경우</li>
<li><code>same-site</code> : 요청 개시자와 resource를 호스팅하는 서버가 동일한 scheme, 도메인/서브도메인을 가지지만 port가 다른 경우</li>
<li><code>none</code> : 요청이 사용자로부터 시작되었을 경우. (ex. 주소창에 URL 입력, 브라우저 창에 파일 끌어다 놓기 등.)<ul>
<li>sec-fetch-mode</li>
</ul>
</li>
<li><code>cors</code> : CORS protocol 요청</li>
<li><code>navigate</code> : HTML document 사이 이동 시</li>
<li><code>no-cors</code> : no-cors 요청</li>
<li><code>same-origin</code> : 요청 중인 resource와 동일한 출처</li>
<li><code>websocket</code> : websocket 연결을 설정하기 위한 요청<ul>
<li>sec-fetch-dest</li>
</ul>
</li>
<li>서버는 요청이 사용되는 방식이 적절한지에 따라 요청의 허용 여부를 결정<ul>
<li>sec-fetch-user</li>
</ul>
</li>
<li>서버는 해당 속성값을 통해 document, iframe 등의 탐색 요청이 사용자로부터 시작된 것인지 식별 가능</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
<p><a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers">Request Header 참고하기 좋은 사이트</a></p>
</blockquote>
<hr />
<h2 id="httpservletresponse">HttpServletResponse</h2>
<h3 id="요청에-대한-servlet의-역할">요청에 대한 Servlet의 역할</h3>
<ol>
<li>요청을 받는다.<ul>
<li>HTTP method GET/POST 요청에 따라 parameter로 전달받은 데이터를 꺼내어 활용할 수 있다.</li>
</ul>
</li>
<li>비지니스 로직을 처리(DB접속과 CRUD에 대한 로직 처리)한다.<ul>
<li>서비스 메소드를 호출하여 처리 로직을 수행하도록 한다.</li>
</ul>
</li>
<li>응답한다.<ul>
<li>문자열로 동적인 웹(HTML 태그)페이지를 만들고 스트림을 이용해 내보내거나 응답 데이터를 담아 결과 페이지를 호출한다.</li>
</ul>
</li>
</ol>
<hr />
<h3 id="역할-1">역할</h3>
<p>요청에 대한 처리 결과를 작성하기 위해 사용하는 객체이다.</p>
<table>
<thead>
<tr>
<th>method 명</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>setContentType(String)</td>
<td>응답으로 작성하는 페이지의 MIME type을 설정</td>
</tr>
<tr>
<td>setCharacterEncoding(String)</td>
<td>응답하는 데이터의 CharacterSet을 지정</td>
</tr>
<tr>
<td>getWriter()</td>
<td>페이지에 문자 전송을 위한 Stream을 가져옴</td>
</tr>
<tr>
<td>getOutputStream()</td>
<td>페이지에 byte단위의 전송을 위한 Stream을 가져옴</td>
</tr>
<tr>
<td>sendRedirect(String)</td>
<td>client가 매개변수의 페이지를 다시 서버에 요청함</td>
</tr>
</tbody></table>
<hr />
<h2 id="활용-request-header-이전까지">활용 (Request Header 이전까지)</h2>
<p>doGet(), doPost() 메소드를 활용해서 같이 볼 수 있다.</p>
<p><strong>index.jsp</strong></p>
<pre><code class="language-jsp">&lt;%@ page contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;JSP - Hello World&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;Request Parameter&lt;/h1&gt;
&lt;h3&gt;Get 방식의 요청&lt;/h3&gt;
&lt;h4&gt;form 태그를 이용한 get 방식 요청&lt;/h4&gt;
&lt;form action=&quot;querystring&quot; method=&quot;get&quot;&gt;
    &lt;label&gt;이름 : &lt;/label&gt;&lt;input type=&quot;text&quot; name=&quot;name&quot;&gt; &lt;br&gt;
    &lt;label&gt;나이 : &lt;/label&gt;&lt;input type=&quot;number&quot; name=&quot;age&quot;&gt; &lt;br&gt;
    &lt;label&gt;생일 : &lt;/label&gt;&lt;input type=&quot;date&quot; name=&quot;birthday&quot;&gt; &lt;br&gt;
    &lt;label&gt;성별 : &lt;/label&gt;
    &lt;input type=&quot;radio&quot; name=&quot;gender&quot; id=&quot;male&quot; value=&quot;M&quot;&gt;
    &lt;label for=&quot;male&quot;&gt;남자&lt;/label&gt;
    &lt;input type=&quot;radio&quot; name=&quot;gender&quot; id=&quot;female&quot; value=&quot;F&quot;&gt;
    &lt;label for=&quot;female&quot;&gt;여자&lt;/label&gt; &lt;br&gt;
    &lt;label&gt;국적 : &lt;/label&gt;
    &lt;select name=&quot;national&quot;&gt;
        &lt;option value=&quot;ko&quot;&gt;한국&lt;/option&gt;
        &lt;option value=&quot;ch&quot;&gt;중국&lt;/option&gt;
        &lt;option value=&quot;jp&quot;&gt;일본&lt;/option&gt;
        &lt;option value=&quot;etc&quot;&gt;기타&lt;/option&gt;
    &lt;/select&gt;
    &lt;br&gt;
    &lt;label&gt;취미 : &lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;movie&quot; value=&quot;movie&quot;&gt;&lt;label for=&quot;movie&quot;&gt;영화&lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;music&quot; value=&quot;music&quot;&gt;&lt;label for=&quot;music&quot;&gt;음악&lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;sleep&quot; value=&quot;sleep&quot;&gt;&lt;label for=&quot;sleep&quot;&gt;취침&lt;/label&gt;
    &lt;br&gt;

    &lt;input type=&quot;submit&quot; value=&quot;GET 요청&quot;&gt;
    &lt;button type=&quot;button&quot;&gt;GET 요청&lt;/button&gt;
&lt;/form&gt;

&lt;h4&gt;a 태그의 href 속성에 직접 파라미터를 포함한 쿼리 스트링 방식으로 작성해get 요청하기&lt;/h4&gt;
&lt;a href=&quot;querystring?name=홍길동&amp;age=30&amp;birthday=2024-02-06&amp;gender=M&amp;national=ko&amp;hobbies=movie&quot;&gt;
    a 태그를 활용한 쿼리 스트링 값 전달
&lt;/a&gt;

&lt;h4&gt;form 태그를 이용한 post 방식 요청&lt;/h4&gt;
&lt;form action=&quot;formdata&quot; method=&quot;post&quot;&gt;
    &lt;label&gt;이름 : &lt;/label&gt;&lt;input type=&quot;text&quot; name=&quot;name&quot;&gt; &lt;br&gt;
    &lt;label&gt;나이 : &lt;/label&gt;&lt;input type=&quot;number&quot; name=&quot;age&quot;&gt; &lt;br&gt;
    &lt;label&gt;생일 : &lt;/label&gt;&lt;input type=&quot;date&quot; name=&quot;birthday&quot;&gt; &lt;br&gt;
    &lt;label&gt;성별 : &lt;/label&gt;
    &lt;input type=&quot;radio&quot; name=&quot;gender&quot; id=&quot;male&quot; value=&quot;M&quot;&gt;
    &lt;label for=&quot;male&quot;&gt;남자&lt;/label&gt;
    &lt;input type=&quot;radio&quot; name=&quot;gender&quot; id=&quot;female&quot; value=&quot;F&quot;&gt;
    &lt;label for=&quot;female&quot;&gt;여자&lt;/label&gt; &lt;br&gt;
    &lt;label&gt;국적 : &lt;/label&gt;
    &lt;select name=&quot;national&quot;&gt;
        &lt;option value=&quot;ko&quot;&gt;한국&lt;/option&gt;
        &lt;option value=&quot;ch&quot;&gt;중국&lt;/option&gt;
        &lt;option value=&quot;jp&quot;&gt;일본&lt;/option&gt;
        &lt;option value=&quot;etc&quot;&gt;기타&lt;/option&gt;
    &lt;/select&gt;
    &lt;br&gt;
    &lt;label&gt;취미 : &lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;movie&quot; value=&quot;movie&quot;&gt;&lt;label for=&quot;movie&quot;&gt;영화&lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;music&quot; value=&quot;music&quot;&gt;&lt;label for=&quot;music&quot;&gt;음악&lt;/label&gt;
    &lt;input type=&quot;checkbox&quot; name=&quot;hobbies&quot; id=&quot;sleep&quot; value=&quot;sleep&quot;&gt;&lt;label for=&quot;sleep&quot;&gt;취침&lt;/label&gt;
    &lt;br&gt;

    &lt;input type=&quot;submit&quot; value=&quot;POST 요청&quot;&gt;
    &lt;button type=&quot;button&quot;&gt;POST 요청&lt;/button&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5fde253a-4dc3-4884-865b-39824504b45a/image.png" /></p>
<p>사진과 같이 만들어진다. 이제 값들을 넣어보자.</p>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Arrays;

@WebServlet(&quot;/querystring&quot;)
public class QueryStringServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter(&quot;name&quot;);
        System.out.println(&quot;name = &quot; + name);

        int age = Integer.parseInt(req.getParameter(&quot;age&quot;));
        System.out.println(&quot;age = &quot; + age);

        java.sql.Date birthday = java.sql.Date.valueOf(req.getParameter(&quot;birthday&quot;));
        System.out.println(&quot;birthday = &quot; + birthday);

        char gender = req.getParameter(&quot;gender&quot;).charAt(0);
        System.out.println(&quot;gender = &quot; + gender);

        String national = req.getParameter(&quot;national&quot;);
        System.out.println(&quot;national = &quot; + national);

        String[] hobbies = req.getParameterValues(&quot;hobbies&quot;);
        System.out.println(&quot;hobbies = &quot; + Arrays.toString(hobbies));
    }
}</code></pre>
<pre><code class="language-java">import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Enumeration;

@WebServlet(&quot;/formdata&quot;)
public class FormDataServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter(&quot;name&quot;);
        System.out.println(&quot;name = &quot; + name);

        Enumeration&lt;String&gt; names = req.getParameterNames();
        while(names.hasMoreElements()) {
            System.out.println(names.nextElement());
        }
    }
}
</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ee008f37-ba4c-4ac9-a175-980f62adbcaf/image.png" /></p>
<p>a 태그에 쿼리 스트링 방식으로 넣은건 <code>querystring?name=홍길동&amp;age=30&amp;birthday=2024-02-06&amp;gender=M&amp;national=ko&amp;hobbies=movie</code> 이 부분인걸 알 수 있을 것이다.</p>
<hr />
<p><strong>실행결과</strong></p>
<ul>
<li><p>GET 요청 버튼 클릭 시 웹 브라우저 URL
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/836d4c11-1fed-4a52-a2ad-4e9a07b797e9/image.png" /></p>
</li>
<li><p>GET 요청 버튼 클릭 시 인텔리제이 출력
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c03a581e-6d39-4734-928c-a499d1f9c6a8/image.png" /></p>
</li>
<li><p>a 태그 링크 클릭 시 웹 브라우저 URL
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/db099c29-502a-4018-b4d0-e3a90b69b194/image.png" /></p>
</li>
<li><p>a 태그 링크 클릭 시 인텔리제이 출력
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/de61486b-1a4e-4cd8-a1d2-25a07d0785e9/image.png" /></p>
</li>
<li><p>POST 요청 버튼 클릭 시
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3187d02e-d5a6-48cb-b437-5f20dd99a7fd/image.png" /></p>
</li>
</ul>
<p>일단 이정도로 줄이고 다음 편으로 이어서 작성하겠다.</p>