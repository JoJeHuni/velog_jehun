<p>상속에 대해 다뤄보자.</p>
<h1 id="상속">상속</h1>
<blockquote>
<p><strong>상속</strong> : 부모 클래스가 가지는 멤버 (필드, 메소드)를 자식 클래스가 물려 받아 자신의 멤버인 것처럼 사용할 수 있도록하는 것</p>
</blockquote>
<ul>
<li>멤버 외에도 <strong>부모 클래스의 타입 또한 상속 가능</strong> (다형성의 토대)</li>
<li>Java는 <strong>단일 상속</strong>(자식 클래스는 하나의 부모 클래스만 가짐)만 지원한다.</li>
</ul>
<blockquote>
<p>자식 - 부모 - 조부모 관계와 같이 가능은 하다.</p>
</blockquote>
<hr />
<p>상속은 <code>extends</code> 키워드를 사용한다.</p>
<pre><code class="language-java">public class Academy extends Company {

}</code></pre>
<p><code>Academy</code> 가 자식 클래스, <code>Company</code> 가 부모 클래스이다.</p>
<p>-&gt; Academy is a Company -&gt; 아카데미는 하나의 회사이다. 라는 말이 어울리므로 자식이다.</p>
<p><strong>자식 클래스는 부모 클래스의 필드와 메소드를 포함하고, 확장한 클래스이다.</strong></p>
<h2 id="상속-사용-이유">상속 사용 이유</h2>
<ul>
<li><strong>기존에 작성됐던 클래스의 멤버를 재사용할 수 있다.</strong></li>
<li>클래스 간 <strong>계층 관계를 형성</strong>해 <strong>다형성 문법의 토대</strong>가 된다.</li>
</ul>
<hr />
<p>부모 클래스는 여러 자식 클래스를 가질 수 있으니 가능하면 여러 자식 클래스마다 공통적으로 포함되는 메소드들은 부모 클래스에 정의하는게 좋다.</p>
<hr />
<h1 id="상속의-키워드">상속의 키워드</h1>
<h2 id="super-super">super, super()</h2>
<p>인스턴스 생성 시 부모 생성자를 호출해 부모 클래스의 인스턴스도 함께 생성하게 된다.</p>
<p>이 때 생성한 부모 인스턴스의 주소를 보관하는 레퍼런스 변수로 자식 클래스는 모든 생성자, 메소드에서 사용할 수 있다.</p>
<blockquote>
<p>super() : 부모 생성자를 호출하는 구문, 인자와 매개변수 타입, 개수, 순서가 일치하는 생성자를 호출하게 된다.</p>
</blockquote>
<p>사실 super는 안 적어도 기본생성자처럼 실행된다.</p>
<p>computer is a product 라는 문장을 이용해서 computer와 product를 상속 관계로 해보자.</p>
<p><strong>Product</strong></p>
<pre><code class="language-java">public class Product {
    private String code;
    private String brand;
    private String name;
    private int price;
    private java.util.Date manufactureDate;

    public Product() {
    }

    public Product(String code, String brand, String name, int price, Date manufactureDate) {
        this.code = code;
        this.brand = brand;
        this.name = name;
        this.price = price;
        this.manufactureDate = manufactureDate;
    }
}</code></pre>
<p><strong>Computer</strong></p>
<pre><code class="language-java">public class Computer extends Product {
    private String cpu;
    private int hdd;
    private int ram;
    private String operatingSystem;
}</code></pre>
<p>이 때, Computer에도 기본생성자를 만들려고 하면
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ffb86d3b-c0d0-4515-a322-e66a4ab36f38/image.png" /></p>
<p>이렇게 나오는데, 위의 코드의 생성자에 대해서 말하는 것이다.</p>
<p>아래와 같이 만들 수 있다. (Product의 기본 생성자는 super()를 생략할 수 있어서 빈 칸으로 보이지만 추가해줬다.)</p>
<pre><code class="language-java">import java.util.Date;

public class Computer extends Product {
    private String cpu;
    private int hdd;
    private int ram;
    private String operatingSystem;

    public Computer() {
        super();
    }

    public Computer(String cpu, int hdd, int ram, String operatingSystem) {
        super();
        this.cpu = cpu;
        this.hdd = hdd;
        this.ram = ram;
        this.operatingSystem = operatingSystem;
    }

    public Computer(String code, String brand, String name, int price, Date manufactureDate, String cpu, int hdd, int ram, String operatingSystem) {
        super(code, brand, name, price, manufactureDate);
        this.cpu = cpu;
        this.hdd = hdd;
        this.ram = ram;
        this.operatingSystem = operatingSystem;
    }
}</code></pre>
<p><code>Product</code> 와 <code>Computer</code> 둘다 <code>toString()</code>도 추가해주고 실행해보면?</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.Date;

public class Application {

    public static void main(String[] args) {
        Product product = new Product();
        System.out.println(product);

        Product product2 = new Product(&quot;p01&quot;, &quot;예시&quot;, &quot;자바&quot;, 1000, new Date());
        System.out.println(product2);
//        System.out.println(new Date().toString());

        Computer computer = new Computer();
        System.out.println(computer);

        Computer computer1 = new Computer(&quot;snap dragon&quot;, 512, 16, &quot;Android&quot;);
        System.out.println(computer1);

        Computer computer2 = new Computer(&quot;s-1234&quot;, &quot;Google&quot;, &quot;픽셀&quot;, 1000000, new Date(), &quot;snap dragon&quot;, 1024, 32, &quot;Android&quot;);
        System.out.println(computer2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/06a05785-8cc8-4607-80f6-dda02b773dae/image.png" /></p>
<p><code>computer2</code> 는 부모의 생성자들도 받아온 객체여서 <code>code, brand, name, price, manufatureDate</code>가 더 필요한데 <code>toString()</code>에서 더 나와야 하니 수정해준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/956d2320-2532-4759-9f6b-50af07b4d9da/image.png" /></p>
<p>원래는 이렇게 생성됐지만</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d37f729a-a1c7-48b2-a9a0-8f2acc96c974/image.png" /></p>
<p><code>super.</code> 을 활용해서 더 추가해줬다.</p>
<ul>
<li>필드가 더 많아지면 엄청 불편해진다... -&gt; <strong>개선할 방법?</strong></li>
</ul>
<blockquote>
<p><strong>부모 클래스의 toString()을 활용</strong></p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8bafea1c-d8e1-4b0c-b676-799e01ca08bc/image.png" /></p>
<p>위 사진처럼 변경해주면 된다.</p>
<blockquote>
<p>부모 클래스의 필드인 <code>code</code>, <code>brand</code> 등 출력하기 위해 추가했던 필드들은 자식 클래스에서 출력할 때 내 필드라고 할 수 있기 때문에 꼭 <code>super.</code>을 붙이지 않아도 된다.</p>
<ul>
<li><code>this.getCode()</code></li>
<li><code>getCode()</code>
를 해줘도 괜찮다.</li>
</ul>
<p>하지만, 자식클래스에서도 똑같은 <code>code</code>라는 필드가 있다면? -&gt; 그 때 <code>super.</code>을 붙이거나 <code>this</code>를 붙이거나 하는 것</p>
</blockquote>
<p><strong>실행결과</strong></p>
<pre><code>Product{code='null', brand='null', name='null', price=0, manufactureDate=null}
Product{code='p01', brand='예시', name='자바', price=1000, manufactureDate=Wed Jul 17 10:39:34 KST 2024}
Product{code='null', brand='null', name='null', price=0, manufactureDate=null}Computer{cpu='null', hdd=0, ram=0, operatingSystem='null'}
Product{code='null', brand='null', name='null', price=0, manufactureDate=null}Computer{cpu='snap dragon', hdd=512, ram=16, operatingSystem='Android'}
Product{code='s-1234', brand='Google', name='픽셀', price=1000000, manufactureDate=Wed Jul 17 10:39:34 KST 2024}Computer{cpu='snap dragon', hdd=1024, ram=32, operatingSystem='Android'}</code></pre><hr />
<h2 id="오버라이딩-overriding">오버라이딩 (Overriding)</h2>
<blockquote>
<p><strong>오버라이딩</strong> : 부모클래스에서 상속받은 메소드를 <strong>자식클래스가 재정의</strong>하여 사용하는 기술</p>
</blockquote>
<ul>
<li><strong>성립 조건</strong></li>
</ul>
<ol>
<li>메소드명 동일</li>
<li>메소드 리턴타입 동일</li>
<li>매개변수의 타입, 개수, 순서가 동일</li>
<li>부모 클래스의 private 메소드는 오버라이딩 불가능</li>
<li>부모 클래스의 final 키워드가 사용된 메소드는 오버라이딩 불가능</li>
<li>접근제어자는 부모 메소드와 같거나 더 넓은 범위여야 함</li>
<li>예외처리는 같은 예외이거나 더 구체적(하위)인 예외를 처리해야 함</li>
</ol>
<blockquote>
<p>오버로딩과 헷갈릴만 하지만 구별하기는 쉽다.</p>
</blockquote>
<p>오버라이딩은 말그대로 매개변수, 메소드명 그대로 두고 자식클래스에서 다시 정의하는 것이고,
오버로딩은 같은 클래스에서 매개변수를 다르게 해서 정의하는 것이다.</p>
<p>스프링에서는 <code>@Override</code> 라고 하는 어노테이션이 있다.</p>
<ul>
<li><code>@Override</code> 어노테이션 사용 이유</li>
</ul>
<ol>
<li>메소드 중에 부모로부터 물려받은 메소드인 것을 한 눈에 알아보기 쉽게 하기 위해 (가동석 측면)</li>
<li>부모의 메소드를 오버라이딩할 때 발생할 수 있는 실수를 방지하기 위해 (실수 방지 측면)</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b3cdbc97-1978-4359-aa71-5c274cb50905/image.png" /></p>
<p>진짜 간단하게 출력문을 이용해서 알아보자.
<code>Car</code>, <code>FireCar</code>, <code>RacingCar</code> 3개의 클래스를 만들어본다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3b3c52f6-14ef-492f-ab66-162699a60045/image.png" /></p>
<p>이런 상속 관계로 시작한다.</p>
<p><strong>Car</strong></p>
<pre><code class="language-java">public class Car {

    private boolean runningStatus;

    public Car() {
        System.out.println(&quot;Car 클래스의 기본 생성자 호출됨.&quot;);
    }

    public void run() {
        runningStatus = true;
        System.out.println(&quot;자동차가 달리기 시작합니다.&quot;);
    }

    public void soundHorn() {
        if (runningStatus) {
            System.out.println(&quot;뿌 뿌&quot;);
        } else {
            System.out.println(&quot;running 중이 아니면 경적 못 울림&quot;);
        }
    }

    public boolean isRunningStatus() {
        return runningStatus;
    }

    public void stop() {
        runningStatus = false;
        System.out.println(&quot;자동차가 멈춥니다.&quot;);
    }
}</code></pre>
<p><strong>FireCar</strong></p>
<pre><code class="language-java">public class FireCar extends Car {

    public FireCar() {
        super();
        System.out.println(&quot;FireCar 클래스의 기본 생성자 호출됨.&quot;);
    }

    public void sprayWater() {
        System.out.println(&quot;화재지 발견 쉭쉭 (물 뿌리는 소리)&quot;);
    }

    @Override
    public void soundHorn() {
        System.out.println(&quot;빠아아아아아아앙&quot;);
    }
}</code></pre>
<p><strong>RacingCar</strong></p>
<pre><code class="language-java">public class RacingCar extends Car {

    @Override
    public void run() {
        System.out.println(&quot;레이싱 자동차가 신나게 달림 ㅋ 쓔우웅~~&quot;);
    }

    @Override
    public void soundHorn() {
        System.out.println(&quot;얘는 유명한 경적없는 차량임&quot;);
    }
}</code></pre>
<p>위와 같이 <code>run()</code>, <code>soundHorn()</code> 2개의 메소드를 부모 클래스 뿐만이 아닌 자식 클래스에서도 재정의가 가능하다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        Car car = new Car();

        car.soundHorn();
        car.run();
        car.soundHorn();
        car.stop();
        car.soundHorn();

        System.out.println();

        FireCar fireCar = new FireCar();
        fireCar.soundHorn();
        fireCar.run();
        fireCar.soundHorn();
        fireCar.stop();
        fireCar.soundHorn();
        fireCar.sprayWater();

        System.out.println();

        RacingCar racingCar = new RacingCar();
        racingCar.soundHorn();
        racingCar.run();
        racingCar.soundHorn();
        racingCar.stop();
        racingCar.soundHorn();
    }
}</code></pre>
<p><strong>실행결과</strong></p>
<pre><code>Car 클래스의 기본 생성자 호출됨.
running 중이 아니면 경적 못 울림
자동차가 달리기 시작합니다.
뿌 뿌
자동차가 멈춥니다.
running 중이 아니면 경적 못 울림

Car 클래스의 기본 생성자 호출됨.
FireCar 클래스의 기본 생성자 호출됨.
빠아아아아아아앙
자동차가 달리기 시작합니다.
빠아아아아아아앙
자동차가 멈춥니다.
빠아아아아아아앙
화재지 발견 쉭쉭 (물 뿌리는 소리)

Car 클래스의 기본 생성자 호출됨.
얘는 유명한 경적없는 차량임
레이싱 자동차가 신나게 달림 ㅋ 쓔우웅~~
얘는 유명한 경적없는 차량임
자동차가 멈춥니다.
얘는 유명한 경적없는 차량임</code></pre><hr />
<h3 id="추가---클래스에-final">추가 - 클래스에 final</h3>
<blockquote>
<p><code>class</code>에 <code>final</code> 키워드가 붙으면 <strong>부모 클래스가 될 수 없다.</strong></p>
</blockquote>