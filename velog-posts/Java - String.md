<h1 id="string">String</h1>
<blockquote>
<p><strong>String</strong> : 문자열을 나타내는 자료형</p>
</blockquote>
<p>String 인스턴스는 한 번 생성되면 그 값을 읽기만 가능하고 변경할 수는 없다. 이러한 객체를 불변 객체(immutable object) 라고 한다.</p>
<p>즉, 덧셈(+) 연산자를 이용해 문자열을 결합하는 경우 기존 문자열이 변경되는 것 X
문자열이 합쳐진 새로운 String 인스턴스가 생성되는 것</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e40d997b-1f56-4644-a019-b1b692e0c30d/image.png" /></p>
<h2 id="주요-메소드">주요 메소드</h2>
<table>
<thead>
<tr>
<th>메소드</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>charAt()</td>
<td>해당 문자열의 특정 인덱스에 해당하는 문자를 반환한다. 인덱스는 0부터 시작하는 숫자 체계를 의미하며 인덱스를 벗어난 정수를 인자로 전달하는 경우에는  IndexOutOfBoundsException이 발생한다.</td>
</tr>
<tr>
<td>compareTo()</td>
<td>인자로 전달된 문자열과 사전 순으로 비교를 하여 두 문자열이 같다면 0 을 반환, 인자로 전달된 문자열보다 작으면 음수를, 크면 양수를 반환한다. 단, 이 메소드는 대소문자를 구분하여 비교한다.</td>
</tr>
<tr>
<td>compareToIgnoreCase()</td>
<td>대소문자를 구분하지 않고 비교한다</td>
</tr>
<tr>
<td>concat()</td>
<td>문자열에 인자로 전달된 문자열을 합치기해서 새로운 문자열을 반환한다. 원본 문자열에는 영향을 주지 않는다.</td>
</tr>
<tr>
<td>indexOf()</td>
<td>문자열에서 특정 문자를 탐색하여 처음 일치하는 인덱스 위치를 정수형으로 반환한다. 단, 일치하는 문자가 없는 경우 -1을 반환한다.</td>
</tr>
<tr>
<td>lastIndexOf()</td>
<td>문자열 탐색을 뒤에서부터 하고 처음 일치하는 위치의 인덱스를 반환한다. 단, 일치하는 문자가 없는 경우 -1을 반환한다.</td>
</tr>
<tr>
<td>trim()</td>
<td>문자열의 앞 뒤에 공백을 제거한 문자열을 반환한다.</td>
</tr>
<tr>
<td>toLowerCase()</td>
<td>모든 문자를 소문자로 변환시킨다. 원본에는 영향을 주지 않는다.</td>
</tr>
<tr>
<td>toUpperCase()</td>
<td>모든 문자를 대문자로 변환시킨다. 원본에는 영향을 주지 않는다.</td>
</tr>
<tr>
<td>substring()</td>
<td>문자열의 일부분을 잘라내어 새로운 문자열을 반환한다. 원본에 영향을 주지 않는다.</td>
</tr>
<tr>
<td>replace()</td>
<td>문자열에서 대체할 문자열로 기존 문자열을 변경해서 반환한다. 원본에 영향을 주지 않는다.</td>
</tr>
<tr>
<td>length()</td>
<td>문자열의 길이를 정수형으로 반환한다.</td>
</tr>
<tr>
<td>isEmpty()</td>
<td>문자열의 길이가 0이면 true를 반환, 아니면 false를 반환한다. 길이가 0인 문자열은 null과는 다르다.</td>
</tr>
<tr>
<td>split()</td>
<td>정규표현식을 이용하여 문자열을 분리한다.</td>
</tr>
</tbody></table>
<hr />
<h2 id="활용">활용</h2>
<h3 id="charat">charAt()</h3>
<p>해당 인덱스에 있는 문자를 반환한다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application1 {
    public static void main(String[] args) {
        String str1 = &quot;apple&quot;;
        for (int i = 0; i &lt; str1.length(); i++) {
            System.out.println(&quot;charAt(&quot; + i + &quot;) = &quot; + str1.charAt(i));
        }
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7d4897c9-cc86-453b-890b-22d22368573d/image.png" /></p>
<hr />
<h3 id="compareto">compareTo</h3>
<p>인자로 전달된 문자열과 사전 순으로 비교를 하여 두 문자열이 같다면 0 을 반환, 인자로 전달된 문자열보다 작으면 음수를, 크면 양수를 반환한다.
단, 이 메소드는 대소문자를 구분하여 비교한다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application1 {
    public static void main(String[] args) {
        String str1 = &quot;apple&quot;;
        for (int i = 0; i &lt; str1.length(); i++) {
            System.out.println(&quot;charAt(&quot; + i + &quot;) = &quot; + str1.charAt(i));
        }

        String str2 = &quot;java&quot;;
        String str3 = &quot;java&quot;;
        String str4 = &quot;JAVA&quot;;
        String str5 = &quot;mariaDB&quot;;

        System.out.println(str2.compareTo(str3));
        System.out.println(str2.compareTo(str4));
        System.out.println(str4.compareTo(str2));
        System.out.println(str2.compareTo(str5));
        System.out.println(str5.compareTo(str2));
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/769d622d-b6b2-4285-b91a-67f86ed1b270/image.png" /></p>
<p><code>(int) j : 106, (int) J : 74</code> 라서 32 차이가 나는 것이다.</p>
<blockquote>
<p>대소문자 비교 안 할거면 <code>compareToIgnoreCase()</code> 사용하기</p>
</blockquote>
<hr />
<h3 id="concat">concat()</h3>
<blockquote>
<p><strong>concat()</strong> : 문자열 합치기</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;concat : &quot; + str2.concat(str5));</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ed36c095-eb35-4d7e-b48f-e19962d2c1f1/image.png" /></p>
<hr />
<h3 id="indexof-lastindexof">indexOf(), lastIndexOf()</h3>
<blockquote>
<p><strong>indexOf()</strong> : 문자열에서 특정 문자를 탐색하여 처음 일치하는 인덱스 위치를 정수형으로 반환한다. 단, 일치하는 문자가 없는 경우 -1을 반환한다.</p>
</blockquote>
<blockquote>
<p>lastIndexOf() : 문자열 탐색을 <strong>뒤에서부터</strong> 하고 처음 일치하는 위치의 인덱스를 반환한다. 단, 일치하는 문자가 없는 경우 -1을 반환한다.</p>
</blockquote>
<pre><code class="language-java">String indexOf = &quot;java mariaDB&quot;;
System.out.println(&quot;indexOf('a') = &quot; + indexOf.indexOf('a'));
System.out.println(&quot;indexOf('b') = &quot; + indexOf.indexOf('b'));</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/728964d3-618b-4be9-973f-478e51b15b3f/image.png" /></p>
<hr />
<h3 id="trim">trim()</h3>
<blockquote>
<p><strong>trim()</strong> : 문자열 앞, 뒤의 공백을 제거한 문자열 반환</p>
</blockquote>
<pre><code class="language-java">String trimStr = &quot;    java    &quot;;
System.out.println(&quot;trimStr = #&quot; + trimStr + &quot;#&quot;);
System.out.println(&quot;trim() = #&quot; + trimStr.trim() + &quot;#&quot;);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/803872b5-2984-42f9-b93b-8cce0ce9c573/image.png" /></p>
<hr />
<h3 id="tolowercase-touppercase">toLowerCase(), toUpperCase()</h3>
<blockquote>
<p><strong>toLowerCase()</strong> : 모든 문자를 소문자로 변환
<strong>toUpperCase()</strong> : 모든 문자를 대문자로 변환</p>
</blockquote>
<pre><code class="language-java">String caseStr = &quot;javamariaDB&quot;;

System.out.println(&quot;toLowerCase() = &quot; + caseStr.toLowerCase());
System.out.println(&quot;toUpperCase() = &quot; + caseStr.toUpperCase());
System.out.println(&quot;caseStr : &quot; + caseStr);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1bd24c4c-c12e-4c84-8490-c4b8dab71271/image.png" /></p>
<hr />
<h3 id="substring">substring()</h3>
<blockquote>
<p><strong>substring</strong> : 문자열의 일부분을 잘라내 새로운 문자열을 반환하는 것</p>
</blockquote>
<pre><code class="language-java">String javamariaDB = &quot;javamariaDB&quot;;

System.out.println(&quot;substring(3, 6) : &quot; + javamariaDB.substring(3, 6));
System.out.println(&quot;substring(3) : &quot; + javamariaDB.substring(3));
System.out.println(&quot;javamariaDB : &quot; + javamariaDB);</code></pre>
<ul>
<li><p><code>substring(start, end)</code> : <code>start</code>번째부터 <code>(end - 1)</code>번째 인덱스까지 자른다.
즉, <code>substring(3, 6)</code> -&gt; 3 ~ 5 인덱스까지 자르기</p>
</li>
<li><p><code>substring(start)</code> : start 인덱스부터 끝까지</p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b3bb9189-8084-49f8-ab6a-0c5fc4270af9/image.png" /></p>
<hr />
<h3 id="replace">replace()</h3>
<blockquote>
<p><strong>replace()</strong> : 문자열에서 대체할 문자열로 기존 문자열을 변경</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;replace() : &quot; + javamariaDB.replace(&quot;java&quot;, &quot;python&quot;));
System.out.println(&quot;javamariaDB : &quot; + javamariaDB);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/159d45d6-a1d2-4776-9a59-082991d07888/image.png" /></p>
<hr />
<h3 id="length">length()</h3>
<blockquote>
<p><strong>length()</strong> : 문자열의 길이를 정수형을 반환</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;length() : &quot; + javamariaDB.length());
System.out.println(&quot;빈 문자열 길이 :&quot; + &quot;&quot;.length());</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e8790b4-1f37-4dcc-ba14-3a266b9c011b/image.png" /></p>
<hr />
<h3 id="isempty">isEmpty()</h3>
<blockquote>
<p><strong>isEmpty()</strong> : 문자열의 길이가 0이면 true, 아니면 false</p>
</blockquote>
<pre><code class="language-java">System.out.println(&quot;isEmpty() : &quot; + javamariaDB.isEmpty());
System.out.println(&quot;isEmpty() : &quot; + &quot;abc&quot;.isEmpty());
System.out.println(&quot;isEmpty() : &quot; + &quot;&quot;.isEmpty());</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1c8d2de0-b626-43f6-873c-8ca2f7b8835e/image.png" /></p>
<hr />
<h3 id="split">split()</h3>
<blockquote>
<p><strong>split()</strong> : 특정 문자열을 기준으로 기존 문자열 분리</p>
</blockquote>
<pre><code class="language-java">String emp1 = &quot;100/홍길동/서울/영업부&quot;;
String emp2 = &quot;200/유관순//총무부&quot;;
String emp3 = &quot;300/이순신/경기도/&quot;;
String emp4 = &quot;400/우왕우?마라도/&quot;;</code></pre>
<p>위와 같은 데이터들을 특정 문자열을 기준으로 나눠보자.</p>
<ol>
<li><code>split()</code><strong>을 활용해서 배열에 저장하기</strong><pre><code class="language-java">String[] empArr1 = emp1.split(&quot;/&quot;);
String[] empArr2 = emp2.split(&quot;/&quot;);
String[] empArr3 = emp3.split(&quot;/&quot;);
String[] empArr4 = emp4.split(&quot;[/?]&quot;); // 이건 / 하고 ? 2개의 기준으로 split 한다.
</code></pre>
</li>
</ol>
<p>System.out.println(Arrays.toString(empArr1));
System.out.println(Arrays.toString(empArr2));
System.out.println(Arrays.toString(empArr3));
System.out.println(Arrays.toString(empArr4));</p>
<pre><code>![](https://velog.velcdn.com/images/jojehuni_9759/post/70b87fad-083d-4c0f-8298-fd83504788a3/image.png)

2. **StringTokenizer 활용**
```java
String colors = &quot;red, yellow, green, purple, blue&quot;;
StringTokenizer colorStringTokenizer = new StringTokenizer(colors, &quot;, &quot;);

while (colorStringTokenizer.hasMoreTokens()) {
    // System.out.println(colorStringTokenizer.nextToken());

    // nextToken은 한 번 저장하고 다음 커서로 이동하기 때문에 연달아 사용하고 싶으면 아래처럼 사용해야 한다.
    String token = colorStringTokenizer.nextToken(); 
    System.out.println(token);
}</code></pre><p>&quot;,&quot; 문자열 기준으로 토큰 단위로 저장한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ee6f4760-ed74-4da1-b9d1-1ef49437c3a6/image.png" /></p>
<hr />
<h2 id="string-클래스-인스턴스-생성-방식">String 클래스 인스턴스 생성 방식</h2>
<h3 id="1-문자열-비교하는-방식-2가지-확인">1. 문자열 비교하는 방식 2가지 확인</h3>
<p>아래와 같은 코드가 있을 때 어떻게 출력될까?</p>
<pre><code class="language-java">public class Application2 {
    public static void main(String[] args) {
        String str1 = &quot;java&quot;;
        String str2 = &quot;java&quot;;
        String str3 = new String(&quot;java&quot;);
        String str4 = new String(&quot;java&quot;);

        System.out.println(&quot;str1 == str2 : &quot; + (str1 == str2));
        System.out.println(&quot;str2 == str3 : &quot; + (str2 == str3));
        System.out.println(&quot;str3 == str4 : &quot; + (str3 == str4));
    }
}</code></pre>
<p>나는 기존에 String 클래스는 <code>==</code> 비교 연산자로 하면 false가 나온다고 알고 있었기에 셋 다 false라고 생각했지만</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/80f73781-08b5-44f1-b396-df978384d494/image.png" /></p>
<p><code>str1 == str2</code> 에서 주소값을 비교하는데 true? -&gt; <strong>str1, str2 는 객체가 1개라는 뜻이다.</strong></p>
<ul>
<li><strong>stack</strong> 영역에는 <code>str1</code>, <code>str2</code>, <code>str3</code>, <code>str4</code> 가 생긴다.</li>
<li><strong>heap</strong> 영역에는 <code>str1</code>, <code>str2</code>는 리터럴 문자열 형태로 받는 경우에 <strong>상수 풀 (constant pool)</strong>에 &quot;java&quot;라는 동일한 값의 인스턴스가 들어가고, 그 데이터에 대한 주소 값은 <code>str1</code>, <code>str2</code>가 얕은 복사와 같이 되는 것이다.</li>
<li>하지만, <code>new</code> 연산자로 만든 <code>str3</code>, <code>str4</code> 는 각각 <strong>다른 주소값을 가진 다른 객체</strong>인 것이다.</li>
</ul>
<p><strong>Application2</strong></p>
<pre><code>public class Application2 {
    public static void main(String[] args) {
        String str1 = &quot;java&quot;;
        String str2 = &quot;java&quot;;
        String str3 = new String(&quot;java&quot;);
        String str4 = new String(&quot;java&quot;);

        System.out.println(&quot;str1 == str2 : &quot; + (str1 == str2)); // 주소값을 비교하는데 true? -&gt; str1, str2 는 객체가 1개라는 뜻이다.
        System.out.println(&quot;str2 == str3 : &quot; + (str2 == str3));
        System.out.println(&quot;str3 == str4 : &quot; + (str3 == str4));

        System.out.println(&quot;str1.equals(str3) : &quot; + (str1.equals(str3)));
        System.out.println(&quot;str1.hashCode() == str3.hashCode() : &quot; + (str1.hashCode() == str3.hashCode()));
    }
}</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c2b01fd3-6e51-4144-ae27-20cf0f4bd122/image.png" /></p>
<blockquote>
<p>즉, 문자열을 비교하고 싶으면 <strong>equals()</strong> 를 쓰자.</p>
</blockquote>
<hr />
<h3 id="2-문자열은-불변객체">2. 문자열은 불변객체</h3>
<blockquote>
<p>  문자열은 불변객체로 <strong>변화를 주면 항상 새로운 객체를 생성</strong>한다.</p>
</blockquote>
<pre><code class="language-java">String str = &quot;apple&quot;;
System.out.println(System.identityHashCode(str));

str += &quot;, banana&quot;;
System.out.println(System.identityHashCode(str));
System.out.println(&quot;fruit : &quot; + str);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/236c5201-abf3-4367-8b15-e4f0650fe93d/image.png" /></p>
<hr />
<h2 id="특수-기능을-위한-문자">특수 기능을 위한 문자</h2>
<h3 id="이스케이프-문자">이스케이프 문자</h3>
<blockquote>
<p>이스케이프 문자 : 문자열 내에서 사용하는 특수기능을 위한 문자</p>
</blockquote>
<ul>
<li><code>\n</code> : 개행</li>
<li><code>\t</code> : 탭</li>
<li><code>\'</code> : 작은 따옴표</li>
<li><code>\&quot;</code> : 큰 따옴포</li>
<li><code>\\</code> : 역슬래시 표기</li>
</ul>
<pre><code class="language-java">System.out.println(&quot;안녕하세요 \n저는 홍길동입니다.&quot;);
System.out.println(&quot;안녕하세요 \t저는 홍길동입니다.&quot;);

System.out.println(&quot;안녕하세요 저는 '홍길동'입니다.&quot;);
System.out.println(&quot;안녕하세요 저는 \&quot;홍길동\&quot;입니다.&quot;);

System.out.println(&quot;역슬래시(\\)입니다.&quot;);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c6f9fe20-d5bb-40c9-b424-33e7a014a789/image.png" /></p>
<ul>
<li><strong>&quot;탭&quot;</strong>은 이미 일정 구간으로 나눠논 공간이 있다면 현재 구간에서 다음 구간으로 이동하는데 사용하는 특수기능이기 때문에 어쩔 때는 멀리, 어쩔 때는 조금 이동하기도 한다.</li>
</ul>
<hr />
<h3 id="printf-관련-문법">printf 관련 문법</h3>
<pre><code class="language-java">/* %.2f : 소수점 둘째자리까지 실수를 표현하며 셋째자리에서 반올림 */
/* %d : 정수형에서 표현 */

System.out.printf(&quot;원주율은 %.2f입니다. 우린 %d로 하죠&quot;, 3.141592, 3);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ebec8377-e69b-42d0-8272-a7afb0fa8a68/image.png" /></p>
<hr />
<h2 id="문자열을-다양한-기본-자료형을-바꾸기">문자열을 다양한 기본 자료형을 바꾸기</h2>
<pre><code class="language-java">byte b = Byte.valueOf(&quot;1&quot;);
short s = Short.valueOf(&quot;2&quot;);
int i = Integer.valueOf(&quot;4&quot;);
long l = Long.valueOf(&quot;8&quot;);
float f = Float.valueOf(&quot;4.0&quot;);
double d = Double.valueOf(&quot;8.0&quot;);
boolean isTrue = Boolean.valueOf(&quot;true&quot;);
char c = &quot;abc&quot;.charAt(0);                   // Character가 제공하지 않아 String의 charAt() 사용</code></pre>