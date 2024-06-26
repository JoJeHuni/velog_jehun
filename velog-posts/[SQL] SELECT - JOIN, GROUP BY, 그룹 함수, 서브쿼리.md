<p>set operator, join의 차이 같은 개념은 꼭 알아야 한다.
그러기 위해 join을 알아보고 그룹함수를 많이 다루지는 않고 가볍게 알아보자. 
추후에 그룹함수에 대해서도 작성할 것이다.</p>
<p>JOIN은 2개 이상의 테이블을 공통 속성을 묶어서 하나의 테이블로 나타낼 때 사용할 수 있다.
이전에 나왔던 AS (별칭) 을 활용해서 2개의 테이블을 간단하게 나타내면서 해볼 것이다.</p>
<h2 id="inner-join--join">INNER JOIN (== JOIN)</h2>
<p>Mapping 되는 것들만 묶어줄 때 사용하는 내부 조인이다.</p>
<pre><code class="language-sql">SELECT
         category_name
      , menu_name
  FROM tbl_menu a
 INNER JOIN tbl_category b
 ON a.category_code = b.category_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/83ac7687-ea7c-4003-a2f5-ae1958d5ed52/image.png" /></p>
<p>간단하게 <code>tbl_menu</code>를 <code>a</code>라는 별칭, <code>tbl_category</code>를 <code>b</code>라는 별칭으로 나타낸다.</p>
<h3 id="on-절">ON 절</h3>
<p>위에서 쓰인 <strong>on절은 조건이 만족하지 않으면 (true가 아니면) 조인을 하지 않는다.</strong>
즉, <code>a.category_code = b.category_code</code>가 만족하지 않으면 조인이 안 된다.
(같은 카테고리 코드가 없으면 안 한다는 뜻)</p>
<p>그리고 <strong>조건에 만족하면 그에 맞춰서 JOIN을 해준다.</strong></p>
<p>그래서 카테고리 코드를 맞춰서 조인했기 때문에 메뉴마다 하나의 카테고리 이름으로 연결지어진 것이다.</p>
<hr />
<h3 id="select-에-특정한-것이-아닌-전체를-조회한다면">SELECT 에 특정한 것이 아닌 전체를 조회한다면?</h3>
<pre><code class="language-sql">SELECT
         *
  FROM tbl_menu a
 INNER JOIN tbl_category b
 ON a.category_code = b.category_code;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f7eda4d4-36a2-4144-8324-2aa16ac0ef03/image.png" /></p>
<p>join은 그냥 <code>menu</code> 테이블에 <code>category</code> 테이블을 이어붙여준 것이다.
위에서는 <code>category_name</code>, <code>menu_name</code> 만 조회했기 때문에 간단하게 나왔던 것이다.</p>
<p>사진에서처럼 category_code가 2개가 보이게 되는데, 둘은 같은 속성이기 때문에 2개가 보일 이유가 없다. </p>
<p><strong>별칭을 활용해서 하나만 조회</strong>하면 된다.</p>
<hr />
<h3 id="using">USING</h3>
<p>on과 관련있지만 권장되지는 않는 using 방식도 있다.</p>
<pre><code class="language-sql">SELECT
         b.category_name
      , a.menu_name
      , a.category_code
  FROM tbl_menu a
 INNER JOIN tbl_category b USING (category_code);</code></pre>
<p>이렇게 해도 category_code만 컬럼이 추가됐을 뿐 결과 자체는 다르지 않다.
권장되지 않는 이유는 <strong>join할 테이블들의 mapping column 명이 동일할 때만 사용 가능한 문법</strong>이기 때문이다.</p>
<hr />
<h2 id="outer-join">OUTER JOIN</h2>
<blockquote>
<p>조건에 부합하지 않는 행도 포함시켜 결합하는 것</p>
</blockquote>
<p>기본적으로 LEFT, RIGHT or FULL이 OUTER JOIN이다.
FULL JOIN 은 사용할 일이 거의 없으며, ODBC에 따라 지원하지 않는 경우도 있다.</p>
<hr />
<h2 id="left-join">LEFT JOIN</h2>
<p><strong>왼쪽 테이블을 중심으로 오른쪽의 테이블을 매치</strong>시킨다.
왼쪽 테이블을 무조건 표시하고, 매치되는 레코드가 오른쪽에 없으면 NULL을 표시한다.</p>
<pre><code class="language-sql">SELECT
         a.category_name
      , b.menu_name
  FROM tbl_category a
  LEFT JOIN tbl_menu b ON (a.category_code = b.category_code);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a7370d20-665d-4a70-b518-1e6b80425b2b/image.png" /></p>
<hr />
<h2 id="right-join">RIGHT JOIN</h2>
<p><strong>LEFT JOIN과의 반대로 오른쪽 중심</strong>이다.
코드도 순서를 바꿔서 실행해보았는데 컬럼의 순서만 바뀌고 똑같이 나온다.</p>
<pre><code class="language-sql">SELECT
         a.menu_name
      , b.category_name
  FROM tbl_menu a
  RIGHT JOIN tbl_category b ON (a.category_code = b.category_code);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/41da3076-b05a-442d-9c3f-02e90a5020f1/image.png" /></p>
<hr />
<p>다음은 모든 경우를 나타내주는 CROSS JOIN, 내 테이블이 나를 참조해서 결합하는 SELF JOIN에 대해 알아보자.</p>
<h2 id="cross-join">CROSS JOIN</h2>
<p>그냥 유의미한 코드는 아니지만 CROSS JOIN을 나타내기 위한 코드이다.
단순하게 모든 경우를 나타내준다.</p>
<pre><code class="language-sql">SELECT
         a.menu_name
      , b.category_name
  FROM tbl_menu a
 CROSS JOIN tbl_category b;</code></pre>
<p>모든 경우의 수로 곱해져서 총 252개 row가 조회됐다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/20021607-0e3f-4b4e-9018-76306d67678f/image.png" /></p>
<hr />
<h2 id="self-join">SELF JOIN</h2>
<pre><code class="language-sql">SELECT
         a.category_name
      , b.category_name
  FROM tbl_category a
  JOIN tbl_category b ON (a.ref_category_code = b.category_code)</code></pre>
<p>결과는 아래 사진처럼 나온다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7a153a12-d234-4712-b25f-104af5e93ee0/image.png" /></p>
<p>마치 <strong>a가 하위 카테고리, b가 상위 카테고리처럼 나오게 되는데</strong>
그 이유는
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5bc29a33-5e1e-4626-bd41-60fc19b241ad/image.png" />
<code>a</code>의 <code>ref_category_code</code> 가 <code>null</code> 때문에 <code>b</code>의 <code>category_code</code>와 매치되지 않다가, <strong>4번째 row와 1번째 row가 매치되기 시작하기 때문</strong>이다.</p>
<hr />
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
<hr />
<p>다음은 서브쿼리에 대해 다뤄보려고 한다.</p>
<h2 id="서브쿼리">서브쿼리</h2>
<p>간단하게 시작해보자.</p>
<p>선행해서 쿼리에서 동작해야 할 쿼리를 서브쿼리로 작성한다.</p>
<p>내가 '민트미역국' (우웩) 이라고 하는 메뉴가 들어가는 <code>카테고리 번호</code>를 조회하고 싶다면</p>
<pre><code class="language-sql">SELECT
        category_code
  FROM tbl_menu
 WHERE menu_name LIKE '%민트미역국%';</code></pre>
<p>으로 구할 수 있다.</p>
<blockquote>
<p>위의 결과는 category_code = 4 의 데이터를 조회할 수 있다.</p>
</blockquote>
<hr />
<p>그걸 이제 다르게 '민트 미역국'과 같은 카테고리의 <code>메뉴</code>를 조회를 하고 싶다면
-&gt; 서브쿼리가 필요하다.</p>
<pre><code class="language-sql">SELECT
         menu_name
  FROM tbl_menu
 WHERE category_code = (SELECT category_code
                                    FROM tbl_menu
                                   WHERE menu_name LIKE '%민트미역국%'
                                );</code></pre>
<p>메인 쿼리 안에 서브 쿼리를 넣어서 <code>category_code = 4</code>라는 것을 where 절로 보낼 수 있다.</p>
<hr />
<blockquote>
<p><strong>서브쿼리는 어느 절이든 들어갈 수 있다.</strong>
from 절에 들어가는건 '<strong>인라인 뷰</strong>'라는 이름으로 부른다.</p>
</blockquote>
<blockquote>
<p>MariaDB 에서는 서브쿼리를 <strong>from절에 사용 시(인라인 뷰) 반드시 별칭</strong>이 필요하다.
서브쿼리의 그룹함수의 결과를 <strong>메인 쿼리에서 쓰기 위해서는 반드시 별칭</strong>을 달아야 한다.</p>
</blockquote>
<hr />
<pre><code class="language-sql">-- 다중행 다중열 서브쿼리
SELECT
         *
  FROM tbl_menu;

-- 다중행 단일열 서브쿼리
SELECT
         menu_name
  FROM tbl_menu;

-- 단일행 다중열 서브쿼리
SELECT
         *
  FROM tbl_menu
 WHERE menu_name = '우럭스무디';

-- 단일행 단일열 서브쿼리
SELECT
         category_code
  FROM tbl_menu
 WHERE menu_name = '우럭스무디';</code></pre>
<hr />
<h3 id="상관-서브쿼리">상관 서브쿼리</h3>
<p>메인 쿼리를 활용한 서브쿼리라면 <strong>상관(메인쿼리와 서브쿼리의 상관관계 활용) 서브쿼리</strong> 라고 한다.</p>
<p>메뉴가 존재하는 카테고리 별로 평균을 구하기 위해서는</p>
<pre><code class="language-sql">SELECT
         AVG(menu_price)
  FROM tbl_menu
 WHERE category_code = 4;</code></pre>
<p>위와 같은 코드로 category_code 에 따라 4, 5, 6 ... 계속 바꿔줘야 하는 번거로움이 있다.</p>
<p>상관 서브쿼리를 활용해서 &quot;메뉴별 각 메뉴가 속한 카테고리의 평균보다 높은 가격의 메뉴들만 조회&quot;를 해보자.</p>
<pre><code class="language-sql">SELECT
        a.menu_code
      , a.menu_name
      , a.menu_price
      , a.category_code
      , a.orderable_status
  FROM tbl_menu a
 WHERE a.menu_price &gt; (SELECT AVG(b.menu_price)
                          FROM tbl_menu b
                        WHERE b.category_code = a.category_code    
                      );</code></pre>
<p>Where 절에서 하나의 메뉴에는 상위 개념으로 category_code가 있기 때문에 category_code 별로 가격 평균을 구하고 그것보다 가격이 높은 메뉴들을 조회할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/de534745-b026-4b2f-946c-7ccfb753c08c/image.png" /></p>
<hr />
<h3 id="exists">EXISTS</h3>
<p>서브쿼리의 결과를 체크해주는 키워드이다. (절 아님)</p>
<blockquote>
<p>서브쿼리의 결과가 한 행이라도 조회된다면 -&gt; true
되지 않는다면 -&gt; false</p>
</blockquote>
<p>&quot;카테고리 중에 메뉴에 부여된 카테고리들의 카테고리 이름 조회 후 오름차순 정렬&quot;을 해보자</p>
<pre><code class="language-sql">SELECT
       category_name
  FROM tbl_category a
 WHERE EXISTS (SELECT menu_code
                  FROM tbl_menu b
                 WHERE b.category_code = a.category_code)
 ORDER BY 1;</code></pre>
<p>파생 테이블과 비슷한 개념이며 코드의 가독성과 재사용성을 위해 파생 테이블 대신 사용하게 된다.)</p>