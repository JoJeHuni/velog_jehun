<p>기본적으로 사용했던 날짜, 시간을 다루는 API -&gt; <code>java.util.Date</code> 와 <code>java.util.Calendar</code>
하지만 Date, Calendar는 불편해서 JDK 8에서 개선된 날짜와 시간 API <code>java.time</code> 패키지를 제공한다.</p>
<hr />
<h1 id="date-와-calendar-클래스">Date 와 Calendar 클래스</h1>
<p><code>Date</code> 클래스는 JDK 1.0 부터 날짜를 가볍게 취급하기 위해 사용되던 클래스
지금은 생성자를 비롯해 대부분의 메소드가 Deprecated -&gt; 사용 권장 X</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3f858aa8-8bc7-4388-951f-53120e1bf75a/image.png" /></p>
<p><code>Calendar</code> 클래스는 JDK 1.1 부터 새롭게 제공되는 시간과 날짜에 관한 처리를 담당하는 클래스
Calendar 클래스가 추가되면서 Date의 많은 메소드는 Deprecated 되었다.</p>
<p>Date 클래스보다는 많은 보완이 됐지만 여전한 단점들이 있다.</p>
<blockquote>
<ol>
<li><strong>Calendar 인스턴스는 불변객체가 아니기 때문에 값을 수정할 수 있다.</strong><ul>
<li>set 메소드를 통해 값을 변경할 수 있기 때문에 어느 흐름에 중간에 값이 바뀐다고 하면 알아채기가 힘들고 사이드 이팩트가 발생할 가능성이 높다. 또한 멀티 스레드 환경에서도 안전하지 않다.</li>
</ul>
</li>
<li><strong>윤초(leap second)를 고려하지 않는다.</strong><ul>
<li>윤초란?
협정 세계시에서 사용하는 세슘 원자 시계와 실제 지구의 자전/공전 속도를 기준으로 한 태양시의 차이로 인해 발생한 오차를 보정하기 위해 추가하는 1초이다. 12월 31일의 마지막에 추가하거나, 혹은 6월 30일의 마지막에 추가한다. 윤초는 사소해 보이지만 실제 2012년 링크드인 과 같은 대규모 서비스의 서버를 마비시킨 버그를 발생한 적도 있다.</li>
</ul>
</li>
<li><strong>Calendar 클래스는 월을 나타낼 때 0 부터 11까지로 표현하는 불편함이 있다.</strong><ul>
<li>하지만 일주일을 나타내는 숫자는 1부터 시작이다. 즉, 일관성이 부족하다.</li>
</ul>
</li>
</ol>
</blockquote>
<hr />
<h2 id="date-다뤄보기">Date 다뤄보기</h2>
<pre><code class="language-java">import java.text.SimpleDateFormat;
import java.util.Date;

public class Application1 {
    public static void main(String[] args) {
        Date date = new Date();         // 시스템의 현재 시간을 가진 객체 생성
        System.out.println(date);

        System.out.println(&quot;long 타입 시간 : &quot; + date.getTime());   // KST 기준으로 1970/1/1 오전 9시 이후 흐른 시간(milliseconds)
        System.out.println(&quot;long 타입 시간을 활용한 Date : &quot; + new java.util.Date(0L));
        System.out.println(&quot;현재 시간을 활용한| Date : &quot; + new java.util.Date(date.getTime()));

        SimpleDateFormat sdf = new SimpleDateFormat(&quot;yyyy-MM-dd HH:mm:ss&quot;); // 날짜 출력 형식을 문자열로 만들기
        // SimpleDateFormat sdf2 = new SimpleDateFormat(&quot;yyyy-MM-dd&quot;);

        String todayFormat = sdf.format(date);
        System.out.println(todayFormat);
    }
}
</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c74006f0-5309-4ae2-822b-3599abf20f32/image.png" /></p>
<p>sdf 에 날짜 포맷에 <code>&quot;yyyy-MM-dd HH:mm:ss E요일&quot;</code>을 하면 요일도 추가된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/12afea71-4120-42ba-8ab1-d19ac57eec01/image.png" /></p>
<hr />
<h2 id="calendar-다뤄보기">Calendar 다뤄보기</h2>
<blockquote>
<p><strong>Date 형 대비 개선점</strong></p>
</blockquote>
<ol>
<li>timezone과 관련된 기능이 추가됐다.</li>
<li>윤년 관련 기능이 내부적으로 추가됐다.</li>
<li>날짜 및 시간 필드 개념을 추가해 불필요한 메소드명을 줄였다.</li>
</ol>
<pre><code class="language-java">import java.util.Calendar;
import java.util.GregorianCalendar;

public class Application2 {
    public static void main(String[] args) {
Calendar calendar = Calendar.getInstance();     // new 연산자 사용 불가. 생성자가 protected로 막혀 있기 때문
        System.out.println(&quot;calendar = &quot; + calendar);
        // Month와 같은 것들을 잘 보면인덱스형태로 되어 있는 것을 알 수 있다.

        Calendar calendar2 = new GregorianCalendar();
        System.out.println(calendar2.get(Calendar.YEAR));

        int year = 2000;
        int month = 1 - 1; // 1월
        int day = 3;
        int hour = 3;
        int minute = 31;
        int second = 28;

        Calendar birthday = new GregorianCalendar(year, month, day, hour, minute, second);
        System.out.println(birthday);

        System.out.print(birthday.get(Calendar.YEAR) + &quot; &quot;);
        System.out.print((birthday.get(Calendar.MONTH) + 1) + &quot; &quot;);
        System.out.print(birthday.get(Calendar.DAY_OF_MONTH) + &quot; &quot;);
        System.out.print(birthday.get(Calendar.HOUR_OF_DAY) + &quot; &quot;);
        System.out.print(birthday.get(Calendar.MINUTE) + &quot; &quot;);
        System.out.print(birthday.get(Calendar.SECOND) + &quot; &quot;);
        System.out.print(birthday.get(Calendar.MILLISECOND) + &quot; &quot;);

        String weekDay = &quot;&quot;;
        int dayNum = birthday.get(Calendar.DAY_OF_WEEK);
        switch (dayNum) {
            case 1: weekDay = &quot;일&quot;; break;
            case 2: weekDay = &quot;월&quot;; break;
            case 3: weekDay = &quot;화&quot;; break;
            case 4: weekDay = &quot;수&quot;; break;
            case 5: weekDay = &quot;목&quot;; break;
            case 6: weekDay = &quot;금&quot;; break;
            case 7: weekDay = &quot;토&quot;; break;
        }
        System.out.println(&quot;내가 태어난 요일은 &quot; + weekDay +&quot;요일&quot;);

        System.out.println(&quot;AM/PM &quot; + birthday.get(Calendar.AM_PM));
        System.out.println(&quot;hourOfDay &quot; + birthday.get(Calendar.HOUR_OF_DAY));
        System.out.println(&quot;hour = &quot; + birthday.get(Calendar.HOUR));
        System.out.println(&quot;minute = &quot; + birthday.get(Calendar.MINUTE));
        System.out.println(&quot;second = &quot; + birthday.get(Calendar.SECOND));
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cf8c7500-24dc-440f-8921-c7d994fea5d8/image.png" /></p>
<p><code>hourOfDay</code> : 24시간 체계
<code>hour</code> : 12시간 체계</p>
<hr />
<h1 id="time-패키지">Time 패키지</h1>
<p>간단하게 Date, Calendar 패키지를 알아봤으니</p>
<p>JDK 1.8 버전에 추가된 Time 패키지는 기존의 시간을 다뤘던 것들의 단점을 해소하기 위해 탄생됐다.</p>
<h2 id="time-하위-패키지">Time 하위 패키지</h2>
<table>
<thead>
<tr>
<th>패키지</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>java.time</td>
<td>날짜와 시간 관련 클래스들을 제공한다</td>
</tr>
<tr>
<td>java.time.chrono</td>
<td>ISO-8601 에 정의된 외에 달력 시스템을 위한 클래스들을 제공한다</td>
</tr>
<tr>
<td>java.time.format</td>
<td>날짜와 시간 파싱과 형식화 관련 클래스들을 제공한다</td>
</tr>
<tr>
<td>java.time.temporal</td>
<td>날짜와 시간의 필드와 단위 관련 클래스들을 제공한다</td>
</tr>
<tr>
<td>java.time.zone</td>
<td>시간대 관련된 클래스들을 제공한다</td>
</tr>
</tbody></table>
<h2 id="핵심-클래스">핵심 클래스</h2>
<table>
<thead>
<tr>
<th>클래스명</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>LocalTime</td>
<td>시간 관련 작업할 때 사용하는 클래스. LocalTime 객체는 두 개의 정적 메소드를 통해 반환 받을 수 있다.</td>
</tr>
<tr>
<td>LocalDate</td>
<td>날짜 관련 작업할 때 사용하는 클래스. LocalDate 객체도 두 개의 정적 메소드로 반환 받는다.</td>
</tr>
<tr>
<td>LocalDateTime</td>
<td>시간과 날짜를 함께 작업해야할 때 사용하는 클래스</td>
</tr>
<tr>
<td>ZonedDateTime</td>
<td>시간대(Time Zone) 을 활용한 작업해야할 때 사용하는 클래스</td>
</tr>
</tbody></table>
<pre><code class="language-java">import java.time.*;

public class Application1 {
    public static void main(String[] args) {
        LocalTime timeNow = LocalTime.now();
        LocalTime timeNow2 = LocalTime.of(18, 30, 20);
        System.out.println(&quot;time = &quot; + timeNow);
        System.out.println(&quot;time2 = &quot; + timeNow2);

        LocalDate dateNow = LocalDate.now();
        LocalDate dateOf = LocalDate.of(2024, 7, 19);
        System.out.println(&quot;dateNow = &quot; + dateNow);
        System.out.println(&quot;dateOf = &quot; + dateOf);

        LocalDateTime dateTimeNow = LocalDateTime.now();
        LocalDateTime dateTimeOf = LocalDateTime.of(dateNow, timeNow);
        System.out.println(&quot;dateTimeNow = &quot; + dateTimeNow);
        System.out.println(&quot;dateTimeOf = &quot; + dateTimeOf);

        ZonedDateTime zonedDateTimeNow = ZonedDateTime.now();
        ZonedDateTime zonedDateTimeOf = ZonedDateTime.of(dateOf, timeNow2, ZoneId.of(&quot;Asia/Seoul&quot;));
        System.out.println(&quot;dateTimeNow = &quot; + zonedDateTimeNow);
        System.out.println(&quot;zonedDateTimeOf = &quot; + zonedDateTimeOf);

    }
}</code></pre>
<h3 id="필드값-확인하기">필드값 확인하기</h3>
<pre><code class="language-java">import java.time.LocalDate;
import java.time.LocalTime;
import java.time.ZonedDateTime;

public class Application2 {
    public static void main(String[] args) {

        /* 수업목표. time 패키지의 클래스들이 가지고 있는 필드값을 확인할 수 있다. */
        LocalTime localTime = LocalTime.now();

        System.out.println(&quot;localTime = &quot; + localTime);
        System.out.println(&quot;시간 : &quot; + localTime.getHour());
        System.out.println(&quot;분 : &quot; + localTime.getMinute());
        System.out.println(&quot;초 : &quot; + localTime.getSecond());
        System.out.println(&quot;나노초 : &quot; + localTime.getNano());

        LocalDate localDate = LocalDate.now();
        System.out.println(&quot;localDate = &quot; + localDate);
        System.out.println(&quot;년 : &quot; + localDate.getYear());
        System.out.println(&quot;월 : &quot; + localDate.getMonth());
        System.out.println(&quot;월 숫자 : &quot; + localDate.getMonthValue());
        System.out.println(&quot;월 중에 몇 번째 일 : &quot; + localDate.getDayOfMonth());
        System.out.println(&quot;1년 중에 몇 번째 일 : &quot; + localDate.getDayOfYear());
        System.out.println(&quot;한 주의 몇 번째 일 : &quot; + localDate.getDayOfWeek());

        ZonedDateTime zonedDateTime = ZonedDateTime.now();
        System.out.println(&quot;zonedDateTime = &quot; + zonedDateTime);
        System.out.println(&quot;zone 정보 : &quot; + zonedDateTime.getZone());
        System.out.println(&quot;시자 : &quot; + zonedDateTime.getOffset());

    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/de4fb05c-fe14-4133-9c3c-34d7a100aeea/image.png" /></p>
<hr />
<h3 id="클래스-활용한-덧셈-뺄셈-불변-특성-알아보기">클래스 활용한 덧셈, 뺄셈, 불변 특성 알아보기</h3>
<p>객체를 변화시킬 수 없어서 (불변이어서) 매번 새로운 객체를 생성한다.</p>
<pre><code class="language-java">import java.time.LocalDateTime;

public class Application3 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println(&quot;현재 시간 = &quot; + localDateTime);
        System.out.println(&quot;주소 값 = &quot; + System.identityHashCode(localDateTime));

        LocalDateTime localDateTime2 = localDateTime.plusMinutes(30);
        System.out.println(&quot;30분 후 = &quot; + localDateTime2);
        System.out.println(&quot;주소 값 = &quot; + System.identityHashCode(localDateTime2));

        LocalDateTime localDateTime3 = localDateTime.minusHours(3);
        System.out.println(&quot;3시간 전 = &quot; + localDateTime3);
        System.out.println(&quot;주소 값 = &quot; + System.identityHashCode(localDateTime3));
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d160716f-54d5-477f-973c-3d7c27a2a711/image.png" /></p>
<p>주소가 다 다른다는건 객체가 매번 생성됐다는 뜻이다.</p>
<hr />
<h3 id="날짜-비교-연산-메소드">날짜 비교 연산 메소드</h3>
<pre><code class="language-java">import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.ZonedDateTime;

public class Application4 {
    public static void main(String[] args) {
        LocalDate localDate = LocalDate.now();
        LocalDateTime localDateTime = LocalDateTime.now();
        ZonedDateTime zonedDateTime = ZonedDateTime.now();

        LocalDate past = LocalDate.of(2022, 11, 11);
        LocalDateTime future = LocalDateTime.of(2025, 1, 3, 15, 20, 30);
        ZonedDateTime now = ZonedDateTime.now();

        System.out.println(localDate.isAfter(past));
        System.out.println(localDateTime.isBefore(future));
        System.out.println(zonedDateTime.isEqual(now));
    }
}</code></pre>
<p><code>isAfter(날짜)</code> : 날짜보다 이후인지 boolean 값 반환
<code>isBefore(날짜)</code> : 날짜보다 이전인지 boolean 값 반환
<code>isEqual(날짜)</code> : 날짜와 같은 시간인지 boolean 값 반환</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0ccd3844-8fdd-4891-8eb2-49763f7e676d/image.png" /></p>
<blockquote>
<p><code>isEqual()</code> 로 비교하는건 time 패키지 자료형마다 전달인자가 같은 타입이어야 True를 반환할 수 있다.</p>
</blockquote>
<hr />
<h3 id="시간-패턴-parser-문자열-변환">시간 패턴 parser, 문자열 변환</h3>
<pre><code class="language-java">import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class Application5 {
    public static void main(String[] args) {
        String timeNow = &quot;14:05:10&quot;;
        String dateNow = &quot;2022-10-12&quot;;

        LocalTime localTime = LocalTime.parse(timeNow);
        LocalDate localDate = LocalDate.parse(dateNow);
        LocalDateTime localDateTime = LocalDateTime.parse(localDate + &quot;T&quot; + localTime);

        System.out.println(&quot;localTime = &quot; + localTime);
        System.out.println(&quot;localDate = &quot; + localDate);
        System.out.println(&quot;localDateTime = &quot; + localDateTime);

        /* 패턴 인식해줘야할 때 */
        String timeNow2 = &quot;14-05-10&quot;;
        String dateNow2 = &quot;221012&quot;;

        LocalTime localTime2 = LocalTime.parse(timeNow2, DateTimeFormatter.ofPattern(&quot;HH-mm-ss&quot;));
        LocalDate localDate2 = LocalDate.parse(dateNow2, DateTimeFormatter.ofPattern(&quot;yyMMdd&quot;));

        System.out.println(&quot;localTime2 = &quot; + localTime2);
        System.out.println(&quot;localDate2 = &quot; + localDate2);

        /* 설명. time 패키지가 인식한 날자 및 시간을 원하는 문자열로 반환하기 */
        String dateFormat = localDate2.format(DateTimeFormatter.ofPattern(&quot;yyyy-MM-dd&quot;));
        String timeFormat = localTime2.format(DateTimeFormatter.ofPattern(&quot;HH:mm:ss&quot;));

        System.out.println(&quot;dateFormat = &quot; + dateFormat);
        System.out.println(&quot;timeFormat = &quot; + timeFormat);
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c72fee93-09dc-43fb-bde4-6629ca99f6ce/image.png" /></p>