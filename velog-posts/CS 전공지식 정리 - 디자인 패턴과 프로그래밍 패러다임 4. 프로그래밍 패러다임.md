<h1 id="프로그래밍-패러다임">프로그래밍 패러다임</h1>
<blockquote>
<p>프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론</p>
</blockquote>
<p>간단하게 알아보면 프로그래밍 패러다임은 크게 <code>선언형</code> / <code>명령형</code> 2가지로 나눠지며
추가적으로는</p>
<ul>
<li>선언형<ul>
<li><code>함수형</code></li>
</ul>
</li>
<li>명령형<ul>
<li><code>객체지향형</code></li>
<li><code>절차지향형</code></li>
</ul>
</li>
</ul>
<p>위와 같이 나눠진다.</p>
<h2 id="선언형과-함수형-프로그래밍">선언형과 함수형 프로그래밍</h2>
<p>선언형 프로그래밍 : '무엇을' 풀어내는가 &lt;- 이것에 집중하는 패러다임</p>
<ul>
<li>&quot;프로그램은 함수로 이루어진 것이다&quot; 라는 명제가 담겨있다.</li>
</ul>
<p>함수형 프로그래밍은 선언형 프로그래밍의 일종이다.</p>
<p>배열 속에서 최댓값을 찾기 위해서는 로직을 구성하는데</p>
<pre><code class="language-javascript">const ret = [1, 2, 3, 4, 5, 6, 7]
.reduce((max, num) =&gt; num &gt; max ? num : max, 0)
console.log(ret) // 7</code></pre>
<p><code>reduce()</code>는 '배열'만 받아서 누적한 결과값을 반환하는 순수 함수로, </p>
<p>함수형 프로그래밍은 '순수 함수'들을 블록처럼 쌓아 로직을 구현하고 '고차 함수'를 통해 재사용성을 높인 프로그래밍 패러다임이다.</p>
<p><strong>JavaScript는</strong> 함수가 일급 객체이기 때문에 -&gt; 객체지향보다는 <strong>함수형 프로그래밍 방식이 선호된다.</strong></p>
<blockquote>
<p><strong>순수 함수</strong> : 출력이 입력에만 의존하는 것</p>
</blockquote>
<blockquote>
<p><strong>고차 함수</strong> : 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것
일급 객체의 특징
• 변수나 메서드에 함수를 할당할 수 있습니다.
• 함수 안에 함수를 매개변수로 담을 수 있습니다.
• 함수가 함수를 반환할 수 있습니다.</p>
</blockquote>
<hr />
<h2 id="객체지향-프로그래밍">객체지향 프로그래밍</h2>
<blockquote>
<p>객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식</p>
</blockquote>
<pre><code class="language-javascript">const ret = [1, 2, 3, 4, 5, 6, 7]
class List {
    constructor(list) {
        this.list = list
        this.mx = list.reduce((max, num) =&gt; num &gt; max ? num : max, 0)
    }
    getMax() {
        return this.mx
    }
}
const a = new List(ret)
console.log(a.getMax()) // 7</code></pre>
<p>List 라는 클래스를 만들어 a 라는 객체를 만들 때 최댓값을 추출하는 메서드의 예제이다.</p>
<hr />
<h3 id="객체지향-프로그래밍-특징">객체지향 프로그래밍 특징</h3>
<p><a href="https://velog.io/@jojehuni_9759/java-%ED%81%B4%EB%9E%98%EC%8A%A4-%EA%B0%9D%EC%B2%B4-%EA%B0%9D%EC%B2%B4%EB%B0%B0%EC%97%B4">추상화, 캡슐화</a></p>
<p><a href="https://velog.io/@jojehuni_9759/Java-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%EC%9D%98-%ED%8A%B9%EC%A7%95-%EC%83%81%EC%86%8D-%EB%8B%A4%ED%98%95%EC%84%B1">상속성</a></p>
<p><a href="https://velog.io/@jojehuni_9759/Java-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%ED%8A%B9%EC%A7%95-%EB%8B%A4%ED%98%95%EC%84%B1">다형성</a></p>
<p>오버로딩과 오버라이딩에 대해서는 각각 추상화, 캡슐화 / 상속성에 대해 정리할 때 적어뒀으니 참고하면 좋을 것 같다.</p>
<hr />
<h3 id="객체지향-프로그래밍-설계-원칙">객체지향 프로그래밍 설계 원칙</h3>
<ul>
<li><p>단일 책임 원칙</p>
<blockquote>
<p>단일 책임 원칙(SRP, Single Responsibility Principle) :모든 클래스는 각각 하나의 책임만 가져야 하는 원칙</p>
<p>예를 들어 A라는 로직이 존재한다면 어떠한 클래스는 A에 관한 클래스여야 하고 이를 수정한다고 했을 때도 A와 관련된 수정이어야 한다.</p>
</blockquote>
</li>
<li><p>개방-폐쇄 원칙</p>
<blockquote>
<p>개방-폐쇄 원칙(OCP, Open Closed Principle)은 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙 </p>
<p>즉, 기존의 코드 변경은 줄이고, 확장성은 있게끔</p>
</blockquote>
</li>
<li><p>리스코프 치환 원칙</p>
<blockquote>
<p>리스코프 치환 원칙(LSP, Liskov Substitution Principle)은 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것</p>
<p>클래스는 상속이 되기 마련이고 부모, 자식이라는 계층 관계가 만들어지는데, 이 때 부모 객체에 자식 객체를 넣어도 시스템이 문제없이 돌아가게 만드는 것이다.</p>
</blockquote>
</li>
<li><p>인터페이스 분리 원칙</p>
<blockquote>
<p>인터페이스 분리 원칙(ISP, Interface Segregation Principle)은 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙</p>
</blockquote>
</li>
<li><p>의존 역전 원칙</p>
<blockquote>
<p>의존 역전 원칙(DIP, Dependency Inversion Principle)은 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙</p>
<p>ex) 타이어를 갈아끼울 수 있는 틀을 만들어 놓은 후 다양한 타이어를 교체할 수 있어야 한다. 즉, 상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립</p>
</blockquote>
</li>
</ul>
<hr />
<h2 id="절차형-프로그래밍">절차형 프로그래밍</h2>
<p>로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있다.</p>
<p>코드를 구현하기만 하면 되기 때문에 코드의 가독성이 좋으며 실행 속도가 빠른 편이다.</p>
<pre><code class="language-javascript">const ret = [1, 2, 3, 4, 5, 6, 7]
let a = 0
for (let i = 0; i &lt; ret.length; i++) {
    a = Math.max(ret[i], a)
}
console.log(a) // 7</code></pre>