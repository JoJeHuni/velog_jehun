<h1 id="java-설치">Java 설치</h1>
<h2 id="1-java-seeeme">1. JAVA SE/EE/ME</h2>
<h3 id="1-1-java-sestandard-edition">1-1. JAVA SE(Standard Edition)</h3>
<blockquote>
<p>💡 Java SE는 일반 PC, 서버, 고사양 시스템 들을 위한 표준 자바 플랫폼이다. 
표준의 개발 환경을 지원하는 자바 가상 머신 규격 및 API를 포함한다.</p>
<p>즉, 자바 언어라고 하는 대부분의 패키지가 포함된 에디션이다.
우리가 잘 자주 사용하게 될 <code>java.lang.*</code>, <code>java.util.*</code>, <code>java.io.*</code> 등등이 있다.</p>
</blockquote>
<h3 id="1-2-java-eeenterprise-edition">1-2. JAVA EE(Enterprise Edition)</h3>
<blockquote>
<p>💡 Java EE는 자바를 이용해 서버측 개발을 할 때 사용하는 플랫폼이다.
EJB 아키텍처 기반 컴포넌트, JSP, Sevlet, JNDI 등을 포함한 개발에 주로 사용된다.</p>
<p>즉, 자바로 구현되는 웹 프로그래밍에 많이 사용된다.</p>
</blockquote>
<h3 id="1-3-java-memicro-edition">1-3. JAVA ME(Micro Edition)</h3>
<blockquote>
<p>💡 Java ME는 제한된 자원을 가진 모바일과 같은 한정된 자원을 가진 곳을 지원하기 위해 만들어진 플랫폼 중 하나이다.</p>
</blockquote>
<hr />
<h2 id="2-jdk-와-jre">2. JDK 와 JRE</h2>
<h3 id="2-1-jdk-와-jre-란">2-1. JDK 와 JRE 란?</h3>
<blockquote>
<p>💡 <strong>JDK(Java Development Kit)</strong> 는 ‘자바 개발 키트’의 약자
JDK는 자바 개발할 때 필요한 컴파일러(javac)나 자바콘솔, javadoc, 등과 같은 키트(kit)들을 포함하고 있어서 프로그램을 생성하고 컴파일을 할 수 있다.</p>
<p><strong>JRE(Java Runtime Environment)</strong> 는 ‘자바 실행 환경’의 약자
자바로 만들어진 프로그램은 JRE가 있어야 기동 가능, JRE는 자바 가상머신(Java Virtual Machine) 과 자바 클래스 라이브러리(Java Class Library),<br />자바 명령(Java Command)를 포함한 자바 실행에 필요한 패키지를 가지고 있다.</p>
</blockquote>
<ul>
<li>JDK는 JRE를 포함하고 있다. Java 프로그램을 실행만 한다면 JRE만 설치하면 되지만, Java 프로그래밍을 한다면 JDK를 설치해야한다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/53ba03a5-45b0-4e5b-a47e-28037659e72d/image.png" /></p>
<hr />
<h3 id="1-3-1-open-jdk-세팅하기">1-3-1. Open JDK 세팅하기</h3>
<ol>
<li>Open JDK 17 버전을 깃허브에서 다운 받을 수 있다.</li>
</ol>
<p><a href="https://github.com/ojdkbuild/ojdkbuild">GitHub - ojdkbuild/ojdkbuild</a></p>
<p>11버전 또한 open jdk 를 찾아보면 찾을 수 있을 것이다.</p>
<blockquote>
<p>왜 11버전이 아닌 17버전을 쓰는지에 대해서는 추후에 작성하겠다.</p>
</blockquote>
<p>다운을 받고 나서는 환경 변수를 편집해줘야 한다.</p>
<ul>
<li>window + R 을 누르고<pre><code>sysdm.cpl ,3</code></pre><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/020854a7-cbf2-4f39-94d7-d9ef0f62c347/image.png" /></li>
</ul>
<p>시스템 속성 -&gt; 환경 변수 -&gt; Path 더블 클릭으로 열면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1c951c41-38e0-4355-8f68-2e99d744e82a/image.png" /></p>
<p>이렇게 open-jdk 가 있는데, <code>\bin</code> 앞 까지 복사해둔다.</p>
<p>시스템 변수 -&gt; 새로 만들기를 누른 뒤</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9c70c410-cd58-4261-8b81-2abf1281e575/image.png" /></p>
<p>다 되면 다시 Path 더블 클릭 후 사진과 같이 편집해준 뒤 상단으로 이동</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/81533517-0e9f-4fd4-812a-88f535d735c2/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4e32bd5c-b8d6-4e59-a662-dc13c1a0fa61/image.png" /></p>
<p>까지 해주면 끝이다.</p>
<p>cmd 창에서</p>
<pre><code>$ java -version
openjdk version &quot;17.0.3&quot; 2022-04-19 LTS
OpenJDK Runtime Environment 21.9 (build 17.0.3+6-LTS)
OpenJDK 64-Bit Server VM 21.9 (build 17.0.3+6-LTS, mixed mode, sharing)</code></pre><p>라고 뜨면 잘 된 것</p>
<p>인텔리제이도 사용할 것인데 Jetbrain에서 exe 파일로 다운 받아서 유료지만 돈 내고 사용하면 된다.</p>