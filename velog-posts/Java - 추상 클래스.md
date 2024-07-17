<h1 id="추상-클래스">추상 클래스</h1>
<blockquote>
<p><strong>추상 클래스</strong> : 객체를 생성 못하며 메소드의 기능이 없고 메소드의 헤더부만 존재하는 불완전한 클래스</p>
</blockquote>
<ul>
<li><strong>추상메소드를 &quot;0개&quot; 이상 포함하는 클래스</strong></li>
</ul>
<p>ex) Product 클래스 자체는 필요 없지만, Product 아래의 computer, phone과 같은 객체는 필요할 때 Product를 추상 클래스도 사용한다.</p>
<p>추상 클래스는 <strong>상속을 활용해 하위 클래스 타입의 인스턴스를 이용해서 인스턴스를 생성해야 한다.</strong></p>
<hr />
<h2 id="추상-메소드">추상 메소드</h2>
<blockquote>
<p><strong>추상 메소드</strong> : 메소드의 헤드(head)부만 있고 바디(body)부가 없는 메소드
불완전한 메소드를 하나라도 가진 클래스는 반드시 추상메소드가 돼야 한다.
<code>ex) public abstract void method();</code> </p>
</blockquote>
<hr />
<h2 id="추상-클래스-사용-이유">추상 클래스 사용 이유</h2>
<blockquote>
<p>추상 클래스는 스스로 인스턴스 생성은 못하지만, 다형성 적용을 위한 <strong>부모 타입 역할을 해낼 수 있다.</strong></p>
</blockquote>
<blockquote>
<p>추상 메소드를 포함한 추상 클래스는 추상 메소드를 통해 자식 클래스에 오버라이딩에 대한 <strong>강제성을 부여할 수 있다.</strong></p>
</blockquote>
<p>-&gt; 추상 클래스를 상속받는 자식 클래스는 반드시 추상 메소드를 오버라이딩해야 한다</p>
<ul>
<li><strong>강제성 부여, 규약</strong></li>
</ul>
<p>필수 기능을 정의해 일관된 <strong>인터페이스(동일 기능)를 제공함에 있어 도움이 된다</strong></p>
<hr />
<p>상위 추상 클래스인 <strong>Product</strong></p>
<pre><code class="language-java">public abstract class Product {

    private int nonStaticField;
    private static int staticField;

    public Product() {

    }

    public void nonStaticMethod() {

    }

    public static void staticMethod() {

    }

    /* 추상메소드가 0개 이상 존재하면 해당 클래스는 반드시 추상클래스가 된다. */
    public abstract void abstractMethod();
}</code></pre>
<hr />
<p>그것의 자식 클래스인 <strong>SmartPhone</strong> 클래스</p>
<pre><code class="language-java">public class SmartPhone extends Product {
    public SmartPhone() {
        /* 추상클래스라도 자식 객체를 위해서는 객체가 생성된다. */
        super();
    }

    /* 추상메소드를 물려받은 자식 메소드(온전한)는 반드시 오버라이딩해야함 (강제성 부여, 규약) */

    @Override
    public void abstractMethod() {
        System.out.println(&quot;Product 클래스로부터 물려받은 abstractMethod를 오버라이딩한 메소드 호출됨..&quot;);
    }

    public void printSmartPhone() {
        System.out.println(&quot;SmartPhone 클래스의 printSmartPhone 메소드 호출됨&quot;);
    }
}
</code></pre>
<hr />
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
//        Product product = new Product();      // 추상 클래스는 객체 생성 X

        /* abstract 메소드를 구현한 온전한 자식 클래스는 인스턴스를 생성할 수 있다. */
        SmartPhone smartPhone = new SmartPhone();
        Product smartPhone2 = new SmartPhone();     // 추상 클래스도 다형성 적용됨
        smartPhone.abstractMethod();
        smartPhone2.abstractMethod();               // 동적 바인딩에 의한 자식 클래스의 오버라인딩 메소드 실행
    }
}</code></pre>
<p><strong>실행결과</strong></p>
<pre><code>Product 클래스로부터 물려받은 abstractMethod를 오버라이딩한 메소드 호출됨..
Product 클래스로부터 물려받은 abstractMethod를 오버라이딩한 메소드 호출됨..</code></pre><hr />
<p>인터페이스는 다음 게시글로 다뤄보겠다.</p>