<p>상속과 관련돼 있는 다형성에 대해 다뤄보자.</p>
<h1 id="다형성">다형성</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8fa8e888-50ba-49ec-af6e-0ba20742ef20/image.png" /></p>
<p>간단한 트리 구조로</p>
<ol>
<li>A에 있는 자료형을 B가 물려 받고, 그럼 D, E는 A, B의 자료형을 물려받을 수 있다.</li>
<li>A에 있는 자료형을 C가 물려 받고, 그럼 F는 A, C의 자료형을 물려받을 수 있다.</li>
</ol>
<h2 id="활용">활용</h2>
<p><strong>Animal</strong></p>
<pre><code class="language-java">public class Animal {
    public void eat() {
        System.out.println(&quot;동물이 먹이를 먹습니다.&quot;);
    }

    public void run() {
        System.out.println(&quot;동물이 달립니다.&quot;);
    }

    public void cry() {
        System.out.println(&quot;동물이 울음소리를 냅니다.&quot;);
    }

}</code></pre>
<p><strong>Rabbit</strong></p>
<pre><code class="language-java">public class Rabbit extends Animal {
    @Override
    public void eat() {
        System.out.println(&quot;토끼가 풀을 뜯어먹고 있습니다.&quot;);
    }

    @Override
    public void run() {
        System.out.println(&quot;토끼가 달립니다 껑충&quot;);
    }

    @Override
    public void cry() {
        System.out.println(&quot;토끼가 울음소리를 냅니다. 끼익~~&quot;);
    }

    public void jump() {
        System.out.println(&quot;토끼가 점프를 뜁니다!&quot;);
    }
}</code></pre>
<p><strong>Tiger</strong></p>
<pre><code class="language-java">public class Tiger extends Animal {
    @Override
    public void eat() {
        System.out.println(&quot;호랑이가 고기를 뜯어먹습니다.&quot;);
    }

    @Override
    public void run() {
        System.out.println(&quot;호랑이는 웬만에선 달리지 않습니다.&quot;);
    }

    @Override
    public void cry() {
        System.out.println(&quot;호랑이가 울부짖습니다.&quot;);
    }

    public void bite() {
        System.out.println(&quot;호랑이가 물어뜯습니다.&quot;);
    }
}</code></pre>
<blockquote>
<p>Tiger, Rabbit은 Animal을 상속해서 Animal의 자료형을 물려받을 수 있지만, Animal은 Tiger, Rabbit의 존재를 모른다.</p>
</blockquote>
<p><strong>Application</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section01.polymorphism;

public class Application {
    public static void main(String[] args) {
        /* 수업목표. 다형성과 타입 형변환에 대해 이해할 수 있다. */
        Animal animal = new Animal();
        animal.eat();
        animal.run();
        animal.cry();

        System.out.println();

        Tiger tiger = new Tiger();
        tiger.eat();
        tiger.run();
        tiger.cry();
        tiger.bite();

        System.out.println();

        Rabbit rabbit = new Rabbit();
        rabbit.eat();
        rabbit.run();
        rabbit.cry();
        rabbit.jump();

        Animal an1 = new Animal();  // 다형성 X
        Animal an2 = new Tiger();   // 다형성 O
        Animal an3 = new Rabbit();  // 다형성 O
    }
}</code></pre>
<p>아래 2줄을 보면 <strong>다형성 적용</strong>을 한 것을 알 수 있다.</p>
<ul>
<li><strong>부모 타입으로 자식 인스턴스의 주소값 저장</strong></li>
</ul>
<p>아래 2가지는 다형성을 적용한 것이 아니다. 애초에 에러가 뜬다.</p>
<pre><code class="language-java">Tiger tiger = new Animal(); // 다형성 X
Rabbit rabbit = new Animal(); // 다형성 X</code></pre>
<blockquote>
<p><code>Animal</code>은 <code>Tiger</code>나 <code>Rabbit</code>이 아니기 때문에 안 된다. (반대는 가능하니까 O)</p>
</blockquote>
<hr />
<h2 id="동적-바인딩">동적 바인딩</h2>
<p><strong>Application</strong> 위 코드 아래에 추가해보자.</p>
<pre><code class="language-java">        System.out.println(&quot;===== 동적 바인딩 확인하기 =====&quot;);
        an1.cry();
        an2.cry();
        an3.cry();</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d6b88d92-6a1e-4e12-b2f2-1e01d81b54b4/image.png" /></p>
<p><code>cry()</code>를 들어가보면 셋다 <code>Animal</code>의 <code>cry</code> 메소드를 쳐다보고 있다.</p>
<p>컴파일 시점 -&gt; 정적 바인딩을 통해 <code>Animal</code> 타입의 <code>cry()</code> 메소드로 생각한다.
실행하면 (런타임 시점) 객체가 생성된다. 그래서 <code>new Tiger()</code>, <code>new Rabbit()</code> 들로 인해 어떤 타입들이 구체화 돼 있는지 알 수 있어서 사진과 같이 출력되는 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e4baef1e-60c5-414f-968b-d30e8e85007f/image.png" /></p>
<blockquote>
<p>동적바인딩의 조건</p>
<ol>
<li>상속</li>
<li>오버라이딩</li>
</ol>
</blockquote>
<blockquote>
<p>동적 바인딩 : 컴파일 당시에는 해당 타입의 메소드와 연결돼 있다가
런타임 당시 실제 객체가 가진 오버라이딩 된 메소드로 바인딩이 바뀌어 동작하는 것을 의미한다.</p>
</blockquote>
<hr />
<h3 id="컴파일-시점-런타임-시점-시-주의사항">컴파일 시점, 런타임 시점 시 주의사항</h3>
<blockquote>
<p><strong>부모타입을 자식타입으로 강제 형변환 하는 것은 가능하다 (조심해야 함)</strong></p>
</blockquote>
<p><strong>Application</strong></p>
<pre><code class="language-java">         ((Tiger) an3).cry(); 
         // 컴파일 시점에서 에러 발생 X지만, 런타임 시점에 에러 발생</code></pre>
<p>컴퓨터는 an3 를 <code>Animal an3 = new Rabbit()</code> 으로 적어놔도 컴파일 시점에서 an3는 현재 <code>Animal</code> 자료형인 것이다. </p>
<p>그래서 위 코드에서는 an3는 <code>Animal</code> 이기에 강제 형변환이 가능하지만, <strong>런타임 시 an3는 <code>Rabbit</code> 타입인걸 알게되므로 런타임 에러가 생기는 것이다.</strong></p>
<hr />
<h3 id="instanceof-연산자">instanceof 연산자</h3>
<pre><code class="language-java">        if (an3 instanceof Tiger) {
            ((Tiger) an3).cry();
        }

        if (an3 instanceof Animal) {
            System.out.println(&quot;Animal은 맞지&quot;);
        }</code></pre>
<p><code>an3</code>는 위에서 <code>Rabbit</code>으로 만들었었는데 둘다 가능할까?</p>
<blockquote>
<p>아래 조건문의 실행부분만 가능하다.</p>
</blockquote>
<blockquote>
<p><code>instanceof</code> : 해당 객체의 타입을 런타임 시점에 확인하기 위한 연산자</p>
</blockquote>
<hr />
<h2 id="업캐스팅-다운캐스팅">업캐스팅, 다운캐스팅</h2>
<pre><code class="language-java">Animal an4 = new Tiger();       // 다형성을 적용, 자동형변환(auto up-casting), 묵시적 형변환
Rabbit rabbit2 = (Rabbit)an3;   // 다형성 적용 X, 강제형변환(down-casting), 명시적 형변환</code></pre>
<p>Tiger가 상위 타입 형변환으로 Animal로 클래스 형변환이 되는 것과
an3는 Animal인데 Rabbit으로 down-casting 된다.</p>