<p>코딩테스트를 준비하면서 자료구조와 알고리즘에 대한 개념을 정리하고자 한다.</p>
<h1 id="hash">Hash</h1>
<blockquote>
<p><strong>해시 (Hash)</strong> : 입력 데이터를 고정된 길이의 데이터로 변환된 값</p>
</blockquote>
<p>데이터의 KEY 값이 Hash 함수를 통해 변환된 간단한 정수이고,</p>
<p>정수로 변환된 Hash는 배열의 index, 위치, 데이터 값을 저장, 검색할 때 활용</p>
<h2 id="특징">특징</h2>
<ul>
<li>Key - Value 를 매핑할 수 있는 데이터 구조</li>
<li>Hash 함수를 통해 Key의 데이터를 배열에 저장할 수 있는 주소를 계산</li>
<li>Key를 통해서 저장된 데이터를 빠르게 찾고, 저장 및 탐색 속도가 획기적으로 빨라진다.</li>
</ul>
<h2 id="hash-function">Hash Function</h2>
<blockquote>
<p><strong>Hash Function</strong> : 임의의 데이터를 고정된 길이의 값으로 리턴해주는 함수</p>
</blockquote>
<p>HashFunction 은 '입력받은 데이터를 Hash 값으로 출력시키는 알고리즘'</p>
<p>예시)</p>
<pre><code>Integer hashFunction(String key) {

    return (int)(key.charAt(0)) % 100;
}</code></pre><p>String key에서 charAt(0) 으로 문자의 0번에 해당하는 부분을 (int)로 정수화하여 100으로 나머지를 반환하는 함수다.</p>
<p>반환된 값은 배열의 인덱스가 되고, 해당 인덱스에 맞게 저장할 수 있게 된다.</p>
<h2 id="hash-table">Hash Table</h2>
<blockquote>
<p><strong>Hash Table</strong> : 키와 값을 함께 저장해 둔 데이터 구조</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2c76346c-3272-48bd-a5fa-d69ae411eb3e/image.png" /></p>
<p>행과 열로 구성된 표에 저장되는 것과 유사하다.</p>
<blockquote>
<p>추가 용어</p>
<ul>
<li>버킷(bucket) : 하나의 주소를 갖는 파일의 한 구역, 버킷의 크기는 같은 주소에 포함될 수 있는 레코드 수</li>
<li>슬롯(slot) : 한 개의 레코드를 저장할 수 있는 공간, 한 버킷 안에 여러 개의 슬롯이 있다.</li>
</ul>
</blockquote>
<h2 id="hashing-해싱">Hashing (해싱)</h2>
<blockquote>
<p><strong>Hashing</strong> : Hash Function 에서 해시를 출력하고, Hash Table에 저장하는 과정까지의 행위</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/55c2ebeb-3bac-489d-a43a-b2da41ca5724/image.png" /></p>
<h2 id="장단점">장단점</h2>
<h3 id="장점">장점</h3>
<ol>
<li>데이터 저장 / 읽기 속도가 좋다.</li>
<li>Hash는 key에 대한 데이터가 있는지 확인하기 쉽다.</li>
</ol>
<h3 id="단점">단점</h3>
<ol>
<li>일반적으로 저장공간이 많이 필요하다.</li>
<li>여러 키에 해당하는 주소 (index) 가 동일한 경우 충돌을 해결하기 위한 별도의 자료구조가 필요하다.</li>
</ol>
<hr />
<h1 id="hashmap">HashMap</h1>
<p>데이터를 저장할 때 key, value가 짝을 이루어 저장된다.</p>
<h2 id="hashmap-생성방법">HashMap 생성방법</h2>
<pre><code class="language-java">import java.util.HashMap;

public class Hash {

    HashMap&lt;String, Integer&gt; map1 = new HashMap&lt;&gt;();        // capacity : 16, load factor : 0.75
    HashMap&lt;String, Integer&gt; map2 = new HashMap&lt;&gt;(20);        // capacity : 20, load factor : 0.75
    HashMap&lt;String, Integer&gt; map3 = new HashMap&lt;&gt;(20, 0.8);    // capacity : 20, load factor : 0.8
    HashMap&lt;String, Integer&gt; map4 = new HashMap&lt;&gt;(map1);    // Map(map1)의 데이터로 초기화

}</code></pre>
<blockquote>
<p>capacity : 데이터 저장 용량
load factor : 데이터 저장공간을 추가로 확보해야하는 시점을 지정</p>
</blockquote>
<h2 id="hashmap-메소드">HashMap 메소드</h2>
<ol>
<li>데이터 추가
```java</li>
<li>V put(K key, V value) : key와 value를 저장</li>
<li>void putAll(Map&lt;? extends K, ? extends V&gt; m) : Map m의 데이터를 전부 저장</li>
<li>V putIfAbsent(K key, V value) : 기존 데이터에 key가 없으면  key와 value를 저장<pre><code></code></pre></li>
</ol>
<ul>
<li><strong>예제 코드</strong><pre><code class="language-java">import java.util.HashMap;
</code></pre>
</li>
</ul>
<p>public class Hash {
    public static void main(String[] args) {
        HashMap&lt;String, Integer&gt; map1 = new HashMap&lt;&gt;();
        HashMap&lt;String, Integer&gt; map2 = new HashMap&lt;&gt;();</p>
<pre><code>    // put : key, value 저장
    map1.put(&quot;A&quot;, 1);
    map1.put(&quot;B&quot;, 2);
    map1.put(&quot;C&quot;, 3);

    // putIfAbsent : 기존 데이터에 key (&quot;D&quot;) 가 없으면 key, value 저장
    map1.putIfAbsent(&quot;D&quot;, 4);
    map1.putIfAbsent(&quot;E&quot;, 5);

    // putAll : Map map1의 데이터를 전부 저장
    map2.putAll(map1);

    System.out.println(&quot;map1 = &quot; + map1);
    System.out.println(&quot;map2 = &quot; + map2);
}</code></pre><p>}</p>
<pre><code>![](https://velog.velcdn.com/images/jojehuni_9759/post/3dad8474-0b77-4307-9b17-76a3f140f8d0/image.png)

---
2. 데이터 확인
```java
V get(Object key) : key와 맵핑된 value값을 반환
V getOrDefault(Object key, V defaultValue) : key와 맵핑된 value값을 반환하고 없으면 defaultValue값을 반환
Set&lt;Map.Entry&lt;K, V&gt;&gt; entrySet( ) : 모든 key-value 맵핑 데이터를 가진 Set 데이터를 반환합니다. 
Set&lt;K&gt; keySet( ) : 모든 key 값을 가진 Set 데이터를 반환
Collection&lt;V&gt; values( ) : 모든 value 값을 가진 Collection 데이터를 반환</code></pre><ol start="3">
<li><p>데이터 수정</p>
<pre><code class="language-java">V replace(K key, V value) : key와 일치하는 기존 데이터의 value를 변경
V replace(K key, V oldValue, V newValue) : key와 oldValue가 동시에 일치하는 데이터의 value를 newValue로 변경</code></pre>
</li>
<li><p>데이터 삭제</p>
<pre><code class="language-java">void clear( ) : 모든 데이터를 삭제
V remove(Object key) : key와 일치하는 기존 데이터( key와 value)를 삭제
boolean remove(Object key, Object value) : key와 value가 동시에 일치하는 데이터를 삭제</code></pre>
</li>
</ol>
<h3 id="최종-예제-코드">최종 예제 코드</h3>
<pre><code class="language-java">package jehun.study.section01;

import java.util.HashMap;

public class Hash {
    public static void main(String[] args) {
        HashMap&lt;String, Integer&gt; map1 = new HashMap&lt;&gt;();
        HashMap&lt;String, Integer&gt; map2 = new HashMap&lt;&gt;();

        // put : key, value 저장
        map1.put(&quot;A&quot;, 1);
        map1.put(&quot;B&quot;, 2);
        map1.put(&quot;C&quot;, 3);

        // putIfAbsent : 기존 데이터에 key (&quot;D&quot;) 가 없으면 key, value 저장
        map1.putIfAbsent(&quot;D&quot;, 4);
        map1.putIfAbsent(&quot;E&quot;, 5);

        // putAll : Map map1의 데이터를 전부 저장
        map2.putAll(map1);

        System.out.println(&quot;map1 = &quot; + map1);
        System.out.println(&quot;map2 = &quot; + map2);

        // 데이터 확인
        System.out.println(&quot;[1]: &quot; + map1.containsKey(&quot;A&quot;));    // true
        System.out.println(&quot;[2]: &quot; + map1.containsValue(1));    // true
        System.out.println(&quot;[3]: &quot; + map1.isEmpty());           // false
        System.out.println(&quot;[4]: &quot; + map1.size());              // 5
        System.out.println(&quot;[5]: &quot; + map1);                     // {A=1, B=2, C=3, D=4, E=5}
        System.out.println(&quot;[6]: &quot; + map1.remove(&quot;A&quot;, 1));      // true
        System.out.println(&quot;[7]: &quot; + map1.put(&quot;B&quot;, 0));         // 2
        System.out.println(&quot;[8]: &quot; + map1.replace(&quot;C&quot;, 0));     // 3
        System.out.println(&quot;map1 : &quot; + map1);                   // map1 : {B=0, C=0, D=4, E=5}
        System.out.println(&quot;map2 : &quot; + map2);                   // map2 : {A=1, B=2, C=3, D=4, E=5}
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b13747ba-d070-4609-aaaa-bea4e918be1a/image.png" /></p>