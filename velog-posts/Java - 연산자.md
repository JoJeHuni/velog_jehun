<h1 id="연산자">연산자</h1>
<blockquote>
<p>연산 : <strong>프로그래밍 중 데이터를 처리하여 결과를 만드는 것</strong>
연산자 : 그 연산 과정에서 사용되는 기호 또는 부호
피연산자 : 연산되는 데이터</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8ee1150e-3056-4704-bba8-0cb4269e2b29/image.png" /></p>
<hr />
<h2 id="연산자-종류">연산자 종류</h2>
<table>
<thead>
<tr>
<th>종류</th>
<th>연산자</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>산술 연산자</td>
<td>+, -, *, /, %</td>
<td>사칙연산 포함 기타 연산자</td>
</tr>
<tr>
<td>대입 연산자</td>
<td>=, +=, -=, *=, /=, %=</td>
<td>= 오른쪽에 있는 값을 왼쪽에 대입하는 연산자</td>
</tr>
<tr>
<td>증감 연산자</td>
<td>++, --</td>
<td>피연산자를 하나만 갖는 단항 연산자</td>
</tr>
<tr>
<td>비교 연산자</td>
<td>&gt;, &lt;, &gt;=, &lt;=, ==, !=</td>
<td>두 피연산자의 상대적인 크기를 비교하는 연산자</td>
</tr>
<tr>
<td>논리 연산자</td>
<td>&amp;&amp;,</td>
<td></td>
</tr>
<tr>
<td>삼항 연산자</td>
<td>? :</td>
<td>피연산자 항목이 3개인 연산자</td>
</tr>
<tr>
<td>비트 연산자</td>
<td>&amp;,</td>
<td>, ^, ~, &lt;&lt;, &gt;&gt;, &gt;&gt;&gt;</td>
</tr>
</tbody></table>
<hr />
<h2 id="산술연산자">산술연산자</h2>
<pre><code class="language-java">        int num1 = 20;
        int num2 = 7;

        System.out.println(&quot;num1 + num2 = &quot; + (num1 + num2));
        System.out.println(&quot;num1 - num2 = &quot; + (num1 - num2));
        System.out.println(&quot;num1 * num2 = &quot; + (num1 * num2));</code></pre>
<pre><code>num1 + num2 = 27
num1 - num2 = 13
num1 * num2 = 140</code></pre><p>num1, num2 를 더할 때 괄호가 없다면 자동 형변환이 되어 이어붙여진다.</p>
<blockquote>
<p>나누기는 소수점이 나올 수도 있어서 실수 자료형이 필요하다.</p>
</blockquote>
<pre><code class="language-java">        double testNum = num1 / (double)num2;         // 강제 형변환이 필요하다.
        System.out.println(&quot;testNum = &quot; + testNum);</code></pre>
<pre><code>testNum = 2.857142857142857</code></pre><ul>
<li><p>소수점 이하 3자리까지만 나타내보기 (math 사용 X)</p>
<pre><code class="language-java">      double resultNum = (int)(testNum * 1000)/(double)1000; // 1000으로 나눠줄 때 double형이어야지 뒤 소수점이 생긴다.
      System.out.println(&quot;resultNum = &quot; + resultNum);</code></pre>
<pre><code>resultNum = 2.857</code></pre></li>
<li><p>나머지
위에 있던 num1, num2로 나머지 구해보자</p>
<pre><code class="language-java">      System.out.println(&quot;num1 % num2 = &quot; + (num1 % num2));</code></pre>
<pre><code>num1 % num2 = 6</code></pre><h3 id="산술연산자의-종류">산술연산자의 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>+</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 더함</td>
</tr>
<tr>
<td>-</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 뺌</td>
</tr>
<tr>
<td>*</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 곱함</td>
</tr>
<tr>
<td>/</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 나눔</td>
</tr>
<tr>
<td>%</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 나눈 나머지를 반환</td>
</tr>
</tbody></table>
</li>
</ul>
<hr />
<h2 id="대입연산자">대입연산자</h2>
<blockquote>
<p>대입 연산자 : 변수에 값을 대입할 때 사용하는 이항 연산자</p>
</blockquote>
<ul>
<li>대입연산자</li>
<li>산술 복합 대입 연산자</li>
</ul>
<h3 id="대입연산자-종류">대입연산자 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>=</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 대입함</td>
</tr>
<tr>
<td>+=</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 더한 후, 그 결괏값을 왼쪽의 피연산자에 대입함</td>
</tr>
<tr>
<td>-=</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 뺀 후, 그 결괏값을 왼쪽의 피연산자에 대입함</td>
</tr>
<tr>
<td>*=</td>
<td>왼쪽의 피연산자에 오른쪽의 피연산자를 곱한 후, 그 결괏값을 왼쪽의 피연산자에 대입함</td>
</tr>
<tr>
<td>/=</td>
<td>왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후, 그 결괏값을 왼쪽의 피연산자에 대입함</td>
</tr>
<tr>
<td>%=</td>
<td>왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후, 그 나머지를 왼쪽의 피연산자에 대입함</td>
</tr>
</tbody></table>
<pre><code class="language-java">        int num = 12;
        System.out.println(&quot;num : &quot; +num);        // 12

        //3증가시
        num = num + 3;                            // 15 대입연산자의 오른쪽에는 값, 왼쪽에는 공간의 의미이다.
        System.out.println(&quot;num : &quot; + num);

        num += 3;                                  // 18 num = num + 3; 과 같은 의미임
        System.out.println(&quot;num : &quot; + num);

        num -= 5;                                  // 13 num = num - 5;
        System.out.println(&quot;num : &quot; + num);

        num *= 2;                                  // 26 num값 2배 증가
        System.out.println(&quot;num : &quot; + num);

        num /= 2;                                  // 13 num값 2배 감소
        System.out.println(&quot;num : &quot; + num);

        /* 주의! 산술 복합 대입연산자의 작성 순서에 주의해야 한다. */
        /* 산술 대입 연산자가 아닌 '-5'를 num에 대입한 것이다. */
        num =- 5;
        System.out.println(&quot;num : &quot; + num);        // - 5</code></pre>
<pre><code>num : 12
num : 15
num : 18
num : 13
num : 26
num : 13
num : -5</code></pre><hr />
<h2 id="증감연산자">증감연산자</h2>
<blockquote>
<p>증감 연산자 : 피연산자를 1 증가 or 감소시킬 때 사용하는 연산자</p>
</blockquote>
<ul>
<li>피연산자가 하나이다.</li>
</ul>
<h3 id="증감연산자-종류">증감연산자 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>++var</td>
<td>피연산자의 값을 먼저 1을 증가시킨 후 다른 연산을 진행함</td>
</tr>
<tr>
<td>var++</td>
<td>다른 연산을 먼저 진행하고 난 뒤 마지막에 피연산자의 값을 1 증가시킴</td>
</tr>
<tr>
<td>--var</td>
<td>피연산자의 값을 먼저 1 감소 시킨 후 다른 연산을 진행함</td>
</tr>
<tr>
<td>var--</td>
<td>다른 연산을 먼저 진행하고 난 뒤 마지막에 피연산자의 값을 1 감소시킴</td>
</tr>
</tbody></table>
<hr />
<h2 id="비교-연산자">비교 연산자</h2>
<blockquote>
<p>비교 연산자 : 피연산자 사이에서 상대적인 크기를 판단해 True or False 반환
True or False를 반환하는 연산자는 삼항연산자의 조건식이나 조건절에 많이 사용된다.</p>
</blockquote>
<h3 id="비교-연산자-종류">비교 연산자 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>==</td>
<td>왼쪽의 피연산자와 오른쪽의 피연산자가 같으면 true 다르면 false를 반환</td>
</tr>
<tr>
<td>!=</td>
<td>왼쪽의 피연산자와 오른쪽의 피연산자가 다르면 true 같으면 false를 반환</td>
</tr>
<tr>
<td>&gt;</td>
<td>왼쪽의 피연산자가 오른쪽의 피연산자보다 크면 true 아니면 false를 반환</td>
</tr>
<tr>
<td>&gt;=</td>
<td>왼쪽의 피연산자가 오른쪽의 피연산자보다 크거나 같으면 true 아니면 false를 반환</td>
</tr>
<tr>
<td>&lt;</td>
<td>왼쪽의 피연산자가 오른쪽의 피연산자보다 작으면 true 아니면 false를 반환</td>
</tr>
<tr>
<td>&lt;=</td>
<td>왼쪽의 피연산자가 오른쪽의 피연산자보다 작거나 같으면 true 아니면 false를 반환</td>
</tr>
</tbody></table>
<p>정수형과 실수형의 비교 연산자는 크게 어려움이 없을 것이고, <strong>문자형에 대해서 다뤄보자.</strong></p>
<p>아스키 코드에서 나타내는 'A', 'a'는 각각 65, 97로 나타내게 돼 숫자로 나타내는 것과 같다.</p>
<pre><code class="language-java">        char ch1 = 'a';
        char ch2 = 'A';

        System.out.println(&quot;ch1과 ch2가 같은지 비교 : &quot; + (ch1 == ch2));
        System.out.println(&quot;ch1과 ch2가 같지 않은지 비교 : &quot; + (ch1 != ch2));
        System.out.println(&quot;ch1이 ch2보다 큰지 비교 : &quot; + (ch1 &gt; ch2));
        System.out.println(&quot;ch1이 ch2보다 크거나 같은지 비교 : &quot; + (ch1 &gt;= ch2));
        System.out.println(&quot;ch1이 ch2보다 작은지 비교 : &quot; + (ch1 &lt; ch2));
        System.out.println(&quot;ch1이 ch2보다 작은거나 같은지 비교 : &quot; + (ch1 &lt;= ch2));</code></pre>
<pre><code>ch1과 ch2가 같은지 비교 : false
ch1과 ch2가 같지 않은지 비교 : true
ch1이 ch2보다 큰지 비교 : true
ch1이 ch2보다 크거나 같은지 비교 : true
ch1이 ch2보다 작은지 비교 : false
ch1이 ch2보다 작은거나 같은지 비교 : false</code></pre><blockquote>
<p>논리형은 같은지 아닌지만 비교가 가능하다.</p>
</blockquote>
<hr />
<h2 id="논리-연산자">논리 연산자</h2>
<ul>
<li>논리 연결 연산자</li>
<li>논리 부정 연산자</li>
</ul>
<blockquote>
<p>논리 연결 연산자 : 2개의 피연산자를 가지는 이항 연산자, 연산자의 결합 방향은 왼쪽 -&gt; 오른쪽이며 2개의 논리식을 판단해 참, 거짓을 판단한다.</p>
</blockquote>
<blockquote>
<p>논리 부정 연산자 : 피연산자가 하나인 단항 연산자, 피연산자의 결합 방향은 왼쪽 -&gt; 오른쪽</p>
</blockquote>
<h3 id="논리-연산자의-종류">논리 연산자의 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>&amp;&amp;</td>
<td>두 개의 논리식 모두 참 일 경우 참을 반환, 둘 중 한 개라도 거짓인 경우 거짓을 반환하는 연산자이다.(AND)</td>
</tr>
<tr>
<td></td>
<td></td>
</tr>
<tr>
<td>!</td>
<td>논리식의 결과가 참이면 거짓을, 거짓이면 참을 반환한다.(NOT)</td>
</tr>
</tbody></table>
<blockquote>
<p><strong>AND 연산과 OR 연산의 특징</strong>
논리식 &amp;&amp; 논리식 : 앞의 결과가 false이면 뒤를 실행 안함
논리식 || 논리식 : 앞의 결과가 true이면 뒤를 실행 안함</p>
</blockquote>
<pre><code class="language-java">        System.out.println(true &amp;&amp; true);        // true
        System.out.println(true &amp;&amp; false);        // false
        System.out.println(false &amp;&amp; true);        // false
        System.out.println(false &amp;&amp; false);        // false

        System.out.println(true || true);        // true
        System.out.println(true || false);        // true
        System.out.println(false || true);        // true
        System.out.println(false || false);        // false

        System.out.println(!true);                // false
        System.out.println(!false);                // true</code></pre>
<p>위와 같이 활용하는건 아니고, 논리식과 연산자를 함께 사용한다.</p>
<pre><code class="language-java">        int num1 = 10;
        int num2 = 20;
        int num3 = 30;
        int num4 = 40;

        System.out.println(num1 &lt; num2 &amp;&amp; num3 &lt; num4);     // true &amp;&amp; true -&gt; true
        System.out.println(num1 &lt; num2 &amp;&amp; num3 &gt; num4);     // true &amp;&amp; false -&gt; false
        System.out.println(num1 &gt; num2 &amp;&amp; num3 &lt; num4);     // false &amp;&amp; true -&gt; false
        System.out.println(num1 &gt; num2 &amp;&amp; num3 &gt; num4);     // false &amp;&amp; false -&gt; false</code></pre>
<hr />
<h2 id="삼항-연산자">삼항 연산자</h2>
<blockquote>
<p>(<code>조건식</code>) ? <code>참일 때 사용할 값1</code> : <code>거짓일 때 사용할 값2</code>
위와 같은 형태로 반드시 결과가 true or false가 나오게끔 해야 한다.</p>
</blockquote>
<pre><code class="language-java">        int num1 = 10;
        int num2 = 20;
        String result1 = (num1 &gt; 0) ? &quot;양수다.&quot; : &quot;양수가 아니다.&quot;;
        String result2 = (num2 &gt; 0) ? &quot;양수다.&quot; : &quot;양수가 아니다.&quot;;

        System.out.println(&quot;result1 = &quot; + result1);
        System.out.println(&quot;result2 = &quot; + result2);</code></pre>
<p>num1 &gt; 0 이면 &quot;양수다.&quot;, num1 &lt;]= 0 이면 &quot;양수가 아니다.&quot;와 같이 참일 때 사용할 값, 거짓일 때 사용할 값을 나눠서 조건식을 세울 수 있다.</p>
<pre><code>result1 = 양수다.
result2 = 양수다.</code></pre><blockquote>
<p>중첩 또한 가능하지만 권장하지는 않는다.</p>
</blockquote>
<pre><code class="language-java">        int num3 = 0;
        int num4 = 1;
        int num5 = -1;

        String result3 = (num3 &gt; 0) ? &quot;양수다&quot; : (num3 == 0) ? &quot;0이다.&quot; : &quot;음수다.&quot;;
        String result4 = (num4 &gt; 0) ? &quot;양수다&quot; : (num4 == 0) ? &quot;0이다.&quot; : &quot;음수다.&quot;;
        String result5 = (num5 &gt; 0) ? &quot;양수다&quot; : (num5 == 0) ? &quot;0이다.&quot; : &quot;음수다.&quot;;

        System.out.println(&quot;result3 = &quot; + result3);
        System.out.println(&quot;result4 = &quot; + result4);
        System.out.println(&quot;result5 = &quot; + result5);</code></pre>