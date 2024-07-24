<p>열거형 클래스인 Enum에 대해 알아보자.</p>
<h1 id="enum">Enum</h1>
<blockquote>
<p>Enum : 열거타입(enum) 이란 관련이 있는 상수의 집합의 클래스를 의미한다. </p>
</blockquote>
<ol>
<li>클래스처럼 보이게 하는 상수</li>
<li>서로 관련있는 상수들끼리 모아 상수들을 대표할 수 있는 이름으로 타입을 정의하는 것</li>
<li>Enum 클래스 형을 기반으로 한 클래스형 선언</li>
</ol>
<hr />
<h2 id="enum-사용-x">Enum 사용 X</h2>
<p>Enum을 사용하지 않을 때 (= 정수 열거 패턴) 의 문제점들을 보자.</p>
<p><strong>Subjects 인터페이스</strong></p>
<pre><code class="language-java">public interface Subjects {
    // BACK
    public static final int JAVA = 0;
    public static final int MARIADB = 1;
    public static final int JDBC = 2;

    // FRONT
    public static final int HTML = 0;
    public static final int CSS = 1;
    public static final int JAVASCRIPT = 2;
}
</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">import java.util.Scanner;

public class Application {
    public static void main(String[] args) {
        int subjectOne = Subjects.JAVA;
        int subjectTwo = Subjects.HTML;

        // 인터페이스에서 변수명이 다름에도 값을 똑같은 0으로 넣어주면 아래 조건문이 true가 된다..
        // 즉, 1. 런타임 시점에서 둘 다 같은 상수이자 숫자일 뿐 구분할 수 없다.
        if (subjectOne == subjectTwo) {
            System.out.println(&quot;두 과목은 같은 과목이다.&quot;);
        }

        // 2. 이름 충돌 방지를 위해서는 접두어를 써서 구분해야만 한다. (이름, 번호가 같을 때)
        // JS는 프론트도 되고 백도 돼서 둘다 쓰고 싶다면 별도의 작업을 해줘야 한다. (코드는 X)

        // 3. 변수명에 쓰인 이름(문자열)을 출력하기 어렵다. - (feat.switch)
        Scanner scanner = new Scanner(System.in);
        System.out.print(&quot;과목번호를 입력해주세요. : &quot;);
        int fieldNo = scanner.nextInt();
        String subName = &quot;&quot;;
        switch(fieldNo) {
            case Subjects.JAVA: subName = &quot;JAVA&quot;; break;
            case Subjects.MARIADB: subName = &quot;MARIADB&quot;; break;
            case Subjects.JDBC: subName = &quot;JDBC&quot;; break;
        }

        System.out.println(&quot;선택한 과목명은 : &quot; + subName + &quot;입니다.&quot;);

        // 4. 같은 클래스에 속한 상수들을 순회(반복자/반복문)할 수 없다. (필드가 총 몇 개인지, 어떤 필드들이 있는지 알 수 없다.)

        // 5. 타입 안정성을 보장할 수 없다.
        printSubject(Subjects.JAVA);
        printSubject(0);        // 과목 상관없고 그냥 숫자의 개념도 허용된다.
    }

    private static void printSubject(int subjectsName) {

    }
}
</code></pre>
<blockquote>
<ol>
<li>런타임 시점에서 둘 다 같은 상수이자 숫자일 뿐 구분할 수 없다.</li>
<li>이름, 번호가 같을 때 이름 충돌 방지를 위해서는 접두어를 써서 구분해야만 한다.</li>
<li>변수명에 쓰인 이름(문자열)을 출력하기 어렵다. -&gt; switch 사용해야 함</li>
<li>같은 클래스에 속한 상수들을 순회(반복자/반복문)할 수 없다. (필드가 총 몇 개인지, 어떤 필드들이 있는지 알 수 없다.)</li>
<li>타입 안정성을 보장할 수 없다.</li>
</ol>
</blockquote>
<hr />
<h2 id="enum-사용-시">Enum 사용 시</h2>
<p><strong>Subjects - Enum 클래스</strong></p>
<pre><code class="language-java">public enum Subjects {
    JAVA,
    MARIADB,
    JDBC,
    HTML,
    CSS,
    JAVASCRIPT;

    Subjects() {
        System.out.println(&quot;기본 생성자&quot;);
    }

    @Override
    public String toString() {
        return &quot;----&quot; + this.name() + &quot;----&quot;;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        Subjects subjectOne = Subjects.JAVA;
        Subjects subjectTwo = Subjects.HTML;

        /* 설명. 1. 열거 타입으로 선언된 인스턴스는 싱글톤으로 관리되며 인스턴스가 각각 한 개임을 보장한다.
        *       작성한 순서에 따라 각각은 다른 인스턴스가 생성돼 최초 호출 시에 enum의 생성자를 활용해 생성된다.
        *       -&gt; lazy singleton 개념 */
        if (subjectOne == subjectTwo) {
            System.out.println(&quot;두 과목은 같은 과목이다.&quot;);
        } else {
            System.out.println(&quot;서로 다른 과목이다.&quot;);
        }

        // 2. 싱글톤이기에 == 비교가 가능하다. (동일 객체 비교 가능)
        Subjects subjectThree = Subjects.JAVA;

        if (subjectOne == subjectThree) {
            System.out.println(&quot;싱글톤이다.&quot;);
        }

        // 3. 오버라이딩되지 않은 toString() 또는 name() 메소드를 활용해 필드명을 문자열로 변경하기 쉽다.
        // 쉽게 말해 switch 안 써도 됨 -&gt; 개선사항
        System.out.println(Subjects.JAVA.toString());
        System.out.println(Subjects.JAVA.name());

        // 4. values()를 사용해 상수값 배열을 반환받고 이를 활용해 연속처리를 해줄 수 있다.
        Subjects[] subjects = Subjects.values();
        for (Subjects subject : subjects) {
            System.out.println(subject.toString());
            System.out.println(subject.ordinal());
        }

        // 5. 타입 안정성을 보장한다.
        printSubjects(Subjects.JAVA);
//        printSubjects(0);             //Subjects 타입이 아니어서 에러 발생
    }

    private static void printSubjects(Subjects subjects) {
    }

}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/86ab1ca7-b205-495e-a086-f08a667ab152/image.png" /></p>
<hr />
<h2 id="enum-문법">Enum 문법</h2>
<p>enum 타입의 필드를 최소 사용 시에만 열거 타입의 인스턴스를 생성하고 생성자를 따로 호출하지 않는다.</p>
<p>또한 enum 타입은 set으로 바꿔 반복시키며, 필드들을 한 번에 확인하고 활용할 수 있다.</p>
<p><strong>UserRole1 - Enum 클래스</strong></p>
<pre><code class="language-java">public enum UserRole1 {
    GUEST,
    CONSUMER,
    PRODUCER,
    ADMIN;
}</code></pre>
<p>Application</p>
<pre><code class="language-java">package com.ohgiraffers.section3.grammer;

public class Application {
    public static void main(String[] args) {
        UserRole1 admin = UserRole1.ADMIN;
        System.out.println(admin.name());
    }
}</code></pre>
<p>위가 기본 세팅으로 되어 있을 때</p>
<h3 id="메소드나-필드-추가">메소드나 필드 추가</h3>
<p>UserRole1에 단순한 메소드를 추가해보자.</p>
<pre><code class="language-java">public enum UserRole1 {
    GUEST,
    CONSUMER,
    PRODUCER,
    ADMIN;

    public String getNameToLowerCase() {
        return this.name().toLowerCase();
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        UserRole1 admin = UserRole1.ADMIN;
        System.out.println(admin.name());
        System.out.println(admin.getNameToLowerCase());
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7ce30e6c-c81d-46ce-852d-eb80cdc6864d/image.png" /></p>
<hr />
<p>이번엔 메소드 말고 필드를 가져보자.</p>
<p><strong>UserRole2 - Enum 클래스</strong></p>
<pre><code class="language-java">public enum UserRole2 {
    GUEST(&quot;게스트&quot;),
    CONSUMER(&quot;구매자&quot;),
    PRODUCER(&quot;판매자&quot;),
    ADMIN(&quot;관리자&quot;);

    private final String DESCRIPTION;

    UserRole2(String description) {
        DESCRIPTION = description;
    }

    public String getDescription() {
        return DESCRIPTION;
    }
}</code></pre>
<p><strong>Application</strong></p>
<pre><code class="language-java">public class Application {
    public static void main(String[] args) {
        UserRole2 consumer = UserRole2.CONSUMER; // 이 순간에 매개변수 생성자를 이용해서 사용 가능
        System.out.println(consumer.ordinal() + &quot;, &quot; + consumer.name() + &quot;, &quot; + consumer.getDescription());

        System.out.println(&quot;Set 으로 바꿔서 반복자 사용해보기&quot;);
        EnumSet&lt;UserRole2&gt; roles = EnumSet.allOf(UserRole2.class);

        Iterator&lt;UserRole2&gt; iterator = roles.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next().name());
        }
    }
}</code></pre>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cc5bed38-5f56-487d-924c-9f3e60716a26/image.png" /></p>
<blockquote>
<p><code>.class</code>** 는 뭘까?**
<code>UserRole2</code> 는 주소를 지칭하고
<code>UserRole2.class</code> 는 해당 타입만을 지칭한다.</p>
</blockquote>
<p>즉, <code>EnumSet.allOf(UserRole2.class)</code> 는 <code>EnumSet</code>의 요소들은 <code>UserRole2</code> 타입인 모든 요소들을 포함하고 있다고 생각하면 된다.</p>
<hr />