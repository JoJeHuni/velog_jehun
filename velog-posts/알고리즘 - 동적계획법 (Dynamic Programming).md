<h1 id="dynamic-programming">Dynamic Programming</h1>
<p>동적계획법. 이름만 보면 뭔가 싶을 수도 있지만 간단히 말하면 &quot;프로그램이 작동하는 동안 컴퓨터가 메모리에 할당해 '기억'하는 것&quot; 이라고 말하려고 한다.</p>
<p>프로그래밍이 붙어서 컴퓨터 프로그래밍이라고 생각을 했었지만, 테이블을 만드는 의미로 쓰인다고 한다.</p>
<hr />
<ul>
<li>분할은 하지 않는다.</li>
<li>시작부터 작은 단위부터 풀어나간다. (분할정복법에서의 '정복'의 과정과 비슷함)</li>
<li>분할정복법과 비슷한 점<ul>
<li>작은 문제들을 풀어나가면서 생긴 솔루션을 어딘가에 저장해두고 다음 step에서 쓴다.</li>
<li>작은 문제를 다시 푸는 것이 아닌, 솔루션한 것만 가져다 쓴다.</li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/82c3a3e0-6df9-4ffa-8aa8-5fd57a4de326/image.png" /></p>
<p>동적 계획은 B를 구하기 위해 DEF를 가져다 쓴다. C를 구하기 위해 EFG를 가져다 쓴다.</p>
<p>분할 정복과 다른 점 : E,F가 <strong>B를 구할 때도, C를 구할 때도 사용</strong>한다. <strong>(재사용이 가능하다.)</strong></p>
<p>별로 좋지 않은 예시로 단순 재귀로 하는 피보나치 수열이 있다.</p>
<pre><code class="language-java">public class Simple {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        dp  = new int[n+1];
        System.out.println(fibo(n));
    }

    static int fibo(int x) {
        if( x ==1 || x==2) return 1;
        return fibo(x-1) + fibo(x-2);
    }
}</code></pre>
<p>피보나치 수열은 $$f(n) = f(n-1) + f(n-2)$$ 로 계속 메소드를 재귀를 통해 중복 호출하다보면 시간복잡도가 $$O(2^n)$$ 까지 간다.. 아주 안 좋다...</p>
<p>이것을 해결하기 위해서 동적 계획법을 사용하면 좋다!</p>
<blockquote>
<p><strong>DP 문제가 성립하기 위한 조건</strong></p>
</blockquote>
<ol>
<li>최적 부분 구조 : 상위 문제를 하위 문제로 나눌 수 있고, 하위 문제를 모아서 상위 문제를 해결할 때</li>
<li>중복되는 부분 문제 : 동일한 작은 문제를 반복적으로 해결해야 할 때</li>
</ol>
<p>-&gt; 즉, 피보나치 수열에 DP가 어울린다.</p>
<p>DP 구현 방법은 일반적으로 1. <strong>Top-down(하향식)</strong>과 2. <strong>Bottom-up(상향식)</strong> 2가지가 있다.</p>
<hr />
<h2 id="top-down-하향식">Top-down (하향식)</h2>
<blockquote>
<p>Top-down : 상위 문제를 해결하기 위해 하위 문제를 재귀적으로 호출하여 해결해서 상위 문제를 해결하는 방식
    하위 문제들을 저장해두기 위해 Memoization 사용</p>
</blockquote>
<pre><code class="language-java">public class Topdown {
    static int[] dp;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        dp = new int[n+1];
        System.out.println(fibo(n));

    }

    // Top-down
    static int fibo(int x) {
        if(x ==1 || x==2) return 1;
        if(dp[x] != 0) return dp[x];
        dp[x] = fibo(x-1) + fibo(x-2);
        return dp[x];
    }
}</code></pre>
<p>메소드에서 x를 매개변수로 받아서 <code>if(dp[x] != 0)</code>이면 dp[x]를 반환. -&gt; fibo(x)에 저장된다. -&gt; 즉, 기억을 하고 있음</p>
<p>그렇게 fibo(x) 에는 dp[x]가 들어간 상태가 되면서 결국엔 하위 문제들인 f(x-1), f(x-2) 들을 계속 구하면서 기억해두기 때문에 재귀만 하던 피보나치 수열과 다르다.</p>
<hr />
<h2 id="bottom-up-상향식">Bottom-up (상향식)</h2>
<blockquote>
<p>Bottom-up : 하위에서부터 문제를 해결해나가면서 먼저 계산했던 문제들의 값을 활용해서 상위 문제를 해결해나가는 방식</p>
</blockquote>
<p>애초에 DP의 전형적인 형태는 Bottom-up 형태이며, 하위 문제 결과 값을 저장하는 리스트를 D<strong>P 테이블</strong> 이라고 부른다.</p>
<pre><code class="language-java">public class Bottomup {

    static int[] dp;
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        dp  = new int[n+1];

        System.out.println(fibo(n));
    }

    // Bottom-up
    static int fibo(int x) {
        dp[1] =1;
        dp[2] =1;
        for(int i=3; i&lt;x+1; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[x];
    }
}</code></pre>
<p>애초에 작은 곳부터 저장해두면서 올라가는게 알아보기도 편하다고 느낀다. 간편하게 이해할 수 있을 것이라고 생각한다.</p>
<hr />
<h2 id="memoization">Memoization</h2>
<p>DP 구현 방법 중 하나, 한 번 계산한 결과를 메모리 공간에 저장해두는 기법으로 모든 하위 문제들이 한 번씩만 계산된다고 보장할 수 있다.</p>
<p>Top-down 과 재귀에서 fibo를 다시 보자</p>
<pre><code class="language-java">// Memoization 
static int fibo(int x) {
   if( x ==1 || x==2) return 1;
   if(dp[x] != 0) return dp[x];
   dp[x] = fibo(x-1) + fibo(x-2);
   return dp[x];
}

// 일반 재귀 함수
static int fibo(int x) {
   if( x ==1 || x==2) return 1;
   return fibo(x-1) + fibo(x-2);
}</code></pre>
<p>크게 차이 없어 보이지만 dp[x]에 값을 저장해두면 다음에 불러올 때 그대로 가져오면 된다.
하지만, 일반 재귀함수는 값 저장을 안 하기에</p>
<p>fibo(4) = fibo(3) + fibo(2)
fibo(3) = fibo(2) + fibo(1)</p>
<p>이렇게만 봐도 벌써 fibo(2)를 2번 구해야 하는데, 갈수록 점점 늘어나게 된다.</p>
<p>Memoization 은 이렇게 동작한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/76db97a4-0b33-46c5-9394-72f4f9079f48/image.png" /></p>