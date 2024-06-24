<p>이전에도 MySQL을 활용해본 적이 있지만, DB를 MariaDB를 활용해 공부해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/66e62563-c3e8-46a2-9a0b-54d780cea402/image.png" /></p>
<h2 id="mariadb란">MariaDB란?</h2>
<p><strong>MariaDB</strong>는 오픈 소스의 관계형 데이터베이스(RDBMS)로 MySQL의 포크(fork)로 시작되어, MySQL과 API(Application Programming Interface) 및 ABI(Application Binary Interface) 호환성을 유지하면서도 독자적인 기능과 개선사항을 추가해 나가고 있다.</p>
<h3 id="특징">특징</h3>
<table>
<thead>
<tr>
<th>특징</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>MySQL과의 호환성</td>
<td>MariaDB는 MySQL과의 높은 호환성을 유지하며 MySQL에서 MariaDB로의 전환은 매우 간단하다.</td>
</tr>
<tr>
<td>개선된 성능</td>
<td>MariaDB는 쿼리 최적화, 새로운 인덱스 및 스토리지 엔진, 병렬 복제 등 MySQl 대비 성능을 향상시킨 여러 기능을 제공한다.</td>
</tr>
<tr>
<td>오픈 소스</td>
<td>GPL v2 라이센스 하에 배포되어 누구나 자유롭게 사용, 수정, 배포할 수 있다.</td>
</tr>
<tr>
<td>다양한 스토리지 엔진 지원</td>
<td>MariaDB는 Aria, InnoDB, MyRocks, TokuDB등 다양한 스토리지 엔진을 지원하여, 사용자의 요구 사항에 맞게 데이터 저장 방식을 선택할 수 있다.</td>
</tr>
<tr>
<td>활발한 커뮤니티 및 개발</td>
<td>MariaDB는 활발한 커뮤니티와 지속적인 개발로 새로운 기능과 보안 패치를 꾸준히 제공하고 있다.</td>
</tr>
</tbody></table>
<blockquote>
<p>GPL v2 라이센스는 간단하게 개발자가 무료로 공유했으니, 그걸 사용해 기술을 개발해도 무료로 공유해야 한다는 것으로 알면 된다.</p>
</blockquote>
<hr />
<p>MariaDB를 설치한 뒤 (root 계정 비밀번호는 꼭 기억해야 한다. + 포트번호도 안 겹치게.)
CLI 환경에서 켜 보았다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ad9050d7-aff1-4cbd-921f-afac340a0008/image.png" /></p>
<p>하지만, GUI 환경에서 하는 것이 훨씬 편하기 때문에 HeidiSQL을 실행하겠다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3d04ca89-4d3e-4a18-8c1d-0ce16297b636/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6b53675a-7429-4110-8fbe-1a89431e6b16/image.png" /></p>
<h3 id="mariadb-계정-생성-및-권한-부여">MariaDB 계정 생성 및 권한 부여</h3>
<pre><code class="language-sql">-- 현재 존재하는 데이터베이스 확인
SHOW databases;

-- mysql 데이터베이스로 계정 정보 확인하기
USE mysql;    -- 기본 적으로 제공되는 mysql database

-- 1) 새로운 계정 만들기
CREATE USER '{계정이름}'@'%' IDENTIFIED BY  '{계정비밀번호}';  -- 'localhost' 대신 '%'를 쓰면 외부 ip로 접속 가능

-- mysql database에서 user를 확인해 계정이 추가된 것을 확인한다.
SELECT * FROM user;    

-- 2) 데이터베이스 생성 후 계정에 권한 부여
-- 데이터베이스(스키마) 생성
CREATE DATABASE menudb;

--  계정의 권한 확인하기
SHOW GRANTS FOR '{계정이름}'@'%';

GRANT ALL PRIVILEGES ON menudb.* TO '{계정이름}'@'%';    -- menu에 대한 모든 권한 부여</code></pre>
<p>이렇게 만든 계정에 권한을 주고 DB 스크립트를 사용해 확인해본다.</p>
<h3 id="db-스크립트">DB 스크립트</h3>
<h4 id="erb-논리모델">ERB 논리모델</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b10c4607-6deb-4b68-b944-f80398c7bfda/image.png" /></p>
<h4 id="물리모델">물리모델</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9fd05256-4ac0-4620-b01a-c84e95a97034/image.png" /></p>
<h4 id="스크립트">스크립트</h4>
<pre><code class="language-sql">-- 테이블 삭제
DROP TABLE IF EXISTS tbl_payment_order CASCADE;
DROP TABLE IF EXISTS tbl_payment CASCADE;
DROP TABLE IF EXISTS tbl_order_menu CASCADE;
DROP TABLE IF EXISTS tbl_order CASCADE;
DROP TABLE IF EXISTS tbl_menu CASCADE;
DROP TABLE IF EXISTS tbl_category CASCADE;

-- 테이블 생성
-- category 테이블 생성
CREATE TABLE IF NOT EXISTS tbl_category
(
    category_code    INT AUTO_INCREMENT COMMENT '카테고리코드',
    category_name    VARCHAR(30) NOT NULL COMMENT '카테고리명',
    ref_category_code    INT COMMENT '상위카테고리코드',
    CONSTRAINT pk_category_code PRIMARY KEY (category_code),
    CONSTRAINT fk_ref_category_code FOREIGN KEY (ref_category_code) REFERENCES tbl_category (category_code)
) ENGINE=INNODB COMMENT '카테고리';

CREATE TABLE IF NOT EXISTS tbl_menu
(
    menu_code    INT AUTO_INCREMENT COMMENT '메뉴코드',
    menu_name    VARCHAR(30) NOT NULL COMMENT '메뉴명',
    menu_price    INT NOT NULL COMMENT '메뉴가격',
    category_code    INT NOT NULL COMMENT '카테고리코드',
    orderable_status    CHAR(1) NOT NULL COMMENT '주문가능상태',
    CONSTRAINT pk_menu_code PRIMARY KEY (menu_code),
    CONSTRAINT fk_category_code FOREIGN KEY (category_code) REFERENCES tbl_category (category_code)
) ENGINE=INNODB COMMENT '메뉴';

CREATE TABLE IF NOT EXISTS tbl_order
(
    order_code    INT AUTO_INCREMENT COMMENT '주문코드',
    order_date    VARCHAR(8) NOT NULL COMMENT '주문일자',
    order_time    VARCHAR(8) NOT NULL COMMENT '주문시간',
    total_order_price    INT NOT NULL COMMENT '총주문금액',
    CONSTRAINT pk_order_code PRIMARY KEY (order_code)
) ENGINE=INNODB COMMENT '주문';

CREATE TABLE IF NOT EXISTS tbl_order_menu
(
    order_code INT NOT NULL COMMENT '주문코드',
    menu_code    INT NOT NULL COMMENT '메뉴코드',
    order_amount    INT NOT NULL COMMENT '주문수량',
    CONSTRAINT pk_comp_order_menu_code PRIMARY KEY (order_code, menu_code),
    CONSTRAINT fk_order_menu_order_code FOREIGN KEY (order_code) REFERENCES tbl_order (order_code),
    CONSTRAINT fk_order_menu_menu_code FOREIGN KEY (menu_code) REFERENCES tbl_menu (menu_code)
) ENGINE=INNODB COMMENT '주문별메뉴';

CREATE TABLE IF NOT EXISTS tbl_payment
(
    payment_code    INT AUTO_INCREMENT COMMENT '결제코드',
    payment_date    VARCHAR(8) NOT NULL COMMENT '결제일',
    payment_time    VARCHAR(8) NOT NULL COMMENT '결제시간',
    payment_price    INT NOT NULL COMMENT '결제금액',
    payment_type    VARCHAR(6) NOT NULL COMMENT '결제구분',
    CONSTRAINT pk_payment_code PRIMARY KEY (payment_code)
) ENGINE=INNODB COMMENT '결제';

CREATE TABLE IF NOT EXISTS tbl_payment_order
(
    order_code    INT NOT NULL COMMENT '주문코드',
    payment_code    INT NOT NULL COMMENT '결제코드',
    CONSTRAINT pk_comp_payment_order_code PRIMARY KEY (payment_code, order_code),
    CONSTRAINT fk_payment_order_order_code FOREIGN KEY (order_code) REFERENCES tbl_order (order_code),
    CONSTRAINT fk_payment_order_payment_code FOREIGN KEY (order_code) REFERENCES tbl_payment (payment_code)
) ENGINE=INNODB COMMENT '결제별주문';

-- 데이터 삽입
INSERT INTO tbl_category VALUES (null, '식사', null);
INSERT INTO tbl_category VALUES (null, '음료', null);
INSERT INTO tbl_category VALUES (null, '디저트', null);
INSERT INTO tbl_category VALUES (null, '한식', 1);
INSERT INTO tbl_category VALUES (null, '중식', 1);

INSERT INTO tbl_category VALUES (null, '일식', 1);
INSERT INTO tbl_category VALUES (null, '퓨전', 1);
INSERT INTO tbl_category VALUES (null, '커피', 2);
INSERT INTO tbl_category VALUES (null, '쥬스', 2);
INSERT INTO tbl_category VALUES (null, '기타', 2);

INSERT INTO tbl_category VALUES (null, '동양', 3);
INSERT INTO tbl_category VALUES (null, '서양', 3);

INSERT INTO tbl_menu VALUES (null, '열무김치라떼', 4500, 8, 'Y');
INSERT INTO tbl_menu VALUES (null, '우럭스무디', 5000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '생갈치쉐이크', 6000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '갈릭미역파르페', 7000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '앙버터김치찜', 13000, 4, 'N');

INSERT INTO tbl_menu VALUES (null, '생마늘샐러드', 12000, 4, 'Y');
INSERT INTO tbl_menu VALUES (null, '민트미역국', 15000, 4, 'Y');
INSERT INTO tbl_menu VALUES (null, '한우딸기국밥', 20000, 4, 'Y');
INSERT INTO tbl_menu VALUES (null, '홍어마카롱', 9000, 12, 'Y');
INSERT INTO tbl_menu VALUES (null, '코다리마늘빵', 7000, 12, 'N');

INSERT INTO tbl_menu VALUES (null, '정어리빙수', 10000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '날치알스크류바', 2000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '직화구이젤라또', 8000, 12, 'Y');
INSERT INTO tbl_menu VALUES (null, '과메기커틀릿', 13000, 6, 'Y');
INSERT INTO tbl_menu VALUES (null, '죽방멸치튀김우동', 11000, 6, 'N');

INSERT INTO tbl_menu VALUES (null, '흑마늘아메리카노', 9000, 8, 'Y');
INSERT INTO tbl_menu VALUES (null, '아이스가리비관자육수', 6000, 10, 'Y');
INSERT INTO tbl_menu VALUES (null, '붕어빵초밥', 35000, 6, 'Y');
INSERT INTO tbl_menu VALUES (null, '까나리코코넛쥬스', 9000, 9, 'Y');
INSERT INTO tbl_menu VALUES (null, '마라깐쇼한라봉', 22000, 5, 'N');

INSERT INTO tbl_menu VALUES (null, '돌미나리백설기', 5000, 11, 'Y');

COMMIT;</code></pre>
<hr />
<p>루트 계정에서 만든 계정으로 tbl_menu와 tbl_category 안에 있는 것들을 조회해보았다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/932712d9-b7b9-42d7-afbe-e6f4501a8b2a/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c843fe04-dba8-4c5f-b5c0-e18dba6add4d/image.png" /></p>
<hr />
<h2 id="mariadb-동작원리">MariaDB 동작원리</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a31fc53f-45c9-4d56-87cb-91eef4c28afb/image.png" /></p>
<ol>
<li><p><strong>Client / Server 통신</strong>
클라이언트에서 SQL 쿼리를 MariaDB 서버로 전송한다.</p>
</li>
<li><p><strong>Query Parsing</strong>
MariaDB 서버가 쿼리를 받으면 쿼리를 Parsing 한다.
Parser(파서)는 SQL 문장을 이해할 수 있는 단위로 나누고 문법이 유효한지, 키워드가 올바른지, 사용한 컬럼 및 테이블이 존재하는지 등 확인하여 오류가 있으면 프로세스를 중단한다.</p>
</li>
<li><p><strong>최적화 및 실행 계획 생성</strong>
Parsing이 완료되면, 쿼리 최적화기 (optimizer)가 작동해 쿼리를 효율적으로 실행하려고 한다.</p>
</li>
</ol>
<p>-&gt; 사용할 인덱스 결정, Join 순서, 데이터를 읽는 방법 등 = '실행계획' 형태로 생성</p>
<ol start="4">
<li><p><strong>쿼리 실행</strong>
실행 계획에 따라, MariaDB 서버는 스토리지 엔진을 통해 필요한 데이터를 불러오거나 변경하고 실제 데이터베이스 파일 또는 인덱스에 접근한다.</p>
</li>
<li><p><strong>결과 반환</strong>
쿼리 실행이 완료되면 MariaDB 서버는 결과 세트(Result Set)를 클라이언트에게 반환한다.</p>
</li>
</ol>
<p>-&gt; SELECT의 경우는 검색된 행들이 되고 INSERT, UPDATE, DELETE의 경우 영향을 받는 행의 수가 된다.</p>
<hr />
<p><strong>from 절 없는 select 해보기</strong></p>
<pre><code class="language-sql">SELECT 7 + 3;
SELECT 10 * 2; -- 사칙연산
SELECT 6 % 3, 6 % 4; -- 나머지
SELECT NOW(); -- os의 시간 나타내기
SELECT CONCAT('유', ' ', '관순'); -- 문자열 합하기

-- 별칭(alias)
SELECT 7 + 3 AS '합', 10 * 2 AS '곱';
SELECT NOW() AS '현재 시간';
SELECT 7 + 3 AS ' 합입니다.' -- 별칭을 달 때 특수기호가 있다면 싱글쿼테이션(')이 필수다.</code></pre>
<p><strong>select, from 절을 활용해보기</strong></p>
<pre><code class="language-sql">SELECT
         menu_code
      , menu_name
      , menu_price
      , category_code
      , orderable_status
    FROM tbl_menu;

SELECT
         category_code
      , category_name
      , ref_category_code
    FROM tbl_category;</code></pre>
<p><strong>join 절 사용해보기</strong>
2개의 테이블을 연결해주는 join 절도 사용 가능하다.</p>
<pre><code class="language-sql">SELECT
         menu_name
      , category_name
    FROM tbl_menu m 
    JOIN tbl_category c ON m.category_code = c.category_code;</code></pre>
<p>select, from, join, order by, group by, like 등 여러가지 문법에 대해서는 다음 게시글에 작성한다.</p>