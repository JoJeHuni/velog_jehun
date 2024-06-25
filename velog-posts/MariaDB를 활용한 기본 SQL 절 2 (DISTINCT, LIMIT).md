<p><a href="https://velog.io/@jojehuni_9759/MariaDB%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EA%B8%B0%EB%B3%B8-SQL-%EC%A0%88-1">MariaDB를 활용한 기본 SQL절 1</a></p>
<p>위 게시글에 이어 마저 조회에 관련된 것들에 대해 알아보자.</p>
<h2 id="distinct">DISTINCT</h2>
<p>간단하게 말해 조회할 때 중복되는 값들을 없애고 1개 값만 나타낼 때 사용한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/64d98336-7b78-4e29-b3c8-dfe4f3d4c479/image.png" /></p>
<p>Distinct 없이는 값이 중복돼 나오지만</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7513a48d-087e-4f13-ac75-1292e09a6a40/image.png" /></p>
<p>중복을 없앨 수 있다.</p>
<hr />
<h3 id="다중-열-distinct">다중 열 DISTINCT</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e04b386e-730f-467f-8238-1fe891aa5782/image.png" /></p>
<blockquote>
<p>SELECT 절에서 맨 앞에 DISTINCT 를 적으면 하나만 중복이 없어지는 것이 아니라, <strong>여러 개의 열을 적어도 같이 중복 제거가 된다.</strong></p>
</blockquote>
<p>(자주 쓰이지는 않는다)</p>
<hr />
<h2 id="limit">LIMIT</h2>
<p>페이징 처리나 원하는 범위만큼 잘라서 보내주고 싶을 때 사용하는 LIMIT이다.</p>
<p>LIMIT은 조회하고 싶은 개수를 선정하고 싶을 때 ORDER BY 절에서 사용하는 것이다.
예를 들어 menu 테이블 안에 행들을 가격순으로 조회해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a033ad08-8bdb-44b5-a8dc-5bea40048dbe/image.png" /></p>
<p>여기에서 내가 <strong>상위 3개</strong>만 골라오고 싶다면?</p>
<pre><code class="language-sql">SELECT
         menu_code
      , menu_name
      , menu_price
  FROM tbl_menu
 ORDER BY menu_price DESC
 LIMIT 3;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/77608bc3-f401-40bd-8d68-8af3e1b4910e/image.png" /></p>
<hr />
<p>만약 5~7번까지 가져오고 싶다면?
LIMIT은 index 형태로 나타내기 때문에 5번부터 3개라는건
인덱스로 4, 가져오는건 3
즉, LIMIT 4, 3 으로 나타내면 된다.</p>
<pre><code class="language-sql">SELECT
         menu_code
      , menu_name
      , menu_price
  FROM tbl_menu
 ORDER BY menu_price DESC
 LIMIT 4, 3;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/943ff7e8-e372-4cce-86ad-2fc3236fb5af/image.png" /></p>
<hr />
<h3 id="limit-시-재정렬에-대한-궁금증">LIMIT 시 재정렬에 대한 궁금증</h3>
<p>여기서 궁금증이 생겼다.
원래 전체 행 조회했을 때는 13000원으로 가격이 같은 행은 ORDER BY 로 정렬하면</p>
<pre><code>14 과메기 커틀릿
5 앙버터김치찜</code></pre><p>순서였는데 LIMIT 4, 3 으로 잘랐을 때는</p>
<pre><code>5 앙버터김치찜
14 과메기 커틀릿</code></pre><p>이것과 같은 순서가 된다. 왜지??</p>
<p>알아보니 LIMIT은 ORDER BY된 것들을 가지고 또 정렬을 하는데,
LIMIT이 없을 때와 LIMIT이 있을 때의 정렬하는 내부 알고리즘에서 약간의 차이가 있어서 생기는 순서변화라고 한다.</p>
<p>순서가 중요하다면 ORDER BY에 하나 더 속성을 추가해야된다고 한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b6bd78b7-c06f-42f1-a828-eb03b649bbab/image.png" /></p>
<blockquote>
<p>참고 링크 : <a href="https://dev.mysql.com/doc/refman/5.7/en/limit-optimization.html">https://dev.mysql.com/doc/refman/5.7/en/limit-optimization.html</a></p>
</blockquote>
<blockquote>
<p>즉, <strong>LIMIT이 포함된 ORDER BY랑 단순 ORDER BY는 정렬 기준 컬럼의 값이 같으면 약간의 순서 차이가 있을 수 있다</strong></p>
</blockquote>