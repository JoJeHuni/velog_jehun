<h1 id="view">VIEW</h1>
<p>개발자에게 필요한 만큼만 기존 테이블에서 추출해서 가상의 테이블을 만드는데 그것을 VIEW 라고 한다.</p>
<blockquote>
<p>즉, 실제 데이터를 가지고 있지 않고, 원본 테이블을 참조해서 보여주는 용도로 보여지는건 실제 테이블(베이스 테이블)의 값이다.</p>
</blockquote>
<hr />
<p>이러한 조회 쿼리가 있다고 하자.</p>
<pre><code class="language-sql">SELECT
         menu_name
      , menu_price
  FROM tbl_menu;</code></pre>
<p>이것을 뷰로 가상 테이블을 만들 수 있다.</p>
<pre><code class="language-sql">CREATE VIEW v_menu
AS
SELECT
         menu_name
      , menu_price
  FROM tbl_menu;</code></pre>
<p>이렇게 만들면 VIEW는 위의 SELECT 조회 쿼리만 저장하고 있는 것이다.</p>
<blockquote>
<p>즉, 데이터를 가지고 있지 않고, SELECT 조회 쿼리만 저장하고 잇는 것이다.
ex) 매크로</p>
</blockquote>
<hr />
<p>매크로라고 해서 마구마구 쓰는게 아니다.</p>
<ol>
<li>DBA가 개발자가 보여주고 싶은 것만 추출해서 보여줄 때 사용</li>
</ol>
<ul>
<li>&quot;view를 만들어뒀으니 이거를 가지고 사용해라.&quot;</li>
</ul>
<ol start="2">
<li>column에 대해 DBA는 그걸 알지만, 중간 참여한 사람이 이해하기엔 별칭이 없으면 시간이 소모되기 때문에 별칭을 달면서 추출해주는 것이다.</li>
</ol>
<p>아까 만든 view를 지우고 별칭을 달아서 다시 만들어서 조회한다.</p>
<pre><code>DROP VIEW v_menu;

CREATE VIEW v_menu
AS
SELECT
         menu_name AS '메뉴명'
      , menu_price AS '메뉴단가'
  FROM tbl_menu;

SELECT * FROM v_menu;</code></pre><blockquote>
<p>그럼 view를 계속 지웠다가 할 수는 없기 때문에 <code>replace</code> 를 달아주면 편하다.</p>
</blockquote>
<pre><code class="language-sql">CREATE or REPLACE VIEW v_menu
AS
SELECT
         menu_name AS '메뉴명'
      , menu_price AS '메뉴단가'
  FROM tbl_menu;</code></pre>
<hr />
<p>DML 작업을 VIEW를 이용해서 해서는 안 된다.
그럼에도 view를 통한 DML을 보기는 하자. (실제 사용하는건 좋지 않다.)</p>
<p>tbl_menu 에서 한식에 대한 view를 만든다.</p>
<pre><code class="language-sql">CREATE OR REPLACE VIEW hansik
AS
SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
  FROM tbl_menu
 WHERE category_code = 4;

SELECT * FROM hansik;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1df035d7-4f5a-42a5-a9cd-75e6da3b2296/image.png" /></p>
<p>여기에 hansik view에 데이터를 삽입해보자.</p>
<pre><code class="language-sql">INSERT
  INTO hansik
VALUES
(NULL, '식혜맛국밥', 5500, 4, 'Y');

SELECT * FROM hansik;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ac1aad95-f87c-4e3f-8bba-04c037a96977/image.png" /></p>
<blockquote>
<p>hansik 이라는 view를 통해 tbl_menu라는 베이스 테이블에 영향을 준다.</p>
</blockquote>
<p>진짜인지 확인해보자.</p>
<pre><code class="language-sql">SELECT * FROM tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b63f1dc2-3dcc-47a2-9b2e-670a8783e45d/image.png" /></p>
<p>이번엔 view를 통해 update도 해보자.</p>
<pre><code class="language-sql">UPDATE hansik
   SET menu_name = '버터맛국밥'
     , menu_price = 6000
 WHERE menu_name = '식혜맛국밥'</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0f453684-9efe-489d-9125-fb529eaca219/image.png" /></p>
<hr />
<h2 id="view를-통해-베이스-테이블에-영향을-주지-않는-경우">view를 통해 베이스 테이블에 영향을 주지 않는 경우</h2>
<ul>
<li>view 정의에 포함되지 않은 column을 조작할 때</li>
<li>view에 포함되지 않은 column 중에 베이스가 되는 테이블 column이 NOT NULL 제약조건이 지정된 경우</li>
<li>산술 표현식이 정의된 경우</li>
<li>JOIN을 이용해 여러 테이블을 연결했을 때</li>
<li>DISTINCT를 포함한 경우</li>
<li>그룹함수나 GROUP BY 절을 포함한 경우</li>
</ul>
<p>ex) 그룹함수를 사용한 경우</p>
<pre><code class="language-sql">CREATE OR REPLACE VIEW v_test
AS
SELECT
        AVG(menu_price) + 3
  FROM tbl_menu;

SELECT * FROM v_test;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/92506d5c-9890-4755-afbd-fef51b1f77e6/image.png" /></p>
<hr />
<blockquote>
<p>VIEW는 데이터를 쉽게 읽고 이해할 수 있도록 돕는 동시에, 원본 데이터의 보안을 유지하는데 도움이 된다.</p>
</blockquote>