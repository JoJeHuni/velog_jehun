<h1 id="컬렉션">컬렉션</h1>
<blockquote>
<p>컬렉션 : 많은 데이터들을 효과적으로 처리할 수 있는 방법을 제공하는 클래스들의 집합</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/21faa2ad-d5d7-48df-a4f3-50982ebf8fc5/image.png" /></p>
<h2 id="사용-이유">사용 이유</h2>
<ol>
<li><p>일관된 API
<code>Collection</code> 에서 제공하는 규격화된 메소드를 사용함으로 일관된 사용과 유지보수가 가능하다.</p>
</li>
<li><p>프로그래밍 비용 감소
이미 제공된 자료구조를 활용하는 것으로 low-level의 알고리즘을 고민할 시간과 노력을 아낄 수 있다.</p>
</li>
<li><p>프로그래밍 속도 및 품질 향상+
필요한 자료구조를 사용함으로써 프로그래밍의 속도 뿐만 아니라 기동 속도, 품질 향상을 기대할 수 있다.</p>
</li>
</ol>
<hr />
<h2 id="주요-인터페이스와-특징">주요 인터페이스와 특징</h2>
<table>
<thead>
<tr>
<th>인터페이스</th>
<th>설명</th>
<th>구현 클래스</th>
</tr>
</thead>
<tbody><tr>
<td>List</td>
<td>순서가 있는 데이터의 집합으로, 데이터의 중복을 허용한다.</td>
<td>ArrayList, LinkedList, Stack, Queue, Vector</td>
</tr>
<tr>
<td>Set</td>
<td>순서가 없는 데이터의 집합으로, 데이터 중복 허용하지 않는다.</td>
<td>HashSet, TreeSet</td>
</tr>
<tr>
<td>Map&lt;K, V&gt;</td>
<td>키와 값이 쌍를 이루어 구성되는 데이터 집합으로 순서가 없다. 키의 중복은 허용되지 않지만, 값의 중복은 허용된다.</td>
<td>HashMap, TreeMap, Properties</td>
</tr>
</tbody></table>
<hr />
<h1 id="list">List</h1>
<p>순서가 있는 데이터의 집합으로 같은 데이터의 중복 저장을 허용</p>
<ul>
<li>ArrayList</li>
<li>LinkedList</li>
<li>Vector</li>
<li>Stack</li>
</ul>
<p>위와 같은 것들이 있다.</p>
<h2 id="list-특징">List 특징</h2>
<ul>
<li>List 인터페이스를 구현한 모든 클래스는 저장 순서가 유지된다.</li>
<li>List 계열의 클래스는 중복 저장을 허용한다.</li>
</ul>
<hr />
<h2 id="arraylist">ArrayList</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0532806f-ce8a-4003-9e3f-85302d1c1c83/image.png" /></p>
<h3 id="arratlist-특징">ArratList 특징</h3>
<ul>
<li>ArrayList는 인스턴스를 생성하게 되면 내부적으로 10칸짜리 배열을 생성해서 관리한다.</li>
<li>배열의 단점을 보완하기 위해 만들어졌기 때문에 크기 변경, 요소 추가/삭제/정렬 기능들을 메소드로 제공하고 있다.</li>
<li>자동적으로 수행되는 것이지 속도가 빨라지는 것은 아니다.</li>
<li>ArrayList는 스레드간 동기화가 지원되지 않는다. 따라서 다수의 스레드가 동시에 접근하여 데이터를 조작하게 될 경우 데이터 훼손이 일어날 수 있다.</li>
<li>ArrayList는 인덱스로 데이터에 접근할 수 있기 때문에 조회 기능적으로 뛰어나다.</li>
</ul>
<p>간단한 메소드도 함께 알아보자.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        // 1. List
        List list = new ArrayList&lt;&gt;(); // 자료형을 안 적으면 Object가 적힌거랑 같다.
        // List로 선언하는 이유 : List 계열의 다른 인스턴스로 바뀌어도 영향을 주지 않기 위해 다형석 적용한 채로 사용한다.

        list.add(&quot;apple&quot;);
        list.add(123);
        list.add(45.67);
        list.add(new java.util.Date());

        // 컬렉션들은 toString()이 잘 오버라이드 되어 있어 출력하기 편함
        System.out.println(&quot;list 한 번에 출력 : &quot; + list);

        // 1. 특정 인덱스 값 반환
        // 배열에서는 arr[0] 처럼 했다면 List에서는 get() 메소드를 사용한다.
        System.out.println(&quot;list.get(0) : &quot; + list.get(0));
        System.out.println(&quot;list.get(2) : &quot; + list.get(2));

        // 2. 데이터의 크기 반환
        // 배열에서는 length, length()를 사용했다면 List에서는 size() 메소드 사용
        System.out.println(&quot;list에 담긴 데이터의 크기 : &quot; + list.size());

        // list에 있는 데이터 전체 확인해보는 방법
        for (int i = 0; i &lt; list.size(); i++) {
            list.get(i);
        }

        // 3. ArrayList는 중복 허용이어서 동등값이 허용된다.
        list.add(&quot;apple&quot;);
        list.add(123);
        // 실행결과 : [apple, 123, 45.67, Tue Jul 23 11:42:20 KST 2024, apple, 123]

        // 4. 특정 인덱스의 값을 수정하기 위해서는 set() 사용
        list.set(0, 777);
        // 실행결과 : [777, 123, 45.67, Tue Jul 23 11:43:45 KST 2024, apple, 123]

        // 5. 특정 인덱스 값 삭제를 위해서는 remove() 사용 -&gt; size도 줄어든다.
        list.remove(0);
        System.out.println(list);
        // 실행결과 : [123, 45.67, Tue Jul 23 11:44:40 KST 2024, apple, 123]

        // + null도 add가 가능하다 (중간에 끼워넣고 싶으면 매개변수 2개 사용 add.(인덱스, 값))
        list.add(0, null);
        System.out.println(list);
        // 실행결과 : [null, 123, 45.67, Tue Jul 23 11:46:24 KST 2024, apple, 123]
        // 7. list 안에 특정한 값을 가지고 있는지 확인하기 위해서는 contains() 사용
        System.out.println(list.contains(null));
        // 실행결과 : true
    }
}</code></pre>
<blockquote>
<p><strong>배열보다 ArrayList가 나은 점</strong></p>
</blockquote>
<ol>
<li>처음부터 크기를 할당해줄 필요가 없다.</li>
<li>중간에 값을 추가하거나 삭제하기가 상대적으로 용이하다.</li>
</ol>
<hr />
<h3 id="배열과-arraylist-값-추가방식-차이">배열과 ArrayList 값 추가방식 차이</h3>
<h4 id="배열일-때">배열일 때</h4>
<pre><code class="language-java">import java.util.Arrays;

public class Application {
    public static void main(String[] args) {
        int[] intArr = new int[5];
        int num = 0;
        for (int i = 0; i &lt; intArr.length; i++) {
            intArr[i] = ++num;
        }

        System.out.println(Arrays.toString(intArr));

        int[] newArr = new int[intArr.length +1];
        System.arraycopy(intArr, 0, newArr, 0, intArr.length);

        System.out.println(Arrays.toString(newArr));

        // 2번 인덱스에 7 이라는 값 끼워넣기
        for (int i = intArr.length - 1; i &gt; 1; i--) {
            newArr[i + 1] = newArr[i];
        }
        newArr[2] = 7;

        System.out.println(Arrays.toString(newArr));
    }
}</code></pre>
<p><strong>실행결과</strong></p>
<pre><code>[1, 2, 3, 4, 5]
[1, 2, 3, 4, 5, 0]
[1, 2, 7, 3, 4, 5]</code></pre><hr />
<h4 id="리스트일-때">리스트일 때</h4>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;Integer&gt; intList = new ArrayList&lt;&gt;();
        for (int i = 0; i &lt; 5; i++) {
            intList.add(i + 1);
        }
        System.out.println(intList);

        intList.add(2,7);
        System.out.println(intList);
    }
}</code></pre>
<p><strong>실행결과</strong></p>
<pre><code>[1, 2, 3, 4, 5]
[1, 2, 7, 3, 4, 5]</code></pre><hr />
<h3 id="arraylist-추가적인-메소드">ArrayList 추가적인 메소드</h3>
<h4 id="정렬">정렬</h4>
<ol>
<li>Collections.sort() : 오름차순 정렬</li>
<li>Collections.reverse() : 리스트 역순 (sort 후 사용하면 내림차순)<pre><code class="language-java">import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
</code></pre>
</li>
</ol>
<p>public class Application {
    public static void main(String[] args) {
        List stringList = new ArrayList&lt;&gt;();
        stringList.add(&quot;apple&quot;);
        stringList.add(&quot;orange&quot;);
        stringList.add(&quot;banana&quot;);
        stringList.add(&quot;mango&quot;);
        stringList.add(&quot;grape&quot;);</p>
<pre><code>    System.out.println(&quot;초기화 된 stringList = &quot; + stringList);

    Collections.sort(stringList);
    System.out.println(&quot;오름차순 정렬 후 stringList = &quot; + stringList);

    Collections.reverse(stringList); // 오름차순 후에 역순을 해주는 것
    // Collections.sort(stringList, Collections.reverseOrder()); // 이건 바로 내림차순정렬이다.
    System.out.println(&quot;내림차순 정렬은 오름차순 후 reverse -&gt; stringList = &quot; + stringList);

    // 또다른 내림차순 방법
    /* Iterator(반복자)는 hasNext()와 next()를 활용해 들어있는 data를 반복시킬 수 있는 타입 */
    /* 다루는 Iterator와 관련된 컬렉션의 제네릭 타입과 일치하는 제네릭 타입을 꼭 써줘야 한다. (다운캐스팅에 대한 고민이 없도록) */
    stringList = new LinkedList&lt;&gt;(stringList); // LinkedList 개념으로 바꾸기
    Iterator&lt;String&gt; stringIterator = ((LinkedList&lt;String&gt;) stringList).descendingIterator();

    while (stringIterator.hasNext()) {
        System.out.print(stringIterator.next() + &quot; &quot;);
    }
    // 다시 오름차순으로 바뀐다.


}</code></pre><p>}</p>
<pre><code>
![](https://velog.velcdn.com/images/jojehuni_9759/post/8b7c43e5-46ec-41fb-bd16-4761c2d610d0/image.png)

&gt; ArrayList는 안에 들어있는 데이터 타입에 따라 정의된 기준대로 정렬을 한다.
그래서 String에 문자열 오름차순에 대한 정의가 있어서 그거대로 정렬을 한 것.
String 처럼 Wrapper 클래스는 전부 다 정의돼 있다.

---
#### Comparable, Comparator을 활용한 정렬

우리가 원하는 필드의 오름차순, 내림차순을 할 수 있다.
필드가 n개면 총 (n * 2) 가지의 정렬기준을 가질 수 있다.. 오름차순 내림차순 2개씩
정렬은 compareTo() 메소드가 반환하는 int형의 부호에 따라 정해지게 된다.
(해당 필드가 String형일 경우 String의 compareTo() 메소드를 활용하자)

BookDTO
```java
public class BookDTO implements Comparable&lt;BookDTO&gt; {
    private int number;
    private String title;
    private String author;
    private int price;

    public BookDTO() {
    }

    public int getNumber() {
        return number;
    }

    public void setNumber(int number) {
        this.number = number;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public BookDTO(int number, String title, String author, int price) {
        this.number = number;
        this.title = title;
        this.author = author;
        this.price = price;
    }

    @Override
    public String toString() {
        return &quot;BookDTO{&quot; +
                &quot;number=&quot; + number +
                &quot;, title='&quot; + title + '\'' +
                &quot;, author='&quot; + author + '\'' +
                &quot;, price=&quot; + price +
                '}';
    }

    @Override
    public int compareTo(BookDTO o) {
        // 가격에 대한 오름차순
//        return this.price - o.getPrice();

        // 가격에 대한 내림차순
//        return o.getPrice() - this.price;

        // 책 제목에 대한 오름차순
//        return this.title.compareTo(o.getTitle());

        // 책 제목에 대한 내림차순
        return -this.title.compareTo(o.getTitle());
    }
}</code></pre><p>Comparable 인터페이스를 구현하게끔 하는건데 BookDTO 타입으로 제네릭해주지 않으면 Object로 들어가서 다운캐스팅으로 인한 문제가 생길 수 있기 때문에 적어줘야 한다.</p>
<p>주석처리 된 가격 오름, 내림차순 / 제목 오름, 내림차순에 대한 것들을 꼭 실행해보는 것이 좋다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import {패키지 구조}.BookDTO;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;BookDTO&gt; bookList = new ArrayList&lt;&gt;();
        bookList.add(new BookDTO(1, &quot;홍길동전&quot;, &quot;허균&quot;, 50000));
        bookList.add(new BookDTO(2, &quot;목민심서&quot;, &quot;정약용&quot;, 30000));
        bookList.add(new BookDTO(3, &quot;동의보감&quot;, &quot;허준&quot;, 40000));
        bookList.add(new BookDTO(4, &quot;삼국사기&quot;, &quot;김부식&quot;, 46000));
        bookList.add(new BookDTO(5, &quot;삼국유사&quot;, &quot;일연&quot;, 58000));

        Collections.sort(bookList);

        for(int i = 0; i &lt; bookList.size(); i++) {
            System.out.println(bookList.get(i));
        }
    }
}</code></pre>
<p>이번엔 Comparable이 아닌 Comparator를 사용해보자.</p>
<hr />
<p><strong>AscendingPrice</strong></p>
<pre><code class="language-java">import {패키지구조}.BookDTO;

import java.util.Comparator;

public class AscendingPrice implements Comparator&lt;BookDTO&gt; {

    // 가격 오름차순이 가능하도록 compare() 메소드 오버라이딩
    @Override
    public int compare(BookDTO o1, BookDTO o2) {
        return o1.getPrice() - o2.getPrice();
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import {패키지구조}.AscendingPrice;
import {패키지구조}.BookDTO;

import java.util.ArrayList;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;BookDTO&gt; bookList = new ArrayList&lt;&gt;();
        bookList.add(new BookDTO(1, &quot;홍길동전&quot;, &quot;허균&quot;, 50000));
        bookList.add(new BookDTO(2, &quot;목민심서&quot;, &quot;정약용&quot;, 30000));
        bookList.add(new BookDTO(3, &quot;동의보감&quot;, &quot;허준&quot;, 40000));
        bookList.add(new BookDTO(4, &quot;삼국사기&quot;, &quot;김부식&quot;, 46000));
        bookList.add(new BookDTO(5, &quot;삼국유사&quot;, &quot;일연&quot;, 58000));

        // Comparable 방식
//        Collections.sort(bookList);

        // Comparator 방식
//        Collections.sort(bookList, new AscendingPrice());
        bookList.sort(new AscendingPrice());

        for(int i = 0; i &lt; bookList.size(); i++) {
            System.out.println(bookList.get(i));
        }
    }
}
</code></pre>
<p><strong>실행결과</strong></p>
<pre><code>BookDTO{number=2, title='목민심서', author='정약용', price=30000}
BookDTO{number=3, title='동의보감', author='허준', price=40000}
BookDTO{number=4, title='삼국사기', author='김부식', price=46000}
BookDTO{number=1, title='홍길동전', author='허균', price=50000}
BookDTO{number=5, title='삼국유사', author='일연', price=58000}</code></pre><p>만약 위와 같이 나오는 것이 아니라, 주소값이 반환된다면 BookDTO에 toString을 추가해주자.</p>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/043ef1b8-1729-4e96-baf7-b583d5ebb02e/image.png" /></p>
<hr />
<p>이번엔 List 계열을 출력하는 4가지 방법에 대해 알아본다.</p>
<p>주석으로 적어두고 만들어뒀다.
<strong>Application</strong></p>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        List&lt;String&gt; arrList = new ArrayList&lt;&gt;();
        arrList.add(&quot;apple&quot;);
        arrList.add(&quot;orange&quot;);
        arrList.add(&quot;banana&quot;);
        arrList.add(&quot;mango&quot;);
        arrList.add(&quot;grape&quot;);

        /* 설명 1. toString() 활용하기 */
        System.out.println(&quot;arrList = &quot; + arrList);

        /* 설명 2. for문 활용하기 */
        for (int i = 0; i &lt; arrList.size(); i++) {
            System.out.println(arrList.get(i));
        }

        /* 설명 3. for-each문 활용하기 */
        for(String str : arrList) {
            System.out.println(str);
        }

        /* 설명 4. iterator 활용하기 */
        Iterator&lt;String&gt; iterator = arrList.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}</code></pre>