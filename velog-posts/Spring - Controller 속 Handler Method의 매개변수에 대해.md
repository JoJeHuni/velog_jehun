<p>Controller와 연관지어서 핸들러 메소드의 매개변수에 대해 살펴보자.</p>
<h1 id="controller">@Controller</h1>
<ul>
<li><code>@Controller</code> : @Component의 하위 개념 중 하나로 실행할 때 스프링 컨테이너에 클래스가 등록돼 Controller의 역할을 한다는 것을 스프링 컨테이너에게 인식시켜줄 수 있다.</li>
</ul>
<blockquote>
<p>내부 메소드들은 <code>@Bean</code> 등록을 하지 않아도 된다.
=&gt; Dispatcher Servlet이 핸들러 매핑과 연결되면서 요청에 맞는 Controller를 찾아주기 때문에 내부 메소드를 활용할 수 있다.</p>
</blockquote>
<p>기본 세팅
<strong>index.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;핸들러 메소드의 파라미터와 어노테이션&lt;/h1&gt;
&lt;h3&gt;1. HttpServletRequest를 요청 파라미터로 전달받기&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/first/regist'&quot;&gt;파라미터 전달하기&lt;/button&gt;

&lt;h3&gt;2. @RequestParam을 이용하여 요청 파라미터 전달받기&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/first/modify'&quot;&gt;@RequestParam 이용하기&lt;/button&gt;

&lt;h3&gt;3. @ModelAttribute를 이용하여 파라미터 전달받기&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/first/search'&quot;&gt;@ModelAttribute 이용하기&lt;/button&gt;

&lt;h3&gt;4. HttpSession 이용하기&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/first/login'&quot;&gt;session 정보 담기&lt;/button&gt;

&lt;h3&gt;5. @RequestBody 등의 핸들러 메소드의 어노테이션을 활용한 전달 받기&lt;/h3&gt;
&lt;button onclick=&quot;location.href='/first/body'&quot;&gt;@RequestBody 등의 어노테이션 이용하기&lt;/button&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><strong>messageprinter.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h3 th:text=&quot;${message}&quot;&gt;&lt;/h3&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<hr />
<h1 id="핸들러-메소드의-매개변수">핸들러 메소드의 매개변수</h1>
<h2 id="requestmapping">@RequestMapping()</h2>
<ul>
<li>클래스에 달린 <code>@RequestMapping()</code><ul>
<li>사용 예시 ex) <code>@RequestMapping(&quot;/first&quot;)</code></li>
</ul>
</li>
</ul>
<p><code>@RequestMapping()</code> 를 클래스에 적으면 핸들러 메소드들이 모두 <code>/first</code> 경로 아래의 요청들을 받게 될 것이다.</p>
<p>나머지는 메소드에 적을 것임</p>
<p><strong>FirstController</strong></p>
<pre><code class="language-java">@Controller
@RequestMapping(&quot;/first&quot;)
public class FirstController {

//    @GetMapping(&quot;/register&quot;)
//    public String register() {
//        return &quot;/first/register&quot;;
//    }

    /* 반환형이 void인 핸들러 메소드는 요청 경로 자체가 view의 경로 및 이름을 반환한 것으로 바로 해석된다. */
    @GetMapping(&quot;/register&quot;)
    public void register() {}

    @PostMapping(&quot;/register&quot;)
    public String registerMenu(HttpServletRequest request, Model model) {
        String name = request.getParameter(&quot;name&quot;);
        int price = Integer.parseInt(request.getParameter(&quot;price&quot;));
        int categoryCode = Integer.parseInt(request.getParameter(&quot;categoryCode&quot;));

        String message = name + &quot;을(를) 신규 메뉴 목록의 &quot; + categoryCode + &quot;번 카테고리에 &quot; + price + &quot;원으로 등록하였습니다.&quot;;

        model.addAttribute(&quot;message&quot;, message);

        return &quot;first/messageprinter&quot;;
    }
}</code></pre>
<p>이러한 코드가 있을 때 메소드에 있는 <code>Mapping</code>과 관련된 어노테이션 속 url은 전부 클래스의 <code>/first</code> 아래의 해당하는 것들이다.</p>
<blockquote>
<p>즉, @PostMapping(&quot;/first/register&quot;) 가 본래의 모습인데 저렇게 나타낼 수 있다.</p>
<p>참고 - @PostMapping(&quot;/register&quot;) 와 @PostMapping(&quot;register&quot;) 둘다 가능하다. <strong>슬래시가 없어도 된다.</strong></p>
</blockquote>
<p><strong>register.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;파라미터로 값 전송하기&lt;/h1&gt;
&lt;h3&gt;신규 메뉴 등록하기&lt;/h3&gt;
&lt;form action=&quot;register&quot; method=&quot;post&quot;&gt;
    등록할 메뉴의 이름: &lt;input type=&quot;text&quot; name=&quot;name&quot;&gt;&lt;br&gt;
    등록할 메뉴의 가격: &lt;input type=&quot;number&quot; name=&quot;price&quot;&gt;&lt;br&gt;
    등록할 메뉴의 카테고리:
    &lt;select name=&quot;categoryCode&quot;&gt;
        &lt;option value=&quot;1&quot;&gt;식사&lt;/option&gt;
        &lt;option value=&quot;2&quot;&gt;음료&lt;/option&gt;
        &lt;option value=&quot;3&quot;&gt;디저트&lt;/option&gt;
    &lt;/select&gt;&lt;br&gt;
    &lt;button type=&quot;submit&quot;&gt;등록하기&lt;/button&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><strong>index 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e31498ce-8db8-4398-a7a6-3748cb830478/image.png" /></p>
<hr />
<p><strong>register 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fda16959-8527-41ab-bdd8-33c2d3b201fe/image.png" /></p>
<hr />
<p><strong>결과 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/004d9460-0c27-4a61-9de0-b3405a645286/image.png" /></p>
<hr />
<h2 id="requestparam">@RequestParam</h2>
<p>Request의 parameter를 나타낼 때 사용하는 어노테이션으로 컨트롤러에서는 아래와 같이 나타낼 수 있다.</p>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;/modify&quot;)
    public void modify() {}

    @PostMapping(&quot;/modify&quot;)
    public String modifyMenu(Model model, @RequestParam String name, @RequestParam int modifyPrice) { // @RequestParam은 생략 가능하다.
        String message = name + &quot;메뉴의 가격을 &quot; + modifyPrice + &quot;로 변경하였습니다.&quot;;
        model.addAttribute(&quot;message&quot;, message);

        return &quot;first/messagePrinter&quot;;
    }

    @PostMapping(&quot;/modify2&quot;)
    public String modifyMenu2(Model model, @RequestParam Map&lt;String, String&gt; map) {
        String name = map.get(&quot;name2&quot;);
        int modifyPrice = Integer.parseInt(map.get(&quot;modifyPrice2&quot;));

        String message = name + &quot;메뉴의 가격을 &quot; + modifyPrice + &quot;로 변경하였습니다.&quot;;
        model.addAttribute(&quot;message&quot;, message);

        return &quot;first/messagePrinter&quot;;
    }</code></pre>
<ul>
<li><code>@RequestParam</code>의 속성들<ol>
<li><code>defaultValue</code> : 사용자의 입력값이 없거나 (&quot;&quot;) 아니면 request의 parameter 키 값과 일치하지 않는 매개변수일 때 사용하고 싶은 값을 default값으로 설정 가능</li>
<li><code>name</code> : request parameter의 <strong>키 값과 다른 매개변수 명을 사용하고 싶을 때 request parameter의 키 값을 작성</strong>한다.</li>
</ol>
</li>
</ul>
<blockquote>
<p><code>@RequestParam</code> 어노테이션은 생략이 가능하다.</p>
</blockquote>
<p>html 부분은 생략하겠다.</p>
<p><strong>index 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b7054b36-b6e6-431e-b287-9e332ae5c90f/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/90261b39-1a59-4e78-97f5-6ea7e9eb5a58/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/13f04f39-a768-4ceb-a68c-a7e400af32ec/image.png" /></p>
<p>2번째는 2000으로 변경되게끔 똑같은 화면을 나타낸다.</p>
<hr />
<h2 id="modelattribute">@ModelAttribute</h2>
<p><code>@ModelAttribute</code>는 HTTP Body 내용과 HTTP 파라미터의 값들을 Getter, Setter, 생성자를 통해 주입하기 위해 사용한다.</p>
<p>일반 변수의 경우 전달이 불가능하기 때문에 model 객체를 통해서 전달해야 한다.</p>
<p>이것을 사용하기 위해서 MenuDTO 클래스를 만들고</p>
<pre><code class="language-java">    private String name;
    private int price;
    private int categoryCode;
    private String orderableStatus;</code></pre>
<p>위 4가지 필드에 대해 기본 생성자, 모든 필드를 매개변수로 갖는 생성자, Getter, Setter, ToString 전부 적어둔 채로 시작했다.</p>
<p>사용 예시 : <code>@ModelAttribute(&quot;파라미터명&quot;)</code></p>
<p>하지만 이번 코드에서는 파라미터명에 적어야하는 부분과 menuDTO 부분이 겹치기때문에 따로 적지 않았다.</p>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;/search&quot;)
    public void search() {}

    @PostMapping(&quot;/search&quot;)
    public String searchMenu(@ModelAttribute MenuDTO menuDTO) { //객체를 Model에 담아서 return 하겠다는 뜻이다.
        System.out.println(&quot;menuDTO = &quot; + menuDTO);
        return &quot;/first/searchResult&quot;;
    }</code></pre>
<p>위와 같이 키 값을 안 넣어주는 경우와 <code>@ModelAttribute(&quot;menu&quot;)</code> 이렇게 적는 경우
<strong>searchResult.html</strong>은 아래처럼 작성해줘야 한다.</p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1 align=&quot;center&quot;&gt;Model에 담긴 커맨드 객체의 정보 출력&lt;/h1&gt;

&lt;!-- 설명. 1. @ModelAttribute에 키 값이 없을 경우 커맨드 객체의 타입으로 접근해서 활용(첫 글자 소문자) --&gt;
&lt;!--    &lt;h3 th:text=&quot;|메뉴의 이름: ${menuDTO.name}|&quot;&gt;&lt;/h3&gt;--&gt;
&lt;!--    &lt;h3 th:text=&quot;|메뉴의 가격: ${menuDTO.price}|&quot;&gt;&lt;/h3&gt;--&gt;
&lt;!--    &lt;h3 th:text=&quot;|메뉴의 카테고리: ${menuDTO.categoryCode}|&quot;&gt;&lt;/h3&gt;--&gt;
&lt;!--    &lt;h3 th:text=&quot;|메뉴의 판매상태: ${menuDTO.orderableStatus}|&quot;&gt;&lt;/h3&gt;--&gt;

&lt;!-- 설명. 2. @ModelAttribute에 키 값이 있을 경우 키 값으로 접근해서 활용 --&gt;
&lt;h3 th:text=&quot;|메뉴의 이름: ${menu.name}|&quot;&gt;&lt;/h3&gt;
&lt;h3 th:text=&quot;|메뉴의 가격: ${menu.price}|&quot;&gt;&lt;/h3&gt;
&lt;h3 th:text=&quot;|메뉴의 카테고리: ${menu.categoryCode}|&quot;&gt;&lt;/h3&gt;
&lt;h3 th:text=&quot;|메뉴의 판매상태: ${menu.orderableStatus}|&quot;&gt;&lt;/h3&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>핸들러 메소드의 매개변수에 우리가 작성한 클래스를 스프링이 객체로 만들어 주고 내부적으로 setter를 활용해 값도 주입해준다. (command 객체)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a65c6ef0-d962-44c3-bf70-c119e4e8d622/image.png" /></p>
<p>search.html, searchResult.html 파일로 프론트를 간단하게 나타내고 값을 넣었더니 잘 나왔다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cb087245-e0ed-471c-a143-b57c6308be68/image.png" /></p>
<hr />
<h2 id="httpsession">HttpSession</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e704e688-5e15-4db0-9141-1ce96df4a91d/image.png" /></p>
<p>이렇게 html로 login 하는 것에 대해 HttpSession을 사용해보고자 한다.</p>
<h3 id="httpsession을-매개변수로-선언하기">HttpSession을 매개변수로 선언하기</h3>
<p><strong>Controller에 추가</strong></p>
<pre><code class="language-java">    @GetMapping(&quot;login&quot;)
    public void login(){}

    @PostMapping(&quot;login&quot;)
    public String sessionTest1(HttpSession session, @RequestParam String id) {
        session.setAttribute(&quot;id&quot;, id);
        return &quot;first/loginResult&quot;;
    }

    @GetMapping(&quot;logout1&quot;)
    public String logoutTest1(HttpSession session) {
        session.invalidate();

        return &quot;first/loginResult&quot;;
    }</code></pre>
<p>sessionTest1, logout1은 매개변수로 선언해서 사용한 것이다.</p>
<p>이 때는 로그인도 잘 되고, <code>invalidate()</code>를 통한 로그아웃도 되는 것을 볼 수 있다.</p>
<p>하지만 <code>@SessionAttributes</code> 을 사용하는 방식도 있다.</p>
<hr />
<h3 id="sessionattribute-사용하기">@SessionAttribute 사용하기</h3>
<p><strong>Controller 클래스에 어노테이션 추가</strong></p>
<pre><code class="language-java">@Controller
@RequestMapping(&quot;/first&quot;)
@SessionAttributes(&quot;id&quot;)
public class FirstController {

    (중략...)

    @PostMapping(&quot;login2&quot;)
    public String sessionTest2(Model model, @RequestParam String id) {
        model.addAttribute(&quot;id&quot;, id);

        return &quot;first/loginResult&quot;;
    }

    @GetMapping(&quot;logout2&quot;)
    public String logoutTest2(SessionStatus sessionStatus) {
        sessionStatus.setComplete();

        return &quot;first/loginResult&quot;;
    }
}</code></pre>
<p>이전에 invalidate() 방식으로 로그아웃을 하려고 하면 안 된다. 그 이유는</p>
<p><code>@SessionAttributes</code> 는 메소드 레벨이 아닌 클래스 레벨에 작성하여 Model로 값을 보낼때 그 name 값과 동일하면 세션으로 인식하여 저장하기 때문인데 그래서 &quot;id&quot;의 값을 지정해준 것이다.</p>
<p>이 때 로그아웃을 하기 위해서는 코드에 적힌 대로 <code>SessionStatus</code>를 매개변수에 넣고 <code>setComplete()</code> 를 사용해야 한다.</p>
<p>대신 이제 id 값을 <code>@SessionAttributes</code>로 지정해준 뒤로는 <code>logout1</code>이 작동하지 않는 것을 알 수 있을 것이다.</p>
<p>로그인 했을 때
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bcdf6fe1-b54b-4193-ba48-c83be1aca561/image.png" /></p>
<p>로그아웃2을 눌렀을 때
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cc7d52dc-15b0-440e-982a-6c797dff542a/image.png" /></p>
<hr />
<h2 id="requestbody-등의-핸들러-메소드의-어노테이션">@RequestBody 등의 핸들러 메소드의 어노테이션</h2>
<p>아래 사진은 body.html의 사진이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/47a82868-a469-40ed-b7f2-c465aed9ae9a/image.png" /></p>
<p>Controller에 추가</p>
<pre><code class="language-java">    @GetMapping(&quot;body&quot;)
    public void body() {}

    @PostMapping(&quot;body&quot;)
    public void body(@RequestBody String body, @RequestHeader(&quot;content-type&quot;) String contentType, @CookieValue(value=&quot;JSESSIONID&quot;) String sessionId) {
        System.out.println(&quot;body = &quot; + body);
        System.out.println(&quot;contentType = &quot; + contentType);
        System.out.println(&quot;sessionId = &quot; + sessionId);
    }</code></pre>
<ul>
<li><code>@RequestBody()</code></li>
<li><code>@RequestHeader()</code></li>
<li><code>@CookieValue()</code></li>
</ul>
<p>값을 넣으면 아래처럼 출력된다</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a9813581-e786-4753-a536-557f5704906c/image.png" /></p>