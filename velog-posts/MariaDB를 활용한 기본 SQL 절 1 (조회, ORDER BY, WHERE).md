<p><a href="https://velog.io/@jojehuni_9759/MariaDB%EC%97%90-%EB%8C%80%ED%95%B4">MariaDB에 대해</a>
Select, From 부분은 이전에 간단하게 하였다.</p>
<p>이번에는 ORDER BY, WHERE 절에 대해서 알아보자.</p>
<h1 id="sql-query-실행순서">SQL query 실행순서</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/24ac68f3-9599-4b40-a822-c8527bb4ea6e/image.png" /></p>
<h2 id="order-by-절">ORDER BY 절</h2>
<p>ORDER BY 절은 정렬할 때 사용하는 절로
Ascending (오름차순) 과 Descending (내림차순) 2가지 방법으로 속성들을 정렬할 수 있다.</p>
<blockquote>
<p>정렬의 Default는 오름차순이다.</p>
</blockquote>
<ol>
<li><p>가격을 기준으로 오름차순 정렬하기</p>
<pre><code class="language-sql">SELECT
      menu_code
   , menu_name
   , menu_price
 FROM tbl_menu
ORDER BY menu_price ASC;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/aa633590-5ce4-4c9a-9495-24b80d74cdc0/image.png" /></p>
</li>
<li><p>가격을 기준으로 오름차순 (가격이 같다면 메뉴명의 내림차순으로 정렬)</p>
<pre><code class="language-sql">SELECT
      menu_code
   , menu_name
   , menu_price
 FROM tbl_menu
ORDER BY menu_price ASC, menu_name DESC;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bbfe1cef-5364-4ea1-b8b2-780512b482d3/image.png" /></p>
</li>
</ol>
<hr />
<h3 id="별칭alias을-이용한-정렬">별칭(alias)을 이용한 정렬</h3>
<p><code>AS</code> 을 활용해서 매번 길게 적을 필요 없다.
<code>menu_price</code> 를 <code>mp</code>로 별칭 활용해 내림차순 정렬해보았다.</p>
<pre><code class="language-sql">SELECT
         menu_code AS '메뉴코드'
      , menu_name AS 'MN'
      , menu_price AS 'MP'
    FROM tbl_menu
  ORDER BY mp DESC;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d8b1b0e6-182d-44af-9952-752211623ebf/image.png" /></p>
<hr />
<h3 id="field-를-활용한-정렬">field 를 활용한 정렬</h3>
<p><code>field</code>는 (orderable_status, 'N', 'Y') 에서 
orderable_status 가 <strong>N이면 1을, Y면 2를</strong> 넣어주는데 이것을 활용해서 N, Y로 정렬할 수 있다.</p>
<p><strong>주문 가능한 것부터 보이게 정렬하기</strong></p>
<pre><code class="language-sql">SELECT
         menu_name
      , orderable_status
  FROM tbl_menu
 ORDER BY FIELD(orderable_status, 'N', 'Y') DESC;</code></pre>
<p>내림차순으로 Y를 위로 올려보내줬다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/33e798ef-fc85-44c5-87e7-3facf0ba32ee/image.png" /></p>
<hr />
<h3 id="null-값에-대해-정렬하기">null 값에 대해 정렬하기</h3>
<pre><code class="language-sql">-- 1) 오름차순 식 - null 값이 먼저
SELECT
         *
  FROM tbl_category
 ORDER BY ref_category_code ASC;

-- 2) 내림차순 식 - null이 나중에
SELECT
         *
  FROM tbl_category
 ORDER BY ref_category_code DESC;

-- 3) 오름차순에서 null이 나중에 나오게 바꾸기
SELECT
         *
  FROM tbl_category
 ORDER BY -ref_category_code DESC;

 -- 4) 내림차순에서 null이 먼저 나오게 하기
SELECT
         *
  FROM tbl_category
 ORDER BY -ref_category_code ASC;</code></pre>
<p>(-)를 붙이면 데이터값은 반대로 정렬되지만 null 값은 그대로 유지된다.</p>
<ol start="3">
<li>내림차순인 DESC 지만 -가 붙어서 오름차순 + null값이 마지막</li>
<li>오름차순인 ASC 지만 -가 붙어서 내림차순 + null이 처음</li>
</ol>
<blockquote>
<p>즉 order by만 있다면 from -&gt; select -&gt; order by 순서대로 이뤄진다.</p>
</blockquote>
<hr />
<h2 id="where-절">Where 절</h2>
<p>내용에 따른 참조에 대한 내용을 해볼 것이다.
Where 절은 조건문이라고 생각하면 된다.</p>
<pre><code class="language-sql">SELECT
         menu_name
      , menu_price
      , orderable_status
  FROM tbl_menu
 WHERE orderable_status = 'Y';
 -- WHERE orderable_status = 'N';</code></pre>
<p>주문 상태가 주문 가능한지 조건문을 달아서 확인할 수 있다.</p>
<pre><code class="language-sql">--  '기타' 카테고리에 해당하지 않는 메뉴를 조회해보자
SELECT
         *
  FROM tbl_category
 WHERE category_name = '기타'; -- 이것으로 일단 '기타' 카테고리인 메뉴를 찾고

-- 해당하는 메뉴를 제외하고 조회하면 된다. (10번임)

SELECT
         *
  FROM tbl_menu
 WHERE category_code != 10;
 -- WHERE category_code &lt;&gt; 10;</code></pre>
<blockquote>
<p>!= 또는 &lt;&gt; 라는 표시는 같은 의미이다. (&quot;같지 않다&quot; 는 의미)
대소 비교 (&gt;, &lt;, &gt;=, &lt;=) 또한 where 절에서 사용 가능</p>
</blockquote>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE menu_price &gt;= 5000;
 -- ORDER BY menu_price ASC;</code></pre>
<p>이렇게 대소비교도 가능한데, 만약 price 가 5000보다 크거나 같고, 10000보다 작은걸 찾고 싶다면 어떻게 해야 할까?</p>
<p><code>WHERE 5000 &lt;= menu_price &lt; 10000</code> 하면 되는거 아닌가? -&gt; X 안 된다.
SQL은 삼항 연산이 되지 않는다. 그걸 해결하기 위해 있는 것이</p>
<p><strong>논리 연산자 AND, OR</strong> 이다. 당연히 이번에는 AND를 써야 한다.</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE menu_price &gt;= 5000 
   AND menu_price &lt; 10000
 ORDER BY menu_price ASC;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f55e5b95-a819-4136-8981-6ead5d7aea90/image.png" /></p>
<p>OR을 사용했을 때의 경우다. (10000원 초과 또는 5000원 이하인 메뉴)</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE menu_price &gt; 10000 
    OR menu_price &lt;= 5000
 ORDER BY menu_price ASC;</code></pre>
<hr />
<h3 id="between-연산">Between 연산</h3>
<p>&lt;=, &gt;= 로 나타낼 때 길어지는 것때문에 만든 연산자이다.</p>
<pre><code class="language-sql">-- 가격이 5000원 이상이면서 9000원 이하인 메뉴 전체 컬럼 조회
SELECT
         *
  FROM tbl_menu
 WHERE menu_price BETWEEN 5000 AND 9000;</code></pre>
<p>반대인 경우엔? -&gt; <code>not</code> 을 붙여라.</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE menu_price NOT BETWEEN 5000 AND 9000;</code></pre>
<hr />
<h3 id="like-문">Like 문</h3>
<p>제목, 작성자 등 DB 값 중에서 <strong>특정 단어를 검색</strong>할 때 사용한다.</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE menu_name LIKE '밥%';
 -- WHERE menu_name LIKE '%밥';
 -- WHERE menu_name LIKE '%밥%';</code></pre>
<p><code>밥%</code> : &quot;밥&quot;으로 시작하는 것을 검색할 때 ex) &quot;밥OOO&quot;
<code>%밥</code> : &quot;밥&quot;으로 끝나는 것을 검색할 때 ex) &quot;OOO밥&quot;
<code>%밥%</code> : &quot;밥&quot;이라는 글자가 들어가는 것을 검색할 때 ex) &quot;OO밥OO&quot;</p>
<hr />
<h3 id="in연산자">in연산자</h3>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE category_code = 5
    OR category_code = 8
    OR category_code = 10;</code></pre>
<p>이렇게 OR을 이용하는 것이 불편하기 때문에 in 연산자로 간단하게 나타낼 수 있다.</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu
 WHERE category_code IN (5, 8, 10);</code></pre>
<p>반대인 경우엔? -&gt; <code>not</code> 을 붙여라.</p>
<pre><code class="language-sql">```sql
SELECT
         *
  FROM tbl_menu
 WHERE category_code NOT IN (5, 8, 10);</code></pre>
<hr />
<h3 id="is-null-연산자-활용">IS NULL 연산자 활용</h3>
<p>상위 카테고리를 조회하는 방법으로 사용한다.</p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_category
 WHERE ref_category_code IS NULL;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7d3e5195-f0be-4c6a-b88d-9bb7404ad031/image.png" /></p>
<p>하위 카테고리를 조회하려면? -&gt; NOT을 붙여라. -&gt; <code>IS NOT NULL</code></p>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_category
 WHERE ref_category_code IS NOT NULL;</code></pre>
<hr />
<ul>
<li>각 테이블의 컬럼명을 빠르게 확인하는 방법<h2 id="desc-컬럼명-확인">DESC (컬럼명 확인)</h2>
<pre><code class="language-sql">DESC tbl_menu;</code></pre>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/49cf3e66-f5f6-49e1-9083-2a02a02067ea/image.png" /></li>
</ul>