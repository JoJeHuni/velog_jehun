<h1 id="greedy-알고리즘">Greedy 알고리즘</h1>
<blockquote>
<p>Greedy : 탐욕스러운, 욕심이 많은</p>
</blockquote>
<p>간단하게 <strong>현재 상황에서 가장 좋은 것만 고르는 방법</strong>이다.</p>
<p>대표적인 예제로는 거스름돈이 있다.</p>
<p>input : 거스름돈에 대한 값
unit의 종류 : 한국에서는 500, 100, 50, 10, 1 이 있다.</p>
<p>자연어로 써보면</p>
<ol>
<li>각 코인의 개수는 모두 0개부터 시작</li>
<li>가장 큰 유닛 (위에서는 500원)부터 input 값에서 빼준다</li>
<li>거스름돈이 0이 될 때까지 2번을 반복한다.</li>
<li>최종 동전의 개수를 반환한다.</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d0ffb306-ce22-4729-9589-a0a77738e194/image.png" /></p>
<pre><code class="language-java">import java.util.Scanner;

public class Main
{
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int total = scan.nextInt();
        int minCoinCnt = 0;
        int coins[] = {500, 100, 50, 10};

        for (int coin : coins){
            minCoinCnt += (total/coin);
            total %= coin;
        }

        System.out.println(&quot;result = &quot; + minCoinCnt);
    }
}</code></pre>
<blockquote>
<p>근데 만약에 500원과 100원 사이에 160원이라는 unit이 추가되면?</p>
</blockquote>
<p>값이 바뀌게 되는데 즉, Greedy 알고리즘은 항상 정답을 내지 않는다.</p>
<p>코딩테스트 문제와 함께 보자.</p>