<blockquote>
<p>제네릭 : 데이터의 타입을 일반화한다 &lt;- 이런 의미를 가진다.
제네릭스를 활용하는 제네릭 클래스는 제네릭 타입 (T, E, K, V)을 활용해 하나의 클래스로 해당 제네릭 타입에 변화를 줘서 제네릭 클래스의 인스턴스를 다양한 타입의 인스턴스로 활용 가능하다.</p>
</blockquote>
<p><strong>MyGenericTest</strong></p>
<pre><code class="language-java">public class MyGenericTest {

    private Object value;

    public Object getValue() {
        return value;
    }

    public void setValue(Object value) {
        this.value = value;
    }
}</code></pre>
<p>제네릭 사용 이전의 클래스를 활용해서 나타내면</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section01.generic;

public class Application {
    public static void main(String[] args) {
        MyGenericTest myGenericTest = new MyGenericTest();

        myGenericTest.setValue(&quot;Hello World&quot;);
        myGenericTest.setValue(1);
        myGenericTest.setValue(3.14);

        System.out.println(myGenericTest.getValue());
        Object result = myGenericTest.getValue();
        double primitiveResult = (Double)result;
        // 여기서 런타임 에러가 발생한다. result는 double형으로 다운캐스팅해야하지만, String형으로 다운캐스팅도 컴파일 시점에 가능해지는데
        // 이는 런타임 에러를 발생시킨다. -&gt; Object를 활용하는 것은 자료형이 안전하지 않다.
    }
}</code></pre>
<p>Application 주석에서 적혀 있는 것처럼 이런 문제를 해결하기 위해 제네릭를 사용할 수 있다.</p>
<hr />
<h2 id="활용">활용</h2>
<p>바로 활용해보자.</p>
<p><strong>GenericTest</strong></p>
<pre><code class="language-java">/* 다이아몬드 연산자에 들어갈 수 있는 4가지 타입
*   1. E - Element
*   2. T - Type
*   3. K - Key
*   4. V - Value
* */
public class GenericTest&lt;T&gt; {

    private T value;

    public T getValue() {
        return value;
    }

    public void setValue(T value) {
        this.value = value;
    }
}
</code></pre>
<p><code>T</code> &lt;- 제네릭 타입으로 여기에는 변화를 줄 수 있다. <code>Integer</code>, <code>String</code> 등등</p>
<p>Application</p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        GenericTest&lt;Integer&gt; genericTest = new GenericTest&lt;&gt;();
    }
}
</code></pre>
<p>위의 코드를 작성하면 이제 메소드를 불러올 때 자료형이 바뀐 것을 볼 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/826e1774-d424-4f30-a18c-b9e14f77883a/image.png" /></p>
<p>이전에는 Object 형이어서 여러 가지 다운캐스팅이 가능해서 문제가 생겼지만, 제네릭을 사용하면 이제 다른 자료형을 사용하려고 하면 오류가 생긴다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/891050c0-de28-4808-b0c1-53c52ed7016e/image.png" /></p>
<p>이제 컴파일 시점에서도 오류를 알려줘서 안전하게 사용할 수 있다.</p>
<hr />
<h2 id="사용-이유">사용 이유</h2>
<p>활용을 해봤으니 사용 이유에 대해서도 알아보자.</p>
<ul>
<li><strong>구현의 편의성</strong> : 하나의 클래스만 작성해도 여러 타입의 필드 값을 가진 클래스로 변형해 다룰 수 있다.</li>
<li><strong>자료형의 안전성</strong> : 자료형의 타입을 명확히 알고 있어 제네릭 클래스에서도 반환형을 알고 사용한다. (컴파일 시점에서 오류가 생기거나 하면 바로 알 수 있다.)</li>
</ul>
<hr />
<h2 id="와일드카드">와일드카드</h2>
<blockquote>
<p><strong>와일드카드</strong> : 제네릭 클래스의 인스턴스를 유연하게 활용하기 위한 문법
제네릭 클래스 타입의 객체를 메소드의 매개변수로 받을 때 타입 변수를 제한할 수 있다.</p>
<p><code>&lt;?&gt;</code> : 모든 타입을 허용하는 와일드 카드
<code>&lt;? extends T&gt;</code> : T 타입 또는 T의 하위 타입을 허용하는 와일드 카드
<code>&lt;? super T&gt;</code> : T 타입 또는 T의 상위 타입을 허용하는 와일드 카드</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fb659c59-f633-4b26-a069-a099254034e9/image.png" /></p>
<p><strong>RabbitFarm</strong></p>
<pre><code class="language-java">public class RabbitFarm&lt;T extends Rabbit&gt; {
    private T animal;

    public RabbitFarm() {

    }

    public RabbitFarm(T animal) {
        this.animal = animal;
    }

    public T getAnimal() {
        return animal;
    }

    public void setAnimal(T animal) {
        this.animal = animal;
    }
}
</code></pre>
<p><strong>Rabbit, Bunny, DrunkenBunny</strong></p>
<pre><code class="language-java">public class Rabbit extends Mammal {
    public void cry() {
        System.out.println(&quot;끽끽 (토끼 울음소리)&quot;);
    }
}

public class Bunny extends Rabbit {
    @Override
    public void cry() {
        System.out.println(&quot;바니바니 X 2 당근당근&quot;);
    }
}

public class DrunkenBunny extends Rabbit {
    @Override
    public void cry() {
        System.out.println(&quot;바니 쨔스!&quot;);
    }
}</code></pre>
<p><strong>WildCardFarm</strong></p>
<pre><code class="language-java">// 제네릭 타입을 활용하는 기능을 포함한 클래스
public class WildCardFarm {

    // 어떤 타입의 RabbitFarm이 와도 상관 없다.
    // RabbitFarm은 애초에 Rabbit을 상속받아 객체를 만들 수 있는 범위가 정해져 있다. 
    public void anyType(RabbitFarm&lt;?&gt; farm) {
        farm.getAnimal().cry();
    }

    // RabbitFarm 중에서도 Bunny 또는 하위 타입이 있는 RabbitFarm만 가능하다.
    public void extendsType(RabbitFarm&lt;? extends Bunny&gt; farm) {
        farm.getAnimal().cry();
    }

    // RabbitFarm 중에서도 Bunny 또는 상위 타입이 있는 RabbitFarm만 가능하다.
    // 즉, Bunny, Rabbit만 가능하다.
    public void superType(RabbitFarm&lt;? super Bunny&gt; farm) {
        farm.getAnimal().cry();
    }
}</code></pre>
<p><code>anyType()</code>도 <code>RabbitFarm</code> 에 있는 것들 중 하나를 가져오고
<code>extends</code>, <code>super</code> 부분을 보면 <code>Bunny</code>라고 되어 있다. 그래서 <code>Application</code>도 보면 주석처리 된 부분은 오류가 나는 것을 알 수 있을 것이다.</p>
<blockquote>
<p>Mammal, Reptile, Snake 는 아직 X</p>
</blockquote>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application2 {
    public static void main(String[] args) {
        WildCardFarm wildCardFarm = new WildCardFarm();
        wildCardFarm.anyType(new RabbitFarm&lt;Rabbit&gt;(new Rabbit()));
        wildCardFarm.anyType(new RabbitFarm&lt;Bunny&gt;(new Bunny()));
        wildCardFarm.anyType(new RabbitFarm&lt;DrunkenBunny&gt;(new DrunkenBunny()));

//        wildCardFarm.extendsType(new RabbitFarm&lt;Rabbit&gt;(new Rabbit()));
        wildCardFarm.extendsType(new RabbitFarm&lt;Bunny&gt;(new Bunny()));
//        wildCardFarm.extendsType(new RabbitFarm&lt;DrunkenBunny&gt;(new DrunkenBunny()));

        wildCardFarm.superType(new RabbitFarm&lt;Rabbit&gt;(new Rabbit()));
        wildCardFarm.superType(new RabbitFarm&lt;Bunny&gt;(new Bunny()));
//        wildCardFarm.superType(new RabbitFarm&lt;DrunkenBunny&gt;(new DrunkenBunny()));

    }
}</code></pre>