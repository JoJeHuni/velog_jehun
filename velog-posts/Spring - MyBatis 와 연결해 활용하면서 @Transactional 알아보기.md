<h1 id="활용">활용</h1>
<h2 id="기본-세팅">기본 세팅</h2>
<p>이번엔 Spring 프로젝트로 Mybatis와 연결하려고 한다.</p>
<ul>
<li>Spring 버전 3.3.2</li>
<li>JDK 17</li>
</ul>
<p><strong>build.gradle</strong></p>
<pre><code>dependencies {
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.3'
    runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter-test:3.0.3'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}</code></pre><p><strong>application.yml</strong></p>
<pre><code class="language-yaml">spring:
  datasource:
    driver-class-name: org.mariadb.jdbc.Driver
    url: jdbc:mariadb://localhost:3306/menudb
    username: {username}
    password: {password}</code></pre>
<p>위와 같은 기본 세팅으로 시작했다.</p>
<p>이번엔 이전과 달리 <code>@Mapper</code>를 활용해서 <code>sqlSession</code>을 쓰지 않고 해볼 것이다.</p>
<hr />
<p>우선 설정파일
<strong>com.jehun.transactional.MybatisConfiguration</strong></p>
<pre><code class="language-java">@Configuration
@MapperScan(basePackages = &quot;com.jehun.transactional&quot;, annotationClass = Mapper.class)
public class MybatisConfiguration {

}</code></pre>
<p>라이브러리를 추가해줬기 때문에</p>
<pre><code>import org.apache.ibatis.annotations.Mapper;
import org.mybatis.spring.annotation.MapperScan;</code></pre><p>와 같이 import를 추가하게 된다.</p>
<hr />
<p>mapper 부터는 특별히 언급이 없다면, 같은 패키지에 속하는 클래스 or 인터페이스들이다.</p>
<p><strong>com.jehun.transactional.section01.annotation.MenuMapper</strong></p>
<pre><code class="language-java">@Mapper
public interface OrderMapper {
}</code></pre>
<hr />
<p><strong>OrderService</strong></p>
<pre><code class="language-java">@Service
public class OrderService {

    /* 설명 sqlSession.getMapper() 대신 @Mapper가 달려 하위 구현체가 관리되면 의존성 주입을 받을 수 있다.*/
    private OrderMapper orderMapper;

    @Autowired
    public OrderService(OrderMapper orderMapper) {
        this.orderMapper = orderMapper;
    }
}</code></pre>
<hr />
<p>이제 테이블들이 필요하다.
Menu, Order, 다대다 관계를 해결해줄 OrderMenu</p>
<p><strong>Menu</strong></p>
<pre><code class="language-java">public class Menu {
    private int menuCode;
    private String menuName;
    private int menuPrice;
    private int categoryCode;
    private String orderableStatus;

    // 기본 생성자
    // 매개변수를 갖는 생성자
    // getter
    // toString() 오버라이딩
}</code></pre>
<p>주석처리 된 부분은 Alt + Insert로 인텔리제이에서는 자동 추가 가능하다.</p>
<hr />
<p><strong>Order</strong></p>
<pre><code class="language-java">public class Order {
    private int orderCode;
    private String orderDate;
    private String orderTime;
    private int totalOrderPrice;

    // 기본 생성자
    // 매개변수를 갖는 생성자
    // getter
    // toString() 오버라이딩
}</code></pre>
<hr />
<p><strong>OrderMenu</strong></p>
<pre><code class="language-java">public class OrderMenu {
    private int menuCode;
    private int orderCode;
    private int orderAmount;

    // 기본 생성자
    // 매개변수를 갖는 생성자
    // getter
    // toString() 오버라이딩
}</code></pre>
<hr />
<p><strong>OrderDTO에 담겨 컨트롤러에서 넘어온다는 가정에, 주문 한 건 발생한 경우</strong>라는 가정으로 할 것이다.</p>
<p>Service 계층부터 개발할 때는 사용자가 입력한 값들이 어떻게 DTO 또는 Map으로 묶여서 Controller로 부터 넘어올지 충분히 고민한 후 매개변수를 작성하고 개발한다.</p>
<p>현재의 경우 (주문 한 건 발생한 경우) 사용자가 고른 메뉴들 각각의 코드 번호와 고른 메뉴 개수, 그리고 서버의 현재 시간이 담긴 채로 넘어왔다는 생각을 가지고 개발하자.</p>
<hr />
<p>테이블에 꼭 일치하지 않더라도 주문을 위해 사용자가 넘겨준 값을 담을 수 있는 DTO 클래스</p>
<blockquote>
<p>DTO (Data Transfer Object) : 계층을 넘어다니며 값을 옮겨주는 클래스</p>
</blockquote>
<p><strong>즉, 컨트롤러와 서비스 계층을 왔다갔다 해주는 것이 DTO</strong>이다.</p>
<p>(DTO는 setter도 만들었다.)</p>
<p><strong>OrderDTO</strong></p>
<pre><code class="language-java">public class OrderDTO {
    // 서버의 현재 시간을 2개로 나눔
    private LocalDate orderDate;
    private LocalTime orderTime;
    private List&lt;OrderMenuDTO&gt; orderMenus;

    // 기본 생성자
    // 매개변수를 갖는 생성자
    // getter
    // setter
    // toString() 오버라이딩
}</code></pre>
<p><strong>OrderMenuDTO</strong></p>
<pre><code class="language-java">public class OrderMenuDTO {
    private int menuCode;       // 고른 메뉴 번호
    private int orderAmount;    // 고른 메뉴의 개수

    // 기본 생성자
    // 매개변수를 갖는 생성자
    // getter
    // toString() 오버라이딩
}</code></pre>
<p>메뉴 + 주문한 개수를 담는 <code>OrderMenuDTO</code>를 여러 개 담고 있는 <code>OrderDTO</code></p>
<p>기본적인 세팅은 끝났다.</p>
<hr />
<p>이제부터 서비스부터 기능을 만들어보자.</p>
<h2 id="기능">기능</h2>
<ol>
<li><p>주문한 메뉴 코드 추출 (DTO)</p>
</li>
<li><p>메뉴 별로 <code>Menu</code> Entity에 담아서 조회(Select)해 오기 (부가적인 메뉴의 정보 추출(단가 등))</p>
</li>
<li><p>이 주문건에 대한 주문 총 합계를 계산한다. (Insert 한 번으로 처리하기 위해)</p>
</li>
</ol>
<ul>
<li>위 과정을 하면 tbl_order 테이블에 추가(Insert)가 가능하다.<ul>
<li>tbl_order_menu 에도 주문한 메뉴 개수만큼 주문한 메뉴를 추가(Insert)할 수 있다.</li>
</ul>
</li>
</ul>
<h3 id="1번">1번</h3>
<p><strong>OrderService 에 추가</strong> -&gt; (1번 과정)</p>
<pre><code class="language-java">public void registerNewOrder(OrderDTO orderInfo) {
        /* 설명. 1. 주문한 메뉴 코드 추출 (DTO) */
        List&lt;Integer&gt; menuCode = new ArrayList&lt;&gt;();
        for (OrderMenuDTO orderMenuDTO : orderInfo.getOrderMenus()) {
            Integer code = orderMenuDTO.getMenuCode();
            menuCode.add(code);
        }

        /* stream 방식
        List&lt;Integer&gt; menuCode = orderInfo.getOrderMenus()
                .stream()
                .map(OrderMenuDTO::getMenuCode)
                .collect(Collectors.toList());
        */
    }</code></pre>
<p>한 번 1번 과정을 test 코드에서 해보자.</p>
<blockquote>
<p>ctrl + shift + T 를 누르면 자동으로 클래스를 만들어준다.</p>
</blockquote>
<hr />
<p><strong>OrderServiceTests</strong></p>
<pre><code class="language-java">@SpringBootTest
class OrderServiceTests {

    @Autowired
    private OrderService registerOrder;

    private static Stream&lt;Arguments&gt; getOrderInfo() {
        OrderDTO orderInfo = new OrderDTO();
        orderInfo.setOrderDate(LocalDate.now());
        orderInfo.setOrderTime(LocalTime.now());

        orderInfo.setOrderMenus(
                List.of(
                        new OrderMenuDTO(3, 10),
                        new OrderMenuDTO(4, 5)
                )
        );

        return Stream.of(
                Arguments.of(orderInfo)
        );
    }

    @DisplayName(&quot;주문 등록 테스트&quot;)
    @ParameterizedTest
    @MethodSource(&quot;getOrderInfo&quot;)
    void testRegisterNewOrder(OrderDTO orderInfo) {
        Assertions.assertDoesNotThrow(
                () -&gt; registerOrder.registerNewOrder(orderInfo)
        );
    }
}</code></pre>
<p>잘 되고 있다.</p>
<hr />
<h3 id="2번">2번</h3>
<p>이번엔 2번 코드를 구현해보자.</p>
<p>1번 과정에서 나온 List를 map으로 묶은 뒤에 할 것이다.</p>
<p><strong>MenuService에 추가</strong></p>
<pre><code class="language-java">        Map&lt;String, List&lt;Integer&gt;&gt; map = new HashMap&lt;&gt;();
        map.put(&quot;menuCodes&quot;, menuCode);

        /* 설명. 2. 주문한 메뉴 별로 Menu Entity에 담아서 조회(Select)해 오기 (부가적인 메뉴의 정보 추출(단가 등)) */
        List&lt;Menu&gt; menus = menuMapper.selectMenuByMenuCodes(map);</code></pre>
<p>보면 menuMapper로 인해 새로운 MenuMapper 인터페이스가 필요하다.</p>
<p><strong>MenuMapper</strong></p>
<pre><code class="language-java">@Mapper
public interface MenuMapper {
    List&lt;Menu&gt; selectMenuByMenuCodes(Map&lt;String, List&lt;Integer&gt;&gt; map);
}</code></pre>
<hr />
<p>위의 인터페이스 생성으로 인해 OrderService에는 생성자에 필드와 매개 변수가 추가된다.</p>
<p><strong>OrderService 수정</strong></p>
<pre><code class="language-java">    private OrderMapper orderMapper;
    private MenuMapper menuMapper;

    @Autowired
    public OrderService(OrderMapper orderMapper, MenuMapper menuMapper) {
        this.orderMapper = orderMapper;
        this.menuMapper = menuMapper;
    }</code></pre>
<hr />
<p>Mapper가 생성됐으니 이제 resources 에도 xml 파일을 추가해줘야 한다.</p>
<pre><code>com/jehun/transactional/section01/annotation/MenuMapper.xml
com/jehun/transactional/section01/annotation/OrderMapper.xml</code></pre><p>둘다 추가해주자.</p>
<hr />
<p><strong>OrderMapper.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;com.jehun.transactional.section01.annotation.OrderMapper&quot;&gt;

&lt;/mapper&gt;</code></pre>
<p><strong>MenuMapper.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot;
        &quot;https://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;com.jehun.transactional.section01.annotation.MenuMapper&quot;&gt;

&lt;/mapper&gt;</code></pre>
<hr />
<p>이제 이전에 배웠던 동적 쿼리를 사용할 수 있게 됐다. 해보자.</p>
<p><strong>MenuMapper.xml 에 추가</strong></p>
<pre><code class="language-xml">    &lt;resultMap id=&quot;menuResultMap&quot; type=&quot;com.jehun.transactional.section01.annotation.Menu&quot;&gt;
        &lt;id property=&quot;menuCode&quot; column=&quot;MENU_CODE&quot;/&gt;
        &lt;result property=&quot;menuName&quot; column=&quot;MENU_NAME&quot;/&gt;
        &lt;result property=&quot;menuPrice&quot; column=&quot;MENU_PRICE&quot;/&gt;
        &lt;result property=&quot;categoryCode&quot; column=&quot;CATEGORY_CODE&quot;/&gt;
        &lt;result property=&quot;orderableStatus&quot; column=&quot;ORDERABLE_STATUS&quot;/&gt;
    &lt;/resultMap&gt;</code></pre>
<hr />
<p><code>resultMap</code>을 해뒀으니 <code>selectMenuByMenuCodes</code> 의 쿼리를 작성해보자.</p>
<p><strong>MenuMapper.xml에 추가</strong></p>
<pre><code class="language-xml">    &lt;select id=&quot;selectMenuByMenuCodes&quot; resultMap=&quot;menuResultMap&quot; parameterType=&quot;hashmap&quot;&gt;
        SELECT
               A.MENU_CODE
             , A.MENU_NAME
             , A.MENU_PRICE
             , A.CATEGORY_CODE
             , A.ORDERABLE_STATUS
          FROM TBL_MENU a
         WHERE A.MENU_CODE IN
        &lt;foreach collection=&quot;menuCodes&quot; item=&quot;menuCode&quot; open=&quot;(&quot; close=&quot;)&quot; separator=&quot;, &quot;&gt;
            #{ menuCode }
        &lt;/foreach&gt;
    &lt;/select&gt;</code></pre>
<hr />
<p>동적 쿼리를 활용해보았고, 로그도 함께 봐보자.</p>
<blockquote>
<p>참고 : <strong>로그를 보기 위한 세팅</strong>
<strong>log4jdbc.log4j2.properties</strong></p>
<pre><code class="language-properties">log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0</code></pre>
</blockquote>
<pre><code>&gt; **log4jj2.xml**
&gt; ```xml
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;configuration debug=&quot;true&quot;&gt;
    &lt;!-- https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte3:fdl:logging:log4j_2:%EC%84%A4%EC%A0%95_%ED%8C%8C%EC%9D%BC%EC%9D%84_%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94_%EB%B0%A9%EB%B2%95 --&gt;
    &lt;!-- Appenders --&gt;
    &lt;appender name=&quot;console&quot; class=&quot;ch.qos.logback.core.ConsoleAppender&quot;&gt;
        &lt;encoder&gt;
            &lt;charset&gt;UTF-8&lt;/charset&gt;
            &lt;Pattern&gt;%d %5p [%c] %m%n %r&lt;/Pattern&gt;
        &lt;/encoder&gt;
    &lt;/appender&gt;
&gt;
    &lt;!-- info부터 적용 table사용시 --&gt;
    &lt;logger name=&quot;jdbc.resultsettable&quot; additivity=&quot;false&quot;&gt;
        &lt;level value=&quot;info&quot; /&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/logger&gt;
&gt;
    &lt;!-- sql 로깅 --&gt;
    &lt;!-- SQL문이 preparedStatement일 경우 argument값으로 대체된 SQL문 출력 --&gt;
    &lt;logger name=&quot;jdbc.sqlonly&quot; additivity=&quot;false&quot;&gt;
        &lt;level value=&quot;info&quot; /&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/logger&gt;
&gt;
    &lt;!-- resultset제외한 모든 jdbc 호출 정보 출력 --&gt;
    &lt;logger name=&quot;jdbc.audit&quot; additivity=&quot;false&quot;&gt;
        &lt;level value=&quot;info&quot; /&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/logger&gt;
&gt;
    &lt;!-- info부터 적용 기본적인 resultset--&gt;
    &lt;!-- resultset을 포함한 모든 jdbc 호출 정보 로그 --&gt;
    &lt;!-- resultsettable 만 사용하기 위해서 로그레벨은 warning시킨다. resultset이 중복됨 --&gt;
    &lt;logger name=&quot;jdbc.resultset&quot; additivity=&quot;false&quot;&gt;
        &lt;level value=&quot;info&quot; /&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/logger&gt;
&gt;
    &lt;!-- 수행시간을 찍는다--&gt;
    &lt;logger name=&quot;jdbc.sqltiming&quot; additivity=&quot;false&quot;&gt;
        &lt;level value=&quot;info&quot; /&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/logger&gt;
&gt;
    &lt;!-- Root Logger --&gt;
    &lt;root level=&quot;off&quot;&gt;
        &lt;appender-ref ref=&quot;console&quot; /&gt;
    &lt;/root&gt;
&lt;/configuration&gt;</code></pre><p><strong>application.yml</strong>도 수정해줘야 한다.</p>
<pre><code class="language-yaml">spring:
  datasource:
    driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
    url: jdbc:log4jdbc:mariadb://localhost:3306/menudb
    username: {username}
    password: {password}

logging:
  level:
    #    root: info
    com:
      jehun:
        transactional:
          section01:
            annotation: debug</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0a2a6bd0-43cb-4a8c-8bc6-d107bebb8e2b/image.png" /></p>
<p>테스트도 로그도 잘 된다.</p>
<hr />
<h3 id="3번">3번</h3>
<p>다음은 3번 총 합계 금액 계산을 기능 구현을 해보자.</p>
<p><strong>OrderService</strong>의 <code>registerNewOrder()</code> 에 추가</p>
<pre><code class="language-java">/* 설명. 3. 이 주문건에 대한 주문 총 합계를 계산 (Insert 한 번으로 처리하기 위해) */
        int totalOrderPrice = calculateTotalOrderPrice(orderInfo.getOrderMenus(), menus);</code></pre>
<blockquote>
<p><strong>calculateTotalOrderPrice</strong> 메소드 : 주문 건에 대한 총 합계 금액 계산 메소드 
(orderMenus : 사용자의 주문 내용, menus : 조회된 메뉴 전체 내용)</p>
</blockquote>
<p>이것도 추가해보자.</p>
<p><strong>OrderService</strong>의 <code>calculateTotalOrderPrice()</code> 메소드</p>
<pre><code class="language-java">private int calculateTotalOrderPrice(List&lt;OrderMenuDTO&gt; orderMenus, List&lt;Menu&gt; menus) {
        int totalOrderPrice = 0;

        int orderMenuSize = orderMenus.size();

        for (int i = 0; i &lt; orderMenuSize; i++) {
            OrderMenuDTO orderMenu = orderMenus.get(i);
            Menu menu = menus.get(i);

            totalOrderPrice += menu.getMenuPrice() * orderMenu.getOrderAmount();
        }

        return totalOrderPrice;
    }</code></pre>
<p><code>System.out.println(&quot;totalOrderPrice = &quot; + totalOrderPrice);</code> 로 출력해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/40a4f0ad-76e1-46f8-bca2-04d215e8c790/image.png" /></p>
<p>테스트에서 로 10개, 5개, 20개를 받았는데 가격 계산을 해보면 알맞다.</p>
<pre><code>new OrderMenuDTO(3, 10), // 300원 10개 3000원
new OrderMenuDTO(4, 5),  // 7000원 5개 35000원
new OrderMenuDTO(10, 20) // 7000원 20개 140000원</code></pre><hr />
<h3 id="4번">4번</h3>
<p>이제 tbl_order 테이블에 insert가 가능하다.</p>
<ol>
<li>insert를 하기 위해 테이블과 매칭된느 Entity 클래스(Order)로 옮겨 담는다. (DTO -&gt; Entity)</li>
</ol>
<p><strong>OrderService 에 추가</strong></p>
<pre><code class="language-java">        List&lt;OrderMenu&gt; orderMenus = new ArrayList&lt;&gt;(
                orderInfo.getOrderMenus().stream()
                        .map(dto -&gt; {
                            return new OrderMenu(dto.getMenuCode(), dto.getOrderAmount());
                        }).collect(Collectors.toList())
        );</code></pre>
<blockquote>
<p><strong>for-each로 나타내는 방법</strong></p>
</blockquote>
<pre><code class="language-java">         List&lt;OrderMenu&gt; orderMenus = new ArrayList&lt;&gt;();
        List&lt;OrderMenuDTO&gt; orderMenuDTOs = orderInfo.getOrderMenus();
        for (OrderMenuDTO orderMenuDTO : orderMenuDTOs) {
            orderMenus.add(new OrderMenu(orderMenuDTO.getMenuCode(), orderMenuDTO.getOrderAmount()));
        }</code></pre>
<hr />
<p>여기서 이제 <code>menuCode</code>, <code>orderAmount</code> 로 된 생성자는 <code>OrderMenu</code>에 없기 때문에</p>
<p><strong>OrderMenu에도 추가</strong></p>
<pre><code class="language-java">    public OrderMenu(int orderAmount, int menuCode) {
        this.orderAmount = orderAmount;
        this.menuCode = menuCode;
    }</code></pre>
<hr />
<p>OrderDTO를 Order로 바꿔주자.</p>
<p><strong>OrderService에 추가</strong></p>
<pre><code class="language-java">        Order order = new Order(orderInfo.getOrderDate(), orderInfo.getOrderTime(), totalOrderPrice);
</code></pre>
<p>여기서 Order도 생성자가 저 3가지 매개변수로 이뤄지지 않은데..
String으로 해뒀기 때문에 변환 작업이 필요하다.</p>
<p><strong>Order에 생성자 추가</strong></p>
<pre><code class="language-java">    public Order(LocalDate orderDate, LocalTime orderTime, int totalOrderPrice) {
        /* 설명. LocalDate 또는 LocalTime 형을 DB에 맞춰서 저장하기 위한 변환작업 */
        this.orderDate = orderDate.format(DateTimeFormatter.ofPattern(&quot;yyyyMMdd&quot;));
        this.orderTime = orderTime.format(DateTimeFormatter.ofPattern(&quot;HH:mm:ss&quot;));
        this.totalOrderPrice = totalOrderPrice;
    }</code></pre>
<hr />
<p>이제 tbl_order 테이블에 insert 하기 위해서 Mapper를 사용해야 한다.</p>
<p><strong>OrderService에 추가</strong></p>
<pre><code class="language-java">        orderMapper.registerOrder(order);</code></pre>
<p><strong>OrderMapper에도 추가</strong></p>
<pre><code class="language-java">public interface OrderMapper {
    void registerOrder(Order order);
}</code></pre>
<p>이제 sql문을 추가하자. <strong>OrderMapper.xml</strong></p>
<pre><code class="language-xml">    &lt;insert id=&quot;registerOrder&quot;&gt;
        INSERT
          INTO TBL_ORDER
        (
          ORDER_DATE
        , ORDER_TIME
        , TOTAL_ORDER_PRICE
        )
        VALUES
        (
          #{ orderDate }
        , #{ orderTime }
        , #{ totalOrderPrice }
        )

        &lt;!--  insert 가 끝나면 int 값이 들어가는 것이 당연하지만, 추가로 조회 결과 (현재는 insert 당시 pk 값) 를 가지고 돌아갈 수 있게 해주는 selectKey--&gt;
        &lt;selectKey keyProperty=&quot;orderCode&quot; order=&quot;AFTER&quot; resultType=&quot;_int&quot;&gt;
            SELECT MAX(ORDER_CODE) FROM TBL_ORDER
        &lt;/selectKey&gt;
    &lt;/insert&gt;</code></pre>
<hr />
<p>이제 4번까지 완료했다.</p>
<p><strong>OrderService</strong> 에 <code>System.out.println(&quot;order insert 후 order에 담긴 pk값 확인 = &quot; + order);</code> 을 추가해서 pk값이 조회되는지 확인해보자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ad4ec48c-90a1-4395-9f81-cf589dd039ef/image.png" /></p>
<p>파랑 상자를 보면 <strong>insert 후 selectKey로 조회된 pk값이 담겨 돌아오는 것을 알 수 있다.</strong></p>
<hr />
<h3 id="5번">5번</h3>
<blockquote>
<p>tbl_order_menu 에도 주문한 메뉴 개수만큼 주문한 메뉴를 추가(Insert)할 수 있다.</p>
</blockquote>
<p><strong>OrderService에 추가</strong></p>
<pre><code class="language-java">        int orderMenuSize = orderMenus.size();

        /* 설명. 주문한 메뉴들의 orderCode 들이 비어 있었으니 주문 insert 후 알게 된 pk를 채워넣는다. */
        for (int i = 0; i &lt; orderMenuSize; i++) {
            OrderMenu orderMenu = orderMenus.get(i);
            orderMenu.setOrderCode(order.getOrderCode()); // Order에 setter 하나 추가

            orderMapper.registerOrderMenu(orderMenu);
        }</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7a09da5b-a00c-4ae5-9ae8-69bacf978d68/image.png" /></p>
<p><code>OrderMenu</code>를 보면 4번에서 <code>Order</code>에 <code>selectKey</code>를 활용했기 때문에 <code>orderCode</code>도 받아와진다.</p>
<p>마지막으로 <code>registerOrderMenu</code> 의 이름으로 된 쿼리를 만들어주면 끝난다.</p>
<p><strong>OrderMapper에 추가</strong></p>
<pre><code class="language-java">    void registerOrderMenu(OrderMenu orderMenu);</code></pre>
<p><strong>OrderMapper.xml에 추가</strong></p>
<pre><code class="language-xml">    &lt;insert id=&quot;registerOrderMenu&quot; parameterType=&quot;com.jehun.transactional.section01.annotation.OrderMenu&quot;&gt;
        INSERT
          INTO TBL_ORDER_MENU
        (
          ORDER_CODE
        , MENU_CODE
        , ORDER_AMOUNT
        )
        VALUES
        (
          #{ orderCode }
        , #{ menuCode }
        , #{ orderAmount }
        )
    &lt;/insert&gt;</code></pre>
<hr />
<h1 id="transactional-사용">@Transactional 사용</h1>
<p>그대로 테스트 코드에서 실행하면 db에 적용이 된다.</p>
<p>근데 실패를 해도 됐던 곳까지는 계속 적용이 되는데, 그것을 방지하기 위해</p>
<p><code>OrderService</code> -&gt; <code>registerNewOrder</code> 메소드에 <code>@Transactional</code> 추가를 해주면 된다.</p>
<blockquote>
<p>참고) 원래 테스트 코드에서 하는 작업이 DB에 적용이 되면 안 된다.
그래서 활용에서는 적용이 됐지만 주의해야 한다.
해결방법 : Test 코드 클래스에 <code>@Transactional</code> 추가하기</p>
</blockquote>
<pre><code class="language-java">@SpringBootTest
@Transactional
class OrderServiceTests {
    ... (중략)</code></pre>
<ul>
<li>테스트코드에서의 <code>@Transactional</code><ul>
<li>DML(insert, update, delete) 작업 테스트 시 실제 DB 적용을 안 하기 위해 사용한다.</li>
</ul>
</li>
</ul>
<hr />
<h2 id="전파행위-옵션">전파행위 옵션</h2>
<p>A 트랜잭션, B 트랜잭션이 있다고 보자.</p>
<p>A 트랜잭션을 진행하는 도중 다른 트랜잭션(B)을 진행하기도 한다.</p>
<p>그 때의 A와 B는 다른 트랜잭션이라고 봐야 할까 같은 트랜잭션으로 봐야 할까? 에 대해 다루는 것이 전파행위 옵션이다.</p>
<ul>
<li><p><code>REQUIRED</code> : <strong>진행 중인 트랜잭션이 있으면</strong> 현재 메소드를 <strong>그 트랜잭션에서 실행</strong>하되 그렇지 않은 경우 <strong>(진행 중인 것이 없는 경우) 새 트랜잭션을 시작해서 실행</strong>한다.</p>
</li>
<li><p><code>REQUIRED_NEW</code> : <strong>항상 새 트랜잭션을 시작</strong>해 메소드를 실행하고 진행중인 트랜잭션이 있으면 잠시 중단시킨다.</p>
</li>
<li><p><code>SUPPORTS</code> : <strong>진행 중인 트랜잭션이 있으면 현재 메소드를 그 트랜잭션 내에서 실행</strong>하되, 그렇지 않을 경우 <strong>(진행 중인 트랜잭션이 없으면) 트랜잭션 없이 실행</strong>한다.</p>
</li>
<li><p><code>NOT_SUPPORTED</code> : 트랜잭션 <strong>없이 현재 메소드를 실행</strong>하고 <strong>진행 중인 트랜잭션이 있으면 잠시 중단</strong>한다</p>
</li>
<li><p><code>MANDATORY</code> : 반드시 트랜잭션을 걸고 현재 메소드를 실행하되 진행 중인 트랜잭션이 있으면 예외를 던진다.</p>
</li>
<li><p><code>NEVER</code> : <strong>반드시 트랜잭션 없이 현재 메소드를 실행하되 진행 중인 트랜잭션이 있으면 예외</strong>를 던진다.</p>
</li>
<li><p><code>NESTED</code> : <strong>진행중인 트랜잭션이 있으면 현재 메소드를 이 트랜잭션의 중첩 트랜잭션 내에서 실행</strong>한다. </p>
<ul>
<li>진행 중인 트랜잭션이 없으면 새 트랜잭션을 실행한다.</li>
<li>배치 실행 도중 처리 할 업무가 100 만개라고 하면 10만개씩 끊어서 커밋하는 경우, 중간에 잘못 되어도 중첩 트랜잭션을 롤백하면 전체가 아닌 10만개만 롤백된다.</li>
<li>세이브포인트를 이용하는 방식이다. 따라서 세이브포인트를 지원하지 않는 경우 사용 불가능하다.</li>
</ul>
</li>
</ul>
<hr />
<h2 id="격리-수준">격리 수준</h2>
<p><a href="https://velog.io/@jojehuni_9759/SQL-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EA%B3%BC-%EC%9E%A0%EA%B8%88#mysql%EC%9D%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80">mysql의-격리-수준</a></p>
<p>여기서 같이 보면 괜찮을 것이다.</p>
<hr />
<h2 id="오염된-값">오염된 값</h2>
<p><strong>오염된 값</strong> : 하나의 트랜젝션이 데이터를 변경 후 잠시 기다리는 동안 다른 트랜젝션이 데이터를 읽게 되면, 격리레벨이 <code>READ_UNCOMMITTED</code>인 경우 아직 변경 후 커밋하지 않은 재고값을 그대로 읽게 된다</p>
<p>그러나 처음 트랜젝션이 데이터를 롤백하게 되면 다른 트랜젝션이 읽은 값은 더 이상 유효하지 않은 일시적인 값이 된다. </p>
<p>이것을 오염된 값라고 한다.</p>
<hr />
<h2 id="재현-불가능한-값-읽기">재현 불가능한 값 읽기</h2>
<p>재현 불가능한 값 읽기 : 처음 트랜젝션이 데이터를 수정하면 수정이 되고 아직 커밋되지 않은 로우에 수정 잠금을 걸어둔 상태에다. </p>
<p>결국 다른 트랜젝션은 이 트랜젝션이 커밋 혹은 롤백 되고 수정잠금이 풀릴 때까지 기다렸다가 읽을 수 밖에 없게 된다. </p>
<p>하지만 다른 로우에 대해서는 또 다른 트랜젝션이 데이터를 수정하고 커밋을 하게 되면 가장 처음 동작한 트랜젝션이 데이터를 커밋하고 다시 조회를 한 경우 처음 읽은 그 값이 아니게 된다. </p>
<p>이것이 재현 불가능한 값이라고 한다.</p>
<hr />
<h2 id="허상-읽기">허상 읽기</h2>
<p>허상 읽기 : 처음 트랜젝션이 테이블에서 여러 로우를 읽은 후 이후 트랜젝션이 같은 테이블의 로우를 추가하는 경우 처음 트랜젝션이 같은 테이블을 다시 읽으면 자신이 처음 읽었을 때와 달리 새로 추가 된 로우가 있을 것이다. </p>
<p>이것을 허상 읽기라고 한다. (재현 불가능한 값 읽기와 유사하지만 허상 읽기는 여러 로우가 추가되는 경우를 말한다.)</p>