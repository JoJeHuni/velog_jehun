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
<h2 id="outer-join">OUTER JOIN</h2>
<blockquote>
<p>조건에 부합하지 않는 행도 포함시켜 결합하는 것</p>
</blockquote>
<p>기본적으로 LEFT, RIGHT or FULL이 OUTER JOIN이다.
FULL JOIN 은 사용할 일이 거의 없으며, ODBC에 따라 지원하지 않는 경우도 있다.</p>