<h1 id="입출력-io">입출력 (IO)</h1>
<blockquote>
<p>Input Output의 약자. 컴퓨터 내부 or 외부 장치와 <strong>프로그램 간의 데이터 연동을 위한 자바 라이브러리</strong>
단방향 데이터 송, 수신을 위해 Stream 활용</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f09d807f-6d17-4a7d-8a32-64ae7794e07b/image.png" /></p>
<hr />
<h2 id="스트림-stream">스트림 (Stream)</h2>
<blockquote>
<p>입, 출력 장치에서 <strong>데이터를 읽고 쓰기 위한 단방향 통로</strong>로 자바에서 제공하는 클래스이다.
바이트 단위 처리와 문자 단위 처리를 위한 스트림 등이 존재한다.</p>
</blockquote>
<blockquote>
<p>기본적으로 1바이트 단위의 데이터만 지나가게 되고, 주고 받는 데이터의 <strong>기본 단위가 1바이트라는 것은 한 방향만 처리 가능하다는 것</strong>이다.
즉, <strong>입력 스트림, 출력 스트림을 따로 구성</strong>해야 한다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e3cc174c-d445-4c35-bdf8-e104ec8f2b49/image.png" /></p>
<hr />
<h2 id="사용-이유">사용 이유</h2>
<blockquote>
<p>입출력을 사용함으로써 사용자로 부터 입력을 받거나 화면이나 스피커로 출력해 줄 수 있다. 
또한 파일 형태로 프로그램의 종료 여부와 상관없이 영구적으로 데이터를 저장할 수도 있다.</p>
</blockquote>
<hr />
<h1 id="파일-관련-입출력">파일 관련 입,출력</h1>
<h2 id="파일-클래스-file-class">파일 클래스 (File class)</h2>
<blockquote>
<p>파일 시스템의 <strong>파일을 다루기 위한 클래스</strong></p>
</blockquote>
<pre><code class="language-java">File file = new File(&quot;file path&quot;);
File file = new File(&quot;C:/data/childDir/grandChildDir/fileTest.txt&quot;);$</code></pre>
<ul>
<li><strong>파일/디렉토리 생성 및 삭제 메소드</strong></li>
</ul>
<table>
<thead>
<tr>
<th>리턴 타입</th>
<th>메소드</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>boolean</td>
<td>createNewFile()</td>
<td>새로운 파일 생성</td>
</tr>
<tr>
<td>boolean</td>
<td>mkdir()</td>
<td>새로운 디렉토리 생성</td>
</tr>
<tr>
<td>boolean</td>
<td>mkdirs()</td>
<td>경로 상에 없는 모든 디렉토리 생성</td>
</tr>
<tr>
<td>boolean</td>
<td>delete()</td>
<td>파일 또는 디렉토리 삭제</td>
</tr>
</tbody></table>
<ul>
<li><strong>파일/디렉토리 생성 및 삭제 메소드</strong></li>
</ul>
<table>
<thead>
<tr>
<th>리턴 타입</th>
<th>메소드</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>boolean</td>
<td>canExecute()</td>
<td>실행할 수 있는 파일인지 여부</td>
</tr>
<tr>
<td>boolean</td>
<td>canRead()</td>
<td>읽을 수 있는 파일인지 여부</td>
</tr>
<tr>
<td>boolean</td>
<td>canWrite()</td>
<td>수정 및 저장할 수 있는 파일인지 여부</td>
</tr>
<tr>
<td>String</td>
<td>getName()</td>
<td>파일 이름 리턴</td>
</tr>
<tr>
<td>String</td>
<td>getParent()</td>
<td>부모 디렉토리 리턴</td>
</tr>
<tr>
<td>File</td>
<td>getParentFile()</td>
<td>부모 디렉토리를 File객체로 생성 후 리턴</td>
</tr>
<tr>
<td>String</td>
<td>getPath()</td>
<td>전체 경로 리턴</td>
</tr>
<tr>
<td>boolean</td>
<td>isDirectory()</td>
<td>디렉토리인지 여부</td>
</tr>
<tr>
<td>boolean</td>
<td>isFile()</td>
<td>파일인지 여부</td>
</tr>
<tr>
<td>boolean</td>
<td>isHidden()</td>
<td>숨김 파일인지 여부</td>
</tr>
<tr>
<td>long</td>
<td>lastModified()</td>
<td>마지막 수정 날짜 및 시간 리턴</td>
</tr>
<tr>
<td>long</td>
<td>length()</td>
<td>파일 크기 리턴</td>
</tr>
</tbody></table>
<hr />
<h2 id="파일-입출력-관련-기반-스트림의-종류">파일 입출력 관련 기반 스트림의 종류</h2>
<ul>
<li><p>Byte 단위 (영어, 숫자, 특수기호 사용 시)</p>
<ul>
<li><strong>InputStream</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c6d4af5c-319b-46e5-bae9-dda4abec1416/image.png" /></li>
<li><strong>OutputStream</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1046e286-82f4-4eaf-9a9c-eaf16df1a4dd/image.png" /></li>
</ul>
</li>
<li><p><strong>문자 단위(한글까지 사용 시)</strong></p>
<ul>
<li><strong>Reader</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e6f0bb54-99fc-4813-98ac-ed03818086c2/image.png" /></li>
</ul>
</li>
</ul>
<pre><code>- **Writer**
![](https://velog.velcdn.com/images/jojehuni_9759/post/9587571b-462b-4880-99e0-2736a26a8742/image.png)</code></pre><hr />
<h2 id="파일-입출력-관련-보조-스트림의-종류">파일 입출력 관련 보조 스트림의 종류</h2>
<blockquote>
<p>보조 스트림 : 스트림의 기능을 향상시키거나 새로운 기능을 추가하기 위해 사용</p>
</blockquote>
<ul>
<li><p>보조 스트림만으로는 데이터를 주고 받는 대상에 대한 처리를 하는 것이 아니므로 입출력 처리가 불가능 하고 기반 스트림에 추가로 적용되어야 한다.</p>
</li>
<li><p><strong>보조 스트림의 종류</strong></p>
</li>
</ul>
<ol>
<li><strong>입출력 성능 향상</strong>
BufferedInputStream/BufferedOutputStream</li>
</ol>
<ul>
<li>입출력 속도 향상 및 한 줄씩 출력 및 입력 관련 메소드 제공 보조 스트림</li>
</ul>
<ol start="2">
<li><strong>형변환 보조스트림</strong>
InputStreamReader/OutputStreamWriter</li>
</ol>
<ul>
<li>인코딩 방식을 고려한 한글 깨짐 방지를 위해 고려할 수 있는 보조 스트림</li>
</ul>
<ol start="3">
<li><strong>기본 자료형 데이터 입출력</strong>
DataInputStream/DataOutputStream</li>
</ol>
<ul>
<li>기본자료형 및 문자열 관련 타입에 따른 메소드를 제공하는 보조 스트림</li>
</ul>
<ol start="4">
<li><strong>객체 자료형 데이터 입출력</strong>
ObjectInputStream/ObjectOutputStream</li>
</ol>
<ul>
<li>객체 단위 입출력을 위한 보조 스트림</li>
</ul>
<pre><code class="language-java">import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class Application1 {
    public static void main(String[] args) {
        BufferedWriter writer = null;
        try {
            writer = new BufferedWriter(new FileWriter(&quot;src/main/java/com/.../testBuffered.txt&quot;, true));
            writer.write(&quot;This is a test\n&quot;);
            writer.write(&quot;우와와우와우아우\n&quot;);

            // Buffer를 사용해서 출력하는 경우 버퍼 공간이 가득차지 않으면 출력이 되지 않는 경우가 있다.
            // 이럴 경우 버퍼에 담긴 내용을 강제로 내보내기 위해 flush() 메소드를 활용해야 한다.

            writer.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(writer != null) writer.close();    // BufferedWriter 스트림을 닫으면 내부적으로 flush()가 동작한다.
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

        /* BufferedReader를 활용한 한 줄(개행 문자 전까지)씩 읽어오기 */
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testBuffered.txt&quot;));

            String returnString = &quot;&quot;;
            while((returnString = br.readLine()) != null) {
                System.out.println(returnString);
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(br != null) br.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}</code></pre>
<p>기반 스트림인 <code>FileWriter</code>를 먼저 생성하고 그걸 돕는 <code>BufferedWriter</code> 보조 스트림을 생성한다.</p>
<p>-&gt; Buffered는 값을 옮길 단위의 주머니라고 생각하면 된다.</p>
<p><code>if(writer != null) writer.close();</code> 에서 <strong>보조스트림을 닫으면 그 이전에(내부에) 만든 스트림들은 자동으로 close() 된다.</strong>에 대해 알아두는 것이 좋다.</p>
<p>그 뒤 한 줄씩 출력하기 위해 <code>BufferedReader</code>를 사용해보았다.</p>
<hr />
<h3 id="콘솔-입출력으로-bufferedreader-사용하기">콘솔 입출력으로 BufferedReader 사용하기</h3>
<p>-&gt; scanner를 사용하는 것과 비슷하다. (효율적이기 위해서는 BufferedReader 사용.!)</p>
<pre><code class="language-java">import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Application {
    public static void main(String[] args) {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // 보조 스트림을 2개 쓰지만 InputStream은 Reader 계열을 만들어주는 보조 스트림이다.

        System.out.print(&quot;문자열 입력 : &quot;);
        try {
            String input = br.readLine();

            System.out.println(&quot;input = &quot; + input);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (br != null) br.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}</code></pre>
<p>try-catch 문도 되지만 main에 IOException만 적어줘도 되긴 한다.
(둘의 차이점이 있나? 찾아보겠다.)</p>
<pre><code class="language-java">package com.ohgiraffers.section03.filterstream;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Application {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        System.out.print(&quot;문자열 입력 : &quot;);

        String input = br.readLine();
        System.out.println(&quot;input = &quot; + input);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/985fc8c0-cdb3-4155-a5d8-f00d37cfd63a/image.png" /></p>
<hr />
<h3 id="데이터-단위-입출력-활용">데이터 단위 입출력 활용</h3>
<pre><code class="language-java">import java.io.*;

public class Application {
    public static void main(String[] args) {

        DataOutputStream dos = null;
        try {
            dos = new DataOutputStream(
                    new FileOutputStream(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testData.txt&quot;));
            dos.writeUTF(&quot;홍길동&quot;);
            dos.writeInt(20);
            dos.writeChar('A');

            dos.writeUTF(&quot;유관순&quot;);
            dos.writeInt(16);
            dos.writeChar('B');

            dos.writeUTF(&quot;강감찬&quot;);
            dos.writeInt(38);
            dos.writeChar('O');

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(dos != null) dos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

        DataInputStream dis = null;
        try {
            dis = new DataInputStream(
                    new FileInputStream(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testData.txt&quot;));
            while(true) {
                System.out.println(dis.readUTF());
                System.out.println(dis.readInt());
                System.out.println(dis.readChar());
            }

        } catch (EOFException e) {
            /* data 단위 입출력은 EOFException을 활용하여 파을의 끝까지 입력받았다는 것을 처리할 수 있다.*/
            System.out.println(&quot;파일 다 읽었음&quot;);
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(dis != null) dis.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}</code></pre>
<p>출력할 때</p>
<blockquote>
<p>data 단위 입출력은 EOFException을 활용하여 파을의 끝까지 입력받았다는 것을 처리할 수 있다.</p>
</blockquote>
<p>위와 같은 부분을 알아두자.
<code>while(true)</code>로 무한루프를 돌고 있음에도 <code>EOFException</code> 일 때는 <code>System.out.println()</code> 으로 출력해 실행에 성공하는 것을 알 수 있다.</p>
<hr />
<p>직렬화 -&gt; Serialzable 이며 이 객체가 어느 소속이었다는 것을 마크하는 것
성으로 된 레고를 write로 다 분해해도 어느 레고가 쓰였는지 알기 때문에 다시 원상복구가 가능하다.</p>
<p>반대로 하는 것 (write 된 것을 보고 자바에서 읽는 것) 을 역직렬화라고 한다.</p>
<hr />
<h3 id="객체-자료형-입출력-예제">객체 자료형 입출력 예제</h3>
<p><strong>Application</strong></p>
<pre><code class="language-java">import com.ohgiraffers.section03.filterstream.dto.MemberDTO;

import java.io.*;

public class Application {
    public static void main(String[] args) {
        MemberDTO[] members = new MemberDTO[100];
        members[0] = new MemberDTO(&quot;user01&quot;, &quot;pass01&quot;, &quot;홍길동&quot;, &quot;hong123@gmail.com&quot;, 25, '남');
        members[1] = new MemberDTO(&quot;user02&quot;, &quot;pass02&quot;, &quot;유관순&quot;, &quot;korea123@gmail.com&quot;, 16, '여');
        members[2] = new MemberDTO(&quot;user03&quot;, &quot;pass03&quot;, &quot;강감찬&quot;, &quot;kanggam123@gmail.com&quot;, 38, '남');

        ObjectOutputStream objectOutputStream = null;
        try {
            objectOutputStream = new ObjectOutputStream(new BufferedOutputStream(new FileOutputStream(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testObject.txt&quot;)));

            for (int i = 0; i &lt; members.length; i++) {
                if (members[i] == null) break;
                objectOutputStream.writeObject(members[i]);
            }

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(objectOutputStream != null) objectOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

        MemberDTO[] newMembers = new MemberDTO[members.length];

        ObjectInputStream objectInputStream = null;
        try {
            objectInputStream = new ObjectInputStream(new BufferedInputStream(new FileInputStream(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testObject.txt&quot;)));

            int i = 0;
            while (true) {
                newMembers[i] = (MemberDTO) objectInputStream.readObject();
                i++;
            }
        } catch (EOFException e) {
            System.out.println(&quot;객체 단위 파일 입력 완료&quot;);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if(objectInputStream != null) objectInputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            for (MemberDTO m : newMembers) {
                if (m == null) break;
                System.out.println(m);
            }
        }
    }
}</code></pre>
<p><strong>MemberDTO</strong></p>
<pre><code class="language-java">package com.ohgiraffers.section03.filterstream.dto;

import java.io.Serializable;

public class MemberDTO implements Serializable {

    private String id;
    private String password;
    private String name;
    private String email;
    private int age;
    private char gender;

    public MemberDTO() {
    }

    @Override
    public String toString() {
        return &quot;MemberDTO{&quot; +
                &quot;id='&quot; + id + '\'' +
                &quot;, password='&quot; + password + '\'' +
                &quot;, name='&quot; + name + '\'' +
                &quot;, email='&quot; + email + '\'' +
                &quot;, age=&quot; + age +
                &quot;, gender=&quot; + gender +
                '}';
    }

    public MemberDTO(String id, String password, String name, String email, int age, char gender) {
        this.id = id;
        this.password = password;
        this.name = name;
        this.email = email;
        this.age = age;
        this.gender = gender;
    }
}</code></pre>
<blockquote>
<p><strong>실행 후 testObject.txt를 확인해보면 추가된 것을 알 수 있다.</strong></p>
</blockquote>
<blockquote>
<p><strong>실행결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a6e73865-1193-41b2-ae1d-554cdd71e71a/image.png" /></p>
</blockquote>
<p>지금까지처럼 true가 없이 한다면 객체를 더 추가할 때마다 이제 덮어쓰기로 다시 처음부터 데이터를 넣어줘야 해서 비효율적이다.</p>
<p>그래서 끝에 기존 내용에 추가하기 위해 끝에 true를 추가하면 어떻게 될까?</p>
<p><code>objectOutputStream = new ObjectOutputStream(new BufferedOutputStream(new FileOutputStream(&quot;src/main/java/com/ohgiraffers/section03/filterstream/testObject.txt&quot;, true)));</code></p>
<p><strong>에러가 생긴다.</strong></p>
<blockquote>
<p>왜? -&gt; <code>OutputStream</code> 클래스에서 <code>writeStreamHeader</code> 메소드 때문에 객체를 새로 추가해주면 <strong>헤더가 계속 들어오려고 하는데</strong>, <strong>파일에는 하나의 헤더만 필요하다.</strong></p>
</blockquote>
<p><strong>이것을 개선해보자.</strong> (직접 OutputStream의 클래스를 오버라이드해주면 된다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d935645a-d21e-4d42-9a0b-012fec833484/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7ee3993c-bce7-4558-b7a1-7da42c9e7e5f/image.png" /></p>
<p>이것을 오버라이드 해주고</p>
<pre><code class="language-java">import java.io.IOException;
import java.io.ObjectOutputStream;
import java.io.OutputStream;

public class MyOutput extends ObjectOutputStream {
    public MyOutput(OutputStream out) throws IOException {
        super(out);
    }

    @Override
    protected void writeStreamHeader() throws IOException {
    }
}</code></pre>
<p>이렇게 헤더가 추가되지 않게끔 해주고 <code>Application</code>에 적용만 시켜주면 실행할 때마다 객체가 덮어쓰기가 아닌 추가가 될 것이다.</p>
<p>근데 한 번 실수하게 되면 아래와 같은 예외가 뜬다.. (내가 겪음)</p>
<blockquote>
<p>caused by java.io.streamcorruptedexception invalid stream header</p>
</blockquote>
<p>이 예외의 이유는 <code>ObjectOutputStream</code>이 자동으로 텍스트를 객체로 변환할 것으로 기대할 수 없기 때문에 <code>ObjectOutputStream</code>을 사용하여 데이터를 전송하지 않기 때문에 발생한다.</p>
<blockquote>
<p>이게 무슨 소리냐면 <code>true</code>를 해주고 (즉, 이어쓰기를 허용하고) 실행했을 때 처음은 괜찮지만 2번째부터 <code>stream</code>이 들어올 때 <code>Header</code>가 추가가 된다.
그래서 파일에는 하나의 헤더만 존재할 수 있는데 실행할 수록 계속 헤더가 추가가 되고, <code>Reader</code>로 읽어올 때 헤더 중복으로 인한 문제가 발생해서 읽을 수가 없다는 뜻이다.</p>
</blockquote>
<p><strong>testObject.txt 파일을 아예 지운채로 다시 실행하면 괜찮아질 것이다.</strong></p>