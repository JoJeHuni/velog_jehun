<h1 id="heap">Heap</h1>
<blockquote>
<p>Heap (힙) : <strong>최솟값 또는 최댓값</strong>을 <strong>빠르게</strong> 찾아내기 위해 <strong>완전 이진 트리</strong> 형태로 만들어진 자료구조</p>
</blockquote>
<h2 id="트리">트리</h2>
<p>간단하게 트리부터 알고 가자
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d40b14d7-521c-40bd-8090-5d0de2b404de/image.png" /></p>
<ul>
<li><p><strong>루트 노드</strong> (root node) : 일명 뿌리 노드라고 하며 루트 노드는 하나의 트리에선 하나밖에 존재하지 않고, 부모노드가 없다. (이미지 상 A)</p>
</li>
<li><p><strong>부모 노드</strong>(parent node) : 자기 자신(노드)과 연결 된 노드 중 자신보다 높은 노드를 의미 (ex. C의 부모노드 : A)</p>
</li>
<li><p><strong>자식 노드</strong>(child node) : 자기 자신(노드)과 연결 된 노드 중 자신보다 낮은 노드를 의미 (ex. A의 자식노드 : C)</p>
</li>
<li><p><strong>단말 노드</strong>(leaf node) : 리프 노드라고도 불리며 자식 노드가 없는 노드를 의미한다. (이미지에서 H, I, E, F, G)</p>
</li>
<li><p><strong>내부 노드</strong>(internal node) : 단말 노드가 아닌 노드</p>
</li>
<li><p><strong>형제 노드</strong>(sibling node) : 부모가 같은 노드를 말한다. (ex. F, G는 모두 부모노드가 C이므로 F, G는 형제노드다.)</p>
</li>
<li><p><strong>깊이</strong>(depth) : 특정 노드에 도달하기 위해 거쳐가야 하는 '간선의 개수'를 의미 (ex. E의 깊이 : A→B→E 이므로 깊이는 2가 됨)</p>
</li>
<li><p><strong>레벨</strong>(level) : 특정 깊이에 있는 노드들의 집합을 말하며, 구현하는 사람에 따라 0 또는 1부터 시작한다.</p>
</li>
<li><p><strong>차수</strong>(degree) : 특정 노드가 하위(자식) 노드와 연결 된 개수 (ex. B의 차수 = 2 {D, E} )</p>
</li>
</ul>
<blockquote>
<p>부모 노드는 항상 자식 노드보다 우선순위가 높다.</p>
</blockquote>
<hr />
<h2 id="이진-트리">이진 트리</h2>
<blockquote>
<p>모든 노드의 최대 차수를 2로 제한한 것이다.</p>
</blockquote>
<p>위의 이미지는 한 부모 노드의 자식 노드는 2개씩 연결돼 최대 차수가 2인 이진 트리이다.</p>
<h2 id="완전-이진-트리">완전 이진 트리</h2>
<ol>
<li>마지막 레벨을 제외한 모든 노드가 채워져 있어야 한다.</li>
<li>모든 노드는 왼쪽부터 채워져 있어야 한다.</li>
</ol>
<p>2가지 조건이 충족돼야 한다.</p>
<p>(<strong>포화 이진 트리</strong> : 마지막 레벨을 제외한 모든 노드가 2개의 자식노드를 가져야 한다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/47816502-3756-42bf-9d8e-6a68ae34ce61/image.png" /></p>
<hr />
<h2 id="heap-종류">Heap 종류</h2>
<blockquote>
<ul>
<li>최소 힙</li>
<li>최대 힙</li>
</ul>
</blockquote>
<h3 id="최소-힙">최소 힙</h3>
<ul>
<li>부모 노드 값이 자식 노드의 값보다 작거나 같다.</li>
<li>루트 값은 저장된 원소 중 가장 작다.</li>
</ul>
<h3 id="최대-힙">최대 힙</h3>
<ul>
<li>부모 노드 값이 자식 노드의 값보다 크거나 같다.</li>
<li>루트 값은 저장된 원소 중 가장 크다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9213fde9-c760-44a2-9102-740b2886bae6/image.png" /></p>
<hr />
<h2 id="장단점">장단점</h2>
<h3 id="장점">장점</h3>
<ol>
<li><p><strong>빠른 삽입 및 삭제</strong> : Heap은 특정한 순서에 따라 정렬된 상태를 유지한다.
삽입과 삭제 시간이 ( O(log n) )에 이뤄진다.</p>
</li>
<li><p><strong>우선순위 기반 작업 처리</strong> : Heap은 우선순위 큐와 같은 다른 추상 자료형을 구현하는데 사용된다.</p>
</li>
</ol>
<ul>
<li>최대 힙의 경우) 가장 큰 우선순위를 가진 요소에 빠르게 접근 가능</li>
<li>최소 힙의 경우) 가장 작은 우선순위를 가진 요소에 빠르게 접근 가능</li>
</ul>
<h3 id="단점">단점</h3>
<ol>
<li><p><strong>임의 접근의 어려움</strong> : Heap은 완전 이진 트리의 형태로, 배열 or 연결 리스트를 사용해 구현한다. 특정 요소를 찾는데에는 ( O(n) ) 시간이 걸린다.</p>
</li>
<li><p><strong>정렬 유지의 오버헤드</strong> : Heap은 정렬된 상태를 유지하기 위해 삽입과 삭제 연산 시에 정렬을 조정해야 한다. -&gt; 오버헤드를 발생시킬 수 있다.</p>
</li>
<li><p><strong>배열로 구현 시 추가적 공간 요구</strong> : 배열 기반의 Heap은 공간을 미리 할당해야 하므로 요소의 개수에 따라 추가적인 공간을 필요로 한다.</p>
</li>
</ol>
<hr />
<h2 id="최소-heap-구현">최소 Heap 구현</h2>
<p>이는 PriorityQueue로 만든 것과 같다.</p>
<pre><code class="language-java">import java.util.PriorityQueue;
import java.util.Queue;

public class QueueApplication {
    public static void main(String[] args) {
        Queue queue = new PriorityQueue();

        queue.offer(500);
        queue.offer(35);
        queue.offer(77);
        queue.offer(1);

        System.out.println(queue);

        // 최소값 확인
        int minValue = minHeap.peek();
        System.out.println(&quot;Min value: &quot; + minValue); // Min value: 1

        // 최소값 삭제
        int deletedValue = minHeap.poll();
        System.out.println(&quot;Deleted value: &quot; + deletedValue); // Deleted value: 1

        // 최소값 확인
        minValue = minHeap.peek();
        System.out.println(&quot;Min value: &quot; + minValue); // Min value: 2
    }
}</code></pre>
<blockquote>
<p>최대 Heap은 아래 링크에서 확인하는것이 좋다고 본다.</p>
</blockquote>
<p><a href="https://velog.io/@jojehuni_9759/Java-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A442587-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4">코딩테스트 문제로 최대 힙</a></p>