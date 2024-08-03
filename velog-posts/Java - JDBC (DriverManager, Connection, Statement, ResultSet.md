<h1 id="jdbc">JDBC</h1>
<p>JDBC에 대해 알아보자.</p>
<blockquote>
<p>JDBC : Java에서 DataBase에 접근 가능하도록 하는 Programming API 이다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/07315133-7f84-4474-9847-cd5c5da5390f/image.png" /></p>
<p>Java.sql 패키지에서 JDBC에서 다룰 수 있다. </p>
<ul>
<li>ava.sql 패키지 docs: <a href="https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html">java.sql (Java Platform SE 8 ) (oracle.com)</a></li>
</ul>
<p>이전에 부트캠프 db 공부를 진행하면서 employeedb로 활용도 했었는데 그걸 사용해서 JDBC 활용도 해보려고 한다.</p>
<hr />
<h2 id="drivermanager">DriverManager</h2>
<p>DataBase에 JDBC driver를 통하여 Connection을 만드는 역할</p>
<ul>
<li>Class.forName() method를 통해 생성되어 메모리에 할당된다.<ul>
<li><code>Class.forName()</code>에서 <strong>Class</strong> 클래스는 클래스의 메타 정보 가지고 있다.</li>
<li><code>forName()</code> 메소드에 풀클래스명을 작성하면 해당 클래스를 메모리에 올려 사용할 준비를 하도록 하는 것으로, 동적 로딩을 이용하여 DB driver를 로딩한다.</li>
</ul>
</li>
</ul>
<hr />
<h3 id="활용-예시">활용 예시</h3>
<pre><code class="language-java">Connection con = null;

try {
    Class.forName(&quot;com.mysql.cj.jdbc.Driver&quot;);
    con = DriverManager.getConnection(&quot;jdbc:mysql://localhost:3306&quot;, &quot;{user}&quot;, &quot;{password}&quot;);
} catch (ClassNotFoundException e) {
    throw new RuntimeException(e);
} catch (SQLException e) {
    throw new RuntimeException(e);
}</code></pre>
<p>위의 예시를 보면 알 수 있듯이 몇 가지 주의사항이 있다.</p>
<blockquote>
<ol>
<li>반드시 예외처리를 해야 한다.</li>
<li>직접 instance 생성이 불가하고, DriverManager 클래스의 getConnection() 메소드를 사용하여 Connection instance를 생성할 수 있다.</li>
</ol>
</blockquote>
<ul>
<li><code>getConnection()</code> 메소드의 매개변수 속성은 다음과 같다.<ul>
<li><code>url</code> : 연결할 Database의 주소</li>
<li><code>user</code> : Database에 접속할 user 이름</li>
<li><code>password</code> : Database 접속을 위한 비밀번호</li>
</ul>
</li>
</ul>
<p>위 조건들 때문에 DB를 만들면서, root 계정을 통해 user 와 password 에게 권한을 줘야 한다.</p>
<p>계속 저런 try-catch문을 작성해줄 수 없기 때문에 <code>properties</code> 파일을 만들어보았다.</p>
<pre><code>driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/employeedb
user={user}
password={password}</code></pre><hr />
<p>이제 Connection 객체에 properties 파일을 읽게 해서 정보를 줄 수 있다.</p>
<pre><code class="language-java">Properties prop = new Properties();
Connection con = null;

try {
    prop.load(
            new FileReader(&quot;src/main/java/com/ohgiraffers/config/connection-info.properties&quot;));
    String driver = prop.getProperty(&quot;driver&quot;);
    String url = prop.getProperty(&quot;url&quot;);
    String user = prop.getProperty(&quot;user&quot;);
    String password = prop.getProperty(&quot;password&quot;);

    Class.forName(driver);
    con = DriverManager.getConnection(url, user, password);

} catch (Exception e) {
    throw new RuntimeException(e);
}</code></pre>
<hr />
<h2 id="connection">Connection</h2>
<p>DriverManager에도 나왔듯이 Connection의 역할도 알아보자.</p>
<ul>
<li>쿼리문을 실행할 수 있는 <code>Statement</code> 혹은 <code>PreparedStatement</code> 객체를 생성할 수 있는 기능을 제공한다.<ul>
<li>Connection instance를 생성하고 <code>createStatement()</code> 메소드를 호출하여 Statement instance를 생성할 수 있다.</li>
<li>따라서 SQL문 실행 전 우선 Connection instance가 있어야 한다.</li>
</ul>
</li>
</ul>
<p>Connection 객체를 생성하는건 위의 DriverManager 클래스에서 활용해보았다.</p>
<blockquote>
<p>Connection 객체를 생성하는 코드 또한 중복 작성하게 되므로, Template 클래스의 static 메소드로 Connection 객체를 생성하거나 반납하는 코드를 작성하여 공통으로 사용하도록 하는 것이 일반적이다.</p>
</blockquote>
<p>그러므로 Template 코드를 작성해보자.</p>
<pre><code class="language-java">import java.io.FileReader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JDBCTemplate {
    public static Connection getConnection() {
        Properties prop = new Properties();
        Connection con = null;

        try {
            prop.load(
                    new FileReader(
                            &quot;src/main/java/com/ohgiraffers/section01/connection/jdbc-config.properties&quot;));
            String driver = prop.getProperty(&quot;driver&quot;);
            String url = prop.getProperty(&quot;url&quot;);
            String user = prop.getProperty(&quot;user&quot;);
            String password = prop.getProperty(&quot;password&quot;);

            Class.forName(driver);
            con = DriverManager.getConnection(url, user, password);

        } catch (Exception e) {
            throw new RuntimeException(e);
        }

        return con;
    }

    public static void close(Connection con) {
        try {
            if(con != null &amp;&amp; !con.isClosed()) con.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}</code></pre>
<hr />
<h2 id="statement">Statement</h2>
<p>간단하게 말해서 <strong>SQL문을 저장한 뒤, 실행해서 결과를 반환해주는 메소드들이 모인 클래스이다.</strong></p>
<ol>
<li>Connection class의 <code>createStatement()</code> 메소드를 호출하여 Statement instance를 생성한다.</li>
<li>생성한 instance의  <code>executeQuery()</code> 메소드를 호출하여 SQL문 수행한다. (SQL문을 String 형태로 인자로 전달한다.)</li>
</ol>
<hr />
<h3 id="사용-예시">사용 예시</h3>
<p>활용은 ResultSet 개념을 얘기하고 함께 해보자.</p>
<pre><code class="language-java">try {
    String query = &quot;SELECT ID, LAST_NAME FROM EMP&quot;;
    stmt = conn.createStatement(); // conn = Connection 객체이다.
    rset = stmt.executeQuery(query);
} catch (SQLException e) {
    e.printStackTrace();
}</code></pre>
<p>Connection에서의 close 처럼 자원 반납을 해줘야하는데 ResultSet 때 같이 알아보자.</p>
<blockquote>
<p>PreparedStatement 는 추후에 알아보고 일단 ResultSet 부터</p>
</blockquote>
<hr />
<h2 id="resultset">ResultSet</h2>
<ul>
<li>SELECT문 수행 성공 시 반환한 결과값을 받아오는 객체이다.</li>
<li>SQL문에 의해 생성된 결과 테이블을 담고 있다.</li>
<li>커서(cursor)로 특정 행에 대한 참조 조작을 할 수 있다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a3d93b45-3fcd-4b3a-8b41-4c37b2ef3e96/image.png" /></p>
<ul>
<li><code>getString()</code><ul>
<li>ResultSet의 현재 커서 위치에 존재하는 로우에서 인자로 전달한 컬럼의 결과 값을 가지고 온다.</li>
<li>데이터 자료형에 따라 <strong>get<em>자료형</em>(”<em>컬럼명</em>”)</strong> 형식으로 사용한다.</li>
</ul>
</li>
<li><code>next()</code><ul>
<li><code>ResultSet</code>의 커서 위치를 하나 내리면서 다음 행이 존재하면 <code>true</code> / 존재하지 않으면 <code>false</code>를 반환한다.</li>
<li><code>while</code>문을 활용하여 쿼리 수행 결과의 마지막 행까지 반복 수행하여 결과를 한 행씩 가지고 올 때 활용한다.</li>
<li>단, 수행 결과 반환될 컬럼이 1건임이 확실하면 <code>while</code> 블럭 대신 if문에 조건으로 사용할 수 있다.</li>
</ul>
</li>
</ul>
<blockquote>
<p>Connection과 Statement도 close()로 자원 반납하듯, ResultSet도 close()를 통해 자원을 반납해야 한다.</p>
</blockquote>
<hr />
<h3 id="사용-예시-1">사용 예시</h3>
<pre><code class="language-java">while(rset.next()) {
    System.out.println(rset.getString(&quot;EMP_ID&quot;) + &quot;, &quot; + rset.getString(&quot;EMP_NAME&quot;));
}</code></pre>
<hr />
<h2 id="활용">활용</h2>
<p><code>Statement</code>, <code>ResultSet</code>도 함께 활용해보자.</p>
<blockquote>
<p>properties 에는 필요한 driver, url, user, password 에 적어둔 상황이다.</p>
</blockquote>
<hr />
<p>properties -&gt; <code>connection-info.properties</code></p>
<pre><code>driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/employeedb
user={user}
password={password}</code></pre><hr />
<p><strong>자원 반납까지 포함된 JDBCTemplate</strong></p>
<pre><code class="language-java">import java.io.FileReader;
import java.sql.*;
import java.util.Properties;

public class JDBCTemplate {
    public static Connection getConnection() {
        Properties prop = new Properties();
        Connection con = null;

        try {
            prop.load(
                    new FileReader(
                            &quot;src/main/java/com/ohgiraffers/config/connection-info.properties&quot;));
            String driver = prop.getProperty(&quot;driver&quot;);
            String url = prop.getProperty(&quot;url&quot;);
            String user = prop.getProperty(&quot;user&quot;);
            String password = prop.getProperty(&quot;password&quot;);

            Class.forName(driver);
            con = DriverManager.getConnection(url, user, password);

        } catch (Exception e) {
            throw new RuntimeException(e);
        }

        return con;
    }

    public static void close(Connection con) {
        try {
            if(con != null &amp;&amp; !con.isClosed()) con.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public static void close(ResultSet resultSet) {
        try {
            if(resultSet != null &amp;&amp; !resultSet.isClosed()) resultSet.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public static void close(Statement statement) {
        try {
            if(statement != null &amp;&amp; !statement.isClosed()) statement.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}</code></pre>
<hr />
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import static com.ohgiraffers.common.JDBCTemplate.close;
import static com.ohgiraffers.common.JDBCTemplate.getConnection;

public class Application {
    public static void main(String[] args) {
        // Connection을 통해 트랜잭션 처리
        Connection con = getConnection();

        System.out.println(&quot;con = &quot; + con);

        Statement statement = null;     //쿼리를 운반하고 결과를 반환하는 역할
        ResultSet resultSet = null;     // 그 결과들을 모아서 조회하는 역할 (DML 작업이면 ResultSet 대신 int로 처리하면 된다.)

        try {
            statement = con.createStatement();
            resultSet = statement.executeQuery(&quot;SELECT  * FROM EMPLOYEE&quot;); // 해당 쿼리에 맞는 결과를 statement가 반환하고, resultset은 조회하게 해준다.

            while(resultSet.next()) {
                /* 설명. while문 안의 resultSet은 한 행을 의미한다. */
                System.out.print(resultSet.getString(&quot;EMP_NAME&quot;) + &quot; &quot;);
                System.out.print(resultSet.getInt(&quot;SALARY&quot;) + &quot;\n&quot;);
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            /* 설명. 생성과 달리 역순으로 각 스트림을 닫는다.*/
            close(resultSet);
            close(statement);
            close(con);
        }
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b4c76df8-a704-4447-8c05-a67d87c2b6cd/image.png" /></p>
<p>이제 직접 쿼리로 보는 것이 아닌 SQL을 받아서 확인 가능하다.</p>
<p>그래도 난 sql 쿼리로 확인해본 뒤 사용할 것 같다..!</p>
<hr />
<h2 id="preparedstatement">PreparedStatement</h2>
<ul>
<li>PreparedStatement도 Statement이다. 따라서 Template 클래스를 작성하여 Statement를 close() 하는 메소드를 함께 사용할 수 있다.</li>
<li>완성된 쿼리문과 미완성된 쿼리문(= 위치홀더를 사용한 쿼리문)을 모두 사용할 수 있다.</li>
<li>PreparedStatement는 위치홀더(placeholder) 개념에 해당되는 인수가 많아 특정 값만 바꾸어 여러 번 실행하는 상황에 유용하다.</li>
</ul>
<hr />
<h3 id="장점">장점</h3>
<ol>
<li><strong>Statement 보다 빠르다.</strong></li>
</ol>
<p><strong>cache</strong>에 <strong>조건값은 <code>?</code></strong> 로 두고 (아무렇게나 가능하게끔) 쿼리는 <code>key : SQL문</code>, <code>value : 컴파일된 SQL문</code> 으로 담아둔 뒤 <strong>바인딩 되는 변수만 바꿔서 조회하기 때문에 빠르다.</strong></p>
<ol start="2">
<li><strong>SQL injection 공격에 대해 안전하다.</strong></li>
</ol>
<ul>
<li>Statement 사용 시 전달하는 조건 변수에 <strong>OR 1=1</strong> 조건을 작성하면 시스템이 제공하는 의도와 다르게 데이터가 조회될 수 있다.</li>
<li>예를 들어 아래처럼 조건을 주게 되면, empName이 홍길동인 사람이 아니라 empID가 200인 사람의 정보를 조회하게 된다.</li>
</ul>
<pre><code class="language-java">empName = &quot;홍길동' OR 1=1 AND EMP_ID = '200&quot;;</code></pre>
<blockquote>
<p>PreparedStatement와 위치홀더를 사용하면, 조건으로 세팅되는 값을 메소드의 지정 형식에 따라 따옴표로 감싸는 등의 처리를 알아서 한다.</p>
</blockquote>
<hr />
<h3 id="객체-생성-및-사용">객체 생성 및 사용</h3>
<ol>
<li><code>Connection</code> class의 <code>preparedStatement()</code> 메소드를 사용하여 instance 생성한다.</li>
<li>SQL문을 위치홀더(placeholder) 「?」로 표현하는 String으로 정의한다.</li>
<li><code>PreparedStatement</code>의 <code>setString()</code> 메소드로 위치홀더의 순서와 넣어 줄 변수 값을 세팅한다.</li>
<li>생성하고 위치홀더 값을 세팅한 instance의 <code>executeQuery()</code> 메소드를 호출하여 SQL문 수행한다.</li>
</ol>
<pre><code class="language-java">try {
    String query = &quot;INSERT INTO MEMBER VALUES(?, ?)&quot;;
    pstmt = conn.preparedStatement(query);
    pstmt.setString(1, id);
    pstmt.setString(2, password);
    rset = pstmt.executeQuery();
} catch (SQLException e) {
    e.printStackTrace();
}</code></pre>
<p>사용하면 SQL문을 xml 파일로 분리해 관리가 가능하다.</p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!DOCTYPE properties SYSTEM &quot;http://java.sun.com/dtd/properties.dtd&quot;&gt;
&lt;properties&gt;
    &lt;comment/&gt;
    &lt;entry key=&quot;selectEmpByFamilyName&quot;&gt;
        SELECT
        E.*
        FROM EMPLOYEE E
        WHERE E.EMP_NAME LIKE CONCAT(?, '%')
    &lt;/entry&gt;
&lt;/properties&gt;</code></pre>
<hr />
<h4 id="xml-파일의-쿼리를-읽어와-사용하는-코드-작성-예시">XML 파일의 쿼리를 읽어와 사용하는 코드 작성 예시</h4>
<pre><code class="language-java">try {
    prop.loadFromXML(new FileInputStream(&quot;employee-query.xml&quot;));
    String query = prop.getProperty(&quot;selectEmpByFamilyName&quot;);

    pstmt = con.prepareStatement(query);
    pstmt.setString(1, empName);

    rset = pstmt.executeQuery();
} </code></pre>