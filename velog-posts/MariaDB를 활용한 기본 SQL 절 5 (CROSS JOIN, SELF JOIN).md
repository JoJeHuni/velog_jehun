<p>모든 경우를 나타내주는 CROSS JOIN, 내 테이블이 나를 참조해서 결합하는 SELF JOIN에 대해 알아보자.</p>
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