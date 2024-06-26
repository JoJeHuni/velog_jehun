<p>데이터베이스를 정의하는 언어로, <strong>데이터 생성 / 수정 / 삭제 등 데이터의 전체 골격을 결정하는 역할의 언어</strong>이다.</p>
<ul>
<li>CREATE : 데이터베이스, 테이블 생성 역할</li>
<li>ALTER : 테이블 수정 역할</li>
<li>DROP : 데이터베이스, 테이블 삭제 역할</li>
<li>TRUNCATE : 테이블 초기화 역할</li>
</ul>
<h2 id="create">CREATE</h2>
<h3 id="테이블-생성하는-방법">테이블 생성하는 방법</h3>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS tb1 (
  pk INT PRIMARY KEY,
  fk INT,
  col1 VARCHAR(255),
  CHECK(col1 IN ('Y', 'N'))
) ENGINE = INNODB;</code></pre>
<h3 id="테이블의-구조-확인하기">테이블의 구조 확인하기</h3>
<pre><code class="language-sql">DESC tb1;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ff06541a-bd4d-4740-b744-221cf68aec68/image.png" /></p>
<pre><code class="language-sql">SELECT * FROM tb1;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/edc92ef7-6c04-4b12-9180-b593c997fa68/image.png" /></p>
<hr />
<h2 id="auto_increment">AUTO_INCREMENT</h2>
<p>자동으로 데이터에 숫자가 증가되도록 메겨주는 기능이다.
Primary Key는 삭제해도 그 숫자를 메꿔서 사용하는 방식이면 안 된다.
그래서 AUTO_INCREMENT 를 사용해서 pk에 넣어주면 편리하다.</p>
<pre><code class="language-sql">CREATE TABLE tb2 (
  pk INT PRIMARY KEY AUTO_INCREMENT,
  fk INT,
  col1 VARCHAR(255),
  CHECK(col1 IN ('Y', 'N'))
) ENGINE = INNODB;</code></pre>
<h3 id="구조-확인">구조 확인</h3>
<pre><code class="language-sql">DESC tb2;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0242ef21-7ad4-466f-8de6-113686a0446a/image.png" /></p>
<h3 id="활용">활용</h3>
<pre><code class="language-sql">INSERT
  INTO tb2
VALUES
(
  NULL
, 2
, 'N'
);</code></pre>
<p>위의 쿼리를 4번 실행하면 auto_increment 덕분에 자동으로 pk가 1씩 늘어나는 것을 확인할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ff1b2cf4-72a5-4995-9734-7a468c7763ba/image.png" /></p>
<hr />
<h2 id="alter">ALTER</h2>
<p>테이블을 수정하는 역할로, 컬럼들을 추가 / 수정 / 삭제 또한 수정에 포함되기 때문에 범용성이 넓다.</p>
<h3 id="컬럼-추가">컬럼 추가</h3>
<p>add를 사용해서 컬럼을 추가해보자.</p>
<pre><code>ALTER TABLE tb2 ADD col2 INT NOT NULL;</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a19bdd7a-43bf-4906-ad41-ea5acd50c054/image.png" /></p>
<hr />
<h3 id="컬럼-삭제">컬럼 삭제</h3>
<p>다시 컬럼을 지워보자.</p>
<pre><code class="language-sql">ALTER TABLE tb2 DROP COLUMN col2;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/15c90362-7429-4912-af4d-1533e69abdb3/image.png" /></p>
<blockquote>
<p>처음 만들 때는 많은 것을 고려하고 만들어서 가능하면 ALTER를 사용하지 않는 것이 좋다.
ALTER는 고도화된 프로젝트에서나 사용하는 것이다.</p>
</blockquote>
<hr />
<h3 id="컬럼-이름-및-컬럼-정의-변경">컬럼 이름 및 컬럼 정의 변경</h3>
<pre><code class="language-sql">ALTER TABLE tb2 CHANGE COLUMN fk change_fk INT NOT NULL;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f42c3563-091e-45cc-a1e9-8913c38a30c7/image.png" /></p>
<hr />
<h3 id="제약조건-제거하기-primary-key-제약조건-제거">제약조건 제거하기 (primary key 제약조건 제거)</h3>
<p>Primary Key 를 제거하려면 
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5696a82b-c6b6-4f06-81c8-ce65533767de/image.png" /></p>
<p>이러한 에러 메세지가 뜬다.</p>
<pre><code class="language-sql">ALTER TABLE tb2 DROP PRIMARY KEY;</code></pre>
<blockquote>
<p>어떻게 하면 될까?</p>
</blockquote>
<p>우선 auto_increment부터 modify 로 변경해줘야 한다.</p>
<pre><code class="language-sql">ALTER TABLE tb2 MODIFY pk INT;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2173a372-6f4a-420c-bab4-f74b0735bf9f/image.png" /></p>
<p>Extra에 없어진 것이 보인다. 이제 primary key도 지워보자.</p>
<pre><code class="language-sql">ALTER TABLE tb2 DROP PRIMARY KEY;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/dd77221e-033a-4f14-b3e8-f039a1cb4b9d/image.png" /></p>
<hr />
<h2 id="truncate">TRUNCATE</h2>
<h3 id="truncate--delete--drop">TRUNCATE / DELETE / DROP</h3>
<blockquote>
<ol>
<li><strong>TRUNCATE</strong>는 테이블의 데이터(데이터 및 관련 제약조건 관련 등 깔끔하게 제거) 초기화</li>
</ol>
<p>-&gt; <strong>create 명령어 때처럼 행 전체가 비워진다.</strong>
-&gt; 자동 commit이 돼서 rollback이 안 된다.</p>
</blockquote>
<blockquote>
<ol start="2">
<li><strong>DELETE</strong> 는 TRUNCATE 처럼 <strong>데이터를 전체 삭제도 가능</strong>하지만, <strong>조건을 걸어 일부 삭제도 가능</strong>하다.</li>
</ol>
<p>-&gt; <code>autocommit = on</code>이면 자동 커밋돼서 TRUNCATE 와 (전체 - 일부) 차이뿐이다.
-&gt; 자동 commit은 안 되기 때문에 <code>autocommit = off</code> 면 rollback도 가능하다.</p>
</blockquote>
<blockquote>
<ol start="3">
<li><strong>DROP</strong> 은 <strong>테이블 자체를 삭제</strong></li>
</ol>
</blockquote>
<p>일단 임의의 테이블을 만들어봤다.</p>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS tb3 (
  pk INT AUTO_INCREMENT,
  fk INT,
  col1 VARCHAR(255), CHECK(col1 IN ('Y', 'N')),
  PRIMARY KEY(pk)
) ENGINE = INNODB;

INSERT
  INTO tb3
VALUES
(
  NULL
, 2
, 'N'
);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6383a837-bd1f-4d3d-9ebc-a31ebb4585e8/image.png" /></p>
<p>이 상태에서 TRUNCATE 를 사용해보자.</p>
<pre><code class="language-sql">TRUNCATE table tb3;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e453aca-c5c1-47ca-a792-284d20e1fc76/image.png" /></p>
<h3 id="truncate-중요한-점">TRUNCATE 중요한 점</h3>
<blockquote>
<p><strong>계정에게 DROP 권한이 있어야지 사용 가능하다.</strong></p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7ad2c6c2-3a50-4afb-a000-ff5d0f74c9e3/image.png" /></p>
<p>참고 링크 : <a href="https://dev.mysql.com/doc/refman/8.4/en/truncate-table.html">https://dev.mysql.com/doc/refman/8.4/en/truncate-table.html</a></p>
<p>DROP 후 CREATE 테이블하는 것처럼 진행된다고 생각을 하면 될 것 같다.</p>