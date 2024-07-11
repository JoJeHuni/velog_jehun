<h1 id="메소드">메소드</h1>
<blockquote>
<p>메소드 : 어떤 특정 작업을 수행하기 위한 명령문의 집합</p>
</blockquote>
<h2 id="사용-목적">사용 목적</h2>
<ol>
<li>중복되는 코드를 메소드로 만들어 코드의 반복 사용을 피할 수 있다.</li>
<li>코드의 가독성이 좋아진다.</li>
<li>기능의 변경이 필요한 경우 메소드 부분만 수정하면 되기 때문에, 손쉬운 유지보수가 가능하다.</li>
</ol>
<h2 id="메소드-선언">메소드 선언</h2>
<pre><code class="language-java">{접근제어자} {반환타입} 메소드이름 (매개변수 목록) {
    // 실행할 코드

    // 반환타입에 맞게 return
    // 반환타입이 void 면 return 생략
}</code></pre>
<ol>
<li><p>접근제어자 : 메소드에 접근ㄴ할 수 있는 범위
A. <strong>public</strong> : 어디서나 접근 가능
B. <strong>protected</strong> : 상속관계이거나 같은 패키지에서 접근 가능
C. <strong>default</strong>(생략가능) : 같은 패키지에서 접근 가능
D. <strong>private</strong> : 같은 클래스 내부에서만 접근 가능</p>
</li>
<li><p>반환타입 : 메소드가 모든 작업을 마치고 반환하는 데이터의 타입
A. <strong>void</strong> : 리턴값 없음
B. <strong>기본 변수 자료형</strong> : int, float, 등등
C. <strong>오브젝트형</strong> : String, 이외 사용자 정의타입</p>
</li>
<li><p>메소드 이름 : 메소드를 호출하기 위한 이름</p>
</li>
<li><p>매개변수 목록(parameters) : 메소드 호출 시에 전달되는 인수의 값을 저장할 변수들</p>
</li>
<li><p>실행할 코드  : 메소드의 기능을 수행하는 코드</p>
</li>
</ol>
<pre><code class="language-java">public class Application1 {

    public static void main(String[] args) {

        /* 수업목표. 메소드의 호출 흐름에 대해 이해할 수 있다. */
        System.out.println(&quot;main() 시작함..&quot;);
        methodA();
        System.out.println(&quot;main() 종료됨..&quot;);
    }

    public static void methodA() {
        System.out.println(&quot;methodA() 호출됨..&quot;);
        methodB();
        System.out.println(&quot;methodA() 종료됨..&quot;);
    }

    public static void methodB() {
        System.out.println(&quot;methodB() 호출됨..&quot;);
        methodC();
        System.out.println(&quot;methodB() 종료됨..&quot;);
    }

    public static void methodC() {
        System.out.println(&quot;methodC() 호출됨..&quot;);

        System.out.println(&quot;methodC() 종료됨..&quot;);
    }
}</code></pre>
<pre><code>main() 시작함..
methodA() 호출됨..
methodB() 호출됨..
methodC() 호출됨..
methodC() 종료됨..
methodB() 종료됨..
methodA() 종료됨..
main() 종료됨..</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/688642c0-aaa7-4c84-a13d-e87e22fb9e47/image.png" /></p>
<p>위의 경우는 메소드 안에서 다른 메소드를 호출한다.</p>
<blockquote>
<p>A가 B호출, B가 C호출 -&gt; C 종료된 후 다시 B로 돌아오고 B 종료 -&gt; A 종료 -&gt; main() 종료</p>
</blockquote>
<hr />
<p>다른 경우도 살펴보자.</p>
<pre><code class="language-java">public class Application2 {

    public static void main(String[] args) {
        System.out.println(&quot;main() 시작함...&quot;);

        Application2 app = new Application2();
        app.methodA();
        app.methodB();
        app.methodC();

        System.out.println(&quot;main() 종료됨...&quot;);
    }

    public void methodA() {
        System.out.println(&quot;methodA() 호출됨...&quot;);
        System.out.println(&quot;methodA() 종료됨...&quot;);
    }

    public void methodB() {
        System.out.println(&quot;methodB() 호출됨...&quot;);
        System.out.println(&quot;methodB() 종료됨...&quot;);
    }

    public void methodC() {
        System.out.println(&quot;methodC() 호출됨...&quot;);
        System.out.println(&quot;methodC() 종료됨...&quot;);
    }
}</code></pre>
<p>이전 예제와는 출력 결과가 다를 것이다. 이전 예제는 메소드가 다른 메소드를 출력하지만 이번 예제는 main이 하나씩 호출하는 것이다.</p>
<blockquote>
<p>main이 A호출 -&gt; A종료 후 다시 main이 B호출 -&gt; ... C까지 종료 후 main 종료</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/71a502e2-d8e5-46e1-80d1-88737e4acf13/image.png" /></p>
<blockquote>
<p>왜 <code>Application2 app = new Application2();</code> 부분이 필요할까? 이전까지는 없었는데
Application2와 main을 보면 앞에 <code>static</code> 의 유무에 차이가 있다.
<strong>static이 없는 메소드 (non-static method)</strong>는 <strong>해당 클래스를 인지시킨 후 접근해 호출</strong>한다.</p>
</blockquote>
<blockquote>
<p>static에 관한건 추후에 더 자세하게 정리하겠다.</p>
</blockquote>
<hr />
<h2 id="전달인자와-매개변수를-이용한-메소드-호출">전달인자와 매개변수를 이용한 메소드 호출</h2>
<p>변수는 main() 안에서만 써왔는데 그런 것들을 <strong>지역 변수</strong> 라고 한다.</p>
<h3 id="변수의-종류">변수의 종류</h3>
<ol>
<li>지역변수</li>
<li>매개변수</li>
<li>전역변수(필드)</li>
<li>클래스(static)변수 (추후에 더 다룰 예정)</li>
</ol>
<blockquote>
<p>지역변수 : 메소드 안에 적은 변수</p>
</blockquote>
<ul>
<li>매개변수
메소드 간 서로 공유해야하는 값이 존재하는 경우 메소드 호출 시, 사용하는 괄호를 이용해서 값을 전달할 수 있다.</li>
</ul>
<blockquote>
<p>전달인자 (argument) : 전달하는 값
매개변수 (parameter) : 괄호 안에 전달인자를 받기 위해 선언하는 변수</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/68adb211-9447-4c59-8bcd-bced27922a92/image.png" /></p>
<blockquote>
<p>전역변수 : 메소드가 아닌 클래스에 적은 변수</p>
</blockquote>
<blockquote>
<p>클래스(static)변수 : 실행하면 컴퓨터가 이미 알고 있는 변수</p>
</blockquote>
<pre><code class="language-java">public class Application3 {

    // 3. 클래스에 쓴 전역변수, 클래스 변수
    static int global = 10;

    public static void main(String[] args) {

        // 메소드 안에 써둔 것 1. 지역변수
        int local = 20;

        System.out.println(&quot;global = &quot; + global);
        System.out.println(&quot;local 출력 : &quot; + local);

        /* 설명. testMethod에 나이를 주고 출력하기 */
        Application3 app3 = new Application3();
        app3.testMethod(13);
        app3.testMethod(19);
        app3.testMethod(15);
        app3.testMethod('a');             // a는 아스키코드고 97번
        app3.testMethod((int)12.34);    // int형으로 바꾸면 12
        app3.testMethod(3 * 6);
    }

    /* 설명. 정수형 나이를 주면 문자열을 출력해주는 기능이 있는 메소드 */
    public void testMethod(int age) {
        System.out.println(&quot;당신의 나이는 &quot; + age + &quot;세 입니다.&quot;);
    }
}</code></pre>
<pre><code>global = 10
local 출력 : 20
당신의 나이는 13세 입니다.
당신의 나이는 19세 입니다.
당신의 나이는 15세 입니다.
당신의 나이는 97세 입니다.
당신의 나이는 12세 입니다.
당신의 나이는 18세 입니다.</code></pre><h2 id="메소드-활용">메소드 활용</h2>
<h3 id="여러-개-전달인자-메소드-테스트">여러 개 전달인자 메소드 테스트</h3>
<pre><code class="language-java">        /* 목차 1. 여러 개의 전달인자를 이용한 메소드 호출 */
        Application4 app4 = new Application4();
        app4.testMethod(&quot;홍길동&quot;, 20, '남');

        /* 목차 2. 변수에 저장된 값을 전달하며 호출할 수 있다. */
        String name = &quot;유관순&quot;;
        int age = 20;
        char gender = '여';

        app4.testMethod(name, age, gender);
    }

    public void testMethod(String name, int age, final char gender) {
        System.out.println(&quot;당신의 이름은 &quot; + name + &quot;이고, 나이는 &quot; + age + &quot;세 이며, 성별은 &quot; + gender + &quot;입니다.&quot;);
    }</code></pre>
<pre><code>당신의 이름은 홍길동이고, 나이는 20세 이며, 성별은 남입니다.
당신의 이름은 유관순이고, 나이는 20세 이며, 성별은 여입니다.</code></pre><hr />
<h3 id="메소드-리턴-테스트">메소드 리턴 테스트</h3>
<p>반환 유형이 void를 제외하고는 전부 <code>return</code>이 필요하다.</p>
<p><strong>1. void 형</strong></p>
<blockquote>
<p><code>return</code> : 현재 메소드를 강제 종료하고 호출한 구문으로 다시 돌아가는 명령어</p>
</blockquote>
<pre><code class="language-java">public class Application5 {
    public static void main(String[] args) {

        /* 수업목표. 메소트 리턴에 대해 이해할 수 있다. */
        Application5 app5 = new Application5();
        app5.testMethod();
    }

    public void testMethod() {
        System.out.println(&quot;testMethod() 동작 확인...&quot;);
    }
}</code></pre>
<pre><code>testMethod() 동작 확인...</code></pre><hr />
<p><strong>2. return 활용</strong></p>
<p>위에는 void 형이기 때문에 return이 없었지만, 한 번 return이 되게끔 해보자.</p>
<pre><code class="language-java">public class Application6 {
    public static void main(String[] args) {

        /* 수업목표. 반환값이 있는 메소드를 작성할 수 있다. */
        System.out.println(testMethod());

        // 위처럼 해도 되고
        // String result = testMethod();
        // System.out.println(result); 해도 된다.
    }

    public static String testMethod() {
        System.out.println(&quot;test() 메소드 실행함&quot;);
        return &quot;test&quot;;
    }
}</code></pre>
<pre><code>test() 메소드 실행함
test</code></pre><ol>
<li>testMethod()를 실행하면서 &quot;test() 메소드 실행함&quot; 을 출력한 뒤, </li>
<li>return으로 &quot;test&quot;를 담아서 원래 호출했던 메소드로 돌아온 것이고,</li>
<li>&quot;test&quot;가 담겨진 채로 출력문이 실행돼 2가지가 보이게 된 것.</li>
</ol>
<hr />
<p><strong>3. 다른 클래스에서 메소드 호출</strong>
main()과 함께 있는 클래스에 메소드를 적는 것이 아닌 다른 클래스에 적어보자.</p>
<p><code>Calculator</code> 클래스</p>
<pre><code class="language-java">public class Calculator {
    public int plusTwoNumbers(int first, int second) {
        return first + second;
    }

    public int minusTwoNumbers(int first, int second) {
        return first - second;
    }

    public int multiplyTwoNumbers(int first, int second) {
        return first * second;
    }

    public int divideTwoNumbers(int first, int second) {
        return first / second;
    }

    public static int minNumberOf(int first, int second) {
        return (first &lt; second) ? first : second;
    }

    public static int maxNumberOf(int first, int second) {
        return (first &gt; second) ? first : second;
    }
}</code></pre>
<blockquote>
<p>최솟값, 최댓값에 대한 마지막 2개 메소드는 static을 적었다.</p>
</blockquote>
<p>main() 메소드가 들어 있는 Application7 클래스</p>
<pre><code class="language-java">public class Application7 {
    public static void main(String[] args) {

        int first = 100;
        int second = 50;

        Calculator cal = new Calculator();

        int plus = cal.plusTwoNumbers(first, second);
        int minus = cal.minusTwoNumbers(first, second);
        int multiply = cal.multiplyTwoNumbers(first, second);
        int divide = cal.divideTwoNumbers(first, second);
        int min = Calculator.minNumberOf(first, second);
        int max = Calculator.maxNumberOf(first, second);

        System.out.println(&quot;두 수의 합 : &quot; + plus);
        System.out.println(&quot;두 수의 차 : &quot; + minus);
        System.out.println(&quot;두 수의 곱 : &quot; + multiply);
        System.out.println(&quot;두 수의 몫 : &quot; + divide);
        System.out.println(&quot;최솟값 : &quot; + min);
        System.out.println(&quot;최댓값 : &quot; + max);
    }
}</code></pre>
<p>최솟값, 최댓값은 새로 객체에 의존성을 주입해줄 필요없이 Calculator 로 나타낸 것을 알 수 있다.</p>
<p>이것이 static 때문인데 static은 컴퓨터의 고정된 static 메모리 영역에 저장해 기억시킨다.</p>
<p>그래서 non-static 인 메소드는 직접 객체를 의존성 주입을 시켜줘야하지만 static은 이미 메모리에 들어있어 굳이 해줄 필요가 없다.</p>
<blockquote>
<p>그럼 무조건적으로 static을 쓰는게 좋은게 아닌가?</p>
</blockquote>
<p>그렇지는 않다.</p>
<h4 id="static-단점">static 단점</h4>
<ol>
<li><p><strong>프로그램 실행 시 static이 많으면 부팅하는데 오래 걸린다.</strong></p>
</li>
<li><p><strong>프로그램 종료 시까지 메모리에 할당된 채로 존재</strong>한다.
 static 영역은 <code>Garbage collector</code> 를 통해 관리를 받지 않아서 악영향을 준다.</p>
</li>
<li><p><strong>객체지향적이지 못하다.</strong>
 static 영역에 할당돼 있어 여러 클래스들이 데이터를 불러오고, 캡슐화되어야 한다는 객체지향 프로그래밍 원칙을 위반한다.</p>
</li>
<li><p><strong>Interface 를 구현하는데 사용될 수 없다.</strong>
 코드의 재사용성을 높여주는 객체지향적 기능인 Interface를 사용하는데 방해가 된다.</p>
</li>
</ol>