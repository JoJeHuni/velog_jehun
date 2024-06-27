<h1 id="index">INDEX</h1>
<p>검색 속도를 높이기 위해 사용하는 하나의 기술로 인덱스를 사용한다.</p>
<blockquote>
<p>PK는 자동으로 인덱스가 부여된다. (고유 식별자이므로)</p>
</blockquote>
<p><strong>장점</strong></p>
<ol>
<li>검색 속도가 빠르다</li>
<li>테이블 행의 고유성 강화</li>
</ol>
<p><strong>단점</strong></p>
<ol>
<li>DB의 추가적인 공간 필요</li>
<li>인덱스 된 필드에서 데이터를 변동할 때 성능이 떨어진다.</li>
<li>여러 사용자 응용 프로그램에서의 여러 사용자가 한 페이지를 동시에 수정할 수 있는 병행성이 줄어든다.</li>
</ol>
<hr />
<h2 id="응용">응용</h2>
<p>그럼 일단 phone 테이블을 만들고, 데이터를 넣어서 조회해보자.</p>
<pre><code class="language-sql">CREATE TABLE phone (
  phone_code INT PRIMARY KEY,
  phone_name VARCHAR(100),
  phone_price DECIMAL(10,2)
);

INSERT
  INTO phone
VALUES
(1, 'galaxyS24', 1200000),
(2, 'iphone14pro', 1430000),
(3, 'galaxyfold3', 1730000);

SELECT * FROM phone;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/11e042f6-34a1-4fff-94f0-86ca8f8f3616/image.png" /></p>
<blockquote>
<p><code>phone_codd</code> -&gt; <code>phone_code</code>
<code>galaxy24</code> -&gt; <code>galaxyS24</code></p>
</blockquote>
<hr />
<p><strong>where 절을 활용한 단순 조회</strong></p>
<pre><code class="language-sql">SELECT * FROM phone WHERE phone_name = 'galaxyS24';</code></pre>
<pre><code class="language-sql">EXPLAIN SELECT * FROM phone WHERE phone_name = 'galaxyS24';</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cf526210-6417-48ad-a509-643144479e90/image.png" /></p>
<hr />
<h3 id="index-추가"><code>Index</code> 추가</h3>
<p><code>where</code> 절과 마찬가지로 <code>phone_name</code> 으로 <code>Index</code> 만들어보자.</p>
<pre><code class="language-sql">CREATE INDEX idx_name ON phone(phone_name);

SHOW INDEX FROM phone;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ad7b3bf3-1b16-4857-993b-bbb47c969554/image.png" /></p>
<p>이렇게 보이는데, <code>cardinality</code> 는 <strong>전체 행에 대한 특정 컬럼의 중복 수치를 나타내는 지표</strong>이다.</p>
<blockquote>
<p>Cardinality : 구분할 수 있는 개수 를 의미한다.
즉, <code>phone_code</code> 는 <code>1, 2, 3</code> -&gt; 3개
<code>phone_name</code> : <code>galaxyS24, iphone14pro, galaxyfold3</code> -&gt; 3개
<code>phone_price</code> : ... -&gt; 3개
구분할 수 있는 개수가 3개로 동일한 상태이므로 3으로 나온다.</p>
</blockquote>
<blockquote>
<p><strong>Cardinality 추가 예시)</strong>
마커의 색상 : 빨간색, 파란색, 초록색, 노란색, 주황색 -&gt; 5개
사용하는 프로그래밍 언어 : Java, Python, Java, C, C -&gt; 3개</p>
</blockquote>
<hr />
<p>다시 index가 추가된 column으로 조회해서 index를 태웠는지 확인해보자.</p>
<pre><code class="language-sql">SELECT * FROM phone WHERE phone_name = 'galaxyS24';
EXPLAIN SELECT * FROM phone WHERE phone_name = 'galaxyS24';</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f2d82ccf-3bbe-4e6d-b8a4-d8f816197cb6/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/af046f4d-7ea4-489d-a565-0d9fea80157e/image.png" /></p>
<hr />
<h3 id="index-최적화">INDEX 최적화</h3>
<p>주기적으로 한 번씩 index를 rebuild 해줘야 한다.</p>
<ul>
<li>인덱스 최적화(재구성)은 인덱스가 파편화되었거나, 데이터의 대부분이 변경된 경우에 유용하다.</li>
<li>이는 인덱스의 성능을 개선하고, 디스크 공간을 더 효율적으로 사용하게 해준다.</li>
<li>단, 인덱스를 재구성하는 동안 해당 테이블은 잠길 수 있으므로, 이 작업은 주의해서 수행해야 한다.</li>
</ul>
<pre><code class="language-sql">ALTER TABLE phone DROP INDEX idx_name;
ALTER TABLE phone ADD INDEX idx_name(phone_name);</code></pre>
<blockquote>
<p>MySQL의 INNODB 를 사용하는 경우에는 최적화를 간단한 명령으로 할 수 있다.</p>
</blockquote>
<pre><code class="language-sql">OPTIMIZE TABLE phone;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/68049690-2e08-41c5-baaa-8c333ff6cd64/image.png" /></p>