<h1 id="set---hashset">Set - HashSet</h1>
<p>간단하게 '집합'이기에 <strong>중복 값을 허용하지 않고</strong>, <strong>저장 순서도 유지하지 않는 것</strong></p>
<p>-&gt; 순서를 유지하고 싶다면 <code>LinkedHashSet</code> 클래스 사용하기</p>
<h2 id="중복값-허용-x-순서-보장-x">중복값 허용 X, 순서 보장 X</h2>
<pre><code class="language-java">import java.util.Set;
import java.util.HashSet;

public class Application {
    public static void main(String[] args) {
        Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

        hashSet.add(&quot;java&quot;);
        hashSet.add(&quot;mariaDB&quot;);
        hashSet.add(&quot;servlet&quot;);
        hashSet.add(&quot;spring&quot;);
        hashSet.add(&quot;html&quot;);

        System.out.println(&quot;hashSet = &quot; + hashSet); // hashSet = [mariaDB, spring, java, servlet, html]

        hashSet.add(new String(&quot;mariaDB&quot;)); // 중복값(동등객체)은 Set에 추가되지 않는다.
        hashSet.add(new String(&quot;mariaDB1&quot;));

        System.out.println(&quot;hashSet = &quot; + hashSet); // hashSet = [mariaDB, spring, java, mariaDB1, servlet, html]
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a636d3a8-711d-4072-b32c-ddb2f06d56f9/image.png" /></p>
<p>데이터 삽입 후 출력을 해보았는데 앞서 말했듯 순서 보장 X, 중복값 허용 X인 것을 알 수 있다.</p>
<hr />
<h2 id="인덱스-x">인덱스 X</h2>
<p>인덱스 개념이 없기 때문에 iterator(반복자) 를 사용해야 한다.
위 코드에 아래 추가해서 확인 가능</p>
<pre><code class="language-java">        Iterator&lt;String&gt; iterator = hashSet.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/31a54c08-52f3-41a5-9c52-1c55f5f5c5e4/image.png" /></p>
<hr />
<h3 id="배열로-활용-가능-권장-x">배열로 활용 가능 (권장 X)</h3>
<pre><code class="language-java">        Object[] array = hashSet.toArray();
        for (int i = 0; i &lt; array.length; i++) {
            System.out.println((String)array[i]);
        }</code></pre>
<p>이 개념을 하는 것보다 Iterator를 사용하는 것을 권장한다.</p>
<hr />
<h2 id="메소드">메소드</h2>
<ol>
<li>객체 선언 - <code>new HashSet&lt;&gt;();</code><pre><code class="language-java">Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();
HashSet&lt;String&gt; hashSet = new HashSet&lt;&gt;();</code></pre>
</li>
</ol>
<p>위와 같이 하면 된다.</p>
<ol start="2">
<li><p>데이터 삽입 - <code>add()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);</code></pre>
</li>
<li><p>데이터 삭제 - <code>remove()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);

     hashSet.remove(&quot;java&quot;);</code></pre>
</li>
<li><p>출력은 sout 이용</p>
</li>
<li><p>값을 포함하는지 true, false로 확인하기 - <code>contains()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);

     System.out.println(hashSet.contains(&quot;java&quot;));</code></pre>
</li>
</ol>
<p>실행결과</p>
<pre><code>true</code></pre><ol start="6">
<li><p>Set은 남겨두고 데이터만 없애기 - <code>clear()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);

     hashSet.clear();
     System.out.println(hashSet.toString());</code></pre>
</li>
</ol>
<p>실행결과</p>
<pre><code>[]</code></pre><ol start="7">
<li><p>Set이 비어있는지 아닌지 확인하기 - <code>isEmpty()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);

     hashSet.clear();
     System.out.println(hashSet.isEmpty());</code></pre>
</li>
</ol>
<p>실행결과</p>
<pre><code>false</code></pre><ol start="8">
<li><p>Set 데이터 크기 확인하기 - <code>size()</code></p>
<pre><code class="language-java">     Set&lt;String&gt; hashSet = new HashSet&lt;&gt;();

     hashSet.add(&quot;java&quot;);
     hashSet.add(&quot;mariaDB&quot;);
     hashSet.add(&quot;servlet&quot;);
     hashSet.add(&quot;spring&quot;);

     hashSet.clear();
     System.out.println(hashSet.size());</code></pre>
</li>
</ol>
<p>실행결과</p>
<pre><code>4</code></pre><hr />
<blockquote>
<p>LinkedHashSet 은 순서를 유지하며 메소드는 똑같다.</p>
</blockquote>