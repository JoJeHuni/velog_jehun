<h1 id="완전탐색">완전탐색</h1>
<blockquote>
<p>완전 탐색 : 가능한 모든 경우의 수를 다 체크해서 정답을 찾는 방법</p>
</blockquote>
<p><strong>Brute Force</strong> 라고 부른다.</p>
<ol>
<li>해결하고자 하는 문제의 가능한 모든 경우의 수를 대략적으로 계산</li>
<li>가능한 모든 방법을 다 고려</li>
<li>실제 답을 구할 수 있는지 확인</li>
</ol>
<p>위 3가지를 고려해서 수행하는 것이 완전탐색 알고리즘이다.</p>
<p>그럼 방법들은 무엇들이 있을까?</p>
<ul>
<li>Brute Force : 반복, 조건문으로 모두 테스트하는 방법</li>
<li>순열 : n개의 원소 중 r개의 원소를 중복 X로 나열하는 방법</li>
<li>재귀 호출 : 원소가 포함되면 원소를 넣고 함수를 재귀하여 호출하면서 찾는 방법</li>
<li>비트마스크 : 원소가 포함되거나, 안 되거나 2가지 선택으로 경우의 수가 나올 때 용이한 방법</li>
<li>DFS, BFS : DFS(깊이우선탐색), BFS(너비우선탐색)</li>
</ul>
<h2 id="dfs-bfs">DFS, BFS</h2>
<p>DFS 먼저 알아보자.</p>
<h3 id="dfs">DFS</h3>
<p>트리의 한 요소와 다음 Level의 자식 노드를 따라가는 방향으로 탐색하는 방식이다.</p>
<ol>
<li><strong>재귀적으로 동작</strong>한다. (재귀, 스택)</li>
<li>그래프 탐색의 경우에는 어떤 노드를 거쳤는지 여부를 검사해야한다. (안 하면 무한 루프)</li>
<li><strong>모든 노드를 방문하고자 할 때 사용</strong>한다.</li>
<li>BFS 보다 간단하지만, 상대적으로 느리다.</li>
</ol>
<pre><code class="language-java">public static void dfs(int i) {
    visit[i] = true;
    for(int j=1; j&lt;n+1; j++) {
        if(map[i][j] == 1 &amp;&amp; visit[j] == false) {
            dfs(j);
        }
    }
}</code></pre>
<p>i번째 노드를 받고, 방문여부를 확실히 하기 위해 <code>visit[i] = true</code>
i번째 노드의 자식노드를 확인하면서 일단 깊이를 우선으로 쭉 내려간 뒤, 다시 위에서부터 찾는다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6ec0dec8-aee2-46eb-8d9d-aec6ed1e554f/image.gif" /></p>
<h2 id="bfs">BFS</h2>
<p>트리의 한 노드에서 같은 Level의 인접한 모드 노드를 우선 방문하는 방식</p>
<ul>
<li><strong>재귀적으로 동작 X</strong></li>
<li>당연히 방문 여부는 확실하게 해야 한다.</li>
<li><strong>방문한 노드들을 차례대로 저장하고 꺼낼 수 있는 큐 사용</strong> (큐는 FIFO니까)</li>
<li><strong>최단 경로 or 임의의 경로를 찾고 싶을 때 사용</strong>한다.</li>
</ul>
<pre><code class="language-java">public static void bfs(int i) {
    Queue&lt;Integer&gt; q = new LinkedList&lt;&gt;();
    q.offer(i);
    visit[i] = true;
    while (!q.isEmpty()) {
        int temp = q.poll();
        for (int j = 1; j &lt; n + 1; j++) {
            if (map[temp][j] == 1 &amp;&amp; !visit[j]) {
                q.offer(j);
                visit[j] = true;
            }
        }
    }
}</code></pre>
<blockquote>
<p>map : 인접 행렬로, 연결상태를 나타내는 2차원 배열이다.
map[a][b] = 1이면 a, b 노드가 연결되어 있다는 의미이다.</p>
</blockquote>
<p>i번째 노드부터 시작해서 방문여부를 확실하게 한 뒤, 큐가 빌 때까지 넣어진 순서대로 poll() 즉, 루트를 조회하고 제외하면서 차례대로 인접한지 확인하면서 반복한다.</p>
<p><a href="https://velog.io/@hyehyes/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%99%84%EC%A0%84%ED%83%90%EC%83%89">참고 블로그</a></p>
<p>DFS와 BFS에 대해 좀 더 자세하게는 다음 게시글로 작성하겠다.</p>