<h2 id="1---2-팩토리-패턴">1 - 2. 팩토리 패턴</h2>
<blockquote>
<p>정의 : 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴</p>
</blockquote>
<p>상위 클래스와 하위 클래스가 분리돼 느슨한 결합을 가진다.</p>
<p>그래서 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없어서 더 많은 유연성을 갖게 된다.</p>
<p>그리고 객체 생성 로직이 따로 있기 때문에 코드를 리팩터링하더라도 한 곳만 고치면 돼 유지보수성이 증가한다.</p>
<p>ex) 카페에서 여러 음료의 레시피마다 구체적인 내용이 들어 있는 하위 클래스가 있어 상위 클래스로 전달되고, 상위 클래스에서 이 레시피들을 토대로 우유 등을 생산하는 생산 공정을 생각하면 된다.</p>
<hr />
<p>JavaScript에서 팩토리 패턴을 구현한다면 간단하게 <code>new Object()</code>로 구현 가능</p>
<pre><code class="language-javascript">const num = new Object(5)
const str = new Object('jojehun')
num.constructor.name; // Number
str.constructor.name; // String</code></pre>
<p>숫자가 문자열을 전달함에 따라 다른 타입의 객체를 생성하는 것을 볼 수 있는데 전달받은 값에 따라 다른 객체를 생성하며 인스턴스의 타입을 정한다.</p>
<p>예시를 보자.</p>
<pre><code class="language-javascript">class Latte {
    constructor() {
        this.name = &quot;latte&quot;
    }
}

class Americano {
    constructor() {
        this.name = &quot;Americano&quot;
    }
}

class LatteFactory {
    static createCoffee() {
        return new Latte()
    }
}
class AmericanoFactory {
    static createCoffee() {
        return new Americano()
    }
}

const factoryList = { LatteFactory, AmericanoFactory }

class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}

const main = () =&gt; {
    const coffee = CoffeeFactory.createCoffee(&quot;LatteFactory&quot;)
    console.log(coffee.name) // latte
}

main()</code></pre>
<p>CoffeeFactory 라는 상위 클래스가 중요한 뼈대면
하위 클래스인 LatteFactory, AmericanoFactory로 구체적인 내용을 담고 있다.</p>
<p>이 때 의존성 주입으로 볼 수 있는데 CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌 LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문이다.</p>
<p>그리고 정적 메소드인 static 으로 <code>createCoffee()</code>를 정의했다.</p>
<p>정적 메소드를 쓰면 클래스의 인스턴스 없이 호출이 가능해서 메모리를 절약할 수 있다...</p>
<blockquote>
<p>하지만 무작정 정적 메소드로 도배를 하면 애플리케이션 실행 ~ 종료 동안 해당 정보를 static 메모리 영역에 저장하는데 그것 또한 절약한다고 보기에는 어렵다.</p>
</blockquote>
<hr />
<h2 id="1---3-전략-패턴">1 - 3. 전략 패턴</h2>
<p>전략 패턴(strategy pattern) = 정책 패턴(policy pattern)</p>
<blockquote>
<p>객체의 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라고 부르는 <code>캡슐화한 알고리즘</code>을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4234d30d-8204-460b-a55b-e06c2f2a837f/image.png" /></p>
<hr />
<h2 id="1---4-옵저버-패턴">1 - 4. 옵저버 패턴</h2>
<blockquote>
<p>어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 패턴</p>
</blockquote>
<ul>
<li>주체 : 객체의 상태 변화를 보고 있는 관찰자</li>
<li>옵저버 : 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체들을 의미</li>
</ul>
<p>주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 한다.</p>
<p>ex) 트위터 : 내가 어떤 누군가를 팔로우했을 때 해당 사용자가 포스팅을 올리면 알림이 나에게 와야 한다.</p>
<p>주로 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용된다.</p>
<p>주체라고 볼 수 있는 모델(model)에서 변경 사항이 생기면, 옵저버인 뷰에 알려주고, 이를 기반으로 컨트롤러(controller) 등이 작동하는 것이다.</p>
<hr />
<h3 id="java-에서의-옵저버-패턴">Java 에서의 옵저버 패턴</h3>
<pre><code class="language-java">import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List&lt;Observer&gt; observers;
    private String message;

    public Topic() {
        this.observers = new ArrayList&lt;&gt;();
        this.message = &quot;&quot;;
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {
        this.observers.forEach(Observer::update); 
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 

    public void postMessage(String msg) {
        System.out.println(&quot;Message sended to Topic: &quot; + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + &quot;:: got message &gt;&gt; &quot; + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber(&quot;a&quot;, topic);
        Observer b = new TopicSubscriber(&quot;b&quot;, topic);
        Observer c = new TopicSubscriber(&quot;c&quot;, topic);
        topic.register(a);
        topic.register(b);
        topic.register(c);

        topic.postMessage(&quot;Hello&quot;); 
    }
}
/*
Message sended to Topic: Hello
a:: got message &gt;&gt; Hello
b:: got message &gt;&gt; Hello
c:: got message &gt;&gt; Hello
*/</code></pre>
<p>topic을 기반으로 옵저버 패턴을 구현</p>
<p>-&gt; topic이 주체이자 객체가 된다.</p>
<p><code>class Topic implements Subject</code>를 통해 <code>Subject interface</code>를 구현
<code>Observer a = new TopicSubscriber(&quot;a&quot;, topic);</code>으로 옵저버를 선언할 때 해당 이름과 어떠한 토픽의 옵저버가 될 것인지를 정한다.</p>
<hr />
<h3 id="javascript에서의-옵저버-패턴">JavaScript에서의 옵저버 패턴</h3>
<p>프록시 객체를 통해 구현할 수도 있는데 
<strong>프록시(proxy) 객체</strong> : 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체</p>
<ul>
<li>target: 프록시할 대상</li>
<li>handler: 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수</li>
</ul>
<pre><code class="language-javascript">const handler = {
    get: function(target, name) { 
        return name === 'name' ? `${target.a} ${target.b}` : target[name]
    }
}
const p = new Proxy({a: 'JoJeHun', b: 'is a man'}, handler)
console.log(p.name) // JoJeHun is a man</code></pre>
<p><code>new Proxy</code>로 선언한 객체의 a와 b라는 속성에 특정 문자열을 담아서 handler에 name이라는 속성에 접근할 때는 a와 b라는 것을 합쳐서 문자열을 만들게끔 했다.</p>
<p>p라는 변수에 name이라는 속성을 선언하지 않았는데도 p.name으로 name 속성에 접근하려고 할 때, 그 부분을 가로채 문자열을 만들어 반환하는 것을 볼 수 있다.</p>
<p>다음은 프록시 패턴이다.</p>