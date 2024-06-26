<p>트랜잭션이란 '거래'라는 뜻으로 DB 내에서 하나의 그룹으로 처리되어야 하는 명령문들을 모아 놓은 논리적인 작업 단위이다.</p>
<ul>
<li>여러 개의 명령어의 집합이 <strong>정상적으로 처리되면 정상 종료</strong>된다.</li>
<li><strong>하나의 명령어라도 잘못되면 전체 취소</strong>된다.</li>
<li><strong>데이터의 일관성을 유지</strong>하면서 <strong>안정적으로 데이터를 복구</strong>하기 위해 사용한다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4294168f-d340-4b12-824e-9e486f7b73e2/image.png" /></p>
<h2 id="시작하기-전">시작하기 전</h2>
<blockquote>
<p>autocommit 이 set 되어 있을 수 있다.</p>
</blockquote>
<pre><code>-- autocommit 비활성화
SET autocommit = 0;
SET autocommit = OFF;</code></pre><p>둘 중 하나를 하자. 반대로 키고 싶다면</p>
<pre><code class="language-sql">SET autocommit = 1;
SET autocommit = ON;</code></pre>
<hr />
<h2 id="트랜잭션">트랜잭션</h2>
<p>트랜잭션을 시작할 때는 이것을 적고 단계를 정한다.</p>
<pre><code class="language-sql">START TRANSACTION;</code></pre>
<hr />
<pre><code class="language-sql">-- 1단계
INSERT
  INTO tbl_menu
VALUES
(
  NULL, '바나나해장국', 8500
, 4, 'Y'
);

-- 2단계
UPDATE tbl_menu
    SET menu_name = '수정된 메뉴'
 WHERE menu_code = 5;</code></pre>
<p><strong>만약 시작하기 전에 autocommit을 끄지 않았다면</strong> 2단계까지 진행하면서 commit이 2번 진행돼 처리과정을 <strong>DB에 영구 저장하게 된다.</strong></p>
<p>하지만 꺼뒀기 때문에?
아직은 영구 저장이 되지는 않은 채로 우리 눈에는 바뀐 것처럼 보이게 된다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/874b77f7-e89f-42fc-82ef-fdbd4a84eed1/image.png" /></p>
<blockquote>
<p>25, 26 메뉴는 이전에 DELETE 예제하면서 지워졌다.</p>
</blockquote>
<hr />
<h3 id="rollback">ROLLBACK</h3>
<p>이것을 진행하면 commit하기 전으로 돌아가게 된다.</p>
<pre><code class="language-sql">ROLLBACK;</code></pre>
<p>위에서 아직 완전 저장이 되지 않았기 때문에
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/313f3252-38f4-4845-a5da-c2e70d7b390d/image.png" /></p>
<p>이전처럼 ROLLBACK이 된다.</p>
<hr />
<h3 id="commit">COMMIT</h3>
<p>앞으로도 예제를 계속 해나갈 것이기 때문에 commit은 하지 않을 것이다.
autocommit을 키고 예제를 진행하거나</p>
<pre><code class="language-sql">-- 1단계
INSERT
  INTO tbl_menu
VALUES
(
  NULL, '바나나해장국', 8500
, 4, 'Y'
);

commit;

-- 2단계
UPDATE tbl_menu
    SET menu_name = '수정된 메뉴'
 WHERE menu_code = 5;

commit;</code></pre>
<p>이것처럼 성공하는 단계마다 commit을 추가해주면 된다.</p>
<hr />
<h2 id="savepoint">SAVEPOINT</h2>
<p>SAVEPOINT는 '임시저장' 의 역할을 한다.</p>
<ul>
<li>ROLLBACK 과 달리 <strong>특정 부분에서 트랜잭션을 취소시킬 수 있다.</strong></li>
<li>SAVEPOINT를 지정한뒤 <code>ROLLBACK TO SAVEPOINT 이름;</code>을 실행하면 지정한 해당 SAVEPOINT 지점까지 처리한 작업이 취소(ROLLBACK)된다.</li>
</ul>
<pre><code class="language-sql">SAVEPOINT S1;
-- 1단계
INSERT
  INTO tbl_menu
VALUES
(
  NULL, '바나나해장국', 8500
, 4, 'Y'
);

SAVEPOINT S2;

-- 2단계
UPDATE tbl_menu
    SET menu_name = '수정된 메뉴'
 WHERE menu_code = 5;</code></pre>
<p>위와 같이 SAVEPOINT 를 S1, S2로 만들어주고</p>
<pre><code class="language-sql">ROLLBACK TO S1
-- ROLLBACK TO S2</code></pre>
<p>을 통해 S1 이나 S2과 같이 특정 구간 ROLLBACK이 가능하다.</p>