<h1 id="wrapper-클래스">Wrapper 클래스</h1>
<p>기존 기본 자료형은 자료의 크기만 나타냈다.
즉, 클래스가 아니었기에 기능을 추가할 수 없었다.</p>
<p><strong>기본 타입의 데이터를 인스턴스화하기 위해 Wrapper 클래스를 만들었다.</strong></p>
<ul>
<li><strong>장점</strong></li>
</ul>
<ol>
<li>기능, 속성을 가질 수 있다.</li>
<li>클래스는 최상위 부모가 Object 여서 여러 메소드를 사용할 수 있다.</li>
<li>다형성도 적용할 수 있다.</li>
</ol>
<h2 id="wrapper-클래스-종류">Wrapper 클래스 종류</h2>
<table>
<thead>
<tr>
<th>기본 타입</th>
<th>래퍼 클래스</th>
</tr>
</thead>
<tbody><tr>
<td>byte</td>
<td>Byte</td>
</tr>
<tr>
<td>short</td>
<td>Short</td>
</tr>
<tr>
<td>int</td>
<td>Integer</td>
</tr>
<tr>
<td>long</td>
<td>Long</td>
</tr>
<tr>
<td>float</td>
<td>Float</td>
</tr>
<tr>
<td>double</td>
<td>Double</td>
</tr>
<tr>
<td>char</td>
<td>Character</td>
</tr>
<tr>
<td>boolean</td>
<td>Boolean</td>
</tr>
</tbody></table>
<hr />
<h2 id="boxing-unboxing">Boxing, UnBoxing</h2>
<blockquote>
<ul>
<li><code>Boxing</code> : 기본 타입을 래퍼클래스의 인스턴스로 인스턴스화 하는 것</li>
</ul>
</blockquote>
<ul>
<li><code>UnBoxing</code> : 래퍼클래스 타입의 인스턴스를 기본 타입으로 변경하는 것</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cb0fcde6-db75-4569-9e0a-0a5f2c0a7d62/image.png" /></p>
<pre><code class="language-java">    public static void main(String[] args) {
        int intValue = 20;

        // 박싱
        Integer boxingInt = (Integer)20;
        Integer boxingInt2 = Integer.valueOf(intValue);

        // 언박싱
        int unboxingValue = boxingInt.intValue();</code></pre>
<blockquote>
<p>JDK 1.5부터는 Boxing, UnBoxing을 필요한 상황에서 컴파일러가 자동으로 해준다.</p>
</blockquote>
<pre><code class="language-java">        // 오토박싱, 오토언박싱
        Integer autoBoxingInt = intValue;
        int autoUnboxingInt = autoBoxingInt;
    }
}</code></pre>
<p><code>Application</code> 클래스에 아래 메소드를 추가해보자.</p>
<pre><code class="language-java">    public static void anythingMethod(Object obj) {
        System.out.println(&quot;obj : &quot; + obj.toString());
    }</code></pre>
<p>매개변수로 <code>Object</code>를 받는데
<code>Application</code> 에서</p>
<pre><code class="language-java">anythingMethod(10);</code></pre>
<p>를 하면 뭐가 나올까?</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/61ae61f0-8559-481c-aee9-872959f3f456/image.png" /></p>
<p><code>int</code>형을 받는데 <code>Integer</code>로 오토박싱이 된 후, <code>Object</code>로 다형성이 실행된다.</p>
<blockquote>
<p>출력 -&gt; <code>Object</code>의 <code>toString()</code>에서(정적 바인딩) <code>Integer</code>의 <code>toString()</code>이(동적바인딩) 실행됨</p>
</blockquote>
<hr />
<h3 id="wrapper-클래스-주소-값-비교">Wrapper 클래스 주소 값 비교</h3>
<pre><code class="language-java">Integer integerTest = 30;
Integer integerTest2 = 30;

System.out.println(&quot;== 비교 : &quot; + (integerTest == integerTest2));
System.out.println(&quot;equals() 비교 : &quot; + integerTest.equals(integerTest2));
System.out.println(&quot;integerTest 주소 : &quot; + System.identityHashCode(integerTest));
System.out.println(&quot;integerTest2 주소 : &quot; + System.identityHashCode(integerTest2));</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/78aa194e-5d94-4b4f-81c4-03b521285495/image.png" /></p>