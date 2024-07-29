<h1 id="stream-스트림">Stream (스트림)</h1>
<p>컬렉션에 저장된 요소들을 순회하면서 처리할 수 있는 기능으로 람다식 또한 사용할 수 있다.
내부 반복자를 사용하기 때문에 병렬처리가 쉽다는 장점이 있다.</p>
<ul>
<li><p>Java 6 이전
<code>iterator</code>를 사용하여 엘리먼트를 순회</p>
</li>
<li><p>Java 8 이전
<code>for</code>, <code>for-each</code>를 사용하여 엘리먼트를 컬렉션에서 꺼내 다룸</p>
</li>
<li><p>Java 8 이후
<code>stream</code> 사용</p>
</li>
</ul>
<h2 id="특징">특징</h2>
<ol>
<li>스트림은 <strong>원본을 변경하지 않는 읽기 전용</strong>이다.</li>
<li>스트림은 <code>iterator</code> 처럼 <strong>한 번만 사용되고 사라진다.</strong> 필요하면 <strong>다시 스트림을 생성</strong>해야 한다.</li>
<li><strong>최종 연산 전까지 중간 연산이 처리되지 않는다.</strong></li>
<li><strong>병렬 처리가 용이</strong>하다.</li>
</ol>
<blockquote>
<p>병렬 처리? -&gt; 간단하게 쓰레드와 함께 알아보자.
Ex) 어플리케이션 사용자가 2명일 때 (초록, 파랑) / (공용 데이터는 노랑)
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/49679a4b-7cf2-4f46-9cbf-f5a2624a4e1c/image.png" />
이 때 사용자에게는 각각 하나의 쓰레드를 할당 받는다.</p>
</blockquote>
<ol>
<li>초록 사용자가 어플리케이션을 실행했을 때 -&gt; 첫 번째로는 main 쓰레드가 생성된다.</li>
</ol>
<p>-&gt; 1 application, 1 thread
2. 파랑 사용자가 어플리케이션을 실행했을 때 -&gt; 첫 번째로는 main 쓰레드가 생성된다.
-&gt; 1 application, 1 thread
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cf47219d-5f46-42ee-8120-8441f5f4c976/image.png" /></p>
<blockquote>
<p>경우에 따라서는 파랑 사용자는 main 쓰레드 말고도 추가적인 쓰레드를 생성해서 사용 가능하다. (멀티 쓰레드)</p>
</blockquote>
<p>멀티 쓰레드가 되면 일종의 병렬처리가 가능한 것이다. </p>
<p>이런 <strong>어플리케이션 속 쓰레드의 병렬처리와는 다른 Stream의 병렬처리를 알아보자.</strong></p>
<p><strong>Stream의 병렬</strong>
쓰레드가 1개 일 때 cpu가 1개로 담당하기도 하고 요즘은 기본적으로 4개가 내장된 상태인데 
1개 쓰레드를 처리하기 위해서 cpu 1개로만 작업을 처리한다면 cpu가 4개임에도 1개를 사용한 것과 같은 시간이 걸리게 된다.</p>
<p>Stream은 쓰레드의 부하를 분산하는 개념으로 병렬 처리를 한다는 의미이다.</p>
<blockquote>
<p><strong>즉, Stream의 병렬 처리는 1개의 쓰레드를 분산해 처리하는 시간을 줄인다.</strong></p>
</blockquote>
<p>간단하게 알아봤으니 활용해보자.</p>
<hr />
<h1 id="2-stream-활용">2. Stream 활용</h1>
<p>스트림은 크게 생성, 가공, 결과 3가지로 구분된다. </p>
<ol>
<li>생성 : 스트림 생성</li>
<li>가공 : 원하는 결과를 만들기 위한 필터링, 매핑 등의 작업</li>
<li>결과 : 최종 결과를 만들어 내는 작업</li>
</ol>
<pre><code class="language-java">배열, 컬렉션 -&gt; 스트림 생성 -&gt; 매핑 -&gt; 필터링 -&gt; 결과</code></pre>
<hr />
<h2 id="2-1-stream-생성">2-1. Stream 생성</h2>
<h3 id="컬렉션-스트림-생성">컬렉션 스트림 생성</h3>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;String&gt; stringList = new ArrayList&lt;&gt;(Arrays.asList(&quot;Hello&quot;, &quot;World!!&quot;, &quot;Stream&quot;));

        System.out.println(&quot;====== for each&quot;);
        for(String str: stringList) {
            System.out.println(str);
        }

        System.out.println(&quot;====== stream&quot;);
//        stringList.stream().forEach(x -&gt; System.out.println(x)); // Stream으로 바뀐 ArrayList의 요소들이 각각 람다식에 적용돼 동작한다.
        stringList.stream().forEach(System.out::println);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5582e8d2-db64-4cfd-af1c-6d8db22c3fe6/image.png" /></p>
<p>이런 상태인 것인데 일단 for-each와는 똑같이 동작하기 때문에 $O(n)$만큼 걸리는 것은 똑같다.</p>
<hr />
<h4 id="쓰레드-확인하기">쓰레드 확인하기</h4>
<ul>
<li><strong>스트림 사용 전</strong></li>
</ul>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;String&gt; stringList = new ArrayList&lt;&gt; (Arrays.asList(&quot;java&quot;, &quot;mysql&quot;, &quot;jdbc&quot;, &quot;html&quot;, &quot;css&quot;));

        System.out.println(&quot;====== for-each&quot;);
        for (String s : stringList) {
            System.out.println(s + &quot;: &quot; + Thread.currentThread().getName());
        }
    }
}</code></pre>
<p>static 메소드로 제공해주는 Thread 클래스에서도 현재 돌아가고 있는 main 쓰레드에 대해서 확인 가능하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a7760774-9e64-416a-9d5d-c827e07149bd/image.png" /></p>
<ul>
<li><strong>스트림 사용 후 쓰레드 확인 (병렬 처리 X)</strong><pre><code class="language-java">import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
</code></pre>
</li>
</ul>
<p>public class Application {
    public static void main(String[] args) {
        List stringList = new ArrayList&lt;&gt; (Arrays.asList(&quot;java&quot;, &quot;mysql&quot;, &quot;jdbc&quot;, &quot;html&quot;, &quot;css&quot;));</p>
<pre><code>    System.out.println(&quot;====== normal stream&quot;);</code></pre><p>//        stringList.stream().forEach(x -&gt; Application.print(x));
        stringList.stream().forEach(Application::print);</p>
<pre><code>}

private static void print(String s) {
    System.out.println(&quot;s &quot; + &quot;: &quot; + Thread.currentThread().getName());
}</code></pre><p>}</p>
<pre><code>
![](https://velog.velcdn.com/images/jojehuni_9759/post/69a73bdb-641f-47ec-a66b-de5050db2092/image.png)

- **main 쓰레드 상에서 병렬 스트림을 사용**

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;String&gt; stringList = new ArrayList&lt;&gt; (Arrays.asList(&quot;java&quot;, &quot;mysql&quot;, &quot;jdbc&quot;, &quot;html&quot;, &quot;css&quot;));

        System.out.println(&quot;===== parallel stream&quot;);
        stringList.parallelStream().forEach(Application::print);
    }

    private static void print(String s) {
        System.out.println(s + &quot;: &quot; + Thread.currentThread().getName());
    }
}</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c02c42af-aa4c-40d5-a80e-723835e05c96/image.png" /></p>
<hr />
<h3 id="배열-stream-생성">배열 Stream 생성</h3>
<pre><code class="language-java">import java.util.Arrays;
import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {
        String[] sArr = new String[]{&quot;java&quot;, &quot;mariadb&quot;, &quot;jdbc&quot;};

        System.out.println(&quot;====== 배열로 스트림 생성 ======&quot;);
        Stream&lt;String&gt; stringStream = Arrays.stream(sArr);
        stringStream.forEach(System.out::println);

        System.out.println(); // 구분을 위한 개행

        Stream&lt;String&gt; stringStream1 = Arrays.stream(sArr, 0, 2); // start ~ (end - 1)까지 잘라준다.
        stringStream1.forEach(System.out::println);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c95dc01f-0031-498a-98c1-cd21c47ffb0d/image.png" /></p>
<hr />
<h3 id="builder를-활용한-stream-생성">Builder를 활용한 Stream 생성</h3>
<p><code>Builder</code>는 <code>static&lt;T&gt;</code>으로 되어 있는 메소드이며, 호출 시 타입 파라미터를 메소드 호출 방식으로 전달한다.</p>
<p>공간은 더 잡아먹지만 편리하다. 추가적인 setter나 필드 생성이 필요 없이 편리하게 할 수 있다. 하지만 공간을 더 잡아먹는다는 단점때문에 너무 남발하는 것은 좋지 않다.</p>
<pre><code class="language-java">import java.util.Arrays;
import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {
        String[] sArr = new String[]{&quot;java&quot;, &quot;mariadb&quot;, &quot;jdbc&quot;};

        System.out.println(&quot;====== Builder로 스트림 생성 ======&quot;);
        Stream&lt;String&gt; builderStream = Stream.&lt;String&gt;builder()
                .add(&quot;홍길동&quot;)
                .add(&quot;유관순&quot;)
                .add(&quot;윤봉길&quot;)
                .build();

        builderStream.forEach(System.out::println);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3e3200d0-fd16-4ab1-8ba8-d38b3c5d8b4d/image.png" /></p>
<hr />
<h3 id="스트림에서-제공하는-iterate-사용해보기">스트림에서 제공하는 iterate 사용해보기</h3>
<p><code>iterate()</code>를 활용해 수열 형태의 스트림을 생성한다.</p>
<pre><code class="language-java">import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {

        System.out.println(&quot;====== iterate로 스트림 생성 ======&quot;);
        Stream&lt;Integer&gt; integerStream = Stream.iterate(10, value -&gt; value * 2)
                .limit(10);
        integerStream.forEach(value -&gt; System.out.print(value + &quot; &quot;));
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d05395a7-58bc-4b58-91e4-2ee63de7612a/image.png" /></p>
<p>초기값은 10, 그것의 2배를 limit 만큼 반복한다</p>
<pre><code class="language-java">import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {

        System.out.println(&quot;====== iterate로 스트림 생성 ======&quot;);
        Stream.iterate(10, value -&gt; value * 2)
                .limit(10).
                forEach(value -&gt; System.out.print(value + &quot; &quot;));
    }
}</code></pre>
<p>이렇게도 가능하다.</p>
<hr />
<h3 id="기본-타입-스트림-생성">기본 타입 스트림 생성</h3>
<p>제네릭을 사용하지 않고, 기본 타입으로 스트림 생성이 가능하다.</p>
<p><code>range(시작값, 종료값)</code> : 시작값부터 1씩 증가하는 숫자로 종료값 직전(종료값 - 1)까지 범위의 스트림 생성
<code>rangeClosed(시작값, 종료값)</code> : 시작값부터 1씩 증가하는 숫자로 종료값까지 포함한 스트림 생성</p>
<p>제네릭을 사용하지 않기 때문에 불필요한 오토박싱도 일어나지 않는다. </p>
<p>필요한 경우에는 <code>boxed()</code> 를 사용해 박싱을 할 수도 있다.</p>
<pre><code class="language-java">import java.util.stream.IntStream;
import java.util.stream.LongStream;

public class Application {
    public static void main(String[] args) {
        IntStream intStream = IntStream.range(5, 10);
        intStream.forEach(value -&gt; System.out.print(value + &quot; &quot;));
        System.out.println();

        LongStream longStream = LongStream.rangeClosed(5, 10);
        longStream.forEach(value -&gt; System.out.print(value + &quot; &quot;));
        System.out.println();

        Stream&lt;Double&gt; doubleStream = new Random().doubles(5).boxed();
        doubleStream.forEach(value -&gt; System.out.print(value + &quot; &quot;));

    }
}</code></pre>
<p><code>Wrapper</code> 클래스 자료형의 스트림이 필요한 경우 boxing도 가능하다.</p>
<ul>
<li><code>double(개수)</code> : 난수를 활용한 <code>DoubleStream</code>을 개수만큼 생성해 반환하고</li>
<li><code>boxed()</code> : 기본 타입 스트림인 <code>XXXStream</code>을 박싱해 <code>Wrapper</code> 타입의 <code>Stream&lt;XXX&gt;</code>로 변환한다.</li>
</ul>
<p><code>Intstream</code>, <code>LongStream</code> 으로 된 것을 박싱할 때 사용하는 것을 <code>boxed()</code>를 활용해 나타내는 것</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b84be6c5-efb7-4317-8679-6edcaabf491e/image.png" /></p>
<hr />
<h3 id="문자열을-split-하여-stream으로-생성">문자열을 split 하여 Stream으로 생성</h3>
<pre><code class="language-java">import java.util.regex.Pattern;
import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {

        Stream&lt;String&gt; splitStream = Pattern.compile(&quot;, &quot;).splitAsStream(&quot;html, css, javascript&quot;);
        splitStream.forEach(System.out::println);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8622fd3a-777d-46e1-95a2-b9c45973bebb/image.png" /></p>
<hr />
<h2 id="2-2-stream-가공">2-2. Stream 가공</h2>
<p>스트림에 있는 데이터에 대해 내가 원하는 결과를 만들기 위해서 중간 처리 작업을 가공이라고 한다.</p>
<p>스트림에서는 데이터를 가공할 수 있는 메소드들을 제공하는데, 해당 메소드들은 Stream을 전달받아 Stream을 반환하므로 연속해서 메소드를 연결할 수 있다. 또한 Stream 가공할 때에 필터-맵(filter-map) 기반 API를 사용하기 때문에 지연연산을 통해 성능 최적화가 가능하다.</p>
<table>
<thead>
<tr>
<th>상세 역할</th>
<th>메소드</th>
</tr>
</thead>
<tbody><tr>
<td>필터링</td>
<td>filter(), distinct()</td>
</tr>
<tr>
<td>변환</td>
<td>map(), flatMap()</td>
</tr>
<tr>
<td>제한</td>
<td>limit(), skip()</td>
</tr>
<tr>
<td>정렬</td>
<td>sorted()</td>
</tr>
<tr>
<td>결과 확인</td>
<td>peek()</td>
</tr>
</tbody></table>
<hr />
<h3 id="filter">Filter</h3>
<p>필터(filter)는 스트림에서 특정 데이터만을 걸러내기 위한 메소드이다.</p>
<ul>
<li>중계 연산 : Stream을 반환하며 Stream 관련 메소드 체이닝이 이어진다.</li>
</ul>
<pre><code class="language-java">import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        IntStream intStream = IntStream.range(0, 10);
        intStream.filter(i -&gt; i % 2 ==0)
                .forEach(i -&gt; System.out.print(i + &quot; &quot;));
    }
}</code></pre>
<p><code>filter()</code>에서 <code>i % 2 == 0</code>이 <code>true</code>인 것만 골라서 Stream으로 뱉어준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6cf95441-1cfe-4ef1-8a5d-53e943b696fe/image.png" /></p>
<hr />
<h3 id="map">map</h3>
<p>스트림에 들어 있는 데이터를 람다식으로 가공하고 새로운 스트림에 담아주는 메소드이다.</p>
<pre><code class="language-java">import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        IntStream intStream = IntStream.range(1, 10);
        intStream.filter(i -&gt; i % 2 != 0)
                .map(i -&gt; i * 5)
                .forEach(result -&gt; System.out.print(result + &quot; &quot;));
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f49bdb04-da60-4677-a0bb-7440888e3038/image.png" /></p>
<p>이번엔 홀수들만 골라서 map으로 그 골라진 홀수들을 5배해주는 것을 통해 가공한 뒤 새로운 스트림에 담아주는 것이다.</p>
<hr />
<p>다음은 번외 느낌으로 알아보자.</p>
<h3 id="flatmap">flatMap</h3>
<pre><code class="language-java">import java.util.Arrays;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;List&lt;String&gt;&gt; list = Arrays.asList(
                Arrays.asList(&quot;JAVA&quot;, &quot;SPRING&quot;, &quot;SPRINGBOOT&quot;),
                Arrays.asList(&quot;java&quot;, &quot;spring&quot;, &quot;springboot&quot;)
        );
        list.stream().forEach(System.out::println);
        System.out.println(&quot;list = &quot; + list);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7cf61f59-6049-41a6-a8b7-d8ab85d8dabc/image.png" /></p>
<p>위와 같은 리스트가 있다고 치자.</p>
<p><code>flatMap()</code> 는 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어준다. 이러한 작업을 플래트닝(flattening)이라고한다.</p>
<pre><code class="language-java">import java.util.Arrays;
import java.util.Collection;
import java.util.List;
import java.util.stream.Collectors;

public class Application {
    public static void main(String[] args) {
        List&lt;List&lt;String&gt;&gt; list = Arrays.asList(
                Arrays.asList(&quot;JAVA&quot;, &quot;SPRING&quot;, &quot;SPRINGBOOT&quot;),
                Arrays.asList(&quot;java&quot;, &quot;spring&quot;, &quot;springboot&quot;)
        );

        List&lt;String&gt; flatList = list.stream()
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
        flatList.stream().forEach(System.out::println);
        System.out.println(&quot;flatList = &quot; + flatList);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/74df0152-3f75-45b1-a0d5-c38447e0cbb6/image.png" /></p>
<hr />
<h3 id="sorted-메소드">sorted 메소드</h3>
<p>스트림에 있는 데이터를 정렬할 때 사용한다.</p>
<pre><code class="language-java">import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        List&lt;Integer&gt; integerList = IntStream.of(5, 10, 99, 2, 1, 35)
                .boxed()
                .sorted()
                .collect(Collectors.toList());
        System.out.println(&quot;정렬 된 integerList = &quot; + integerList);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/06dcccee-aa52-4300-ab1d-576a29f43d50/image.png" /></p>
<p>이번엔 Comparator를 활용한 내림차순을 만들어보자.</p>
<p><strong>DescInteger</strong></p>
<pre><code class="language-java">import java.util.Comparator;

public class DescInteger implements Comparator&lt;Integer&gt; {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2 - o1;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        List&lt;Integer&gt; integerList = IntStream.of(5, 10, 99, 2, 1, 35)
                .boxed()
//                .sorted()
                .sorted(new DescInteger())
                .collect(Collectors.toList());
        System.out.println(&quot;정렬 된 integerList = &quot; + integerList);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d5635614-266e-4775-bb44-79c80d6e9406/image.png" /></p>
<hr />
<h2 id="최종-연산-터미널">최종 연산 (터미널)</h2>
<p>최종 연산 : Stream이 아닌 값을 반환하며 Stream이 끝날 때 사용한다.</p>
<h3 id="calculation">Calculation</h3>
<p>최소/최대/총합/평균 등 과 같은 결과를 알아보자.</p>
<pre><code class="language-java">import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        long count = IntStream.range(1, 10).count();
        long sum = IntStream.range(1, 10).sum();

        System.out.println(&quot;count = &quot; + count);
        System.out.println(&quot;sum = &quot; + sum);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2a1cb845-220e-4565-864c-3534586d024c/image.png" /></p>
<pre><code class="language-java">import java.util.OptionalInt;
import java.util.stream.IntStream;

public class Application {
    public static void main(String[] args) {
        long count = IntStream.range(1, 10).count();
        long sum = IntStream.range(1, 10).sum();

        System.out.println(&quot;count = &quot; + count);
        System.out.println(&quot;sum = &quot; + sum);

        OptionalInt max = IntStream.range(1, 10).max();
        OptionalInt min = IntStream.range(1, 10).min();

        System.out.println(&quot;max = &quot; + max);
        System.out.println(&quot;min = &quot; + min);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9b62f1ac-d957-4be2-949e-56707be5a5b0/image.png" /></p>
<p><code>OptionalInt</code> : 기본자료형도 존재하지 않으면 empty의 개념을 추가하기 위한 타입</p>
<p>만약 위 코드에서 <code>OptionalInt max = IntStream.range(1,1).max();</code> 로 하게 되면
그 때 출력은 <code>OptionalInt.empty</code> 을 반환한다. (min도 마찬가지)</p>
<hr />
<h3 id="reduction">Reduction</h3>
<p><code>reduce()</code> 라는 메소드는 스트림에 있는 데이터들의 총합을 계산해준다.</p>
<pre><code class="language-java">import java.util.OptionalInt;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class Application {
    public static void main(String[] args) {

        /* 설명. 인자가 1개인 경우 */
        OptionalInt reduceOneParam = IntStream.range(1, 4)
                .reduce((a, b) -&gt; Integer.sum(a, b));

        System.out.println(&quot;reduceOneParam = &quot; + reduceOneParam.getAsInt());

        /* 설명. 인자가 2개인 경우 */
        int reduceTwoParam = IntStream.range(1, 4)
                .reduce(100, Integer::sum); // identity (100) 부터 누적 시작

        System.out.println(&quot;reduceTwoParam = &quot; + reduceTwoParam);

        /* 설명. 인자가 3개인 경우 */
        Integer reduceThreeParam = Stream.of(1,2,3,4,5,6,7,8,9,10)
                .reduce(100, Integer::sum, (x, y) -&gt; x + y);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7eb2b959-7602-4ff7-a100-9499b7da86f6/image.png" /></p>
<p>매개변수 3번째는 좀 더 효율적인 가산 누적연산을 위한 중간 합계 처리용 람다식을 작성한다.
(2번째 인자의 결과와 호환이 가능해야 한다.)</p>
<hr />
<h3 id="collecting">Collecting</h3>
<p><code>collect()</code> 는 Collector 객체에서 제공하는 정적(static) 메소드를 사용할 수 있고, 해당 메소드를 통해 컬렉션을 출력으로 받을 수 있다.</p>
<ol>
<li><strong>Collectors.toList()</strong>
스트림 작업 결과를 리스트로 반환해주는 메소드이다.</li>
</ol>
<ol start="2">
<li><strong>Collectors.joining()</strong>
스트림의 작업 결과를 String 타입으로 이어 붙인다.
3개의 인자를 받을 수 있다. 각 인자는 <code>delimiter</code>(구분자), <code>prefix</code>(맨 앞 문자), <code>suffix</code>(맨 뒤 문자) 이다.</li>
</ol>
<p><strong>Member</strong></p>
<pre><code class="language-java">public class Member {
    private String memberId;
    private String memberName;

    public Member() {
    }

    public String getMemberId() {
        return memberId;
    }

    public void setMemberId(String memberId) {
        this.memberId = memberId;
    }

    public String getMemberName() {
        return memberName;
    }

    public void setMemberName(String memberName) {
        this.memberName = memberName;
    }

    public Member(String memberId, String memberName) {
        this.memberId = memberId;
        this.memberName = memberName;
    }
}
</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Application {
    public static void main(String[] args) {
        List&lt;Member&gt; memberList = Arrays.asList(
                new Member(&quot;test01&quot;, &quot;testName01&quot;),
                new Member(&quot;test02&quot;, &quot;testName02&quot;),
                new Member(&quot;test03&quot;, &quot;testName03&quot;)
        );

        List&lt;String&gt; collectorCollection = memberList.stream()
                .map(Member::getMemberName)
                .collect(Collectors.toList());

        collectorCollection.forEach(System.out::println);

        String str = memberList.stream()
                .map(Member::getMemberName)
                .collect(Collectors.joining());
        System.out.println(&quot;str = &quot; + str);

        String str2 = memberList.stream()
                .map(Member::getMemberName)
                .collect(Collectors.joining(&quot;||&quot;, &quot;**&quot;, &quot;!!&quot;));
        System.out.println(&quot;str2 = &quot; + str2);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/29937bb5-b293-4602-9c73-e0e75ff5d303/image.png" /></p>
<hr />
<h3 id="matching">Matching</h3>
<p><code>Matching</code> 은 <code>Predicate</code> 를 인자로 받아 조건을 만족하는 엘리먼트가 있는지 확인하고 <code>boolean</code> 으로 리턴해준다.</p>
<p><code>anyMatch()</code> : 위의 요소 중 하나라도 만족하면 <code>true</code>
<code>allMatch()</code> : 위의 요소가 전부 다 만족하면 <code>true</code> (하나라도 만족 못 하면 <code>false</code>)
<code>noneMatch()</code> : 위의 요소 중 하나라도 만족하지 않으면 <code>true</code></p>
<pre><code class="language-java">import java.util.Arrays;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;String&gt; stringList = Arrays.asList(&quot;Java&quot;, &quot;Spring&quot;, &quot;SpringBoot&quot;);

        boolean anyMatch = stringList.stream()
                .anyMatch(str -&gt; str.contains(&quot;p&quot;));
        boolean allMatch = stringList.stream()
                .allMatch(str -&gt; str.length() &gt; 4);
        boolean noneMatch = stringList.stream()
                .noneMatch(str -&gt; str.contains(&quot;c&quot;));

        System.out.println(&quot;anyMatch = &quot; + anyMatch);
        System.out.println(&quot;allMatch = &quot; + allMatch);
        System.out.println(&quot;noneMatch = &quot; + noneMatch);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2bd6ee57-9a9e-46f1-b727-400bd10fe551/image.png" /></p>
<hr />
<p>더 있긴 하지만, 스트림에 대해서는 일단 이 정도로 정리를 마치겠다.</p>