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
<pre><code class="language-java">import org.apache.ibatis.datasource.pooled.PooledDataSource;
import org.apache.ibatis.mapping.Environment;
import org.apache.ibatis.session.Configuration;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.apache.ibatis.transaction.jdbc.JdbcTransactionFactory;

import java.util.Date;

public class Application {
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
<p>Application 클래스</p>
<pre><code class="language-java">import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.Date;

public class Application {
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

    public int getMenuCode() {
        return menuCode;
    }

    public void setMenuCode(int menuCode) {
        this.menuCode = menuCode;
    }

    public String getMenuName() {
        return menuName;
    }

    public void setMenuName(String menuName) {
        this.menuName = menuName;
    }

    public int getMenuPrice() {
        return menuPrice;
    }

    public void setMenuPrice(int menuPrice) {
        this.menuPrice = menuPrice;
    }

    public int getCategoryCode() {
        return categoryCode;
    }

    public void setCategoryCode(int categoryCode) {
        this.categoryCode = categoryCode;
    }

    public String getOrderableStatus() {
        return orderableStatus;
    }

    public void setOrderableStatus(String orderableStatus) {
        this.orderableStatus = orderableStatus;
    }

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
<pre><code class="language-java">import java.util.Scanner;

public class Application {
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
<pre><code class="language-java">import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class Template {

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
<pre><code class="language-java">import java.util.List;

public class MenuController {
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
<pre><code class="language-java">import org.apache.ibatis.session.SqlSession;

import java.util.List;

import static com.jehun.section01.xmlconfig.Template.getSqlSession;

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
<pre><code class="language-java">import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class MenuDAO {
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
<p>전부 다 잘 되는 것을 알 수 있었다.</p>
<p>다음 게시글로는 <code>MenuMapper</code> 라는 인터페이스에 어노테이션을 활용하여 <code>menu-mapper.xml</code> 의 역할과 DAO의 역할을 해주게끔 하는 2번째 예제를 다뤄볼 생각이다.</p>
<p>이번 게시글에서 수정될만한건 DAO를 쓰지 않고, Service에서 MenuMapper를 불러와서 사용하는 것밖에 없지만 너무 길어지는 느낌이 있는 것 같아 한 번 끊고 다음 게시글에서 다뤄보자.</p>