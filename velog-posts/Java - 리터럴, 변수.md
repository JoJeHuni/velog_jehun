<h1 id="리터럴">리터럴</h1>
<blockquote>
<p>리터럴 (literal) : 변하지 않는 데이터 그 자체</p>
</blockquote>
<p>상수와 혼동할 수 있지만, </p>
<ul>
<li>상수는 데이터가 저장되는 메모리 상의 공간을 의미</li>
<li>리터럴은 고정된 값 자체</li>
</ul>
<pre><code class="language-java">int age = 20;                // 20은 리터럴이다.
final int MAX_AGE = 100;     // 100은 리터럴이다. (MAX_AGE는 상수이다.)
String str = &quot;java&quot;          // &quot;text&quot;는 리터럴이다.</code></pre>
<h3 id="리터럴의-종류">리터럴의 종류</h3>
<table>
<thead>
<tr>
<th>종류</th>
<th>예시</th>
<th>접두사, 접미사</th>
</tr>
</thead>
<tbody><tr>
<td>논리형</td>
<td>false, true</td>
<td>-</td>
</tr>
<tr>
<td>정수형</td>
<td>100, 0b0011, 077, 0xFF, 12L</td>
<td>L (long 타입은 L을 접미사로 사용한다. / l은 1과 헷갈릴 수 있어서 대문자로 쓴다)0 (8진수는 리터럴 앞에 접두사 0을 쓴다) / 0x (16진수는 리터럴 앞에 접두사 ‘0x’ 또는 ‘0X’를 쓴다)</td>
</tr>
<tr>
<td>실수형</td>
<td>3.14, 12.0e8, 7.7f</td>
<td>f (float는 접미사 ‘f’, ‘F’를 쓴다) / d (double 타입은 ‘d’, ‘D’를 쓴다. 실수형은 double이 기본 자료형이므로 생략도 가능하다.)</td>
</tr>
<tr>
<td>문자형</td>
<td>‘O’, ‘1’, ‘\n’</td>
<td>-</td>
</tr>
<tr>
<td>문자열</td>
<td>“OHGIRAFFERS”, “100”, “false”</td>
<td>-</td>
</tr>
</tbody></table>
<blockquote>
<p>리터럴의 활용은 이전 게시글에서 다뤘기 때문에 변수에 대해 알아보자.</p>
</blockquote>
<hr />
<h1 id="변수">변수</h1>
<blockquote>
<p>변수 : 데이터를 저장하기 위해 할당 받은 메모리 공간을 의미한다.</p>
</blockquote>
<p>전역 변수, 지역 변수 중 일단은 지역 변수에 대해 다뤄본다.</p>
<h2 id="변수-사용-방법">변수 사용 방법</h2>
<blockquote>
<ol>
<li>변수 선언 후 변수에 값을 대입하는 방법 -&gt; 선언 후 초기화</li>
<li>동시에 작성하는 방법 -&gt; 선언과 동시에 초기화</li>
</ol>
</blockquote>
<pre><code class="language-java">// 변수의 선언 예시
int age;

// 선언한 변수에 값 대입 예시
age = 20;
age = age;

// 선언과 동시에 초기화 예시
int point = 100;</code></pre>
<h2 id="변수의-명명규칙">변수의 명명규칙</h2>
<ol>
<li><strong>컴파일 에러를 발생시킬 수 있는 규칙</strong><ol>
<li>동일한 범위 내에서 동일한 변수명을 가질 수 없다.</li>
<li>변수의 이름에는 자바에서 사용중인 키워드(keyword)를 사용할 수 없다.
ex) int, float, while, continue, 등등</li>
<li>변수의 이름은 영문자 대소문자를 구분한다.</li>
</ol>
 <strong>ex</strong>)** age와 Age는 다르다.<ol start="4">
<li>변수의 이름은 숫자로 시작할 수 없다.
ex<strong>)</strong> 123abc - <strong>사용불가</strong></li>
<li>특수기호는 '-'와 '$'만 사용 가능하다.
ex<strong>)</strong> <em>abc</em>_zxc, abc$123, _abc123, 등등</li>
</ol>
</li>
</ol>
<pre><code class="language-java">/* 1-1. 동일한 범위 내에서 동일한 변수명을 가질 수 없다. */
int age = 20;
//int age = 20;            //동일한 변수명을 가지므로 에러 발생함

/* 1-2. 예약어는 사용이 불가능하다. */
//int true = 1;            //예약어 사용 불가
//int for = 20;            //예약어 사용 불가

/* 1-3. 변수명은 대소문자를 구분한다. */
int Age = 20;            //위에서 만든 age와 다른 것으로 취급한다.
int True = 10;     //예약어 True와 다른 것으로 취급한다.

/* 1-4. 변수명은 숫자로 시작할 수 없다. */
//int 1age = 20;        //숫자로 시작해서 에러 발생
int age1 = 20;            //숫자가 처음에 시작하지 않으면 섞어서 사용도 가능함

/* 1-5. 특수기호는 '-'와 '$'만 사용 가능하다. */
//int sh@rp = 20;        //사용 가능한 특수문자 외에는 사용 불가능
int _age = 20;                //언더스코어는 사용 가능함. 처음도 가능하고 중간이나 끝에도 가능함
int $harp = 20;            //$도 사용 가능함. 처음도 가능하고 중간이나 끝에도 가능함</code></pre>
<ol start="2">
<li>컴파일 에러 발생은 X 지만, 개발자끼리의 암묵적인 규칙1. 변수명의 길이 제한은 없다. (하지만 적당히 해야 한다.)<ol start="2">
<li>변수명이 합성어로 이루어진 경우 첫 단어는 소문자, 두 번째 시작 단어는 대문자로 시작한다. (camel-case)
ex) int memberAddress;</li>
<li>단어와 단어 사이의 연결을 언더스코어( _ )로 하지 않는다. (타 언어 네이밍 규칙이다.)</li>
<li>한글로 변수명을 짓는 것이 가능하지만, 권장하지 않는다. (한글을 취급하는 다양한 방식들이 존재하기 때문에 에러를 유발할 수 있다.)</li>
<li>변수 안에 저장된 값이 어떤 의미를 가지는지 명확하게 표현하도록 한다.</li>
<li>전형적인 변수 이름이 있다면 가급적 사용하도록 한다.</li>
<li>명사형으로 작성할 수 있도록 한다.</li>
<li>boolean 형은 의문문으로 가급적 긍정 형태로 네이밍한다.</li>
</ol>
</li>
</ol>
<pre><code class="language-java"> /* 2-1. 변수명의 길이 제한은 없다. */
int sadjfsadkjhfkjsadhfkjhsafkjhsdfjkhsafkjhsdjkfhsdajkfhdsakjfhsdakjfhasdjkfhsdafkjhfsdakj;

/* 2-2. 변수명이 합성어로 이루어진 경우 첫 단어는 소문자, 두 번째 시작 단어는 대문자로 시작한다. */
/* 자바에서는 클래스명만 유일하게 대문자로 시작한다. */
int maxAge = 20;
int minAge = 10;

/* 2-3. 단어와 단어 사이의 연결을 언더스코어(_)로 하지 않는다. */
String user_name;            //에러가 발생하지 않지만 이렇게 하면 안된다.
String userName;              //이게 올바른 표현이다.

/* 2-4. 한글로 변수명을 짓는 것이 가능하지만, 권장하지 않는다. */
int 나이;                          //가능하지만 권장하지 않음

/* 2-5. 변수 안에 저장된 값이 어떤 의미를 가지는지 명확하게 표현하도록 한다. */
String s;                        //변수가 어떤 의미인지 파악하기 힘들다.
String name;                    //문자열 형태의 이름이 저장되겠구나 하는 의도가 파악이 된다.

/* 2-6. 전형적인 변수 이름이 있다면 가급적 사용하도록 한다. */
int sum = 0;
int max = 10;
int min = 0;
int count = 1;

/* 2-7. 명사형으로 작성할 수 있도록 한다. */
String goHome;                    //가능하긴 하지만 가급적 명사형으로 짓는다.
String home;

/* 2-8. boolean 형은 의문문으로 가급적 긍정형태로 네이밍한다. */
boolean isAlive = true;
boolean isDead = false;    //부정형보다는 긍정형이 더 나은 방식이다.</code></pre>
<hr />
<h2 id="변수를-사용하는-목적">변수를 사용하는 목적</h2>
<h3 id="1-값에-의미를-부여하기-위한-목적-가독성">1. 값에 의미를 부여하기 위한 목적 (가독성)</h3>
<pre><code class="language-java">        /* 목차 1. 값에 의미를 부여하기 위한 목적 (가독성) */
        /* 아래 처럼 작성하면 어느 값이 급여인지 어느 값이 보너스 금액인지 알 수 없다. */
        System.out.println(&quot;== 값에 의미 부여 테스트 ==&quot;);
        System.out.println(&quot;보너스를 포함한 급여 : &quot; + (1000000 + 200000) + &quot;원&quot;);

        /* 아래 처럼 의미를 부여하게 되면 어느 값이 급여인지, 보너스인지를 명확하게 알 수 있게 한다. */
        int salary = 1000000;
        int bonus = 200000;
        System.out.println(&quot;보너스를 포함한 급여 : &quot; + (salary + bonus) + &quot;원&quot;);
</code></pre>
<pre><code>그냥 숫자보다 훨씬 의미가 부각돼 다른 사람이 봐도 알 수 있다.</code></pre><hr />
<h3 id="2-한-번-저장해둔-값을-재사용하기-위한-목적">2. 한 번 저장해둔 값을 재사용하기 위한 목적</h3>
<pre><code class="language-java">        /* 목차 2. 한 번 저장해둔 값을 재사용하기 위한 목적 */
        System.out.println(&quot;== 변수에 저장한 값 재사용 테스트 ==&quot;);
        System.out.println(&quot;1번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;2번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;3번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;4번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;5번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;6번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;7번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;8번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;9번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);
        System.out.println(&quot;10번 고객에게 포인트를 100포인트 지급하였습니다.&quot;);

        System.out.println();

        int point = 100;
        System.out.println(&quot;1번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;2번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;3번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;4번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;5번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;6번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;7번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;8번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;9번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);
        System.out.println(&quot;10번 고객에게 포인트를 &quot; + point + &quot;포인트 지급하였습니다.&quot;);</code></pre>
<pre><code>똑같이 출력되지만 변수로 재사용 가능하다.</code></pre><hr />
<h3 id="시간에-따라-변하는-값을-저장하고-사용할-목적">시간에 따라 변하는 값을 저장하고 사용할 목적</h3>
<pre><code class="language-java">        /* 목차 3. 시간에 따라 변하는 값을 저장하고 사용할 목적 */
        System.out.println(&quot;=== 변수에 저장된 값 변경 테스트 ===&quot;);
        int sum = 0;

        sum = sum + 10;
        System.out.println(&quot;sum에 10을 더하면 현재 sum의 값은? : &quot; + sum);

        sum = sum + 10;
        System.out.println(&quot;sum에 10이 있었으니 10을 추가로 더하면? &quot; + sum);

        sum = sum + 10;
        System.out.println(&quot;sum에 20이 있었는데 추가로 10을 더 더하면? &quot; + sum);</code></pre>
<pre><code>0
10
20
30</code></pre><p>변하는 값을 저장하고 사용할 수 있다.</p>
<hr />
<h2 id="변수의-자료형">변수의 자료형</h2>
<ul>
<li>자료형이란?<blockquote>
<p>다양한 값의 형태별로 어느 정도의 크기를 하나의 값으로 취급할 것인지 미리 Compiler와 약속한 키워드이다.</p>
</blockquote>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6cbc0d34-9c71-4d7c-bc18-6cec602b1e44/image.png" /></p>
<blockquote>
<p>char형의 변수를 cNum -&gt; ch 로 변경</p>
</blockquote>
<p>기본자료형(<code>Primary type</code>)과 참조자료형(<code>Reference type</code>)으로 나누어진다.</p>
<ul>
<li>기본자료형 (<code>Primary type</code>)</li>
</ul>
<ol>
<li>정수형</li>
</ol>
<table>
<thead>
<tr>
<th>타입</th>
<th>할당되는 메모리 크기</th>
<th>데이터 표현 범위</th>
</tr>
</thead>
<tbody><tr>
<td>byte</td>
<td>1 바이트</td>
<td>$-128$ ~ $127$</td>
</tr>
<tr>
<td>short</td>
<td>2 바이트</td>
<td>$-2^{15}$ ~ $(2^{15} -1)$</td>
</tr>
<tr>
<td>int</td>
<td>4 바이트</td>
<td>$-2^{31}$ ~ $(2^{31} -1)$</td>
</tr>
<tr>
<td>long</td>
<td>8 바이트</td>
<td>$-2^{63}$ ~ $(2^{63} -1)$</td>
</tr>
</tbody></table>
<p>int를 대표자료형으로 여기고, 특수한 경우가 아닌 이상 byte, short는 잘 사용하지 않는다.</p>
<ol start="2">
<li>실수형</li>
</ol>
<table>
<thead>
<tr>
<th>타입</th>
<th>할당되는 메모리 크기</th>
<th>데이터 표현 범위</th>
</tr>
</thead>
<tbody><tr>
<td>float</td>
<td>4 바이트</td>
<td>$(3.4 * 10^{-38})$ ~ $(3.4 * 10^{38})$</td>
</tr>
<tr>
<td>double</td>
<td>8 바이트</td>
<td>$(1.7 * 10^{-308})$ ~ $(1.7 * 10^{308})$</td>
</tr>
</tbody></table>
<p>double을 대표 자료형으로 여기고, float는 특수한 목적이 있는 경우에만 사용하게 된다.</p>
<blockquote>
<p>정수형과 실수형 둘 다 맨 앞 비트를 <code>부호비트</code>로 둔다.</p>
</blockquote>
<ol start="3">
<li>문자형</li>
</ol>
<table>
<thead>
<tr>
<th>타입</th>
<th>할당되는 메모리 크기</th>
<th>데이터 표현 범위</th>
</tr>
</thead>
<tbody><tr>
<td>char</td>
<td>2 바이트</td>
<td>$0$ ~ $2^{16}$</td>
</tr>
</tbody></table>
<p>short와 동일하게 2바이트지만, 부호 비트 없이 16개의 비트를 모두 값 비트로 활용</p>
<ol start="4">
<li>논리형</li>
</ol>
<table>
<thead>
<tr>
<th>타입</th>
<th>할당되는 메모리 크기</th>
<th>데이터 표현 범위</th>
</tr>
</thead>
<tbody><tr>
<td>boolean</td>
<td>1 바이트</td>
<td>true 혹은 false</td>
</tr>
</tbody></table>
<p>실제 사용하는 비트가 8비트 중 가장 오른쪽 1비트만 이용을 하지만 실제 할당되는 크기는 1바이트를 할당한다.</p>
<hr />
<ul>
<li>참조 자료형</li>
<li>문자열
문자열을 저장하기 위한 자료형이라고 생각하고 사용하고 나중에 정리한다.</li>
</ul>
<hr />
<h3 id="값-대입해보기">값 대입해보기</h3>
<h4 id="정수형">정수형</h4>
<pre><code class="language-java">        bNum = 1;
        sNum = 2;
        iNum = 4;
        lNum = 6;   // lNum은 long 자료형이지만, 자바는 6을 보고 int로 본다.
                    // long 형이지만, lNum = 2200000000 (22억) 으로 실행하면 에러가 생긴다. -&gt; int로 보는데 넘어가니까.

        // lNum = 2200000000; &lt;- 오류 생김
        lNum = 2200000000L; // 이것처럼 꼭 붙여야 한다.</code></pre>
<h4 id="실수형">실수형</h4>
<pre><code class="language-java">        fNum = 3.14f;   // double형으로 인식되는 실수를 float에 담을 때는 f를 붙여야 한다.
        dNum = 3.555;</code></pre>
<h3 id="문자형">문자형</h3>
<pre><code class="language-java">        ch = 'a';
        ch = 97;</code></pre>
<h3 id="논리형">논리형</h3>
<pre><code class="language-java">        isTrue = true;</code></pre>
<hr />
<h2 id="변수를-활용한-합계-sum-평균-avg-출력">변수를 활용한 합계 (sum), 평균 (avg) 출력</h2>
<pre><code class="language-java">        /*  
        ## 설명. 변수를 활용한 합계 (sum), 평균 (avg) 출력*/
        int kor = 90;
        int math = 80;
        int eng = 75;

        int sum = kor + math + eng;
        System.out.println(&quot;합계 : &quot; + sum);

        // int avg = sum / 3; // 정수 나누기 정수를 하면 소수점이 날아가기 때문에
        double avg = sum / 3.0f; // 나눗셈은 일반적으로 실수 결과가 나오기에 double형
        System.out.println(&quot;평균 = &quot; + avg);</code></pre>
<hr />