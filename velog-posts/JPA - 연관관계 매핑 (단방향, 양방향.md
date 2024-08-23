<h1 id="연관관계-매핑">연관관계 매핑</h1>
<p><code>@ManyToOne</code>, <code>@OneToMany</code>, <code>@ManyToMany</code> 3가지 어노테이션을 활용하면서 단방향 연관관계와 양방향 연관관계에 대해 알아볼 것이다.</p>
<h2 id="기본-세팅">기본 세팅</h2>
<p><strong>build.gradle</strong></p>
<pre><code class="language-gradle">dependencies {
    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'

    implementation(&quot;org.hibernate.orm:hibernate-core:6.3.1.Final&quot;)
    implementation 'com.mysql:mysql-connector-j:8.0.33'
}</code></pre>
<p><strong>resources.META-INF.persistence.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;persistence xmlns=&quot;http://xmlns.jcp.org/xml/ns/persistence&quot; version=&quot;2.2&quot;&gt;

    &lt;!-- 설명. 엔티티 매니저 팩토리를 식별하기 위한 이름 설정 --&gt;
    &lt;persistence-unit name=&quot;jpatest&quot;&gt;

        &lt;!-- 설명. 엔티티는 설정에 따로 추가할 것 --&gt;
        &lt;class&gt;com.jehun.section01.manytoone.Category&lt;/class&gt;
        &lt;class&gt;com.jehun.section01.manytoone.MenuAndCategory&lt;/class&gt;
        &lt;class&gt;com.jehun.section02.onetomany.CategoryAndMenu&lt;/class&gt;
        &lt;class&gt;com.jehun.section02.onetomany.Menu&lt;/class&gt;
        &lt;class&gt;com.jehun.section03.bidirection.Menu&lt;/class&gt;
        &lt;class&gt;com.jehun.section03.bidirection.Category&lt;/class&gt;

        &lt;properties&gt;

            &lt;!-- 설명. 데이터베이스 연결 정보 --&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.driver&quot; value=&quot;com.mysql.cj.jdbc.Driver&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.url&quot; value=&quot;jdbc:mysql://localhost:3306/menudb&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.user&quot; value=&quot;swcamp&quot;/&gt;
            &lt;property name=&quot;jakarta.persistence.jdbc.password&quot; value=&quot;swcamp&quot;/&gt;

            &lt;!-- 설명. hibernate 설정(실행되는 sql 구문을 괜찮은 format 형태로 보여주기) --&gt;
            &lt;property name=&quot;hibernate.show_sql&quot; value=&quot;true&quot;/&gt;
            &lt;property name=&quot;hibernate.format_sql&quot; value=&quot;true&quot;/&gt;
            &lt;property name=&quot;hibernate.dialect&quot; value=&quot;org.hibernate.dialect.MariaDBDialect&quot;/&gt;
&lt;!--            &lt;property name=&quot;hibernate.hbm2ddl.auto&quot; value=&quot;create&quot;/&gt;--&gt;
        &lt;/properties&gt;
    &lt;/persistence-unit&gt;
&lt;/persistence&gt;</code></pre>
<p>패키지 구조는 총 3가지</p>
<ul>
<li><code>com.jehun.section01.manytoone</code></li>
<li><code>com.jehun.section02.onetomany</code></li>
<li><code>com.jehun.section03.bidirection</code></li>
</ul>
<p>1번째껏부터 해볼 것이다.</p>
<hr />
<p>우선 간단하게 알아보면 </p>
<p>Menu와 Category라고 했을 때</p>
<p>4가지 경우가 있다.</p>
<ol>
<li>onetoone</li>
<li>onetomany</li>
<li>manytoone</li>
<li>manytomany</li>
</ol>
<ul>
<li><p><code>OnetoOne</code>
모든 객체들에 대해 1개 Menu에는 1개 Category만 매핑할 때 -&gt; OnetoOne</p>
</li>
<li><p><code>OneToMany</code> (ManyToOne과 헷갈리지 말라는 의미에서 굵게 표시했다. 크게 중요 X)
Menu N개가 1개의 Category에 속하는 경우에서 <strong>Category가 Menu를 바라볼 때</strong></p>
</li>
<li><p><strong><code>ManyToOne</code></strong>
Menu N개가 1개의 Category에 속하는 경우에서 <strong>Menu가 Category를 바라볼 때</strong></p>
</li>
<li><p><code>ManyToMany</code>
둘다 많은 것 (즉, 학생과 시험)
ex) 학생의 종류와 시험의 종류가 여러 개씩 있을 때 '학생이 본 시험'은 ManyToMany를 풀기 위한 중간에 들어간다.</p>
</li>
</ul>
<p>JPA에서는 이 상황에서 '학생이 본 시험'과 같은 엔티티를 만드는 개념 자체가 없다.</p>
<blockquote>
<p>JPA에서는 중간 엔티티를 만들지 않아도 자동으로 만들어준다. 물리적으로 불가능하기 때문에 그렇다. -&gt; <strong>중간 엔티티의 존재를 파악 못 하게 되고 따로 Insert 하는 것이 힘들다..</strong></p>
<p>그냥 안 쓰는게 제일 좋다.</p>
</blockquote>
<p><strong>ManyToOne부터 정리해보자.</strong></p>
<hr />
<h2 id="manytoone">ManyToOne</h2>
<p>Menu와 Category는 Menu N개가 Category 1개에 해당이 된다.</p>
<p>즉 Menu -&gt; Category 로 부터의 연관관계를 나타낸다고 생각하면 된다.</p>
<p>Menu는 Category가 없으면 안 되기 때문에 Category가 자식 테이블이다.</p>
<p><strong>Category</strong></p>
<pre><code class="language-java">@Entity(name=&quot;category_section01&quot;)
@Table(name=&quot;tbl_category&quot;)
public class Category {

    @Id
    @Column(name=&quot;category_code&quot;)
    private int categoryCode;

    @Column(name=&quot;category_name&quot;)
    private String categoryName;

    @Column(name=&quot;ref_category_code&quot;)
    private Integer refCategoryCode;

    // 기본 생성자, 매개변수가 있는 생성자, getter, toString()
}</code></pre>
<hr />
<p>Menu 지만 ManyToOne이라는 입장을 나타내기 위해서 나타낸 MenuAndCategory 이다.</p>
<p>무슨 뜻이지? 생각하지 않아도 된다.</p>
<p><strong>MenuAndCategory</strong></p>
<pre><code class="language-java">@Entity(name=&quot;menu_and_category&quot;)
@Table(name=&quot;tbl_menu&quot;)
public class MenuAndCategory {

    @Id
    @Column(name=&quot;menu_code&quot;)
    private int menuCode;

    @Column(name=&quot;menu_name&quot;)
    private String menuName;

    @Column(name=&quot;menu_price&quot;)
    private int menuPrice;

    /* JoinColumn에 쓰이는 컬럼명은 FK 제약조건이 걸린 자식 테이블의 컬럼명을 쓰게 된다. */
    @JoinColumn(name=&quot;category_code&quot;)
    @ManyToOne                          // 두 엔터디 간의 전체 카디널리티(N:1)를 고려해서 작성한다.
    private Category category;          // 하나의 메뉴는 하나의 카테고리를 가지고 있다

    @Column(name=&quot;orderable_status&quot;)
    private String orderableStatus;

    //기본 생성자, 매개변수가 있는 생성자, getter, toString()
}</code></pre>
<hr />
<p>이제 ManyToOne 연관관계 테스트를 해보자.</p>
<p><strong>ManyToOneAssociationTests</strong></p>
<pre><code class="language-java">public class ManyToOneAssociationTests {
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

    @AfterEach
    public void closeManager() {
        em.close();
    }

    @AfterAll
    public static void closeFactory() {
        emf.close();
    }
}</code></pre>
<p>위와 같이 기본 세팅을 해두고 다대일 연관관계 객체 그래프 탐색을 통한 조회를 해보자.</p>
<p><strong>위의 테스트 코드에 추가</strong></p>
<pre><code class="language-java">    @Test
    public void 다대일_연관관계_객체_그래프_탐색을_이용한_조회_테스트() {
        int menuCode = 15;

        MenuAndCategory foundMenu = em.find(MenuAndCategory.class, menuCode);
        Category menuCategory = foundMenu.getCategory();

        assertNotNull(menuCategory);
        System.out.println(&quot;foundMenu = &quot; + foundMenu);
        System.out.println(&quot;menuCategory = &quot; + menuCategory);
    }</code></pre>
<p><strong>출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/297962e1-2590-475a-919d-9c938643157a/image.png" /></p>
<blockquote>
<p><code>@ManyToOne</code>을 이용해 한 번에 조인이 돼서 쿼리가 나가는 것을 볼 수 있다.</p>
</blockquote>
<hr />
<h2 id="onetomany">OneToMany</h2>
<h3 id="n1-문제">N+1 문제</h3>
<p>Menu와 Category는 Menu N개가 Category 1개에 해당이 된다.</p>
<p>즉, Category -&gt; Menu 연관관계를 나타낸다고 생각하면 된다.</p>
<p>자연스럽게 Category를 Menu 역할로, MenuAndCategory를 CategoryAndMenu 역할로 바꿀 것이다.</p>
<p><strong>Menu</strong></p>
<pre><code class="language-java">@Entity(name=&quot;menu&quot;)
@Table(name=&quot;tbl_menu&quot;)
public class Menu {

    @Id
    @Column(name=&quot;menu_code&quot;)
    private int menuCode;

    @Column(name=&quot;menu_name&quot;)
    private String menuName;

    @Column(name=&quot;menu_price&quot;)
    private int menuPrice;

    @Column(name=&quot;category_code&quot;)
    private int categoryCode;

    @Column(name=&quot;orderable_status&quot;)
    private String orderableStatus;

    //기본 생성자, 매개변수가 있는 생성자, getter, toString()
}</code></pre>
<p>Menu는 Category에 속하기 때문에 컬럼에도 적어줬다.</p>
<p><strong>CategoryAndMenu</strong></p>
<pre><code class="language-java">@Entity(name=&quot;category_section02&quot;)
@Table(name=&quot;tbl_category&quot;)
public class CategoryAndMenu {

    @Id
    @Column(name=&quot;category_code&quot;)
    private int categoryCode;

    @Column(name=&quot;category_name&quot;)
    private String categoryName;

    @Column(name=&quot;ref_category_code&quot;)
    private Integer refCategoryCode;

    @JoinColumn(name=&quot;category_code&quot;)
    @OneToMany
    private List&lt;Menu&gt; menuList;

    //기본 생성자, 매개변수가 있는 생성자, getter, toString()
}</code></pre>
<p>잘 보면 <code>category_code</code> 의 컬럼명을 가진 <code>Menu</code>와 조인하면서 <code>menuList</code>에 담아준다.</p>
<p>즉, 쿼리로 카테고리를 조회하면?</p>
<p>카테고리 1번 + N개의 메뉴 -&gt; 1 + N개의 쿼리가 나오게 된다..</p>
<p>이것을 <strong>N + 1 문제</strong>라고 한다.</p>
<p>테스트 코드에서 확인해보자.</p>
<p><strong>OneToManyAssociationTests</strong></p>
<pre><code class="language-java">기존 세팅에 추가

    @Test
    public void 일대다_연관관계_객체_그래프_탐색을_이용한_조회_테스트() {
        int categoryCode = 4;

        CategoryAndMenu categoryAndMenu = em.find(CategoryAndMenu.class, categoryCode);

        assertNotNull(categoryAndMenu);

        System.out.println(&quot;categoryAndMenu = &quot; + categoryAndMenu);
        // 카테고리 1개를 뽑고 싶은데 N개가 딸려온다. (N+1 문제가 발생)
    }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3572aaea-9930-47f6-b1fe-8ae3f4920c57/image.png" /></p>
<p>카테고리를 조회하고, 해당 카테고리에 속하는 메뉴도 조회하는 문제가 생기게 되는 것이 <code>@OneToMany</code> 이다.</p>
<hr />
<h2 id="manytomany">ManyToMany</h2>
<p>아주 좋지 않다.. 양방향은 그냥 하지 않는 것을 추천한다.</p>
<p>일단은 1번째 테스트인 ManyToOne를 한다.</p>
<p><strong>Menu</strong></p>
<pre><code class="language-java">@Entity(name=&quot;menu_section03&quot;)
@Table(name=&quot;tbl_menu&quot;)
public class Menu {

    @Id
    @Column(name=&quot;menu_code&quot;)
    private int menuCode;

    @Column(name=&quot;menu_name&quot;)
    private String menuName;

    @Column(name=&quot;menu_price&quot;)
    private int menuPrice;

    @JoinColumn(name=&quot;category_code&quot;)
    @ManyToOne
    private Category category;

    @Column(name=&quot;orderable_status&quot;)
    private String orderableStatus;

    //기본 생성자, 매개변수가 있는 생성자, getter, toString()
}</code></pre>
<p>이제 번거로움이 생기게 된다..</p>
<p>OneToMany도 Category에서 해주는 것이다.</p>
<p><strong>Category</strong></p>
<pre><code class="language-java">@Entity(name=&quot;category_section03&quot;)
@Table(name=&quot;tbl_category&quot;)
public class Category {

    @Id
    @Column(name=&quot;category_code&quot;)
    private int categoryCode;

    @Column(name=&quot;category_name&quot;)
    private String categoryName;

    @Column(name=&quot;ref_category_code&quot;)
    private Integer refCategoryCode;

    @OneToMany(mappedBy = &quot;category&quot;) // 양방향을 하기 위해서
    private List&lt;Menu&gt; menuList;</code></pre>
<p>이미 Menu에서 <code>category_code</code>와 조인을 했다는 것을 알고 보자. (Menu는 Category를 바라보는 중)</p>
<p>원래는 OneToMany에서 <code>JoinColumn</code>을 통해 조인을 했었는데 여기서는 <strong><code>OneToMany</code>에 <code>mappedBy</code>를 하여 매핑한다.</strong></p>
<p><strong>양방향 관계에서는</strong> menu에 있는 필드인 category를, <strong>즉. mappedBy에서 연관관계의 주인의 필드값을 적는 것이다.</strong></p>
<p>이제 메뉴를 조회해도 카테고리를, 카테고리를 조회해도 메뉴를 가져온다.</p>
<p>그렇게 됐을 때, 오버라이딩한 toString()으로 그 문제점을 쉽게 알 수 있는데</p>
<p><strong>BidirectionTests</strong></p>
<pre><code class="language-java">    // 이전부터 하던 기존 세팅 코드에 추가

    @Test
    public void 양방향_연관관계_매핑_조회_테스트() {
        int menuCode = 10;
        int categoryCode = 10;

        /* 설명. 연관관계의 주인인 엔티티는 한 번에 join문으로 관계를 맺은 엔티티를 조회해 온다. */
        Menu foundMenu = em.find(Menu.class, menuCode);

        /* 설명. 양방향에서 부모에 해당하는 엔티티는 가짜 연관관계이고, 필요 시 연관관계 엔티티를 조회하는 쿼리를 다시 실행하게 된다. (N + 1 문제 야기) */
         System.out.println(&quot;foundMenu = &quot; + foundMenu);
    }</code></pre>
<p>Menu나 Category 중 아무거나 EntityManager에서 find를 해도 상관없다. (어차피 둘다 조회된다.) 그 때 <code>toString()</code>이 오버라이딩된 채로 출력까지 해버리면?</p>
<p>맨처음에 쿼리는</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/aba066e5-fd15-4c16-bfc1-a12d847a636f/image.png" /></p>
<p>이렇게 잘 되는 것처럼 보이겠지만</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bcff0935-6c8c-4349-a7a7-5e9d5182a473/image.png" /></p>
<p>이런 식으로 <strong>오버플로우 오류가 난다.</strong></p>
<p>바로 메뉴를 불러서 카테고리를 부르는데 카테고리에서 또 메뉴를 부르고? 또 카테고리를 부르고? -&gt; <strong>순환 참조.. 무한으로 참조하기 때문에 생기는 오류인 것이다.</strong></p>
<hr />
<p>이렇게 간단한 예시만으로 단방향, 양방향 연관관계에 대해 알아보았다.</p>