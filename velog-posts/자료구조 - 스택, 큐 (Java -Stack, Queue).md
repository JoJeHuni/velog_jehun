<p>내가 아는 개념으로는</p>
<blockquote>
<p>스택 : 계속 데이터가 쌓이면서 최근에 들어온 데이터가 나가는 Last In First Out (LIFO)
큐 : 먼저 들어온게 먼저 나가는 Fist In First Out (FIFO)</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/de1c7ee5-03ad-45bf-877f-334f3436cb8a/image.png" /></p>
<p>이정도였다. 더 자세하게 정리하고자 한다.</p>
<p>Stack : 수식 계산, 워드프로세서의 undo / redo, 웹브라우저의 뒤로/ 앞으로 와 같은 기능을 구현할 때 활용
Queue : 최근 사용 문서, 인쇄 작업 대기목록, 버퍼(buffer) 등을 구현할 때 활용</p>
<blockquote>
<p>자바에서는 Stack을 클래스로 구현하여 제공하지만, Queue는 인터페이스만 있고 별도의 클래스가 없어서 구현한 클래스를 사용해야 한다.</p>
</blockquote>
<hr />
<h1 id="stack-method">Stack Method</h1>
<ul>
<li><p><code>empty()</code> : Stack이 비어 있는지 알려준다.</p>
<blockquote>
<p>boolean 값을 반환해준다. -&gt; 비어 있으면 true, 아니면 false</p>
</blockquote>
</li>
<li><p><code>push(Object ob)</code> : Stack에 객체(<code>ob</code>) 를 저장한다.</p>
</li>
<li><p><code>peek()</code> : Stack 의 맨 위에 저장된 객체를 반환한다.</p>
<blockquote>
<p>비어 있으면 <code>EmptyStackException</code> 발생
제거하는 것이 아닌, <strong>반환만 하는 것</strong>이다.</p>
</blockquote>
</li>
<li><p><code>pop()</code> : Stack의 맨 위에 있는 객체를 반환해주고 제거한다.</p>
<blockquote>
<p>비어 있으면 <code>EmptyStackException</code> 발생</p>
</blockquote>
</li>
<li><p><code>search(Object ob)</code> : Stack 에 주어진 객체(<code>ob</code>)를 찾아서 위치 반환</p>
<blockquote>
<p>못 찾으면 <code>-1</code> 을 반환한다.</p>
</blockquote>
</li>
</ul>
<blockquote>
<p><strong>스택은 배열과 달리 위치가 1부터 시작한다.</strong></p>
</blockquote>
<p><strong>예제 코드</strong></p>
<pre><code class="language-java">import java.util.Stack;

public class StackApplication {
    public static void main(String[] args) {
        Stack stack = new Stack();

        stack.push(&quot;100&quot;);
        stack.push(&quot;200&quot;);
        stack.push(&quot;300&quot;);

        int i = 1;
        while (!stack.empty()) {
            System.out.println(i + &quot;번째&quot;);
            System.out.println(&quot;100 search : &quot; + stack.search(&quot;100&quot;)); // &quot;100&quot; 찾기
            System.out.println(&quot;peek : &quot; + stack.peek()); // 맨 위에 있는거 반환
            System.out.println(&quot;pop : &quot; + stack.pop()); // 맨 위에 있는거 반환 후 제거
            i++;
        }

        System.out.println(stack.empty());
        System.out.println(&quot;End&quot;);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9333f12e-7077-4b65-91a9-1c1dedb55f8e/image.png" /></p>
<hr />
<h1 id="queue-method">Queue Method</h1>
<ul>
<li><p>추가, 삭제, 검사 순</p>
</li>
<li><p><code>add(Object ob)</code> : 지정된 객체를 Queue에 추가한다.</p>
<blockquote>
<p>성공하면 true 반환, 저장공간 부족 시 <code>IllegalStateException</code></p>
</blockquote>
</li>
<li><p><code>remove()</code> : Queue에서 객체를 꺼내 반환한다.</p>
<blockquote>
<p>비어있으면 <code>NoSuchElementException</code> 발생</p>
</blockquote>
</li>
<li><p><code>element()</code> : 삭제 없이 요소를 읽어온다.</p>
<blockquote>
<p>peek과 달리 Queue가 비어있으면 <code>NoSuchElementException</code> 발생</p>
</blockquote>
</li>
<li><p><code>offer(Object o)</code> : Queue에 객체를 저장한다.</p>
<blockquote>
<p>성공하면 <code>true</code>, 실패 시 <code>false</code></p>
</blockquote>
</li>
<li><p><code>poll()</code> : Queue에서 객체를 꺼내서 반환</p>
<blockquote>
<p>비어있으면 <code>null</code> 반환</p>
</blockquote>
</li>
<li><p><code>peek()</code> : 삭제 없이 요소를 읽어온다.</p>
<blockquote>
<p>비어 있으면 <code>null</code> 반환</p>
</blockquote>
</li>
</ul>
<p><strong>예제 코드</strong></p>
<pre><code class="language-java">import java.util.LinkedList;
import java.util.Queue;

public class QueueApplication {
    public static void main(String[] args) {
        Queue queue = new LinkedList();

        queue.offer(1);
        queue.offer(2);
        queue.offer(3);

        int i = 1;
        while (!queue.isEmpty()) {
            System.out.println(i + &quot;번째&quot;);
            System.out.println(&quot;1st element : &quot; + queue.element());
            System.out.println(&quot;element : &quot; + queue.element());
            System.out.println(&quot;remove : &quot; + queue.remove());
            i++;
        }

        System.out.println(queue.isEmpty());
        System.out.println(&quot;End&quot;);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7acce3e5-f611-4316-9dbc-f9754199fcb2/image.png" /></p>
<h1 id="priorityqueue">PriorityQueue</h1>
<p>Queue 인터페이스 구현체 중 하나로, 저장한 순서 상관없이 우선순위가 높은 것부터 꺼낼 수 있다.</p>
<p>PriorityQueue는 저장 공간으로 배열을 사용하며, 요소들을 heap 형태로 저장한다.</p>
<pre><code class="language-java">import java.util.PriorityQueue;
import java.util.Queue;

public class QueueApplication {
    public static void main(String[] args) {
        Queue queue = new PriorityQueue();

        queue.offer(500);
        queue.offer(35);
        queue.offer(77);

        System.out.println(queue);

        int i = 1;
        while (!queue.isEmpty()) {
            System.out.println(i + &quot;번째&quot;);
            System.out.println(&quot;1st element : &quot; + queue.element());
            System.out.println(&quot;element : &quot; + queue.element());
            System.out.println(&quot;remove : &quot; + queue.remove());
            i++;
        }

        System.out.println(queue.isEmpty());
        System.out.println(&quot;End&quot;);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cb721d3c-bbcb-47a2-b1d1-fa55c54a8e38/image.png" /></p>
<p>실행해보니 이상한 점을 발견했다.</p>
<p><code>queue.offer()</code> 후 출력했을 때는 <code>35, 500, 77</code> 순서로 우선순위가 되어 있는 것처럼 보이더니</p>
<p>막상 <code>element()</code>로 접근해보니 우선순위가 <code>35 -&gt; 77 -&gt; 500</code> 으로 뒤바뀐 것이었다.</p>
<p>왜 그런건지 알아보자.</p>
<p><code>PriorityQueue</code>는 내부적으로 <code>Heap</code> 자료구조를 사용해 우선순위를 관리한다.</p>
<p><code>Heap</code>은 완전 이진 트리로, 요소들이 특정 규칙에 따라 정렬이 되긴 하지만 <strong>항상 완전하게 정렬되는 것은 아니다.</strong></p>
<p><code>PriorityQueue</code>는 기본적으로 <strong>오름차순</strong>이고 <strong>최소 힙을 사용</strong>한다.</p>
<blockquote>
<p>내부 구조와의 관련이 있어서 최소 힙을 유지하지만, 직관적으로 오름차순 내림차순이 일치하지 않을 수도 있다고 한다.</p>
</blockquote>
<p><code>peek()</code>, <code>element()</code> 는 접근하는 요소가 항상 최소 요소이기 때문에 저렇게 우선순위가 보이지만, 반환할 때는 잘 보이고, 제거할 때도 최소 요소부터 제거되는 것이다.</p>
<hr />