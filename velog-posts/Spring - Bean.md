<p>쇼핑카트에 제품들을 넣는 것으로 Bean에 대한 인스턴스의 범위도 알아보려고 한다.</p>
<p>일단 이 게시글에서 공통적으로 사용할 클래스들은 <code>Product</code> , <code>Beverage</code> , <code>Bread</code> , <code>ShoppingCart</code> 4개 클래스이다.</p>
<p><strong>Product</strong></p>
<pre><code class="language-java">import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;
import lombok.ToString;

@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Product {
    private String name;
    private int price;
}</code></pre>
<p><strong>Beverage</strong></p>
<pre><code class="language-java">import lombok.ToString;

@ToString
public class Beverage extends Product{
    private int capacity;

    public Beverage() {
    }

    public Beverage(String name, int price, int capacity) {
        super(name, price);
        this.capacity = capacity;
    }
}</code></pre>
<p><strong>Bread</strong></p>
<pre><code class="language-java">import lombok.ToString;

import java.util.Date;

@ToString
public class Bread extends Product{
    private java.util.Date bakedDate;

    public Bread() {
    }

    public Bread(String name, int price, Date bakedDate) {
        super(name, price);
        this.bakedDate = bakedDate;
    }
}</code></pre>
<p><strong>ShoppingCart</strong></p>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.List;

public class ShoppingCart {

    private final List&lt;Product&gt; items;

    public ShoppingCart() {
        items = new ArrayList&lt;&gt;();
    }

    public void addItem(Product item) {
        items.add(item);
    }

    public List&lt;Product&gt; getItems() {
        return items;
    }

}</code></pre>
<p>롬복을 사용했기때문에 build.gradle에 대한 것도 남겨두겠다.</p>
<pre><code>dependencies {
    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'

    // https://mvnrepository.com/artifact/org.springframework/spring-context
    implementation 'org.springframework:spring-context:6.1.11'

    // https://mvnrepository.com/artifact/org.projectlombok/lombok
    implementation 'org.projectlombok:lombok:1.18.24'
    annotationProcessor 'org.projectlombok:lombok:1.18.24'
}</code></pre><h2 id="bean-scope">Bean Scope</h2>
<blockquote>
<p><code>bean scope</code>란 스프링 빈이 생성될 때 생성되는 인스턴스의 범위를 의미한다.</p>
</blockquote>
<table>
<thead>
<tr>
<th>Bean Scope</th>
<th>Description</th>
</tr>
</thead>
<tbody><tr>
<td>Singleton</td>
<td>하나의 인스턴스만을 생성하고, 모든 빈이 해당 인스턴스를 공유하여 사용한다.</td>
</tr>
<tr>
<td>Prototype</td>
<td>매번 새로운 인스턴스를 생성한다.</td>
</tr>
<tr>
<td>Request</td>
<td>HTTP 요청을 처리할 때마다 새로운 인스턴스를 생성하고, 요청 처리가 끝나면 인스턴스를 폐기한다. 웹 애플리케이션 컨텍스트에만 해당된다.</td>
</tr>
<tr>
<td>Session</td>
<td>HTTP 세션 당 하나의 인스턴스를 생성하고, 세션이 종료되면 인스턴스를 폐기한다. 웹 애플리케이션 컨텍스트에만 해당된다.</td>
</tr>
</tbody></table>
<hr />
<h3 id="singleton">singleton</h3>
<blockquote>
<p>Singleton은 애플리케이션 내에서 하나의 인스턴스만 사용, 모든 빈이 해당 인스턴스를 공유하며 사용한다.</p>
</blockquote>
<ul>
<li>장점 : 메모리 사용량 감소, 성능 향상</li>
</ul>
<p><strong>ContextConfiguration</strong></p>
<pre><code class="language-java">import com.jehun.common.Beverage;
import com.jehun.common.Bread;
import com.jehun.common.Product;
import com.jehun.common.ShoppingCart;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class ContextConfiguration {

    @Bean
    public Product carpBread() {
        return new Bread(&quot;붕어빵&quot;, 1000, new java.util.Date());
    }

    @Bean
    public Product milk() {
        return new Beverage(&quot;딸기우유&quot;, 1500, 500);
    }

    @Bean
    public Product water() {
        return new Beverage(&quot;쌈디수&quot;, 3000, 1000);
    }

    @Bean
    @Scope(&quot;singleton&quot;) // 기본값
    public ShoppingCart cart() {
        return new ShoppingCart();
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section01.scope.singleton;

import com.ohgiraffers.common.Beverage;
import com.ohgiraffers.common.Bread;
import com.ohgiraffers.common.Product;
import com.ohgiraffers.common.ShoppingCart;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        /* 설명. 빈 설정 파일을 기반으로 IoC 컨테이너 생성 */
        ApplicationContext context = new AnnotationConfigApplicationContext(ContextConfiguration.class);

        /* 설명. 붕어빵, 딸기우유, 쌈디수 등의 빈 객체를 반환 받는다. */
        Product carpBread = context.getBean(&quot;carpBread&quot;, Bread.class);
        Product milk = context.getBean(&quot;milk&quot;, Beverage.class);
        Product water = context.getBean(&quot;water&quot;, Beverage.class);

        /* 설명. 첫 번째 손님이 쇼핑 카트를 꺼내 물건을 담는다. */
        ShoppingCart cart1 = context.getBean(&quot;cart&quot;, ShoppingCart.class);
        cart1.addItem(carpBread);
        cart1.addItem(milk);

        /* 설명. 붕어빵과 쌈디수가 담겨있다. */
        System.out.println(&quot;cart1에 담긴 내용 : &quot; + cart1.getItems());

        /* 설명. 두 번째 손님이 쇼핑 카트를 꺼내 물건을 담는다. */
        ShoppingCart cart2 = context.getBean(&quot;cart&quot;, ShoppingCart.class);
        cart2.addItem(water);

        /* 설명. 붕어빵과 딸기우유와 쌈디수가 담겨있다. */
        System.out.println(&quot;cart2에 담긴 내용 : &quot; + cart2.getItems());

        /* 설명. 두 카드의 hashcode를 출력해보면 동일한 것을 볼 수 있다. */
        System.out.println(&quot;cart1의 hashcode : &quot; + cart1.hashCode());
        System.out.println(&quot;cart2의 hashcode : &quot; + cart2.hashCode());
    }
}</code></pre>
<p><code>Application</code> 에서 손님 2명이 각각 쇼핑 카트를 활용해 상품을 담는다고 했지만, <code>Singleton</code>으로 관리되는 <code>cart</code>는 원래 하나의 객체이므로 동일한 카트에 물건을 담는 상황이 발생.</p>
<p>-&gt; 기본값이 <code>singleton</code> 스코프가 아닌 <code>prototype</code> 스코프가 필요할 수도 있다.</p>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8890d530-dce1-481d-a548-36bb56a58ac4/image.png" /></p>
<hr />
<h3 id="prototype">prototype</h3>
<blockquote>
<p><code>prototype</code> 스코프를 갖는 Bean은 매번 새로운 인스턴스를 생성한다</p>
</blockquote>
<ul>
<li>장점 : 객체 생성에 대한 부담을 줄일 수 있다.</li>
</ul>
<p>ContextConfiguration 에서 Scope 어노테이션만 prototype으로 바꿔주자.</p>
<pre><code class="language-java">    ...

    @Bean
    @Scope(&quot;prototype&quot;)
    public ShoppingCart cart() {
        return new ShoppingCart();
    }

    ...</code></pre>
<p>그대로 실행하면 다른 <code>getBean()</code>으로 인스턴스를 꺼내올 때마다 새로운 인스턴스를 생성하게 돼서 다르게 나온다.</p>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8712d83a-0d61-44c0-82d6-dea4c0a5cd7e/image.png" /></p>
<hr />
<h2 id="init-destroy-method">init, destroy method</h2>
<p>스프링 빈은 <code>초기화(init)</code>와 <code>소멸화(destroy)</code>의 라이프 사이클을 가지고 있다
객체가 생성되고 소멸될 때 추가적인 작업을 할 수 있다.</p>
<p>위의 클래스에서 시작할 때 Owner(사장)가 문을 열고, 끝날 때 닫는 것을 추가하려고 한다.</p>
<p><strong>Owner</strong></p>
<pre><code class="language-java">public class Owner {
    public void openShop() {
        System.out.println(&quot;사장님이 가게 문을 열었습니다. 이제 쇼핑을 하실 수 있습니다.&quot;);
    }

    public void closeShop() {
        System.out.println(&quot; 사장님이 가게 문을 닫았습니다. 이제 쇼핑을 하실 수 없습니다.&quot;);
    }
}</code></pre>
<p>이걸 이제 ContextConfiguration에서도 init, destory 메소드를 활용할 수 있다.</p>
<p><strong>ContextConfiguration</strong></p>
<pre><code class="language-java">public class ContextConfiguration {
    // ...기존

    /* 설명. Owner 빈 추가 */
    @Bean(initMethod = &quot;openShop&quot;, destroyMethod = &quot;closeShop&quot;)
    public Owner owner() {
        return new Owner();
    }
}</code></pre>
<p>Application 에서도 종료하는 메소드를 추가해줘야 한다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        // 기존의 코드
        ((AnnotationConfigApplicationContext)context).close();
    }
}</code></pre>
<p><code>destroy</code> 메소드는 <strong>Bean 객체 소멸 시점에 동작</strong>하므로 컨테이너가 종료 되지 않을 경우 확인할 수 없다.
가비지 컬렉터가 해당 빈을 메모리에서 지울 때 <code>destroy</code> 메소드가 동작하게 되는데 메모리에서 지우기 전에 프로세스는 종료되기 때문이다.
따라서 아래와 같이 강제로 컨테이너를 종료시키면 <code>destroy</code> 메소드가 동작할 것이다.</p>
<p><strong>실행 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/bf064adc-90a0-4a47-8091-058aed072e0a/image.png" /></p>
<hr />
<h2 id="properties">Properties</h2>
<blockquote>
<p><code>Properties</code> : 키/값 쌍으로 이루어진 간단한 파일이다. 보통 소프트웨어 설정 정보를 저장할 때 사용된다.</p>
</blockquote>
<p>Properties 파일의 각 줄은 다음과 같은 형식으로 구성된다. 주석은 #으로 시작하며, 빈 줄은 무시된다.</p>
<pre><code># 주석
key=value</code></pre><p>Properties 파일이 있고 yml파일이라는 것도 있으니 알아두면 좋다
지금은 properties를 만드려고 한다.</p>
<p>resources 폴더 하위 경로에 넣으면 된다.
<code>section03/properties/subsection01/properties/product-info.properties</code></p>
<p><strong>product-info.properties</strong></p>
<pre><code>bread.carpbread.name=붕어빵
bread.carpbread.price=1000
beverage.milk.name=딸기우유
beverage.milk.price=1500
beverage.milk.capacity=500
beverage.water.name=지리산암반수
beverage.water.price=3000
beverage.water.capacity=500</code></pre><p>위처럼 파일을 만들어주는데 <code>@PropertySource</code> 어노테이션으로 경로를 적어서 사용하고 싶은 곳에서 읽어올 수도 있다.</p>
<p>그리고 <code>@Value</code> 어노테이션을 이용해서 properties의 값을 읽어올 수 있다.</p>
<p><strong>ContextConfiguration</strong></p>
<pre><code class="language-java">import com.jehun.common.Beverage;
import com.jehun.common.Bread;
import com.jehun.common.Product;
import com.jehun.common.ShoppingCart;
import com.jehun.section02.initdestory.Owner;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.annotation.Scope;

@Configuration
@PropertySource(&quot;section03/properties/subsection01/properties/product-info.properties&quot;)
public class ContextConfiguration {

    /* 추가적인 팁 : 리터럴 변수
     * #{}
     */

    // 바인딩 변수
     @Value(&quot;${bread.carpbread.name}&quot;)
    private String carpBreadName;

    @Value(&quot;${bread.carpbread.price}&quot;)
    private int carpBreadPrice;

    @Value(&quot;${beverage.milk.name}&quot;)
    private String beverageMilkName;

    @Value(&quot;${beverage.milk.price}&quot;)
    private int beverageMilkPrice;

    @Value(&quot;${beverage.milk.capacity}&quot;)
    private int beverageMilkCapacity;

    @Value(&quot;${beverage.water.name}&quot;)
    private String beverageWaterName;

    @Value(&quot;${beverage.water.price}&quot;)
    private int beverageWaterPrice;

    @Value(&quot;${beverage.water.capacity}&quot;)
    private int beverageWaterCapacity;

    // 기존 코드와 동일
}</code></pre>
<p><code>@Value</code> 어노테이션 안에는 key 값을 넣어주면 된다. (중괄호 주의..)</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import com.jehun.common.Beverage;
import com.jehun.common.Bread;
import com.jehun.common.Product;
import com.jehun.common.ShoppingCart;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(ContextConfiguration.class);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName : beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        Product carpBread = context.getBean(&quot;carpBread&quot;, Bread.class);
        Product milk = context.getBean(&quot;milk&quot;, Beverage.class);
        Product water = context.getBean(&quot;water&quot;, Beverage.class);

        ShoppingCart cart1 = context.getBean(&quot;cart&quot;, ShoppingCart.class);
        cart1.addItem(carpBread);
        cart1.addItem(milk);

        System.out.println(&quot;cart1에 담긴 내용 : &quot; + cart1.getItems());

        ShoppingCart cart2 = context.getBean(&quot;cart&quot;, ShoppingCart.class);
        cart2.addItem(water);

        System.out.println(&quot;cart2에 담긴 내용 : &quot; + cart2.getItems());

        System.out.println(&quot;cart1의 hashcode : &quot; + cart1.hashCode());
        System.out.println(&quot;cart2의 hashcode : &quot; + cart2.hashCode());

        ((AnnotationConfigApplicationContext)context).close();
    }
}</code></pre>
<p><strong>실행결과</strong></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2a8f57f0-6690-4357-b2ae-75bb5dc50344/image.png" /></p>
<p>만약 한글이 깨진다면 <code>ctrl + alt + s</code> 로 setting에 들어가서</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c70a67dd-1121-4306-8bef-1da085f696f1/image.png" /></p>
<p>이렇게 UTF-8로 해준 뒤 properties에 ???로 된 부분을 다시 고친 뒤 실행하면 잘 될 것이다. </p>