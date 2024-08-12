<p>Spring의 방대한 데이터를 간단하게 사용할 수 있게끔 해주는 SpringBoot를 인텔리제이에서 사용해보자.</p>
<h1 id="spring-boot">Spring Boot</h1>
<h2 id="spring-boot의-dependencies">Spring Boot의 Dependencies</h2>
<pre><code>Spring Web
Spring Boot Devtools
thymeleaf</code></pre><p>예제 진행을 위해 3개의 라이브러리를 사용해보려고 한다.</p>
<p><strong>Chap01AutoConfigurationLectureSourceApplication</strong></p>
<pre><code class="language-java">package com.jehun.chap01;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Chap01AutoConfigurationLectureSourceApplication {

    public static void main(String[] args) {
        SpringApplication.run(Chap01AutoConfigurationLectureSourceApplication.class, args);
    }
}</code></pre>
<p><strong>applicatoin.properties</strong></p>
<pre><code class="language-properties">test.value=abc
test.username=jehun</code></pre>
<p>스프링에서의 main을 실행하면 @SpringBootApplication의 설정 정보로 인해</p>
<ol>
<li>톰캣이 켜진다 -&gt; 서블릿 컨테이너가 켜진다. 디스패처 서블릿 (Bean X) 또한 톰캣 내부에 뜨게 된다.</li>
<li>그 이후 IoC Container가 켜져 우리가 등록한 Bean 들 또한 컨테이너에 등록한다.</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/063a88f8-c97f-4614-8f52-e63963ef356b/image.png" /></p>
<ul>
<li>지금은 application.properties에 적은 데이터들이 가져와져서 실행했을 때 보이게 된 것</li>
</ul>
<blockquote>
<p>참고 : embedded.tomcat 부분이 내장 톰켓을 의미한다. 현재는 8080 포트로 돼 있다.</p>
</blockquote>
<hr />
<p>하지만 요즘은 properties 파일을 잘 사용하지 않는다..
반복되는 코드가 많아질 수 있어서 가독성이 떨어질 가능성도 있기 때문이다.
그래서 <code>yaml 파일</code>에 대해 알아보자.</p>
<p><strong>application.yml</strong></p>
<pre><code class="language-yml">test:
  value: def
  age: 10</code></pre>
<p>value는 위의 properties 파일과 겹친다. 누가 더 우선순위인지 알아보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/35aa2317-19c3-49c6-8e01-2c36323888bd/image.png" /></p>
<p>우선순위가 properties 에게 밀리긴 한다. 그렇기 때문에 yaml 파일을 사용할 것인데 겹치면 헷갈릴 수도 있기 때문에 하나만 택한다.</p>
<blockquote>
<p><strong>properties 파일 삭제</strong></p>
</blockquote>
<ul>
<li>왜 yaml을 쓰느냐</li>
</ul>
<ol>
<li>한글 작성이 용이하다.</li>
<li>들여쓰기 (가독성에 좋다) 사용, 동일한 코드 반복 줄여줌 (중복 X)</li>
<li>주석을 작성할 수 있고,들여쓰기를 기준으로 주석처리를 할 수 있다.</li>
</ol>
<p>포트 변경하려면 yml 파일에서</p>
<pre><code>server:
    port: 8888</code></pre><p>이런 식으로 변경해주면 바뀌게 된다.</p>
<hr />
<h1 id="spring-request-mapping-테스트">Spring Request Mapping 테스트</h1>
<h2 id="get-요청">GET 요청</h2>
<p><code>/menu/register</code> 경로의 GET 요청을 받아보자.</p>
<p><strong>Chap02RequestMappingLectureSourceApplication</strong></p>
<pre><code class="language-java">package com.jehun.chap02;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Chap02RequestMappingLectureSourceApplication {

    public static void main(String[] args) {
        SpringApplication.run(Chap02RequestMappingLectureSourceApplication.class, args);
    }

}</code></pre>
<p><strong>MethodMappingTestController</strong></p>
<pre><code class="language-java">package com.jehun.chap02;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class MethodMappingTestController {

//    @GetMapping(&quot;/menu/register&quot;)
    @RequestMapping(value = &quot;/menu/register&quot;, method= RequestMethod.GET)
    public String registerMenu() {
        System.out.println(&quot;/menu/register 경로의 GET 요청 받아보기&quot;);

        return &quot;mappingResult&quot;;
    }
}</code></pre>
<p><strong>resources/static/index.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Spring Request Mapping 테스트&lt;/h1&gt;
    &lt;h2&gt;1. 메소드에 요청 매핑하기&lt;/h2&gt;
    &lt;h3&gt;GET: /menu/register&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='/menu/register'&quot;&gt;GET 메뉴 등록 요청&lt;/button&gt;

&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><strong>resources/template/mappingResult.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h3&gt;응답 페이지&lt;/h3&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>브라우저에서 <code>localhost:8080/</code>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/549a78ef-d8a4-45e4-bb1f-80299e0f904e/image.png" /></p>
<p><strong>버튼 누른 뒤</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/aa655093-8d08-4ff7-be9e-a45369f2fdd3/image.png" /></p>
<p>이런 상태에서 이제 model을 추가하면서 다른 방식으로도 할 수 있다.</p>
<p><strong>mappingResult.html</strong> 수정</p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;!--    &lt;h3&gt;응답 페이지&lt;/h3&gt;--&gt;
    &lt;h3 th:text=&quot;${message}&quot;&gt;&lt;/h3&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><strong>MethodMappingTestController</strong> 수정</p>
<pre><code class="language-java">    @RequestMapping(value = &quot;/menu/register&quot;, method= RequestMethod.GET)
    public String registerMenu(Model model) {
        System.out.println(&quot;/menu/register 경로의 GET 요청 받아보기&quot;);

        model.addAttribute(&quot;message&quot;, &quot;신규 메뉴 등록용 핸들러 메소드 호출하고 응답한 페이지&quot;);

        return &quot;mappingResult&quot;;
    }</code></pre>
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e1e6c6e8-ab78-4ca3-8c7e-2b56486965f6/image.png" /></p>
<hr />
<h2 id="post-요청">POST 요청</h2>
<p><strong>컨트롤러에 추가</strong></p>
<pre><code class="language-java">//    @PostMapping(&quot;/menu/modify&quot;)
    @RequestMapping(&quot;/menu/modify&quot;)
    public String modifyMenu(Model model) {
        model.addAttribute(&quot;message&quot;, &quot;POST 방식의 메뉴 수정용 핸들러 메소드 호출함...&quot;);

        return &quot;mappingResult&quot;;
    }</code></pre>
<p><strong>index.html 에도 button 아래에 추가</strong></p>
<pre><code class="language-html">&lt;h3&gt;POST: /menu/modify&lt;/h3&gt;
&lt;form action=&quot;/menu/modify&quot; method=&quot;post&quot;&gt;
    &lt;button type=&quot;submit&quot;&gt;POST 메뉴 수정 요청&lt;/button&gt;
&lt;/form&gt;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f94bf0e0-4950-40db-9b38-2c910ab3ca5d/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3045605d-7fb9-4aa4-8b18-3d2ecab0aa92/image.png" /></p>
<hr />
<h3 id="한-번-더-해보기">한 번 더 해보기</h3>
<p>컨트롤러에 추가</p>
<pre><code class="language-java">    @GetMapping(&quot;/menu/delete&quot;)
    public String getDeleteMenu(Model model) {
        model.addAttribute(&quot;message&quot;, &quot;GET 방식의 메뉴 삭제용 핸들러 메소드 호출함&quot;);

        return &quot;mappingResult&quot;;
    }

    @PostMapping(&quot;/menu/delete&quot;)
    public String postDeleteMenu(Model model) {
        model.addAttribute(&quot;message&quot;, &quot;POST 방식의 메뉴 삭제용 핸들러 메소드 호출함&quot;);

        return &quot;mappingResult&quot;;
    }</code></pre>
<p>index.html에 추가</p>
<pre><code class="language-html">&lt;h3&gt;GET: /menu/delete&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/menu/delete'&quot;&gt;GET 메뉴 삭제 요청&lt;/button&gt;

&lt;h3&gt;POST: /menu/delete&lt;/h3&gt;
&lt;form action=&quot;/menu/delete&quot; method=&quot;post&quot;&gt;
    &lt;button&gt;POST 메뉴 삭제 요청&lt;/button&gt;
&lt;/form&gt;</code></pre>
<p>실행해보면 둘다 잘 될 것이다.</p>