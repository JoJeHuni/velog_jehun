<h1 id="mybatis">MyBatis</h1>
<p>데이터의 CRUD를 편하게 할 수 있도록 xml로 구조화 한 Mapper 설정 파일을 통해 JDBC를 구현한 영속성 프레임워크다.</p>
<p>기존의 JDBC를 통해 구현했던 상당량의 코드, Parameter 설정 및 결과 mapping을 xml 설정으로 쉽게 구현할 수 있게 한다.</p>
<h2 id="흐름-및-동작-구조">흐름 및 동작 구조</h2>
<h3 id="흐름">흐름</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/78bc1bdb-7897-4a8f-bdde-566e3776f0bb/image.png" /></p>
<p>JDBC 템플릿을 통해 SQL을 실행하면 MyBatis는 같은 흐름을 전용 라이브러리로 대체해 동작한다.</p>
<hr />
<h3 id="동작-구조">동작 구조</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/115e507a-7003-4dd8-a648-5a453b87aa35/image.png" /></p>
<p><a href="https://mybatis.org/mybatis-3/ko/index.html">MyBatis Introduction 바로 가기</a></p>
<p>한국어 또한 지원해준다.</p>
<hr />
<h2 id="ibatis---mybatis">iBatis -&gt; MyBatis</h2>
<ul>
<li>기존에 Apache project에서 iBatis를 운영하던 팀이 2010년 5월 9일에 Google 팀으로 이동하면서 MyBatis로 이름을 바꾸었다.</li>
<li>MyBatis는 기존 iBatis의 한계점인 Dynamic query(동적 쿼리)와 Annotation 처리를 보강하여 더 나은 기능을 제공한다.</li>
<li>iBatis는 현재 비활성화 상태이며, 기존에 iBatis로 만들어진 애플리케이션의 지원을 위한 라이브러리만 제공하고 있다.</li>
</ul>
<hr />
<h2 id="차이점">차이점</h2>
<ol>
<li><p>Java 요구 버전</p>
<ul>
<li>iBatis는 JDK 1.4 이상, MyBatis에서는 JDK 1.5 이상 사용 가능하다.</li>
</ul>
</li>
<li><p>패키지 구조 변경</p>
<ul>
<li>iBatis : com.ibatis.*</li>
<li>MyBatis : org.apache.ibatis.*</li>
</ul>
</li>
<li><p>사용 용어의 변경</p>
<table>
<thead>
<tr>
<th>iBatis</th>
<th>MyBatis</th>
</tr>
</thead>
<tbody><tr>
<td>SqlMapConfig</td>
<td>Configuration</td>
</tr>
<tr>
<td>sqlMap</td>
<td>Mapper</td>
</tr>
<tr>
<td>resultClass</td>
<td>resultType</td>
</tr>
</tbody></table>
</li>
<li><p>동적 쿼리 지원</p>
<ul>
<li>Mybatis는 if, choose, trim, foreach 문을 지원한다.</li>
</ul>
</li>
<li><p>자바 Annotation 지원</p>
</li>
</ol>
<hr />
<h2 id="세팅">세팅</h2>
<p>dependency 에 추가한다.</p>
<p><strong>Build.gradle</strong></p>
<pre><code class="language-gradle">    // https://mvnrepository.com/artifact/org.mybatis/mybatis
    implementation 'org.mybatis:mybatis:3.5.6'
    // https://mvnrepository.com/artifact/mysql/mysql-connector-java
    implementation 'mysql:mysql-connector-java:8.0.28'</code></pre>