<p>그룹 함수 (SUM, AVG, COUNT) 를 적용하기 위해서 GROUP BY를 사용한다.
컬럼에 있는 값을 이용해서 그룹을 만드는데 DISTINCT와 유사해보이긴 한다.</p>
<h2 id="group-by">GROUP BY</h2>
<h3 id="count-개수-세는-함수">COUNT (개수 세는 함수)</h3>
<pre><code class="language-sql">SELECT
         COUNT(*)
  FROM tbl_menu a
 GROUP BY a.category_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f0423dab-4e55-4aa4-9771-f5af8dbdd5df/image.png" /></p>
<hr />
<p>좀 더 추가하면? (concat 문자열 붙이는걸 사용해보았다.)</p>
<pre><code class="language-sql">SELECT
         COUNT(*)
      , CONCAT(a.category_code, '번 카테고리') AS '카테고리 번호'
  FROM tbl_menu a
 GROUP BY a.category_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7ccd190b-8982-45b0-8155-f06d7ba9e0bb/image.png" /></p>
<ul>
<li>GROUP BY 한 컬럼말고 다른 컬럼은 조회하지 않아야 한다. 
(MariaDB는 조회가 되지만 다른 곳은 오류가 난다.)</li>
</ul>
<blockquote>
<p><strong>COUNT메소드는 아래와 같이 사용해야 한다.</strong></p>
</blockquote>
<pre><code class="language-sql">1. count(컬럼명 or *)
2. count(컬럼명) 또는 해당 컬럼에 null이 아닌 행의 개수
3. count(*) 모든 행의 개수</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6ccd6b9d-b684-4ee4-a40f-e51d1fba2989/image.png" />
사진에서처럼 null 값이 3개가 있어서 상위 카테고리가 존재하는 카테고리는 3개 더 적다.</p>
<hr />
<h3 id="sum-함수">SUM 함수</h3>
<pre><code class="language-sql">SELECT
         category_code
      , SUM(menu_price)
  FROM tbl_menu
 GROUP BY category_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1409ff52-cace-4036-8401-22b63bb87733/image.png" /></p>
<p>category_code 로 그룹을 묶으면서 select로 카테고리 코드를 조회하고 그 때의 sum 함수를 활용해서 메뉴 가격을 합산해보았다.</p>
<hr />
<h3 id="avg-함수">AVG 함수</h3>
<pre><code class="language-sql">SELECT
         category_code
      , AVG(menu_price)
  FROM tbl_menu
 GROUP BY category_code;</code></pre>
<p>위의 코드로 sum이 아니라 평균을 구해보았다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/435c1736-4566-4d84-ae6d-6e1bfe968392/image.png" /></p>
<hr />
<h4 id="궁금증-해결">궁금증 해결</h4>
<p>어제 SQL에서 <code>SELECT 7 / 3;</code> 을 해보았는데, 2.333333... 이 아닌, <code>2.3333</code> 딱 소수점 4자리까지만 나왔다. 5번째 자리에서 반올림을 하는 것이었어서 5번째 자리는 0이기 때문에</p>
<pre><code class="language-sql">SELECT 7 / 3;
  WHERE 7 / 3 = 2.33333;</code></pre>
<p>해도 나오지 않는다.</p>
<blockquote>
<p><code>SELECT 7 / 3;</code> = '2.33330' 으로 조회가 된다.</p>
</blockquote>
<p>그래서 위에서 평균에서도 소수 4번째 자리는 반올림에 의해 7이 나오게 된다.</p>
<hr />
<h2 id="having-절">HAVING 절</h2>
<p>GROUP BY 절 속에서의 조건문이라고 생각하면 편하다.
<strong>그룹 별로 조건이 가능</strong>하다.</p>
<blockquote>
<p>where : 한 행씩 조건 (ex.직원 별로 조건)
group by : 그룹별로 조건 (ex.부서 별로 조건)</p>
</blockquote>
<pre><code class="language-sql">SELECT
         SUM(menu_price)
      , category_code
  FROM tbl_menu
 GROUP BY category_code
 HAVING category_code BETWEEN 5 AND 9;</code></pre>
<p>그룹화한 category_code 가 5~9인 것들 중 메뉴 합산이 sum인 것들을 조회할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8852e8ad-fe8f-4d5c-8096-9b8ddce7e40a/image.png" /></p>
<blockquote>
<p>이 때는 where절을 사용해도 나온다. 그룹 함수를 사용하지 않기 때문이다.</p>
</blockquote>
<hr />
<p>그럼 아래는 어떻게 나올까?</p>
<pre><code class="language-sql">SELECT
         SUM(menu_price)
      , category_code
  FROM tbl_menu
 GROUP BY category_code
 HAVING SUM(menu_price &gt;= 20000);</code></pre>
<p><code>SUM(menu_price &gt;= 20000);</code> 인 그룹을 가져오게 될 것이다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/59204458-4da5-4e30-86af-7addaa7f8f4a/image.png" /></p>
<blockquote>
<p>이 때는 그룹함수 중 하나인 SUM을 사용하기 때문에 where 절로 나타내면 안 된다.</p>
</blockquote>
<hr />
<h2 id="rollup">Rollup</h2>
<p>그룹을 묶을 때 <strong>하나의 기준 (하나의 컬럼)으로 그룹화</strong>하여 Rollup을 하면 총 합계의 개념이 나오게 된다.</p>
<pre><code class="language-sql">SELECT
         SUM(menu_price)
      , category_code
  FROM tbl_menu
 GROUP BY category_code
  WITH ROLLUP;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7a2c03f4-b4a8-443d-85ae-6cea9cee4e77/image.png" /></p>
<hr />
<blockquote>
<p><strong>여러 개의 기준도 가능</strong>하다.</p>
</blockquote>
<p>컬럼을 2개 이상 써보자.</p>
<pre><code class="language-sql">SELECT
         SUM(menu_price)
      , menu_price
      , category_code
  FROM tbl_menu
 GROUP BY menu_price, category_code
  WITH ROLLUP;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8e1cf8ea-95cb-4c59-b45f-b9a3240080f1/image.png" /></p>
<p>이렇게 메뉴 가격과 카테고리 코드로 그룹화하면 위 사진처럼 중간 합계가 이어지다가</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fc6aa121-4527-4663-8ff6-8bbde6f19dbb/image.png" /></p>
<p>마지막에 최종 합계가 따로 또 나온다. (중간에 생략돼있다.)</p>