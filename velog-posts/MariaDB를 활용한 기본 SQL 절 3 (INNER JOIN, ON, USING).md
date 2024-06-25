<p>set operator, join의 차이 같은 개념은 꼭 알아야 한다.
그러기 위해 join에 대해서 알아보자.</p>
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