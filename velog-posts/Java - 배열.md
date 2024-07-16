<h1 id="배열">배열</h1>
<blockquote>
<p><strong>배열</strong> : <strong>동일한 자료형의 묶음</strong></p>
<ul>
<li>heap 영역에 <strong>new</strong> 연산자를 통해 할당된다.</li>
<li><strong>배열의 길이는 최초 선언한 값으로 &quot;고정&quot;</strong>되고, 인덱스를 통해 데이터에 접근할 수 있다.</li>
</ul>
</blockquote>
<h2 id="배열-사용하는-이유">배열 사용하는 이유</h2>
<p>배열을 사용하지 않으면 동일한 자료형을 가진 다양한 값들을 각각의 변수에 저장해서 사용해야 한다.</p>
<blockquote>
<p>변수의 선언을 줄여주고, 반복문 등을 이용해 계산과 같은 과정을 쉽게 할 수 있다.</p>
</blockquote>
<hr />
<h2 id="1차원-배열">1차원 배열</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3fb0f901-0222-4efb-a45a-2953ce98517b/image.png" />
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/64fa8d6d-e079-4304-9681-c0e89a7bd58d/image.png" /></p>
<p>사진과 같이 선언할 수 있다.</p>
<ol>
<li>참조 변수만 먼저 선언 -&gt; 크기 및 값을 이후에 초기화</li>
<li>최초 선언 시 배열의 크기 및 값을 할당</li>
</ol>
<p>2가지 방법이 있다.</p>
<hr />
<h2 id="다차원-배열-개요">다차원 배열 개요</h2>
<h3 id="2차원-배열-선언">2차원 배열 선언</h3>
<p>2차원 배열 이상의 배열도 선언 가능하지만 그다지 사용하지는 않는다.</p>
<blockquote>
<p><strong>2차원 배열</strong> : 1차원 배열 여러 개를 하나로 묶어서 관리하는 배열</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9195e8b5-a786-4b61-8d1d-2f267a572676/image.png" />
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9f73c82d-716f-4e73-9f5e-8836877338d7/image.png" />
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/84a3cb0a-f364-4d82-a1e0-4921bf5eac2c/image.png" /></p>
<hr />
<h1 id="배열의-복사">배열의 복사</h1>
<p>배열은 목적에 따라 복사를 해야할 수도 있다.</p>
<h2 id="배열-복사의-종류">배열 복사의 종류</h2>
<ol>
<li><strong>얕은 복사</strong> (shallow copy) : stack의 주소값만 복사 -&gt; 원본을 공유할 목적</li>
<li><strong>깊은 복사</strong> (deep copy) : heap의 배열에 저장된 값을 복사 -&gt; 원본과 사본을 분리하여 관리할 목적</li>
</ol>
<hr />
<h3 id="얕은-복사-개요">얕은 복사 개요</h3>
<blockquote>
<p><strong>얕은 복사</strong> : stack에 저장되어 있는 배열의 주소값만 복사
즉, 2개의 레퍼런스 변수는 동일한 배열의 주소값을 가지고 있게 돼 <strong>하나의 변수에서 배열의 값을 수정하게 되면 다른 변수에서도 같이 변경된다.</strong></p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fa620d9d-b11e-4d25-8034-88233e286cd6/image.png" /></p>
<hr />
<h3 id="깊은-복사-개요">깊은 복사 개요</h3>
<blockquote>
<p><strong>깊은 복사</strong> : heap에 생성된 배열이 가지고 있는 값을 또 다른 배열에 복사해놓은 것
<strong>서로 같은 값을 가지고 있지만, 다른 배열이어서 영향을 주지 않는다.</strong></p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f0375c7d-acab-4b8f-8a7f-9c62f598f257/image.png" /></p>