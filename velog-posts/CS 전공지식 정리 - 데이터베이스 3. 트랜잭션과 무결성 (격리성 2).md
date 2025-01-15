<h2 id="repeatable-read">Repeatable Read</h2>
<blockquote>
<p>MySQL에서는 트랜잭션마다 트랜잭션 ID를 부여하여 트랜잭션 ID보다 작은 트랜잭션 번호에서 변경한 것만 읽게 된다.</p>
</blockquote>
<p>아래에서 할 예시들은 트랜잭션 번호를 생략해두고 진행했다.</p>
<hr />
<blockquote>
<p><code>Repeatable Read</code> 에서는 다른 트랜잭션 격리수준인 <code>Read Uncommitted</code> 나 <code>Read Committed</code> 에서와 달리 <code>Non-Repeatable Read</code> *<em>현상이 발생하지 않는다. *</em></p>
</blockquote>
<blockquote>
<p><strong>Non-Repeatable Read</strong> : 하나의 트랜잭션에서 같은 값을 두 번 select 했을 때 각각 다른 값이 읽히는 현상</p>
</blockquote>
<p>이게 무슨 소리냐면</p>
<ul>
<li>똑같은 레코드를 2번 조회하는 트랜잭션 A</li>
<li>해당 레코드를 수정하고 커밋하는 트랜잭션 B</li>
</ul>
<ol>
<li>트랜잭션 A의 1번째 조회가 진행</li>
<li>트랜잭션 B가 해당 레코드 수정 -&gt; 커밋</li>
<li>트랜잭션 A의 2번째 조회가 진행 == <strong>데이터가 변경돼 Non-Repeatable Read 발생</strong></li>
</ol>
<p>위 순서대로 진행이 된다면 데이터가 변경돼 Non-Repeatable Read 발생한다는 것이다.</p>
<p>그럼 문득 &quot;<strong>하나의 트랜잭션에서 같은 데이터의 조회를 2번 할 일이 없다면?</strong>&quot; 이라고 생각이 들 수도 있다.</p>
<blockquote>
<p>MySQL InnoDB 스토리지 엔진의 기본 트랜잭션 격리수준으로 <code>Repeatable Read</code> 을 설정한 이유가 뭘까?</p>
</blockquote>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ec8d7ba0-15e0-4923-ac25-9c78ca8cfba6/image.png" /></p>
<p>위 사진은 MySQL docs에서 Repeatable Read를 정의한 내용으로</p>
<p>첫 줄의 &quot;Consistent reads within the same transaction read the snapshot established by the first read.&quot; </p>
<p>= 동일한 트랜잭션 내에서의 일관된 조회는 첫 번째 조회에 의해 설정된 스냅샷을 읽는다. 는 뜻이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7570d58b-5c29-4cf7-97e6-6923f34e0cfe/image.png" /></p>
<p><strong>스냅샷</strong> : &quot;일관된 조회를 위해 특정 격리 수준에서 사용되는 것으로서 다른 트랜잭션에 의해 변경 사항이 커밋되더라도 동일하게 유지되는 특정 시간의 데이터 표현&quot;</p>
<p>이번엔 다른 형태의 트랜잭션을 가정을 들어 설명하겠다.</p>
<ul>
<li>각자 다른 테이블에 대해 1번씩 조회하는 트랜잭션 C</li>
<li>C가 2번째로 조회하는 테이블의 레코드를 수정 후 커밋하는 트랜잭션 D</li>
</ul>
<ol>
<li>트랜잭션 C의 1번째 테이블의 특정 레코드 조회가 진행</li>
<li>트랜잭션 D가 2번째 테이블의 특정 레코드 수정 -&gt; 커밋</li>
<li>트랜잭션 C의 2번째 테이블의 해당 레코드 조회가 진행</li>
</ol>
<p>위 과정을 겪으면 어떻게 될까?</p>
<ol>
<li>C는 한 트랜잭션에서 1, 2번째 조회를 하기 때문에 D가 수정하기 전의 2번째 테이블 속 레코드를 읽기</li>
<li>C는 2번째 조회 시 D가 수정한 레코드의 데이터로 읽기</li>
</ol>
<p>정답은 바로 1번이다. 말이 길어져서 간단히 그림으로 설명하면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8cf3c50b-73ca-4a5c-bfa7-a6c2535b5637/image.png" /></p>
<p>이렇게 plan을 C 중간에 수정하고 커밋하더라도 before이었던 레코드가 그대로 출력된다는 뜻이다.</p>
<p>트랜잭션 격리수준이 <code>Repeatable Read</code> 라면 한 트랜잭션 내에서 동일한 select 를 2번 이상 시도해도 동일한 결과가 반환된다.</p>
<p>다른 테이블 데이터 역시 최초 조회(first read) 시점에서의 데이터가 마치 스냅샷(snapshot)처럼 유지되어 있던 것이다.</p>
<hr />
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/13567f7b-2d06-4d27-aa0b-e3ca49e5a74e/image.png" /></p>
<p>그렇게 할 수 있는 이유는 </p>
<blockquote>
<p><strong>일관된 조회</strong> : <strong>조회한 시점</strong>을 기준으로 쿼리 결과를 표시하는 스냅샷 정보를 사용하는 것</p>
</blockquote>
<p>2번째 줄의 <code>If queried ~</code> 로 시작하는 문구를 보면 <strong>&quot;조회된 데이터(queried data)가 다른 트랜잭션에 의해 변경되었다면 본래 데이터는 undo log 의 내용을 통해 재구성된다.&quot;</strong> 라는 뜻인데</p>
<p>트랜잭션 C는 D가 수정한 데이터의 원본 데이터가 undo log 에 들어갔고 이 undo log 로부터 해당 데이터를 가져왔다는 것을 알 수 있다.</p>
<blockquote>
<p><strong>언두 로그</strong> : 활성 트랜잭션에 의해 수정된 데이터의 복사본을 보관하는 저장 영역</p>
<p>즉, 수정하기 전 원본 데이터가 저장되는 영역</p>
</blockquote>
<hr />
<p>대신 Phantom Read 라고 하는 현상이 발생하기도 한다.</p>
<h3 id="phantom-read">Phantom Read</h3>
<p><strong>Phantom Read</strong> : 한 트랜잭션 내에서 동일한 쿼리를 보냈을 때 해당 조회 결과가 다른 경우</p>
<blockquote>
<ul>
<li><strong>Phantom Read와 Non-Repeatable Read의 차이??</strong></li>
</ul>
</blockquote>
<ul>
<li>Non-Repeatable Read는 1개의 Row의 데이터의 값이 변경되는 것이며 (Update 또는 Delete)</li>
<li>Phantom Read는 다수의 건을 요청하는 것에 대해서 데이터의 값이 변경되는 것<blockquote>
<p>즉, Non-Repeatable Read는 위에서 봤다시피 1개의 Row의 데이터를 2번 조회하는 트랜잭션 내에서 다른 트랜잭션에 의해 update나 delete로 하나의 데이터가 변경됐을 때 언두 로그로 인해 변경되기 전의 데이터를 조회할 수 있다는 것이다.</p>
</blockquote>
</li>
</ul>
<p><code>Phantom Read</code>를 예를 들어 설명하면</p>
<ul>
<li>member라는 테이블을 select로 다 조회하는 트랜잭션 E<pre><code class="language-sql">select * from member</code></pre>
</li>
</ul>
<p>E는 위와 같은 쿼리문이 2번 사용되는 트랜잭션이라고 생각하면 된다.</p>
<ul>
<li>member라는 테이블에 Insert와 같이 데이터를 하나 추가 후 commit 하는 트랜잭션 F<pre><code class="language-sql">INSERT INTO member (member_id, member_name) 
VALUES (70, 'Jehun')</code></pre>
<pre><code class="language-sql">commit</code></pre>
</li>
</ul>
<ol>
<li>E에서 1번째 select =&gt; Jehun이라는 멤버가 추가되기 전 데이터들이 조회</li>
<li>F에서 Insert 후 commit =&gt; member 테이블에 Jehun 추가</li>
<li>E에서 2번째 select =&gt; Jehun이라는 멤버가 추가된 후 데이터들이 조회 =&gt; 이 때 <code>Phantom Read</code>가 발생하는 것이다.</li>
</ol>
<hr />
<p>기존에 Real MySQL 8.0 책을 정리했을 때 언두 로그를 활용한 Repeatable Read에 대해서 잘 몰랐는데 이제 어느 정도 이해가 된 것 같다.</p>