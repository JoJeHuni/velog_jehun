<h1 id="spring-framework-구성-모듈">Spring Framework 구성 모듈</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/378101dc-8b8c-4fc3-aeab-c5b03dab2f38/image.png" /></p>
<h2 id="spring-core-container">Spring Core Container</h2>
<blockquote>
<p>스프링에서 가장 기본적이며 중요한 모듈 중 하나
Spring Core Container 모듈은 <strong>스프링에서 객체의 생성과 관리를 담당</strong>한다.
스프링의 <code>DI(Dependency Injection)</code>과 <code>IoC(Inversion of Control)</code> 개념이 구현되어 있다.
이를 통해, 코드의 <strong>재사용성</strong>과 <strong>유지보수성</strong>을 높일 수 있다.</p>
</blockquote>
<hr />
<p>Context를 사용해서 개발방식에 대해 알아보자.</p>
<h3 id="1-javaconfig-방식"><strong>1. JavaConfig 방식</strong></h3>
<p><strong>build.gradle</strong></p>
<pre><code>    implementation 'org.springframework:spring-context:6.1.11'
    implementation 'org.projectlombok:lombok:1.18.24'</code></pre><p><strong>MemberDTO</strong></p>
<pre><code class="language-java">import lombok.*;

@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter @ToString
public class MemberDTO {
    private int sequence;
    private String id;
    private String pwd;
    private String name;
}</code></pre>
<ul>
<li><code>@NoArgsConstructor</code> : 기본 생성자를 만들어준다는 뜻의 어노테이션</li>
<li><code>@AllArgsConstructor</code> : 모든 필드를 매개변수로 갖는 생성자를 만들어준다는 어노테이션</li>
<li><code>@Getter</code> : 각 필드들의 get 메소드를 만들어준다는 뜻의 어노테이션</li>
<li><code>@Setter</code> : 각 필드들의 set 메소드를 만들어준다는 뜻의 어노테이션</li>
<li><code>@ToString</code> : toString() 메소드를 만들어준다는 뜻의 어노테이션</li>
</ul>
<p><strong>ContextConfiguration</strong>
아래 예제에서도 같은 이름의 클래스를 만들 것이다.</p>
<blockquote>
<p>같은 이름의 클래스를 만들 때는 다른 패키지에 만들어야 한다..!</p>
</blockquote>
<pre><code class="language-java">import com.jehun.common.MemberDTO;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ContextConfiguration {
    // 설정용 자바 클래스

    @Bean(name=&quot;member&quot;)
    public MemberDTO getMember() {
        return new MemberDTO(1, &quot;user01&quot;, &quot;pass01&quot;, &quot;홍길동&quot;);
    }
}</code></pre>
<ul>
<li><code>@Bean</code> : 객체를 의미하는 어노테이션
코드에서는 member 라는 key와 <code>1, &quot;user01&quot;, &quot;pass01&quot;, &quot;홍길동&quot;</code> 라는 value를 갖는다.</li>
</ul>
<p>스프링 컨테이너가 켜지는 순간 <code>@Bean</code> 어노테이션이 달린 객체를 생성해서 갖고 있을 것이다. (싱글톤 방식)</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import com.jehun.common.MemberDTO;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        // 컨테이너라고 생각하면 된다.
        ApplicationContext context = new AnnotationConfigApplicationContext(ContextConfiguration.class);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName : beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        // bean의 id(이름)을 이용해서 bean을 가져오는 방법
        MemberDTO member = (MemberDTO) context.getBean(&quot;member&quot;);
        System.out.println(&quot;id(이름)을 이용 - member = &quot; + member);

        // bean의 클래스 메타 정보 (bean의 타입)을 전달하여 가져오는 방법
        MemberDTO member2 = context.getBean(MemberDTO.class);
        System.out.println(&quot;클래스 메타 정보 - member = &quot; + member2);

        // bean의 id 와 클래스 메타 정보를 전달하여 가져오는 방법
        MemberDTO member3 = context.getBean(&quot;member&quot;, MemberDTO.class);
        System.out.println(&quot;id 와 클래스 메타 정보 - member = &quot; + member3);
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/324f332d-0654-4258-8ca8-8dd7b15279e3/image.png" /></p>
<p><code>beanName = member</code> 를 제외한 5가지는 자동으로 생성해주는 bean들이다.</p>
<hr />
<h3 id="2-xml-파일에-bean-등록해두는-방법"><strong>2. xml 파일에 bean 등록해두는 방법</strong></h3>
<p><code>resources/xmlconfig/spring-context.xml</code> 을 만들고</p>
<p><strong>spring-context.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
       xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
       xsi:schemaLocation=&quot;
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd&quot;&gt;

    &lt;!-- MemberDTO bean 등록--&gt;
    &lt;bean id =&quot;member2&quot; class=&quot;com.jehun.common.MemberDTO&quot;&gt;
        &lt;constructor-arg index=&quot;0&quot; value=&quot;2&quot; /&gt;
        &lt;constructor-arg name=&quot;id&quot; value=&quot;2&quot; /&gt;
        &lt;constructor-arg index=&quot;2&quot;&gt;&lt;value&gt;pass01&lt;/value&gt;&lt;/constructor-arg&gt;
        &lt;constructor-arg name=&quot;name&quot;&gt;&lt;value&gt;pass01&lt;/value&gt;&lt;/constructor-arg&gt;
    &lt;/bean&gt;
&lt;/beans&gt;</code></pre>
<pre><code class="language-java">import com.jehun.common.MemberDTO;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new GenericXmlApplicationContext(&quot;section02/xmlconfig/spring-context.xml&quot;);

        MemberDTO member2 = (MemberDTO) context.getBean(&quot;member2&quot;);
        System.out.println(&quot;member2 = &quot; + member2);
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/eab0c9c5-1dc0-4ebd-b3d9-5991bbf9f841/image.png" /></p>
<hr />
<h3 id="3-어노테이션-방식"><strong>3. 어노테이션 방식</strong></h3>
<p><code>@Repository</code> 어노테이션을 이용해서 자동으로 스프링에게 객체들을 만들어달라고 할 수도 있다.</p>
<blockquote>
<p><code>@Repository</code></p>
<ol>
<li><code>@Component</code> 계열로 스프링 컨테이너(IOC 컨테이너) 가 bean으로 등록하는 클래스에 추가하는 어노테이션이다.</li>
<li>DAO (또는 Repository) 계층에 MVC 구조에 맞춰 구분하기 위해 추가하는 어노테이션이다.<ul>
<li>(추가적으로 DB에서 발생한 에러를 자바의 예외타입으로 바꿔주는 부가 기능이 있다.)</li>
</ul>
</li>
</ol>
</blockquote>
<p>한 번 보자.</p>
<p><strong>Member.DAO</strong></p>
<pre><code class="language-java">import org.springframework.stereotype.Repository;

import java.util.HashMap;
import java.util.Map;

@Repository
public class MemberDAO {
    // DAO : 레포지토리 계층의 또다른 표시
    private final Map&lt;Integer, MemberDTO&gt; memberMap;

    public MemberDAO() {
        memberMap = new HashMap&lt;&gt;();

        memberMap.put(1, new MemberDTO(1, &quot;user01&quot;, &quot;pass01&quot;, &quot;홍길동&quot;));
        memberMap.put(2, new MemberDTO(2, &quot;user02&quot;, &quot;pass02&quot;, &quot;유관순&quot;));
    }

    /* 설명. 회원 조회용 메소드 */
    public MemberDTO selectMember(int sequence) {
        return memberMap.get(sequence);
    }

    /* 설명. 회원 가입용 메소드 */
    public int insertMember (MemberDTO registerMember) {
        int before = memberMap.size();

        memberMap.put(registerMember.getSequence(), registerMember);

        int after = memberMap.size();

        return after - before;
    }
}</code></pre>
<p>메소드들은 쓰이지는 않지만 DAO처럼 보이기 위해서 만들어보긴 했다. 추후에 DAO를 활용해서 예시를 작성해보겠다.</p>
<p><strong>ContextConfiguration</strong>
위에서 말한 것과 다른 패키지에 생성한 클래스</p>
<p>설정용 클래스이다.</p>
<pre><code class="language-java">import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration(&quot;configurationAnother&quot;)
@ComponentScan(basePackages = &quot;com.jehun.common&quot;)
public class ContextConfiguration {

}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(ContextConfiguration.class);

        String[] beanNames = context.getBeanDefinitionNames();

        for (String beanName : beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a3fdbc13-a537-4e6f-a0eb-38fce0f319a1/image.png" /></p>
<hr />
<h3 id="4-xml-파일에-어노테이션-형태로-나타내는-방식"><strong>4. xml 파일에 어노테이션 형태로 나타내는 방식</strong></h3>
<p><strong>spring-context.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
       xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
       xmlns:context=&quot;http://www.springframework.org/schema/context&quot;
       xsi:schemaLocation=&quot;
       http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd&quot;&gt;

    &lt;context:component-scan base-package=&quot;com.jehun.common&quot;/&gt;

&lt;/beans&gt;</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new GenericXmlApplicationContext(&quot;section03/annotationconfig/subsection02/xml/spring-context.xml&quot;);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName : beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }
    }
}</code></pre>
<hr />
<h3 id="패키지-구조">패키지 구조</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/14479bbb-6dab-4cd7-818b-dadd6cced429/image.png" /></p>
<p>위 4가지 방식이 있는데 주로 3번째 방식인 어노테이션 방식을 사용할 것이다.</p>
<hr />
<h2 id="lombok">Lombok</h2>
<ul>
<li><code>@Getter</code> : 해당 멤버 변수에 대한 getter를 생성한다.</li>
<li><code>@Setter</code> : 해당 멤버 변수에 대한 setter를 생성한다.</li>
<li><code>@ToString</code> : 해당 클래스의 toString()을 자동으로 생성한다.</li>
<li><code>@EqualsAndHashCode</code> : 해당 클래스의 equals()와 hashCode()를 자동으로 생성한다.</li>
<li><code>@NoArgsConstructor</code> : 파라미터가 없는 생성자를 생성한다.</li>
<li><code>@AllArgsConstructor</code> : 모든 멤버 변수를 파라미터로 받는 생성자를 생성한다.</li>
<li><code>@RequiredArgsConstructor</code> : final이나 <code>@NonNull</code>인 멤버 변수만 파라미터로 받는 생성자를 생성한다.</li>
<li><code>@Data</code> : <code>@ToString</code>, <code>@EqualsAndHashCode</code>, <code>@Getter</code>, <code>@Setter</code>, <code>@RequiredArgsConstructor</code>을 모두 적용한 것과 같다.</li>
</ul>
<hr />
<h3 id="data-를-권장하지-않는-이유"><code>@Data</code>** 를 권장하지 않는 이유**</h3>
<p>너무 많은 메소드를 담고 있어서 무겁다. 필요없는 메소드들까지 가지고 있어야 해서 좋지 않다.
그리고 <code>@Setter</code>또한 권장하지 않기 때문에 매번 생성자를 만들면서 불변객체를 만드는 것이 낫다.</p>