<h1 id="클래스와-객체">클래스와 객체</h1>
<h2 id="클래스">클래스</h2>
<blockquote>
<p><strong>클래스</strong> : 서로 다른 타입의 데이터와 메소드를 정의하여 사용자 정의의 타입을 만든 것</p>
</blockquote>
<p>-&gt; 클래스 : <strong>사용자 정의의 자료형</strong></p>
<p>유사한 것 : C++의 구조체</p>
<h2 id="객체">객체</h2>
<blockquote>
<p>객체 : 현실에 존재하는 독립적이면서 하나로 취급되는 사물, 개념</p>
</blockquote>
<p>클래스에 정의되는데, new 연산자를 통해 heap에 할당된 공간 =&gt; <strong>인스턴스</strong></p>
<p>객체에게 공통적인 기능, 속성을 <strong>추상화</strong>하여 클래스를 정의한 뒤, 고유 객체를 취급하듯 <strong>메모리를 할당해주면 인스턴스</strong>라고 한다.</p>
<pre><code class="language-java">        /* 목차 1. 변수를 이용한 회원 데이터 관리 */
        String id = &quot;user01&quot;;
        String pwd = &quot;pass01&quot;;
        String name = &quot;홍길동&quot;;
        int age = 20;
        char gender = 'M';
        String[] hobbies = new String[]{&quot;축구&quot;, &quot;볼링&quot;, &quot;테니스&quot;};

        System.out.println(&quot;id = &quot; + id);
        System.out.println(&quot;pwd = &quot; + pwd);
        System.out.println(&quot;name = &quot; + name);
        System.out.println(&quot;age = &quot; + age);
        System.out.println(&quot;gender = &quot; + gender);
        System.out.println(&quot;hobbies = &quot; + hobbies);</code></pre>
<blockquote>
<p><strong>위와 같이 변수들로만 관리할 때 발생할 수 있는 문제점</strong></p>
<ol>
<li>많은 변수명들을 관리하기 힘들 수 있다.</li>
<li>메소드의 전달인자로 전달할 값이 너무 많아 회원과 관련된 기능을 호출할 때 매개변수가 많아진다.</li>
<li>메소드의 반환형으로 회원이라는 개념을 반환할 수 없다.</li>
</ol>
</blockquote>
<p>위와 같이 여러가지 자료형들을 모아서 사용자 정의 자료형이라고 할 수 있는 Member 클래스를 만들어봤다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/938bc0f0-84f1-401c-ab72-cddd4029955d/image.png" /></p>
<p>컴퓨터에 메모리를 할당해준다. stack 영역과 heap 영역에 생기는데 stack에는 Member 클래스, heap 영역에는 Member의 각 데이터에 대한 내용이 들어간다.</p>
<p>사진처럼 만들기만 하면 값이 할당되지 않았기에 null이 들어간다.</p>
<pre><code class="language-java">        Member member = new Member();        // null
        System.out.println(member.name);

        member.name = &quot;김철수&quot;;              // 김철수
        System.out.println(member.name);</code></pre>
<p>나머지 데이터들도 만들어보고 메소드도 만들어봤다.</p>
<pre><code class="language-java">    public static void main(String[] args) {
        /* 설명. 2명의 회원 객체를 만들어 각각 다른 이름 부여해보기 */
        Member member = new Member();
        Member member2 = new Member();
        System.out.println(member.name);
        System.out.println(member2.name);

        member.name = &quot;김철수&quot;;
        System.out.println(member.name);

        member2.name = &quot;김영희&quot;;
        System.out.println(member2.name);

        member.id = &quot;user03&quot;;
        member.pwd = &quot;pass03&quot;;
        member.age = 30;
        member.gender = 'M';
        member.hobby = new String[]{&quot;볼링&quot;, &quot;하키&quot;};

        Member returnValue = changeName(member);
        System.out.println(&quot;개명됐는지 확인 = &quot; + returnValue.name);
    }

        public static Member changeName(Member member) {
        System.out.println(&quot;개명합니다.&quot;);
        member.name = &quot;강태공&quot;;
        return member;
    }</code></pre>
<pre><code>null
null
김철수
김영희
개명합니다.
개명됐는지 확인 = 강태공</code></pre><hr />
<h2 id="객체지향의-특징">객체지향의 특징</h2>
<ol>
<li>추상화</li>
<li>캡슐화</li>
<li>상속</li>
<li>다형성</li>
</ol>
<hr />
<h3 id="추상화">추상화</h3>
<blockquote>
<p>추상화 : 현실 세계의 복잡한 사건을 단순화해 새로운 객체 지향 세계를 창조하는 과정
유연성을 확보하기 위해 공통적인 것을 추출하고, 아닌 것을 제거하는 것</p>
</blockquote>
<p>과정을 통해 객체가 도출되고, 객체를 생성하기 위해 클래스를 설계하게 된다.</p>
<h4 id="객체-지향-프로그래밍이란">객체 지향 프로그래밍이란?</h4>
<blockquote>
<p>현실 세계의 모든 사건(event)는 객체와 객체의 상호작용에 의해 일어난다는 세계관을 프로그램을 만들 때 이용하여 새로운 세계를 창조하는 방법론</p>
</blockquote>
<p>모든 객체들은 수행할 것들을 너무 많이 책임을 가질 필요 없이, <strong>단일 책임의 원칙</strong>을 지키는 것이 중요하다.</p>
<h4 id="객체-간의-상호작용">객체 간의 상호작용</h4>
<p>객체와 객체는 메세지(<strong>메소드 호출</strong>)를 통해 서로 상호작용을 한다.
보내는 쪽을 송신자, 받는 쪽을 수신자라고 하며, 수신자는 메세지를 전달 받아 그 메세지에 해당하는 내용을 처리하는 방법을 스스로 결정한다. </p>
<blockquote>
<ol>
<li><strong>협력</strong> : 애플리케이션에 구현에 필요한 객체 간의 상호작용</li>
<li><strong>책임</strong> : 객체가 협력에 참여하기 위해 수행해야할 작업 -&gt; 기능</li>
<li><strong>역할</strong> : 객체의 책임이 모여 하나의 역할이 된다.</li>
</ol>
</blockquote>
<hr />
<h3 id="캡슐화">캡슐화</h3>
<blockquote>
<p>캡슐화 : 유지보수성 증가를 위해 필드의 직접 접근을 제한하고, public 메소드를 활용해 간접적으로 접근하여 사용할 수 있도록 클래스를 작성하는 기법</p>
</blockquote>
<p>유지보수성 증가를 위해서는 <strong>결합도는 낮게, 응집도는 높게</strong> 해야 한다. (결저응고)</p>
<p>위에서는 직접 member에 값을 넣어주면서 직접 접근했는데 이번엔 클래스를 만들어서 나눠보자.</p>
<ul>
<li><p><strong>Monster</strong> 클래스</p>
<pre><code class="language-java">public class Monster {
  /* 설명. 메소드를 추가함으로 인해 필드가 수정돼도 이 클래스 내에서만 에러가 발생한다. -&gt; 단일 책임의 원칙 */
  String kinds;
  int mp;

  public void setKinds(String kinds) {
      this.kinds = kinds;
  }

  public void setMp(int mp) {
      this.mp = mp;
  }
}</code></pre>
</li>
<li><p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
  public static void main(String[] args) {
      Monster monster1 = new Monster();

      /* 필기. 유지보수에 더 좋은 방법 */
      monster1.setKinds(&quot;프랑켄 슈타인&quot;);
      monster1.setMp(200);
  }
}</code></pre>
</li>
</ul>
<blockquote>
<p>하지만 아직도 직접 접근은 가능한 상태이다.
<code>System.out.println(monster1.kinds);</code> 하면 출력이 된다.</p>
</blockquote>
<p>이것도 이제 캡슐화로 직접 접근도 못하게 할 수 있다.</p>
<ul>
<li><p><strong>Monster</strong>
여기에서 <code>name</code>, <code>hp</code>에 <code>private</code> 접근 지정자를 넣어서 같은 클래스만 접근할 수 있게 했다.</p>
<pre><code class="language-java">public class Monster {
  private String name;
  private int hp;

  public void setName(String name) {
      this.name = name;
  }

  public void setHp(int hp) {
      this.hp = hp;
  }
}</code></pre>
</li>
<li><p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
  public static void main(String[] args) {
      Monster monster = new Monster();
      monster.setName(&quot;드라큘라&quot;);
      monster.setHp(200);

      System.out.println(monster.name);
  }
}</code></pre>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2556a927-9d4e-472c-ae5e-b41e4c993192/image.png" /></p>
<p>이런 식으로 클래스 작성 시에는 특별한 목적이 있지 않으면 캡슐화를 적용하는 것을 기본 원칙으로 한다.</p>
<table>
<thead>
<tr>
<th>구분</th>
<th>같은 클래스</th>
<th>같은 패키지</th>
<th>자식 클래스</th>
<th>전체</th>
</tr>
</thead>
<tbody><tr>
<td>public</td>
<td>O</td>
<td>O</td>
<td>O</td>
<td>O</td>
</tr>
<tr>
<td>protected</td>
<td>O</td>
<td>O</td>
<td>O</td>
<td></td>
</tr>
<tr>
<td>(default)</td>
<td>O</td>
<td>O</td>
<td></td>
<td></td>
</tr>
<tr>
<td>private</td>
<td>O</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>
<hr />
<h2 id="생성자">생성자</h2>
<blockquote>
<p>생성자 : 인스턴스를 생성할 때 초기 수행할 명령이 있는 경우 미리 작성해두고, 인스턴스를 생성할 때 단 한 번 호출되는 함수</p>
</blockquote>
<p>이전까지는 인스턴스를 생성할 대 <code>{클래스명} {레퍼런스 변수} = new {클래스명()};</code> 이렇게 사용했다.</p>
<p>new 뒤에 클래스명은 사실 <strong>생성자</strong> 라는 메소드를 호출하는 구문이다.</p>
<ul>
<li>기본 생성자 : 매개변수가 없는 생성자</li>
</ul>
<p>매개변수가 있는 생성자들은 다른 호칭은 없다.</p>
<p><strong>User 클래스</strong></p>
<pre><code class="language-java">public class User {
    private String id;
    private String pwd;
    private String name;
    private java.util.Date enrollDate;

    /* 설명. 기본 생성자(매개변수가 없는)를 활용한 객체 생성( 반드시 명시적으로 써줄 것  ) */
    public User() {

    }

    /*. 원하는 필드를 초기화하는 매개변수 있는 생성자를 활용한 객체 생성 */
    public User(String id, String pwd, String name, java.util.Date enrollDate) {
        /* 설명. 생성자 내부의 this.은 이 생성자로 생성될 객체를 뜻한다. */
        this.id = id;
        this.pwd = pwd;
        this.name = name;

        /* 위의 id, pwd, name을 다른 생성자를 활용하여 코드의 줄 수를 줄일 수 있다. */
        /* this()를 통해 다른 생성자를 활용할 때는 한 번만 코드 첫 줄에서 활용할 수 있다. */
        // this(id, pwd, name);
        this.enrollDate = enrollDate;
    }

    public String information() {
        return &quot;id: &quot; + this.id + &quot;pwd : &quot; + this.pwd + &quot;name: &quot; + this.name + &quot;enrollDate: &quot; + this.enrollDate;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        User user1 = new User();
        System.out.println(user1.information());

        User user2 = new User(&quot;1L&quot;,&quot;pwd22&quot;,&quot;홍길동&quot;,new java.util.Date());
        System.out.println(user2.information());
    }
}</code></pre>
<hr />
<h3 id="dto-활용">DTO 활용</h3>
<p>계층에 대한 개념이 필요하긴 하지만 일단은 DTO라는 것을 이용한다고 여기고 한 번 보자.</p>
<p><strong>자바빈 (Java Bean)</strong> 이란?
JSP(자바 서버 페이지)에서 사용되는  표준 액션 태그로 접근할 수 있게 만든 자바 클래스혀태이다.
자바코드를 모르는 웹 퍼블릿들도 자바 코드를 사용할 수 있도록 하고, 태그 형식으로 지원한느 문법을 의미하는데, 그 때 사용할 수 있도록 규칙을 지정해놓은 java 클래스를 자바빈 이라고 정의한다.</p>
<p>지금은 <strong>특정 목적 및 프레임워크를 위해 클래스를 작성하는 규칙</strong>이라고 보면 된다.</p>
<p><strong>자바빈 작성 규칙</strong></p>
<ol>
<li>자바빈은 특정 패키지에 속해 있어야 한다.(defalt 패키지 사용 금지)</li>
<li>필드의 접근제어자는 private 로 선언해야 한다. (캡슐화 적용)</li>
<li>기본 생성자가 명시적으로 존재해야 한다. (매개변수 있는 생성자는 선택사항)</li>
<li>모든 필드에 접근 가능한 생성자(setter)와 접근자(getter)가 public으로 작성되어 있어야 함.</li>
<li>직렬화(Serializable) 구현을 고려해야 한다. (선택사항)</li>
</ol>
<p><strong>UserDto</strong></p>
<pre><code class="language-java">import java.util.Date;

public class UserDTO {

    /* 필드(멤버 변수) */
    private String id;
    private String pwd;
    private String name;
    private java.util.Date enrollDate;

    /* 기본생성자 필수로 명시적 작성 */
    public UserDTO() {
    }

    public UserDTO(String id, String pwd, String name, Date enrollDate) {
        this.id = id;
        this.pwd = pwd;
        this.name = name;
        this.enrollDate = enrollDate;
    }

    /* 설정자(setter) 접근자(getter)*/
    public String getId() {
        return this.id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Date getEnrollDate() {
        return enrollDate;
    }

    public void setEnrollDate(Date enrollDate) {
        this.enrollDate = enrollDate;
    }

    /* 모든 멤버 변수를 하나의 String 문자열로 반환하는 toString() */
    @Override
    public String toString() {
        return &quot;UserDTO{&quot; +
                &quot;id='&quot; + id + '\'' +
                &quot;, pwd='&quot; + pwd + '\'' +
                &quot;, name='&quot; + name + '\'' +
                &quot;, enrollDate=&quot; + enrollDate +
                '}';
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {

        /* 수업목표. 생성자를 이용한 객체 초기화와 설정자를 이용한 초기화의 장단점을 이해할 수 있다.*/
        UserDTO user1 = new UserDTO();
        System.out.println(user1.toString());
        user1.setId(&quot;user01&quot;);
        System.out.println(user1.getId());
        System.out.println(user1);
    }
}</code></pre>
<hr />
<h2 id="오버로딩">오버로딩</h2>
<p>오버라이딩과 자주 거론되는 개념으로,</p>
<p>간단하게 같은 이름의 메소드를 메소드 속 매개변수의 <code>타입, 개수, 순서</code> 를 차이로 두고 여러 개 만들 수 있는 것을 뜻한다.</p>
<blockquote>
<p><strong>메소드의 시그니처</strong> : <code>public void method(int num) {}</code> 이라면, 메소드의 메소드 명과 파라미터 선언부 부분을 <strong>메소드의 시그니처</strong>라고 한다. (즉, method(int num))</p>
</blockquote>
<h3 id="오버로딩의-조건">오버로딩의 조건</h3>
<p>매개변수의 타입, 개수, 순서를 다르게 작성하여 하나의 클래스 내에 동일한 이름의 메소드를 여러 개 작성 가능하다.
메소드의 헤드부에 있어 시그니처를 제외한 부분이 다르게 저장되는 것은 인정되지 않는다.</p>
<pre><code class="language-java">public class OverloadingTest {
    // 매개변수 개수, 순서, 타입에 따라 갈린다.
    public void test() {}

    public void test(int num) {}

    public void test(int num1, int num2) {}

    public void test(int num1, String str) {}

    // 위의 타입의 순서가 뒤바뀐 것으로 차이가 생긴 것이다.
    public void test(String str, int num1) {}

}</code></pre>
<hr />
<h2 id="파라미터-parameter">파라미터 (Parameter)</h2>
<blockquote>
<p><strong>파라미터로 사용 가능한 자료형</strong></p>
</blockquote>
<ol>
<li>기본자료형</li>
<li>기본자료형 배열</li>
<li>클래스자료형(참조자료형)</li>
<li>클래스자료형 배열(객체 배열이지만 다음 챕터에서 다룬다.)</li>
<li>가변인자 (사용하는걸 권장하지는 않는다.)</li>
</ol>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        ParameterTest pt = new ParameterTest();

        /* 목차 1. 기본자료형 매개변수로 전달 받는 메소드 호출 */
        int num = 20;
        System.out.println(&quot;call by value 전 : &quot; + num);
        pt.testPrimitiveTypeParameter(num);
        System.out.println(&quot;call by value 후 : &quot; + num);
    }
}</code></pre>
<p><strong>ParameterTest</strong></p>
<pre><code class="language-java">    public void testPrimitiveTypeParameter(int num) {
        num = 10;
        System.out.println(&quot;매개변수로 전달받은 값 : &quot; + num);
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/593a5f78-6a28-4bc6-9f07-eae3a129190d/image.png" /></p>
<p><strong>1. 기본 자료형의 동작</strong> : 값을 넘기면서 매개변수로 활용하는 것 같지만 <strong>call by value 즉, 리터럴 값을 넘기기 때문에 매개변수로 들어가면서 활용되는 것이 아니다.</strong></p>
<blockquote>
<p>리터럴 값 (참조 주소값 X) 을 전달해서 메소드 호출 시, 서로 다른 지역 변수는 서로 영향 X</p>
</blockquote>
<hr />
<p><strong>2. 기본 자료형 (배열)의 동작</strong>
<strong>Application</strong></p>
<pre><code class="language-java">        int[] iArr = new int[]{1, 2, 3, 4, 5};
        System.out.println(&quot;call by reference 전: &quot; + Arrays.toString(iArr));
        pt.testPrimitiveTypeArrayParameter(iArr); // 참조 값에 의한 호출
        System.out.println(&quot;call by reference 후: &quot; + Arrays.toString(iArr));</code></pre>
<p><strong>ParameterTest</strong></p>
<pre><code class="language-java">        public void testPrimitiveTypeArrayParameter(int[] iArr) {
        iArr[0] = 100;
        System.out.println(&quot;매개변수로 전달받은 값 : &quot; + Arrays.toString((iArr)));
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3641aa16-126f-4f8a-a97b-9e51050ceb8f/image.png" /></p>
<p>배열의 주소값을 매개변수로 넣게 돼<code>testPrimitiveTypeArrayParameter</code> 메소드에서 <strong>변경이 가능하다.</strong></p>
<hr />
<p><strong>3. 클래스 자료형</strong>
<strong>Application</strong></p>
<pre><code class="language-java">        Rectangle r1 = new Rectangle(22, 12);
    //    r1.calArea();
    //    r1.calRound();

        pt.testClassTypeParameter(r1);
    }</code></pre>
<p><strong>ParameterTest</strong></p>
<pre><code class="language-java">    public void testClassTypeParameter(Rectangle rectangle) {
        rectangle.calArea();
        rectangle.calRound();
    }</code></pre>
<p><strong>Rectangle</strong></p>
<pre><code class="language-java">public class Rectangle {
    private int height;
    private int width;

    public Rectangle() {
    }

    public Rectangle(int height, int width) {
        this.height = height;
        this.width = width;
    }

    public void calArea() {
        System.out.println(&quot;사각형의 넓이는 &quot; + (this.width * this.height));
    }

    public void calRound() {
        System.out.println(&quot;사각형의 둘레는 &quot; + (this.width + this.height) * 2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/84a03896-9350-4203-82cb-556d27749ecb/image.png" /></p>
<hr />
<ol start="4">
<li>클래스 배열을 추후에 정리한다.</li>
</ol>
<hr />
<p><strong>5. 가변 인자</strong>
어떤 값을 보내든 해당 자료형이면 다 받아내는 것을 가변인자라고 한다.</p>
<p>자바는 웬만하면 권장하지 않지만,, 해보자.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">        pt.testVariableLengthArrayParameter();
        pt.testVariableLengthArrayParameter(&quot;홍길동&quot;);
        pt.testVariableLengthArrayParameter(&quot;유관순&quot;, &quot;볼링&quot;);</code></pre>
<p><strong>ParameterTest</strong></p>
<pre><code class="language-java">    public void testVariableLengthArrayParameter(String... str) {
        System.out.println(&quot;str = &quot; + str);
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/73bb9ee6-3f07-41d0-a628-4ddbe460a5d7/image.png" /></p>
<p>사진을 보면 알 수 있지만 <code>String... str</code> 부분은 <strong>String 배열</strong>인 것을 알 수 있다.</p>
<p>권장하지 않는 이유로는 매개변수를 2개만 넣어야 할 때 3개 이상을 넣어도 잘 돌아가게 된다는 것 자체를 좋아하지 않는다.</p>
<hr />
<h2 id="싱글톤">싱글톤</h2>
<blockquote>
<p><strong>싱글톤 패턴</strong> : 애플리케이션이 시작되고 난 후 어떤 클래스가 최소 한 번만 메모리에 할당되고 그 메모리에 인스턴스가 <strong>단 하나만 생성돼 공유되게 하는 것</strong></p>
</blockquote>
<p>싱글톤과 함께 해서 static을 같이 알아보자.</p>
<blockquote>
<p><strong>static</strong> : 정적 메모리 영역에서 프로그램이 start 되자마자 종료할 때까지 저장하고 싶은 것들을 지정하는 키워드</p>
</blockquote>
<p><strong>EagerSingleton</strong></p>
<pre><code class="language-java">public class EagerSingleton {

    private static EagerSingleton eager = new EagerSingleton(); // 자신이 자신의 주소를 가지고 있다.

    private EagerSingleton() {
    }

    public static EagerSingleton getInstance() {
        return eager;
    }
}</code></pre>
<p>여기에서 <code>EagerSingleton eager</code> 와 <code>getInstance()</code> 는 static 키워드를 가지고 있다. 즉, stack / heap / static 메모리 영역에서</p>
<ol>
<li>시작하자마자 <code>EagerSingleton eager</code> 는 static 영역에 생성된 객체이다.</li>
<li>잘 보면 <code>new EagerSingleton()</code> 로 결국 heap 영역에 같이 <code>EagerSingleton</code>이 생기게 된다.</li>
<li>그것의 주소값을 <code>eager</code> 안에 저장하는 것이다.</li>
<li><code>getInstance()</code> 는 그러면 주소값을 반환하는 것이다.</li>
</ol>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        EagerSingleton eager1 = EagerSingleton.getInstance(); // 프로그램이 켜지자마자 생성된 객체의 주소
        EagerSingleton eager2 = EagerSingleton.getInstance();

        System.out.println(&quot;eager1의 주소 = &quot; + eager1);
        System.out.println(&quot;eager2의 주소 = &quot; + eager2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/16a56817-2ed3-4e1d-8d85-315361db6754/image.png" /></p>
<p>이것은 프로그램이 시작하자마자 만들어지기 때문에 이번엔 시작할 때가 아닌, <strong>메소드가 호출될 때 생성되게끔 만들어보자.</strong></p>
<p><strong>LazySingleton</strong></p>
<pre><code class="language-java">public class LazySingleton {
    private static LazySingleton lazy;

    private LazySingleton() {
    }

    public static LazySingleton getInstance() {
        if (lazy == null) {
            lazy = new LazySingleton();
        }
        return lazy;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        EagerSingleton eager1 = EagerSingleton.getInstance(); // 프로그램이 켜지자마자 생성된 객체의 주소
        EagerSingleton eager2 = EagerSingleton.getInstance();

        System.out.println(&quot;eager1의 주소 = &quot; + eager1);
        System.out.println(&quot;eager2의 주소 = &quot; + eager2);

        LazySingleton lazy1 = LazySingleton.getInstance(); // Lazy는 이 시점에 객체가 생성됨
        LazySingleton lazy2 = LazySingleton.getInstance();

        System.out.println(&quot;lazy1의 주소 = &quot; + lazy1);
        System.out.println(&quot;lazy2의 주소 = &quot; + lazy2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5b6c3a3a-caf1-4ade-813e-fe66253d9585/image.png" /></p>
<p>둘다 보면 그냥 객체 하나로만 활용할 수 있는 것이다. -&gt; 싱글톤.</p>
<h3 id="장단점">장단점</h3>
<p><strong>장점</strong></p>
<ol>
<li>첫 번째 이용 시에는 인스턴스를 생성해야 하므로 속도 차이가 나지 않지만, 2번재 이용 시에는 인스턴스 생성 시간 없이 바로 사용 가능하다 (재사용)</li>
<li>인스턴스가 절대적으로 1개만 추구하는 것을 보증할 수 있다.</li>
</ol>
<p><strong>단점</strong></p>
<ol>
<li>싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유하면 결합도가 높아진다.</li>
<li>동시성 문제를 고려해서 설계해야 하기 때문에 난이도가 높다.</li>
</ol>
<blockquote>
<p><strong>싱글톤 구현 방법</strong></p>
</blockquote>
<ol>
<li>이른 초기화(Eager Initialization)</li>
<li>늦은 초기화(Lazy Initialization)</li>
</ol>
<hr />
<h2 id="static">Static</h2>
<blockquote>
<p><strong>static</strong> : 포로그램이 실행될 때 정적 메모리 영역 (static 영역 or 클래스 영역) 에 할당하는 키워드이다.</p>
</blockquote>
<p>여러 인스턴스가 공유해서 사용할 목적의 공간이다.</p>
<p>대표적인 예 : 싱글톤 객체</p>
<p><strong>static 키워드가 달린 필드</strong>들은 애초에 정적 메모리 영역에 프로그램 시작 ~ 끝가지 저장돼있는 것으로, <strong>관련된 메소드들도 전부 static이 붙어야 한다.</strong></p>
<blockquote>
<p>이미 컴퓨터가 알고 있는 필드인데 해당 필드를 다루는 메소드도 이미 알고 있어야 하는게 맞다.</p>
</blockquote>
<p><strong>StaticTest</strong></p>
<pre><code class="language-java">public class StaticTest {
    private int nonStaticCount;
    private static int staticCount;

    public StaticTest() {
    }

    public int getNonStaticCount() {
        return nonStaticCount;
    }

    public static int getStaticCount() {
        return staticCount;
    }

    public void increaseNonStaticCount() {
        this.nonStaticCount++;
    }

    public static void increaseStaticCount() {
        staticCount++;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        StaticTest st1 = new StaticTest();

        /* 객체를 만들어서 사용하는 것 -&gt; non static */
        System.out.println(&quot;non-static field : &quot; + st1.getNonStaticCount());
        System.out.println(&quot;static field : &quot; + StaticTest.getStaticCount());

        /* 설명. 각 필드의 값들을 하나씩 증가 */
        st1.increaseNonStaticCount();
        StaticTest.increaseStaticCount();

        /* 설명. 두 필드 값 확인 */
        System.out.println(&quot;non-static field : &quot; + st1.getNonStaticCount());
        System.out.println(&quot;static field : &quot; + StaticTest.getStaticCount());

        /* 설명. 새로운 객체 생성 */
        StaticTest st2 = new StaticTest();
        System.out.println(&quot;non-static field : &quot; + st2.getNonStaticCount());
        System.out.println(&quot;static field : &quot; + StaticTest.getStaticCount());
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3892bece-665d-499e-91c3-c21c2918cc00/image.png" /></p>
<p><strong>Application</strong> 에서 시작하자마자
static 영역 : <code>staticCount</code> 가 생기고</p>
<p><code>StaticTest st1 = new StaticTest()</code> 에서
heap 영역 : <code>non-staticCount</code>
stack 영역 : <code>non-staticCount</code>에 대한 값을 가진 <code>st1</code></p>
<p>이렇게 3개 영역에 생기게 된다.</p>
<p>그 때 st2가 새로운 객체가 생기면
static 영역 : <code>staticCount</code> 이것은 공유하는 필드이기에 그대로 1이지만
heap 영역 : <code>non-staticCount</code>
stack 영역 : <code>non-staticCount</code>에 대한 값을 가진 <code>st2</code></p>
<p>st2에서의 <code>non-staticCount</code>는 또다른 객체여서 공유 안 하는 것을 알 수 있다.</p>
<hr />
<h1 id="객체-배열">객체 배열</h1>
<p>레퍼런스 변수에 대한 배열로, 동일한 타입의 인스턴스들을 배열로 관리할 수 있다.</p>
<p><strong>Car</strong></p>
<pre><code>package com.ohgiraffers.section08.object_array;

public class Car {
    private String modelName;
    private int maxSpeed;

    public Car() {
    }

    public Car(String modelName, int maxSpeed) {
        this.modelName = modelName;
        this.maxSpeed = maxSpeed;
    }

    public void driveMaxSpeed() {
        System.out.println(modelName + &quot;차량이 최고 시속 &quot; + maxSpeed + &quot;(km/h)로 달립니다.&quot;);
    }
}</code></pre><p>이런 클래스가 있을 때 Application 에서는 각 객체들을 선언해서 일일이 함수를 호출해야할까?</p>
<pre><code>Car car1 = new Car(&quot;페라리&quot;, 300);
        Car car2 = new Car(&quot;람보르기니&quot;, 510);
        Car car3 = new Car(&quot;롤스로이스&quot;, 250);
        Car car4 = new Car(&quot;부가티&quot;, 400);
        Car car5 = new Car(&quot;포터&quot;, 500);

        car1.driveMaxSpeed();
        car2.driveMaxSpeed();
        car3.driveMaxSpeed();
        car4.driveMaxSpeed();
        car5.driveMaxSpeed();</code></pre><p>이렇게 반복하는 것보다</p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        Car car1 = new Car(&quot;페라리&quot;, 300);
        Car car2 = new Car(&quot;람보르기니&quot;, 510);
        Car car3 = new Car(&quot;롤스로이스&quot;, 250);
        Car car4 = new Car(&quot;부가티&quot;, 400);
        Car car5 = new Car(&quot;포터&quot;, 500);

        Car[] cars = {car1, car2, car3, car4, car5};

        for (Car car : cars) {
            car.driveMaxSpeed();
        }
    }
}</code></pre>
<p>Car에 대한 배열 -&gt; 객체 배열을 선언할 수 있다.</p>