<h1 id="이분탐색">이분탐색</h1>
<p>'정렬된' 배열 or 리스트에 적합한 고속 탐색 방법이다.</p>
<p>배열의 중앙에 있는 값을 조사해 찾고자 하는 항목이 왼쪽 or 오른쪽에 있느냐에 따라 탐색하는 범위를 반으로 줄이는 것을 반복하면서 찾는 방식이다.</p>
<p>ex) 중앙에 있는 값보다 찾고자 하는 것이 오른쪽에 있다면? -&gt; 왼쪽은 볼 필요 없다.</p>
<p>이런 방식을 계속 반복해서 찾는 범위를 반으로 나눠가면서 찾는 방식이다.</p>
<blockquote>
<p>그렇기에 꼭 정렬이 돼 있어야 하는 것</p>
</blockquote>
<h2 id="구현">구현</h2>
<ol>
<li><p>array[low] ~ array[high] (array[left], array[right]라고 하기도 한다.) 에 다 값들이 들어있다고 생각한다.</p>
</li>
<li><p>array[mid] -&gt; 에서 mid는 $$(low + high) / 2$$</p>
</li>
</ol>
<ul>
<li>왼쪽 배열은 array[low] ~ array[mid - 1]</li>
<li>오른쪽 배열은 array[mid + 1] ~ array[high]</li>
</ul>
<ol start="3">
<li>당연히 특정 값(key) 이 중앙값과 같은지 처음에 확인해야 한다.</li>
</ol>
<ul>
<li>key == mid : 찾고자 하는 값을 찾았으니 return</li>
<li>key &gt; mid : low를 mid + 1 로 만들어주면 된다.<ul>
<li>array[mid + 1] ~ array[high]</li>
</ul>
</li>
<li>key &lt; mid : high를 mid - 1 로 만들어주면 된다.<ul>
<li>array[low] ~ array[mid - 1]</li>
</ul>
</li>
</ul>
<ol start="4">
<li>위 과정을 반복한다.</li>
</ol>
<p>그럼 재귀하는 방법이 있을 것이고, 그냥 반복문을 돌리는 방법도 있을 것이다.</p>
<pre><code class="language-java">int binarySearch(int key, int low, int high) {
    int mid;

    if(low &lt;= high) {
        mid = (low + high) / 2;

        if(key == arr[mid]) { // 탐색 성공 
            return mid;
        } else if(key &lt; arr[mid]) {
            // 왼쪽 부분으로 범위를 바꿔서 재귀
            return binarySearch(key ,low, mid-1);  
        } else {
            // 오른쪽 부분으로 범위를 바꿔서 재귀
            return binarySearch(key, mid+1, high); 
        }
    }

    return -1; // 탐색 실패 
}</code></pre>
<pre><code class="language-java">int binarySearch(int key, int low, int high) {
    int mid;

    while(low &lt;= high) {
        mid = (low + high) / 2;

        if(key == arr[mid]) { // 탐색 성공 
            return mid;
        } else if(key &lt; arr[mid]) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return -1; // 탐색 실패 
}</code></pre>
<p>int 는 정수형이니까 만약 $(0+1)/2 = 0.5$ 같이 나온다면? mid = 0이 된다.
계속 범위를 줄여나가다가 2개만 남았을 경우에 걱정할 필요 없다.</p>
<h2 id="성능">성능</h2>
<table>
<thead>
<tr>
<th>비교</th>
<th>범위</th>
</tr>
</thead>
<tbody><tr>
<td>q0</td>
<td>n</td>
</tr>
<tr>
<td>q1</td>
<td>$n/2$</td>
</tr>
<tr>
<td>q2</td>
<td>$n/4$</td>
</tr>
<tr>
<td>...</td>
<td>...</td>
</tr>
<tr>
<td>qk</td>
<td>$n/2^k$</td>
</tr>
</tbody></table>
<p>마지막 행에서 $n/2^k$ = 1 이기 때문에 $k = log2n$ 이다.
즉, $O(logn)$</p>
<ul>
<li>log 2n의 경우 밑이 10인 log n으로 계산한다.</li>
</ul>