<p>'<strong>면접을 위한 CS 전공지식 노트</strong>' 책을 읽으면서 전공 지식에 대해 정리하려고 한다.
<a href="https://www.yes24.com/Product/Goods/108887922?pid=123487&amp;cosemkid=go16505206189583564&amp;utm_source=google_pc&amp;utm_medium=cpc&amp;utm_campaign=book_pc&amp;utm_content=ys_240530_google_pc_cc_book_pc_12203%EB%8F%84%EC%84%9C&amp;utm_term=%EB%A9%B4%EC%A0%91%EC%9D%84%EC%9C%84%ED%95%9CCS%EC%A0%84%EA%B3%B5%EC%A7%80%EC%8B%9D%EB%85%B8%ED%8A%B8&amp;gad_source=1&amp;gclid=CjwKCAiAjp-7BhBZEiwAmh9rBcqGsXBtCrUREwkX-mhBNU6fbeLt7oWofbMcBWd_gsU3hJttOqioWxoCBsMQAvD_BwE">면접을 위한 CS 전공지식 노트</a></p>
<h1 id="1-디자인-패턴">1. 디자인 패턴</h1>
<blockquote>
<p>프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용해 해결할 수 있도록 하나의 '규약' 형태로 만들어둔 것</p>
</blockquote>
<ul>
<li>자바스크립트 테스트 링크 : <a href="https://playcode.io/new/">https://playcode.io/new/</a></li>
<li>자바 테스트 링크 : <a href="https://www.tutorialspoint.com/compile_java_online.php">https://www.tutorialspoint.com/compile_java_online.php</a></li>
</ul>
<hr />
<h2 id="1---1-싱글톤-패턴">1 - 1. 싱글톤 패턴</h2>
<blockquote>
<p>하나의 클래스에 오직 하나의 Instance 만 가지는 패턴</p>
<ul>
<li>ex) 데이터베이스 연결 모듈</li>
</ul>
</blockquote>
<p>하나의 인스턴스를 만들어두고 해당 인스턴스를 다른 모듈들이 공유하며 사용한다.
그래서 <strong>인스턴스 생성 시 드는 비용이 줄어드는 장점</strong>이 있고, 역으로는 <strong>의존성이 높아진다는 단점</strong>이 있다.</p>
<p><code>JavaScript</code> 에서는 <code>리터럴 {}</code> 또는 <code>new Object</code>로 객체를 생성하게 되면 어떤 객체와도 같지 않다.</p>
<p>-&gt; 즉, 그 자체로도 싱글톤 패턴 구현 가능하다.</p>
<pre><code class="language-javascript">const obj = {
    a: 27
}
const obj2 = {
    a: 27
}
console.log(obj === obj2)
// false</code></pre>
<p>위의 코드는 각 다른 인스턴스를 가지는 것을 확인할 수 있는데</p>
<pre><code class="language-javascript">class Singleton {
    constructor() {
        if (!Singleton.instance) {
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance() {
        return this.instance
    }
}
const a = new Singleton()
const b = new Singleton() 
console.log(a === b) // true</code></pre>
<p>Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스했을 때는 <code>true</code>로 나오는 것을 보면 같은 인스턴스를 가지는 것을 알 수 있다.</p>
<blockquote>
<p>즉, <code>JavaScript</code> 에서는 <code>리터럴 {}</code> 또는 <code>new Object</code>로 객체를 생성하면 싱글톤 패턴 구현 가능</p>
</blockquote>
<hr />
<h3 id="싱글톤-패턴---javascript-예시">싱글톤 패턴 - JavaScript 예시</h3>
<p>이런 형태로 데이터베이스 연결 모듈도 DB 클래스를 만들어서 하나의 인스턴스를 기반으로 생성할 수 있다.</p>
<blockquote>
<pre><code class="language-javascript">const URL = 'mongodb://localhost:27017/jojehun'
const createConnection = url =&gt; ({&quot;url&quot; : url})
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL)
console.log(a === b) // true</code></pre>
</blockquote>
<hr />
<h3 id="싱글톤-패턴---java-예시">싱글톤 패턴 - Java 예시</h3>
<pre><code class="language-java">class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static synchronized Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld { 
     public static void main(String[] args) { 
        Singleton a = Singleton.getInstance(); 
        Singleton b = Singleton.getInstance(); 
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());  
        if (a == b) {
         System.out.println(true); 
        } 
     }
}</code></pre>
<p><strong>출력 결과</strong></p>
<pre><code>705927765
705927765
true</code></pre><hr />
<h3 id="싱글톤-패턴---mongodb의-mongoose-모듈">싱글톤 패턴 - MongoDB의 mongoose 모듈</h3>
<p>실제로 싱글톤 패턴은 Node.js에서 MongoDB 데이터베이스를 연결할 때 쓰는 <code>mongoose</code> 모듈에서 볼 수 있다.</p>
<p><code>mongoose</code>의 데이터베이스를 연결할 때 쓰는 <code>connect()</code>라는 함수는 싱글톤 인스턴스를 반환한다. 다음은 connect() 함수를 구현할 때 쓰인 실제 코드다.</p>
<pre><code class="language-javascript">Mongoose.prototype.connect = function(uri, options, callback) {
    const _mongoose = this instanceof Mongoose ? this : mongoose;
    const conn = _mongoose.connection;

    return _mongoose._promiseOrCallback(callback, cb =&gt; {
        conn.openUri(uri, options, err =&gt; {
            if (err != null) {
                return cb(err);
            }
            return cb(null, _mongoose);
        });
    });
};</code></pre>
<hr />
<h3 id="싱글톤-패턴---mysql">싱글톤 패턴 - MySQL</h3>
<pre><code class="language-javascript">// 메인 모듈
const mysql = require('mysql');
const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'example.org',
    user: 'jojehun',
    password: 'jojehun',
    database: '디비디비'
});
pool.connect();

// 모듈 A
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});

// 모듈 B
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});</code></pre>
<ol>
<li>메인 모듈에서 DB 연결에 관한 인스턴스를 정의</li>
<li>다른 모듈인 A 나 B 에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식</li>
</ol>
<p>위 형식으로 쓰인다.</p>
<hr />
<h3 id="싱글톤-패턴의-단점">싱글톤 패턴의 단점</h3>
<ol>
<li><p><strong>TDD할 때 문제가 생긴다.</strong></p>
<ul>
<li>TDD 할 땐 단위 테스트를 주로 하는데, 단위 테스트는 서로 독립적이어야 하고 테스트를 어떤 순서로든 실행할 수 있어야 한다.</li>
<li>싱글톤 패턴은 미리 생성돼 있는 인스턴스를 기반으로 구현하기에 각 테스트마다 '독립적인' 인스턴스를 만드는데 걸림돌이 된다.</li>
</ul>
</li>
<li><p>모듈 간의 결합을 강하게 만들 수 있다는 단점이 있다.</p>
</li>
</ol>
<hr />
<h3 id="의존성-주입">의존성 주입</h3>
<p>위 단점과 같이 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있는데 이 때 <code>의존성 주입 (Dependency Injection)</code> 을 통해 모듈 간의 결합을 조금 느슨하게 만들어 해결도 가능하다.</p>
<blockquote>
<p>의존성 = 종속성
ex) A가 B에 의존성이 있다는 것은 B의 변경사항에 따라 A 또한 변해야 된다는 것을 의미한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5d75410e-dbcf-4a91-86de-69e80e6fd2a3/image.png" /></p>
<p>위 그림처럼 <code>메인 모듈</code>이 '직접' 다른 하위 모듈에 대한 의존성을 주는 것보다 중간에 <code>의존성 주입자</code>가 이 부분을 가로채 <strong>메인 모듈이 '간접'적으로 의존성을 주입하는 방식</strong>이다.</p>
<blockquote>
<p>메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다. 이를 ‘디커플링이 된다’고도 한다.</p>
</blockquote>
<hr />
<h3 id="의존성-주입의-장단점">의존성 주입의 장단점</h3>
<h4 id="장점">장점</h4>
<ol>
<li>모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월하다</li>
<li>구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션 추론하기에 도움이 되며 모듈 간 관계가 더 명확해진다.</li>
</ol>
<h4 id="단점">단점</h4>
<p>모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 런타임 페널티가 생기기도 한다.</p>
<hr />
<h3 id="의존성-주입-원칙">의존성 주입 원칙</h3>
<p>의존성 주입은 의존성 주입 원칙을 지키며 만들어야 하며 원칙은 다음과 같다.</p>
<blockquote>
<p>상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.</p>
</blockquote>
<hr />