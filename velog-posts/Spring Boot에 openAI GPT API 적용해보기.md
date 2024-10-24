<p>이번 헬스관련 프로젝트를 진행하면서 openAI 에서의 개발한 인공지능 모델인 ChatGPT 를 활용하여 운동 루틴을 만드는 기능을 추가했다.</p>
<p>원하는 답변을 얻기 위해서는 여러 가지 고려해야할 것들이 있다. 맨 아래에 추가해두겠다.</p>
<p>적용하는 부분이 궁금하면 중단원에 적용을 넣어둘테니 바로 가서 확인하면 된다.</p>
<hr />
<h2 id="모델">모델</h2>
<blockquote>
<ol>
<li>ChatGPT-3(babbage-002, davinci-002)</li>
<li>ChatGPT-3.5 instruct(4k)</li>
<li>ChatGPT-3.5 Turbo(16k)</li>
<li>ChatGPT-4(32k)</li>
<li>ChatGPT-4 Turbo(128k)</li>
</ol>
</blockquote>
<p>GPT API 를 활용하는건 유료이기 때문에 토큰을 사고 그걸 통해 사용하는 것이라는걸 알아두자.</p>
<hr />
<h2 id="프롬프팅-파인튜닝-관련-개념">프롬프팅, 파인튜닝 관련 개념</h2>
<p>나는 이전까지 프롬프팅을 통해 GPT api 에게 내가 원하는 결과값을 도출해내기 위해 정보를 입력하는 하나의 과정이라고 생각하였다. 근데 딱 그 정도일 뿐 파인튜닝이 무엇인지, 어떻게 도출해내는지는 전혀 몰랐다.</p>
<p>AI 모델과 상호작용할 때의 접근 방식에 대해 몇 가지 알아보자.</p>
<ul>
<li><strong>프롬프팅</strong> (Prompting)</li>
<li><strong>프롬프트 엔지니어링</strong> (Prompt Engineering)</li>
<li><strong>파인튜닝</strong> (Fine-tuning)</li>
</ul>
<hr />
<h3 id="프롬프팅">프롬프팅</h3>
<ul>
<li>기본 상호작용 방식으로 <strong>프롬프트를 작성하여 원하는 결과를 얻는 가장 간단한 방법</strong></li>
</ul>
<p>사용자가 AI 모델에 질의나 명령을 주는 일반적인 행위이며, 간단한 질문이나 명령어를 통해 AI의 기본적인 출력을 얻어낼 수 있다.</p>
<hr />
<h3 id="프롬프트-엔지니어링">프롬프트 엔지니어링</h3>
<ul>
<li><strong>프롬프트 최적화 기법</strong>으로 AI 모델의 출력을 개선하기 위한 기술적 접근으로, 프롬프트 자체를 세밀하게 조정한다.</li>
</ul>
<p>AI 모델이 최적화된 결과를 내도록 하기 위해 프롬프트를 세밀하게 설계하는 과정 </p>
<p>이 기술은 AI가 더 복잡한 질문에 대해 정확한 답을 내놓을 수 있도록 프롬프트의 구조나 내용을 조정가능하다.</p>
<p>목적으로는 &quot;AI의 성능을 극대화하기 위해 질문을 설계하고 최적화&quot;이다.</p>
<p><code>예시: &quot;5가지 주요 포인트로 개념을 요약하고 각 포인트에 예시를 제공해줘&quot;</code></p>
<p>나도 몰랐는데 ChatGPT 에게 질문할 때 자연스럽게 프롬프트 엔지니어링을 하고 있었던 것이다.</p>
<hr />
<h3 id="파인튜닝">파인튜닝</h3>
<ul>
<li><strong>모델 재학습 및 최적화 방법</strong></li>
</ul>
<p>이미 사전 학습된 AI 모델에 특정 도메인 또는 작업에 맞는 데이터를 추가적으로 학습시켜 모델을 조정하는 과정이며, 이는 모델 자체를 변경하여 특정한 작업에 더 적합하게 만드는 방식이다.</p>
<p>목적으로는 &quot;특정 작업에 맞는 AI 모델을 훈련하여 더 높은 정확성과 성능을 달성&quot;이다.</p>
<p><code>예시: 특정 산업(의료, 법률 등)에 맞춘 데이터로 AI 모델을 재훈련시켜 해당 분야에 특화된 답변을 제공하도록 조정</code></p>
<hr />
<h3 id="공통점-및-차이점">공통점 및 차이점</h3>
<ul>
<li><p>공통점:
모두 AI 모델의 성능을 향상시키기 위한 다양한 접근 방식을 나타낸다.</p>
</li>
<li><p>차이점:</p>
</li>
<li><p><em>파인튜닝*</em>은 프롬프트와 상관없이 모델 자체를 조정(다시 학습)하는 작업이고, <strong>프롬프팅</strong>과 <strong>프롬프트 엔지니어링</strong>은 AI 모델에 입력을 주는 방식(상호작용)의 차이에 집중하는것이다.</p>
</li>
</ul>
<hr />
<h2 id="흐름">흐름</h2>
<ol>
<li>사용자가 프롬프트를 입력</li>
<li>GPT 모델에 맞춰 해당 프롬프트를 수행</li>
<li>입력받은 값을 토큰으로 변환</li>
<li>입력받은 값이 토큰으로 변환될 때까지 프로세스 반복</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4bb4ee62-a1fa-449e-abcb-7f4c8d4b5e8d/image.png" /></p>
<h3 id="token">Token</h3>
<p>ChatGPT의 Token -&gt; 사용자가 입력한 프롬프트 메시지가 'Token' 단위로 나뉘게 되는데, 이것은 단어 or 단어의 일부가 된다.</p>
<p>토큰에는 한도가 있는데, 모델마다 프롬프트 값과 응답받은 값의 합에 따라 한도를 지정한다.</p>
<p>ex) 최대 5000개의 토큰이 있는데 프롬프트로 4500개의 토큰을 사용하면 최대 500개 토큰으로 응답 값을 반환하게 된다.</p>
<hr />
<h2 id="적용">적용</h2>
<h3 id="gpt-api-key-발급">GPT API Key 발급</h3>
<ul>
<li><a href="https://platform.openai.com/api-keys">https://platform.openai.com/api-keys</a> 로 이동</li>
<li>API Keys -&gt; &quot;Create new Secret Key&quot;
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fd43cdeb-9177-4482-a2d0-3e0628f71f18/image.png" /></li>
<li>위 화면에서 키 이름을 입력하고 secret key를 생성하면 된다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/223e9c2f-5736-4c58-a3d5-a3010f235e26/image.png" /></p>
<blockquote>
<p>그리고 이제 결제를 해두면 된다. 10달러만 해도 엄청 오래 쓸 수 있긴 하다.</p>
</blockquote>
<blockquote>
<p>이 key는 application.yml에 추가해줘야하기 때문에 다른 곳에 보이지 않게끔 하자.</p>
</blockquote>
<hr />
<h3 id="spring-boot-설정">Spring Boot 설정</h3>
<p>여기서부터는 나뉘게 될텐데 이해를 위해 참고한 사이트는 아래 링크로 걸어두겠다.
<a href="https://velog.io/@rising_developer/SPRING-spring-openAI%EC%B1%97%EC%A7%80%ED%94%BC%ED%8B%B0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0">[SPRING] spring + openAI(챗지피티) 사용하기</a></p>
<ul>
<li><strong>application.yml</strong><pre><code class="language-yml">openai:
model: gpt-4o
secret-key: {내 gpt의 api key}
</code></pre>
</li>
</ul>
<pre><code>&gt; 나는 gpt-4o 를 사용했었고, 3.5 turbo를 사용하고 싶다면 `&quot;gpt-3.5-turbo&quot;`

나도 작성하면서 많은 게시글을 보고 추가적으로 알게된 점들을 작성하는데, 다들 세팅법이 조금씩 다르긴 하지만 맨 처음 접한 사람은 그대로 따라해도 괜찮을 것이다.

---

- **GptConfig**
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;

@Configuration
public class GptConfig {

    @Value(&quot;${openai.secret-key}&quot;)
    private String secretKey;

    @Value(&quot;${openai.model}&quot;)
    private String model;

    public String getSecretKey() {
        return secretKey;
    }

    public String getModel() {
        return model;
    }
}</code></pre><p><code>RestTemplate</code>, <code>HttpHeaders</code>를 여기에 추가적으로 적는 사람들도 있다.</p>
<ul>
<li><p>선택사항 (<code>GptConfig</code>에 추가)</p>
<pre><code class="language-java">  @Bean
  public RestTemplate restTemplate() {
      RestTemplate restTemplate = new RestTemplate();
      return restTemplate;
  }

  @Bean
  public HttpHeaders httpHeaders() {
      HttpHeaders headers = new HttpHeaders();
      headers.setBearerAuth(secretKey);
      headers.setContentType(MediaType.APPLICATION_JSON);
      return headers;
  }</code></pre>
</li>
</ul>
<p>우선은 난 아래 선택사항을 추가하지 않고 진행했다.</p>
<hr />
<h4 id="프로젝트-흐름">프로젝트 흐름</h4>
<ol>
<li><p>사용자가 운동부위와 운동 할 시간을 선택하고 운동루틴 생성 요청</p>
</li>
<li><p>controller에서 해당 요청을 처리하기 위해 service 호출</p>
</li>
<li><p>service에서 운동 루틴을 만들어주는 로직 실행</p>
<ul>
<li><p>prompt를 String 형으로 만들어주고</p>
</li>
<li><p>OpenAI를 불러오는 <code>callOpenAI</code> 메소드를 담아서 불러온 뒤 운동 루틴을 데이터 형태로 나누기 위해 Parser 활용</p>
<pre><code class="language-java">@Override
public String generateRoutine(String userCode, String bodyPart, int time) {

   List&lt;ExerciseEquipmentDTO&gt; exerciseEquipment = exerciseEquipmentService.findByBodyPart(bodyPart);
   String prompt = generatePrompt(userCode, bodyPart, time, exerciseEquipment);

   try {
       String response = callOpenAI(prompt, 3000);
       routineParser(response);
       return response;
   } catch (JsonProcessingException e) {
       throw new CommonException(StatusEnum.INTERNAL_SERVER_ERROR);
   }
}</code></pre>
</li>
</ul>
</li>
</ol>
<hr />
<blockquote>
<p>물론 여기서 prompt는 맨 아래에 있는 링크를 참고해 <strong>직접 해보는 것을 추천한다.</strong>
우리는 바로 밑 사진과 같이 진행하긴 했다.</p>
</blockquote>
<ul>
<li><code>generatePropmt</code> 코드
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/63b45d0f-e52e-4119-b978-46d6fd68adc3/image.png" />전체 코드는 길어서 대략 이런 느낌으로 했다는걸 보기만 하고 직접 적용해보자.</li>
</ul>
<hr />
<ul>
<li><code>callOpenAI</code> 코드 : prompt 한 것을 가지고 gpt-api에 넣어준다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/14fd55e6-73bc-468d-9ff2-9c0811538c2f/image.png" /></li>
</ul>
<hr />
<h3 id="결과">결과</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cfe08ca3-2216-4c8b-acc7-c9652c1cef79/image.png" />
로그인 한 유저의 userCode를 받아오고, 운동 부위와 시간을 입력해 요청하면 운동 루틴을 만들어준다.</p>
<hr />
<h2 id="마무리하며">마무리하며</h2>
<p><code>routineParser</code> 에 대한건 스스로 gpt-api를 활용해서 원하는 데이터를 얻어내는 코드이다.
우리는 운동루틴을 만든 것이기 때문에 <code>routinParser</code> 라는 이름으로 했지만, 이대로 적용을 해보려는 사람들은 또 다른 이름으로 직접 해보는 것이 어떨까 하는 생각을 해본다.</p>
<blockquote>
<p>참고로 gpt가 주는 운동 중에 필요한 데이터인 운동명, 운동 세트, 운동 세트 당 개수 등등 몇 가지 데이터를 뽑아냈다는 것을 힌트로 적용을 하는 사람들에게 도움이 되었으면 한다.</p>
</blockquote>
<p>원하는 데이터 형태를 얻어내기 위해 프롬프트하는 과정에서 돈을 내는 것을 두려워 말고 화이팅</p>
<hr />
<h2 id="참고">참고</h2>
<p><a href="https://modulabs.co.kr/blog/gpt-prompt-engineering/">프롬프트 참고 링크</a></p>
<table>
<thead>
<tr>
<th>원칙</th>
<th></th>
</tr>
</thead>
<tbody><tr>
<td><span style="font-size: 80%;">간결성과 명확성</span></td>
<td><span style="font-size: 80%;">프롬프트는 간결하면서도 구체적이어야 하며, 불필요한 정보는 피해야 합니다.</span></td>
</tr>
<tr>
<td><span style="font-size: 80%;">맥락적 <br />관련성</span></td>
<td><span style="font-size: 80%;">태스크의 배경과 영역을 이해하는데 도움이 되는 키워드, 영역별 전문 용어, 상황 설명 등의 맥락을 제공해야 합니다.</span></td>
</tr>
<tr>
<td><span style="font-size: 80%;">과제 정렬</span></td>
<td><span style="font-size: 80%;">프롬프트는 해당 과제의 성격이 드러나는 언어와 구조를 사용하여 구성되어야 합니다. 이는 형식에 맞는 질문, 명령 또는 빈칸 채우기 문장의 형태로 프롬프트를 구성하는 것을 뜻합니다.</span></td>
</tr>
<tr>
<td><span style="font-size: 75%;">편향 피하기</span></td>
<td><span style="font-size: 80%;">중립적인 언어를 사용하고 윤리적 함의를 고려해야 합니다.</span></td>
</tr>
<tr>
<td><span style="font-size: 80%;">점진적프롬프팅</span></td>
<td><span style="font-size: 80%;">복잡한 과제는 단계별로 나누어 안내할 수 있습니다.</span></td>
</tr>
<tr>
<td><span style="font-size: 75%;">조정 가능성</span></td>
<td><span style="font-size: 80%;">모델의 성능, 응답, 그리고 인간의 피드백에 따라 프롬프트를 조정할 수 있어야 합니다.</span></td>
</tr>
</tbody></table>
<hr />
<h2 id="참고-2-api관련하여-주요-사이트">참고 2 API관련하여 주요 사이트</h2>
<ul>
<li>Open AI API 가격 <a href="https://openai.com/pricing">https://openai.com/pricing</a></li>
<li>Open AI Key 관리 <a href="https://platform.openai.com/api-keys">https://platform.openai.com/api-keys</a></li>
<li>Open AI 사용량 관리 <a href="https://platform.openai.com/usage">https://platform.openai.com/usage</a></li>
<li>Open AI Tokenizer <a href="https://platform.openai.com/tokenizer">https://platform.openai.com/tokenizer</a></li>
<li>Open AI 모델 종류 <a href="https://platform.openai.com/docs/models/overview">https://platform.openai.com/docs/models/overview</a></li>
<li>Open AI API Document <a href="https://platform.openai.com/docs/introduction">https://platform.openai.com/docs/introduction</a></li>
<li>Open AI 최신 모델 API <a href="https://platform.openai.com/docs/api-reference/chat">https://platform.openai.com/docs/api-reference/chat</a></li>
<li>Open AI 레거시 모델 API <a href="https://platform.openai.com/docs/api-reference/completions">https://platform.openai.com/docs/api-reference/completions</a></li>
</ul>