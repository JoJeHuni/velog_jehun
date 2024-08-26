<h1 id="response-확인하는-3가지-방법">Response 확인하는 3가지 방법</h1>
<ol>
<li>IntelliJ http 파일</li>
<li>PostMan</li>
<li>Swagger</li>
</ol>
<p>3가지 방법이 있다.</p>
<p>간단하게 RestController 파일을 만들어서 확인해보자.</p>
<p><strong>ResponseController</strong></p>
<pre><code class="language-java">@RestController
@RequestMapping(&quot;/response&quot;)
public class ResponseRestController {

    @GetMapping(&quot;/hello&quot;)
    public String helloWorld() {
        return &quot;hello world&quot;;
    }

    @GetMapping(&quot;random&quot;)
    public int getRandomNumber() {
        return (int)(Math.random() * 10) + 1;
    }

    @GetMapping(&quot;message&quot;)
    public Message getMessage() {
        return new Message(200, &quot;메시지를 응답합니다.&quot;);
    }

    @GetMapping(&quot;jsonTest&quot;)
    public List&lt;Map&lt;String, Object&gt;&gt; getJsonTest() {
        List&lt;Map&lt;String, Object&gt;&gt; list = new ArrayList&lt;&gt;();
        Map&lt;String, Object&gt; map = new HashMap&lt;&gt;();
        map.put(&quot;test1&quot;, new Message(200, &quot;성공 1&quot;));
        map.put(&quot;test2&quot;, new Message(200, &quot;성공 2&quot;));

        Map&lt;String, Object&gt; map2 = new HashMap&lt;&gt;();
        map2.put(&quot;nextUrl&quot;, &quot;http://localhost:8080/hello&quot;);

        list.add(map);
        list.add(map2);

        return list;
    }
}</code></pre>
<p><strong>Message 클래스</strong></p>
<pre><code class="language-java">public class Message {
    private int httpStatusCode;
    private String message;

    // 기본 생성자, 매개변수를 가진 생성자, getter, setter, toString()
}</code></pre>
<p>일단 인텔리제이에서 확인하는 방법부터 알아보자.</p>
<h2 id="intellij-http-파일">IntelliJ http 파일</h2>
<p><strong>get.http 파일</strong></p>
<pre><code class="language-http">### GET request hello
GET localhost:8080/response/hello

### GET request random
GET localhost:8080/response/random

### GET request message
GET localhost:8080/response/message

### GET request jsontest
GET localhost:8080/response/jsonTest</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/33e5a650-7c0a-48ef-b375-f35778ea135a/image.png" /></p>
<p>위와 같은 형태로 확인이 가능하다.</p>
<p>random은 1~10까지 잘 출력이 될 것이고, message도 지정해둔 message가 잘 보일 것이다.</p>
<p>마지막 것만 같이 확인해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a5814344-4a8a-4f50-a355-4b5b63c11711/image.png" /></p>
<hr />
<h2 id="postman">PostMan</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9f20aaa9-cbb5-4e91-8ba0-5416dc6c0b17/image.png" /></p>
<p>똑같이 다운로드만 받고 활용하면 된다.</p>
<hr />
<h2 id="swagger">Swagger</h2>
<p>스웨거는 dependency가 일단 build.gradle에 추가돼야 한다.</p>
<pre><code>    // https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-starter-webmvc-ui
    implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.3.0'</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c5f093b6-aab6-42ef-b3a5-94b69579693f/image.png" /></p>
<p>위 사진처럼 적용이 되면 확인 가능한데,</p>
<pre><code>http://localhost:8080/swagger-ui/index.html</code></pre><p>SwaggerConfig에서 설정파일로 지정해둔 뒤 위의 URL로 이동하면 확인 가능할 것이다.</p>
<pre><code class="language-java">@OpenAPIDefinition(
        info = @Info(title = &quot;Lecture API 명세서&quot;,
                description = &quot;수업용 API 명세서&quot;,
                version = &quot;v1&quot;))
@Configuration
public class SwaggerConfig {

}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4a1591ce-60f5-4c4b-884a-981a4b8b48c9/image.png" /></p>
<hr />
<p>이미지 파일도 추가해보자. </p>
<p><strong>ResponseRestController</strong> 에 추가</p>
<pre><code class="language-java">    @GetMapping(value = &quot;image&quot;, produces = MediaType.IMAGE_JPEG_VALUE)
    public byte[] getImage() throws IOException {
        return getClass().getResourceAsStream(&quot;/static/images.jpg&quot;).readAllBytes();
    }</code></pre>
<p><strong>get.http에 추가</strong></p>
<pre><code class="language-http">### GET request image
GET localhost:8080/response/image</code></pre>
<p><strong>출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/71b63183-7abd-44e4-9756-73d4abbc71a3/image.png" /></p>
<hr />
<h1 id="response-entity로-반환해보기">Response Entity로 반환해보기</h1>
<blockquote>
<p><strong>Response Entity</strong> : <code>HttpRequest</code>를 응답하기 위해 <code>httpEntity</code>를 상속받아 <code>HttpStatus</code>, <code>HttpHeaders</code>, <code>HttpBody</code>를 정의하여 사용되는 클래스이다. (필수는 아니지만 관례상 많이 사용)</p>
</blockquote>
<p>User들을 Response Entity로 기본 생성자를 활용해 여러 정보를 임의로 담아 리스트에 담은 채로 반환하려고 한다.</p>
<p><strong>UserDTO</strong></p>
<pre><code class="language-java">public class UserDTO {
    private int userCode;
    private String id;
    private String pwd;
    private String name;
    private java.util.Date enrollAt;

    // 기본 생성자, 매개변수가 있는 생성자, getter, setter. toString()
}</code></pre>
<p>Http 상태코드와 메세지, 결과를 담고 있을 <strong>ResponseMessage</strong></p>
<pre><code class="language-java">public class ResponseMessage {
    private int httpStatusCode;
    private String message;
    private Map&lt;String, Object&gt; result;

    // 기본 생성자, 매개변수가 있는 생성자, getter, setter. toString()
}</code></pre>
<p><strong>ResponseEntityTestController</strong></p>
<pre><code class="language-java">@RestController
@RequestMapping(&quot;/entity&quot;)
public class ResponseEntityTestController {

    private List&lt;UserDTO&gt; users;
    public ResponseEntityTestController() {
        this.users = new ArrayList&lt;&gt;();

        users.add(new UserDTO(1, &quot;user01&quot;, &quot;pass01&quot;, &quot;홍길동&quot;, new java.util.Date()));
        users.add(new UserDTO(2, &quot;user02&quot;, &quot;pass02&quot;, &quot;유관순&quot;, new java.util.Date()));
        users.add(new UserDTO(3, &quot;user03&quot;, &quot;pass03&quot;, &quot;이순신&quot;, new java.util.Date()));
    }

    @GetMapping(&quot;users&quot;)
    public ResponseEntity&lt;ResponseMessage&gt; findAllUsers() {
        HttpHeaders headers = new HttpHeaders();

        headers.setContentType(new MediaType(&quot;application&quot;, &quot;json&quot;, Charset.forName(&quot;UTF-8&quot;)));

        Map&lt;String, Object&gt; responseMap = new HashMap&lt;&gt;();
        responseMap.put(&quot;users&quot;, users);

        ResponseMessage responseMessage = new ResponseMessage(200, &quot;조회성공&quot;, responseMap);

        return new ResponseEntity&lt;&gt;(responseMessage, headers, HttpStatus.OK);
    }
}</code></pre>
<p>이렇게 컨트롤러에서 기본 생성자로 user들을 만든 뒤 Message 타입으로 된 ResponseEntity를 만들고, user들을 담아서 조회가 되는지 확인할 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9814de1f-1a35-4ffd-be6c-c0e7f1b6311b/image.png" /></p>
<p>user.http 파일에 만들어서 GET 요청을 보냈을 때의 response도 확인해보았다.</p>
<p>프로젝트 준비로 인해 시간을 많이 보내야할 것 같아서 교육을 들을 때는 한동안 블로그 글은 작성하기 힘들지만, 그 공백을 Real MySQL8.0을 읽고 작성해 채워보려고 한다.</p>