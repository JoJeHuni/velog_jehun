<p>MySQL은 문자열, 숫자, 날짜, 시간 등 작업 수행에 도움을 주는 내장 함수를 제공한다.</p>
<h1 id="function">FUNCTION</h1>
<h2 id="문자열-관련-함수">문자열 관련 함수</h2>
<h3 id="ascii-char">ASCII, CHAR</h3>
<ul>
<li>ASCII(아스키 코드)</li>
<li>CHAR(숫자)</li>
</ul>
<p><code>ASCII: 아스키 코드 값 추출</code>
<code>CHAR: 아스키 코드로 문자 추출</code></p>
<p>EX)</p>
<pre><code class="language-sql">SELECT ASCII('A'), CHAR(65);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8f51e171-78b9-4130-8c5e-db856a8bbe76/image.png" /></p>
<hr />
<h3 id="bit_length-char_length-length">BIT_LENGTH, CHAR_LENGTH, LENGTH</h3>
<ul>
<li>BIT_LENGTH(문자열)</li>
<li>CHAR_LENGTH(문자열)</li>
<li>LENGTH(문자열)</li>
</ul>
<p><code>BIT_LENGTH: 할당된 비트 크기 반환</code>
<code>CHAR_LENGTH: 문자열의 길이 반환</code>
<code>LENGTH: 할당된 BYTE 크기 반환</code></p>
<p>EX)</p>
<pre><code class="language-sql">SELECT BIT_LENGTH('pie'), CHAR_LENGTH('pie'), LENGTH('pie');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7a190e61-a51c-4c0c-90c7-7cb39b806643/image.png" /></p>
<pre><code class="language-sql">SELECT menu_name, BIT_LENGTH(menu_name), CHAR_LENGTH(menu_name), LENGTH(menu_name) from tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/95e6358c-b412-447b-98c6-5a8606372222/image.png" /></p>
<hr />
<h3 id="concat-concat_ws">CONCAT, CONCAT_WS</h3>
<ul>
<li>CONCAT(문자열1, 문자열2, ...)</li>
<li>CONCAT_WS(구분자, 문자열1, 문자열2, ...)</li>
</ul>
<p><code>CONCAT: 문자열을 이어붙임</code>
<code>CONCAT_WS: 구분자와 함께 문자열을 이어붙임</code></p>
<p>ex)</p>
<pre><code class="language-sql">SELECT CONCAT('호랑이', '기린', '토끼');
SELECT CONCAT_WS(',', '호랑이', '기린', '토끼');
SELECT CONCAT_WS('-', '2023', '05', '31');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e3703760-5ab9-4d15-a848-1a5767e459fc/image.png" /></p>
<hr />
<h3 id="elt-field-find_in_set-instr-locate">ELT, FIELD, FIND_IN_SET, INSTR, LOCATE</h3>
<ul>
<li>ELT(위치, 문자열1, 문자열2, ...)</li>
<li>FIELD(찾을 문자열, 문자열1, 문자열2, ...) </li>
<li>FIND_IN_SET(찾을 문자열, 문자열 리스트)</li>
<li>INSTR(기준 문자열, 부분 문자열)</li>
<li>LOCATE(부분 문자열, 기준 문자열)</li>
</ul>
<p><code>ELT: 해당 위치의 문자열 반환</code>
<code>FIELD: 찾을 문자열 위치 반환</code>
<code>FIND_IN_SET: 찾을 문자열의 위치 반환</code>
<code>INSTR: 기준 문자열에서 부분 문자열의 시작 위치 반환</code>
<code>LOCATE: INSTR과 동일하고 순서는 반대</code></p>
<pre><code class="language-sql">SELECT 
       ELT(2, '사과', '딸기', '바나나') -- 딸기
     , FIELD('딸기', '사과', '딸기', '바나나') -- 2
     , FIND_IN_SET('바나나', '사과,딸기,바나나') -- 3
     , INSTR('사과딸기바나나', '딸기') -- 3
     , LOCATE('딸기', '사과딸기바나나'); -- 3</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0778add9-a257-46b1-abf9-6e0aacd14e20/image.png" /></p>
<hr />
<h3 id="format">FORMAT</h3>
<ul>
<li>FORMAT(숫자, 소수점 자릿수)</li>
</ul>
<p><code>FORMAT: 1000단위마다 콤마(,) 표시를 해 주며 소수점 아래 자릿수(반올림)까지 표현한다.</code></p>
<pre><code class="language-sql">SELECT FORMAT(123142512521.5635326, 3);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/58c88ec5-948b-4a63-99ee-01f4f5b3a160/image.png" /></p>
<hr />
<h3 id="bin-oct-hex">BIN, OCT, HEX</h3>
<ul>
<li>BIN(숫자)</li>
<li>OCT(숫자)</li>
<li>HEX(숫자)</li>
</ul>
<p><code>BIN: 2진수 표현</code>
<code>OCT: 8진수 표현</code>
<code>HEX: 16진수 표현</code></p>
<pre><code class="language-sql">SELECT BIN(65), OCT(65), HEX(65);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/949632f5-cc68-4696-8e6b-967789b101a6/image.png" /></p>
<hr />
<h3 id="insert">INSERT</h3>
<ul>
<li>INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)</li>
</ul>
<p><code>INSERT: 기준 문자열의 위치부터 길이만큼을 지우고 삽입할 문자열을 끼워 넣는다.</code></p>
<pre><code class="language-sql">SELECT INSERT('내 이름은 아무개입니다.', 7, 3, '홍길동');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/70348e48-576b-41e3-b188-b0b526cfff2d/image.png" /></p>
<hr />
<h3 id="left-right">LEFT, RIGHT</h3>
<ul>
<li>LEFT(문자열, 길이)</li>
<li>RIGHT(문자열, 길이)</li>
</ul>
<p><code>LEFT: 왼쪽에서 문자열의 길이만큼을 반환</code>
<code>RIGHT: 오른쪽에서 문자열의 길이만큼을 반환</code></p>
<pre><code class="language-sql">SELECT LEFT('Hello World!', 3), RIGHT('Hello World!', 3);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/73fc8442-5180-4a9e-9c8a-adb155532e86/image.png" /></p>
<hr />
<h3 id="upper-lower">UPPER, LOWER</h3>
<ul>
<li>UPPER(문자열)</li>
<li>LOWER(문자열)</li>
</ul>
<p><code>UPPER: 소문자를 대문자로 변경</code>
<code>LOWER: 대문자를 소문자로 변경</code></p>
<pre><code class="language-sql">SELECT LOWER('Hello World!'), UPPER('Hello World!');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9b5c1146-773a-4a6b-95e5-cf7950b6a543/image.png" /></p>
<hr />
<h3 id="lpad-rpad">LPAD, RPAD</h3>
<ul>
<li>LPAD(문자열, 길이, 채울 문자열)</li>
<li>RPAD(문자열, 길이, 채울 문자열)</li>
</ul>
<p><code>LPAD: 문자열을 길이만큼 왼쪽으로 늘린 후에 빈 곳을 문자열로 채운다.</code>
<code>RPAD: 문자열을 길이만큼 오른쪽으로 늘린 후에 빈 곳을 문자열로 채운다.</code></p>
<pre><code class="language-sql">SELECT LPAD('왼쪽', 6, '@'), RPAD('오른쪽', 6 ,'@');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7c8a26b0-0382-4afa-9b15-78195d69087f/image.png" /></p>
<hr />
<h3 id="ltrim-rtrim-trim">LTRIM, RTRIM, TRIM</h3>
<ul>
<li>LTRIM(문자열)</li>
<li>RTRIM(문자열)</li>
</ul>
<p><code>LTRIM: 왼쪽 공백 제거</code>
<code>RTRIM: 오른쪽 공백 제거</code></p>
<pre><code class="language-sql">SELECT LTRIM('    왼쪽'), RTRIM('오른쪽    ');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/550c0e95-03c1-453f-ba0d-2ab589a7c367/image.png" /></p>
<ul>
<li>TRIM(문자열)</li>
<li>TRIM(방향 자를_문자열 FROM 문자열)</li>
</ul>
<p><code>TRIM: TRIM은 기본적으로 앞뒤 공백을 제거하지만 방향(LEADING(앞), BOTH(양쪽), TRAILING(뒤))이 있으면 해당 방향에 지정한 문자열을 제거할 수 있다.</code></p>
<pre><code class="language-sql">SELECT TRIM('    MySQL    '), TRIM(BOTH '@' FROM '@@@@MySQL@@@@');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/09b00c9b-0b4a-485b-8714-828cfe6b2e6f/image.png" /></p>
<hr />
<h3 id="repeat">REPEAT</h3>
<ul>
<li>REPEAT(문자열, 횟수)</li>
</ul>
<p><code>REPEAT: 문자열을 횟수만큼 반복</code></p>
<pre><code class="language-sql">SELECT REPEAT('맛있어', 3);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2f4eab5a-b47d-4693-af0f-3bae1a72d291/image.png" /></p>
<hr />
<h3 id="replace">REPLACE</h3>
<ul>
<li>REPLACE(문자열, 찾을 문자열, 바꿀 문자열)</li>
</ul>
<p><code>REPLACE: 문자열에서 문자열을 찾아 치환</code></p>
<pre><code class="language-sql">SELECT REPLACE('마이SQL', '마이', 'My');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/13c68cd4-87fd-4a5e-ae25-5e765540f5ce/image.png" /></p>
<hr />
<h3 id="reverse">REVERSE</h3>
<ul>
<li>REVERSE(문자열)</li>
</ul>
<p><code>REVERSE: 문자열의 순서를 거꾸로 뒤집음</code></p>
<pre><code class="language-sql">SELECT REVERSE('stressed');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/72b0170c-ed2d-4f33-b6a7-081766d3bb38/image.png" /></p>
<hr />
<h3 id="space">SPACE</h3>
<ul>
<li>SPACE(길이)</li>
</ul>
<p><code>SPACE: 길이 만큼의 공백을 반환</code></p>
<pre><code class="language-sql">SELECT CONCAT('제 이름은', SPACE(5), '이고 나이는', SPACE(3), '세입니다.');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e7131261-469d-47a7-89e0-52bec5774425/image.png" /></p>
<hr />
<h3 id="substring">SUBSTRING</h3>
<ul>
<li>SUBSTRING(문자열, 시작위치, 길이)</li>
</ul>
<p><code>SUBSTRING: 시작 위치부터 길이만큼의 문자를 반환(길이를 생략하면 시작 위치부터 끝까지 반환)</code></p>
<pre><code class="language-sql">SELECT SUBSTRING('안녕하세요 반갑습니다.', 7, 2), SUBSTRING('안녕하세요 반갑습니다.', 7);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/906d8f50-ab84-42d5-960f-01ec1b1f519f/image.png" /></p>
<h3 id="substring_index">SUBSTRING_INDEX</h3>
<ul>
<li>SUBSTRING_INDEX(문자열, 구분자, 횟수)</li>
</ul>
<p><code>SUBSTRING_INDEX: 구분자가 왼쪽부터 횟수 번쨰 나오면 그 이후의 오른쪽은 버린다. 횟수가 음수일 경우 오른쪽부터 세고 왼쪽을 버린다.</code></p>
<pre><code class="language-sql">SELECT SUBSTRING_INDEX('hong.test@gmail.com', '.', 2), SUBSTRING_INDEX('hong.test@gmail.com', '.', -2);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4f3ace23-c0a4-492e-a0e4-1971baa6f36b/image.png" /></p>
<hr />
<h2 id="숫자-관련-함수">숫자 관련 함수</h2>
<h3 id="abs-절댓값">ABS (절댓값)</h3>
<ul>
<li>ABS(숫자)</li>
</ul>
<p><code>ABS: 절댓값 반환</code></p>
<pre><code class="language-sql">SELECT ABS(-123);</code></pre>
<pre><code>123</code></pre><hr />
<h3 id="ceiling-floor-round-올림-내림-반올림">CEILING, FLOOR, ROUND (올림, 내림, 반올림)</h3>
<ul>
<li>CEILING(숫자)</li>
<li>FLOOR(숫자)</li>
<li>ROUND(숫자)</li>
</ul>
<p><code>CEILING: 올림값 반환</code>
<code>FLOOR: 버림값 반환</code>
<code>ROUND: 반올림값 반환</code></p>
<pre><code class="language-sql">SELECT CEILING(1234.56), FLOOR(1234.56), ROUND(1234.56);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a46a3e32-8915-4937-97b0-3e9752cef636/image.png" /></p>
<hr />
<h3 id="conv-진수-변환">CONV (진수 변환)</h3>
<ul>
<li>CONV(숫자, 원래 진수, 변환할 진수)</li>
</ul>
<p><code>CONV: 원래 진수에서 변환하고자 하는 진수로 변환</code></p>
<pre><code class="language-sql">SELECT CONV('A', 16, 10), CONV('A', 16, 2), CONV(1010, 2, 8);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/59e1d8db-23de-46d9-8348-9c2c6083fcfb/image.png" /></p>
<hr />
<h3 id="mod-나머지-구하기">MOD (나머지 구하기)</h3>
<ul>
<li>MOD(숫자1, 숫자2) or <code>숫자1 % 숫자2</code> or <code>숫자1 MOD 숫자2</code></li>
</ul>
<p><code>MOD: 숫자 1을 숫자 2로 나눈 나머지 추출</code></p>
<pre><code class="language-sql">SELECT MOD(75, 10), 75 % 10, 75 MOD 10;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4a57ec52-4f89-4348-8124-de2df82ddd89/image.png" /></p>
<hr />
<h3 id="pow-제곱-sqrt-제곱근">POW (제곱), SQRT (제곱근)</h3>
<ul>
<li>POW(숫자1, 숫자2)</li>
<li>SQRT(숫자)</li>
</ul>
<p><code>POW: 거듭제곱값 추출</code>
<code>SQRT: 제곱근을 추출</code></p>
<pre><code class="language-sql">SELECT POW(2, 4), SQRT(16);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/65d2e8cf-517d-4a3e-ad73-9b0ab8f65197/image.png" /></p>
<hr />
<h3 id="rand">RAND</h3>
<ul>
<li>RAND()</li>
</ul>
<p><code>RAND: 0이상 1 미만의 실수를 구한다.</code>
<code>'m &lt;= 임의의 정수 &lt; n'을 구하고 싶다면 FLOOR((RAND() * (n - m) + m)을 사용한다.</code></p>
<p><code>1부터 10까지 난수 발생: FLOOR(RAND() * (11 - 1) + 1)</code></p>
<pre><code class="language-sql">SELECT RAND(), FLOOR(RAND() * (11 - 1) + 1);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/feaed672-e666-4287-810b-9eaa57d0f4a0/image.png" /></p>
<p>1부터 10까지의 난수를 발생시킨 뒤, FLOOR 를 통해 버림하여 정수를 구한 것이다.</p>
<hr />
<h3 id="sign">SIGN</h3>
<ul>
<li>SIGN(숫자)</li>
</ul>
<p><code>SIGN: 양수면 1, 0이면 0, 음수면 -1을 반환</code></p>
<pre><code class="language-sql">SELECT SIGN(10.1), SIGN(0), SIGN(-10.1);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ef420365-689f-4924-aae1-0a2169ffc152/image.png" /></p>
<hr />
<h3 id="truncate-지정된-소수점-자리까지만-조회">TRUNCATE (지정된 소수점 자리까지만 조회)</h3>
<ul>
<li>TRUNCATE(숫자, 정수)</li>
</ul>
<p><code>TRUNCATE: 소수점을 기준으로 정수 위치까지 구하고 나머지는 버림</code></p>
<pre><code class="language-sql">SELECT TRUNCATE(12345.12345, 2), TRUNCATE(12345.12345, -2);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/abbb4664-fbdc-4ced-a8ea-e38a6ac071e9/image.png" /></p>
<hr />
<h2 id="날짜-및-시간-관련-함수">날짜 및 시간 관련 함수</h2>
<h3 id="adddate-subdate">ADDDATE, SUBDATE</h3>
<ul>
<li>ADDDATE(날짜, 차이)</li>
<li>SUBDATE(날짜, 차이)</li>
</ul>
<p><code>ADDDATE: 날짜를 기준으로 차이를 더함</code>
<code>SUBDATE: 날짜를 기준으로 날짜를 뺌</code></p>
<pre><code class="language-sql">SELECT ADDDATE('2023-05-31', INTERVAL 30 DAY), ADDDATE('2023-05-31', INTERVAL 6 MONTH);
SELECT SUBDATE('2023-05-31', INTERVAL 30 DAY), SUBDATE('2023-05-31', INTERVAL 6 MONTH);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e4cfead0-2ec5-414b-9852-f10655a8bafc/image.png" /></p>
<hr />
<h3 id="addtime-subtime">ADDTIME, SUBTIME</h3>
<ul>
<li>ADDTIME(날짜/시간, 시간)</li>
<li>SUBTIME(날짜/시간, 시간)</li>
</ul>
<p><code>ADDTIME: 날짜 또는 시간을 기준으로 시간을 더함</code>
<code>SUBTIME: 날짜 또는 시간을 기준으로 시간을 뺌</code></p>
<pre><code class="language-sql">SELECT ADDTIME('2023-05-31 09:00:00', '1:0:1'), SUBTIME('2023-05-31 09:00:00', '1:0:1');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3dfcb7b5-8d3d-4921-8a2b-d445e7e2ebfd/image.png" /></p>
<hr />
<h3 id="curdate-curtime-now-sysdate">CURDATE, CURTIME, NOW, SYSDATE</h3>
<ul>
<li>CURDATE(), CURTIME(), NOW(), SYSDATE()</li>
</ul>
<p><code>CURDATE: 현재 연-월-일 추출</code>
<code>CURTIME: 현재 시:분:초 추출</code>
<code>NOW() OR SYSDATE(): 현재 연-월-일 시:분:초 추출</code></p>
<pre><code class="language-sql">SELECT CURDATE(), CURTIME(), NOW(), SYSDATE();

-- CURDATE(), CURRENT_DATE(), CURRENT_DATE는 동일
SELECT CURDATE(), CURRENT_DATE(), CURRENT_DATE;

-- CURTIME(), CURRENT_TIME(), CURRENT_TIME은 동일 
SELECT CURTIME(), CURRENT_TIME(), CURRENT_TIME;

-- NOW(), LOCALTIME, LOCALTIME(), LOCALTIMESTAMP, LOCALTIMESTAMP()는 동일
SELECT NOW(), LOCALTIME, LOCALTIME(), LOCALTIMESTAMP, LOCALTIMESTAMP();</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/00256df6-12ee-4177-ae6e-43e530b15660/image.png" /></p>
<hr />
<h3 id="year-month-dayofmonth">YEAR, MONTH, DAYOFMONTH</h3>
<ul>
<li>YEAR(날짜)</li>
<li>MONTH(날짜)</li>
<li>DAYOFMONTH(날짜)</li>
</ul>
<p><code>HOUR(시간), MINUTE(시간), SECOND(시간), MICROSECOND(시간) : 날짜 또는 시간에서 연, 월, 일, 시, 분, 초, 밀리초를 추출</code></p>
<pre><code class="language-sql">SELECT YEAR(CURDATE()), MONTH(CURDATE()), DAYOFMONTH(CURDATE());
SELECT HOUR(CURTIME()), MINUTE(CURTIME()), SECOND(CURRENT_TIME), MICROSECOND(CURRENT_TIME);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7cfa72c9-07dd-4b52-b8b9-13232d080f0e/image.png" /></p>
<hr />
<h3 id="date-time">DATE, TIME</h3>
<ul>
<li>DATE()</li>
<li>TIME()</li>
</ul>
<p><code>DATE: 연-월-일만 추출</code>
<code>TIME: 시:분:초만 추출</code></p>
<pre><code class="language-sql">SELECT DATE(NOW()), TIME(NOW());</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/18dd77d4-c4e5-4a39-bc8d-c6328f43c256/image.png" /></p>
<hr />
<h3 id="datediff-timediff">DATEDIFF, TIMEDIFF</h3>
<ul>
<li>DATEDIFF(날짜1, 날짜2)</li>
<li>TIMEDIFF(날짜1 또는 시간1, 날짜1 또는 시간2)</li>
</ul>
<p><code>DATEDIFF: 날짜1 - 날짜2의 일수를 반환</code>
<code>TIMEDIFF: 시간1 - 시간2의 결과를 구함</code></p>
<pre><code class="language-sql">SELECT DATEDIFF('2023-05-31', '2023-02-27'), TIMEDIFF('17:07:11','13:06:10');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/affeb024-4beb-44f0-929e-793aa4b7c2bf/image.png" /></p>
<hr />
<h3 id="dayofweek-monthname-dayofyear">DAYOFWEEK, MONTHNAME, DAYOFYEAR</h3>
<ul>
<li>DAYOFWEEK(날짜), MONTHNAME(), DAYOFYEAR(날짜)</li>
</ul>
<p><code>DAYOFWEEK: 요일 반환(1이 일요일)</code>
<code>MONTHNAME: 해당 달의 이름 반환</code>
<code>DAYOFYEAR: 해당 년도에서 몇 일이 흘렀는지 반환</code></p>
<pre><code class="language-sql">SELECT DAYOFWEEK(CURDATE()), MONTHNAME(CURDATE()), DAYOFYEAR(CURDATE());</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a89c7e4e-6d80-4f62-adf6-44ddbb58260e/image.png" /></p>
<hr />
<h3 id="last_day">LAST_DAY</h3>
<ul>
<li>LAST_DAY(날짜)</li>
</ul>
<p><code>LAST_DAY: 해당 날짜의 달에서 마지막 날의 날짜를 구한다.</code></p>
<pre><code class="language-sql">SELECT LAST_DAY('20230201');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a189a6ba-8d2d-4437-873b-570baf18925d/image.png" /></p>
<hr />
<h3 id="makedate">MAKEDATE</h3>
<ul>
<li>MAKEDATE(연도, 정수)</li>
</ul>
<p><code>MAKEDATE: 해당 연도의 정수만큼 지난 날짜를 구한다.</code></p>
<pre><code class="language-sql">SELECT MAKEDATE(2023, 32);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d4793837-67d0-4289-938c-843e61b31f5e/image.png" /></p>
<hr />
<h3 id="maketime">MAKETIME</h3>
<ul>
<li>MAKETIME(시, 분, 초)</li>
</ul>
<p><code>MAKETIME: 시, 분, 초를 이용해서 '시:분:초'의 TIME 형식을 만든다.</code></p>
<pre><code class="language-sql">SELECT MAKETIME(17, 03, 02);</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/49f7be8f-96bf-458b-a96a-e8d3949b0d67/image.png" /></p>
<hr />
<h3 id="quarter">QUARTER</h3>
<ul>
<li>QUARTER(날짜)</li>
</ul>
<p><code>QUARTER: 해당 날짜의 분기를 구함</code></p>
<pre><code class="language-sql">SELECT QUARTER('2023-05-31');</code></pre>
<pre><code>2</code></pre><hr />
<h3 id="time_to_sec">TIME_TO_SEC</h3>
<ul>
<li>TIME_TO_SEC(시간)</li>
</ul>
<p><code>TIME_TO_SEC: 시간을 초 단위로 구함</code></p>
<pre><code class="language-sql">SELECT TIME_TO_SEC('1:1:1');</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c95e6ca0-06ac-42ff-a8a8-9c8054bd5557/image.png" /></p>