<h1 id="ioc">IoC</h1>
<blockquote>
<p>IoC : 제어의 역전(IoC, Inversion of Control)은 일반적인 프로그래밍에서, 프로그램의 제어 흐름 구조가 뒤바뀌는 것을 의미한다.</p>
</blockquote>
<p>=&gt; 객체의 생성 및 관리, 객체 간의 의존성 처리 등을 <strong>프레임워크에서 대신</strong> 처리해주는 것이 대표적인 예시</p>
<p>개발자가 소스코드를 작성해서 실행하면 컨테이너가 주도권을 가지고 돌린다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d16dd08f-96a8-49f5-bf6f-2d1b15d564ab/image.png" /></p>
<p>ex) 이전 글의 <code>ApplicationContext</code>
사실 이전 글에서 했던 예제로 다루면서 IoC에 대해서 이야기 한 것과 같다.</p>
<blockquote>
<p><a href="https://velog.io/@jojehuni_9759/Spring-SpringFramework%EC%97%90-%EB%8C%80%ED%95%B4"><strong>IoC 예제보러 가기</strong></a></p>
</blockquote>
<ul>
<li><strong>Application Context란?</strong><blockquote>
<p>BeanFactory를 확장한 IoC 컨테이너로 Bean을 등록하고 관리하는 기능은 BeanFactory와 동일하지만 스프링이 제공하는 각종 부가 기능을 추가로 제공한다.</p>
</blockquote>
</li>
</ul>
<p>아래의 Bean에 대해서 읽어보고 다시 보자.</p>
<hr />
<h2 id="sprint-ioc-container">Sprint IoC Container</h2>
<h3 id="bean">Bean</h3>
<blockquote>
<p>Bean : Spring IoC Container에서 관리되는 객체</p>
</blockquote>
<p>⇒ 스프링은 Bean을 생성하고, 초기화하고, 의존성 주입하고, 제거하는 등의 일을 IoC Container를 통해 자동으로 처리할 수 있다.</p>
<h3 id="bean-factory">Bean Factory</h3>
<blockquote>
<p>BeanFactory : Spring IoC Container의 가장 기본적인 형태로, Bean의 생성, 초기화, 연결, 제거 등의 라이프사이클을 관리한다. </p>
</blockquote>
<p>⇒ 이를 위해 <code>Configuration Metadata</code>를 사용한다. </p>
<ul>
<li>Configuration Metadata : BeanFactory가 IoC를 적용하기 위해 사용하는 설정 정보</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/08d7beb1-c40e-49f3-96ac-236d361c8cab/image.png" /></p>
<ul>
<li><code>ListableBeanFactory</code> :  BeanFactory가 제공하는 모든 기능을 포함한다.</li>
<li><code>ApplicationEventPublisher</code> : <code>이벤트 처리(Event Handling)</code> 기능을 제공한다.</li>
<li><code>MessageSource</code> : <code>국제화(i18n)</code> 를 지원하는 메세지를 해결하는 부가 기능을 제공한다.</li>
<li><code>ResourceLoader</code> : <code>리소스 핸들링(Resource Handling)</code> 기능을 제공한다.</li>
<li><code>GenericXmlApplicationContext</code> : ApplicationContext를 구현한 클래스. XML MetaData Configuration을 읽어 컨테이너 역할을 수행한다.</li>
<li><code>AnnotationConfigApplicationContext</code> : ApplicationContext를 구현한 클래스. Java MetaData Configuration을 읽어 컨테이너 역할을 수행한다.</li>
</ul>
<hr />
<h1 id="dependency-injection---di">Dependency Injection - DI</h1>
<blockquote>
<p><strong>Dependency Injection (의존성 주입, 이하 DI)</strong> : 객체 간의 의존 관계를 빈 설정 정보를 바탕으로 컨테이너가 자동적으로 연결해주는 것
-&gt; 객체 간의 결합도를 낮출 수 있으며 이로 인해 유지보수성과 유연성이 증가</p>
</blockquote>
<h2 id="활용-예제-1">활용 예제 1</h2>
<p><strong>spring-context.xml</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
       xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
       xsi:schemaLocation=&quot;
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd&quot;&gt;

    &lt;bean id=&quot;account&quot; class=&quot;com.jehun.common.PersonalAccount&quot;&gt;
        &lt;constructor-arg index=&quot;0&quot; value=&quot;20&quot;/&gt;
        &lt;constructor-arg index=&quot;1&quot; value=&quot;110-234-456789&quot;/&gt;
    &lt;/bean&gt;
&lt;/beans&gt;</code></pre>
<p><strong>MemberDTO</strong></p>
<pre><code class="language-java">package com.jehun.common;

import lombok.*;

@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter @ToString
public class MemberDTO {
    private int sequence;
    private String name;
    private String phone;
    private String email;
    private Account personalAccount = new PersonalAccount(3, &quot;123-2345-6734&quot;);
}</code></pre>
<p><strong>Account</strong></p>
<pre><code class="language-java">package com.jehun.common;

public interface Account {
    // 잔액 조회
    String getBalance();

    // 입금
    String deposit(int money);

    //출금
    String withdraw(int money);
}</code></pre>
<p><strong>PersonalAccount</strong></p>
<pre><code class="language-java">package com.jehun.common;

public class PersonalAccount implements Account {
    private final int backCode;
    private final String accountNo;
    private int balance;

    public PersonalAccount(int backCode, String accountNo) {
        this.backCode = backCode;
        this.accountNo = accountNo;
    }

    @Override
    public String toString() {
        return &quot;PersonalAccount{&quot; +
                &quot;backCode=&quot; + backCode +
                &quot;, accountNo='&quot; + accountNo + '\'' +
                &quot;, balance=&quot; + balance +
                '}';
    }

    @Override
    public String getBalance() {
        return accountNo + &quot; 계좌의 현재 잔액은 &quot; + balance + &quot;입니다.&quot;;
    }

    @Override
    public String deposit(int money) {
        String string = &quot;&quot;;

        if (money &gt;= 0) {
            this.balance += money;
            string = money + &quot;원이 입금되었습니다.&quot;;
        } else {
            string = &quot;금액을 잘못 입력하셨습니다.&quot;;
        }
        return string;
    }

    @Override
    public String withdraw(int money) {
        String string = &quot;&quot;;

        if (balance &gt;= money) {
            balance -= money;
            string = money + &quot;원이 출금되었습니다.&quot;;
        } else {
            string = &quot;잔액이 부족합니다. 잔액을 확인해주세요.&quot;;
        }

        return string;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">package com.jehun.section01.xmlconfig;

import com.jehun.common.MemberDTO;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new GenericXmlApplicationContext(&quot;section01/xmlconfig/spring-context.xml&quot;);

        MemberDTO member = context.getBean(MemberDTO.class);

        System.out.println(member.getPersonalAccount());
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/77e6885d-46a6-4672-a1a1-a41b3c41bda5/image.png" /></p>
<p>사진과 같이 PersonalAccount는 Account에 의존하고 있고, MemberDTO의 Account 필드는 바로 PersonalAccount와 연결돼 있는 것이 아닌, Account를 거쳐서 PersonalAccount의 메소드를 사용할 수 있는 것이다.</p>
<p>이렇게 클래스 간 연결이 돼 있으면 안 되고, DI를 통해 생길 수 있는 문제를 방지한 것이다.</p>
<pre><code class="language-java">package com.jehun.common;

import lombok.*;

@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter @ToString
public class MemberDTO {
    private int sequence;
    private String name;
    private String phone;
    private String email;
    private PersonalAccount personalAccount = new PersonalAccount(3, &quot;123-2345-6734&quot;);
}</code></pre>
<p>만약에 위의 코드처럼 MemberDTO가 돼 있었다면 Member가 가져야 하는 것을 분리하지 못했기 때문에 DI를 지키지 못한 것이라고 생각하면 된다.</p>
<blockquote>
<p><strong>다형성을 이용해서 DI를 지키는 것을 습관화하자!</strong></p>
</blockquote>
<p>추가적으로 예제들을 더 알아보자.</p>
<h2 id="활용-예제-2">활용 예제 2</h2>
<ul>
<li><code>MemberDTO</code>, <code>Account</code>, <code>PersonalAccount</code> 는 그대로</li>
</ul>
<p><code>com.jehun.section02.javaconfig</code> 패키지 안에
<code>Application</code>, <code>ContextConfiguration</code> 클래스를 만들고, xml 파일도 수정해보자.</p>
<p><strong>spring-context.xml 내용 추가</strong></p>
<pre><code class="language-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;beans xmlns=&quot;http://www.springframework.org/schema/beans&quot;
       xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
       xsi:schemaLocation=&quot;
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd&quot;&gt;

    &lt;bean id=&quot;account&quot; class=&quot;com.jehun.common.PersonalAccount&quot;&gt;
        &lt;constructor-arg index=&quot;0&quot; value=&quot;20&quot;/&gt;
        &lt;constructor-arg index=&quot;1&quot; value=&quot;110-234-456789&quot;/&gt;
    &lt;/bean&gt;

    &lt;!-- 1. 생성자 주입 --&gt;
    &lt;bean id=&quot;member&quot; class=&quot;com.jehun.common.MemberDTO&quot;&gt;
        &lt;constructor-arg name=&quot;sequence&quot; value=&quot;1&quot;/&gt;
        &lt;constructor-arg name=&quot;name&quot; value=&quot;홍길동&quot;/&gt;
        &lt;constructor-arg name=&quot;phone&quot; value=&quot;010-1234-5678&quot;/&gt;
        &lt;constructor-arg name=&quot;email&quot; value=&quot;hong@gmail.com&quot;/&gt;
        &lt;constructor-arg name=&quot;personalAccount&quot;&gt;
            &lt;ref bean=&quot;account&quot;/&gt;
        &lt;/constructor-arg&gt;
    &lt;/bean&gt;

    &lt;!-- 2. Setter 주입 --&gt;
&lt;!--    &lt;bean id=&quot;member&quot; class=&quot;com.jehun.common.MemberDTO&quot;&gt;--&gt;
&lt;!--        &lt;property name=&quot;sequence&quot; value=&quot;1&quot;/&gt;--&gt;
&lt;!--        &lt;property name=&quot;name&quot; value=&quot;홍길동&quot;/&gt;--&gt;
&lt;!--        &lt;property name=&quot;phone&quot; value=&quot;010-1234-5678&quot;/&gt;--&gt;
&lt;!--        &lt;property name=&quot;email&quot; value=&quot;hong@gmail.com&quot;/&gt;--&gt;
&lt;!--        &lt;property name=&quot;personalAccount&quot; ref=&quot;account&quot;/&gt;--&gt;
&lt;!--    &lt;/bean&gt;--&gt;
&lt;/beans&gt;</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">package com.jehun.section02.javaconfig;

import com.jehun.common.MemberDTO;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(ContextConfiguration.class);

        MemberDTO member = context.getBean(&quot;memberGenerator&quot;, MemberDTO.class); // 특정 bean을 가져오려면 메소드명을 이름처럼 넣어주면 된다.
        System.out.println(&quot;member = &quot; + member);
        System.out.println(&quot;member.getPersonalAccount() = &quot; + member.getPersonalAccount());

        System.out.println(&quot;입금 : &quot; + member.getPersonalAccount().deposit(100000));
        System.out.println(&quot;잔액 확인 : &quot; + member.getPersonalAccount().getBalance());

        System.out.println(&quot;출금 : &quot; + member.getPersonalAccount().withdraw(30000));
        System.out.println(&quot;잔액 확인 : &quot; + member.getPersonalAccount().getBalance());

    }
}</code></pre>
<p><strong>ContextConfiguration</strong></p>
<pre><code class="language-java">package com.jehun.section02.javaconfig;

import com.jehun.common.Account;
import com.jehun.common.MemberDTO;
import com.jehun.common.PersonalAccount;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ContextConfiguration {

    @Bean
    public Account accountGenerator() {
        return new PersonalAccount(20, &quot;123-45-678989&quot;);
    }

    @Bean
    public MemberDTO memberGenerator() {
        // 생성자 방식
        return new MemberDTO(1, &quot;홍길동&quot;, &quot;010-1234-5678&quot;, &quot;hong@gmail.com&quot;, accountGenerator());

        // setter 방식
//        MemberDTO member = new MemberDTO();
//        member.setSequence(1);
//        member.setName(&quot;홍길동&quot;);
//        member.setPhone(&quot;010-1234-5678&quot;);
//        member.setEmail( &quot;hong@gmail.com&quot;);
//        member.setPersonalAccount(accountGenerator());
//
//        return member;
    }
}</code></pre>
<blockquote>
<p><strong>생성자 방식을 사용하는 것이 더 좋다.</strong> 일단은 2개 다 적어놨다.
추가로 인터페이스에서 정의해둔 메소드들에 대해서도 구현해봤다.</p>
</blockquote>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/afea1b09-fd2d-4400-9271-f62faa1e00f9/image.png" /></p>