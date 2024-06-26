<blockquote>
<p>DML = Data Manipulation Language</p>
</blockquote>
<p>간단하게 CRUD라고 생각하면 된다.</p>
<p>insert, update, delete, select(DQL) 4가지에 대해 알아보자.</p>
<h2 id="insert">INSERT</h2>
<p>새로운 행을 추가하는 구문
-&gt; 테이블의 행의 수가 증가한다.</p>
<pre><code class="language-sql">SELECT *
  FROM tbl_menu;</code></pre>
<p>컬럼과 데이터들을 확인해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fc78a8e4-c726-441e-b3e3-d7e4f84eea47/image.png" /></p>
<hr />
<p>이런 식으로 21개 row개의 데이터가 존재하고 이제 추가해보자.</p>
<pre><code class="language-sql">INSERT
  INTO tbl_menu
(
  menu_name
, menu_price
, category_code
, orderable_status
)
VALUES
(
  '초콜릿죽'
, 6500
, 7
, 'Y'
);</code></pre>
<p>위의 쿼리를 실행하고 다시 select 하면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3094576e-141e-429d-a7ab-7203eaa2becb/image.png" /></p>
<p>22번째 row에 데이터가 추가된 것을 알 수 있다.</p>
<blockquote>
<pre><code class="language-sql">INSERT
  INTO tbl_menu
(
  menu_name
, menu_price
, category_code
, orderable_status
)</code></pre>
</blockquote>
<pre><code>&gt; 이 부분에서 컬럼을 SKIP 해도 되지만 value에서 null 값인지 아닌지 다 적어줘야 하고, 실무에서도 유지보수 측면에서 적어주는 것이 좋다.

---
어느 정도 컬럼에 대해 알고 있기 때문에 SKIP 하고 3개 데이터를 더 추가해줬다.
```sql
INSERT
  INTO tbl_menu
VALUES
(NULL, '참치맛아이스크림', 1700, 12, 'Y'),
(NULL, '멸치맛아이스크림', 1500, 11, 'Y'),
(NULL, '소시지맛커피', 2500, 8, 'Y');</code></pre><hr />
<h2 id="update">UPDATE</h2>
<blockquote>
<p>테이블에 기록된 컬럼값을 수정하는 구문
전체 행 개수에는 변화 X</p>
</blockquote>
<p>소시지맛커피에 대한 정보를 수정해주자.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2b56a00b-8782-41e7-a4a8-96b041cd4bd8/image.png" /></p>
<p>category_code를 7로 바꿔줘보았다.</p>
<pre><code class="language-sql">UPDATE tbl_menu
   SET category_code = 7
 WHERE menu_code = 25;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/65793d6f-0df2-4acf-9738-d7e565d1a129/image.png" /></p>
<hr />
<h3 id="서브쿼리를-활용한-update">서브쿼리를 활용한 UPDATE</h3>
<p>서브쿼리를 쓸만큼 의미 있는 코드는 아니지만 간단한 예제를 만들어봤다.</p>
<pre><code class="language-sql">UPDATE tbl_menu
   SET category_code = 6
 WHERE menu_code = (SELECT menu_code
                              FROM tbl_menu
                             WHERE menu_name = '소시지맛커피');</code></pre>
<p>(where 절을 썼기 때문에 단일 행만 가능하지만 그래도 해봤다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/444e83df-ab70-4afe-8f58-f060955c4fbd/image.png" /></p>
<hr />
<h2 id="delete">DELETE</h2>
<p>테이블의 행 삭제를 할 때 사용한다.</p>
<blockquote>
<p>예제로라도 해볼 때 set auto commit 설정이 on이 되어 있다면 슬픈 상황이 생기게 된다.</p>
</blockquote>
<pre><code class="language-sql">SET autocommit = OFF;</code></pre>
<p>MySQL 또는 MariaDB는 <code>autocommit</code>의 기본값 on으로 되어 있으니 확인해볼것..</p>
<hr />
<blockquote>
<p>DELETE 사용 시 주의할 점 : 나중에 조인을 통해 외래키로 해뒀을 때 DELETE를 하면 조인된 테이블에서의 행도 삭제가 된다.</p>
</blockquote>
<p>소시지맛커피를 삭제해보았다.</p>
<pre><code class="language-sql">DELETE
  FROM tbl_menu
 WHERE menu_name = '소시지맛커피';</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/851532d8-ef52-4811-a1fa-9d08f50083e2/image.png" /></p>
<p>where 절이 있어야지 조건에 맞춰서 행 삭제를 해준다.
where 절이 없으면 테이블 자체를 지워달라는 의미가 된다.</p>
<p>사진에 25개 행이 있던 테이블 중 소시지맛커피를 지워서 24개로 줄어들었다.</p>
<blockquote>
<p>웬만하면 쓰지 않는 것이 좋다. 다른 키워드가 있음</p>
</blockquote>
<hr />
<h2 id="replace">REPLACE</h2>
<p>insert 시 primary key 또는 unique key가 충돌이 발생하지 않도록 replace를 통해 중복된 데이터는 덮어씌우는게 가능하다.</p>
<blockquote>
<p>primary key(null X, 중복 X, 이후 수정 X)
unique key(중복 X)</p>
</blockquote>
<pre><code class="language-sql">REPLACE
   INTO tbl_menu
 VALUES
(
  17
, '참기름소주'
, 5000
, 10
, 'Y'
);</code></pre>
<p>위의 쿼리로 만든 것을 확인하면</p>
<pre><code>SELECT * FROM tbl_menu WHERE menu_code = 17;</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/37180d55-3e1e-4319-b905-972a96d9b95b/image.png" /></p>