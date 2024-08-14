<h1 id="exception-handler">Exception Handler</h1>
<p>일반적인 요청 흐름</p>
<pre><code>WAS(톰캣) -&gt; 필터 -&gt; 서블릿(디스패처 서블릿) -&gt; 인터셉터 -&gt; 컨트롤러</code></pre><p>이 때 컨트롤러에서 에러가 발생했는데도 별도의 예외 처리를 하지 않으면 WAS까지 에러가 전달된다. </p>
<p>그러면 WAS는 애플리케이션에서 처리를 못하는 예와라 exception이 올라왔다고 판단을 하고, 대응 작업을 진행한다.</p>
<pre><code>컨트롤러(예외발생) -&gt; 인터셉터 -&gt; 서블릿(디스패처 서블릿) -&gt; 필터 -&gt; WAS(톰캣)</code></pre><p>WAS는 스프링 부트가 등록한 에러 설정(/error)에 맞게 요청을 전달하게 된다.</p>
<pre><code> WAS(톰캣) -&gt; 필터 -&gt; 서블릿(디스패처 서블릿) -&gt; 인터셉터 -&gt; 컨트롤러(BasicErrorController)</code></pre><blockquote>
<p>기본적인 에러 처리 방식은 결국 에러 컨트롤러를 한번 더 호출하는 것</p>
</blockquote>
<hr />
<p>예외처리하는 방식은 굉~장히 많지만 <code>ExceptionResolver</code> 와 <code>@ExceptionHandler</code> 어노테이션을 활용해서 에러 처리를 하는 방식에 대해서 알아보자.</p>
<h2 id="기본-세팅">기본 세팅</h2>
<p><strong>main.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;Exception Handler 사용하기&lt;/h1&gt;
    &lt;h3&gt;SimpleMappingExceptionResolver를 이용한 방식&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='simple-null'&quot;&gt;NullPointerException 테스트&lt;/button&gt;
    &lt;button onclick=&quot;location.href='simple-user'&quot;&gt;사용자 정의 Exception 테스트&lt;/button&gt;

    &lt;h3&gt;@ExceptionHandler 어노테이션을 이용한 방식&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='annotation-null'&quot;&gt;NullPointerException 테스트&lt;/button&gt;
    &lt;button onclick=&quot;location.href='annotation-user'&quot;&gt;사용자 정의 Exception 테스트&lt;/button&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<hr />
<p>나머지 html도 body 부 속에서만 작성했기 때문에 body부만 작성하고 생략하겠다.</p>
<p><strong>nullPointer.html</strong></p>
<pre><code class="language-html">&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;NullPointerException 발생함&lt;/h1&gt;
&lt;/body&gt;</code></pre>
<hr />
<p>예외 발생 시 예외 메시지를 출력해보기 위해서 작성한 html 파일이다.</p>
<p><strong>memberRegister.html</strong></p>
<pre><code class="language-html">&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;더 이상 가입이 불가합니다.&lt;/h1&gt;
    &lt;h1 align=&quot;center&quot; th:text=&quot;${exceptionMessage.message}&quot;&gt;&lt;/h1&gt;
&lt;/body&gt;</code></pre>
<blockquote>
<p>핸들링 돼 넘어온 페이지에서는 <code>setExceptionAttribute</code>로 작성한 키 값으로 예외 타입과 메시지를 추출할 수 있다.</p>
</blockquote>
<hr />
<p>예외에 따른 설정한 페이지말고는 default 페이지로 적용할 것이다.</p>
<p><strong>default.html</strong></p>
<pre><code class="language-html">&lt;body&gt;
&lt;!--    &lt;h1 align=&quot;center&quot; th:text=&quot;${exceptionMessage}&quot;&gt;&lt;/h1&gt;--&gt;
    &lt;h1 align=&quot;center&quot; th:text=&quot;${userException.message}&quot;&gt;&lt;/h1&gt;

&lt;/body&gt;</code></pre>
<p>기존에는 exceptionMessage를 사용했으나, MemberRegisterException과 관련해서 묶기 위해서 한 줄 더 추가했으니 더 보자.</p>
<hr />
<p>이제 컨트롤러 클래스다.</p>
<p><strong>MainController</strong></p>
<pre><code class="language-java">@Controller
public class MainController {

    @RequestMapping(&quot;/main&quot;)
    public void main() {}

    @RequestMapping(&quot;/&quot;)
    public String mainRoot() {
        return &quot;main&quot;;
    }
}</code></pre>
<p>이건 스프링을 실행하고 <code>localhost:8080</code> 이나 <code>localhost:8080/main</code> 이나 같은 기존 main.html 화면이 뜨게 된다.</p>
<hr />
<p>이제 설정 파일로 지정한 RootConfiguration 클래스다.</p>
<p><strong>RootConfiguration</strong></p>
<pre><code class="language-java">@Configuration
public class RootConfiguration {
    @Bean
    public SimpleMappingExceptionResolver simpleMappingExceptionResolver() {
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();

        Properties properties = new Properties();
        properties.setProperty(&quot;java.lang.NullPointerException&quot;, &quot;error/nullPointer&quot;);
        properties.setProperty(&quot;MemberRegisterException&quot;, &quot;error/memberRegister&quot;);

        /* 설명. 1. 예외에 따른 페이지 설정한 것 적용 */
        exceptionResolver.setExceptionMappings(properties);

        /* 설명. 2. 그 외 나머지 예외에 대한 default 페이지 적용 */
        exceptionResolver.setDefaultErrorView(&quot;error/default&quot;);

        /* 설명. 3. 예외 메시지를 뽑기 위한 Attribute key 값 ( 화면에서 예외 메시지 확인 시 사용할 키 값) */
        exceptionResolver.setExceptionAttribute(&quot;exceptionMessage&quot;);

         return exceptionResolver;
    }
}</code></pre>
<p>예외에 따른 페이지 -&gt; NullPointerException이 뜰 때, MemberRegisterException이 뜰 때
2가지 경우에 적용된다.</p>
<p>나머지는 default...</p>
<hr />
<p>그럼 간단하게 <code>MemberRegisterException</code>은 어떻게 이루어져 있을까?</p>
<p><strong>MemberRegisterException</strong></p>
<pre><code class="language-java">public class MemberRegisterException extends Exception {
    public MemberRegisterException(String message) {
        super(message);
    }
}</code></pre>
<blockquote>
<p>Exception 을 상속하고 있는 것을 기억하고 있자.</p>
</blockquote>
<hr />
<p>다음은 이제 위에서 나온 ExceptionHandlerController이다.</p>
<p>차례대로 알아보자.</p>
<h2 id="1-simplemappingexceptionresolver">1. SimpleMappingExceptionResolver</h2>
<h3 id="nullpointerexception">NullPointerException</h3>
<p><strong>ExceptionHandlerController</strong></p>
<pre><code class="language-java">@Controller
public class ExceptionHandlerController {
    @GetMapping(&quot;simple-null&quot;)
    public String simpleNullPointerExceptionTest() {
        if (true) throw new NullPointerException(); // if 문이 없으면 return 에서 에러 발생이라고 나온다.
        return &quot;/&quot;;
    }
}</code></pre>
<ul>
<li><p>핸들러 메소드가 <code>MainController</code> 의 <code>mainRoot()</code>의 핸들러 메소드로 <code>forward</code> 한 것처럼 만든 것</p>
<ul>
<li>하지만 예외를 발생시키기 때문에 도달하지는 않는다.</li>
</ul>
</li>
<li><p>예외 발생으로 인해 <code>nullPointer.html</code>로 <code>forward</code> 된다.</p>
</li>
</ul>
<hr />
<p><strong>실행 결과 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0a78c7a6-f180-4ffe-b759-e039f7153c58/image.png" /></p>
<hr />
<h3 id="memberregisterexception">MemberRegisterException</h3>
<p><strong>컨트롤러에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;simple-user&quot;)
    public String simpleUserExceptionTest() throws MemberRegisterException {
        if (true) throw new MemberRegisterException(&quot;자리가 가득 찼다.&quot;);

        return &quot;/&quot;;
    }</code></pre>
<p>Exception을 상속했기 때문에 Checked Exception이고 그래서 <code>throws MemberRegisterException</code>가 붙게 된다.</p>
<hr />
<p><strong>실행 결과 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b48c4e26-868d-4ee9-88bc-138dd93f032f/image.png" /></p>
<hr />
<h2 id="exceptionhandler-어노테이션을-이용한-방식">@ExceptionHandler 어노테이션을 이용한 방식</h2>
<h3 id="nullpointerexception-1">NullPointerException</h3>
<p>컨트롤러 안에서 예외 발생 시에는 어떻게 할지에 대해 RootConfiguration 이라고 설정 파일이 있긴 하지만 그것보다 우선적으로 적용할 수 있다.</p>
<p>죽, 이후 핸들러 메소드들은 이 클래스만 해당되는 지역범위 예외처리 설정을 한다.</p>
<p>그것도 한 번 같이 보려고 한다.</p>
<p><strong>컨트롤러에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;annotation-null&quot;)
    public String annotationNullPoinerExceptionTest() {
        String str = null;
        System.out.println(&quot;str.charAt(0) :&quot; + str.charAt(0));

        return &quot;/&quot;;
    }

    @ExceptionHandler(NullPointerException.class)
    public String nullPointerExceptionHandler() {
        System.out.println(&quot;이 Controller에서 NullPointerException 발생 시 이 메소드로 오는지 확인&quot;);

        return &quot;error/nullPointer&quot;;
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6e95a1a3-099c-42fe-abad-0f0db43efb59/image.png" /></p>
<p><code>str = null</code> 이기 때문에 <code>charAt(0)</code>은 <code>NullPointerException</code> 이 터지게 되고, <code>RootConfiguration</code>이 아닌 안의 메소드에서 호출을 하게 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bc6f8e4a-9a44-490d-b24f-0f2033e85d03/image.png" /></p>
<hr />
<h3 id="memberregisterexception-1">MemberRegisterException</h3>
<p>이것도 바로 위에 한 <code>NullPointerException</code>의 예제와 같다.</p>
<p><strong>컨트롤러에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;annotation-user&quot;)
    public String annotationUserExceptionTest() throws MemberRegisterException {
        if(true) throw new MemberRegisterException(&quot;더 이상 받을 수 없다고 다시 알림.&quot;);

        return &quot;/&quot;;
    }

    @ExceptionHandler(MemberRegisterException.class)
    public String UserExceptionHandler(Model model, MemberRegisterException exception) {
        model.addAttribute(&quot;userException&quot;, exception);

        return &quot;error/default&quot;;
    }</code></pre>
<p><strong>캡처 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d6941cfb-ead9-4a26-b0ee-440b59ad77ff/image.png" /></p>
<p>exception이 터지면 똑같이 다른 핸들러가 먼저 호출되게 된다.</p>
<p>간단하게 Exception Handler에 대해서도 정리해봤다.</p>