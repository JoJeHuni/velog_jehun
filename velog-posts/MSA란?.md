<p>개발자라기엔 아직 부족한 내가 다루기에는 먼 기술일지도 모르지만 개념 정도는 알아볼 겸 정리하고자 한다.</p>
<h1 id="msamicroservices-architecture">MSA(Microservices Architecture)</h1>
<p>소프트웨어 개발 방식 중 하나로, 큰 애플리케이션을 <strong>독립적으로 배포</strong>, 운영할 수 있는 작은 서비스들로 나누어 개발하는 접근 방식이다.</p>
<p>각 서비스는 <strong>독립적</strong>이기 때문에, 한 서비스가 업데이트되거나 오류가 발생해도 전체 시스템에 영향을 미치지 않는다.</p>
<ul>
<li><strong>장점</strong><ul>
<li>확장성, 유지보수 용이성, 독립적인 배포 가능성</li>
</ul>
</li>
</ul>
<hr />
<h2 id="구성-요소">구성 요소</h2>
<ul>
<li>Eureka-Client(Service): 실제 서비스별 로직이 실행되는 서비스</li>
<li>Eureka-Server: 서비스들의 정보를 관리하고 라우팅 하는 Eureka Server</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4698ae07-f6ac-4c51-82f8-e7b2a3d86c03/image.png" /></p>
<ul>
<li><code>gateway</code>는 단일 진입점이자 라우팅 기능을 담당</li>
<li>Eureka는 <code>Service Discovery(Registry)</code> 역할을 한다.</li>
<li><code>Load Balancer</code>는 서비스 이름으로 호출하고 health check 역할을 한다.</li>
</ul>
<hr />
<h3 id="api-gateway-서비스">API Gateway 서비스</h3>
<p>사용자나 외부 시스템으로부터 요청을 단일화하여 처리할 수 있도록 해주는 서비스이다.</p>
<p>단일 진입점이 있어야 엔드포인트의 변화에 대해서도 클라이언트 사이드에서 고민할 것이 줄어들며 모든 요청을 일괄적으로 처리된다.</p>
<h4 id="api-gateway-서비스-장점">API Gateway 서비스 장점</h4>
<ol>
<li><strong>인증 및 권한 부여</strong><ul>
<li>인증 및 권한 부여의 단일 작업 =&gt; 사용자의 신원을 확인하고(인증) 해당 사용자가 특정 리소스에 접근할 수 있는 권한이 있는지 확인(권한 부여)하는 작업을 단일 위치에서 처리하며 이는 보안 강화를 위해 중요</li>
</ul>
</li>
</ol>
<blockquote>
<p>인증 에러 : 401 (아이디, 비밀번호를 잘못 입력해서 에러나는 경우 401)
인가 에러 : 403 (권한이 없는데 요청하는 경우에는 403)</p>
</blockquote>
<ol start="2">
<li><strong>서비스 검색 통합</strong><ul>
<li>마이크로 서비스의 검색 통합 =&gt; 마이크로서비스 아키텍처에서 각 서비스의 위치(IP 및 포트 등)를 자동으로 발견하고 연결해 주는 기능으로 이는 서비스 인스턴스가 동적으로 변경될 때 유용</li>
</ul>
</li>
<li><strong>응답 캐싱</strong><ul>
<li>API 게이트웨이 또는 프록시 서버가 자주 요청되는 응답을 캐싱하여 서버 부하를 줄이고 응답 시간을 개선하는 기능</li>
</ul>
</li>
<li><strong>정책, 회로 차단기 및 QoS 다시 시도</strong><ul>
<li>회로 차단기 : 특정 서비스가 오류를 자주 발생시킬 때, 잠시 요청을 차단하여 전체 시스템의 안정성을 유지하는 메커니즘</li>
<li>QoS 다시 시도 : 서비스 요청이 실패할 경우 일정 횟수만큼 자동으로 재시도하는 기능을 제공.</li>
</ul>
</li>
<li><strong>속도 제한</strong><ul>
<li>특정 기간 동안 특정 사용자나 클라이언트가 요청할 수 있는 최대 요청 수를 제한하여 서비스 남용을 방지하는 기능</li>
</ul>
</li>
<li><strong>부하분산</strong><ul>
<li>들어오는 요청을 여러 서비스 인스턴스에 분배하여 시스템의 성능과 안정성을 유지하는 기능</li>
</ul>
</li>
<li><strong>로깅, 추적</strong><ul>
<li>각 요청과 응답에 대한 로그를 기록하고, 여러 서비스 간의 요청 흐름을 추적하여 문제를 디버깅하고 성능을 모니터링하는 데 사용</li>
</ul>
</li>
<li><strong>헤더, 쿼리 문자열 및 청구 변환</strong><ul>
<li>클라이언트로부터 받은 요청의 헤더나 쿼리 문자열을 다른 서비스에서 요구하는 형식으로 변환하거나, 특정 데이터 처리를 수행하는 기능</li>
</ul>
</li>
<li><strong>IP허용 목록에 추가</strong><ul>
<li>특정 IP 주소만이 서비스에 접근할 수 있도록 허용 목록(화이트리스트)에 추가하는 보안 기능으로 이를 통해 허용되지 않은 IP의 접근을 차단 가능</li>
</ul>
</li>
</ol>
<hr />
<h2 id="서버-실행-방법">서버 실행 방법</h2>
<blockquote>
<ol>
<li><code>applcation.yml</code>의 <code>server.port=포트번호</code> 지정해주기</li>
<li>Tomcat쪽 Edit Configuration에 새로운 Configuration 추가하고 VMOption 추가해서 <code>-Dserver.port=포트번호</code></li>
<li>(project 경로의 터미널에서(jar로 필드 후 실행)) <code>java -jar &quot;-Dserver.port=포트번호&quot; .\build\libs\user-service-0.0.1-SNAPSHOT.jar</code></li>
</ol>
</blockquote>
<p>3가지 방법이 있는데, 일단 기본적으로 열어둘 서버 1개와</p>
<p>msa를 실행하는 서버 1개부터 해볼 것이다.</p>
<h3 id="기본적으로-열어둘-서버">기본적으로 열어둘 서버</h3>
<p><strong>build.gradle에 의존성 추가</strong></p>
<pre><code>dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}</code></pre><p><strong>application.yml</strong></p>
<pre><code class="language-yaml">server:
  port: 8761
eureka:
  client:
    # 유레카 서버는 유레카 서버에 저장할 필요 X
    register-with-eureka: false

    # 유레카 서버는 다른 msa가 등록되는걸 확인하기 위해 갱신할 필요 X
    fetch-registry: false</code></pre>
<p><strong>기본 Application에도</strong> <code>@EnableEurekaServer</code> <strong>어노테이션 추가</strong></p>
<pre><code class="language-java">@SpringBootApplication
@EnableEurekaServer
public class JehunApplication {

    public static void main(String[] args) {
        SpringApplication.run(Chap0101EurekaServerLectureSourceApplication.class, args);
    }

}</code></pre>
<p>이렇게 하고 인텔리제이에서는 gradle 쪽에서 build를 찾아볼 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b63e2236-f6d8-4ae9-aee2-29568caa40e1/image.png" /></p>
<p>해당하는 build를 더블클릭하면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/12af122f-cc4a-4ab3-91a0-5b809b60fdeb/image.png" /></p>
<p>build의 libs 속에 .jar 파일이 있는데 명령 프롬프트에서 이것을 실행할 것이다.</p>
<p><code>cd {.jar 파일의 주소}</code> 로 이동한 뒤, <code>java -jar .\</code> + tab키를 누르면 jar 파일이 열린다. 엔터를 눌러 실행을 해두고 끄지 말자</p>
<p><strong>명령 프롬프트에서 실행 시 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6b81e10c-84c7-4ba4-ab38-16ae7e50f089/image.png" /></p>
<blockquote>
<p>끄는 방법은 <strong>ctrl + c</strong> </p>
</blockquote>
<p>실행해둔 채로 다음 예제들을 해볼 것이다.</p>
<hr />
<h3 id="1-서버-1개-열어두기">1. 서버 1개 열어두기</h3>
<p>새로운 프로젝트를 만들자. client 역할을 할 것이다.</p>
<p>이 서버는 client 역할을 위해 build.gradle이 다르다.</p>
<p><strong>build.gradle</strong></p>
<pre><code class="language-java">    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'</code></pre>
<p><strong>application.yml 파일</strong></p>
<pre><code class="language-yaml">server:
  port: 8001

spring:
  application:
    name: eureka-client
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka</code></pre>
<p>이전에 세팅해둔 <a href="http://localhost:8761/eureka">http://localhost:8761/eureka</a> 서버를 defaultZone으로 해두고 8001 포트를 갖는 서버를 열어볼 것이다.</p>
<blockquote>
<p><code>fetch-registry: true</code> -&gt; 약 30초마다 갱신한다는 의미이다.</p>
</blockquote>
<hr />
<p>application에는 <code>@EnableDiscoveryClient</code> 어노테이션이 필요하다.</p>
<p><strong>application</strong></p>
<pre><code class="language-java">@SpringBootApplication
@EnableDiscoveryClient
public class OnlyOneClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(Chap0102EurekaClientLectureSourceApplication.class, args);
    }

}</code></pre>
<p><strong>HelloController</strong></p>
<pre><code class="language-java">@RestController
public class HelloController {

    @GetMapping(&quot;/hello&quot;)
    public String hello() {
        return &quot;Hello!&quot;;
    }
}</code></pre>
<hr />
<p>한 번 실행해보면?</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bc63ec95-ff4c-4ce6-aed5-4290e7d29792/image.png" /></p>
<hr />
<p><strong>링크 클릭 후 hello 입력 시 화면</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8ff7522d-cdfe-4b04-8a56-07a9914db56a/image.png" /></p>
<p>서버를 2개 띄워둔 채로 확인해 간단한 msa를 활용해보았다.</p>
<hr />
<h3 id="2-포트번호가-다른-client-서버-2개-실행하기">2. 포트번호가 다른 client 서버 2개 실행하기</h3>
<p>이번에도 새로운 프로젝트를 만들고 Application과 Controller를 보자.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">@SpringBootApplication
@EnableDiscoveryClient
public class FirstClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(Chap0201FirstServiceLectureSourceApplication.class, args);
    }

}</code></pre>
<p><strong>Controller</strong></p>
<pre><code class="language-java">@RestController
@Slf4j
@RequestMapping(&quot;/first-service&quot;)
public class FirstServiceController {

    private Environment environment;

    @Autowired
    public FirstServiceController(Environment environment) {
        this.environment = environment;
    }


    @GetMapping(&quot;/health&quot;)
    public String healthCheck() {
        /* 설명. Gateway의 로드밸런싱을 통해 RoundRobin 방식으로 실행될 마이크로 서비스의 포트번호 확인 */
        return &quot;OK. 포트는 &quot; + environment.getProperty(&quot;local.server.port&quot;);
    }

    @GetMapping(&quot;message&quot;)
    public String message(@RequestHeader(&quot;first-request&quot;) String header) {
        log.info(&quot;넘어온 헤더 값: {}&quot;, header);
        return &quot;First Service Message&quot;;
    }
}
</code></pre>
<p>이번엔 application.yml 을 설정하자.</p>
<p><strong>application.yml</strong></p>
<pre><code class="language-yaml">server:
  port: 0


spring:
  application:
    name: 1st-service

eureka:
  instance:
    # 랜덤한 인스턴스 아이디 생성 (server.port를 0으로 바꾼 이후)
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka
</code></pre>
<p>1번째 것을 Run 하자.</p>
<p>그리고 2번째 포트번호를 가진 것을 이용하기 위해 인텔리제이에서 Application을 선택할 수 있는데 사진을 보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bac30a07-65e1-45bc-8247-ca5905283fce/image.png" /></p>
<p><strong>Modify Option</strong> -&gt; <strong>Add VM Options</strong> -&gt; <code>-DServer.port=0</code> 입력한 뒤 <strong>Run</strong></p>
<hr />
<p>실행을 한 뒤</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3c7d4515-dc92-461c-bd4b-8ae3cbe233ed/image.png" /></p>
<p>2개의 서버가 뜨는 것을 볼 수 있고, 링크를 눌러보면 랜덤값으로 된 포트번호를 볼 수 있다.</p>
<hr />
<p>내가 눌렀을 때의 화면이다. (매번 바뀌니 같지 않아도 상관없다.)
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/86144d17-9006-4f59-86bf-f0fe49c8ad5d/image.png" /></p>
<p>51071 포트번호를 가지고 <code>localhost:51071/health</code> 를 하면 아래와 같이 나온다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/dae68b50-71ff-4c25-bc08-ed6e9b35e039/image.png" /></p>
<blockquote>
<p>또 다른 링크에서도 다른 포트번호로 뜨는데 그것을 같은 형태로 링크에 넣으면 위와 같은 형태로 보일 것이다.</p>
</blockquote>
<hr />
<p>postman에서도 RequestHeader에 값을 담아서 보내면 그것 또한 출력되는 것을 볼 수 있을 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a9695c09-d40c-49de-8667-7d7795c2d1e6/image.png" /></p>
<hr />
<p><strong>출력된 문장</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c86f5d45-f972-47fd-a643-64f32d8f6321/image.png" /></p>
<hr />
<h2 id="게이트웨이-만들기">게이트웨이 만들기</h2>
<p>원래는 second 까지 first와 별 차이 없게 하나를 더 만들고 진행하려고 했다.</p>
<p>하지만 같은 방식의 코드를 2개를 적는건 너무 길어질 것 같아서 생략하고, 게이트웨이에 대해 다뤄보겠다.</p>
<p>first처럼 생긴 second 프로젝트가 있을 때, <strong>즉 msa에서 인입점이 여러 개가 될 때</strong></p>
<p>위와 같이 2개인 경우도 있지만 많이 늘어나는 경우에는 하나씩 다루기 굉장히 어려운데, 그것을 <strong>게이트웨이가 어느 정도 해결해준다.</strong></p>
<blockquote>
<p>하나의 인입점으로 만들어준다고 생각하면 편하다.</p>
</blockquote>
<p>새로운 프로젝트에</p>
<p><strong>build.gradle</strong></p>
<pre><code>dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
    implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'io.projectreactor:reactor-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}</code></pre><p><strong>application.yml</strong></p>
<pre><code class="language-yaml">server:
  port: 8000

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    server-url:
      defaultZone: http://localhost:8761/eureka
spring:
  application:
    name: gateway-server

  cloud:
    gateway:
      routes:
        - id: 1st-service
          #조건문 충족 시 이동할 uri
          # uri: http://localhost:{포트번호}/ -&gt; 이전까지 사용했던 방식 : 포트번호에는 새로 킬 때마다 확인해서 넣어줘야 한다..
          #위의 과정이 귀찮기 때문에 아래처럼 하면 된다.

          uri: lb://1ST-SERVICE

          # 위의 방식대로 하면 마음대로 스케일 아웃을 할 수 있다. 로드밸런서가 알아서 switching 해주기 때문이다.

          predicates: #게이트 웨이로 요청이 아래와 같은 패턴으로 온다면 (즉, 조건문)
            - Path=/first-service/**

          # first Controller에 있는 RequestMapping을 없애고 싶을 때 사용.
          # 이후 라우팅 될 마이크로 서비스에 /first-service 라는 경로는 제외하고 이후 경로에 대한 요청 진행용 필터이다.
          filters:
            - RewritePath=/first-service/(?&lt;segment&gt;.*), /$\{segment}

            - AddRequestHeader=first-request, first-request-header
            - AddResponseHeader=first-response, first-response-header</code></pre>
<blockquote>
<p>yml에 filters 부분 덕분에 <code>FirstController</code>에 있는 <code>@RequestMapping</code> 어노테이션은 지워도 된다.</p>
</blockquote>
<p>그럼 이제 게이트웨이도 추가가 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b36a8b2c-fbf4-44aa-a66a-64e8c663369a/image.png" /></p>
<p>이제 8000 포트번호에서 first-service에 대해 다룰 수 있게 된다.</p>
<blockquote>
<p>서버를 2개 띄워둔 상태인데 위에 주석에 달린 것처럼 <code>uri: lb://1ST-SERVICE</code> 를 통해 스케일 아웃이 돼 로드 밸런서가 2개의 서버를 자동으로 switching 될 것이다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/23bf1000-e0cd-47a2-8fe8-569cb3c49240/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ef858f3d-29e6-4b57-8f3f-1454b7bbf946/image.png" /></p>
<hr />
<pre><code>- AddRequestHeader=first-request, first-request-header
- AddResponseHeader=first-response, first-response-header</code></pre><p>message도 위와 같이 자동으로 RequestHeader와 ResponseHeader에 추가해주기 때문에 아무것도 헤더에 넣지 않고 해도 잘 될 것이다..</p>
<p>이것으로 간단한 MSA에 다뤄보았고, 추후에 더 실력을 높여서 다루게 된다면 자세하게 정리를 해보겠다.</p>