<p>이전 ArrayList에 이어서 작성해본다.</p>
<p>체인 방식이라고 생각하면 된다. 한 쪽으로만 이어져 있는지 양 쪽으로 이어져 있는지에 따라 다르긴 하지만 예를 들어 설명해본다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2a24d79b-1e3c-4055-aecd-4aeeef7b7f04/image.png" /></p>
<p>위쪽을 보면 1000번 과 1200번이 한 쪽 방향으로 이어져 있는데</p>
<blockquote>
<p>만약 1200번이 없어지면?</p>
</blockquote>
<p>1000번은 바로 1240번에게 손을 뻗는 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e2eb76c0-c269-4349-a974-f49bafbd8172/image.png" /></p>
<h2 id="linked-list-특징">Linked List 특징</h2>
<ul>
<li><strong>데이터들이 연속된 공간에 저장되는 것이 아닌, 각 데이터끼리 링크를 연결해 구성하는 것</strong>이다.</li>
<li>데이터의 삽입, 삭제가 빈번할 경우 연결되는 링크 정보만 수정하면 되기 때문에 ArrayList보다 더 적합하다.</li>
<li>스택, 큐, 양방향 큐 등을 구성하기 용이하다.</li>
<li>LinkedList 에는 단일 연결 리스트와 이중 연결 리스트가 있다.<ul>
<li><strong>단일 연결 리스트</strong><ul>
<li>저장한 요소가 순서를 유지하지 않고 저장되지만 이러한 요소들 사이를 링크로 연결하여 구성하며 마치 연결된 리스트 형태인 것 처럼 만든 자료구조이다.</li>
<li>요소의 저장과 삭제 시 다음 요소를 가리키는 참조 링크만 변경하면 되기 때문에 요소의 저장과 삭제가 빈번히 일어나는 경우 ArrayList보다 성능 면에서 우수하다.</li>
</ul>
</li>
<li><strong>이중 연결 리스트</strong><ul>
<li>단일 연결 리스트는 다음 요소만 링크하는 반면 이중 연결 리스트는 이전 요소도 링크하여 이전 요소로 접근하기 쉽게 고안된 자료구조 이다.</li>
</ul>
</li>
</ul>
</li>
</ul>
<hr />
<pre><code class="language-java">import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

public class Application {
    public static void main(String[] args) {
//        List&lt;String&gt; arrList = new ArrayList&lt;&gt;();
        List&lt;String&gt; arrList = new LinkedList&lt;&gt;();
        arrList.add(&quot;apple&quot;);
        arrList.add(&quot;orange&quot;);
        arrList.add(&quot;banana&quot;);
        arrList.add(&quot;mango&quot;);
        arrList.add(&quot;grape&quot;);

        // 요소 수정
        arrList.set(1, &quot;pineapple&quot;);
        System.out.println(&quot;arrList = &quot; + arrList);

        // list가 관리하는 요소들 제거
        arrList.remove(2);
        System.out.println(&quot;arrList = &quot; + arrList);
        arrList.clear();
        System.out.println(&quot;arrList = &quot; + arrList);

        // 요소가 없는 list 계열인지 확인
        System.out.println(&quot;isEmpty = &quot; + arrList.isEmpty());
    }
}</code></pre>
<p>ArrayList로 해도 동작하긴 한다. 근데 LinkedList가 요소 추가, 삭제에 대해 많아질수록 더 효율적이어서 좋다.</p>
<p>LinkedList와 관련된 스택, 큐에 대해서는 아래 글에서 알아보자.</p>
<p><a href="https://velog.io/@jojehuni_9759/Java-%EC%8A%A4%ED%83%9D-%ED%81%90-Stack-Queue">자료구조 - 스택, 큐 (Java -Stack, Queue)</a></p>