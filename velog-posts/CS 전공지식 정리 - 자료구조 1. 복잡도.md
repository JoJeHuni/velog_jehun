<h1 id="자료구조">자료구조</h1>
<p>자료구조란, 효율적으로 데이터를 관리하고 수정, 삭제, 탐색, 저장할 수 있는 데이터 집합이다.</p>
<h2 id="복잡도">복잡도</h2>
<h3 id="시간-복잡도">시간 복잡도</h3>
<blockquote>
<p><strong>시간 복잡도</strong> : 입력 크기에 대해 어떠한 알고리즘이 실행되는데 걸리는 시간
주요 로직의 반복 횟수를 중점으로 측정된다. 보통 <strong>빅오 표기법</strong>으로 표현</p>
</blockquote>
<h4 id="빅오-표기법">빅오 표기법</h4>
<blockquote>
<p>빅오 표기법 : 입력 범위 n을 기준으로 해서 로직이 몇 번 반복되는지 나타내는 것</p>
</blockquote>
<p>입력 크기가 커질 수록 연산량이 가장 많이 커지는 항 (ex. $n^2$ 항) 과 달리 다른 것은 연산량의 증가가 미미하기 때문에 그것만 신경쓰면 된다.</p>
<p><strong>시간 복잡도가 존재하는 이유</strong>
시간 복잡도를 더욱 효율적인 코드로 개선할 수 있는 척도로 이용
소요 시간: $O(n^2)$ &gt; $O(n)$ &gt; $O(1)$</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8ddaa5bc-f470-472b-b619-7d84d51fefaa/image.png" /></p>
<p>즉, $O(n^2)$ 보다는 $O(n)$, $O(n)$보다도 $O(1)$을 지향해야 한다.</p>
<hr />
<h3 id="자료구조에서의-시간-복잡도">자료구조에서의 시간 복잡도</h3>
<p>거의 평균 시간 복잡도와 최악의 시간 복잡도를 고려하면서 쓴다.</p>
<h4 id="1-평균-시간-복잡도">1) 평균 시간 복잡도</h4>
<table>
<thead>
<tr>
<th>자료 구조</th>
<th>접근</th>
<th>탐색</th>
<th>삽입</th>
<th>삭제</th>
</tr>
</thead>
<tbody><tr>
<td>배열</td>
<td>O(1)</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n)</td>
</tr>
<tr>
<td>스택</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>큐</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>이중 연결 리스트</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>해시 테이블</td>
<td>O(1)</td>
<td>O(1)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>이진 탐색 트리</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
</tr>
<tr>
<td>AVL 트리</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
</tr>
<tr>
<td>레드 블랙 트리</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
</tr>
</tbody></table>
<h4 id="2-최악의-시간-복잡도">2) 최악의 시간 복잡도</h4>
<table>
<thead>
<tr>
<th>자료 구조</th>
<th>접근</th>
<th>탐색</th>
<th>삽입</th>
<th>삭제</th>
</tr>
</thead>
<tbody><tr>
<td>배열</td>
<td>O(1)</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n)</td>
</tr>
<tr>
<td>스택</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>큐</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>이중 연결 리스트</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(1)</td>
<td>O(1)</td>
</tr>
<tr>
<td>해시 테이블</td>
<td>O(1)</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n)</td>
</tr>
<tr>
<td>이진 탐색 트리</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n)</td>
<td>O(n)</td>
</tr>
<tr>
<td>AVL 트리</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
</tr>
<tr>
<td>레드 블랙 트리</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
<td>O(logn)</td>
</tr>
</tbody></table>