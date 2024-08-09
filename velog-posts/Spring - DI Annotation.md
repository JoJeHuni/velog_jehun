<h1 id="di-annotation">DI Annotation</h1>
<p>이번에는 어노테이션을 활용한 DI에 대해 알아보자.</p>
<p><code>@Autowired</code> : <code>Type</code>을 통한 DI를 할 때 사용한다. 스프링 컨테이너가 알아서 해당 타입의 Bean을 찾아서 주입해준다.</p>
<p>기본적으로 작성해두고 사용할 클래스들</p>
<p><strong>BookDTO</strong></p>
<pre><code class="language-java">import lombok.*;

import java.util.Date;

@NoArgsConstructor
@AllArgsConstructor
@Getter @Setter @ToString
public class BookDTO {
    private int sequence;
    private int isbn;
    private String title;
    private String author;
    private String publisher;
    private Date createdDate;
}</code></pre>
<p><strong>BookDAO 인터페이스</strong></p>
<pre><code class="language-java">import java.util.List;

public interface BookDAO {
    List&lt;BookDTO&gt; findAllBook();

    BookDTO searchBookBySequence(int sequence);
}</code></pre>
<p><strong>BookDAOImpl</strong></p>
<pre><code class="language-java">import java.util.*;

@Repository
public class BookDAOImpl implements BookDAO{

    private Map&lt;Integer, BookDTO&gt; bookList;

    public BookDAOImpl() {
        bookList = new HashMap&lt;&gt;();
        bookList.put(1, new BookDTO(1, 123456, &quot;자바의 정석&quot;, &quot;남궁성&quot;, &quot;노우출판&quot;, new Date()));
        bookList.put(2, new BookDTO(2, 222222, &quot;와! 칭찬! 고래춤!&quot;, &quot;whale&quot;, &quot;흰수염&quot;, new Date()));
    }

    @Override
    public List&lt;BookDTO&gt; findAllBook() {
        /* HashMap은 ArrayList로 쉽게 바꿀 수 있다. HashMap -&gt; Collection -&gt; ArrayList */
        return new ArrayList&lt;&gt;(bookList.values());
    }

    @Override
    public BookDTO searchBookBySequence(int sequence) {
        // 실제로는 select 쿼리를 작성해야하지만 일단은 이렇게 두자.
        return bookList.get(sequence);
    }
}</code></pre>
<p>Impl은 인터페이스의 구현체인데 그럼 <code>Service</code> -&gt; <code>BookDAO</code> -&gt; <code>BookDAOImpl</code> 방향으로 의존이 돼 있을까?</p>
<blockquote>
<p>아니다. <code>Service</code> -&gt; <code>BookDAO</code> &lt;- <code>BookDAOImpl</code> 이런 방향으로 돼 있다.
그 이유는 컴파일 시점에는 BookDAO는 Impl이 자신의 메소드를 구현하고 있는지 모른다.
자식이지만 존재를 모르는.. 느낌... </p>
</blockquote>
<p>그럼 서비스 코드도 작성해본다.</p>
<p>이 코드 또한 여러 방식이 있는데</p>
<h2 id="의존성-주입-방식">의존성 주입 방식</h2>
<h3 id="1-필드-주입-방식">1. <strong>필드 주입 방식</strong></h3>
<ul>
<li><p>장점</p>
<ul>
<li><ol>
<li>개발할 때는 편하다.</li>
</ol>
</li>
<li><ol start="2">
<li>그래서 테스트 코드에서는 쓸만 하다.</li>
</ol>
</li>
</ul>
</li>
<li><p>단점</p>
<ul>
<li><ol>
<li>자바의 <code>reflection</code> 기술을 마음대로 쓰면 캡슐화가 적용이 안 될 수 있기 때문에 위험하다. (안티 패턴 중 하나)</li>
</ol>
</li>
<li><ol start="2">
<li>너무 <code>@Autowired</code> 어노테이션을 남발하게 된다.</li>
</ol>
</li>
</ul>
</li>
</ul>
<p><strong>BookService</strong></p>
<pre><code class="language-java">import com.jehun.section01.common.BookDAO;
import com.jehun.section01.common.BookDTO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BookService {
    /*
    이전까지는 아래처럼 초기화해줬지만 @Autowired 어노테이션을 사용할 것이다.
    BookDAO bookDAO = new BookDAOImpl();
    */

    @Autowired
    private BookDAO bookDAO;

    public List&lt;BookDTO&gt; findAllBook() {
        return bookDAO.findAllBook();
    }

    public BookDTO searchBookBySequence(int sequence) {
        return bookDAO.searchBookBySequence(sequence);
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import com.jehun.section01.autowired.subsection01.BookService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.jehun.section01&quot;);

        BookService bookService = context.getBean(&quot;bookService&quot;, BookService.class);

        /* 설명. 전체 도서 목록 조회 후 출력 확인 */
        bookService.findAllBook().forEach(System.out::println);

        /* 설명. 도서 번호로 검색 후 출력 확인 */
        System.out.println(bookService.searchBookBySequence(1));
        System.out.println(bookService.searchBookBySequence(2));
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/55fbda50-01b1-413e-b8ae-d0e175501208/image.png" /></p>
<p><strong>실행했을 때의 흐름</strong></p>
<ol>
<li>Application 클래스 속 <code>&quot;com.jehun.section01&quot;</code> 중에 스프링 컨테이너를 생성 -&gt; <code>@Repository</code>, <code>@Service</code>등의 어노테이션이 작성 된 클래스가 빈 스캐닝을 통해 잘 등록 되었는지 확인하러 감</li>
<li><code>@Service</code> 어노테이션을 보고 <code>BookService</code>의 기본 생성자를 보는데, <code>@Autowired</code> 어노테이션을 보게 된다. -&gt; 해당 타입을 찾으러 감</li>
<li><code>BookDAOImpl</code>에도 <code>@Repository</code> 어노테이션이 있기 때문에 <strong>스프링 컨테이너가 해당 타입(BookDAOImpl)을 뜻하는 것이라는 것을 알게 되며 의존성 자동 주입을 해준다.</strong></li>
</ol>
<hr />
<h3 id="2-setter를-이용한-주입-방식">2. <strong>setter를 이용한 주입 방식</strong></h3>
<p>시작하기 전 필드 방식과 다르게 하기 위해서 패키지를 새로 만들어서 정의할건데 서비스에서 BookService 클래스 이름이 같아서 안 된다.</p>
<blockquote>
<p>같은 component-scan 범위 안에 같은 타입에 같은 이름으로 2개 이상의 bean이 공존할 수 없기 때문이다.</p>
</blockquote>
<p>그래서 이전 BookService에서 <code>@Service</code> 어노테이션을 <code>@Service(&quot;bookServiceField&quot;)</code> 하고, 이번 BookService는 Setter를 사용할 것이기 때문에 아래처럼 만든다.</p>
<p><strong>BookService</strong> (2번째)</p>
<pre><code class="language-java">import com.jehun.section01.common.BookDAO;
import com.jehun.section01.common.BookDTO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service(&quot;bookServiceSetter&quot;)
public class BookService {

    @Autowired
    private BookDAO bookDAO;

    public BookService() {
    }

    /* BookDAO 타입의 빈 객체를 setter에 자동으로 주입해준다. */
    @Autowired
    public void setBookDAO(BookDAO bookDAO) {
        this.bookDAO = bookDAO;
    }

    ...
}</code></pre>
<p><code>setter</code>로 주입했을 때의 문제점 또한 있다.
다른 객체임에도 주소가 같아서 같은 객체라고 여기게 되는 문제가 생길 수도 있다. 
그리고 <code>setter</code>로 인해 무조건 <code>public</code> 접근 지정자로 만들어야하기 때문에 캡슐화에 대해 문제가 생길 수 있다.</p>
<blockquote>
<p><strong>필드와 setter 주입 방식의 가장 큰 단점</strong>
<strong>상수를 넣었을 때 의존성 주입에서 에러가 생긴다.</strong></p>
</blockquote>
<p>그래서 <strong>생성자 주입</strong>을 더 권장하는 것이다.</p>
<hr />
<h3 id="3-생성자-주입-방식">3. 생성자 주입 방식</h3>
<p><strong>BookService</strong></p>
<pre><code class="language-java">import com.jehun.section01.common.BookDAO;
import com.jehun.section01.common.BookDTO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service(&quot;bookServiceConstructor&quot;)
public class BookService {

    private BookDAO bookDAO;

    public BookService() {
    }

    @Autowired
    public BookService(BookDAO bookDAO) {
        this.bookDAO = bookDAO;
    }

    ...
}</code></pre>
<p><strong>생성자 주입 방식을 권장하는 이유</strong>도 위에 적은 것들에 추가적인 것도 알아보자.</p>
<ol>
<li>필드에 <strong>final 키워드를 추가해도 에러가 나지 않는다.</strong> 즉, 사용할 수 있다.</li>
<li><strong>순환 참조를 방지할 수 있다.</strong> =&gt; 필드 주입 방식과 setter주입 방식은 컴퓨터를 키자마자 판별해주지 않기도 하지만, 생성자는 아니다.</li>
<li>필드 주입과 <code>Setter</code> 주입의 단점 : <code>@Autowired</code> 를 남발하게 될 수도 있다. -&gt; 자바의 reflection 기술을 통해 캡슐화 적용이 불가능하다.</li>
<li><strong>테스트 코드 시에 생성자를 통해 편하게 테스트할 수 있다.</strong> (Mock 객체 생성 불필요하다.)</li>
</ol>
<hr />
<h1 id="추가적인-di-어노테이션">추가적인 DI 어노테이션</h1>
<h2 id="primary">@Primary</h2>
<p><code>@Primary</code> : 여러 개의 빈 객체 중에서 우선순위가 가장 높은 빈 객체를 지정하는 어노테이션이다.</p>
<p>포켓몬 예제로 만들어보자. 나에게는 피카츄, 파이리, 꼬부기가 있으며, 셋 중 우선순위가 가장 높은 객체를 피카츄로 정해서 해볼 것이다.</p>
<p><strong>Pokemon 인터페이스</strong></p>
<pre><code class="language-java">public interface Pokemon {
    void attack();
}</code></pre>
<p><strong>Pikachu</strong>, <strong>Charmander</strong>, <strong>Squirtle</strong></p>
<pre><code class="language-java">import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;

@Component
@Primary
public class Pikachu implements Pokemon {

    @Override
    public void attack() {
        System.out.println(&quot;피카츄 공격&quot;);
    }
}</code></pre>
<p>클래스에 코드 내용을 똑같다. (파이리 공격, 꼬부기 공격 출력문과 이름만 다름)
차이점으로는 피카츄에만 <code>@Component</code> 옆에 <code>@Primary</code> 어노테이션을 추가해줬다.</p>
<p><strong>PokemonService</strong></p>
<pre><code class="language-java">import com.jehun.section02.annotation.common.Pokemon;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service(&quot;pokemonServicePrimary&quot;)
public class PokemonService {

    private Pokemon pokemon;

    @Autowired
    public PokemonService(Pokemon pokemon) {
        this.pokemon = pokemon;
    }

    public void pokemonAttack() {
        pokemon.attack();
    }

}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.jehun.section02&quot;);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName: beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        PokemonService pokemonService = context.getBean(&quot;pokemonServicePrimary&quot;, PokemonService.class);
        pokemonService.pokemonAttack();
    }
}</code></pre>
<p>이렇게 했을 때 사실 <code>@Primary</code> 어노테이션을 지우고 실행하면 같은 우선순위인 bean 객체가 피카츄, 파이리, 꼬부기 3개가 겹치면서 에러가 발생한다.</p>
<p>그 문제점을 이제 우선순위가 가장 높은 빈 객체를 지정해 해결한 것이다.</p>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f347aba9-8bfb-489e-8adb-abc159969dce/image.png" /></p>
<hr />
<h2 id="qualifier">@Qualifier</h2>
<p><code>@Qualifier</code> : 여러 개의 빈 객체 중에서 특정 빈 객체를 이름으로 지정하는 어노테이션</p>
<p><code>context.getBean(&quot;pokemonServiceQualifier&quot;)</code> 으로 변경해보면서 Service도 수정해봤다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.ohgiraffers.section02&quot;);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName: beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        PokemonService pokemonService = context.getBean(&quot;pokemonServiceQualifier&quot;, PokemonService.class);
        pokemonService.pokemonAttack();
    }
}</code></pre>
<p><strong>PokemonService</strong></p>
<pre><code class="language-java">import com.jehun.section02.annotation.common.Pokemon;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

@Service(&quot;pokemonServiceQualifier&quot;)
public class PokemonService {

    private Pokemon pokemon;

    @Autowired
    public PokemonService(@Qualifier(&quot;squirtle&quot;) Pokemon pokemon) {
        this.pokemon = pokemon;
    }

    public void pokemonAttack() {
        pokemon.attack();
    }
}</code></pre>
<p>아까는 피카츄였지만 지금은 꼬부기다..</p>
<p>실행결과
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/aa80e788-f494-4db4-8673-b2b23435b8a9/image.png" /></p>
<hr />
<h2 id="collection">Collection</h2>
<p><code>Collection</code> 타입 : 같은 타입의 빈을 여러 개 주입 받고 싶다면 사용할 수 있다.</p>
<h3 id="list-타입">List 타입</h3>
<blockquote>
<p>같은 타입의 Bean을 List 형태로 생성자 주입을 해본다.</p>
</blockquote>
<p><strong>PokemonService</strong></p>
<pre><code class="language-java">import com.jehun.section02.annotation.common.Pokemon;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service(&quot;pokemonServiceCollection&quot;)
public class PokemonService {

    private List&lt;Pokemon&gt; pokemonList;

    @Autowired
    public PokemonService(List&lt;Pokemon&gt; pokemonList) {
        this.pokemonList = pokemonList;
    }

    public void pokemonAttack() {
        pokemonList.forEach(Pokemon::attack);
    }
}</code></pre>
<p>그럼 List에는 어떻게 요소들이 추가될까??
-&gt; 알파벳순이다.</p>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.ohgiraffers.section02&quot;);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName: beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        PokemonService pokemonService = context.getBean(&quot;pokemonServiceCollection&quot;, PokemonService.class);
        pokemonService.pokemonAttack();
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/17067137-a2b4-4489-984f-8c2de0634685/image.png" /></p>
<hr />
<h3 id="map-타입">Map 타입</h3>
<p><strong>PokemonService</strong></p>
<pre><code class="language-java">import com.jehun.section02.annotation.common.Pokemon;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.Map;

@Service(&quot;pokemonServiceCollection&quot;)
public class PokemonService {

    private Map&lt;String, Pokemon&gt; pokemonMap;

    @Autowired
    public PokemonService(Map&lt;String, Pokemon&gt; pokemonMap) {
        this.pokemonMap = pokemonMap;
    }

    public void pokemonAttack() {
        pokemonMap.forEach((k, v) -&gt; {
            System.out.println(&quot;key : &quot; + k);
            System.out.println(&quot;value : &quot; + v);
            v.attack();
        });
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(&quot;com.jehun.section02&quot;);

        String[] beanNames = context.getBeanDefinitionNames();
        for (String beanName: beanNames) {
            System.out.println(&quot;beanName = &quot; + beanName);
        }

        PokemonService pokemonService = context.getBean(&quot;pokemonServiceCollection&quot;, PokemonService.class);
        pokemonService.pokemonAttack();
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7dcc0f6b-65a0-40f8-bfba-c2dedd1615bb/image.png" /></p>
<hr />
<p>다른 것들은 이제</p>
<ul>
<li><p><code>@Resource</code> : <code>@Autowired</code>와 같은 스프링 어노테이션과 다르게 <code>name</code> 속성 값으로 의존성 주입을 할 수 있다.</p>
<ul>
<li>의존성 추가 필요<pre><code> ```
      dependencies {
  implementation(&quot;javax.annotation:javax.annotation-api:1.3.2&quot;)</code></pre>  ...생략
  }
```</li>
</ul>
</li>
<li><p><code>@Inject</code> : <code>@Autowired</code> 어노테이션과 같이 <code>Type</code> 으로 빈을 의존성 주입한다.</p>
<ul>
<li><p>의존성 추가 필요</p>
<pre><code class="language-groovy">
          dependencies {
      implementation(&quot;javax.inject:javax.inject:1&quot;)
  ...생략
  }
</code></pre>
</li>
</ul>
</li>
</ul>
<hr />
<h1 id="정리">정리</h1>
<table>
<thead>
<tr>
<th></th>
<th>@Autowried</th>
<th>@Resource</th>
<th>@Inject</th>
</tr>
</thead>
<tbody><tr>
<td>제공</td>
<td>Spring</td>
<td>Java</td>
<td>Java</td>
</tr>
<tr>
<td>지원 방식</td>
<td>필드, 생성자, 세터</td>
<td>필드, 세터</td>
<td>필드, 생성자, 세터</td>
</tr>
<tr>
<td>빈 검색 우선 순위</td>
<td>타입 → 이름</td>
<td>이름 → 타입</td>
<td>타입 → 이름</td>
</tr>
<tr>
<td>빈 지정 문법</td>
<td>@Autowired <br /> @Qualifier(”name”)</td>
<td>@Resource(name=”name”)</td>
<td>@Inject <br /> @Named(”name”)</td>
</tr>
</tbody></table>