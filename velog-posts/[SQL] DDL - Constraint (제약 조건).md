<p>SQL 에는 제약조건들이 몇 가지 있다.</p>
<ul>
<li>NOT NULL 제약 조건</li>
<li>UNIQUE 제약 조건</li>
<li>PRIMARY KEY 제약 조건 (NOT NULL + UNIQUE)</li>
<li>FOREIGN KEY 제약 조건</li>
<li>CHECK 제약 조건</li>
</ul>
<hr />
<h2 id="not-null-제약-조건">NOT NULL 제약 조건</h2>
<p>지정한 column에 NULL을 허용하지 않는 제약 조건 (NULL을 제외한 데이터의 중복은 허용됨)
column level 에서만 가능하다.</p>
<blockquote>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_notnull(
  user_no INT NOT NULL
);</code></pre>
</blockquote>
<pre><code>이런 식은 가능하지만 아래는 **불가능하다.**
```sql
CREATE TABLE if NOT EXISTS user_notnull(
  user_no INT,
  NOT NULL(user_no)
)</code></pre><p><code>user_notnull</code> 이라는 테이블을 만들어본다.</p>
<pre><code class="language-sql">DROP TABLE if EXISTS user_notnull;

CREATE TABLE if NOT EXISTS user_notnull(
  user_no INT NOT NULL,
  user_id VARCHAR(255) NOT NULL,
  user_pwd VARCHAR(255) NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3),
  phone VARCHAR(255) NOT NULL,
  email VARCHAR(255)
) ENGINE=INNODB;</code></pre>
<p><code>gender</code>, <code>email</code>을 제외한 다른 column들은 전부 not null 조건이 들어간다.</p>
<hr />
<pre><code class="language-sql">INSERT
  INTO user_notnull
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@gmail.com'),
(2, 'user02', 'pass02', '유관순', '여', '010-777-7777', 'yo77@gmail.com');</code></pre>
<p>이런 식으로 넣는건 가능하다. -&gt; <code>gender</code>, <code>email</code>을 제외한 다른 column에는 NULL인 값이 없으니까</p>
<pre><code class="language-sql">INSERT
  INTO user_notnull
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(3, 'user03', 'pass03', null, '남', '010-1234-5677', 'hong1234@gmail.com');</code></pre>
<p>이렇게 <code>user_name</code> column에 null을 넣으니까 제약조건에 의해 에러가 생긴다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/458ef13d-57f5-4d4f-a127-add17d8cbbc4/image.png" /></p>
<hr />
<h2 id="unique-제약-조건">UNIQUE 제약 조건</h2>
<p>중복값이 들어가지 않도록 하는 제약 조건
<code>UNIQUE 제약 조건</code>은 <strong>중복되는 값은 허용하지 않지만</strong> <code>NULL</code> <strong>저장은 가능</strong></p>
<ul>
<li>NOT NULL 제약조건과 다르게 column level 및 table level 모두 가능하다.</li>
</ul>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_unique(
  user_no INT NOT NULL UNIQUE,
  user_id VARCHAR(255) NOT NULL,
  user_pwd VARCHAR(255) NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3),
  phone VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  UNIQUE(phone) -- phone에다가 컬럼댈 필요 없음
) ENGINE=INNODB;

INSERT
  INTO user_unique
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@gmail.com'),
(2, 'user02', 'pass02', '유관순', '여', '010-777-7777', 'yo77@gmail.com');

INSERT
  INTO user_unique
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(3, 'user03', 'pass03', '홍길동3', '남', '010-1234-5678', 'hong1234@gmail.com');</code></pre>
<p><code>create</code> -&gt; <code>insert</code> -&gt; <code>insert</code> 쿼리를 차례대로 실행하면 <code>phone</code> column이 중복해서 에러가 생긴다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/26e14270-b021-4074-aa8c-9be803a918cd/image.png" /></p>
<hr />
<h2 id="primary-key-제약조건">PRIMARY KEY 제약조건</h2>
<p><code>not null + unique</code> 제약 조건이라고 볼 수 있다.</p>
<blockquote>
<p>모든 테이블은 반드시 <strong>primary key를 가져야 한다.</strong> (= 식별자를 가져야 한다.)
<strong>2개 이상의 제약 조건을 할 수는 없다.</strong></p>
</blockquote>
<hr />
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_primarykey(
  user_no INT PRIMARY KEY,
  user_id VARCHAR(255) NOT NULL,
  user_pwd VARCHAR(255) NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3),
  phone VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  UNIQUE(phone)
) ENGINE=INNODB;</code></pre>
<p>이렇게 만든 뒤 확인해보면</p>
<pre><code class="language-sql">DESC user_primarykey;</code></pre>
<p><code>user_no : primary key</code>, <code>phone : unique</code> 로 잘 보인다.</p>
<hr />
<pre><code class="language-sql">INSERT
  INTO user_primarykey
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(1, 'user01', 'pass01', '홍길동', '남', '010-1234-5678', 'hong123@gmail.com'),
(2, 'user02', 'pass02', '유관순', '여', '010-777-7777', 'yo77@gmail.com');

INSERT
  INTO user_primarykey
  (user_no, user_id, user_pwd, user_name, gender, phone, email)
VALUES
(1, 'user03', 'pass03', '홍길3동', '남', '010-1234-5673', 'hong1234@gmail.com');</code></pre>
<p>잘 보면 <code>user_no</code> : 1, 2, 1 로 1이 겹치게 되면서 <code>primary key (NOT NULL + UNIQUE)</code> 에서 중복때문에 에러가 생긴다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/04109bba-bb61-4e32-8c3e-74733dcb6049/image.png" /></p>
<hr />
<h2 id="foreign-key-제약조건">FOREIGN KEY 제약조건</h2>
<p>외래 키(이하 FK)는 다른 테이블의 열을 참조하여 존재하는 값만 입력할 수 있는 제약 조건</p>
<p>멤버쉽을 생각해보자.
멤버십 등급 - 사용자 테이블이 하나씩 있다고 했을 때</p>
<p>'하나의 멤버쉽'은 '여러 사용자'에게 '하나'씩 배정이 된다. 즉, 멤버쉽 : 사용자 = 1 : N 관계인 것이다.</p>
<p>10점이 일반 고객, 20점이 우수 고객, 30점이 특별 고객 이라는 등급이 있을 때의 테이블은 이런 구조일 것이다.</p>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_grade(
  grade_code INT PRIMARY KEY,
  grade_name VARCHAR(255) NOT NULL
);

INSERT
  INTO user_grade
VALUES
(10, '일반회원'),
(20, '우수회원'),
(30, '특별회원');</code></pre>
<hr />
<p>그럼 사용자 테이블은 FK 로 <code>USER_GRADE</code>의 PRIMARY KEY (이하 PK) 를 가지고 있어야 한다.</p>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_foreignkey1 (
  user_no INT PRIMARY KEY,
  user_id VARCHAR(255) NOT NULL,
  user_pwd VARCHAR(255) NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3),
  phone VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  grade_code INT,
  FOREIGN KEY(grade_code) REFERENCES user_grade(grade_code)
) ENGINE=INNODB;</code></pre>
<p>이렇게 <code>grade_code</code>를 외래키로 가지면서 1:N 관계를 갖게 된다. 사용자 등록을 하는데 <code>grade_code</code>를 자세하게 보자.</p>
<ul>
<li><p><strong>잘 되는 경우</strong></p>
<pre><code class="language-sql">INSERT
INTO user_foreignkey1
VALUES
(1, 'user01', 'pass01', '홍길동', '남', '010-111-1111', 'hong@gmail.com', 10),
(2, 'user02', 'pass02', '홍길동전', '남', '010-222-2222', 'hong1@gmail.com', 20);</code></pre>
<p>등급이 안에 속하는 10, 20 이므로 잘 된다.</p>
</li>
<li><p><strong>잘 되는 경우 2 (<code>NULL</code>을 넣을 때)</strong></p>
<pre><code class="language-sql">INSERT
INTO user_foreignkey1
VALUES
(3, 'user01', 'pass01', '홍길동', '남', '010-111-1111', 'hong@gmail.com', NULL);</code></pre>
<p>이것 또한 된다. 왜??</p>
</li>
</ul>
<h3 id="외래키-중요한-부분">외래키 중요한 부분</h3>
<blockquote>
<p><strong>1. FK 값에 NULL을 넣으면 부모의 PK와는 관련이 없다는 의미로 사용할 수 있다.</strong>
<strong>즉, 연관관계의 주도권은 자식 테이블에게 있다.</strong></p>
</blockquote>
<blockquote>
<p><strong>2. FK 는 PK 와 UNIQUE KEY 2가지를 참조할 수 있다.</strong></p>
</blockquote>
<ul>
<li>에러가 생길 때<pre><code class="language-sql">INSERT
INTO user_foreignkey1
VALUES
(4, 'user02', 'pass02', '홍길동전', '남', '010-222-2222', 'hong1@gmail.com', 40);</code></pre>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/37c3613d-38a6-4a3b-abf7-7a0e73c6d36f/image.png" /></p>
<p>부모 테이블 안에 없는 점수를 넣으니까 에러가 발생한다.</p>
<hr />
<h3 id="삭제룰-적용">삭제룰 적용</h3>
<p>위 상황까지는 부모 - 자식 테이블이 연관된 상태이다.</p>
<p>근데 <strong>부모 테이블의 행을 삭제해야하는 상황이 오게 된다면</strong></p>
<ol>
<li><strong>행에 속하는 fk 를 가진 자식테이블을 삭제하고 지워야 하는 상황</strong></li>
<li>부모테이블의 행을 지우면 자식 테이블도 같이 지워지는 상황</li>
</ol>
<p>이런 상황들이 있을 것이다.</p>
<blockquote>
<p>해결방법으로는 1번에서 FK를 가진 자식테이블을 처음에 CREATE 할 때 삭제룰을 사용해 생성하는 방법이 있다.</p>
</blockquote>
<p>위 부모 테이블, 자식 테이블들은 그대로 냅두고
삭제룰을 가진 제 2의 자식 테이블을 만들자.</p>
<pre><code class="language-sql">CREATE TABLE if NOT EXISTS user_foreignkey2 (
  user_no INT PRIMARY KEY,
  user_id VARCHAR(255) NOT NULL,
  user_pwd VARCHAR(255) NOT NULL,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3),
  phone VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  grade_code INT,
  FOREIGN KEY(grade_code) REFERENCES user_grade(grade_code)
  ON DELETE SET NULL
) ENGINE=INNODB;</code></pre>
<blockquote>
<p>foreign key에 붙여서 쓰는 <code>ON DELETE SET NULL</code> 같은 것을 삭제룰 이라고 한다.
set 뒤에 어떤 것이 오느냐에 따라 삭제룰은 바뀐다.</p>
</blockquote>
<hr />
<p><code>user_foreignkey2</code> 에 새로운 값을 삽입해주고</p>
<pre><code class="language-sql">INSERT
  INTO user_foreignkey2
VALUES
(1, 'user01', 'pass01', '홍길동', '남', '010-111-1111', 'hong@gmail.com', 10);</code></pre>
<p>삭제를 하려고 하니 문제가 생겼다.</p>
<blockquote>
<p><strong><code>user_foreighkey1</code> 이 부모 테이블과 연결이 돼 있어 삭제를 할 수 없다.</strong></p>
</blockquote>
<hr />
<p>그래서 <code>truncate</code>로 <code>user_foreignkey1</code> 을 초기화시켜주고 <code>user_foreignkey2</code>를 조회하면?</p>
<pre><code class="language-sql">TRUNCATE user_foreignkey1;

SELECT * FROM user_foreignkey2;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d8816a80-7906-4e2e-bc7b-9602ecea5128/image.png" /></p>
<hr />
<p>삭제룰이 적용된 상태이므로 <code>grade_code = 10</code>인 부모테이블 <code>user_grade</code>를 삭제하면?</p>
<pre><code class="language-sql">DELETE FROM user_grade WHERE grade_code = 10;

SELECT * FROM user_foreignkey2;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e6437a3c-e0d2-476e-8b0b-c6f6b5ec31b5/image.png" /></p>
<p>이렇게 삭제룰을 적용한 FOREIGN KEY 제약 조건을 마치겠다.</p>
<hr />
<h2 id="check-제약-조건">CHECK 제약 조건</h2>
<p><code>CHECK</code> 제약 조건은 &quot;조건식을 활용한&quot; 제약 조건이다.</p>
<p><code>gender</code>, <code>age</code>에 <code>CHECK</code> 제약조건을 넣고 테이블을 만들어보자.</p>
<pre><code class="language-sql">DROP TABLE if EXISTS user_check;

CREATE TABLE if NOT EXISTS user_check (
  user_no INT AUTO_INCREMENT PRIMARY KEY,
  user_name VARCHAR(255) NOT NULL,
  gender VARCHAR(3) CHECK(gender IN ('남', '여')), -- 제약조건을 활용한다
  age INT CHECK(age &gt;= 19) -- 제약조건을 활용. 조건식에 따라 true, false 값
) ENGINE=INNODB;

INSERT
  INTO user_check
VALUES
(NULL, '홍길동', '남', 25),
(NULL, '이순신', '남', 33);</code></pre>
<blockquote>
<p><code>ENGINE=INNODB</code> 를 써야하는 경우에 대해서는 나중에 알아보자.</p>
</blockquote>
<p>위의 쿼리를 실행하고 조회해보면</p>
<pre><code class="language-sql">SELECT * FROM user_check;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3933b63d-a341-4576-bd30-f8db1552659c/image.png" /></p>
<hr />
<ul>
<li>성별에 잘못된 값 넣어보기<pre><code class="language-sql">INSERT
INTO user_check
VALUES
(NULL, '아메바', '중', 19);</code></pre>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5f336b45-3599-4105-b3cc-f8934653f374/image.png" /></p>
<p>남, 여 가 아닌 다른 데이터를 넣으니 에러가 생겼다.</p>
<hr />
<ul>
<li>나이에 잘못된 값 넣어보기<pre><code class="language-sql">INSERT
INTO user_check
VALUES
(NULL, '유관순', '여', 16);</code></pre>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/eed9d929-4552-455f-aa27-cdc064ecb08c/image.png" /></p>
<hr />
<p>다음은 제약 조건이라기엔 애매할 수도 있는 사용자의 편리를 위한 <code>DEFAULT</code> 제약 조건에 대해 알아보자.</p>
<p>이번엔 <code>tbl_country</code> 라는 테이블을 만들어보았다.</p>
<pre><code class="language-sql">DROP TABLE if EXISTS tbl_country;

CREATE TABLE if NOT EXISTS tbl_country (
  country_code INT AUTO_INCREMENT PRIMARY KEY,
  country_name VARCHAR(255) DEFAULT '한국',
  population VARCHAR(255) DEFAULT '0명',
  add_day DATE DEFAULT (CURRENT_DATE),
  add_time DATETIME DEFAULT (CURRENT_TIME)
) ENGINE=INNODB;</code></pre>
<p><code>DEFAULT</code> 는 알다시피 기본값과 같은 느낌이다.
<code>DEFAULT</code> 제약 조건을 입력해뒀다면 <code>INSERT</code> 문으로 데이터를 삽입할 때 <code>DEFAULT</code>를 사용하면 된다.</p>
<pre><code class="language-sql">INSERT
  INTO tbl_country
VALUES
(NULL, DEFAULT, DEFAULT, DEFAULT, DEFAULT);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bdd55015-847b-48b0-8dfb-24e9f79c0458/image.png" /></p>
<p>여기까지 DDL에 대한 내용을 마치겠다.</p>