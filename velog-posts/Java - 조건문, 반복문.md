<p>조건문, 반복문에 대해 알아보자.</p>
<h1 id="조건문">조건문</h1>
<blockquote>
<p>조건문 : &quot;조건식&quot;을 통해 특정 코드를 실행할지 말지 제어해주는 구문이다.</p>
</blockquote>
<p>특정 조건식의 결과가 참이면 조건문 내부의 코드를 실행한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5ee31d17-34ea-4a8d-82f4-e249b49514e6/image.png" /></p>
<h2 id="조건문-종류">조건문 종류</h2>
<ul>
<li><code>if</code>문<ul>
<li><code>if-else</code>문</li>
<li><code>if-else if</code>문</li>
</ul>
</li>
<li><code>switch</code>문</li>
</ul>
<p>위와 같은 종류들로 나뉜다.</p>
<h3 id="if문-개요">if문 개요</h3>
<p>if문은 간단하게</p>
<pre><code class="language-java">if (조건식) {
    수행문;
}</code></pre>
<p>이런 구조로 이루어져있으며, 조건식이 <code>true</code>면 수행문 코드 부분을 실행한다.</p>
<h3 id="if-else문-개요">if-else문 개요</h3>
<pre><code class="language-java">if (조건식) {
    조건식이 true 일 때 실행되는 수행문;
    ...
} else {
    조건식이 false 일 때 실행되는 수행문;
        ...
}</code></pre>
<h3 id="if-elseif문-개요">if-elseif문 개요</h3>
<pre><code class="language-java">if (조건식1) {
    수행문;
    ...
} else if(조건식2) {
    수행문;
    ...
} else {
    수행문;
    ...
}</code></pre>
<ul>
<li>조건식1이 참이면 <code>if</code>문 실행</li>
<li>조건식1이 거짓 + 조건식2가 참이면 <code>else if</code>문 실행</li>
<li>둘다 거짓이면 <code>else</code>문 실행</li>
</ul>
<hr />
<h3 id="switch-문-개요">switch 문 개요</h3>
<pre><code class="language-java">switch(비교할변수) {

    case 비교값1 : 
        비교값1과 일치하는 경우 실행할 구문; 
        break;
    case 비교값2 : 
        비교값2와 일치하는 경우 실행할 구문; 
        break;
    default : 
        case에 모두 해당하지 않는 경우 실행할 구문;         
}</code></pre>
<p><code>switch</code>문은 입력받은 값을 확인해 비교값과 일치하는 <code>case</code>문으로 분기돼 실행한다.
<code>case</code>문에서 실행되고 <code>break</code> 문을 만나면 빠져나올 수 있다.</p>
<hr />
<h1 id="반복문">반복문</h1>
<blockquote>
<p>반복문 : 특정 코드를 반복하여 수행할 수 있도록 제어하는 명령문</p>
</blockquote>
<p>반복문을 모른다면 1~1000까지 출력하는 것을 이렇게 해야 할지도 모른다.</p>
<pre><code class="language-java">public static void main(String[] args) {

    System.out.println(&quot;1부터 1000까지 출력하기&quot;);
    System.out.println(&quot;출력 : 1&quot;);
    System.out.println(&quot;출력 : 2&quot;);
    System.out.println(&quot;출력 : 3&quot;);

    ...

    System.out.println(&quot;출력 : 998&quot;);
    System.out.println(&quot;출력 : 999&quot;);
    System.out.println(&quot;출력 : 1000&quot;); 
}</code></pre>
<p>반복문을 사용하면 간단하게 줄일 수 있다.</p>
<pre><code class="language-java">public static void main(String[] args) {

    System.out.println(&quot;1부터 1000까지 출력하기&quot;);

    for(int i = 1 ; i &lt;= 1000; i++){
        System.out.println(&quot;출력 : &quot; + i);
    }

}</code></pre>
<h2 id="반복문-종류">반복문 종류</h2>
<ol>
<li><strong>for 문</strong></li>
<li><strong>while 문</strong></li>
<li><strong>do-while 문</strong></li>
</ol>
<hr />
<h3 id="for문-개요">for문 개요</h3>
<pre><code class="language-java">int num = 0;

for (int i = 0; i &lt; 5; i++) {
    num++;
}

System.out.println(num);</code></pre>
<pre><code>5</code></pre><p>i의 초기식 : 0
i의 조건식 : i &lt; 5일 때까지 (즉, 0,1,2,3,4)
i의 증감식 : i++ (i는 1씩 증가)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e40b4b1-40b1-40f1-92c3-720dcb5edfb5/image.png" /></p>
<hr />
<h3 id="while-문-개요">while 문 개요</h3>
<p>나는 for문을 주로 사용하긴 하지만 조건에 따라 계속 반복해야할 때는 while문을 사용한다.</p>
<table>
<thead>
<tr>
<th>for 문 사용할 때</th>
<th>while 문을 사용할 때</th>
</tr>
</thead>
<tbody><tr>
<td>반복 횟수를 알고 있을 때</td>
<td>무한 루프나 특정 조건을 만족할 때까지 반복할 때</td>
</tr>
<tr>
<td>배열과 주로 사용</td>
<td>파일 읽기, 쓰기 시 주로 사용</td>
</tr>
</tbody></table>
<pre><code class="language-java">1-초기식;

while(2-조건식) {
    3-조건을 만족하는 경우 수행할 구문(반복할 구문);
    4-증감식;
}</code></pre>
<p>초기식을 통해 2번 조건식이 true라면 반복한다. (조건문이 false가 되면 빠져나올 수 있음)</p>
<p>false가 돼서 빠져 나오기 위해 4. 증감식이 사용되기도 한다.</p>
<hr />
<h3 id="do-while-문-개요">do-while 문 개요</h3>
<p>조건식을 확인하면서 반복할지 안 할지 판단하는건 while과 동일하지만, do-while문은 반드시 1번은 반복할 구문을 실행한다.</p>
<pre><code class="language-java">do {
     1회차에는 무조건 실행하고, 이후에는 조건식을 확인하여 조건을 만족하는 경우 수행할 구문(반복할 구문);
     증감식;
} while(조건식);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3a91dca0-16e5-4c09-b289-5fa1b8b4b2f3/image.png" /></p>
<hr />
<h1 id="분기문">분기문</h1>
<blockquote>
<p>분기문 : 조건문 or 반복문 안에서 실행 흐름을 바꿀 수 있는 구문</p>
</blockquote>
<ul>
<li>break : 강제로 조건문 or 반복문을 빠져나올 때 사용</li>
<li>continue : 반복문에서 해당 회차를 continue 시점에서 건더뛰고 다음 회차로 이동한다.</li>
</ul>