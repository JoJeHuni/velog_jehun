<h1 id="오버플로우">오버플로우</h1>
<blockquote>
<p>오버플로우 : 변수가 담을 수 있는 값의 범위를 벗어난 데이터를 담았을 때 발생하는 현상</p>
</blockquote>
<p>자료형 별 값의 최대 범위를 벗어나면 발생한 carry를 버림처리, sign bit를 반전시켜 최솟값으로 순환시키는 현상</p>
<p>byte로 된 데이터 자료형일 때 <code>01111111</code> = +127 (맨 앞은 부호 비트) 에 +1을 해주면 어떻게 될까?</p>
<blockquote>
<p><code>10000000</code> = 부호 비트가 1이므로 (-)에 나머지가 0이니까 -0 일까?</p>
</blockquote>
<p><strong>아니다. 위에서 말한 대로 최솟값으로 순환</strong>한다.
<strong>즉, -128이다.</strong> 왜 그런지에 대해 알아보자.</p>
<p>각 자료형의 범위를 눈으로 확인하는 방법</p>
<pre><code class="language-java">System.out.println(&quot;byte 값의 범위 : &quot; + Byte.MIN_VALUE + &quot; ~ &quot; + Byte.MAX_VALUE);
System.out.println(&quot;char 값의 범위 : &quot; + (int)Character.MIN_VALUE + &quot; ~ &quot; + (int)Character.MAX_VALUE);
System.out.println(&quot;short 값의 범위 : &quot; + Short.MIN_VALUE + &quot; ~ &quot; + Short.MAX_VALUE);
System.out.println(&quot;int 값의 범위 : &quot; + Integer.MIN_VALUE + &quot; ~ &quot; + Integer.MAX_VALUE);
System.out.println(&quot;long 값의 범위 : &quot; + Long.MIN_VALUE + &quot; ~ &quot; + Long.MAX_VALUE);
System.out.println(&quot;float 값의 범위 : &quot; + Float.MIN_VALUE + &quot; ~ &quot; + Float.MAX_VALUE);
System.out.println(&quot;double 값의 범위 : &quot; + Double.MIN_VALUE + &quot; ~ &quot; + Double.MAX_VALUE);</code></pre>
<h2 id="오버플로우-발생-확인-및-해결-방법">오버플로우 발생 확인 및 해결 방법</h2>
<h3 id="오버플로우-1">오버플로우</h3>
<pre><code class="language-java">        byte num1 = 126;

        System.out.println(&quot;num1 + 1 = &quot; + ++num1);</code></pre>
<pre><code>num1 + 1 = 127 // 127까지는 범위 안에 속해서 발생 X</code></pre><pre><code class="language-java">        byte num2 = 127;

        System.out.println(&quot;num2 + 1 = &quot; + ++num2);</code></pre>
<pre><code>num2 + 1 = -128 // 최대치를 넘어 오버플로우 발생</code></pre><h3 id="언더플로우">언더플로우</h3>
<pre><code class="language-java">        byte num3 = -128;

        System.out.println(&quot;num3 - 1 = &quot; + --num3);</code></pre>
<pre><code>num3 - 1 = 127 // 반대로 너무 작아져서 최대치로 순환하는 언더플로우 발생</code></pre><p>간단한 해결 방법은 <strong>값의 범위를 생각해 너무 작은 자료형을 사용하지 않으면 된다.</strong></p>
<h3 id="논리적-문제-발생과-해결-방법">논리적 문제 발생과 해결 방법</h3>
<hr />
<h1 id="형변환">형변환</h1>
<blockquote>
<p>형변환 : 변수 또는 리터럴을 다른 타입으로 변환하는 것</p>
</blockquote>
<ul>
<li>형변환하는 이유
프로그램에서 변수에 값을 넣거나 연산을 수행할 때는 같은 타입끼리만 가능하기 때문이다.</li>
</ul>
<h2 id="형변환의-종류-및-규칙">형변환의 종류 및 규칙</h2>
<p>이전에도 다뤘던 형변환에 대한 내용이지만 간단하게 해본다.</p>
<h3 id="자동-형변환-묵시적-형변환">자동 형변환 (묵시적 형변환)</h3>
<p>컴파일러가 자동으로 수행해주는 타입 변환.
데이터 손실 가능성이 없는 경우 자동으로 타입을 맞춰준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9d95277c-3e45-4da8-ba61-5c6869071c00/image.png" /></p>
<ul>
<li><p><strong>작은 자료형 -&gt; 큰 자료형</strong></p>
<blockquote>
<p>자동 형변환이 가능하다. 데이터 손실이 없기 때문에</p>
</blockquote>
<pre><code>      /* 설명. 연산 시에도 작은 자료형이 큰 자료형으로 형변환된다. */
      int num1 = 10;
      long num2 = 20L;

      long result = num1 + num2;
      System.out.println(&quot;result = &quot; + result);</code></pre><pre><code>result = 30</code></pre></li>
<li><p><strong>정수는 실수로 자동 형변환된다.</strong></p>
<pre><code class="language-java">      /* 정수를 실수로 변경할 때 소수점 자리수가 없어도 실수형태로 표현이 가능하다. 이 때 데이터 손실이 없기 때문에 자동 형변환이 가능하다.
       * 실제 값을 저장하는 매커니즘을 가진 것과 달리 실수형은 지수부와 가수부를 따로 나눠서 작성하기 때문에
       * 바이트 크기 보다 훨씬 더 많은 값을 표현할 수 있다.
       * */
      //long eight = 888888888888888888888;                    //이것도 지동으로 형변환 된 것이다. (int 범위 벗어나면 에러 발생)
      long eight = 8;
      float four = eight;

      System.out.println(&quot;four : &quot; + four);

      /* 따라서 실수와 정수의 연산은 실수로 연산 결과가 반환된다. */
      float result3 = eight + four;

      System.out.println(&quot;result3 : &quot; + result3);</code></pre>
<pre><code>four : 8.0
result3 : 16.0</code></pre></li>
<li><p><strong>문자형은 int형으로 자동 형변환</strong>된다.</p>
<pre><code class="language-java">      char ch1 = 'a';
      int charNum = ch1;

      System.out.println(&quot;charNum : &quot; + charNum);

      /* int로 type이 정해지지 않은 리터럴 형태의 정수는 char 형 변수에 기록 가능하다. (예외 케이스) */
      char ch2 = 65;

      System.out.println(&quot;ch2 : &quot; + ch2);</code></pre>
<pre><code>charNum : 97
ch2 : A</code></pre></li>
</ul>
<hr />
<h2 id="강제-형변환-암시적-형변환">강제 형변환 (암시적 형변환)</h2>
<p>형변환 (casting) 연산자를 이용한 강제적으로 수행하는 형변환
자동형변환의 조건과 정반대인 경우 강제 형변환을 사용</p>
<ul>
<li><p><strong>큰 자료형 -&gt; 작은 자료형 변경 시 강제 형변환 필요</strong></p>
<pre><code class="language-java">      long lNum = 80000000000L;
      int iNum = (int) lNum;

      System.out.println(&quot;iNum = &quot; + iNum);</code></pre>
<pre><code>iNum = -1604378624</code></pre><p>큰 자료형 -&gt; 작은 자료형 변경할 때 부호비트에 따라 -부호가 돼 이런 결과가 출력됐다.</p>
</li>
</ul>
<p>거의 이런 경우에는 소수점을 없애고 싶을 때나 사용하는 것 정도밖에 안 된다.</p>
<blockquote>
<p>어차피 소수점 없애거나 하는건 math 라이브러리의 메서드 사용하기.</p>
</blockquote>
<pre><code class="language-java">        float avg = 31.235f;
        int floatNum = (int) avg; // 강제형변환 시
        System.out.println(&quot;floatNum = &quot; + floatNum); // 소수점이 버려진다.</code></pre>
<pre><code>floatNum = 31</code></pre><ul>
<li><p>문자형을 int미만 크기의 변수에 저장할 때 </p>
<pre><code class="language-java">      char ch = 'a';
      byte bnum2 = (byte) ch;                //당연히 char 자료형보다 작은 크기이니 강제형변환을 해야 한다.
      short snum2 = (short) ch;            //같은 2byte이지만 부호비트(sign bit)로 인한 값의 범위가 다르기 때문에 강제 형변환을 해 주어야 한다.

      /* 추가적으로 정수를 char 자료형에 강제 형변환해서 대입하기 테스트 */
      int num1 = 97;
      int num2 = -97;

      char ch2 = (char) num1;
      char ch3 = (char) num2;                //음수도 강제 형변환 하면 대입할 수 있다.

      System.out.println(&quot;ch2 : &quot; + ch2);
      System.out.println(&quot;ch3 : &quot; + ch3);</code></pre>
<pre><code>ch2 : a
ch3 : ﾟ</code></pre></li>
</ul>
<blockquote>
<p>논리형은 강제 형변환도 안 된다.</p>
</blockquote>