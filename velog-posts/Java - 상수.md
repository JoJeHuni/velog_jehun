<h1 id="상수">상수</h1>
<blockquote>
<p>변수와 동일하게 데이터를 저장할 수 있는 공간</p>
</blockquote>
<blockquote>
<p><strong>다른점</strong>
한 번 메모리에 저장된 데이터를 변경할 수 없다.</p>
</blockquote>
<h2 id="상수-사용-목적">상수 사용 목적</h2>
<ol>
<li><p>변경되지 않는 고정된 값을 저장할 목적으로 사용</p>
</li>
<li><p>초기화 이후 값 대입 시 컴파일 에러를 발생시켜 값이 수정되지 못하도록 한다.</p>
</li>
</ol>
<h2 id="상수의-사용">상수의 사용</h2>
<pre><code>final 키워드 사용</code></pre><h3 id="상수의-선언-및-초기화">상수의 선언 및 초기화</h3>
<pre><code class="language-java">/* 1. 상수 선언 
* 상수 선언 시 자료형 앞에 final 키워드를 붙인다. */
        /* 1. 상수 선언
         * 상수 선언 시 자료형 앞에 final 키워드를 붙인다. */
        final int AGE;

        /* 2. 초기화 */
        AGE = 20;
        //AGE = 30;        //한 번 초기화 한 이후 값을 재 대입하는 것은 불가능하다.

        /* 3. 필요한 위치에 상수를 호출해서 사용한다. */
        /* 3-1. 출력 구문에서 사용 */
        System.out.println(&quot;AGE의 값 : &quot; + AGE);
        /* 3-2. 필요시 연산식에 호출해서 사용 */
        System.out.println(&quot;AGE의 2배 : &quot; + (AGE * 2));</code></pre>
<blockquote>
<p><strong>기억!</strong>
상수는 한 번 초기화 한 이후 값을 재 대입하는 것은 불가능하다.</p>
</blockquote>
<h3 id="상수의-명명-규칙">상수의 명명 규칙</h3>
<ol>
<li><p><strong>모든 문자는 영문자 대문자 혹은 숫자만 사용한다.</strong></p>
<pre><code class="language-java"> final int AGE1 = 20;
 final int AGE2 = 30;
 final int age3 = 40;        //소문자로 사용은 가능하지만 변수와 구분하기 힘들기 때문에 만들어진 규칙이다.</code></pre>
</li>
<li><p><strong>단어와 단어 연결은 언더스코어( _ )를 사용한다.</strong></p>
<pre><code class="language-java"> final int MAX_AGE = 60;
 final int MIN_AGE = 20;
 final int minAge = 30;        //camel case 사용이 가능하지만 역시 변수와 구분되지 않는다.</code></pre>
</li>
</ol>