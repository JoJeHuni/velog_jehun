<p>인텔리제이 설치 후</p>
<ul>
<li>마우스 휠 확대, 축소</li>
<li>인코딩 설정 (2군데)</li>
</ul>
<p>2가지 과정이 필요하다.</p>
<p>file -&gt; settings로 가서</p>
<p>1.
<code>wheel</code> 검색 후 마우스 휠로 확대, 축소하는거 체크</p>
<p>2.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e7566efb-925a-442d-af2c-39938530cfa9/image.png" /></p>
<ul>
<li>help -&gt; 'Edit custon VM options'</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/18b47a2e-8e17-48ce-a925-3454ce10dbb8/image.png" /></p>
<p>이것까지 해주자.</p>
<hr />
<p>자바 소스코드는 파일 탐색기에서 보면 java 파일로 돼 있지만, 자바 내부의 JVM 속 compiler 가 컴파일을 통해 class 파일로 변경해준다.</p>
<p>컴퓨터가 알아보기 좋은 byte code로 된 자바 파일로 변경된 것.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/17987160-b61a-4b5d-ba07-0544c275c9db/image.png" /></p>
<hr />
<p>설치까지 완료가 됐으니 한 번 패키지와 class 파일을 만들어서 간단한 연산을 해보자.</p>
<pre><code class="language-java">package com.orgiraffers.section01.literal;

public class Application2 {

    public static void main(String[] args) {

        /* 수업목표. 값을 직접 연산하여 출력할 수 있다. */
        /* 목차 1. 정수와 정수의 연산 */
        System.out.println(123 + 456);
        System.out.println(123 - 23);
        System.out.println(123 * 10);
        System.out.println(123 / 10);
        System.out.println(123 % 10);

        /* 목차 2. 실수와 실수의 연산 */
        System.out.println(1.23 + 1.23);
        System.out.println(1.23 - 0.23);
        System.out.println(1.23 * 10.0);
        System.out.println(1.23 / 10.0);            // 소수점 이하를 보고 싶으면 실수를 활용한 나눗셈을 하자.
        System.out.println(1.23 % 1.0);             /* 궁금 : 부동소수점에 대한 공부를 해보자. 왜 2.3이 아닌 22.9999...가 나오는건지 */

        /* 목차 3. 정수와 실수의 연산 */
        System.out.println(123 / 0.5);              // 실수보다 정수가 더 우선순위가 높다.

        /* 목차 4. 문자와 정수의 연산 */
        System.out.println('a' + 1);                // 97 + 1
        System.out.println('a' % 2);                // 1

        /* 목차 5. 문자열과 문자열의 연산 */
        // 문자열과 문자열의 연산은 '+'만 가능하고 이어붙이기 효과
        System.out.println(&quot;Hello &quot; + &quot;world!!&quot;);   // Hello world!!

        /* 목차 6. 문자열과 다른 형태의 값 연산 */
        System.out.println(&quot;Hello &quot; + 123);                 // Hello 123
        System.out.println(&quot;Hello &quot; + true);                // Hello true
        System.out.println(&quot;Hello &quot; + 123.0 + 2);           // Hello 123.02

        // 아래 연산은 좀 신기하게 된다.
        System.out.println(123 + 1 + &quot;Hello &quot; + 123.0 + 2); // 124Hello 123.02

        /* 목차 7. 논리값과 문자열 형태의 값 연산 */
        System.out.println(true + &quot;문자열&quot;);       // true문자열

    }
}
</code></pre>