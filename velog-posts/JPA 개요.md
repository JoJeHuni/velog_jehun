<h1 id="jpa란">JPA란?</h1>
<p>자바 진영의 ORM(Object Relational Mapping) 기술 표준으로 ORM 기술을 사용하기 위한 표준 인터페이스의 모음이다.</p>
<p>ORM은 자바 객체와 DB테이블을 매핑하고 자바 객체간의 관계를 토대로 SQL을 생성 및 실행 할 수 있으며 대중적인 언어에는 대부분 ORM 기술이 존재한다.</p>
<p>JPA 2.1 기준 표준 명세를 구현한 구현체들(Hibernate, EclipseLink, DataNucleus) 중에 대부분 Hibernate를 사용하므로 JPA를 활용하기 위해서는 Hibernate를 사용하게 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fe003486-969f-40df-bd17-245d771d51f4/image.png" /></p>
<hr />
<h2 id="특징">특징</h2>
<p>영속성 컨텍스트가 엔티티를 생명주기를 통해 관리한다.</p>
<p>native SQL을 통해 직접 SQL을 해당 DB에 맞게 작성할 수 있다.
DBMS 별로 dialect (방언) 를 제공한다.</p>
<p>아래 사진처럼 뭐 MySQL 이나 Oracle 이나 H2 나 그런 것들을 자동으로 방언이 있어서 알아들을 수 있긴 하다. (근데 100%는 불가능하다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/88256966-882d-4d26-aa2e-236ef15a1765/image.png" /></p>
<hr />
<h2 id="사용-이유">사용 이유</h2>
<h3 id="장점">장점</h3>
<ol>
<li>객체 지향(Java, 단방향), 관계 지향(양방향) 2가지 다른 패러다임의 불일치를 해소해준다. SQL 중심이 아닌 객체 지향 패러다임 중심의 개발이 가능하다.</li>
<li>직접 SQL을 따로 작성하지 않아도 작성해주므로 생산성 향상</li>
<li>SQL을 수정할 필요가 없어 설정 및 필드 변경 시 자동 수정돼 유지보수가 향상된다.</li>
<li>DB의 종류에 따라 SQL문에 따른 차이가 있긴 하지만(위에서 말한 방언) JPA가 해당 DB에 맞는 SQL 작성이 가능하다.</li>
<li>캐시를 활용한 성능 최적화로 인해 트랜잭션을 처리하는 시간이 많이 단축된다.</li>
</ol>
<hr />
<h3 id="단점">단점</h3>
<ol>
<li>너무 복잡한 SQL을 작성하기에는 적합하지 않다.</li>
<li>JPA를 제대로 이해하지 못하고 작성시 성능저하가 발생할 수 있다.</li>
<li>객체지향 패러다임과 관계형 데이터베이스 패러다임에 대한 이해가 없는 상태로는 제대로 이해할 수 없다.</li>
<li>복잡한 동적 SQL같은 경우 순수 JPA만으로는 부족한 부분에 있어 추가 라이브러리(ex. QueryDSL, JooQ) 를 활용해야 할 경우가 생길 수 있다.</li>
</ol>
<hr />
<h2 id="중요-mybatis와-jpa">(중요) Mybatis와 JPA</h2>
<ul>
<li><strong>Mybatis</strong> : <strong>SQL Mapper</strong>로 SQL Mapping을 사용하는 영속성(DB저장) 프레임워크이다. <ul>
<li>개발자가 직접 SQL코드를 작성하고 객체에 대해 매핑을 위한 설정을 모두 직접 처리해야 한다. </li>
<li>또한, 수정이 이루어질 시 SQL뿐 아니라 매핑 될 객체까지 같이 수정해야 하는 번거로움이 있다.</li>
</ul>
</li>
</ul>
<p><strong>JPA와 마이바티스는 분류상 서로 다르다.</strong></p>
<ul>
<li><p>JPA는 ORM 기술이고, Mybatis는 SQL Mapper의 한 종류이다.</p>
</li>
<li><p>어플리케이션이 고도화 되면 JPA를 구현하여 손이 많이 가는 것 보다 Mybatis가 답이 될 수도 있다.</p>
<ul>
<li>(JPA가 무조건 좋은 것이 아니다. 다만 JPA는 추가 라이브러리를 활용하면 복잡한 SQL이나 동적 SQL에 있어서 도움을 받을 수 있다.)</li>
</ul>
</li>
</ul>
<hr />
<h2 id="jpa의-원리">JPA의 원리</h2>
<h3 id="동작방식">동작방식</h3>
<p>Java 애플리케이션과 JDBC 사이에서 동작하고, 내부적으로 JDBC API를 활용한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1657ad1e-ed54-459a-94f7-93b587ca84d2/image.png" /></p>
<p>JPA는 엔티티를 저장하는 환경인 <strong>영속성 컨텍스트(Persistence Context)</strong> 를 통해 엔티티를 보관하고 관리한다.</p>
<hr />
<p>위에서 말한 영속성 컨텍스트가 엔티티를 관리하는 원리에 대해 알아보자.</p>
<h3 id="영속성-컨텍스트가-엔티티를-관리하는-원리">영속성 컨텍스트가 엔티티를 관리하는 원리</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d5ab31c2-e5ce-4949-bfb5-8277adc2617a/image.png" /></p>
<h3 id="엔티티의-영속성-컨텍스트persistence-context에서의-생명주기">엔티티의 영속성 컨텍스트(Persistence Context)에서의 생명주기</h3>
<p>엔티티는 DB와 1:1 매칭이 되는 것들이다.
객체가 생성이 되면 그것의 PK값을 가지고 1차 캐시에 저장이 된다.</p>
<ul>
<li><p>1차 캐시</p>
<ul>
<li>영속성 컨텍스트 내부에 Map으로 관리되는 캐시(<code>key</code>는 <code>@Id</code>이며 매핑한 식별자이고 <code>value</code>은 엔티티 인스턴스이다.)이며 이 곳에 있는 엔티티는 캐시에서 바로 불러와서 조회 성능이 올라간다.</li>
</ul>
</li>
<li><p>동일성 보장</p>
<ul>
<li>반복해서 호출 시 1차 캐시에서 같은 엔티티 인스턴스를 가져올 수 있다.</li>
</ul>
</li>
</ul>
<p>그림과 같이 예시를 보자.
ex)
최초로 PK값과 <code>menu1</code>이 객체가 들어가고, 그것의 스냅샷(복제본)이 하나 저장된다.</p>
<ol>
<li><p>만약 <code>menu1</code>에서 변형이 생긴 <code>menu1'</code> 가 들어온다고 했을 때</p>
</li>
<li><p>Update 쿼리 생성</p>
</li>
<li><p>flush : DB에 있는 값과 비교해서 변경이 생긴게 맞으면 동기화해준다.</p>
</li>
</ol>
<ul>
<li><p><strong>트랜잭션을 지원하는 쓰기 지연(transactional write-behind)</strong></p>
<ul>
<li><p>(엔티티 등록(INSERT)을 예로 들면) 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 저장(flush) 대신 쓰기 지연 SQL 저장소에 INSERT SQL을 차곡차곡 쌓게 되며 커밋 시에 쿼리를 데이터베이스로 보내는데 이를 트랜잭션을 지원하는 쓰기 지연이라고 한다.</p>
</li>
<li><p>(DELETE를 예로 들면) select, update 3번, delete 쿼리가 있다고 했을 때 저장소에 delete만 유효하다면 이전 4개의 쿼리는 DB에 보내지 않고 delete 쿼리만 보내는데, 이를 쓰기 지연이라고 한다.</p>
</li>
<li><p><strong>플러시(flush)</strong>: flush()는 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.</p>
</li>
<li><p><strong>플러시 절차</strong></p>
<ol>
<li>영속성 컨텍스트에 보관할 때 최초 엔티티 상태를 복사해서 스냅샷으로 저장해 두고 모든 엔티티를 스냅샷과 비교해서 수정된 엔티티를 찾아서 수정 쿼리를 만들어 쓰기 지연 SQL 저장소에 보낸다.</li>
<li>쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 저장한다.</li>
<li>플러시를 하는 경우: a. em.flush()를 직접 호출한다.</li>
<li><strong>트랜잭션 커밋 시 플러시가 자동 호출한다.</strong></li>
<li><strong>JPQL 쿼리 실행 시 플러시가 자동 호출한다.</strong></li>
</ol>
</li>
</ul>
</li>
</ul>
<blockquote>
<p>4, 5번을 보면 플러시가 자동 호출되는데, 변경이 없다면 위 예시처럼 Update 쿼리는 실행되지 않는다.</p>
</blockquote>
<ul>
<li><strong>변경 감지(dirty checking)</strong><ul>
<li>SQL에 의존적이지 않도록 엔티티의 데이터 변경을 감지하고 데이터베이스에 자동으로 반영하는 기능을 변경 감지라고 한다.</li>
<li>영속성 컨텍스트에 보관할 때 최초 엔티티 상태를 복사해서 저장한 스냅샷과 이를 비교하여 감지한다.</li>
<li>영속 상태의 엔티티에만 적용된다.(준영속이나 비영속은 해당되지 않는다.)</li>
<li><strong>변경 감지는 절차(커밋 실행 시)</strong><ol>
<li>우선 엔티티 매니저 내부에서 먼저 플러시(flush)가 호출된다.</li>
<li>엔티티와 스냅샷을 비교해서 변경된 엔티티를 찾는다.</li>
<li>변경된 엔티티와 관련된 수정 쿼리를 생성해서 쓰기 지연 SQL 저장소에 보낸다.</li>
<li>쓰기 지연 저장소의 SQL을 데이터베이스로 보낸다.</li>
<li>데이터베이스에서 트랜잭션을 커밋한다.</li>
</ol>
</li>
</ul>
</li>
</ul>
<hr />
<h3 id="엔티티의-영속성-컨텍스트persistence-context에서의-생명주기-1">엔티티의 영속성 컨텍스트(Persistence Context)에서의 생명주기</h3>
<p>위에서 다뤘던 예시를 이용해서 같이 알아보자.</p>
<p>일단 find() 먼저 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/84ccf4a6-4535-4372-9b09-6196c5d4074a/image.png" /></p>
<ol>
<li>왼쪽 위 <code>persist()</code>
<code>EntityManager</code> (em)에게 persistence context 안에 1번 메뉴를 찾아달라고 요청을 한다.</li>
</ol>
<blockquote>
<ul>
<li>엔티티 메니저는 엔티티를 저장하는 공간으로 엔티티를 보관하고 관리한다.</li>
<li>엔티티 매니저가 생성될 때 하나의 영속성 컨텍스트가 만들어 진다.</li>
</ul>
</blockquote>
<ol start="2">
<li><p>persistence context 속 1차 캐시라는 공간에 1번 메뉴가 없다면 DB를 다녀온다.</p>
</li>
<li><p>DB에서도 찾은 뒤, persistence Context에 <code>id : 1, menu 객체, menu 스냅샷</code> 을 저장해둔 뒤 menu 객체를 요청한 우리에게 준다.</p>
<ul>
<li>DB에도 없다면 : NULL 값이 반환된다.</li>
</ul>
</li>
</ol>
<blockquote>
<p>우리는 EntityManager와 계속 대화를 할 것이다.
즉, Persistence Context 는 Java 쪽으로 DB를 약간 땡겨온 느낌이라고 생각하면 된다.</p>
</blockquote>
<ul>
<li><p>영속성 컨텍스트에 담긴 menu1과 같은 엔티티 -&gt; <strong>영속성 엔티티</strong> 라고 한다.</p>
</li>
<li><p>아직 담기지 않은 엔티티 -&gt; <strong>비영속 엔티티</strong></p>
</li>
<li><p>영속성 컨텍스트에 저장됐다가 분리된 상태 -&gt; <strong>준영속 엔티티</strong></p>
<ul>
<li>다시 관리해달라고 한다면 -&gt; merge()를 사용하는 것</li>
</ul>
</li>
</ul>
<p>객체와 DB와 비교하면서 select 를 하거나 update 를 하는 것이다.</p>
<ul>
<li>ex) DB에도 1번 메뉴가 있고, persistence context 에도 있는데 내가 엔티티를 지워버린다면?<ul>
<li>persistence context에서 flush() 를 통해 비교한 뒤 delete 쿼리를 날린다.</li>
</ul>
</li>
</ul>
<hr />
<h3 id="엔티티의-생명주기">엔티티의 생명주기</h3>
<table>
<thead>
<tr>
<th>상태</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>비영속</td>
<td>엔티티가 영속성 컨텍스트와 전혀 관계가 없는 상태. 즉, 아직 담기지 않은 엔티티</td>
</tr>
<tr>
<td>영속</td>
<td>엔티티가 영속성 컨텍스트에 저장된 상태</td>
</tr>
<tr>
<td>준영속</td>
<td>영속성 컨텍스트에 저장됐다가 분리된 상태</td>
</tr>
<tr>
<td>병합</td>
<td>준영속이었던 엔티티가 다시 영속상태로 변경된 상태</td>
</tr>
<tr>
<td>삭제</td>
<td>엔티티가 삭제된 상태</td>
</tr>
</tbody></table>