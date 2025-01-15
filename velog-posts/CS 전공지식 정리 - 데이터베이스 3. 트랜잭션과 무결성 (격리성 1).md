<h1 id="트랜잭션과-무결성---격리성">트랜잭션과 무결성 - 격리성</h1>
<p>트랜잭션과 무결성파트의 <strong>격리성</strong>에 대해 다뤄볼까 한다.</p>
<p>여러 개의 격리 수준이 있는데 내용이 어려워서 직접 정리를 하면서 봐야 알 수 있지 않을까 싶어서 해본다.</p>
<blockquote>
<p><strong>격리성</strong> : 트랜잭션 수행 시 서로 끼어들지 못하는 것</p>
</blockquote>
<p>여러 개의 병렬 트랜잭션은 서로 격리돼 순차적으로 실행되는 것처럼 작동돼야 한다.</p>
<p>그리고 데이터베이스는 여러 사용자가 같은 데이터에 접근할 수 있어야 한다.</p>
<p>순차적으로 접근할 수 있게 하면 쉽겠지만 성능이 별로기 때문에 여러 개의 격리 수준으로 나눠 격리성을 보장한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9d32a9eb-3ed3-4992-be25-1071755d04a0/image.png" /></p>
<ul>
<li>READ UNCOMMITED</li>
<li>READ COMMITTED</li>
<li>REPEATABLE READ</li>
<li>SERIALIZABLE</li>
</ul>
<p>순서가 위로 갈수록 동시성이 강하면서 / 격리성은 약해진다.</p>
<hr />
<h2 id="serializable">SERIALIZABLE</h2>
<p>가장 엄격한 격리 수준이다.
<code>SERIALIZABLE</code>에서 <strong>여러 트랜잭션이 동일한 레코드에 동시 접근 불가, 어떠한 데이터 부정합 문제도 발생 X</strong> 
하지만 트랜잭션이 순차적으로 처리되어야 하므로 동시 처리 성능이 매우 떨어진다.</p>
<blockquote>
<p>MySQL에서 <code>SELECT FOR SHARE/UPDATE</code>는 대상 레코드에 각각 읽기/쓰기 잠금을 거는 것이다.
하지만 순수한 <code>SELECT</code> 작업은 아무런 레코드 잠금 없이 실행된다.</p>
</blockquote>
<ul>
<li><code>잠금 없는 일관된 읽기(Non-locking consistent read)</code> : 순수한 <code>SELECT</code> 문을 통한 잠금 없는 읽기를 의미하는 것</li>
</ul>
<p><strong>그럼에도</strong> <code>SERIALIZABLE</code>는 순수한 SELECT 작업에서도 대상 레코드에 넥스트 키 락을 읽기 잠금(공유락, Shared Lock)으로 건다.</p>
<p>따라서 한 트랜잭션에서 넥스트 키 락이 걸린 레코드를 다른 트랜잭션에서는 절대 추가/수정/삭제할 수 없다.</p>
<blockquote>
<p>참고로, 스프링이 제공하는 트랜잭션 AOP 기능은 개발자가 별도로 트랜잭션 격리수준을 설정하지 않을 경우 데이터소스의 기본 트랜잭션 격리수준을 따르게 된다.</p>
<p>즉, 따로 격리수준을 설정하면 <code>SERIALIZABLE</code> 로도 격리수준 설정이 가능하다.</p>
<p>설정하지 않고 MySQL InnoDB 스토리지 엔진 기반의 테이블을 사용한다면 <code>Repeatable Read</code> 격리 수준을 따르게 된다.</p>
</blockquote>
<h3 id="동시성-이슈">동시성 이슈</h3>
<p><strong>그럼 동시성 이슈(Lost Update)를 해결하기 위해서 SERIALIZABLE를 적용해야 할까?</strong></p>
<p>동시성 이슈에 대한 대표적인 사례인 '재고 이슈'를 보면</p>
<p>ex) 어느 하나의 재고에 대해 n개가 있는데 변경 작업(재고 1 감소)을 시도하는 A 트랜잭션이 종료되기 전 B 트랜잭션이 실행된다면?</p>
<ul>
<li><strong>SERIALIZABLE 적용 전</strong></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b5f6f908-f701-4fa5-884b-41b539aa920d/image.png" /></p>
<p>A 트랜잭션에서 n개 -&gt; n - 1개로 줄이는 과정에서
B 트랜잭션이 시도돼 n개로 알고 있지만 실제로는 A에서 재고 변경이 일어나면서 n - 1개가 된 상태일 때.</p>
<p>트랜잭션 A 와 트랜잭션 B 가 거의 동시에 처리될 때, 트랜잭션 B 의 변경작업이 유실(Lost Update)되는 이슈가 발생한다는 것을 예측할 수 있다.</p>
<hr />
<ul>
<li><strong>SERIALIZABLE 적용 후</strong></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/65e25a02-9cbb-4f37-99c5-a75af0060bfa/image.png" /></p>
<p>SELECT 작업 시 공유 잠금을 사용하고 있는 것을 볼 수 있다.</p>
<p>한 트랜잭션에서 특정 레코드에 공유 잠금을 걸었다면 -&gt; 다른 트랜잭션에서는 해당 레코드를 조회하거나 공유 잠금을 걸 수는 있으나, 쓰기 잠금이나 변경 작업은 할 수 없게 된다. <strong>(Lock waiting)</strong></p>
<p>그래서 트랜잭션 A 가 해당하는 레코드에 대해 공유 잠금을 동반한 읽기를 하면, 트랜잭션 B 역시 해당 레코드에 대해 공유 잠금을 동반한 읽기를 할 수는 있다.</p>
<hr />
<p>하지만 변경을 하게 된다면?</p>
<p>트랜잭션 B 가 건 공유 잠금 때문에 <code>Lock waiting</code> 에 빠지게 되며, 트랜잭션 B 역시 변경 작업 수행 시 앞서 트랜잭션 A 가 건 공유 잠금 때문에 <code>Lock waiting</code> 에 빠지게 된다. <strong>즉, 데드락(Dead-lock)이 발생하게 된다.</strong></p>
<p>MySQL의 InnoDB 스토리지 엔진의 경우, 내부적으로 잠금이 교착 상태에 빠지지 않았는지 체크하기 위해 잠금 대기 목록(Wait for list)을 관리한다.</p>
<p>이 때, <strong>별도의 데드락 감지 스레드가 주기적으로 해당 잠금 대기 목록을 검사해 교착 상태에 빠진 트랜잭션들을 찾아서 그 중 하나(또는 여러개)를 강제로 종료</strong>한다.</p>
<blockquote>
<p>어느 트랜잭션을 먼저 강제 종료할 것인지를 판단하는 기준은 트랜잭션의 언두 로그 양인데, <strong>적은 언두 로그 양을 가진 트랜잭션을 우선적으로 롤백</strong>시킨다.</p>
</blockquote>
<p>결론적으로, 트랜잭션 격리수준을 <code>Serializable</code> 로 했을 때, <strong>데드락을 발생시키고 다른 트랜잭션을 롤백시킴으로써 Lost Update 를 방지할 수는 있게 된다.</strong></p>
<hr />
<h4 id="데드락을-발생시키고-다른-트랜잭션을-롤백시킴으로써-lost-update-를-방지하는-것이-동시성-이슈를-해결하는-좋은-방안일까">데드락을 발생시키고 다른 트랜잭션을 롤백시킴으로써 Lost Update 를 방지하는 것이 동시성 이슈를 해결하는 좋은 방안일까?</h4>
<p>예시로 들었던 동시성 이슈의 근본적 원인은 <strong>어떤 트랜잭션이 작업 중인 레코드에 대해 다른 트랜잭션에서도 동시 조회를 허용했기 때문</strong>이며,</p>
<p><code>Serializable</code>로 하더라도 다른 트랜잭션에서 공유 잠금이 걸린 레코드를 조회할 수 있기에 이 데이터를 기반으로 <code>Update</code> 하는 행위 자체는 여전히 가능한 것이다.</p>
<p>이미 한 트랜잭션에서 조회, 수정하는 절차를 하고 있다면 그 해당하는 재고 데이터는 불확실한 상황이기 때문에 다른 트랜잭션이 접근을 하게 해서는 안 되는 것이다.</p>
<p>-&gt; <strong>해당 데이터를 읽고 그 다음 작업들까지 수행할 수 있기 때문에 불필요한 서버 리소스를 낭비하는 것</strong></p>
<p>추가로 <strong>데드락으로 인해 롤백되어 희생되는 트랜잭션(= victim)</strong>이 발생하는 것도 서버 입장에서 데드락으로 인해 해당 트랜잭션이 롤백되었을 때 발생하는 예외를 별도로 처리해주어야 하기 때문에 고민거리이다.</p>
<hr />
<h3 id="동시성-이슈-해결책">동시성 이슈 해결책</h3>
<blockquote>
<p><strong>배타 잠금(exclusive lock)</strong> : 공유 잠금과 마찬가지로 SELECT 작업 시 사용하는 것</p>
<ul>
<li>공유 잠금과 달리 배타 잠금이 걸린 레코드에는 다른 트랜잭션에서 쓰기 작업뿐만 아니라 조회 작업도 허용하지 않는다.</li>
<li>추가로, 공유 잠금과 배타 잠금을 걸 수도 없다.</li>
</ul>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cd7e4b5e-9473-49ea-9ed3-f4105e887e35/image.png" /></p>
<p>사진은 배타 잠금을 적용한 이후의 상황이다.</p>
<p>트랜잭션 격리수준을 Serializable 로 적용했을 때와 달리</p>
<p>트랜잭션 B는 불필요한 작업을 수행하지 않으며, 데드락을 유도하지 않아 둘 중 한 트랜잭션이 롤백될 일도 없다. </p>
<blockquote>
<p>즉, 서버 리소스를 보다 효율적으로 사용할 수 있게 되는 것</p>
</blockquote>
<blockquote>
<p>공유 잠금이나 배타 잠금은 MySQL 수준에서 제공하는 잠금 기능으로 비관적 락(Pessimistic lock)이라고도 부른다.</p>
</blockquote>
<p>다음은 Repeatable Read에 대해 정리해보겠다.</p>