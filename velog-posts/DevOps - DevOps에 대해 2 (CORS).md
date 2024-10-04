<p>이번엔 CORS 문제를 해결하는 방식에 대해 알아보려고 한다.</p>
<p>우선 백엔드 프로젝트부터 만들자.</p>
<p><code>controller.CalculatorController</code></p>
<pre><code class="language-java">@RestController
@Slf4j
public class CalculatorController {

    private final CalculatorService calculatorService;

    @Autowired
    public CalculatorController(CalculatorService calculatorService) {
        this.calculatorService = calculatorService;
    }

    @GetMapping(&quot;/health&quot;)
    public String healthCheck() {
        return &quot;I'm alive&quot;;
    }

    @GetMapping(&quot;/plus&quot;)
    public ResponseEntity&lt;CalculatorDTO&gt; plusTwoNumbers(CalculatorDTO calculatorDTO) {
        log.info(&quot;plus 핸들러 실행여부 확인&quot; + calculatorDTO);

        int result = calculatorService.plusTwoNumbers(calculatorDTO);

        calculatorDTO.setSum(result);

        return ResponseEntity.ok().body(calculatorDTO);
    }
}</code></pre>
<p><code>dto.CalculatorDTO</code></p>
<pre><code class="language-java">@NoArgsConstructor
@RequiredArgsConstructor
@Getter @Setter
@ToString
public class CalculatorDTO {
    @NonNull
    private int num1;
    @NonNull
    private int num2;
    private int sum;
}</code></pre>
<p><code>service.CalculatorService</code></p>
<pre><code class="language-java">@Service
public class CalculatorService {
    public int plusTwoNumbers(CalculatorDTO calculatorDTO) {
        return calculatorDTO.getNum1() + calculatorDTO.getNum2();
    }
}</code></pre>
<p><code>resources.application.yml</code></p>
<pre><code class="language-yaml">server:
  port: 7777</code></pre>
<p>즉, 백엔드 서버의 포트는 7777이다.</p>
<hr />
<p>이번엔 Vue 프로젝트를 만들었는데 App.vue에서 만약에 포트를 내 임의대로 바꾸면 어떻게 될까?
<code>App.vue</code></p>
<pre><code class="language-javascript">&lt;template&gt;
  &lt;div class=&quot;plus&quot;&gt;
    &lt;h1&gt;덧셈 기능 만들기&lt;/h1&gt;
    &lt;label&gt;num1: &lt;/label&gt;&lt;input type=&quot;text&quot; v-model=&quot;num1&quot;&gt;&amp;nbsp;
    &lt;label&gt;num2: &lt;/label&gt;&lt;input type=&quot;text&quot; v-model=&quot;num2&quot;&gt;&amp;nbsp;
    &lt;button @click=&quot;sendPlus&quot;&gt;더하기&lt;/button&gt;
    &lt;hr&gt;
    &lt;p&gt;`{{ num1 }} + {{ num2 }} = {{  result }}`&lt;/p&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
  import { ref } from 'vue';

  const num1 = ref(0);
  const num2 = ref(0);
  const result = ref(0);

  const sendPlus = async() =&gt; {
    const response = await fetch(`http://localhost:7777/plus?num1=${num1.value}&amp;num2=${num2.value}`);
    const data = await response.json();
    result.value = data.sum;
  }

&lt;/script&gt;

&lt;style scoped&gt;

&lt;/style&gt;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/85910485-4d23-471f-8432-0bf4fb5852ec/image.png" /></p>
<p>CORS 문제처럼 보이지만 SOP에 의해서 우리의 웹브라우저가 막았다는 뜻이다.</p>
<p>Vue에서는 5173 포트지만 현재 백엔드 서버는 7777 포트이다.
임의로 바꿔봤자 SOP 문제로 의해 웹 브라우저에서 막게 되는 것이고 그것을 해결할 방법을 알아볼 것이다.</p>
<p>proxy로 5173번을 7777번으로 바꿔서 백엔드와 연동하는 방법을 첫 번째로 알아보자.</p>
<p>우선 vueProject 에서 <code>vite.config.js</code> 파일로 가서 사진과 같이 바꿔준다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1537e250-9bb0-4b16-837a-8cc671964033/image.png" /></p>
<pre><code class="language-javascript">  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:7777',
        changeOrigin: true,
        rewrite: (path) =&gt; path.replace(/^\/api/, '') // 프론트에서 fetch로 날아갈 때 해당 하는 URI는 replace를 ''로 속인다.
      }
    }
  }</code></pre>
<p>이제 위와 같이 target도 지정해줬으니 App.vue에서 response를 만져보자.</p>
<p><strong>App.vue</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/668fcb47-4f51-4bbc-af17-b2de411756e7/image.png" /></p>
<p><strong>결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/95dc5441-6c7e-46b9-8598-7ecd174d053b/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8327b5d8-1751-485f-88fa-e8831201c5dc/image.png" /></p>
<hr />
<p>다음은 백엔드에서 처리해주는 방식이다. (이걸 권장하기는 한다.)</p>
<p>Spring 코드로 가서 프로젝트에 추가하자.
<code>config.WebConfig</code></p>
<pre><code class="language-java">@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping(&quot;/**&quot;)
                .allowedOrigins(&quot;http://localhost:5173&quot;)
                .allowedMethods(&quot;GET&quot;, &quot;POST&quot;, &quot;PUT&quot;, &quot;DELETE&quot;);
    }
}</code></pre>
<p>그리고 Vue Project의 App.vue에서도 response 부분을 2개 코드 중 윗 라인으로 바꾼 뒤 실행해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4bbc7695-2e79-49e8-a2ca-f0af4016c5b7/image.png" /></p>
<p><strong>결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/12465355-0585-4bc0-a652-306bc965850d/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7041b030-bb63-47ab-b5c1-52ac849a08bd/image.png" /></p>
<p>잘 되는 것을 알 수 있다.</p>
<hr />
<p>CORS 문제로 어려움을 겪지만 비교적 간단하다.</p>
<p>다음 주는 Docker에 대해 공부해 게시글을 작성할 예정이다.</p>