<h1 id="인터페이스">인터페이스</h1>
<blockquote>
<p>인터페이스 : 추상 메소드와 상수 필드만 가질 수 있는 클래스의 변형체</p>
</blockquote>
<p><strong>InterProduct</strong></p>
<pre><code class="language-java">public interface InterProduct {

//    public static final int MAX_NUM = 100; &lt;- public static final 부분은 지워도 된다.
    int MAX_NUM = 100;

//    private static final int MAX_NUM = 100; &lt;- 접근지정자도 interface에서는 public 이어야 한다.

//    public InterProduct() {} &lt;- 인터페이스는 생성자 자체도 필요로 하지 않는다. (가질 수 없다.)

    /* 설명. 기본적으로 추상메소드만 가능하다. (접근지정자는 public) */
//    public abstract void nonStaticMethod(); &lt;- public abstract 생략 가능
    void nonStaticMethod();

    /* 설명. static 메소드는 바디부까지 작성을 허용했다. (JDK 1.8부터 추가된 기능) */
    public static void staticMethod() {}

    /* 설명. non-static 메소드여도 default 키워드를 추가하면 바디부까지 작성을 허용했다. (JDK 1.8부터 추가된 기능) */
    public default void defaultMethod() {}

    /* 설명. private도 abstract가 아닌 온전한 메소드로 사용은 가능하다. (default도 없이) */
    private void privateMethod() {

        /*설명. 다른 public default 메소드에서 활용할 수만 있는 기능 */
    }
}
</code></pre>
<p>인터페이스를 구현하기 위해서는 <code>implements</code> 키워드를 사용해야 한다.</p>
<p><strong>Product</strong></p>
<pre><code class="language-java">public class Product implements InterProduct {
    @Override
    public void nonStaticMethod() {
        System.out.println(&quot;InterProduct의 nonStaticMethod 오버라이딩 메소드 호출됨&quot;);
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        /* 설명. 인터페이스도 객체를 생성할 수 없다. */
//        InterProduct ip1 = new InterProduct(); &lt;- 이렇게 하면 안 된다.

        Product product = new Product();
//        InterProduct product = new Product(); &lt;- 이것도 가능하다.
        product.nonStaticMethod();

    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/afca360e-f7fa-4888-a23d-1b9e27479ab4/image.png" /></p>
<h2 id="사용-이유">사용 이유</h2>
<ul>
<li><p>공유를 목적으로 하는 상수(<code>public static final</code> 필드)를 기반으로 모든 기능을 공통화(<code>public abstract</code> 메소드)해서 강제성을 부여할 목적</p>
</li>
<li><p>단일 상소이라는 단점을 어느 정도 극복하기 위해 사용하기도 한다. 모든 클래스는 하나의 부모 클래스 외에도 여러 개의 인터페이스를 구현할 수 있다.</p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/41ed9a41-5906-4c9e-87e8-885d21c8934c/image.png" /></p>
<pre><code class="language-java">public class Application implements InterOne, InterTwo {

    @Override
    public void interOneMethod() {}

    @Override
    public void interTwoMethod() {}
}</code></pre>
<h2 id="추상-클래스와-인터페이스의-공통점-차이점">추상 클래스와 인터페이스의 공통점, 차이점</h2>
<h3 id="공통점">공통점</h3>
<table>
<thead>
<tr>
<th>구분</th>
<th>추상 클래스</th>
<th>인터페이스</th>
</tr>
</thead>
<tbody><tr>
<td>자체 인스턴스 생성</td>
<td>생성 불가</td>
<td>생성 불가</td>
</tr>
<tr>
<td>다형성 적용 시 상위 타입 활용 가능 유무</td>
<td>가능</td>
<td>가능</td>
</tr>
</tbody></table>
<h3 id="차이점">차이점</h3>
<table>
<thead>
<tr>
<th>구분</th>
<th>추상 클래스</th>
<th>인터페이스</th>
</tr>
</thead>
<tbody><tr>
<td>상속 가능 범위</td>
<td>단일 상속</td>
<td>다중 상속</td>
</tr>
<tr>
<td>키워드</td>
<td>extends 사용</td>
<td>implements 사용</td>
</tr>
<tr>
<td>추상 메소드 갯수</td>
<td>abstract 메소드 0개 이상</td>
<td>모든 메소드는 abstract</td>
</tr>
<tr>
<td>abstract 키워드 명시</td>
<td>명시적 사용</td>
<td>묵시적으로 abstract</td>
</tr>
</tbody></table>