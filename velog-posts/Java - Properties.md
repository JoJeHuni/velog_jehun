<p><a href="https://velog.io/@jojehuni_9759/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Hash-Java-HashMap">자료구조 - Hash (Java - HashMap)</a></p>
<p>위 작성글에 이어 <code>Properties</code> 에 대해서 같이 알아보자.</p>
<p>Hash와 비슷한데 String, String 형태로 저장하는 단순화된 Collection 클래스다.</p>
<pre><code class="language-java">import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

public class Application {
    public static void main(String[] args) {
        Properties prop = new Properties();

        prop.setProperty(&quot;driver&quot;, &quot;oracle.jdbc.drvier.OracleDriver&quot;);
        prop.setProperty(&quot;url&quot;, &quot;jdbc:oracle:thin:@localhost:1521:xe&quot;);
        prop.setProperty(&quot;user&quot;, &quot;swcamp&quot;);
        prop.setProperty(&quot;password&quot;, &quot;swcamp&quot;);

        System.out.println(&quot;prop = &quot; + prop);
    }
}</code></pre>
<pre><code class="language-java">import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

public class Application {
    public static void main(String[] args) {
        Properties prop = new Properties();

        prop.setProperty(&quot;driver&quot;, &quot;oracle.jdbc.drvier.OracleDriver&quot;);
        prop.setProperty(&quot;url&quot;, &quot;jdbc:oracle:thin:@localhost:1521:xe&quot;);
        prop.setProperty(&quot;user&quot;, &quot;swcamp&quot;);
        prop.setProperty(&quot;password&quot;, &quot;swcamp&quot;);

        /* 설명. Properties를 활용한 설정정보 파일로 출력해 보기 */
        try {
//            prop.store(new FileOutputStream(&quot;driver.dat&quot;), &quot;jdbc driver&quot;);
            prop.storeToXML(new FileOutputStream(&quot;driver.xml&quot;), &quot;jdbc driver xml version&quot;);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        /* 설명. 저장된 설정 파일에서부터 읽어온 데이터를 담을 새로운 Properties 객체 */
        Properties prop2 = new Properties();
        System.out.println(&quot;읽어오기 전: &quot; + prop2);

        try {
//            prop2.load(new FileInputStream(&quot;driver.dat&quot;));
            prop2.loadFromXML(new FileInputStream(&quot;driver.xml&quot;));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        System.out.println(&quot;읽어온 이후 =======================&quot;);
        System.out.println(&quot;driver: &quot; + prop2.getProperty(&quot;driver&quot;));
        System.out.println(&quot;url: &quot; + prop2.getProperty(&quot;url&quot;));
        System.out.println(&quot;user: &quot; + prop2.getProperty(&quot;user&quot;));
        System.out.println(&quot;password: &quot; + prop2.getProperty(&quot;password&quot;));
    }
}</code></pre>
<p>dat 파일도 되고 xml 파일도 된다.</p>
<ol>
<li><p>데이터를 저장하는데 사용되는 <code>setProperty()</code>는 단순히 Hashtable의 put메서드를 호출</p>
</li>
<li><p><code>setProperty()</code>는 기존에 같은 키로 저장된 값이 있는 경우 그 값을 <code>Object</code>타입으로 반환하며, 그렇지 않을 경우 <code>null</code>을 반환함.</p>
</li>
<li><p><code>getProperty()</code>는 <code>properties</code>에 저장된 값을 읽어오는 일을 하는데 만일 읽어오려는 키가 존재하지 않으면 지정된 기본값을 반환함.</p>
</li>
</ol>