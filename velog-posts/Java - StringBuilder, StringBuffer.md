<h1 id="stringbuilder">StringBuilder</h1>
<p><code>StringBuilder</code>, <code>StringBuffer</code> : <code>String</code>은 불변하기에 값을 변경할 수 없어서 계속 객체 생성되는 단점을 해결하기 위해 등장한 것이다.</p>
<p><strong>String과의 차이점</strong></p>
<ul>
<li>String은 불변, StringBuilder는 가변</li>
</ul>
<blockquote>
<p><code>StringBuffer</code>도 있지만 <code>StringBuilder</code> 먼저 보자.</p>
</blockquote>
<pre><code class="language-java">public class Application1 {
    public static void main(String[] args) {
        String testStr = &quot;java&quot;;
        StringBuilder testSb = new StringBuilder(&quot;kotlin&quot;);

        for (int i = 0; i &lt; 9; i++) {
            testStr += i;
            testSb.append(i); // StringBuilder는 메소드로 문자열을 이어붙인다.

            System.out.println(&quot;String의 경우 : &quot; + System.identityHashCode(testStr));
            System.out.println(&quot;StringBuilder의 경우 : &quot; + System.identityHashCode(testSb));
        }

        System.out.println(&quot;&quot;);
        System.out.println(&quot;String 결과 : &quot; + testStr);
        System.out.println(&quot;StringBuilder 결과 : &quot; + testSb);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/621d884f-e6b1-4614-9b4f-55259b7b74c6/image.png" /></p>
<p>사진에서처럼 <code>String</code>은 계속 객체가 생성되기 때문에 주소값이 바뀐다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c63db63f-9fc5-4331-a7b3-26e5a379579f/image.png" /></p>
<p>그래서 성능에서도 차이가 많이 난다.</p>
<hr />
<h2 id="stringbuilder-용량에-대해">StringBuilder 용량에 대해</h2>
<pre><code class="language-java">StringBuilder sb = new StringBuilder();
System.out.println(sb.capacity());</code></pre>
<p>-&gt; 실행해보면 알지만, <code>16</code> 으로 출력된다.</p>
<pre><code class="language-java">for (int i = 0; i &lt; 50; i++) {
    sb.append(i);

    System.out.println(&quot;sb : &quot; + sb);
    System.out.println(&quot;capacity : &quot; + sb.capacity());
    System.out.println(&quot;identityHashCode : &quot; + System.identityHashCode(sb));
}</code></pre>
<p>하다보면 어느 시점에서 기존 <code>capacity</code>의 (<strong>2배 + 2</strong>)만큼 증가하면서 용량이 커진다.
그럼에도 주소값은 그대로 바뀌지 않는다.</p>
<blockquote>
<p>즉, <code>StringBuilder</code>는 가변객체 (필드의 배열의 크기가 가변이다.)</p>
</blockquote>
<p>사진으로 보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0bcb6ead-b157-4973-8f71-c43901e3ec8f/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b1ed85a7-754f-4f0d-b616-8d6f5bd66f72/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f4db0378-4a5d-4056-8a44-c9e7570ac864/image.png" /></p>
<p>객체의 크기가 커지는 것이 아닌, <strong>관리하는 문자열의 크기가 커지는 것</strong>이다.</p>
<hr />
<h2 id="활용">활용</h2>
<h3 id="delete-deletecharat">delete(), deleteCharAt()</h3>
<blockquote>
<p>delete() : 시작 인덱스와 종료 인덱스를 이용해서 문자열에서 원하는 부분의 문자열을 제거한다.
deleteCharAt() : 문자열 인덱스를 이용해서 문자 하나를 제거한다.</p>
</blockquote>
<pre><code class="language-java">StringBuilder sb2 = new StringBuilder(&quot;javamariaDB&quot;);
System.out.println(sb2.capacity());

System.out.println(&quot;delete() : &quot; + sb.delete(2, 5));
System.out.println(&quot;deleteCharAt() : &quot; + sb2.deleteCharAt(0));</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f98f85db-259c-4900-9dd0-4c953d978d76/image.png" /></p>
<hr />
<h3 id="insert">insert()</h3>
<blockquote>
<p>insert() : 인자로 전달된 값을 문자열을 변환 후 지정한 인덱스 위치에 추가한다.</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;insert() : &quot; + sb2.insert(1, &quot;vao&quot;));
System.out.println(&quot;insert() : &quot; + sb2.insert(0, &quot;j&quot;));
System.out.println(&quot;insert() : &quot; + sb2.insert(sb2.length(), &quot;jdbc&quot;));</code></pre>
<p>실제 알고리즘은 삽입할 때 그만큼 있던 데이터를 미룬다.
(맨 뒤에 있는 값부터 insert 되는만큼 미룬 다음 맨 앞쪽 insert 하는 만큼은 비었으므로 거기에 넣어주는 것)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d16fe4fd-7e63-47ff-aaf8-5ed3c61e6a5b/image.png" /></p>
<hr />
<h3 id="reverse">reverse()</h3>
<blockquote>
<p>reverse() : 문자열 인덱스 순번을 역순으로 재배열한다.</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;reverse() : &quot; + sb2.reverse());</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fc4b9cfa-5905-4fa1-b47e-c5a24616088d/image.png" /></p>
<hr />
<p>더 많은 메소드들은 여기에 추가하겠다.</p>
<hr />
<h2 id="stringbuilder-와-stringbuffer-의-차이점">StringBuilder 와 StringBuffer 의 차이점</h2>
<blockquote>
<p>Thread safe</p>
</blockquote>
<p><code>StringBuffer</code>는 <strong>Thread Safe</strong> 하지만, <code>StringBuilder</code>는 그렇지 않다. </p>
<p><code>StringBuffer</code>는 <code>synchronized</code> 키워드가 선언되어 있기 때문에 멀티스레드에서 안전하지만 속도는 <code>StringBuilder</code>에 비해 느리다.</p>
<table>
<thead>
<tr>
<th></th>
<th>String</th>
<th>StringBuilder</th>
<th>StringBuffer</th>
</tr>
</thead>
<tbody><tr>
<td>modifiable</td>
<td>X</td>
<td>O</td>
<td>O</td>
</tr>
<tr>
<td>thread safe</td>
<td>O</td>
<td>X</td>
<td>O</td>
</tr>
<tr>
<td>synchronized</td>
<td>X</td>
<td>X</td>
<td>O</td>
</tr>
<tr>
<td>performance</td>
<td>빠름</td>
<td>빠름</td>
<td>느림</td>
</tr>
</tbody></table>