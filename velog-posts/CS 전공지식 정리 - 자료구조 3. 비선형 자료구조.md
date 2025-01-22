<h2 id="비선형-자료구조">비선형 자료구조</h2>
<blockquote>
<p><strong>비선형 자료구조</strong> : 일렬로 나열하지 않고 자료 순서, 관계가 복잡한 구조</p>
</blockquote>
<h3 id="그래프">그래프</h3>
<blockquote>
<p><strong>그래프</strong> : 정점 (vertex) 와 간선 (edge)로 이루어진 자료구조</p>
</blockquote>
<h4 id="가중치">가중치</h4>
<blockquote>
<p>가중치 : 정점과 간선 사이에 드는 비용</p>
</blockquote>
<p>그럼 그래프에 속하는 것 중 하나인 트리에 대해 알아보자.</p>
<hr />
<h2 id="트리">트리</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6e2e8da9-edaa-45e1-9e1a-863024b733cb/image.png" /></p>
<ul>
<li><strong>루트 노드</strong> : 가장 위에 있는 노드</li>
<li><strong>내부 노드</strong> : 루트 노드와 리프 노드를 제외한 노드</li>
<li><strong>리프 노드</strong> : 자식 노드가 없는 노드</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4d1d79a6-3c57-4bdc-9408-8e0a3ab16f74/image.png" /></p>
<p><strong>깊이</strong> : 루트 노드 ~ 특정 노드까지 최단 거리로 갔을 때의 거리</p>
<ul>
<li>각 노드마다 트리의 깊이가 다르다.</li>
</ul>
<p><strong>레벨</strong> : 보통 '깊이'와 같은 의미를 가진다.</p>
<ul>
<li>1번 노드가 0레벨 =&gt; 2번, 3번 노드는 1레벨</li>
</ul>
<p><strong>높이</strong> : 루트 노드 ~ 리프 노드까지의 거리 중 가장 긴 거리</p>
<p><strong>서브트리</strong> : 트리 내의 하위 집합 (부분 집합)</p>
<hr />
<h3 id="이진-트리">이진 트리</h3>
<blockquote>
<p>이진 트리 : 자식 노드의 수가 2개 이하인 트리</p>
</blockquote>
<h4 id="이진-트리의-종류">이진 트리의 종류</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/97b0874b-6c03-4d55-b52f-e770c7c05013/image.png" /></p>
<ul>
<li><p><strong>정이진 트리</strong> (full binary tree): 자식 노드가 0 또는 2개인 이진 트리를 의미한다.</p>
</li>
<li><p><strong>완전 이진 트리</strong> (complete binary tree): 왼쪽에서부터 채워져 있는 이진 트리를 의미한다.</p>
<ul>
<li>마지막 레벨을 제외하고는 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 경우 왼쪽부터 채워져 있다.</li>
</ul>
</li>
<li><p><strong>변질 이진 트리</strong> (degenerate binary tree): 자식 노드가 하나밖에 없는 이진 트리를 의미한다.</p>
</li>
<li><p><strong>포화 이진 트리</strong> (perfect binary tree): 모든 노드가 꽉 차 있는 이진 트리를 의미한다.</p>
</li>
<li><p><strong>균형 이진 트리</strong> (balanced binary tree): 왼쪽과 오른쪽 노드의 높이 차이가 1 이하인 이진 트리를 의미한다.</p>
<ul>
<li><code>map</code>, <code>set</code>을 구성하는 레드 블랙 트리는 균형 이진 트리 중 하나입니다.</li>
</ul>
</li>
</ul>
<hr />
<h3 id="이진-탐색-트리-bst-binary-search-tree">이진 탐색 트리 (BST, Binary Search Tree)</h3>
<blockquote>
<p><strong>이진 탐색 트리</strong> : 노드의 오른쪽 하위 트리에는 '노드 값보다 큰 값'만 들어 있고, 왼쪽 하위 트리에는 '노드 값보다 작은 값'만 들어 있는 트리</p>
</blockquote>
<p>그래서 검색할 때 용이하다</p>
<p>요소에 접근 시 평균적으로는 $O(logn)$ 소요, 최악의 경우에는 $O(n)$ 소요하며, 삽입 순서에 따라 구조가 선형적일 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e5b2fd21-3953-4880-89c2-be1b24a39a45/image.png" /></p>
<hr />
<h3 id="avl-트리-adelson-velsky-and-landis-tree">AVL 트리 (Adelson-Velsky and Landis tree)</h3>
<blockquote>
<p><strong>AVL 트리</strong> : 선형적인 트리가 되는 최악의 경우를 방지하고, 스스로 균형을 잡는 이진 탐색 트리</p>
</blockquote>
<p>두 자식 서브트리의 높이 =&gt; 항상 최대 1만큼 차이난다는 특징이 있다.</p>
<p>삽입, 삭제 시마다 균형을 맞추기 위해 트리의 일부를 왼쪽 또는 오른쪽으로 회전시킨다.</p>
<ul>
<li>탐색, 삽입, 삭제 =&gt; $O(logn)$ 소요</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9a148c4a-87b3-4f83-b939-7df24d7a73bd/image.png" /></p>
<hr />
<h3 id="레드-블랙-트리">레드 블랙 트리</h3>
<blockquote>
<p><strong>레드 블랙 트리</strong> : 균형 이진 탐색 트리로 탐색, 삽입, 삭제 모두 시간 복잡도가 $O(logn)$</p>
</blockquote>
<p>각 노드에 빨간색 또는 검은색의 색을 저장하는 추가 비트를 저장하고
이는 곧 삽입, 삭제 중 트리가 균형을 유지하는데 사용된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d2e55e3d-b990-4dd2-923d-e8b70a3186c1/image.png" /></p>
<hr />
<h2 id="힙-우선순위-큐">힙, 우선순위 큐</h2>
<p>힙과 우선순위 큐에 대해서는 정리된 것을 보자.</p>
<p><a href="https://velog.io/@jojehuni_9759/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%9E%99-heap-Java">자료구조 - 힙 heap (Java)</a></p>
<p><a href="https://velog.io/@jojehuni_9759/Java-%EC%8A%A4%ED%83%9D-%ED%81%90-Stack-Queue">우선순위 큐</a></p>
<p>우선순위 큐에 대해서는 아래로 내려보면 정리를 해뒀다.</p>
<hr />
<h2 id="맵-해시-테이블">맵, 해시 테이블</h2>
<p><a href="https://velog.io/@jojehuni_9759/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Hash-Java-HashMap">Hash, HashTable</a></p>
<blockquote>
<p><strong>맵</strong> : 특정 순서에 따라 키에 매핑된 값의 조합 (키-값) 으로 구성된 자료구조</p>
<ul>
<li>레드 블랙 트리 자료구조 기반으로 형성된다.</li>
<li>삽입 시 자동으로 정렬된다.</li>
<li>해시 테이블 구현 시 사용한다.<ul>
<li><code>unordered_map</code> : 정렬 보장 X</li>
<li><code>map</code> : 정렬 보장 O</li>
</ul>
</li>
</ul>
</blockquote>
<p><code>map&lt;string, int&gt;</code> 형태로 구현하며 아래와 같은 기능들이 있는데 </p>
<ul>
<li><code>clear()</code> : 맵에 있는 모든 요소 삭제</li>
<li><code>size()</code> : map의 크기 구함</li>
<li><code>erase()</code> : 해당 키와 키에 매핑된 값 삭제</li>
</ul>
<p>Java에서의 <code>HashMap</code> 메서드도 위 링크에 정리를 해놨다.</p>
<hr />
<blockquote>
<p>해시 테이블 : 무한에 가까운 데이터를 유한한 개수의 해시 값으로 매핑한 테이블
<code>unordered_map</code>으로 구현</p>
</blockquote>
<p>삽입, 삭제, 탐색 =&gt; 평균적으로 $O(1)$ 소요</p>
<hr />
<h2 id="셋-set">셋 (set)</h2>
<blockquote>
<p><strong>Set</strong> : 특정 순서에 따라 고유한 요소 저장하는 컨테이너</p>
</blockquote>
<p>중복되는 요소 없이 <code>unique</code> 값만 저장한다.</p>