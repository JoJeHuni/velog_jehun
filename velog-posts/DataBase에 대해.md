<p>DataBase에 대해 작성하며 개념부터 차근차근 다뤄보려고 한다.
간단한 것부터 해서 SQL 관련해서까지 정리해보려고 한다.</p>
<h4 id="기본-용어">기본 용어</h4>
<blockquote>
<ul>
<li>Data : 관찰의 결과로 나타난 <strong>정량적 or 정성적인 실제 값</strong>을 의미한다.</li>
</ul>
</blockquote>
<ul>
<li>Information : 데이터를 기반으로 하여 <strong>의미를 부여한 것</strong>이다.</li>
</ul>
<hr />
<h2 id="database란">Database란?</h2>
<ul>
<li>Database : 한 조직에 필요한 정보를 여러 응용 시스템에서 공용할 수 있게 <strong>데이터를 모으고</strong>, <strong>중복되는 데이터는 최소화</strong>해 <strong>구조적으로 통합 및 저장</strong>해둔 것이다.</li>
</ul>
<hr />
<h3 id="database의-정의">Database의 정의</h3>
<ol>
<li><strong>운영 데이터 (Operational Data)</strong> : 조직의 목적을 위해 사용되는 데이터</li>
<li><strong>공용 데이터 (Shared Data)</strong> : 공동으로 사용되는 데이터</li>
<li><strong>통합 데이터 (Integrated Data)</strong> : <strong>중복을 최소화</strong>하여 중복으로 인한 데이터 불일치 현상 제거 
 -&gt; 이후 DB 모델링에서 anomaly (이상, 변칙) 에 대해 다뤄볼 것이다.</li>
<li><strong>저장 데이터 (Stored Data)</strong> : 컴퓨터 저장 장치에 저장된 데이터 
 -&gt; 영속성을 가져야 한다. (ex. 메모장, Excel 등)</li>
</ol>
<hr />
<h3 id="database의-특징">Database의 특징</h3>
<ol>
<li><strong>실시간 접근성 (Real Time Accessibility)</strong> : 사용자가 데이터를 요청하면 실시간으로 결과를 서비스한다.</li>
<li><strong>계속적인 변화 (Continuous Change)</strong> : 데이터 값은 시간에 따라 항상 바뀐다.</li>
<li><strong>동시 공유 (Concurrent Sharing)</strong> : 서로 다른 업무 또는 여러 사용자에게 동시 공유된다.</li>
<li><strong>내용에 따른 참조 (Reference By Content)</strong> : 저장된 데이터는 데이터의 물리적 위치가 아니라 데이터 값에 따라 참조된다.</li>
</ol>
<hr />
<h2 id="dbms-database-management-system">DBMS (Database Management System)</h2>
<h3 id="dbms란">DBMS란?</h3>
<blockquote>
<p>DB에서 데이터를 정의, 제어, 조작, 추출 등 할 수 있게 해주는 <strong>DB 전용 관리 프로그램</strong>을 말한다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/27be7c72-0fa8-4e86-ae84-bc483436e4c8/image.png" /></p>
</blockquote>
<h3 id="dbms의-기능">DBMS의 기능</h3>
<table>
<thead>
<tr>
<th>주요 기능</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>데이터 추출 (Retrieval)</td>
<td>사용자가 조회하는 데이터 혹은 응용 프로그램의 데이터를 추출 (추출이라고 하지만, 복사하듯이 하는 것)</td>
</tr>
<tr>
<td>데이터 조작 (Manipulation)</td>
<td>데이터를 조작하는 소프트웨어(응용 프로그램)가 요청하는 데이터의 삽입, 수정, 삭제 작업을 지원</td>
</tr>
<tr>
<td>데이터 정의 (Definition)</td>
<td>데이터의 구조를 정의하고 데이터 구조에 대한 삭제 및 변경 기능을 수행</td>
</tr>
<tr>
<td><strong>데이터 제어 (Control)</strong></td>
<td>데이터베이스 사용자를 생성하고 모니터링하며 접근을 제어 백업과 회복, 동시성 제어 등의 기능을 지원</td>
</tr>
</tbody></table>
<hr />
<h3 id="사용-시-이점">사용 시 이점</h3>
<table>
<thead>
<tr>
<th>주요 이점</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>데이터 중복 최소화</td>
<td>데이터와 응용 프로그램을 분리시킴으로써 상호 영향 정도를 줄일 수 있다.</td>
</tr>
<tr>
<td>쿼리 언어</td>
<td>DBMS는 SQL(Structured Query Language)과 같은 강력한 쿼리 언어를 제공하여, 복잡한 검색과 분석 작업을 손쉽게 수행할 수 있게 한다.</td>
</tr>
<tr>
<td>데이터 무결성</td>
<td>DBMS는 데이터의 무결성을 보장하기 위한 다양한 제약 조건과 규칙을 설정할 수 있다. 이는 데이터의 품질과 정확성을 보장한다.</td>
</tr>
<tr>
<td>데이터 백업 및 복구</td>
<td>DBMS는 데이터의 백업과 복구를 지원하여, 시스템 장애나 데이터 손상 시에도 데이터를 복원할 수 있다.</td>
</tr>
<tr>
<td>표준화</td>
<td>DBMS는 표준화된 방법을 통해 데이터를 관리하므로, 데이터의 구조와 접근 방법이 일괄적이다. 이로 인해 애플리케이션 개발 및 유지보수가 용이해 진다.</td>
</tr>
</tbody></table>
<hr />
<h3 id="종류-및-특징">종류 및 특징</h3>
<table>
<thead>
<tr>
<th></th>
<th>SQL Server</th>
<th>Oracle</th>
<th>MySQL</th>
<th>MariaDB</th>
<th>DB2</th>
<th>SQLite</th>
</tr>
</thead>
<tbody><tr>
<td>제조사</td>
<td>MS</td>
<td>Oracle</td>
<td>Oracle</td>
<td>MariaDB재단</td>
<td>IBM</td>
<td>D.Richard Hipp(오픈소스)</td>
</tr>
<tr>
<td>기반 운영체제</td>
<td>윈도우</td>
<td>윈도우, 유닉스, 리눅스</td>
<td>윈도우, 유닉스, 리눅스</td>
<td>윈도우, 유닉스, 리눅스</td>
<td>유닉스</td>
<td>모바일OS (안드로이드, IOS 등)</td>
</tr>
<tr>
<td>용도</td>
<td>윈도우 기반 기업용</td>
<td>대용량 데이터베이스</td>
<td>소용량 데이터베이스</td>
<td>소용량 데이터베이스</td>
<td>대용량 데이터베이스</td>
<td>모바일 전용 데이터베이스</td>
</tr>
</tbody></table>
<hr />
<p>DB는 여러 변천 과정을 겪었으나 이제는 RDBMS를 주로 사용한다.</p>
<h2 id="rdbms">RDBMS</h2>
<ul>
<li><strong>관계 데이터 모델(Relational Data Model)</strong>
: 데이터를 테이블 형태로 구성(행과 열로 구성)하며, <strong>서로 다른 테이블 간의 관계는 키를 통해 정의함.</strong> 데이터의 무결성을 유지하고 SQL을 사용하여 복잡한 쿼리와 데이터 조작을 용이하게 함</li>
</ul>
<hr />
<ul>
<li><strong>관계형 데이터베이스(RDBMS, Relational Database Management System)</strong><blockquote>
<p>Relational 이 번역해서 &quot;관계형&quot;으로 느낄 수 있겠지만, 사실 Relation 이라고 하는 <strong>데이터를 원자 값으로 갖는 이차원의 테이블</strong>의 의미를 갖는다.</p>
</blockquote>
</li>
</ul>
<p>💡 데이터를 테이블의 형태로 저장하며, 각 테이블은 행(레코드)과 열(필드)로 구성되어 있다. 테이블 간의 관계는 공통 필드를 통해 형성된다.
SQL(Structured Query Language)은 RDBMS에서 데이터를 조작하고 쿼리하는데 주로 사용되는 언어로 엄격한 데이터 무결성 규칙을 가지며, ACID(Atomicity, Consistency, Isolation, Durability) 트랜잭션 속성을 지원한다.</p>
<blockquote>
<ol>
<li><strong>Atomicity(원자성)</strong> : 트랜잭션 내의 모든 연산은 하나의 단위로 간주되어야 한다.</li>
<li><strong>Consistency(일관성)</strong> : 트랜잭션의 실행이 완료되면, 데이터베이스는 일관된 상태를 유지해야 한다. (무결성 제약조건을 만족함을 보증)</li>
<li><strong>Isolation(고립성)</strong> : 동시에 실행되는 여러 트랜잭션이 서로 영향을 주지 않도록 관리해서 동시성을 관리해야 한다.</li>
<li><strong>Durability(지속성)</strong> : 트랙잭션이 성공적으로 완료되면 시스템에 장애가 발생하더라도 영구적으로 반영되어야한다.(커밋 되면 안전하게 저장됨을 보증)</li>
</ol>
</blockquote>
<blockquote>
<p><strong>장점</strong></p>
</blockquote>
<ul>
<li>데이터 무결성을 유지하는데 효과적이다.</li>
<li>강력한 쿼리 언어(SQL)를 통해 복잡한 데이터 조작이 가능하다.</li>
<li>데이터의 정규화를 통해 중복성을 최소화한다.</li>
</ul>
<blockquote>
<p><strong>단점</strong></p>
</blockquote>
<ul>
<li>복잡한 객체 관계를 표현하는데 한계가 있다.</li>
<li>스키마 변경이 어렵고 비용이 많이 든다.</li>
</ul>