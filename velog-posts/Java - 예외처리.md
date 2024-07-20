<h1 id="예외처리">예외처리</h1>
<blockquote>
<p>예외처리</p>
<ul>
<li><p>오류 : 시스템 상에서 프로그램에 심각한 문제가 발생해 실행 중인 프로세스가 종료되는 것
  (개발자가 미리 예측하거나 코드로 처리하는 것이 불가능한 경우)</p>
</li>
<li><p>예외 : 개발자가 미리 예측하고 처리할 수 있는 미약한 오류</p>
</li>
</ul>
</blockquote>
<hr />
<h2 id="사용-이유">사용 이유</h2>
<ol>
<li>미리 예측하고 컨트롤 할 수 있는 예외를 처리함으로써 프로그램이 예상치 못한 상황에 봉착하지 않도록 코드의 안정성과 신뢰성을 높여 이를 미연에 방지하거나 의도한 방향으로 컨트롤 할 수 있다.</li>
<li>개발 시에도 디버깅을 용이하게 해서 예외가 발생한 원인과 위치도 쉽게 파악할 수 있어 용이하다.</li>
</ol>
<hr />
<h2 id="예외-클래스의-종류">예외 클래스의 종류</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8bb4a661-b8a9-4d4c-bf8c-9c1965462e06/image.png" /></p>
<p>(Error는 번외니까 신경 안 써도 된다.)</p>
<ul>
<li>오류와 예외는 모두 <code>Throwable</code>을 상속 받는다.</li>
<li>예외의 최상위 클래스는 <code>Exception</code> 클래스이다.</li>
<li><strong>Unchecked Exception</strong> 계열은 <strong>기본적 이미 처리</strong>되어 있고 실행 중인 <strong>프로그램이 종료</strong>되게 작성되어 있다.</li>
<li><strong>Checked Exception</strong> 계열은 <strong>반드시 예외 처리를 해야</strong> 하고 하지 않으면 <strong>컴파일 에러</strong>가 발생한다.</li>
<li><strong>RuntimeException</strong>타입의 예외들은 런타임 시점에 해당 예외 클래스 타입의 Exception이 발생한다.</li>
</ul>
<h3 id="예외처리-문법-활용">예외처리 문법 활용</h3>
<p>예외처리로 if 조건문에 대해 생각할 수도 있지만 if-else는 보통 코드의 흐름을 위해 쓴다고 생각하고 예외처리는 아래 2가지를 사용하자.</p>
<blockquote>
<ol>
<li>throws를 통한 위임</li>
<li>try-catch를 통한 처리</li>
</ol>
</blockquote>
<hr />
<ol>
<li><strong>throws를 통한 위임</strong><blockquote>
<p>Exception이 발생하는 메소드(or 생성자)를 호출한 사우이 메소드에게 처리를 위임하는 방식</p>
</blockquote>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9237244e-7dd0-4610-b6bb-b5e01a2ac6d7/image.png" /></p>
<p>예제)
<strong>ExceptionTest</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section01.exception;

public class ExceptionTest {

    public void checkEnoughMoney (int price, int money) throws Exception {
        System.out.println(&quot;가지고 계신 돈은 &quot; + money + &quot;원입니다.&quot;);

        if (money &gt;= price) {
            System.out.println(price + &quot;원 상품을 구입하기 위한 금액이 충분합니다!&quot;);
            return;
        }

//        System.out.println(&quot;호주머니 상황이 ...&quot;);
        throw new Exception(&quot;호주머니 상황 이슈 ...&quot;);
    }
}</code></pre>
<ol start="2">
<li><strong>try-catch</strong><blockquote>
<p>발생한 Exception을 직접 처리하는 방식이다.</p>
</blockquote>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5810436f-86df-4be7-b62c-28482db3c85e/image.png" /></p>
<p>위 예제와 이어진 예제)
<strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) throws Exception {

        ExceptionTest exceptionTest = new ExceptionTest();

        try {
            /* 개발자가 처리할 예외가 발생할 수 있는 적당한 범위를 try 블럭으로 감싼다. */
            exceptionTest.checkEnoughMoney(10000, 50000);
            exceptionTest.checkEnoughMoney(50000, 10000);
        } catch (Exception e) {
            /* 개발자가 원하는 방식대로 try 블럭에서 발생한 예외 타입 객체를 활용해서 처리할 수 있다. */
            // try 블럭에서 예외가 발생하지 않으면 실행되지 않는 블럭이다.
            System.out.println(&quot;뭔가 예외 발생.&quot;);
            System.out.println(&quot;그 예외는 &quot; + e.getMessage() + &quot;!!&quot;);

            e.printStackTrace();
            System.exit(0); // 원하는 시점에 종료 가능
        }
    }
}</code></pre>
<ul>
<li><p><strong>try 블럭</strong>
예외(Exception)가 발생할 가능성이 있는 코드를 포함하여 작성하는 블럭</p>
</li>
<li><p><strong>catch블럭</strong>
try 블럭에서 예외 발생 시 해당 예외 타입(Exception 클래스 타입)에 대한 처리를 기술하는 블럭이다.
여러 개의 catch블럭을 이어서 사용할 수 있으며 상위 타입의 예외를 처리하는 catch 블럭이 아래 쪽에 위치해야 한다.</p>
</li>
<li><p><strong>finally 블럭</strong></p>
</li>
<li><p><em>예외 발생 여부와 상관 없이 꼭 실행*</em>되어 처리해야 할 코드가 있으면 작성하는 블럭이다. 주로 <code>java.io</code>나 <code>java.sql</code> 패키지의 메소드 처리 시 자원 반납을 위해 사용한다.</p>
</li>
</ul>
<blockquote>
<p>finally 블럭을 만든 이유 : try 당시에 어떤 리소스를 쓰는데, 그 리소스가 예외가 발생했음에도 계속 동작하는 경우를 처리하기 위해서 만들었다.</p>
</blockquote>
<hr />
<h3 id="runtimeexception-하위-클래스-종류">RuntimeException 하위 클래스 종류</h3>
<ol>
<li><code>ArithmeticException</code> : 0으로 나누는 경우에 발생한다.<pre><code class="language-java">int dividend = 3;
System.out.println(dividend / 0);</code></pre>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/dc968b86-9855-466f-b776-7229197b8ad9/image.png" /></li>
</ol>
<ol start="2">
<li><p><code>ArrayIndexOutOfBoundsException</code> : 배열의 index 범위를 넘어서 참조하는 경우 발생</p>
<pre><code class="language-java">int[] intArr = new int[0];
System.out.println(intArr[1]);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/366c0b92-c8e6-4c59-a00f-caf889b2d051/image.png" /></p>
</li>
<li><p><code>NullPointerException</code> : 인스턴스가 참조되지 않은 상태(Null) 로 인스턴스에 접근하면 발생</p>
<pre><code class="language-java">int[] intArr = null;
System.out.println(intArr[0]);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2ff67bfe-8554-47ef-bed0-ea3fd07405a0/image.png" /></p>
</li>
</ol>
<p>4.<code>ClassCastException</code> : 형변환(Cast연산자 사용) 시 자료형에 문제가 있을 때 발생</p>
<pre><code class="language-java">Object obj = new String(&quot;hello&quot;);
int num = (Integer)obj;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6a2e6c93-efef-42d7-b00e-0579fd0312a8/image.png" /></p>
<ol start="5">
<li><code>NegativeArraySizeException</code> : 배열 크기를 음수로 지정한 경우 발생<pre><code class="language-java">int[] intArr = new int[-1];</code></pre>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/49e388eb-e23e-4758-93d2-963e5ad22e72/image.png" /></li>
</ol>
<hr />
<h3 id="오버라이딩-시-예외-발생-가능-범위">오버라이딩 시 예외 발생 가능 범위</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6e44251d-c7b3-44da-961a-7c7a7a721149/image.png" />
Child1, Child2 둘다 Parent와 상속관계에 있으며 오버라이딩하는 메소드는 부모 클래스의 원본 메소드보다 더 상위 타입의 예외를 발생시키면 안 된다.</p>