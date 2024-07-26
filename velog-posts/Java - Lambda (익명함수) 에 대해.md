<h1 id="lambda">Lambda</h1>
<blockquote>
<p>람다식 (익명함수) : 메소드를 하나의 식으로 표현한 것</p>
</blockquote>
<pre><code>f(x, y) = x * y;</code></pre><p>위의 식은 x, y의 곱을 표현하는 함수인데 f 이름을 빼고 람다식으로 변경하면 아래처럼 된다.</p>
<pre><code>(x, y) → x * y;</code></pre><p>함수 이름을 빼고 → 이후로 식이 바로 오게 되고 &quot;메소드의 이름과 반환값이 없어진다.&quot;는 의미는 익명 함수를 의미한다.</p>
<hr />
<h2 id="람다식이-필요한-이유">람다식이 필요한 이유</h2>
<ol>
<li>단순하고 편하다.</li>
<li>람다식을 활용할 수 있게 되면 Collection, stream을 연계해서 쉽게 조작 가능하다.</li>
<li>불필요하게 반복되는 코드를 제거할 수 있다.</li>
</ol>
<hr />
<h2 id="람다-표현식">람다 표현식</h2>
<pre><code>// 매개 변수 없는 경우
() -&gt; { ... }

// 매개 변수 있는 경우
(타입 매개변수, ...) -&gt; { ... }</code></pre><p>람다 표현식에서 매개 변수 타입은 런타임 시에 자동으로 인식되기 때문에 매개변수 타입을 일반적으로 언급하지 않아도 된다. </p>
<blockquote>
<p><strong>하나의 매개변수만 존재하는 경우 ( ) 는 생략할 수 있다. **
**실행문이 하나인 경우에 { } 는 생략 가능하다.</strong></p>
</blockquote>
<hr />
<h1 id="함수적-인터페이스">함수적 인터페이스</h1>
<p>자바는 메소드를 따로 선언할 수 없다. -&gt; <strong>항상 클래스의 내부에서만 선언되고, 메소드를 가지고 있는 객체를 생성해야 한다.</strong></p>
<blockquote>
<p>즉, 람다식은 단독으로 선언 불가능.</p>
<p>람다식은 익명 클래스처럼 <strong>인터페이스 변수에 대입</strong>된다.</p>
</blockquote>
<p>인터페이스는 직접 구현을 할 수 없기 때문에 구현하는 클래스가 필요한데, <strong>람다식을 통해 익명 구현 객체를 생성해 사용할 수 있다.</strong></p>
<p>근데, 인터페이스는 여러 가지 추상 메소드를 가질 수 있는데 람다식이 그게 가능할까?</p>
<p>-&gt; 아니다.</p>
<blockquote>
<p>람다식은 하나의 메소드를 정의하기 때문에, 2<strong>개 이상에 추상 메소드가 선언된 인터페이스는 람다식으로 객체 생성이 불가능하다.</strong></p>
</blockquote>
<p><strong>1개 추상 메소드를 가진 인터페이스</strong>만 타깃 타입이 될 수 있는데, 그러한 인터페이스를 <strong>함수적 인터페이스</strong>라고 한다.</p>
<p><code>@FunctionalInterface</code> 이 어노테이션을 사용하여 함수적 인터페이스로 사용해보자.</p>
<p><strong>Calculator 인터페이스</strong></p>
<pre><code class="language-java">@FunctionalInterface
public interface Calculator {
    int sumTwoNumbers(int first, int second);
//    void test();      // 어노테이션 추가 후에는 추상메소드를 2개 이상 가지지 못한다.
}</code></pre>
<p>Calculator의 구현 클래스인 <strong>CalculatorImpl</strong></p>
<pre><code class="language-java">public class CalculatorImpl implements Calculator{
    @Override
    public int sumTwoNumbers(int first, int second) {
        return first + second;
    }
}</code></pre>
<p>위의 것들을 호출할 람다식을 작성할 <strong>Application</strong></p>
<ol>
<li><p><strong>인터페이스를 구현한 구현체(Impl 클래스)를 이용한 방식</strong> (동적바인딩을 활용한 메소드 호출)</p>
<pre><code class="language-java">public class Application {
 public static void main(String[] args) {
     Calculator c1 = new CalculatorImpl();
     System.out.println(&quot;10과 20의 합은: &quot; + c1.sumTwoNumbers(10, 20));
}</code></pre>
</li>
<li><p><strong>익명클래스를 활용한 방식</strong>(인터페이스의 하위 구현체) - 객체만 만들어서 사용하는 것
구현 클래스를 지워도 작동한다. 이름이 없이 즉시 1번만 사용하기 위해 사용하는 것은 가능하다.</p>
<pre><code class="language-java">public class Application {
 public static void main(String[] args) {
     Calculator c2 = new Calculator() {
         @Override
         public int sumTwoNumbers(int first, int second) {
             return first + second;
         }
     };
     System.out.println(&quot;10과 20의 합은: &quot; + c2.sumTwoNumbers(10, 20));
 }
}</code></pre>
</li>
<li><p><strong>람다식을 활용한 방식</strong></p>
<pre><code class="language-java">public class Application {
 public static void main(String[] args) {
//        Calculator c3 = (x, y) -&gt; {return x + y;};
     Calculator c3 = (x, y) -&gt; x + y;
     System.out.println(&quot;10과 20의 합은: &quot; + c3.sumTwoNumbers(10, 20));
 }
}</code></pre>
</li>
</ol>
<p>구문이 하나일 때는 return, return과 관련된 세미콜론(;), 중괄호 삭제 가능하다</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cfd77a05-8d12-4655-ab80-bd91fcde382e/image.png" /></p>
<p>익명클래스에 작성한 저 내용이 람다식으로 줄어든 것이다.</p>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/805e3645-73f1-4a2c-a828-285788a2945f/image.png" /></p>
<hr />
<p>Java 8에서는 빈번하게 사용되는 함수적 인터페이스를 <code>java.util.function</code> 표준 API 패키지로 제공한다.</p>
<p>크게 <code>Consumer</code>, <code>Supplier</code>, <code>Function</code>, <code>Operator</code>, <code>Predicate</code> 로 구분된다.</p>
<h2 id="consumer">Consumer</h2>
<p><code>Consumer</code> 함수적 인터페이스의 특징은 리턴 값이 없는 <code>accept()</code> 메소드를 가지고 있다는 것이다.</p>
<p><code>accept()</code> : 매개 변수로 넘어온 값을 소비하는 역할만 한다.</p>
<blockquote>
<p>소비한다는 것 = return 값이 없다는 말이다.</p>
</blockquote>
<ul>
<li>표 넣을 곳</li>
</ul>
<h3 id="활용">활용</h3>
<pre><code class="language-java">import java.util.function.Consumer;

public class Application {
    public static void main(String[] args) {
        Consumer&lt;String&gt; consumer = (str) -&gt;
            System.out.println(str + &quot;이(가) 입력됨&quot;);
        };
    }
}</code></pre>
<p>이런 식으로 반환형이 없는 메소드 관련 람다식을 쓸 수 있다.</p>
<p><code>BiConsumer&lt;T, U&gt;</code> , <code>ObjIntConsumer&lt;T&gt;</code> 도 추가해보자.</p>
<pre><code class="language-java">import java.time.LocalTime;
import java.util.Random;
import java.util.function.BiConsumer;
import java.util.function.Consumer;
import java.util.function.ObjIntConsumer;

public class Application {
    public static void main(String[] args) {
//          Consumer&lt;String&gt; consumer = (str) -&gt;
//            System.out.println(str + &quot;이(가) 입력됨&quot;);
//        };

        // 매개변수가 한 개이면 소괄호 생략 가능하며 실행문이 하나인 경우 중괄호도 생략 가능하다.(현재는 return이 없는 구문)
        Consumer&lt;String&gt; consumer = str -&gt; System.out.println(str + &quot;이(가) 입력됨&quot;);
        consumer.accept(&quot;Hello&quot;);

        BiConsumer&lt;String, LocalTime&gt; biConsumer =
                (str, time) -&gt; System.out.println(str + &quot;이(가) &quot; + time + &quot;에 입력됨&quot;);
        biConsumer.accept(&quot;Hello?&quot;, LocalTime.now());

        ObjIntConsumer&lt;Random&gt; objIntConsumer =
                (random, bound) -&gt; System.out.println(&quot;0부터 &quot; + bound + &quot;전 까지의 난수 발생: &quot;
                        + random.nextInt(bound));
        objIntConsumer.accept(new Random(), 10);
    }
}</code></pre>
<p>추가)</p>
<ul>
<li><code>Random</code> 은 <code>Math.random()</code> 과는 다르다.
<code>Math.random()</code>메소드는 -&gt; <code>(int)(Math.random() * 10) + 1</code> 을 통해 1~10까지의 난수를 만들었다.
<code>Random</code> 클래스는 -&gt; <code>new Random().nextInt(10) + 1</code> 으로 할 수 있다.
<code>Math.random</code>은 <code>static</code>이지만, <code>Random</code> 클래스는 <code>non-static</code> 이다.</li>
</ul>
<hr />
<h2 id="supplier">Supplier</h2>
<p><code>Supplier</code> 함수적 인터페이스는 <strong>매개변수가 없고 리턴 값이 있는</strong> <code>getXXX()</code> 메소드를 가지고 있다.</p>
<p><code>Consumer</code> <strong>와 반대 개념</strong>인 것이다. 이 메소드는 실행되면 <strong>호출한 곳으로 값을 리턴해준다.</strong></p>
<ul>
<li>표 넣을 곳</li>
</ul>
<h3 id="활용-1">활용</h3>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.time.LocalDateTime;
import java.util.function.BooleanSupplier;
import java.util.function.Supplier;

public class Application {
    public static void main(String[] args) 
        /* 설명. 추상메소드의 매개변수가 없을 경우나 2개 이상일 경우에는 소괄호()를 생략할 수 없다. */
        Supplier&lt;LocalDateTime&gt; supplier = () -&gt; LocalDateTime.now();
        System.out.println(supplier.get());

        BooleanSupplier booleanSupplier = () -&gt; {
            int random = (int)(Math.random() * 2);
            return random == 0? false: true;
        };
        System.out.println(&quot;랜덤 true or false 생성기: &quot; + booleanSupplier.getAsBoolean());
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/787c7a99-e270-4934-9c55-bec52fb69287/image.png" /></p>
<hr />
<h2 id="function">Function</h2>
<p>이번에는 매개변수와 리턴값도 반환하는 함수적 인터페이스를 알아보자.</p>
<p><code>Function</code> 함수적 인터페이스는 매개변수와 리턴값이 있는 <code>applyXXX()</code> 를 가지고 있다.
이 메소드들은 매개 변수 타입과 리턴 타입에 따라서 다양한 메소드들이 있다.</p>
<ul>
<li>표 넣을 곳</li>
</ul>
<hr />
<h2 id="활용-2">활용</h2>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.function.BiFunction;
import java.util.function.Function;

public class Application {
    public static void main(String[] args) {
        /* 매개변수 및 반환형이 있는 메소드 관련 람다식 */
        Function&lt;String, Integer&gt; function = str -&gt; Integer.valueOf(str);
        String strValue = &quot;12345&quot;;
        System.out.println(function.apply(strValue) instanceof Integer);

        /* 매개변수 2개에 반환형까지 작성해서 람다식 작성 가능하다. */
        BiFunction&lt;String, String, Integer&gt; biFunction =
                (str1, str2) -&gt; Integer.valueOf(str1) + Integer.valueOf(str2);
        System.out.println(biFunction.apply(&quot;12345&quot;, &quot;11111&quot;));
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b14ff3bf-72cc-4651-8f31-3505c1889878/image.png" /></p>
<hr />
<h2 id="operator">Operator</h2>
<p><code>Operator</code> 함수적 인터페이스는 <code>Function</code> 과 똑같이 작동한다. 
매개변수와 리턴값이 있는 <code>applyXXX()</code> 메소드를 가지고 있다. </p>
<p>다른 점은 뭐가 있을까?</p>
<p>-&gt; 매개변수와 반환값이 모두 같은 타입인 메소드 관련 람다식이라는 것이다.</p>
<ul>
<li>표 넣을 곳</li>
</ul>
<hr />
<h3 id="활용-3">활용</h3>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.function.BinaryOperator;
import java.util.function.UnaryOperator;

public class Application {
    public static void main(String[] args) {
        /* 설명. 매개변수 및 반환형이 있지만 모두 같은 타입인 메소드 관련 람다식(feat.제네릭 타입 동일) */
        UnaryOperator&lt;String&gt; unaryOperator = str -&gt; str + &quot;World!!&quot;;
        System.out.println(unaryOperator.apply(&quot;Hello, &quot;));

        BinaryOperator&lt;String&gt; binaryOperator = (str1, str2) -&gt; str1 + str2;
        System.out.println(binaryOperator.apply(&quot;Hello2, &quot;, &quot;World!!2&quot;));
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7b0a6229-9a26-46f1-83b9-eb4ab6e5d3d8/image.png" /></p>
<hr />
<h2 id="predicate">Predicate</h2>
<p><code>Predicate</code> 함수적 인터페이스는 매개 변수와 <code>boolean</code> 리턴 값이 있는 <code>testXXX()</code> 를 가지고 있다.</p>
<p><code>testXXX()</code> : 매개변수 값을 이용해 true 또는 false 를 리턴하는 역할</p>
<ul>
<li>표 넣는 곳</li>
</ul>
<hr />
<h3 id="활용-4">활용</h3>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.function.Predicate;

public class Application {
    public static void main(String[] args) {
        /* 설명. boolean 반환형을 가지는 메소드 관련 람다식 */
        Predicate&lt;Object&gt; predicate = value -&gt; value instanceof String;
        System.out.println(&quot;문자열인지 확인: &quot; + predicate.test(&quot;123&quot;));
        System.out.println(&quot;문자열인지 확인: &quot; + predicate.test(123));
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b7dd4503-cbcf-4cca-9e23-1bf2d5e0e237/image.png" /></p>
<hr />
<h1 id="메소드-참조">메소드 참조</h1>
<blockquote>
<p><strong>메소드 참조</strong> : 함수형 인터페이스를 람다식이 아닌 일반 메소드를 참조시켜 선언하는 방법</p>
</blockquote>
<p>이미 존재하는 메소드를 마치 오버라이딩하는 것처럼 참조해서 적용하는 방법이다.</p>
<p>메소드 참조를 하기 위해서는 3가지 조건을 만족해야 하는데</p>
<p><strong>함수형 인터페이스</strong>의 <strong>매개변수 타입/개수/반환</strong> 형이랑 <strong>메소드의 매개변수 타입/개수/반환</strong> 형이 같아야 한다.</p>
<hr />
<h3 id="매개변수의-메소드-참조">매개변수의 메소드 참조</h3>
<p>위에서 나온 3가지 조건이 만족하면 가능하다.</p>
<p><strong>메소드 참조 표현식</strong></p>
<pre><code class="language-java">클래스이름::메소드이름 (static)일 경우
참조변수이름::메소드이름 (non-static)일 경우</code></pre>
<hr />
<ul>
<li><strong>활용</strong><pre><code class="language-java">import java.util.function.BiFunction;
</code></pre>
</li>
</ul>
<p>public class Application {
    public static void main(String[] args) {</p>
<pre><code>    /* 수업목표. 기존에 존재하는 메소드를 참조해 람다식을 적용할 수 있다. */
    BiFunction&lt;String, String, Boolean&gt; biFunction;

    String str1 = &quot;METHOD&quot;;
    String str2 = &quot;METHOD&quot;;

    boolean result = false;

    biFunction = String::equals;                // 1. 기존에 있는 메소드를 참조한 것</code></pre><p>//        biFunction = (x, y) -&gt; x.equals(y);        // 2. 직접 람다식을 활용한 것</p>
<pre><code>    result = biFunction.apply(str1, str2);      // str1.equals(str2);

    System.out.println(&quot;result = &quot; + result);
}</code></pre><p>}</p>
<pre><code>
둘다 실행해보면 알겠지만 1, 2번 둘다 동일하다.

**실행결과**
![](https://velog.velcdn.com/images/jojehuni_9759/post/1f74f233-c74d-481d-8ecb-37dd44bda84e/image.png)


---
### 생성자 참조
생성자는 `new`로 생성하기 때문에 참조 표현식이 조금 다르긴 하지만 가능하다.

**메소드 참조 표현식**
```java
클래스이름::new</code></pre><hr />
<ul>
<li><p><strong>활용</strong></p>
</li>
<li><p><em>Member 클래스*</em></p>
<pre><code class="language-java">public class Member {
   private String memId;

   public Member(String memId) {
       this.memId = memId;
   }

   public void setMemId(String memId) {
       this.memId = memId;
   }

   @Override
   public String toString() {
       return &quot;Member{&quot; +
               &quot;memId='&quot; + memId + '\'' +
               '}';
   }
}</code></pre>
</li>
</ul>
<p>생성자 메소드 참조를 할 겸 Member 클래스를 만들어봤다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.function.Function;

public class Application {
    public static void main(String[] args) {

        /* 수업목표. 기존에 존재하는 생성자를 참조한 람다식을 활용할 수 있다. */
//        Function&lt;String, Member&gt; constMember = (str) -&gt; {return new Member(str);};
        Function&lt;String, Member&gt; constMember = Member::new;

        Member member1 = constMember.apply(&quot;Lambda A&quot;);
        System.out.println(&quot;member1 = &quot; + member1);

        Member member2 = constMember.apply(&quot;Lambda B&quot;);
        System.out.println(&quot;member2 = &quot; + member2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/937e8f2f-b7f6-41d4-8138-baa5bd379668/image.png" /></p>