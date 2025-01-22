<h1 id="선형-자료-구조">선형 자료 구조</h1>
<p>요소가 일렬로 나열돼 있는 자료구조로</p>
<ul>
<li>연결 리스트</li>
<li>배열</li>
<li>벡터</li>
<li>스택</li>
<li>큐</li>
</ul>
<p>에 대해 알아보자.</p>
<h2 id="연결-리스트">연결 리스트</h2>
<p>이전에 Java의 컬렉션을 정리하면서 리스트 관련은 정리한 것이 있다.</p>
<p><a href="https://velog.io/@jojehuni_9759/Java-Collection-%EC%BB%AC%EB%A0%89%EC%85%98">Java - Collection (List - ArrayList)</a></p>
<p><a href="https://velog.io/@jojehuni_9759/Java-Collection-List-LinkedList">Java - Collection (List - LinkedList)</a></p>
<blockquote>
<p><strong>연결 리스트</strong></p>
</blockquote>
<ul>
<li>싱글 연결 리스트 : next 포인터만 가지는 것</li>
<li>이중 연결 리스트 : next, prev 포인터를 가지는 것</li>
<li>원형 이중 연결 리스트 : 이중 연결 리스트와 같은데 추가로 마지막 노드의 next 포인터가 헤드 노드를 가리키는 것</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8fb69e7c-39df-4c5d-9c39-f91652f26851/image.png" /></p>
<blockquote>
<p>연결 리스트의 함수</p>
</blockquote>
<ul>
<li><code>push_front()</code>: 앞에서부터 요소 추가</li>
<li><code>push_back()</code>: 뒤에서부터 요소 추가</li>
<li><code>insert()</code>: 중간에 요소 추가</li>
</ul>
<hr />
<h2 id="배열">배열</h2>
<p>같은 타입의 변수, 지정된 크기, 인접한 메모리 위치에 있는 데이터를 모아둔 집합이다.
중복을 허용하며 순서가 있다.</p>
<p>정적 배열을 기반으로 알아보자.
우선 배열의 접근에는 $O(1)$의 복잡도를 가지고 랜덤 접근이 가능하다.
삽입과 삭제에는 $O(n)$이 걸린다.</p>
<p>그래서 추가/삭제 시에는 연결 리스트를 사용하고, 접근(참조)을 하는 것은 배열이 좋다.
그런 이유는 아래 비교에서 알아보자.</p>
<blockquote>
<p><strong>랜덤 접근 (random access)</strong>
순차적인 데이터가 있을 때 임의의 인덱스에 해당하는 데이터에 접근할 수 있는 기능</p>
<p><strong>순차적 접근</strong>
저장된 순서대로 데이터를 접근해야 함</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f03d92ab-f028-49f4-abf6-0fcf4036b5da/image.png" /></p>
</blockquote>
<hr />
<h3 id="배열-연결-리스트-비교">배열, 연결 리스트 비교</h3>
<ul>
<li>배열은 인덱스 번호만 알아도 해당 값을 얻을 수 있다.</li>
<li>연결 리스트는 순차 접근이 불가능하여 찾고자 하는 해당 위치까지 탐색을 해야 한다.</li>
</ul>
<blockquote>
<p>즉, 배열은 참조나 탐색하기엔 연결리스트보다 빠르다.
하지만 삽입, 삭제에서는 연결리스트에서는 next, prev 선만 변경하면 가능하기 때문에 연결리스트를 선택하는 것이 좋다.</p>
</blockquote>
<hr />
<h2 id="벡터">벡터</h2>
<blockquote>
<p>벡터 : 동적으로 요소 할당 가능한 <strong>동적 배열</strong>이다.</p>
</blockquote>
<ul>
<li>파일 시점에 요소의 개수를 모른다면 벡터 사용해야 한다.</li>
<li>중복 허용, 순서 있음, 랜덤 접근 가능</li>
</ul>
<p><strong>탐색</strong>하는 것과 <strong>맨 뒤의 요소에 삽입/삭제하는 것</strong>에는 $O(1)$이 걸리고
그렇지 않은 요소에 삽입/삭제 하는 것은 $O(n)$의 시간이 걸린다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/565f5de3-c4f9-47ac-b8a2-c244964f2d1c/image.png" /></p>
<p>2의 제곱승 + 1 (3, 5, 9 ...) 일 때마다 크기를 2배로 늘리는 것을 알 수 있다.</p>
<blockquote>
<p>벡터의 함수</p>
</blockquote>
<ul>
<li><code>push_back()</code>: 뒤에서부터 요소 추가</li>
<li><code>pop_back()</code>: 맨 뒤부터 요소 삭제</li>
<li><code>erase()</code>: 요소 삭제</li>
<li><code>find()</code>: 요소 탐색</li>
<li><code>clear()</code>: 배열 초기화</li>
</ul>
<hr />
<h2 id="스택-큐">스택, 큐</h2>
<p>스택과 큐는 이전에 정리했던 것을 봐도 충분할 것이다.</p>
<p><a href="https://velog.io/@jojehuni_9759/Java-%EC%8A%A4%ED%83%9D-%ED%81%90-Stack-Queue">스택, 큐</a></p>
<p>다음은 비선형 자료 구조에 대해 알아보자.</p>