<p>기존에서는 파일 업로드는 파일 원본 이름, 파일 rename된 이름, 저장된 경로, 카테고리, 올린 사람(로그인한 사람 ID), 용량 등등 을 묶어서 서버로 보내진다.</p>
<p>Spring으로 단일 파일 업로드, 다중 파일 업로드에 대해서 알아보자.</p>
<hr />
<h1 id="단일-파일-업로드">단일 파일 업로드</h1>
<p>일단 단일 파일 업로드의 html 파일부터 보자.</p>
<p><strong>main.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 align=&quot;center&quot;&gt;파일 업로드 하기&lt;/h1&gt;
    &lt;h3&gt;single file 업로드&lt;/h3&gt;
    &lt;form action=&quot;single-file&quot; method=&quot;post&quot; enctype=&quot;multipart/form-data&quot;&gt;
        파일 : &lt;input type=&quot;file&quot; name=&quot;singleFile&quot;&gt;&lt;br&gt;
        파일 설명 : &lt;input type=&quot;text&quot; name=&quot;singleFileDescription&quot;&gt;&lt;br&gt;
        &lt;input type=&quot;submit&quot; value=&quot;업로드&quot;&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<hr />
<p><strong>화면 캡처</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/87684b45-c810-4ef2-96d3-8f6fe3e2f057/image.png" /></p>
<hr />
<p>파일의 업로드는 3가지 방식으로 된다.</p>
<ol>
<li><strong>바이트 배열</strong>로 보낸다.</li>
<li><code>multipart/form-data</code> 이라는 특정한 인코딩 형태일 때 -&gt; <strong>MultipartFile</strong> 로 보낸다.</li>
<li><strong>문자열</strong>로 보낸다. (base64 문자열 인코딩 형태)</li>
</ol>
<p>2번째 방식으로 해볼 것이다. (html 파일에도 적어뒀다.)</p>
<p>이제 파일 업로드 컨트롤러를 보자.</p>
<p><strong>FileUploadController</strong></p>
<pre><code class="language-java">@Controller
public class FileUploadController {

    @PostMapping(&quot;single-file&quot;)
    public String singleFileUpload() {
        return &quot;redirect:/result&quot;;
    }

    @GetMapping(&quot;result&quot;)
    public void result() {}
}</code></pre>
<hr />
<p>위에서 '업로드' 버튼을 누르면 <code>&quot;single-file&quot;</code> 에 해당하는 메소드를 매핑해서 찾아서 실행하고, <code>singleFileUpload()</code> 메소드는 <code>redirect</code> 방식으로 <code>result()</code> 메소드를 또 찾아서 호출한다.</p>
<p><strong>왜 redirect 방식으로 할까?</strong></p>
<p>사진을 올리면 <code>resources/static</code>에 일단 저장이 되고 몇 초 뒤에 build 안에 저장된다.</p>
<p>그리고 JVM이 이해하기 위해서는 build에 올라와야 한다.</p>
<p>하지만 <code>static</code>에 저장이 되고 바로 <code>redirect</code>를 하면 <code>rebuild</code> 하는 동안 알 수가 없기 때문에 JVM이 이미지를 찾을 수 없는 문제가 생긴다.</p>
<blockquote>
<p>해결하기 위해서 <strong>새로운 폴더를 만들어서</strong> 규격을 정해서 <strong>build 안에 저장하는 방식</strong>을 알아보자.</p>
</blockquote>
<p><strong>FileUploadController에 추가</strong></p>
<pre><code class="language-java">@Controller
public class FileUploadController {

    private ResourceLoader resourceLoader;

    @Autowired
    public FileUploadController(ResourceLoader resourceLoader) {
        this.resourceLoader = resourceLoader;
    }

    중략...
}</code></pre>
<p>build 된 곳에 파일 업로드 경로를 지정하기 위해서 <code>ResourceLoader</code> 의존성 주입을 받게 했다.</p>
<p>그리고 <strong>새로운 폴더에 만들 것이기 때문에</strong> 위에 컨트롤러 속 메소드인 <code>singleFileUpload()</code>에 추가하자.</p>
<pre><code class="language-java">    @PostMapping(&quot;single-file&quot;)
    public String singleFileUpload() throws IOException {
        Resource resource = resourceLoader.getResource(&quot;classpath:static/uploadFiles/img/single&quot;);
        System.out.println(&quot;파일 업로드 할 폴더의 절대 경로 : &quot; + resource.getFile().getAbsolutePath());

        return &quot;redirect:/result&quot;;
    }</code></pre>
<p>resource를 만들고 폴더의 절대 경로를 출력해보았다.</p>
<p>일단 서버를 켜보면 build 속의 resources 폴더 안에도 변화가 생긴다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c655ab51-f3bb-4ced-9713-3f07154766f9/image.png" /></p>
<p><strong>출력된 문장</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8558649e-2c7a-4082-a71c-44801ac847e0/image.png" /></p>
<hr />
<p>이제 추가해줘야 하는 것은 <code>singleFileUpload()</code> 메소드에 <strong>매개변수로 MultipartFile과 파일 설명에 대한 문자열이 들어가야 한다.</strong></p>
<pre><code class="language-java">    @PostMapping(&quot;single-file&quot;)
    public String singleFileUpload(@RequestParam MultipartFile singleFile, @RequestParam String singleFileDescription) throws IOException {
        System.out.println(&quot;singleFile = &quot; + singleFile);
        System.out.println(&quot;singleFileDescription = &quot; + singleFileDescription);

        Resource resource = resourceLoader.getResource(&quot;classpath:static/uploadFiles/img/single&quot;);
        String filePath = resource.getFile().getAbsolutePath();
        System.out.println(&quot;파일 업로드 할 폴더의 절대 경로 : &quot; + filePath);

        return &quot;redirect:/result&quot;;
    }</code></pre>
<p>매개변수에 <code>@RequestParam</code> 으로 받게끔 했다.</p>
<p>아직은 시작하고 사진과 설명을 적어서 업로드해도 500 에러는 뜨겠지만, 출력은 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/66f37e77-323f-4c34-b7e3-eb0a070ebdb0/image.png" /></p>
<hr />
<p>근데 또 다른 문제가 있다. </p>
<p>사진을 업로드했을 때 같은 이름의 파일을 저장할 수 없기 때문에 (1), (2)와 같이 이름을 바꾸게 된다.</p>
<p>ex) <code>apple.png</code>를 올리면 <code>apple(1).png</code>, <code>apple(2).png</code> 형식으로 저장된다.</p>
<blockquote>
<p><strong>해결하는 방법</strong></p>
<ol>
<li>날짜 형식으로 이름을 지어서 저장시켜준다.</li>
<li>Random한 이름을 지어서 넣어준다. (UUID) 활용.</li>
</ol>
</blockquote>
<blockquote>
<p>여기서 나온 UUID에 대해서 간단 정리
<strong>UUID</strong> : 16진수로 표현되는 32자리의 숫자인데 랜덤으로 만들어준다. 주로 PK로 사용함.</p>
<p>-&gt; 이걸 마구잡이로 쓰면 좋지 않다.</p>
<p>왜인지 간단하게 말하면, MySQL의 InnoDB같은 스토리지 엔진은 우리가 봤을 때 값이 들어가는 것처럼 보이지만 그렇지 않다.</p>
<p>commit을 해줘야지 값이 반영이 되는 것이고, 눈에 보이는건 반영이 된 데이터들이 아니다.</p>
<p>즉, InnoDB에서 데이터는 commit 하기 전까지 Primary Key를 중심으로 클러스터링(비슷한 것들끼리 모임) 하는데 UUID로 random하게 마구잡이로 만들면 sql 쿼리 처리가 느려질 수 있다.</p>
<p>느려지는 이유 :  Primary Key가 비슷한 애들끼리 클러스터링 되는데? UUID는 random이라 클러스터링 되지 않기 때문에</p>
<p>이걸 <strong>해결하는 방법</strong>을 알아보면, UUID는 16진수로 된 32자리 숫자라고 했는데 앞의 8자리를 'time_low' 라고 한다.</p>
<p>이 <strong>'time_low'를 우리가 아는 날짜 데이터로 하는 것이 하나의 해결 방법</strong>이다.</p>
<p>날짜 데이터 포맷 형식인 'YYYYMMDD' 형태로 8글자를 넣어버리면, 데이터를 넣은 날의 날짜끼리 클러스터링이 돼서 그나마 효율적이게 된다.</p>
</blockquote>
<p>잠시 딴 길로 샜다.. 다시 컨트롤러로 넘어오자.</p>
<ul>
<li>본래의 파일 이름을 저장해두는 <code>originFileName</code> 변수</li>
<li>확장자를 알아둘 <code>ext</code> 변수</li>
<li>UUID 형태로 이름을 저장할 <code>saveName</code> 변수</li>
</ul>
<p>3가지를 작성해봤다. 출력도 해보겠다.</p>
<p><strong>컨트롤러 속 <code>singleFileUpload</code> 메소드에 추가</strong></p>
<pre><code class="language-java">        String originFileName = singleFile.getOriginalFilename();
        System.out.println(&quot;originFileName = &quot; + originFileName);

        String ext = originFileName.substring(originFileName.lastIndexOf(&quot;.&quot;));
        System.out.println(&quot;ext = &quot; + ext);

        String saveName = UUID.randomUUID().toString();
        System.out.println(&quot;saveName = &quot; + saveName);</code></pre>
<p><strong>출력된 문장 사진</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4fef9380-44d0-4356-b7dd-b523ff3176b3/image.png" /></p>
<hr />
<p>하나만 더 수정해보자.</p>
<p>saveName 을 아예 겹칠 일 없고, 파일이름의 형태로 만들어야 하니까
&quot;-&quot; &lt;&lt; 하이푼을 없애고, 뒤에 ext 확장자를 추가해줄 것이다.</p>
<p><code>saveName</code><strong>은 아래처럼 변경</strong></p>
<pre><code class="language-java">String saveName = UUID.randomUUID().toString().replace(&quot;-&quot;, &quot;&quot;) + ext;</code></pre>
<p><strong>출력된 문장 사진</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3a91074b-2f5f-4150-807a-85dd6af6f5af/image.png" /></p>
<hr />
<p>이제 해야할 것은</p>
<ol>
<li><strong>지정된 위치에 업로드된 파일 저장</strong> (+ <strong>예외처리도 생각해봐야 한다.</strong>)</li>
<li>DB에 저장할 정보 추출(DB 모델링)</li>
</ol>
<p>1번 과정부터 해보자.</p>
<p><code>singleFile</code>을 내가 원하는 파일 경로인 <code>filePath</code> 와 저장된 이름 <code>saveName</code>을 이용해서 업로드를 해줘야 한다.</p>
<p>이것을 해주는 것이 <code>transferTo()</code> 메소드이다.</p>
<p>그것을 추가할 것이고, 예외처리도 생각해야하니까 예외문도 추가해줄 것이다.</p>
<p><code>singleFileUpload()</code> <strong>메소드에 추가</strong></p>
<pre><code class="language-java">        try {
            singleFile.transferTo(new File(filePath + &quot;/&quot; + saveName));

            /* 원래는 이 부분에서 비즈니스 로직 구문 작성 (서비스 계층 -&gt; DB) */

        } catch (Exception e) {
            new File(filePath + &quot;/&quot; + saveName).delete();
        }</code></pre>
<p>비즈니스 로직을 성공했다고 치고 Redirect 된 페이지에 값을 넘기기 위해 <code>RedirectAttributes</code> 사용할 것이다. 이것도 추가해본다.</p>
<p>값을 넘기기 위해서 3번째 매개변수에 추가해주고 사용했다.</p>
<pre><code class="language-java">    public String singleFileUpload(@RequestParam MultipartFile singleFile, @RequestParam String singleFileDescription, RedirectAttributes redirectAttributes) throws IOException {
        (기존 코드)

        try {
            (기존 코드)

            redirectAttributes.addFlashAttribute(&quot;message&quot;, &quot;파일 업로드 성공.&quot;);
            redirectAttributes.addFlashAttribute(&quot;img&quot;, &quot;uploadFiles/img/single/&quot; + saveName);
            redirectAttributes.addFlashAttribute(&quot;singleFileDescription&quot;, singleFileDescription);
        } catch (Exception e) {
            new File(filePath + &quot;/&quot; + saveName).delete();
        }
        (기존 코드)
    }</code></pre>
<p>이렇게 하면 저장은 된다.</p>
<p>하지만 기존 코드에서 아직 반환 화면인 result는 작성하지 않았기에 500 에러가 뜬다.</p>
<p>확인하는 방법 : 경로에 파일을 저장을 하긴 했기 때문에 아래 사진과 같이 URL과 <code>saveName</code>을 적어주면 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/356baba0-241a-4e3f-a1a3-007255b05c4d/image.png" /></p>
<p>이렇게 나오는 것을 알 수 있다.</p>
<p><code>saveName</code><strong>이 출력된 문장</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1fbd5776-0c9d-47d5-b9d7-d03e0b0372fb/image.png" /></p>
<hr />
<p><strong>result 화면 만들어주기</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 th:text=&quot;${message}&quot;&gt;&lt;/h1&gt;

    &lt;img th:src=&quot;${img}&quot; alt=&quot;다람쥐&quot;&gt;
    &lt;p th:text=&quot;${singleFileDescription}&quot;&gt;&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<p>이제 잘 나온다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e8689aa3-66c5-4ca9-9290-fe31be00cad6/image.png" /></p>
<hr />
<h1 id="다중-파일-업로드">다중 파일 업로드</h1>
<p>단일에서 만들었던 것에 추가하면서 만들어보겠다.</p>
<p><strong>main.html</strong></p>
<pre><code class="language-html">기존 코드 아래에 추가

    &lt;h3&gt;multi file 업로드&lt;/h3&gt;
    &lt;form action=&quot;multi-file&quot; method=&quot;post&quot; enctype=&quot;multipart/form-data&quot;&gt;
        파일 : &lt;input type=&quot;file&quot; name=&quot;multiFile&quot; multiple&gt;&lt;br&gt;
        파일 설명 : &lt;input type=&quot;text&quot; name=&quot;multiFileDescription&quot;&gt;&lt;br&gt;
        &lt;input type=&quot;submit&quot; value=&quot;업로드&quot;&gt;
    &lt;/form&gt;</code></pre>
<p><code>파일 : &lt;input type=&quot;file&quot; name=&quot;multiFile&quot; multiple&gt;&lt;br&gt;</code> 여기에 single과의 차이는 <code>name=&quot;multiFile&quot;</code> 까지 적는 것이 아닌, 추가로 <code>multiple</code>을 적어줬다.</p>
<hr />
<p><strong>FileUploadController에 메소드 추가 (일부만 추가했음)</strong></p>
<pre><code class="language-java">    @PostMapping(&quot;multi-file&quot;)
    public String multiFileUpload(@RequestParam List&lt;MultipartFile&gt; multiFiles, @RequestParam String multiFileDescription, RedirectAttributes redirectAttributes) throws IOException {

//        Resource resource = resourceLoader.getResource(&quot;classpath:static/uploadFiles/img/multi&quot;);
//        String filePath = resource.getFile().getAbsolutePath();

        String filePath = resourceLoader.getResource(&quot;classpath:static/uploadFiles/img/multi&quot;).getFile().getAbsolutePath();

        List&lt;Map&lt;String, String&gt;&gt; files = new ArrayList&lt;&gt;();     // 파일 하나에 대한 정보를 지닌 map을 모아둔 리스트
        List&lt;String&gt; saveFiles = new ArrayList&lt;&gt;();             // 화면 단에서 img 태그가 참조할 정적 리소스 경로 (src 속성)

        for (int i = 0; i &lt; multiFiles.size(); i++) {
            String originFileName = multiFiles.get(i).getOriginalFilename();
            String ext = originFileName.substring(originFileName.lastIndexOf(&quot;.&quot;));
            String saveName = UUID.randomUUID().toString().replace(&quot;-&quot;, &quot;&quot;) + ext;

            Map&lt;String, String&gt; file = new HashMap&lt;&gt;();
            file.put(&quot;originFileName&quot;, originFileName);
            file.put(&quot;saveName&quot;, saveName);
            file.put(&quot;filePath&quot;, filePath);
            file.put(&quot;multiFileDescription&quot;, multiFileDescription);

            files.add(file);

            multiFiles.get(i).transferTo(new File(filePath + &quot;/&quot; + saveName));
            saveFiles.add(&quot;uploadFiles/img/multi/&quot; + saveName);
        }

        return &quot;redirect:/result&quot;;
    }</code></pre>
<p>files 변수에서 <code>List&lt;Map&lt;String, String&gt;&gt;</code> 는 파일 하나에 대한 정보를 지닌 map을 모아둔 리스트를 의미한다.</p>
<ol>
<li>map 사용</li>
<li>개인적으로 만들어서 FileUploadDTO 사용</li>
</ol>
<p>2가지 방식으로 리스트에 넣는 방법이 있긴 하다.</p>
<p>근데 만약에 내가 3개 파일을 한 번에 넣을건데 1, 2 번 파일은 저장됐고, 3번째 파일에서 에러가 생기면 그 때 다시 1, 2 번 파일을 삭제해주는 예외처리를 해줘야 한다.</p>
<p>위의 코드에 try-catch 문을 작성해주자</p>
<pre><code class="language-java">try {
            for (int i = 0; i &lt; multiFiles.size(); i++) {
                String originFileName = multiFiles.get(i).getOriginalFilename();
                String ext = originFileName.substring(originFileName.lastIndexOf(&quot;.&quot;));
                String saveName = UUID.randomUUID().toString().replace(&quot;-&quot;, &quot;&quot;) + ext;

                Map&lt;String, String&gt; file = new HashMap&lt;&gt;();
                file.put(&quot;originFileName&quot;, originFileName);
                file.put(&quot;saveName&quot;, saveName);
                file.put(&quot;filePath&quot;, filePath);
                file.put(&quot;multiFileDescription&quot;, multiFileDescription);

                files.add(file);

                multiFiles.get(i).transferTo(new File(filePath + &quot;/&quot; + saveName));
                saveFiles.add(&quot;uploadFiles/img/multi/&quot; + saveName);
            }
        } catch (Exception e) {
            for(int i = 0; i &lt; saveFiles.size(); i++) {
                Map&lt;String, String&gt; file = files.get(i);
                new File(filePath + &quot;/&quot; + file.get(&quot;saveName&quot;)).delete();
            }
        }</code></pre>
<p><code>saveFiles</code> 는 문제 없이 파일 업로드된 파일 경로가 담겨 있다. (즉, 1, 2 번 파일이 저장돼 있다.</p>
<p>이제 추가로 예외가 발생하지 않았다면 출력해줄 메시지와
예외 발생 시의 메시지만 출력해주면 끝이다.</p>
<p><strong>try문에서 for문이 지난 뒤에 추가해줄 코드</strong></p>
<pre><code class="language-java">            redirectAttributes.addFlashAttribute(&quot;message&quot;, &quot;다중 파일 업로드 성공&quot;);
            redirectAttributes.addFlashAttribute(&quot;imgs&quot;, saveFiles);
            redirectAttributes.addFlashAttribute(&quot;multiFileDescription&quot;, multiFileDescription);</code></pre>
<p><strong>catch문에서 for문이 끝난 뒤에 추가해줄 코드</strong></p>
<pre><code class="language-java">            redirectAttributes.addFlashAttribute(&quot;message&quot;, &quot;다중 파일 업로드 실패&quot;);</code></pre>
<hr />
<p><strong>마지막으로 수정할 result.html</strong></p>
<pre><code class="language-html">&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 th:text=&quot;${message}&quot;&gt;&lt;/h1&gt;

    &lt;div th:if=&quot;${img != null}&quot;&gt;
        &lt;img th:src=&quot;${img}&quot; alt=&quot;업로드한 사진&quot;&gt;
        &lt;p th:text=&quot;${singleFileDescription}&quot;&gt;&lt;/p&gt;
    &lt;/div&gt;


    &lt;div th:else&gt;
        &lt;th:block th:each=&quot;img: ${imgs}&quot;&gt;
            &lt;img th:src=&quot;${img}&quot; width=&quot;150&quot; height=&quot;150&quot;/&gt;
        &lt;/th:block&gt;
        &lt;p th:text=&quot;${multiFileDescription}&quot;&gt;&lt;&lt;/p&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>
<hr />
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/338f4d69-94fe-4953-8007-a8fb4a3ebd84/image.png" /></p>
<hr />
<p>용량에 관해서는 <code>application.yml</code> 에 대해서 정해주면 된다</p>
<pre><code class="language-yml">spring:
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB</code></pre>