<h1 id="정렬-알고리즘">정렬 알고리즘</h1>
<blockquote>
<p><strong>정렬 알고리즘 : 원소들을 오름차순 or 내림차순으로 열거하는 알고리즘</strong></p>
</blockquote>
<p>정렬 알고리즘을 사용할 때는 다음의 기준들로 사용할 알고리즘을 선정한다.</p>
<ul>
<li>시간 복잡도 (소요되는 시간)</li>
<li>공간 복잡도 (메모리 사용량)</li>
</ul>
<p>둘다 Big-O 표기법으로 나타낼 수 있다.</p>
<p>정렬 알고리즘을 알아보기 전 Java에서 간단하고 자주 사용되는 함수를 알아보면</p>
<h2 id="swap">swap()</h2>
<pre><code class="language-java">public static void swap(int[] arr, int idx1, int idx2) {
    int tmp = arr[idx1];
    arr[idx1] = arr[idx2];
    arr[idx2] = tmp;
}</code></pre>
<p>배열 속 2개의 인덱스를 교환하는 메소드인데, 정렬 알고리즘에서 자주 사용되니 알아두자.</p>
<hr />
<h1 id="버블-정렬-bubble-sort">버블 정렬 (Bubble Sort)</h1>
<p>물 속의 거품들이 수면 위로 올라오는 모습과 흡사하다고 느껴져서 지어진 이름으로</p>
<p>for 문을 2번 사용해서 나타낸다.
n개의 원소 중에서 하나의 원소를 잡고 앞 or 뒤 한 쪽 방향으로 이동하면서 크기 비교를 통해 오름차순 or 내림차순으로 나타낼 때 사용한다.</p>
<p>말로는 길어지니 코드로 보자.</p>
<pre><code class="language-java">public static void bubbleSort(int[] arr) {
    for (int i = 0; i &lt; arr.length - 1; i++) {
        for (int j = 0; j &lt; arr.length - i - 1; j++) {
            if (arr[j] &gt; arr[j + 1]) {
                swap(arr, j, j + 1);
            }
        }
    }
}</code></pre>
<p>오름차순으로 정렬하는 것을 나타낸 메소드이다. arr[j] &gt; arr[a + 1] 이면 swap 해주면서 n개를 총 2번 확인해야 하기에</p>
<p>시간복잡도도 n * n -&gt; $$O(n^2)$$ 인 것을 알 수 있다.. 성능은 그다지 좋지 않다.</p>
<blockquote>
<p><strong>시간복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(n^2)$$</li>
<li>average: $$O(n^2)$$</li>
<li>best: $$O(n^2)$$</li>
</ul>
<hr />
<h1 id="선택-정렬-selection-sort">선택 정렬 (Selection Sort)</h1>
<p>맨 앞 Index 부터 차례대로 들어갈 원소를 <code>선택</code>해 정렬하는 알고리즘이다.</p>
<p>버블 정렬보다 성능이 좋다. 교환 횟수 자체는 O(n)으로 적기 때문이다.</p>
<p>하지만 끝까지 보면 시간복잡도는 다르다.</p>
<pre><code class="language-java">public static void selectionSort(int[] arr) {
    for (int i = 0; i &lt; arr.length - 1; i++) {
        int minIdx = i; 
        for (int j = i + 1; j &lt; arr.length; j++) {
            if (arr[j] &lt; arr[minIdx]) {
                minIdx = j;
            }
        }
        swap(arr, i, minIdx);
    }
}</code></pre>
<p>Index 0 ~ (n - 1)을 돌면서 원소의 값이 가장 작은 것을 찾고 가장 작은 원소를 Index 0의 자리와 <code>swap()</code> 한다.</p>
<blockquote>
<p>당연히 Index 0이 가장 작으면 제자리</p>
</blockquote>
<p>그럼 Index 0의 자리는 정해졌고, 1부터 돌면서 1 ~ (n - 1) 동안 원소의 값이 가장 작은 것을 찾아 Index 1 자리로 <code>swap()</code> 해준다.</p>
<p>위 과정을 계속 반복하면 된다.</p>
<blockquote>
<p><strong>시간 복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(n^2)$$</li>
<li>average: $$O(n^2)$$</li>
<li>best: $$O(n^2)$$</li>
</ul>
<hr />
<h1 id="삽입-정렬-insertion-sort">삽입 정렬 (Insertion Sort)</h1>
<p><strong>새로운</strong> 원소를 이전까지 정렬된 원소 사이에 올바르게 삽입시키는 알고리즘</p>
<blockquote>
<p>즉, <strong>0번째 인덱스는 처음에 이미 정렬된 것이니 비교 X</strong></p>
</blockquote>
<p>1번째 인덱스부터 확인한다.</p>
<pre><code class="language-java">public static void insertionSort(int[] arr) {
    for (int i = 1; i &lt; arr.length; i++) {
        int tmp = arr[i];
        int j = i - 1;
        while (j &gt;= 0 &amp;&amp; tmp &lt; arr[j]) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = tmp;
    }
}</code></pre>
<p>정렬이 돼 있는 경우 한 번만 쭉 확인하면 되므로 O(n)의 속도로 성능이 좋다.</p>
<p>코드를 봐보자.</p>
<p>맨 처음에는 tmp = arr[1], j = 0으로 시작하여
만약에 j &gt;= 0 이면서 tmp -&gt; arr[1] &lt; arr[0] 이면?
-&gt; <code>swap()</code>된다.</p>
<ol>
<li><code>arr[1] = arr[0]</code> 이 들어가고, </li>
<li>j--로 <code>j는 -1</code>이 된 뒤, </li>
<li>이제 while문의 조건이 만족하지 않아서 빠져나오면서 <code>arr[0] = tmp</code>으로</li>
</ol>
<p>이런 형태로 하나의 인덱스를 붙잡고 크기 비교하면서 앞으로 전진시킨다.</p>
<p>(다른 형태라면 뒤로 후퇴시킬 것임)</p>
<p>들어갈 자리가 정해지면 넣어준다.</p>
<blockquote>
<p><strong>시간복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(n^2)$$</li>
<li>average: $$O(n^2)$$</li>
<li>best: $$O(n)$$</li>
</ul>
<hr />
<h1 id="쉘-정렬-shell-sort">쉘 정렬 (Shell Sort)</h1>
<p>삽입 정렬의 장점을 살리고, 단점을 보완한 정렬 알고리즘이다.</p>
<p><strong>삽입 정렬의 단점</strong>
-&gt; (n - 1) 번째 인덱스 원소의 들어가야 할 자리가 0번째 인덱스면 많은 swap()을 해야 한다.</p>
<p>배열을 부분 배열들로 나눠 어느 정도 정렬을 시킨 뒤, 나눈 간격을 줄여 정렬시키는 것을 반복하는 정렬 방법이다.</p>
<p>평균 시간 복잡도는 $$O(n^1.5)$$ 이다.</p>
<pre><code class="language-java">public static void shellSort(int[] arr) {
    for (int h = arr.length / 2; h &gt; 0; h /= 2) {
        for (int i = h; i &lt; arr.length; i++) {
            int tmp = arr[i];
            int j = i - h;
            while (j &gt;= 0 &amp;&amp; arr[j] &gt; tmp) {
                arr[j + h] = arr[j];
                j -= h;
            }
            arr[j + h] = tmp;
        }
    }
}</code></pre>
<ol>
<li>처음에는 배열 전체 길이의 반으로 시작한다. -&gt; <code>h = arr.length / 2</code></li>
<li>반복문을 거듭할 수록 그 간격의 절반으로 줄여나가면서 반복한다. <code>(int h = arr.length / 2; h &gt; 0; h /= 2)</code></li>
<li>2번째 for문에서 부분 배열들을 삽입정렬해준 뒤 다 끝나면 간격을 계속 반으로 줄이는 것이다.</li>
<li>간격이 1이 될 때까지 반복한다.</li>
</ol>
<blockquote>
<p><strong>시간 복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(n^2)$$</li>
<li>average: $$O(n^1.5)$$</li>
<li>best: $$O(n)$$</li>
</ul>
<hr />
<h1 id="병합-정렬-merge-sort">병합 정렬 (Merge Sort)</h1>
<p>분할 정복 알고리즘 중 하나로 배열의 길이가 1이 될 때까지 2개의 부분 배열로 분할한 뒤, 완료되면 다시 2개였던 부분 배열을 합병하면서 정렬하는 것이다.</p>
<p>시간 복잡도는 $$O(nlogn)$$으로 빠르지만 단점 또한 있다. 아래 코드를 보자.</p>
<pre><code class="language-java">public static void sortByMergeSort(int[] arr) {
    int[] tmpArr = new int[arr.length];
    mergeSort(arr, tmpArr, 0, arr.length - 1);
}
public static void mergeSort(int[] arr, int[] tmpArr, int left, int right) {
    if (left &lt; right) {
        int m = left + (right - left) / 2;
        mergeSort(arr, tmpArr, left, m);
        mergeSort(arr, tmpArr, m + 1, right);
        merge(arr, tmpArr, left, m, right);
    }
}
public static void merge(int[] arr, int[] tmpArr, int left, int mid, int right) {
    for (int i = left; i &lt;= right; i++) {
        tmpArr[i] = arr[i];
    }
    int part1 = left;
    int part2 = mid + 1;
    int index = left;
    while (part1 &lt;= mid &amp;&amp; part2 &lt;= right) {
        if (tmpArr[part1] &lt;= tmpArr[part2]) {
            arr[index] = tmpArr[part1];
            part1++;
        } else {
            arr[index] = tmpArr[part2];
            part2++;
        }
        index++;
    }
    for (int i = 0; i &lt;= mid - part1; i++) {
        arr[index + i] = tmpArr[part1 + i];
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ab3f3ff3-c1d6-4263-b374-87e9268f37ac/image.png" /></p>
<p>재귀를 수행하는 <code>mergeSort()</code>와 재귀를 마치고 합병하는 <code>merge()</code>로 구성</p>
<p><code>mergeSort()</code>는 반으로 나눠 재귀호출하며 다 나눠지면 점점 올라오면서 <code>merge()</code> 한다.</p>
<p><code>merge()</code>는 2개의 부분 배열의 인덱스를 점차적으로 비교하며 정렬한다.</p>
<h2 id="합병-정렬의-단점">합병 정렬의 단점</h2>
<p>시간 복잡도가 $$O(nlog n)$$으로 빠르지만, 아래 코드를 보면 <code>tmpArr</code>을 사용해야 돼서 제자리 정렬보다 $$O(n)$$만큼 추가적인 메모리가 사용되는 단점이 있다.</p>
<p>보통은 재귀함수로 구현하므로 이 것또한 메모리를 많이 사용하게 된다.</p>
<blockquote>
<p><strong>시간 복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(nlog n)$$</li>
<li>average: $$O(nlog n)$$</li>
<li>best: $$O(nlog n)$$</li>
</ul>
<blockquote>
<p><strong>공간 복잡도</strong>
$$O(n)$$만큼의 추가 메모리 <code>tmpArr</code> 사용</p>
</blockquote>
<hr />
<h1 id="힙-정렬">힙 정렬</h1>
<p>오름차순 정렬일 때 최대 힙을 사용하는 정렬 (내림차순이면 최소 힙 사용)</p>
<p>최대 힙을 배열로 구현하면, 0번째 인덱스가 가장 큰 수이니까</p>
<p>합병정렬, 퀵 정렬과는 $$O(logn)$$으로 동일하지만 사실 성능은 더 낮다.
-&gt; 매번 루트에서 최댓 값을 뺄 때마다 <code>heapify()</code> 로 최대 힙을 다시 만들어줘야 해서 그렇다.</p>
<pre><code class="language-java">public static void heapSort(int[] arr) {
    for (int i = arr.length / 2 - 1; i &lt; arr.length; i++) {
        heapify(arr, i, arr.length - 1);
    }

    for (int i = arr.length - 1; i &gt;= 0; i--) {
        swap(arr, 0, i);
        heapify(arr, 0, i - 1);
    }
}

public static void heapify(int[] arr, int parentIdx, int lastIdx) {
    int leftChildIdx;
    int rightChildIdx;
    int largestIdx;
    while (parentIdx * 2 + 1 &lt;= lastIdx) {
        leftChildIdx = (parentIdx * 2) + 1;
        rightChildIdx = (parentIdx * 2) + 2;
        largestIdx = parentIdx;
        if (arr[leftChildIdx] &gt; arr[largestIdx]) {
            largestIdx = leftChildIdx;
        }
        if (rightChildIdx &lt;= lastIdx &amp;&amp; arr[rightChildIdx] &gt; arr[largestIdx]) {
            largestIdx = rightChildIdx;
        }
        if (largestIdx != parentIdx) {
            swap(arr, parentIdx, largestIdx);
            parentIdx = largestIdx;
        } else {
            break;
        }
    }
}</code></pre>
<ol>
<li>최대 힙으로 되어 있는 트리에 새로운 요소를 삽입한다.</li>
<li>요소는 부모 노드와 비교하여 점차적으로 swap()을 거쳐가며 가능하다면 루트 노드까지 비교한다.</li>
<li>다 swap()이 된 후에는 최대 힙에 있는 루트 노드를 제거한 뒤, 맨 마지막에 있는 노드를 루트 노드로 가져온다.</li>
<li>그럼 현재 루트 노드에 있는 것은 swap()을 거치든 말든 이전의 루트 노드보다 작은 노드이고</li>
<li>자식 노드와 비교하면서 점차적으로 교환하며 다시 내려간다.</li>
<li>이런 과정을 반복하다보면 자리를 찾아간다.</li>
</ol>
<blockquote>
<p><strong>시간 복잡도</strong></p>
</blockquote>
<ul>
<li>worst: $$O(nlog n)$$</li>
<li>average: $$O(nlog n)$$</li>
<li>best: $$O(nlog n)$$</li>
</ul>
<hr />
<h1 id="퀵-정렬-quick-sort">퀵 정렬 (Quick Sort)</h1>
<p>pivot 을 사용한 정렬 알고리즘으로, 합병 정렬과 같은 분할 정복 알고리즘이다.</p>
<p><strong>mergeSort 와의 차이점</strong></p>
<ul>
<li>mergeSort : n 개의 input이 있으면 반으로 나눠가며 나눌 수 없을 때까지 한 후 병합</li>
<li>QuickSort : 꼭 반으로 나누는 것이 아니다. -&gt; 유연하다.</li>
</ul>
<p><strong>Pivot</strong> : 한 쪽은 작은 값, 한 쪽은 큰 값으로 나누는 기준이 된다.</p>
<blockquote>
<p>나눈 곳에서 또 Pivot을 정해서 또 작은 값, 큰 값으로 나눠간다. -&gt; 즉, <strong>재귀</strong></p>
</blockquote>
<p>처음 pivot을 정하고 정렬을 하면, 마지막에 정렬이 다 돼도 그 <strong>처음 정렬한 위치는 변함이 없다.</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/78b66ea2-bb90-401e-a788-53f44567e71a/image.png" /></p>
<p>코드를 구현하기 위해서는 이 3가지를 다 구현해야 한다.</p>
<ul>
<li>input / output</li>
<li>어떻게 반복할 것인가</li>
<li>반복 작업에서 수행할 작업</li>
</ul>
<pre><code class="language-java">public static void sortByQuickSort(int[] arr) {
    quickSort(arr, 0, arr.length - 1);
}

public static void quickSort(int[] arr, int left, int right) {
    int part = partition(arr, left, right);
    if (left &lt; part - 1) {
        quickSort(arr, left, part - 1);
    }
    if (part &lt; right) {
        quickSort(arr, part, right);
    }
}

public static int partition(int[] arr, int left, int right) {
    int pivot = arr[(left + right) / 2];
    while (left &lt;= right) {
        while (arr[left] &lt; pivot) {
            left++;
        }
        while (arr[right] &gt; pivot) {
            right--;
        }
        if (left &lt;= right) {
            swap(arr, left, right);
            left++;
            right--;
        }
    }
    return left;
}</code></pre>
<ul>
<li><code>left</code>는 부분 배열의 첫 index</li>
<li><code>right</code>는 부분 배열의 마지막 index</li>
<li><code>pivot</code>은 그 중간의 원소 값</li>
</ul>
<p><code>pivot</code>을 일단 0번째 인덱스의 값과 자리를 바꿔준 뒤,</p>
<p><code>partition()</code> 에서 <code>left</code>의 원소 값이 <code>pivot</code> 보다 커질 때까지 증가하면서 찾고,<code>right</code>는 반대로 작아질 때까지 감소하면서 찾는다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ad035b6c-8b5b-4176-be50-a012ee27fb05/image.png" /></p>
<ol>
<li>똑같이 맨 왼쪽으로 옮기고 <code>i</code>, <code>j</code>를 정해두고</li>
</ol>
<ul>
<li><code>i</code> : <code>pivot</code> (현재는 8) 보다 큰 것을 만나면 멈춘다. (11에서 멈춘다.)</li>
<li><code>j</code> : <code>pivot</code> 보다 작은 것을 만나면 멈춘다. (7에서 멈춘다.)<ul>
<li><code>i</code>, <code>j</code>를 바꿔주고 다시 옮긴다. (다음 i는 9, j는 6)</li>
<li>⇒ 총 3번의 <code>swap</code>이 실행된다.</li>
</ul>
</li>
</ul>
<ol>
<li>마지막 <code>i</code>의 위치와 <code>pivot</code> 위치를 바꿔준다.</li>
</ol>
<p>⇒ <strong><code>i</code>는 <code>pivot</code>보다 작은 값을 가리키기 때문에 바꿔줘야 한다.</strong></p>
<p>⇒ <code>pivot</code>을 기준으로 왼쪽은 작은 값, 오른쪽은 큰 값이 된다.</p>
<p>또한 <strong>read, write가 엄청 적게 일어난다.</strong></p>
<p><strong>대신, <code>i</code>와 <code>j</code>라는 포인터를 생성해줘야 한다.</strong></p>
<p>위의 과정을 거치면서 왼쪽과 오른쪽 부분 배열 쪽도 똑같이 pivot을 정하고 반복하다보면 된다.</p>
<p><strong>시간 복잡도</strong></p>
<ul>
<li>worst: $$O(n^2)$$</li>
<li>average: $$O(nlog n)$$</li>
<li>best: $$O(nlog n)$$</li>
</ul>