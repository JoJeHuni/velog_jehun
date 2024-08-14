<h1 id="뷰-리졸버를-이용한-뷰-연결하기">뷰 리졸버를 이용한 뷰 연결하기</h1>
<p>Spring MVC 패턴의 요청 처리 과정이다.</p>
<ol>
<li>요청이 들어온다.</li>
<li>우선 필터를 거쳐서 디스페처 서블릿으로</li>
<li>핸들러 매핑에서 매핑 후</li>
<li>controller 가기 전 인터셉터 거친 뒤(preHandle) 도착</li>
<li>비즈니스 로직 처리</li>
<li>다시 컨트롤러에서 디스페처 서블릿으로 갈 때 인터셉터 (postHandle)</li>
<li>뷰 리졸버에서 뷰 찾기</li>
<li>응답</li>
</ol>
<p>이렇게 있을 때 7번의 뷰 리졸버를 이용한 뷰 연결하는 것을 Controller와 연관지어서 알아보자.</p>
<p>컨트롤러에서는 ModelAndView를 반환하는데 그것을 알맞은 View로 전달하기 위해 DispatcherServlet에 의해 뷰 리졸버가 호출된다.</p>
<blockquote>
<p>즉, 뷰 리졸버는 ModelAndView 객체를 View 영역으로 전달하기 위해 알맞은 View 정보를 설정하는 역할을 한다.</p>
</blockquote>
<p>forwarding 과 redirect 를 활용해서 알아볼 것이다.</p>
<hr />
<p>반환한 String이 뷰 리졸버로 간다고 가정하고 하는 것이다.</p>
<h2 id="기본-세팅">기본 세팅</h2>
<p><strong>mainController</strong></p>
<pre><code class="language-java">@Controller
public class MainController {

     * 설명.
     *  뷰 리졸버 인터페이스를 구현한 ThymeleafViewResolver가 현재 처리하게 된다.
     *  접두사(prefix) : resources/templates/
     *  접미사(suffix) : .html
     *  핸들러 메소드가 반환하는 String 값 앞 뒤에 접두사 및 접미사를 붙여 view 를 찾게 된다.
     */

    @RequestMapping(value={&quot;/&quot;, &quot;/main&quot;})
    public String main() {
        return &quot;main&quot;;
    }
}</code></pre>
<hr />
<p><strong>main.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;뷰 리졸버를 이용한 뷰 연결하기 (feat.forwarding VS redirect)&lt;/h1&gt;

    &lt;h3&gt;문자열로 뷰 이름 반환하기&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='string'&quot;&gt;문자열로 뷰 이름 반환&lt;/button&gt;

    &lt;h3&gt;문자열로 redirect&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='string-redirect'&quot;&gt;문자열로 뷰 이름 반환하여 리다이렉트&lt;/button&gt;

    &lt;!-- script 를 써서 javascript 문법 사용 가능--&gt;
    &lt;script&gt;
        console.log('test');
        const message1 = `[[${message1}]]`;
        console.log(message1);
    &lt;/script&gt;

    &lt;h3&gt;문자열로 뷰어를 반환하면서 flashAttribute 추가하기&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='string-redirect-attr'&quot;&gt;문자열로 뷰 이름 반환하여 리다이렉트&lt;/button&gt;
    &lt;script&gt;
        const flashMessage1 = `[[${flashMessage1}]]`;
        console.log(flashMessage1);
    &lt;/script&gt;

    &lt;h3&gt;ModelAndView로 뷰 이름 지정해서 반환하기&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='modelandview'&quot;&gt;ModelAndView 뷰 이름까지 저장해서 반환하기&lt;/button&gt;

    &lt;h3&gt;ModelAndView로 redirect 하기&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='modelandview-redirect'&quot;&gt;ModelAndView로 뷰 이름 반환하여 리다이렉트&lt;/button&gt;

    &lt;script&gt;
        const message2 = `[[${param.message2}]]`;
        console.log(message2);
    &lt;/script&gt;

    &lt;h3&gt;ModelAndView로 redirect 하면서 flashAttribute 추가&lt;/h3&gt;
    &lt;button onclick=&quot;location.href='modelandview-redirect-attr'&quot;&gt;ModelAndView로 뷰 이름 반환하여 리다이렉트 &amp; flash attr 사용하기&lt;/button&gt;
    &lt;script&gt;
        const flashMessage2 = `[[${flashMessage2}]]`;
        // console.log(flashMessage2);
        if (flashMessage2) {
            alert(flashMessage2); // flash 한 번만 쓰는 용도여서 flash가 아니라면 새로고침할 때마다 계속 이 메시지가 뜬다.
        }
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<hr />
<p><strong>result.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot; th:text=&quot;${forwardMessage}&quot;&gt;&lt;/h1&gt;
&lt;!--    &lt;h1 align=&quot;center&quot; th:text=&quot;${message2}&quot;&gt;&lt;/h1&gt;--&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><code>&lt;h1 align=&quot;center&quot; th:text=&quot;${forwardMessage}&quot;&gt;&lt;/h1&gt;</code> 부분은 문자열로 뷰 이름 반환할 때만 사용한다.</p>
<hr />
<p><strong>화면 사진</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/318388b3-d9eb-4e9e-9875-de16d8199d86/image.png" /></p>
<hr />
<h2 id="string을-반환해주는-resolvercontroller"><strong>String을 반환해주는 ResolverController</strong></h2>
<h3 id="1-문자열로-뷰-이름-반환하기">1. 문자열로 뷰 이름 반환하기</h3>
<p>일단은 문자열로 뷰 이름을 반환해주는 첫 번째 예시를 해보자.</p>
<pre><code class="language-java">@Controller
public class ResolverController {
    /* 설명. 서블릿 때와 마찬가지로 포워딩을 통해 값을 전달할 수 있다. */
    @GetMapping(&quot;string&quot;)
    public String stringReturning(Model model) {
        model.addAttribute(&quot;forwardMessage&quot;, &quot;문자열로 뷰 이름 반환&quot;);

        return &quot;result&quot;;
    }
}</code></pre>
<hr />
<p><strong>실행 결과 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0a61c250-f390-4281-99d4-bacf586129cb/image.png" /></p>
<p>forwarding은 잘 되는 것을 볼 수 있다.</p>
<p>그럼 Redirect도 잘 될까??</p>
<hr />
<h3 id="2-문자열로-redirect">2. 문자열로 redirect</h3>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;string-redirect&quot;)
    public String stringRedirect(Model model) {
        model.addAttribute(&quot;message1&quot;, &quot;문자열로 뷰 이름 반환하여 리다이렉트&quot;); // 이건 안 되는 것을 보여주려고 만들었다.
        // 위의 상태로 해서 개발자 도구를 킨 채로 버튼을 눌러도 변화가 없다..

        return &quot;redirect:/&quot;;
    }</code></pre>
<p>서블릿 때와 마찬가지로 리다이렉트를 하지만, 파라미터를 활용하지 않고서는 전달할 수 없다. </p>
<p>(스프링은 해법을 제시하니 그것 또한 알아볼 것이다.)</p>
<hr />
<h3 id="3-문자열로-뷰어를-반환하면서-flashattribute-추가하기">3. 문자열로 뷰어를 반환하면서 flashAttribute 추가하기</h3>
<p>RedirectAttribute 라는 객체에 attribute를 담으면 리다이렉트 시에도 값이 전달된다.</p>
<p>한 번 해보자.</p>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;string-redirect-attr&quot;)
    public String stringRedirectFlashAttribute(RedirectAttributes redirectAttributes) {
        redirectAttributes.addFlashAttribute(&quot;flashMessage1&quot;, &quot;리다이렉트 attr 사용해 redirect&quot;);

        return &quot;redirect:/&quot;;
    }</code></pre>
<hr />
<p>*<em>실행 결과 화면 *</em>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/70260f9e-0604-4f69-9bf9-02fbe35f46e5/image.png" /></p>
<p>개발자 도구에서 확인할 수 있다.</p>
<hr />
<h2 id="modelandview로-반환하기">ModelAndView로 반환하기</h2>
<h3 id="4-modelandview를-이용한-forward">4. ModelAndView를 이용한 forward</h3>
<p>그럼 위에서 말한대로 원래는 Controller는 String이 아닌 ModelAndView 형태로 반환해주니 그렇게 해보려고 한다.</p>
<p>그러면서 이제 result.html은</p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;!--    &lt;h1 align=&quot;center&quot; th:text=&quot;${forwardMessage}&quot;&gt;&lt;/h1&gt;--&gt;
    &lt;h1 align=&quot;center&quot; th:text=&quot;${message2}&quot;&gt;&lt;/h1&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>이렇게 바꿔줘야 한다.</p>
<p>바꿔준 뒤 활용해보자.</p>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;modelandview&quot;)
    public ModelAndView modelAndViewTest(ModelAndView mv) {
        mv.addObject(&quot;message2&quot;, &quot;ModelAndView를 이용한 forward&quot;);
        mv.setViewName(&quot;result&quot;);

        return mv;
    }</code></pre>
<hr />
<p><strong>실행 결과 화면</strong></p>
<p>단순 forward 반환 시에는 Model 만 활용했을 때와 별 차이가 없다. (반환형이 ModelAndView로 바뀐 정도)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/873307ce-352b-4b67-8361-12c658dfa813/image.png" /></p>
<hr />
<h3 id="5-modelandview를-이용한-redirect">5. ModelAndView를 이용한 redirect</h3>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;modelandview-redirect&quot;)
    public ModelAndView modelAndViewRedirectTest(ModelAndView mv) {
        mv.addObject(&quot;message2&quot;, &quot;ModelAndView를 이용한 redirect&quot;);
        mv.setViewName(&quot;redirect:/&quot;); // `redirect:/?message2=ModelAndView를+이용한+forward`

        return mv;
    }</code></pre>
<p>redirect 시에는 ModelAndView에 addObject로 담긴 key와 value 는 parameter로 리다이렉트 된다. (쿼리스트링 형태)</p>
<hr />
<p><strong>실행 결과 화면</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/477d7b93-c695-4814-b85d-7aedcde05ad3/image.png" /></p>
<hr />
<h3 id="6-modelandview로-redirect-하면서-flashattribute-추가">6. ModelAndView로 redirect 하면서 flashAttribute 추가</h3>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;modelandview-redirect-attr&quot;)
    public ModelAndView modelAndViewRedirectFlashAttribute(ModelAndView mv, RedirectAttributes redirectAttributes) {
        redirectAttributes.addFlashAttribute(&quot;flashMessage2&quot;, &quot;ModelAndView를 이용한 redirect attribute&quot;);
        mv.setViewName(&quot;redirect:/&quot;);

        return mv;
    }</code></pre>
<hr />
<p><strong>실행 결과 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fd941d4b-ded8-4907-9128-0ede9020f111/image.png" /></p>
<p>왜 이건 팝업으로 뜰까?</p>
<p>그 이유는 바로 main.html에서 <code>alert</code>를 활용했기 때문이다.</p>
<pre><code>        if (flashMessage2) {
            alert(flashMessage2); 
        }</code></pre><p>flash는 한 번만 쓰는 용도여서 flash가 아니라면 새로고침할 때마다 계속 이 메시지가 뜨는데, 이것을 적용하지 않으면 어떤 이벤트가 발생했을 때 새로고침을 하면 해당 이벤트에 대한 팝업이 계속 뜨는 것이다.</p>
<blockquote>
<p>나도 포인트가 적립됐다든지 이벤트로 직접 구현해보질 않아서 발생했을 때 flashMessage로 하지 않았을 때 포인트가 적립됐다고 하는 만큼 누적되는건지 궁금하긴 하다.. 알아보고 정리하도록 하겠다.</p>
</blockquote>