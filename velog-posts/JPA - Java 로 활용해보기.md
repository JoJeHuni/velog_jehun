<p>이전 게시글에 이어 간단한 예시로 만들어보려고 한다.</p>
<p>Menu, Category, MenuAndCategory 클래스를 만들어서 할 것이다.</p>
<p><strong>Category</strong></p>
<pre><code class="language-java">public class Category {
    private int categoryCode;
    private String categoryName;

    // 기본 생성자, 매개변수 생성자, getter, setter, toString() 만들기

}</code></pre>
<p><strong>Menu</strong></p>
<pre><code class="language-java">public class Menu {
    private int menuCode;
    private String menuName;
    private int menuPrice;
    private int categoryCode;
    private String orderableStatus;

    // 기본 생성자, 매개변수 생성자, getter, setter, toString() 만들기

}</code></pre>
<p><strong>MenuAndCategory</strong></p>
<pre><code class="language-java">public class MenuAndCategory {
    private int menuCode;
    private String menuName;
    private int menuPrice;
    private Category category;
    private String orderableStatus;

    // 기본 생성자, 매개변수 생성자, getter, setter, toString() 만들기

}</code></pre>
<p>JPA 예제는 테스트 코드에서 해볼 것이다.</p>
<p>JDBC API를 이용해 쿼리를 직접 쓰게 되면 생기는 문제들에 대해 먼저 살펴볼 것이기 때문에 테스트 클래스를 만들어보자.</p>
<p><strong>ProblemOfUsingDirectSQLTests</strong></p>
<pre><code class="language-java">class ProblemOfUsingDirectSQLTests {
    private Connection con;

    @BeforeEach
    void setConnection() throws SQLException {
        String driver = &quot;com.mysql.cj.jdbc.Driver&quot;;
        String url = &quot;jdbc:mysql://localhost:3306/menudb&quot;;
        String user = &quot;swcamp&quot;;
        String password = &quot;swcamp&quot;;

        con = DriverManager.getConnection(url, user, password);
        con.setAutoCommit(false);
    }

        @AfterEach
    void closeConnection() throws SQLException {
        con.rollback(); // 직접 Java에 영향을 주지 않기 위해 롤백
        con.close();
    }
}</code></pre>
<h3 id="jdbc-api를-이용해-직접-sql을-다룰-때-발생할-수-있는-문제점">JDBC API를 이용해 직접 SQL을 다룰 때 발생할 수 있는 문제점</h3>
<ol>
<li>데이터 변환, SQL 작성, JDBC API 코드 등을 중복 작성(개발시간 증가, 유지보수 저하)</li>
<li>SQL에 의존하여 개발</li>
<li>패러다임 불일치(상속, 연관관계, 객체 그래프 탐색, 방향성)</li>
<li>동일성 보장 문제</li>
</ol>
<p>일단은 직접 SQL을 작성해서 메뉴를 조회하면서 문제를 확인해보자.</p>
<p><strong>ProblemOfUsingDirectSQLTests에 추가</strong></p>
<pre><code class="language-java">    @DisplayName(&quot;직접 SQL을 작성하여 메뉴를 조회할 때 발생하는 문제 확인&quot;)
    @Test
    void testDirectSelectSql() throws SQLException {

        // given
        String query = &quot;SELECT MENU_CODE, MENU_NAME, MENU_PRICE, CATEGORY_CODE, ORDERABLE_STATUS&quot;
                       + &quot; FROM TBL_MENU&quot;;

        // when
        Statement stmt = con.createStatement();
        ResultSet rset = stmt.executeQuery(query);

        List&lt;Menu&gt; menuList = new ArrayList&lt;&gt;();
        while(rset.next()) {
            Menu menu = new Menu();
            menu.setMenuCode(rset.getInt(&quot;MENU_CODE&quot;));
            menu.setMenuName(rset.getString(&quot;MENU_NAME&quot;));
            menu.setMenuPrice(rset.getInt(&quot;MENU_PRICE&quot;));
            menu.setCategoryCode(rset.getInt(&quot;CATEGORY_CODE&quot;));
            menu.setOrderableStatus(rset.getString(&quot;ORDERABLE_STATUS&quot;));

            menuList.add(menu);
        }

        // then
        Assertions.assertTrue(!menuList.isEmpty());
        menuList.forEach(System.out::println); // 로그 개념으로 출력해보기
    }</code></pre>
<p>왜 jpa를 쓰는지 알려줄 수 있는 구간이when 부분이다.</p>
<p>Statement, ResultSet을 직접 작성해야 하고 set, get을 이용해서 하는 것부터 굉장히 불편하다. </p>
<p>전체가 아닌 일부일 때도 새로 만들거나 해야 해서 sql부터 새로 짜야 하는 번거로움이 있다.</p>
<blockquote>
<p>데이터 변환, SQL 작성, JDBC API 코드 등을 중복 작성(개발시간 증가, 유지보수 저하)</p>
</blockquote>
<p>이전에 했던 것과 다른 컴퓨터로 옮기면서 데이터 스크립트를 새로 등록했었고,</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/097eb2be-fd6c-4cfa-9879-41b8ac7b76c2/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7dc80e0f-da54-4ee0-b779-6b6724e903a1/image.png" /></p>
<p>잘 되는 것을 볼 수 있었다.</p>
<hr />
<p>2번째 문제를 한 번 예제로 봐보자.</p>
<p><strong>ProblemOfUsingDirectSQLTests에 추가</strong></p>
<pre><code class="language-java">    @DisplayName(&quot;직접 SQL을 작성하여 신규 메뉴를 추가할 때 발생하는 문제 확인&quot;)
    @Test
    void testDirectInsertSQL() throws SQLException {

        // given
        Menu menu = new Menu();
        menu.setMenuName(&quot;민트초코짜장면&quot;);
        menu.setMenuPrice(12000);
        menu.setCategoryCode(1);
        menu.setOrderableStatus(&quot;Y&quot;);

        String query = &quot;INSERT INTO TBL_MENU(MENU_NAME, MENU_PRICE, CATEGORY_CODE, &quot;
                + &quot;ORDERABLE_STATUS) VALUES (?, ?, ?, ?)&quot;;

        // when
        PreparedStatement pstmt = con.prepareStatement(query);
        pstmt.setString(1, menu.getMenuName());
        pstmt.setInt(2, menu.getMenuPrice());
        pstmt.setInt(3, menu.getCategoryCode());
        pstmt.setString(4, menu.getOrderableStatus());

        int result = pstmt.executeUpdate();

        // then
        Assertions.assertEquals(1, result);
        pstmt.close();
    }</code></pre>
<p><code>?</code> 을 활용해서 1, 2, 3, 4를 넣으면서 한다는 것을 보면 SQL에 의존해서 넣어서 개발하는 것을 볼 수 있는데 이것도 문제이다.</p>
<p>특정 변수에 담아서 넣는다면 그나마 괜찮지만 저렇게 값을 넣는 과정은 좋지 않다.</p>
<blockquote>
<p>SQL에 의존하여 개발</p>
</blockquote>
<hr />
<p>3번째 문제를 한 번 예제로 봐보자.</p>
<p><strong>ProblemOfUsingDirectSQLTests에 추가</strong></p>
<pre><code class="language-java">    @DisplayName(&quot;연관된 객체 문제 확인&quot;)
    @Test
    void testAssociationObject() throws SQLException {

        // given
        String query = &quot;SELECT A.MENU_CODE, A.MENU_NAME, A.MENU_PRICE, B.CATEGORY_CODE, &quot;
                + &quot;B.CATEGORY_NAME, A.ORDERABLE_STATUS &quot;
                + &quot;FROM TBL_MENU A &quot;
                + &quot;JOIN TBL_CATEGORY B ON (A.CATEGORY_CODE = B.CATEGORY_CODE)&quot;;

        // when
        Statement stmt = con.createStatement();
        ResultSet rset = stmt.executeQuery(query);

        List&lt;MenuAndCategory&gt; menuAndCategories = new ArrayList&lt;&gt;();
        while(rset.next()) {
            MenuAndCategory menuAndCategory = new MenuAndCategory();
            menuAndCategory.setMenuCode(rset.getInt(&quot;MENU_CODE&quot;));
            menuAndCategory.setMenuName(rset.getString(&quot;MENU_NAME&quot;));
            menuAndCategory.setMenuPrice(rset.getInt(&quot;MENU_PRICE&quot;));
            menuAndCategory.setCategory(new Category(rset.getInt(&quot;CATEGORY_CODE&quot;),
                    rset.getString(&quot;CATEGORY_NAME&quot;)));
            menuAndCategory.setOrderableStatus(rset.getString(&quot;ORDERABLE_STATUS&quot;));

            menuAndCategories.add(menuAndCategory);
        }

        // then
        Assertions.assertTrue(!menuAndCategories.isEmpty());
        menuAndCategories.forEach(System.out::println);
    }</code></pre>
<blockquote>
<p>패러다임 불일치(상속, 연관관계, 객체 그래프 탐색, 방향성)</p>
</blockquote>
<p>위의 문제점들 중 코드에서 다룬건 연관관계이다.</p>
<p>Menu 와 Category가 연관돼있을 때 위와 같이 구현해야 한다.</p>
<p>이후 JPA를 배우는데 <code>@ManyToOne</code>을 통해서 해줄 수 있긴 하지만, 그것도 많이 쓰지 않는 것이 좋다. (여러 문제를 일으킬 수 있기 때문이다.)</p>
<p>상속, 연관관계 등 개념에 대해 보자.</p>
<p><strong>상속 문제</strong>
객체 지향 언어의 상속 개념과 유사한 것이 데이터베이스의 서브타입 엔티티이다.(서브 타입을 별도의 테이블로 나누었을 때)
슈퍼타입의 모든 속성을 서브타입이 공유하지 못하여 물리적으로 다른 테이블로 분리가 된 형태이다. (설계에 따라서는 다를 수 있다.)
하지만 객체지향의 상속은 슈퍼타입의 속성을 공유해서 사용하므로 여기에서 패러다임의 불일치가 발생한다.</p>
<p><strong>연관 관계</strong>
객체지향에서 말하는 가지고 있는(ASSOCIATION 연관관계 혹은 COLLECTION 연관관계) 경우 데이터베이스 저장 구조와 다른 형태이다.</p>
<ul>
<li><p><strong>데이터베이스 테이블에 맞춘 객체 모델</strong></p>
<pre><code class="language-java">public class Menu {
  private int menuCode;
  private String menuName;
  private int menuPrice;
  private int categoryCode;
  private String orderableStatus;
}</code></pre>
</li>
<li><p><strong>객체 지향 언어에 맞춘 객체 모델</strong></p>
<pre><code class="language-java">public class Menu {
  private int menuCode;
  private String menuName;
  private int menuPrice;
  private Category category;
  private String orderableStatus;
}</code></pre>
</li>
</ul>
<p>객체 지향 언어에 맞춘 객체 모델은 JPA에서 추구하는 방식이다.</p>
<p>애초에 고민하는 것 자체가 조인을 염두에 둘 것인가를 두고 하는 것이다.</p>
<hr />
<p>4번째 문제</p>
<p><strong>ProblemOfUsingDirectSQLTests에 추가</strong></p>
<pre><code class="language-java">    @DisplayName(&quot;조회한 두 개의 행을 담은 객체의 동일성 비교 테스트&quot;)
    @Test
    void testEquals() throws SQLException {

        // given
        String query = &quot;SELECT MENU_CODE, MENU_NAME FROM TBL_MENU WHERE MENU_CODE = 12&quot;;

        // when
        Statement stmt1 = con.createStatement();
        ResultSet rset1 = stmt1.executeQuery(query);

        Menu menu1 = null;

        if(rset1.next()) {
            menu1 = new Menu();
            menu1.setMenuCode(rset1.getInt(&quot;MENU_CODE&quot;));
            menu1.setMenuName(rset1.getString(&quot;MENU_NAME&quot;));
        }

        Statement stmt2 = con.createStatement();
        ResultSet rset2 = stmt2.executeQuery(query);

        Menu menu2 = null;

        if(rset2.next()) {
            menu2 = new Menu();
            menu2.setMenuCode(rset2.getInt(&quot;MENU_CODE&quot;));
            menu2.setMenuName(rset2.getString(&quot;MENU_NAME&quot;));
        }

        // then
        Assertions.assertNotEquals(menu1, menu2);
    }</code></pre>
<p>보면 JPA를 사용하지 않고 한다면 계속 객체가 생성되고 낭비가 된다.</p>
<p><strong>JPA를 이용하면</strong> 동일 비교가 가능하다.</p>
<p>영속성 컨텍스트에 담겨있는 객체를 계속 entityManager가 가져다주기 때문에 객체도 낭비되지 않는다.</p>
<pre><code class="language-java">Menu menu1 = entityManager.find(Menu.class, 12);
Menu menu2 = entityManager.find(Menu.class, 12);
System.out.println(menu1 == menu2) // true(동일성이 보장된다. 같은 객체)</code></pre>
<hr />
<h1 id="jpa-활용">JPA 활용</h1>
<h2 id="entitymanager-생명주기-활용">EntityManager 생명주기 활용</h2>
<p>Java 환경에서 JDK 17버전으로 프로젝트를 만들고</p>
<p>TEST 패키지 속 <code>resources/META-INF</code> 안에 <code>persistence.xml</code> 을 작성하자.</p>
<p>여러 패키지를 만들고 이름은 같지만 다른 클래스인 Menu도 만들 것이다.</p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;persistence xmlns=&quot;http://xmlns.jcp.org/xml/ns/persistence&quot; version=&quot;2.2&quot;&gt;

    &lt;!-- 설명. 엔티티 매니저 팩토리를 식별하기 위한 이름 설정 --&gt;
    &lt;persistence-unit name=&quot;jpatest&quot;&gt;

        &lt;!-- 설명. 엔티티는 설정에 따로 추가할 것 --&gt;
        &lt;class&gt;com.jehun.section02.crud.Menu&lt;/class&gt;
        &lt;class&gt;com.jehun.section03.persistencecontext.Menu&lt;/class&gt;

        &lt;properties&gt;

            &lt;!-- 설명. 데이터베이스 연결 정보 --&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.driver&quot; value=&quot;com.mysql.cj.jdbc.Driver&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.url&quot; value=&quot;jdbc:mysql://localhost:3306/menudb&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.user&quot; value=&quot;{username}&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.password&quot; value=&quot;{username}&quot;/&gt;

            &lt;!-- 설명. hibernate 설정(실행되는 sql 구문을 괜찮은 format 형태로 보여주기) --&gt;
            &lt;property name=&quot;hibernate.show_sql&quot; value=&quot;true&quot;/&gt;
            &lt;property name=&quot;hibernate.format_sql&quot; value=&quot;true&quot;/&gt;
            &lt;property name=&quot;hibernate.dialect&quot; value=&quot;org.hibernate.dialect.MariaDB103Dialect&quot;/&gt;
        &lt;/properties&gt;
    &lt;/persistence-unit&gt;
&lt;/persistence&gt;</code></pre>
<hr />
<h3 id="section01">section01</h3>
<p>com.jehun.section01.A_EntityManagerLifeCycleTests</p>
<pre><code class="language-java">import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;
import org.junit.jupiter.api.*;

public class A_EntityManagerLifeCycleTests {
    private static EntityManagerFactory emf;
    private static EntityManager em;

    @BeforeAll
    public static void initFactory() {
        emf = Persistence.createEntityManagerFactory(&quot;jpatest&quot;);
    }

    @BeforeEach
    public void initManager() {
        em = emf.createEntityManager();
    }

    @Test
    public void 엔티티_매니저_팩토리와_엔티티_매니저_생명주기_확인1() {
        System.out.println(&quot;factory의 hashCode1: &quot; + emf.hashCode());
        System.out.println(&quot;manager의 hashCode1: &quot; + em.hashCode());
    }

    @Test
    public void 엔티티_매니저_팩토리와_엔티티_매니저_생명주기_확인2() {
        System.out.println(&quot;factory의 hashCode2: &quot; + emf.hashCode());
        System.out.println(&quot;manager의 hashCode2: &quot; + em.hashCode());
    }

    @AfterEach
    public void closeManager() {
        em.close();
    }

    @AfterAll
    public static void closeFactory() {
        emf.close();
    }
}</code></pre>
<p>생명주기를 알아보면서 개념도 함께 알아보자.</p>
<h3 id="엔티티-매니저와-관련된-개념">엔티티 매니저와 관련된 개념</h3>
<p><strong>엔티티 매니저 팩토리(EntityManagerFactory)란?</strong></p>
<ul>
<li>엔티티 매니저를 생성할 수 있는 기능을 제공하는 팩토리 클래스이다.</li>
<li>thread-safe하기 때문에 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 스레드 간 공유해서 재사용한다.</li>
<li>thread-safe한 기능을 요청 스코프마다 생성하기에는 비용(시간, 메모리) 부담이 크므로</li>
<li>application 스코프와 동일하게 싱글톤으로 생성해서 관리하는 것이 효율적이다.</li>
</ul>
<blockquote>
<p>따라서 데이터베이스를 사용하는 애플리케이션 당 한 개의 EntityManagerFactory를 생성한다.</p>
</blockquote>
<p><strong>persistence.xml 속 엔티티 매니저 팩토리를 식별하기 위한 이름 설정</strong></p>
<pre><code class="language-xml">    &lt;persistence-unit name=&quot;jpatest&quot;&gt;</code></pre>
<p><strong>엔티티 매니저(EntityManager)란?</strong></p>
<ul>
<li>엔티티 매니저는 엔티티를 저장하는 메모리 상의 데이터베이스를 관리하는 인스턴스이다.</li>
<li>엔티티를 저장하고, 수정, 삭제, 조회하는 등의 엔티티와 관련된 모든 일을 한다.</li>
<li>엔티티 매니저는 thread-safe하지 않기 때문에 동시성 문제가 발생할 수 있다.</li>
</ul>
<blockquote>
<p>따라서 스레드 간 공유를 하지 않고, web의 경우 일반적으로 request scope와 일치시킨다.</p>
</blockquote>
<p><strong>영속성 컨텍스트(PersistenceContext)란?</strong></p>
<ul>
<li>엔티티 매니저를 통해 엔티티를 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.</li>
<li>영속성 컨텍스트는 엔티티를 key-value 방식으로 저장하는 저장소이다.</li>
<li>영속성 컨텍스트는 엔티티 매니저를 생성할 때 같이 하나 만들어진다.</li>
<li>그리고 엔티티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있고, 또 관리할 수 있다.</li>
</ul>
<hr />
<p>다시 위의 코드와 함께 보자.</p>
<pre><code class="language-java">    private static EntityManagerFactory emf;
    private static EntityManager em;

    @BeforeAll
    public static void initFactory() {
        emf = Persistence.createEntityManagerFactory(&quot;jpatest&quot;);
    }

    @BeforeEach
    public void initManager() {
        em = emf.createEntityManager();
    }</code></pre>
<p>시작하기 전 팩토리는 식별해뒀던 <code>jpatest</code>로 만든다.</p>
<p>그 다음 jpa 담당 매니저라고 볼 수 있는 EntityManager를 만든 것이다.</p>
<p>매니저라고는 하지만 실제로는 스트림이기 때문에 테스트 케이스가 끝나면 닫아줘야 한다.</p>
<pre><code class="language-java">    @AfterEach
    public void closeManager() {
        em.close();
    }

    @AfterAll
    public static void closeFactory() {
        emf.close();
    }</code></pre>
<p>위의 어노테이션이 달린 부분은 하나의 틀이라고 생각하면 된다.</p>
<p>이제 테스트 코드의 메소드이다.</p>
<pre><code class="language-java">    @Test
    public void 엔티티_매니저_팩토리와_엔티티_매니저_생명주기_확인1() {
        System.out.println(&quot;factory의 hashCode1: &quot; + emf.hashCode());
        System.out.println(&quot;manager의 hashCode1: &quot; + em.hashCode());
    }

    @Test
    public void 엔티티_매니저_팩토리와_엔티티_매니저_생명주기_확인2() {
        System.out.println(&quot;factory의 hashCode2: &quot; + emf.hashCode());
        System.out.println(&quot;manager의 hashCode2: &quot; + em.hashCode());
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b06d1c81-4696-47b2-a705-13fde6e83123/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/989067d8-9a80-4f22-97dd-d4473257630e/image.png" /></p>
<p>싱글톤한지 확인하기 위해 출력해보았다.</p>
<hr />
<h2 id="테스트-코드로-crud-해보기">테스트 코드로 CRUD 해보기</h2>
<p>A_EntityManagerCRUDTests 라는 이름으로 해볼 것이다.</p>
<p>위의 코드 중 <code>@BeforeEach</code>, <code>@BeforeAll</code>, <code>@AfterEach</code>, <code>@AfterAll</code></p>
<p>메소드들은 가져오자.</p>
<p>이제 CRUD를 만들어 볼 것이다.</p>
<h3 id="read">READ</h3>
<p><strong>A_EntityManagerCRUDTests</strong></p>
<pre><code class="language-java">    @Test
    public void 메뉴코드로_메뉴_조회_테스트() {

        // given
        int menuCode = 2;

        // when
        Menu foundMenu = em.find(Menu.class, menuCode);

        // then
        Assertions.assertNotNull(foundMenu);
        Assertions.assertEquals(menuCode, foundMenu.getMenuCode());
        System.out.println(&quot;foundMenu = &quot; + foundMenu);
    }</code></pre>
<p><code>menuCode</code>에 맞는 <code>Menu.class</code> 타입을 보고 <code>Menu</code>에 담아서 반환된 것이 <code>foundMenu</code>이다.</p>
<p>메뉴 클래스는 아래와 같다.</p>
<p><strong>Menu</strong></p>
<pre><code class="language-java">@Entity(name=&quot;section02_menu&quot;)
@Table(name=&quot;tbl_menu&quot;)

public class Menu {
    @Id
    @Column(name=&quot;menu_code&quot;)
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private int menuCode;

    @Column(name=&quot;menu_name&quot;)
    private String menuName;

    @Column(name=&quot;menu_price&quot;)
    private int menuPrice;

    @Column(name=&quot;category_code&quot;)
    private int categoryCode;

    @Column(name=&quot;orderable_status&quot;)
    private String orderableStatus;

    // 기본 생성자, 매개변수 있는 생성자, getter, setter, toString()
}</code></pre>
<p><strong>테스트 코드 출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6b6609d4-2f07-4679-a3da-efe72f1abb6b/image.png" /></p>
<p>READ는 잘 됐다. 다음은 INSERT를 해보자.</p>
<hr />
<h3 id="insert">INSERT</h3>
<p><strong>A_EntityManagerCRUDTests</strong></p>
<pre><code class="language-java">    @Test
    public void 새로운_메뉴_추가_테스트() {

        // given
        Menu menu = new Menu();
        menu.setMenuName(&quot;꿀발린추어탕&quot;);
        menu.setMenuPrice(7000);
        menu.setCategoryCode(4);
        menu.setOrderableStatus(&quot;Y&quot;);

        // when
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {
            em.persist(menu);
            tx.commit();
        } catch(Exception e) {
            tx.rollback();
        }

        // then
        Assertions.assertTrue(em.contains(menu));       // 영속성 컨텍스트에 menu가 존재하는지 확인
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/71a26284-d22b-4410-a7fb-55cf6b594f5a/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3c63eccf-3073-492f-8724-3fa363025bee/image.png" /></p>
<p>INSERT도 잘 된 것을 알 수 있다.</p>
<hr />
<h3 id="update">Update</h3>
<p><strong>A_EntityManagerCRUDTests</strong></p>
<pre><code class="language-java">    @Test
    public void 메뉴_이름_수정_테스트() {

        // given
        Menu menu = em.find(Menu.class, 2);
        System.out.println(&quot;menu = &quot; + menu);

        String menuNameToChange = &quot;문어스무디&quot;;

        // when
//        EntityTransaction tx = em.getTransaction();
//        tx.begin();

        try {
            menu.setMenuName(menuNameToChange);
//            tx.commit();
        } catch(Exception e) {
//            tx.rollback();
        }

        // then
        Assertions.assertEquals(menuNameToChange, em.find(Menu.class, 2).getMenuName());
    }</code></pre>
<p>위의 코드들 다 지금 전부 새로운 EntityManager를 만들고 사용한다.
그래서 일단 비어있기 때문에 Update 코드에서도 2번 Menu를 가져온다.</p>
<p>즉, 영속 상태의 객체인 것이다. (EntityManager 가 가져다주니까)</p>
<p>EntityManager이 현재 보고 있는 상태인데 변경을 한다면, <strong>update문이 쓰기 지연 저장소에 쌓이게 된다.</strong></p>
<p>그리고 그 후에 변화가 없이 commit이 일어난다면 쓰기 지연 저장소에 쌓인 것들을 flush() 하면서 DB에 덮어씌운다.</p>
<p>영속 상태인 객체를 가져와 변경만 한 뒤 DB에 덮어씌우지 않기 위해서 트랜잭션 부분만 주석처리했다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/79e8a043-bebe-4df4-9c98-1cafdd7647cf/image.png" /></p>
<p>바꾸긴 했지만 DB 상에는 변화가 없다. (출력문도 쿼리 보내기 전이다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/dfa41f45-ccb0-482f-bf04-8d24810cb4fd/image.png" /></p>
<hr />
<h3 id="delete">Delete</h3>
<p><strong>A_EntityManagerCRUDTests</strong></p>
<pre><code class="language-java">    @Test
    public void 메뉴_삭제하기_테스트() {

        // given
        Menu menuToRemove = em.find(Menu.class, 2);

        // when
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {
            em.remove(menuToRemove);
            tx.commit();
        } catch(Exception e) {
            tx.rollback();
        }

        // then
        Menu removeMenu = em.find(Menu.class, 2);
        Assertions.assertEquals(null, removeMenu);
    }</code></pre>
<p>삭제도 잘 될 것이다.</p>