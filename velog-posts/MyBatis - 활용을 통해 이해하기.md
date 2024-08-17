<h1 id="mybatis">MyBatis</h1>
<p><a href="https://velog.io/@jojehuni_9759/MyBatis-%EA%B0%9C%EC%9A%94">MyBatis 개요</a></p>
<p>MyBatis 개요 이후로 활용을 해보려고 한다.</p>
<p>TransactionFactory, DataSource 와 같은 것들도 사용해보자.</p>
<h2 id="mapper-인터페이스로-활용하는-경우">Mapper 인터페이스로 활용하는 경우</h2>
<p>쿼리를 꺼내는 작업으로 사용하기 위해 활용할 <strong>Mapper 인터페이스</strong></p>
<pre><code class="language-java">import org.apache.ibatis.annotations.Select;

import java.util.Date;

public interface Mapper {

    @Select(&quot;SELECT NOW()&quot;)
    Date selectNow();
}</code></pre>
<hr />
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    private static String driver = &quot;com.mysql.cj.jdbc.Driver&quot;;
    private static String url = &quot;jdbc:mysql://localhost:3306/menudb&quot;;
    private static String user = &quot;swcamp&quot;;
    private static String password = &quot;swcamp&quot;;

    public static void main(String[] args) {
        Environment environment = new Environment(&quot;dev&quot;, new JdbcTransactionFactory(), new PooledDataSource(driver, url, user, password));

        // configuration은 설계도
        Configuration configuration = new Configuration(environment);
        configuration.addMapper(Mapper.class);

        // 설계도를 줄테니 공장을 지어달라는 의미로 사용
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);

        // 이제 수동 커밋을 해야 한다. false를 해야지 수동 커밋이 된다.
        SqlSession sqlSession = sqlSessionFactory.openSession(false);

        // 쿼리를 꺼내는 작업
        Mapper mapper = sqlSession.getMapper(Mapper.class);
        Date date = mapper.selectNow();

        System.out.println(&quot;date = &quot; + date);

        sqlSession.close();
    }
}</code></pre>
<p>이전부터 사용했던 menudb를 그대로 사용해보았고</p>
<p><code>Mapper</code> 인터페이스와 <code>Application</code>은 같은 패키지에 넣어뒀다.
같은 이름이 있긴 한데 import 할 필요 없다.</p>
<blockquote>
<ul>
<li><code>JdbcTransactionFactory</code> : 수동 커밋</li>
</ul>
</blockquote>
<ul>
<li><code>ManagedTransactionFactory</code> : 자동 커밋<blockquote>
</blockquote>
</li>
</ul>
<hr />
<ul>
<li><code>PooledDataSource</code> : ConnectionPool 사용</li>
<li><code>UnPooledDataSource</code> : ConnectionPool 미사용</li>
</ul>
<p><strong>출력결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e1bdf749-83bd-4d60-98a0-ac92b78ca5bf/image.png" /></p>
<hr />
<h2 id="xml파일-사용하는-경우">xml파일 사용하는 경우</h2>
<p>설정 정보를 작성하기 위해서 <code>mybatis-config.xml</code> 파일을 생성할 것이다.</p>
<h3 id="설정-내용-정의">설정 내용 정의</h3>
<ol>
<li>Mybatis 공식 사이트를 가면 xml 파일로 했을 때 설정 내용을 Mybatis 설정이라고 선언해줘야 하는데, 아래 코드를 최상단에 적어주면 된다.</li>
</ol>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE configuration
        PUBLIC &quot;-//mybatis.org//DTD Config 3.0//EN&quot;
        &quot;http://mybatis.org/dtd/mybatis-3-config.dtd&quot;&gt;</code></pre>
<ul>
<li><code>DOCTYPE</code>으로 설정하는 내용은 하위에 사용 가능한 태그, 속성들이 정의되어 있는 파일이다.
→ 따라서 해당 파일에 작성하는 태그 혹은 속성 등이 맞게 작성되었는지 확인한다.</li>
</ul>
<ol start="2">
<li>Mybatis 설정이라는 것을 작성했기 때문에 설정 내용 아래 최상위 태그인 <code>&lt;configuration&gt;</code>을 작성해주면 된다.<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;!DOCTYPE configuration PUBLIC &quot;-//mybatis.org//DTD Config 3.0//EN&quot; &quot;http://mybatis.org/dtd/mybatis-3-config.dtd&quot;&gt;
&lt;configuration&gt;
. . .
&lt;/configuration&gt;</code></pre>
</li>
</ol>
<hr />
<h3 id="environment">Environment</h3>
<ol>
<li><p>MyBatis에서 연동할 DB 정보를 등록하는 것으로, <code>&lt;environments&gt;</code> 태그는 여러 개의 <code>&lt;environment&gt;</code> 태그를 포함할 수 있으며, <code>default</code> 속성으로 기본 사용할 환경 설정을 지정할 수 있다.</p>
</li>
<li><p><code>&lt;environment&gt;</code> 내부에는 <code>&lt;transactionManager&gt;</code>, <code>&lt;dataSource&gt;</code>, <code>&lt;property&gt;</code> 와 같이 여러 가지 태그가 들어갈 수 있다.</p>
</li>
</ol>
<ul>
<li><p><code>&lt;transactionManager&gt;</code> : 트랜잭션 매니저를 JDBC 혹은 MANAGED로 설정할 수 있다.</p>
<ul>
<li><strong>JDBC</strong> : MyBatis API에서 제공하는 commit, rollback 메소드 등을 사용해서 트랜젝션을 관리하는 방식 (수동 commit)</li>
<li><strong>MANAGED</strong> : MyBatis API 보다는 컨테이너가 직접 트랜젝션을 관리하는 방식 (자동 commit)</li>
</ul>
</li>
<li><p><code>&lt;dataSource&gt;</code> : 속성으로 커넥션풀 사용 여부를 <code>POOLED</code>와 <code>UNPOOLED</code>로 설정할 수 있다.</p>
<ul>
<li><table>
<thead>
<tr>
<th>구분</th>
<th>POOLED</th>
<th>UNPOOLED</th>
</tr>
</thead>
<tbody><tr>
<td>특징</td>
<td>최초 Connection 객체 생성 시 해당 정보를 pool 영역에 저장해 두고 이후 Connection 객체를 생성할 때 이를 재사용한다.</td>
<td>Connection 객체를 별도로 저장하지 않고, 호출 시마다 매번 생성하여 사용한다.</td>
</tr>
<tr>
<td>장점</td>
<td>Connection 객체를 생성하여 데이터베이스와 연결을 구축하는데 걸리는 시간이 단축된다.</td>
<td>Connection 연결이 많지 않은 코드를 작성할 때 간단하게 구현할 수 있다.</td>
</tr>
<tr>
<td>단점</td>
<td>단순한 로직을 수행하는 객체를 만들기에는 설정해야 할 정보가 많다.</td>
<td>매번 새로운 Connection 객체를 생성하므로 속도가 상대적으로 느리다.</td>
</tr>
</tbody></table>
</li>
</ul>
</li>
</ul>
<hr />
<h3 id="properties">properties</h3>
<ol>
<li>설정 파일에서 공통적인 속성을 정의하거나 외부 파일에서 값을 가져오는 태그이다.</li>
<li>외부 프로퍼티 파일은 resource 하위의 경로를 기술하면 된다.<ul>
<li>클래스패스 경로가 아닌 경우 url속성에 &quot;file:d:&quot;로 시작하는 경로를 기술하면 된다.</li>
</ul>
</li>
<li><code>&lt;properties&gt;</code> 태그 안쪽에 <code>&lt;property&gt;</code> 태그를 작성하여 properties 파일에 값을 설정할 수 있다.</li>
</ol>
<hr />
<p>위에 작성한 몇 가지 XML Tag 들을 사용해서 예제를 만들 것이다.</p>
<p>이전에 사용했던 menudb 로 할 것이고, MyBatis를 사용하기 위해서는 mapper, config 파일이 필요하다.</p>
<p><code>resources</code> 폴더에 추가</p>
<p><strong>mybatis-config.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE configuration
        PUBLIC &quot;-//mybatis.org//DTD Config 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-config.dtd&quot;&gt;
&lt;configuration&gt;
    &lt;environments default=&quot;dev&quot;&gt;
        &lt;environment id=&quot;dev&quot;&gt;
            &lt;transactionManager type=&quot;JDBC&quot;/&gt;
            &lt;dataSource type=&quot;POOLED&quot;&gt;
                &lt;property name=&quot;driver&quot; value=&quot;com.mysql.cj.jdbc.Driver&quot;/&gt;
                &lt;property name=&quot;url&quot; value=&quot;jdbc:mysql://localhost:3306/menudb&quot;/&gt;
                &lt;property name=&quot;username&quot; value=&quot;swcamp&quot;/&gt;
                &lt;property name=&quot;password&quot; value=&quot;swcamp&quot;/&gt;
            &lt;/dataSource&gt;
        &lt;/environment&gt;
    &lt;/environments&gt;
    &lt;mappers&gt;
        &lt;mapper resource=&quot;mapper.xml&quot;/&gt;
    &lt;/mappers&gt;
&lt;/configuration&gt;</code></pre>
<hr />
<p><strong>mapper.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;mapper&quot;&gt;
    &lt;select id=&quot;selectNow&quot; resultType=&quot;java.util.Date&quot;&gt;
        SELECT NOW()
    &lt;/select&gt;
&lt;/mapper&gt;</code></pre>
<hr />
<p><strong>Application 클래스</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        String resource = &quot;mybatis-config.xml&quot;;

        try {
            InputStream inputStream = Resources.getResourceAsStream(resource);

            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

            // 커넥션 객체를 session이라고 할 뿐이다. + 수동 커밋
            SqlSession session = sqlSessionFactory.openSession(false);

            // 여기서 꼭 소속과 id가 필요하다. 그래서 mapper.xml에 네임스페이스를 적어준 것이다. 소속이 필요해서.
            Date date = session.selectOne(&quot;mapper.selectNow&quot;);
            System.out.println(&quot;date = &quot; + date);

            session.close();

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}</code></pre>
<p>주석에도 적혀있지만 mapper.xml에 <code>&lt;select&gt;</code> 부분을 보면 name도 같이 적혀 있다.
소속과 id가 있어야 <code>selectOne()</code>을 사용할 수 있는 것이다.</p>
<p><strong>출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4c4b6133-e88a-4baf-a3db-d31fbb1080ff/image.png" /></p>
<hr />
<h1 id="심화-예제-활용">심화 예제 활용</h1>
<h2 id="메뉴-관리-프로그램-만들기">메뉴 관리 프로그램 만들기</h2>
<p>xml 파일에 DB에 적어둔 것들을 <code>resultMap</code>에 컬럼들을 정의해두고 <code>&lt;select&gt;</code> 를 통해서 원하는 쿼리 처리를 하는 방식으로 사용해보려고 한다.</p>
<p>새로운 프로젝트로 한다.</p>
<p><strong>MenuDTO</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section01.xmlconfig;

public class MenuDTO {
    private int menuCode;
    private String menuName;
    private int menuPrice;
    private int categoryCode;
    private String orderableStatus;

    public MenuDTO() {
    }

    public MenuDTO(int menuCode, String menuNmae, int menuPrice, int categoryCode, String orderableStatus) {
        this.menuCode = menuCode;
        this.menuName = menuNmae;
        this.menuPrice = menuPrice;
        this.categoryCode = categoryCode;
        this.orderableStatus = orderableStatus;
    }

    (getter, setter 코드 길어서 생략)

    @Override
    public String toString() {
        return &quot;MenuDTO{&quot; +
                &quot;menuCode=&quot; + menuCode +
                &quot;, menuName='&quot; + menuName + '\'' +
                &quot;, menuPrice=&quot; + menuPrice +
                &quot;, categoryCode=&quot; + categoryCode +
                &quot;, orderableStatus='&quot; + orderableStatus + '\'' +
                '}';
    }
}</code></pre>
<hr />
<p><strong>mybatis-config.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE configuration
        PUBLIC &quot;-//mybatis.org//DTD Config 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-config.dtd&quot;&gt;
&lt;configuration&gt;
    &lt;environments default=&quot;dev&quot;&gt;
        &lt;environment id=&quot;dev&quot;&gt;
            &lt;transactionManager type=&quot;JDBC&quot;/&gt;
            &lt;dataSource type=&quot;POOLED&quot;&gt;
                &lt;property name=&quot;driver&quot; value=&quot;com.mysql.cj.jdbc.Driver&quot;/&gt;
                &lt;property name=&quot;url&quot; value=&quot;jdbc:mysql://localhost:3306/menudb&quot;/&gt;
                &lt;property name=&quot;username&quot; value=&quot;swcamp&quot;/&gt;
                &lt;property name=&quot;password&quot; value=&quot;swcamp&quot;/&gt;
            &lt;/dataSource&gt;
        &lt;/environment&gt;
    &lt;/environments&gt;
    &lt;mappers&gt;
        &lt;mapper resource=&quot;com/jehun/section01/xmlconfig/menu-mapper.xml&quot;/&gt;
    &lt;/mappers&gt;
&lt;/configuration&gt;</code></pre>
<hr />
<p><strong>menu-mapper.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;MenuMapper&quot;&gt;
    &lt;resultMap id=&quot;menuResultMap&quot; type=&quot;com.jehun.section01.xmlconfig.MenuDTO&quot;&gt;
        &lt;id property=&quot;menuCode&quot; column=&quot;MENU_CODE&quot;/&gt;
        &lt;result property=&quot;menuName&quot; column=&quot;MENU_NAME&quot;/&gt;
        &lt;result property=&quot;menuPrice&quot; column=&quot;MENU_PRICE&quot;/&gt;
        &lt;result property=&quot;categoryCode&quot; column=&quot;CATEGORY_CODE&quot;/&gt;
        &lt;result property=&quot;orderableStatus&quot; column=&quot;ORDERABLE_STATUS&quot;/&gt;
    &lt;/resultMap&gt;
&lt;/mapper&gt;</code></pre>
<hr />
<p>기본 세팅의 Application</p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {

        MenuController menuController = new MenuController();
        Scanner sc = new Scanner(System.in);

        do{
            System.out.println(&quot;===== 메뉴 관리 =====&quot;);
            System.out.println(&quot;1. 메뉴 전체 조회&quot;);
            System.out.println(&quot;2. 메뉴 코드로 메뉴 조회&quot;);
            System.out.println(&quot;3. 신규 메뉴 추가&quot;);
            System.out.println(&quot;4. 메뉴 수정&quot;);
            System.out.println(&quot;5. 메뉴 삭제&quot;);
            System.out.println(&quot;9. 프로그램 종료&quot;);

            System.out.print(&quot;메뉴 관리 번호를 입력하세요 : &quot;);
            int num = sc.nextInt();

            switch(num) {
                case 1: break;
                case 9:
                    System.out.println(&quot;프로그램을 종료하겠습니다.&quot;); return;
                default:
                    System.out.println(&quot;번호를 잘못 입력하셨습니다.&quot;);

            }
        } while (true);
    }
}</code></pre>
<p>이제 case문에 1,2,3,4,5번을 다 추가할 것이다.</p>
<p>필요한 클래스들은 Controller, Service, DAO에 값을 사용하려면 DTO도 필요하기에 위에 작성해뒀다.</p>
<p>그리고 xml을 읽을 Template 클래스, 결과를 반환해줄 PrintResult 클래스가 필요하다.</p>
<p>총 <strong>6개의 클래스</strong>가 필요하다는 뜻이다.</p>
<p>xml을 읽을 Template 클래스</p>
<pre><code class="language-java">public class Template {

    private static SqlSessionFactory sqlSessionFactory;

    public static SqlSession getSqlSession() {
        if (sqlSessionFactory == null) {
            String resource = &quot;com/jehun/section01/xmlconfig/mybatis-config.xml&quot;;

            try {
                InputStream inputStream = Resources.getResourceAsStream(resource);
                sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
        return sqlSessionFactory.openSession(false);
    }
}
</code></pre>
<p>1번부터 한 번 해보자.</p>
<hr />
<h3 id="1번-메뉴-전체-조회">1번. 메뉴 전체 조회</h3>
<p>일단 필요한건 Controller에서 알맞은 메소드를 호출하는 것이다.</p>
<p>그럼 Controller는 Service를 의존성 주입하면서 해당하는 메소드를 또 호출할 것이고,</p>
<p>Service 또한 DAO를 의존성 주입한 뒤 호출할 것이다.</p>
<p>코드를 보자.</p>
<p><strong>Application의 switch 문 case 1에 추가</strong></p>
<pre><code>    기존 코드
            switch(num) {
                case 1:
                    menuController.findAllMenus();
                    break;
                case 9:
    기존 코드</code></pre><hr />
<p><strong>MenuController 코드</strong></p>
<pre><code class="language-java">public class MenuController {
    private final MenuService menuService;
    private final PrintResult printResult;

    public MenuController() {
        menuService = new MenuService();
        printResult = new PrintResult();
    }

    public void findAllMenus() {
        List&lt;MenuDTO&gt; menusList = menuService.findAllMenus();

        if (!menusList.isEmpty()) {
            printResult.printMenus(menusList);
        } else {
            printResult.printErrorMessage(&quot;조회할 메뉴가 없습니다.&quot;);
        }
    }
}</code></pre>
<p>기본 생성자에 Service와 printResult를 의존성 주입한다.</p>
<p><code>printResult</code>는 반환(메시지 출력) 해주는 클래스이다.</p>
<p>이제 2,3,4,5 번이 추가되면서는 <code>findAllMenus()</code> 메소드 아래에 계속 추가될 예정이다.</p>
<hr />
<p><strong>MenuService 코드</strong></p>
<pre><code class="language-java">import static com.jehun.section01.xmlconfig.Template.getSqlSession;

public class MenuService {

    private final MenuDAO menuDAO;

    public MenuService() {
        menuDAO = new MenuDAO();
    }

    public List&lt;MenuDTO&gt; findAllMenus() {
        SqlSession sqlSession = getSqlSession();

        List&lt;MenuDTO&gt; menuList = menuDAO.selectAllMenus(sqlSession);

        return menuList;
    }
}</code></pre>
<p><code>Service</code>는 비즈니스 로직이기도 하고, 해당 xml 파일을 읽어오는 <code>Template</code> 클래스의 <code>getSqlSession()</code>을 불러온다.</p>
<hr />
<p><strong>MenuDAO</strong></p>
<pre><code class="language-java">public class MenuDAO {
    public List&lt;MenuDTO&gt; selectAllMenus(SqlSession sqlSession) {
        return sqlSession.selectList(&quot;MenuMapper.selectAllMenus&quot;);
    }
}</code></pre>
<p><code>sqlSession</code>을 비즈니스 로직으로부터 매개변수로 받아서 <code>MenuMapper.xml</code>의 <code>selectAllMenus</code> 쿼리를 리스트로 받아오는 것이 MenuDAO가 한 일이며</p>
<p>그것을 Service가 받아와서 반환해주면 컨트롤러는 출력을 해준다.</p>
<p>만약 비어있으면 조회할 메뉴가 없다면서 에러 메세지라고 출력해주게 된다.</p>
<hr />
<p><strong>최종 출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/78fe74e0-c971-4035-b427-194989bccb89/image.png" /></p>
<hr />
<h3 id="2번-메뉴코드로-1개-조회">2번. 메뉴코드로 1개 조회</h3>
<p>기존 <code>Application</code> 코드에 2번 case 문을 추가해주자.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">                case 2:
                    System.out.print(&quot;조회하고 싶은 메뉴 코드를 입력하세요 : &quot;);
                    int code = sc.nextInt();
                    menuController.findByMenuCode(code);
                    break;</code></pre>
<p>Controller의 <code>findByMenuCode()</code>에 메뉴코드를 입력받는 메소드를 활용해서 넣어주려고 한다.</p>
<p>뭐 입력받는 부분을 메소드로 따로 빼도 괜찮다.</p>
<p>Controller 도 위의 전체 메뉴 조회와 크게 다르지 않다. 그냥 DTO 자체로 받아오느냐, List로 받느냐의 차이일 뿐이다.</p>
<p><strong>Controller</strong></p>
<pre><code class="language-java">기존 코드에 추가

    public void findByMenuCode(int code) {
        MenuDTO menuDTO = menuService.findByMenuCode(code);

        if (menuDTO != null) {
            printResult.printMenu(menuDTO);
        } else {
            printResult.printErrorMessage(&quot;해당 코드로 조회된 메뉴가 없습니다.&quot;);
        }
    }</code></pre>
<p>Service 의존성 주입한 뒤 원하는 메뉴가 없다면 조회되는게 없다는 식으로 했다.</p>
<p>있다면 <code>printMenu</code>가 실행되게 했다.</p>
<p><strong>printResult</strong> 에 추가</p>
<pre><code class="language-java">    public void printMenu(MenuDTO menuDTO) {
        System.out.println(&quot;해당 코드의 메뉴&quot;);
        System.out.println(menuDTO);
    }</code></pre>
<p><strong>MenuService</strong>에 추가</p>
<pre><code class="language-java">    public MenuDTO findByMenuCode(int menuCode) {
        SqlSession sqlSession = getSqlSession();

        MenuDTO menuDTO = menuDAO.selectMenuByMenuCode(sqlSession, menuCode);

        sqlSession.close();

        return menuDTO;
    }</code></pre>
<p>여기서 사용할 SqlSession은 이제 <code>menu-mapper.xml</code> 속에 <code>selectMenuByMenuCode</code> 쿼리를 추가해서 사용할 것이다.</p>
<p><strong>MenuDAO</strong>에 추가</p>
<pre><code class="language-java">    public MenuDTO selectMenuByMenuCode(SqlSession sqlSession, int menuCode) {
        return sqlSession.selectOne(&quot;MenuMapper.selectMenuByMenuCode&quot;, menuCode);
    }</code></pre>
<p><strong>menu-mapper.xml</strong> 에 추가</p>
<pre><code class="language-xml">    &lt;select id=&quot;selectMenuByMenuCode&quot; resultMap=&quot;menuResultMap&quot; parameterType=&quot;_int&quot;&gt;
        SELECT
               MENU_CODE
             , MENU_NAME
             , MENU_PRICE
             , CATEGORY_CODE
             , ORDERABLE_STATUS
          FROM TBL_MENU
         WHERE MENU_CODE = #{ menuCode }
    &lt;/select&gt;</code></pre>
<p>MyBatis를 사용할 때는 정수와 같은 것들을 입력받아서 사용할 때 <code>parameterType=&quot;_int&quot;</code> 해당 부분과 <code>#{ menuCode }</code> 이런 형태로 작성해야 한다고 한다..</p>
<hr />
<p><strong>최종 출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1ef1626a-1d6e-4ca4-9e34-7f9290f67eb4/image.png" /></p>
<hr />
<h3 id="3번-신규-메뉴-추가하기">3번. 신규 메뉴 추가하기</h3>
<p><strong>Application에 case문 추가</strong></p>
<pre><code class="language-java">                case 3:
                    menuController.registerMenu(inputMenu());</code></pre>
<p><strong>inputMenu 메소드도 Application에 작성</strong></p>
<pre><code class="language-java">    private static Map&lt;String, String&gt; inputMenu() {
        Scanner sc = new Scanner(System.in);

        System.out.print(&quot;신규 메뉴의 이름을 입력해 주세요 : &quot;);
        String menuName = sc.nextLine();
        System.out.print(&quot;신규 메뉴의 가격을 입력해 주세요 : &quot;);
        String menuPrice = sc.nextLine();
        System.out.print(&quot;신규 메뉴의 카테고리 코드를 입력해 주세요 : &quot;);
        String categoryCode = sc.nextLine();

        Map&lt;String, String&gt; parameter = new HashMap&lt;&gt;();
        parameter.put(&quot;menuName&quot;, menuName);
        parameter.put(&quot;menuPrice&quot;, menuPrice);
        parameter.put(&quot;categoryCode&quot;, categoryCode);

        return parameter;
    }</code></pre>
<p>각각 이름과 숫자여서 String, int 형이라 다르게 받는 것보다 map에 key, value로 나눠서 일단 String으로 저장한 뒤 바꿔줄 생각으로 작성했다.</p>
<p><strong>Controller의 registerMenu 메소드</strong></p>
<pre><code class="language-java">기존 코드에 추가

    public void registerMenu(Map&lt;String, String&gt; parameter) {
        // 가공처리는 컨트롤러의 역할 (이름, 코드, 카테고리 코드같은 다양한 자료형의 값들을 파싱해 하나의 타입으로)
        String menuName = parameter.get(&quot;menuName&quot;);
        int menuPrice = Integer.parseInt(parameter.get(&quot;menuPrice&quot;));
        int categoryCode = Integer.parseInt(parameter.get(&quot;categoryCode&quot;));

        MenuDTO menuDTO = new MenuDTO();
        menuDTO.setMenuName(menuName);
        menuDTO.setMenuPrice(menuPrice);
        menuDTO.setCategoryCode(categoryCode);

        if (menuService.registerMenu(menuDTO)) {
            printResult.printSuccessMessage(&quot;register&quot;);
        } else {
            printResult.printErrorMessage(&quot;메뉴 추가 실패..&quot;);
        }</code></pre>
<p>여기서 printResult의 printSuccessMessage 는 각 메시지에 따라 <code>신규 메뉴 추가</code>, <code>메뉴 수정</code>, <code>메뉴 삭제</code> 이 중에서 선택해서 그 때마다 다른 메시지가 나오게 하려고 한다.</p>
<p>일단은 메뉴 추가를 하고 있기 때문에 등록만 작성하고 수정, 삭제는 세팅만 해두려고 한다.</p>
<p><strong>printResult의 printSuccesssMessage</strong></p>
<pre><code class="language-java">    /* DML 작업 메소드 : insert, update, delete 작업 성공 시 해당 성공 메시지 출력용 메소드 */
    public void printSuccessMessage(String message) {
        String successMessage = &quot;&quot;;

        switch (message) {
            case &quot;register&quot; :
                successMessage = &quot;신규 메뉴 등록에 성공하였습니다.&quot;;
                break;
            case &quot;modify&quot; :
                break;
            case &quot;remove&quot; :
                break;
        }

        System.out.println(&quot;successMessage = &quot; + successMessage);
    }</code></pre>
<p>다음은 서비스이다.
서비스의 registerMenu를 보자.</p>
<p><strong>MenuService에 추가</strong></p>
<pre><code class="language-java">    public boolean registerMenu(MenuDTO menuDTO) {
        SqlSession sqlSession = getSqlSession();

        int result = menuDAO.insertMenu(sqlSession, menuDTO);

        if (result &gt; 0) sqlSession.commit();
        else sqlSession.rollback();

        sqlSession.close();

        return result &gt; 0 ? true : false;
    }</code></pre>
<p>여기서 result를 boolean 값을 통해서 하려고 했으나, 기존 MyBatis의 SqlSession 속에 있는 insert 메소드가 int형으로 반환을 해준다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e44ee0ff-ba0e-4495-b69d-c96039e472ea/image.png" /></p>
<p>그래서 int형으로 받아서 크기에 따라 커지면 등록이 된 것이고, 아니면 안 된 것이기 때문에 삼항 연산자를 통해서 나타내봤다.</p>
<p><strong>MenuDAO에도 insertMenu 메소드를 추가</strong>해줬다.</p>
<pre><code class="language-java">    public int insertMenu(SqlSession sqlSession, MenuDTO menuDTO) {
        return sqlSession.insert(&quot;MenuMapper.insertMenu&quot;, menuDTO);
    }</code></pre>
<p><code>menu-mapper.xml</code> 에도 <strong>insert문 추가</strong></p>
<pre><code class="language-xml">    &lt;insert id=&quot;insertMenu&quot; parameterType=&quot;com.jehun.section01.xmlconfig.MenuDTO&quot;&gt;
        INSERT
          INTO TBL_MENU
        (
          MENU_NAME
        , MENU_PRICE
        , CATEGORY_CODE
        , ORDERABLE_STATUS
        )
        VALUES
        (
          #{ menuName }
        , #{ menuPrice }
        , #{ categoryCode }
        , 'Y'
        )
    &lt;/insert&gt;</code></pre>
<p>이런 식으로 해서 최종 출력까지 하면 등록도 잘 된다.</p>
<p>하지만 auto-increment로 되어 있는 menu_code로 인해 메뉴 번호는 지정하기 위해선 다른 조치가 필요하다. (일단은 생략할 것이다.)</p>
<hr />
<h3 id="4번-메뉴-수정-및-삭제">4번. 메뉴 수정 및 삭제</h3>
<p>원래는 4번, 5번으로 나눠야 하지만 그냥 같이 알아보려고 한다. 위의 흐름과 크게 다를건 없기 때문이다.</p>
<p><strong>Application case문</strong></p>
<pre><code class="language-java">                case 4:
                    menuController.modifyMenu(inputModifyMenu());
                    break;
                case 5:
                    System.out.print(&quot;삭제하고 싶은 메뉴 코드를 입력하세요 : &quot;);
                    int deleteCode = sc.nextInt();
                    menuController.deleteMenu(deleteCode);
                    break;</code></pre>
<p>메뉴 추가해주는 것처럼 <code>inputModifyMenu()</code> 메소드도 아래와 같이 Application에 추가해줬다.</p>
<pre><code class="language-java">    private static Map&lt;String, String&gt; inputModifyMenu() {
        Scanner sc = new Scanner(System.in);

        System.out.print(&quot;변경할 메뉴의 메뉴 번호를 입력해 주세요 : &quot;);
        String menuCode = sc.nextLine();
        System.out.print(&quot;변경할 메뉴의 이름을 입력해 주세요 : &quot;);
        String menuName = sc.nextLine();
        System.out.print(&quot;변경할 메뉴의 가격 입력해 주세요 : &quot;);
        String menuPrice = sc.nextLine();

        Map&lt;String, String&gt; parameter = new HashMap&lt;&gt;();
        parameter.put(&quot;menuCode&quot;, menuCode);
        parameter.put(&quot;menuName&quot;, menuName);
        parameter.put(&quot;menuPrice&quot;, menuPrice);

        return parameter;
    }</code></pre>
<p><strong>Controller 에도 modifyMenu, deleteMenu 추가</strong></p>
<pre><code class="language-java">    public void modifyMenu(Map&lt;String, String&gt; parameter) {
        int menuCode = Integer.parseInt(parameter.get(&quot;menuCode&quot;));
        String menuName = parameter.get(&quot;menuName&quot;);
        int menuPrice = Integer.parseInt(parameter.get(&quot;menuPrice&quot;));

        MenuDTO menuDTO = new MenuDTO();
        menuDTO.setMenuCode(menuCode);
        menuDTO.setMenuName(menuName);
        menuDTO.setMenuPrice(menuPrice);

        if (menuService.modifyMenu(menuDTO)) {
            printResult.printSuccessMessage(&quot;modify&quot;);
        } else {
            printResult.printErrorMessage(&quot;메뉴 추가 실패..&quot;);
        }
    }

    public void deleteMenu(int deleteCode) {
        if (menuService.deleteMenu(deleteCode)) {
            printResult.printSuccessMessage(&quot;remove&quot;);
        } else {
            printResult.printErrorMessage(&quot;해당 코드로 조회된 메뉴가 없습니다.&quot;);
        }
    }</code></pre>
<p>printResult의 성공했을 때의 메세지를 출력해주는** SuccessMessage case 문**</p>
<pre><code class="language-java">            case &quot;modify&quot; :
                successMessage = &quot;메뉴 수정에 성공하였습니다.&quot;;
                break;
            case &quot;remove&quot; :
                successMessage = &quot;메뉴 삭제에 성공하였습니다.&quot;;
                break;</code></pre>
<p><strong>MenuDAO</strong></p>
<pre><code class="language-java">    public int modifyMenu(SqlSession sqlSession, MenuDTO menuDTO) {
        return sqlSession.update(&quot;MenuMapper.modifyMenu&quot;, menuDTO);
    }

    public int deleteMenuByMenuCode(SqlSession sqlSession, int deleteCode) {
        return sqlSession.delete(&quot;MenuMapper.deleteMenu&quot;, deleteCode);
    }</code></pre>
<p><strong>menu-mapper.xml</strong></p>
<pre><code class="language-xml">    &lt;update id=&quot;modifyMenu&quot; parameterType=&quot;com.ohgiraffers.section01.xmlconfig.MenuDTO&quot;&gt;
        UPDATE
               TBL_MENU
           SET
               MENU_NAME = #{ menuName }
             , MENU_PRICE = #{ menuPrice }
         WHERE MENU_CODE = #{ menuCode }
    &lt;/update&gt;

    &lt;delete id=&quot;deleteMenu&quot; parameterType=&quot;com.ohgiraffers.section01.xmlconfig.MenuDTO&quot;&gt;
        DELETE
          FROM TBL_MENU
         WHERE MENU_CODE = #{ menuCode }
    &lt;/delete&gt;</code></pre>
<hr />
<h2 id="mapper를-dao--mapper-역할을-주기">Mapper를 DAO + Mapper 역할을 주기</h2>
<p>전부 다 잘 되는 것을 알 수 있었다.</p>
<p>이번에는 <code>MenuMapper</code> 라는 인터페이스에 어노테이션을 활용하여 <code>menu-mapper.xml</code> 의 역할과 DAO의 역할을 해주게끔 하는 2번째 예제를 다뤄볼 생각이다.</p>
<p>위의 코드들과 비교하면 DAO를 쓰지 않고, Service에서 MenuMapper를 불러와서 사용하는 것밖에 없다고 보면 된다.</p>
<p><strong>MenuMapper 인터페이스는 DAO + Mapper 의 역할을 할 수도 있다.</strong></p>
<pre><code class="language-java">/* DAO + Mapper 의 의미*/
public interface MenuMapper {
    @Results(id=&quot;menuResultMap&quot;, value = {
            @Result(id = true, property = &quot;menuCode&quot;, column = &quot;MENU_CODE&quot;),
            @Result(property = &quot;menuName&quot;, column = &quot;MENU_NAME&quot;),
            @Result(property = &quot;menuPrice&quot;, column = &quot;MENU_PRICE&quot;),
            @Result(property = &quot;categoryCode&quot;, column = &quot;CATEGORY_CODE&quot;),
            @Result(property = &quot;orderableStatus&quot;, column = &quot;ORDERABLE_STATUS&quot;)
    })
    @Select(&quot;SELECT\n&quot; +
            &quot;       MENU_CODE\n&quot; +
            &quot;     , MENU_NAME\n&quot; +
            &quot;     , MENU_PRICE\n&quot; +
            &quot;     , CATEGORY_CODE\n&quot; +
            &quot;     , ORDERABLE_STATUS\n&quot; +
            &quot;  FROM TBL_MENU&quot;)
    List&lt;MenuDTO&gt; selectAllMenus();

    @Select(&quot;SELECT\n&quot; +
            &quot;               MENU_CODE\n&quot; +
            &quot;             , MENU_NAME\n&quot; +
            &quot;             , MENU_PRICE\n&quot; +
            &quot;             , CATEGORY_CODE\n&quot; +
            &quot;             , ORDERABLE_STATUS\n&quot; +
            &quot;          FROM TBL_MENU\n&quot; +
            &quot;         WHERE MENU_CODE = #{menuCode}&quot;)
    @ResultMap(&quot;menuResultMap&quot;)
    MenuDTO selectMenu(int menuCode);

    @Insert(&quot;&quot;&quot;
            INSERT
                      INTO TBL_MENU
                    (
                      MENU_NAME
                    , MENU_PRICE
                    , CATEGORY_CODE
                    , ORDERABLE_STATUS
                    )
                    VALUES
                    (
                      #{menuName}
                    , #{menuPrice}
                    , #{categoryCode}
                    , 'Y'
                    )
            &quot;&quot;&quot;)
    int insertMenu(MenuDTO menu);

    @Update(&quot;&quot;&quot;
            UPDATE
                           TBL_MENU
                       SET MENU_NAME = #{menuName}
                         , MENU_PRICE = #{menuPrice}
                     WHERE MENU_CODE = #{menuCode}
            &quot;&quot;&quot;)
    int updateMenu(MenuDTO menu);

    @Delete(&quot;&quot;&quot;
            DELETE
                      FROM TBL_MENU
                     WHERE MENU_CODE = #{menuCode}
            &quot;&quot;&quot;)
    int deleteMenu(int menuCode);
}</code></pre>
<p>이는 곧 xml에서 다뤘을 때의 쿼리들이 인텔리제이의 DML과 관련있는 어노테이션과 함께 사용한 것이다.</p>
<p>이 인터페이스를 Service에서 사용하는것과 마찬가지다 아래 코드를 같이 보자.</p>
<hr />
<p><strong>MenuService</strong></p>
<pre><code class="language-java">public class MenuService {

    private MenuMapper menuMapper;

    // 메뉴 전체 조회
    public List&lt;MenuDTO&gt; findAllMenus() {
        SqlSession sqlSession = getSqlSession();

        menuMapper = sqlSession.getMapper(MenuMapper.class);
        List&lt;MenuDTO&gt; menuList = menuMapper.selectAllMenus();

        sqlSession.close();
        return menuList;
    }

    // 메뉴 단 건 조회
    public MenuDTO findByMenuCode(int menuCode) {
        SqlSession sqlSession = getSqlSession();

        menuMapper = sqlSession.getMapper(MenuMapper.class);
        MenuDTO menuDTO = menuMapper.selectMenu(menuCode);

        sqlSession.close();
        return menuDTO;
    }

    // 신규 메뉴 등록
    public boolean registerMenu(MenuDTO menuDTO) {
        SqlSession sqlSession = getSqlSession();

        menuMapper = sqlSession.getMapper(MenuMapper.class);
        int result = menuMapper.insertMenu(menuDTO);

        if (result &gt; 0) sqlSession.commit();
        else sqlSession.rollback();

        sqlSession.close();

        return result &gt; 0 ? true : false;
    }

    // 메뉴 수정
    public boolean modifyMenu(MenuDTO menuDTO) {
        SqlSession sqlSession = getSqlSession();

        menuMapper = sqlSession.getMapper(MenuMapper.class);
        int result = menuMapper.updateMenu(menuDTO);

        if (result &gt; 0) sqlSession.commit();
        else sqlSession.rollback();

        sqlSession.close();

        return result &gt; 0 ? true : false;
    }

    // 메뉴 삭제
    public boolean deleteMenu(int deleteCode) {
        SqlSession sqlSession = getSqlSession();

        menuMapper = sqlSession.getMapper(MenuMapper.class);
        int result = menuMapper.deleteMenu(deleteCode);

        if (result &gt; 0) sqlSession.commit();
        else sqlSession.rollback();

        sqlSession.close();

        return result &gt; 0 ? true : false;
    }
}</code></pre>
<p>잘 보면 그냥 쿼리만 불러올 뿐 똑같은 코드인거나 마찬가지이다..</p>
<p>인터페이스를 잘 보면 <code>SELECT</code> 를 제외한 DML은 사실 int형을 반환해준다.</p>
<p>그래서 <code>result</code>에서 int형으로 삼항 연산을 통해 잘 되는지 아닌지 확인할 수 있다.</p>
<hr />
<h2 id="mapper-인터페이스와-xml-매핑하기">Mapper 인터페이스와 xml 매핑하기</h2>
<p>한 가지 방법이 더 있다.</p>
<p>mapper 인터페이스를 DAO의 역할로만 사용하면서</p>
<p>위에서 처럼 xml에는 쿼리를 담아서 mapper 인터페이스의 메소드와 같은 이름의 id로 둔 다음 사용하는 방식이 있다.</p>
<p><strong>Menumapper.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;com.jehun.section03.remix.MenuMapper&quot;&gt;
    &lt;resultMap id=&quot;menuResultMap&quot; type=&quot;com.jehun.section03.remix.MenuDTO&quot;&gt;
        &lt;id property=&quot;menuCode&quot; column=&quot;MENU_CODE&quot;/&gt;
        &lt;result property=&quot;menuName&quot; column=&quot;MENU_NAME&quot;/&gt;
        &lt;result property=&quot;menuPrice&quot; column=&quot;MENU_PRICE&quot;/&gt;
        &lt;result property=&quot;categoryCode&quot; column=&quot;CATEGORY_CODE&quot;/&gt;
        &lt;result property=&quot;orderableStatus&quot; column=&quot;ORDERABLE_STATUS&quot;/&gt;
    &lt;/resultMap&gt;

    &lt;select id=&quot;selectAllMenus&quot; resultMap=&quot;menuResultMap&quot;&gt;
        SELECT
        MENU_CODE
        , MENU_NAME
        , MENU_PRICE
        , CATEGORY_CODE
        , ORDERABLE_STATUS
        FROM TBL_MENU
    &lt;/select&gt;

    (... 중략)</code></pre>
<p>이 xml 파일에 쿼리가 들어가는데 다 같은 내용이라서 중략했다.</p>
<p>그럼 인터페이스는 어떻게 이뤄질까?</p>
<hr />
<p><strong>MenuMapper 인터페이스</strong></p>
<pre><code class="language-java">public interface MenuMapper {
    List&lt;MenuDTO&gt; selectAllMenus();

    MenuDTO selectMenu(int menuCode);

    int insertMenu(MenuDTO menu);

    int updateMenu(MenuDTO menu);

    int deleteMenu(int menuCode);
}</code></pre>
<p>잘 보면 xml에 있는 <code>&lt;mapper&gt;</code> 와 <code>&lt;select&gt;</code> 태그에 인터페이스에 관해 매핑되어 있고, id 값도 인터페이스의 메소드와 같은 것을 알 수 있다.</p>
<p>그것으로 구분해서 사용할 수도 있다.</p>