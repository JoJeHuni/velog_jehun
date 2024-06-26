<h1 id="set-operator">SET OPERATOR</h1>
<pre><code class="language-sql">SELECT
        menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE category_code = 10;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a638fc33-1fb6-4db4-9121-9e27fd5c88f9/image.png" /></p>
<p>위 코드는 6개 행의 데이터가 조회되고,</p>
<pre><code class="language-sql">SELECT
        menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE menu_price &lt; 9000;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8eb07245-50d4-45fc-82c3-b6b7a8b835d3/image.png" /></p>
<p>위 코드는 9개 행의 데이터가 조회된다.</p>
<p>이 컬럼의 수는 같지만 데이터 개수가 다른 것을 합집합할 수 있는데 바로 <code>UNION</code> 키워드를 활용하는 것이다.</p>
<h2 id="합집합-union-union-all">합집합 UNION, UNION ALL</h2>
<pre><code class="language-sql">SELECT
        menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE category_code = 10
UNION
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE menu_price &lt; 9000;</code></pre>
<p>2개의 쿼리 사이에 UNION을 넣으면 총 10개의 행이 조회된다.</p>
<blockquote>
<p><code>UNION</code>은  <strong>중복은 지우면서 합해준다</strong> (<strong>합집합</strong> 이라고 생각하면 편하다.)
<code>UNION ALL</code> 은 <strong>중복도 보인다.</strong> -&gt; 즉 15개의 행이 보이게 된다.</p>
</blockquote>
<hr />
<h2 id="intersect-교집합">INTERSECT (교집합)</h2>
<blockquote>
<p>MySQl과 MariaDB는 INTERSECT가 공식적으로 지원되지 않는다. (파랑색으로 표시가 안 된다.)</p>
</blockquote>
<h3 id="1-inner-join-활용">1) INNER JOIN 활용</h3>
<pre><code class="language-sql">SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
  JOIN (SELECT b.menu_code
               , b.menu_name
               , b.menu_price
             , b.category_code
             , b.orderable_status
          FROM tbl_menu b
         WHERE b.menu_price &lt; 9000
       ) AS c
    ON a.menu_code = c.menu_code
 WHERE a.category_code = 10;</code></pre>
<ol>
<li>JOIN문 안에서 조회되는 것은 <code>menu_price</code> 가 9000인 것들</li>
<li>그 때, <code>menu_code</code>가 같은 애들만 조인해주고</li>
<li><code>category_code</code> 가 10인 것들을 보는 것이다.</li>
</ol>
<p>(그림 설명 추가)</p>
<hr />
<h3 id="2-in-연산자-활용">2) in 연산자 활용</h3>
<p>위와 같은 문제를 in 연산자로 활용해보자.</p>
<pre><code class="language-sql">SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
 WHERE a.category_code = 10
   AND a.menu_code IN (SELECT b.menu_code
                         FROM tbl_menu b
                        WHERE b.menu_price &lt; 9000);</code></pre>
<hr />
<h2 id="차집합-minus">차집합 (MINUS)</h2>
<p>첫 번째 SELECT 문의 결과에서 두 번째 SELECT 문의 결과가 포함하는 레코드를 제외한 레코드를 반환하는 SQL 연산자이다.</p>
<blockquote>
<p>MySQL은 MINUS를 제공하지 않는다. 하지만 LEFT JOIN을 활용해서 구현하는 것은 가능하다.</p>
</blockquote>
<h3 id="left-join-으로-구현">LEFT JOIN 으로 구현</h3>
<pre><code class="language-sql">SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
      , c.*
  FROM tbl_menu a
  LEFT JOIN (SELECT b.menu_code
                    , b.menu_name
                    , b.menu_price
                  , b.category_code
                  , b.orderable_status
               FROM tbl_menu b
              WHERE b.menu_price &lt; 9000
            ) AS c
     ON a.menu_code = c.menu_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/074aa923-c13e-4222-9862-229b40d8efee/image.png" /></p>
<p>이렇게 데이터가 있을 때 차집합을 구하려면 아래처럼 조건을 더 달아야 한다.</p>
<pre><code class="language-sql">/* set operator */

/* UNION */
-- 6개의 행
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE category_code = 10;

-- 9개의 행
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE menu_price &lt; 9000;

-- 위 2개의 sql문은 컬럼의 수는 같지만 데이터의 개수가 다르다
-- union을 활용해서 중복인 데이터를 지우면서 합집합할 수 있다.
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE category_code = 10
UNION
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE menu_price &lt; 9000;

-- 10개 행이 나온걸 보니 겹치는게 5개 데이터가 있다는 뜻이다.

/* INTERSECT (교집합) */
-- MySQl과 MariaDB는 INTERSECT가 공식적으로 지원되지 않는다. (파랑색으로 표시가 안 됨)
-- 1) inner join 활용
SELECT
         a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
  JOIN (SELECT b.menu_code
                   , b.menu_name
                   , b.menu_price
                 , b.category_code
                 , b.orderable_status
             FROM tbl_menu b
            WHERE b.menu_price &lt; 9000
         ) AS c
     ON a.menu_code = c.menu_code
 WHERE a.category_code = 10;

-- 2) in 연산자 활용
SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
 WHERE a.category_code = 10
     AND a.menu_code IN (SELECT b.menu_code
                                 FROM tbl_menu b
                                WHERE b.menu_price &lt; 9000);

-- 3) LEFT JOIN 활용                                
SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
  LEFT JOIN (SELECT b.menu_code
                           , b.menu_name
                           , b.menu_price
                         , b.category_code
                         , b.orderable_status
                     FROM tbl_menu b
                  WHERE b.menu_price &lt; 9000
                 ) AS c
     ON a.menu_code = c.menu_code
 WHERE a.category_code = 10
   AND c.menu_code IS NULL;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f764d18d-8967-4314-a758-cb223b0838c8/image.png" /></p>
<p>이런 식으로도 사용이 가능하다. SELECT 가 끝나고 다음 게시글은 DML부터 다뤄보겠다.</p>