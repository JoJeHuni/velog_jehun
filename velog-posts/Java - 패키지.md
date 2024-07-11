<h1 id="패키지">패키지</h1>
<blockquote>
<p>패키지 : 서로 관련 있는 클래스 or 인터페이스 등을 모아 하나의 묶음(그룹)으로 단위를 구성한 것</p>
</blockquote>
<ul>
<li>같은 패키지 내 동일한 이름의 클래스 생성은 <strong>불가</strong> (다른 패키지라면 가능하다.)</li>
<li>원래 클래스명의 일부분이다.</li>
<li>경우에 따라 생략이 가능하다. (하지만 자동으로 작성되는 내용이다.) + (같은 패키지라면 생략 가능하다.)</li>
</ul>
<pre><code class="language-java">public class Application1 {
    public static void main(String[] args) {
        com.orgiraffers.section01.method.Calculator cal = new com.orgiraffers.section01.method.Calculator();

        int plusResult = cal.plusTwoNumbers(100, 20);
        System.out.println(&quot;plusResult : &quot; + plusResult);

    }
}</code></pre>
<p>위와 같이 다른 패키지에 있는 클래스 속 메소드라면 풀네임을 적어주면서 객체도 생성 후 의존성 주입을 해줘야 한다.</p>
<p>하지만, static 메소드는 다르다.</p>
<p>이전 게시글에서도 다룬 내용이지만 고정된 static 메모리 영역에 저장되기 때문에 풀네임은 적지만 객체 생성은 필요 없다.</p>
<pre><code class="language-java">        int maxResult = com.orgiraffers.section01.method.Calculator.maxNumberOf(100, 20);
        System.out.println(&quot;maxResult = &quot; + maxResult);</code></pre>
<p>이렇게 긴 패키지 명을 쭉 적는 것을 개선하기 위해서 나온 것이 <code>import</code> 문이다.</p>
<p><code>import</code> 문을 통해 패키지를 적용시키면 저렇게 길게 적지 않아도 된다.</p>
<h2 id="import-문-사용">import 문 사용</h2>
<pre><code class="language-java">import com.orgiraffers.section01.method.Calculator;

public class Application2 {

    public static void main(String[] args) {
        /* 설명. non-static 메소드의 경우 */
        Calculator calculator = new Calculator();

        int result = calculator.plusTwoNumbers(80, 22);
        System.out.println(result);

        /* 설명. static 메소드의 경우 */
        int max = Calculator.maxNumberOf(40, 90);
        System.out.println(max);
    }
}</code></pre>
<p>간략하게 말하면 내가 적은 <code>Calculator</code> = <code>com.orgiraffers.section01.method</code> 속에 있는 <code>Calculator</code> 라는 것을 정의한 것이다.</p>