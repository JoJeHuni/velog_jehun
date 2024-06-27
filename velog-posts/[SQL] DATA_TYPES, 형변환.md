<h1 id="data_types">DATA_TYPES</h1>
<blockquote>
<p>MySQL은 여러 가지 데이터 유형을 지원(문자열, 숫자, 날짜, 시간)한다.</p>
</blockquote>
<h2 id="숫자-데이터-형식">숫자 데이터 형식</h2>
<ul>
<li>정수 또는 실수 등의 숫자를 표현한다.</li>
<li>FLOAT이나 DOUBLE형은 큰 범위의 숫자를 저장할 수 있지만 정확하지 않은 근사치를 저장한다. </li>
</ul>
<blockquote>
<p>따라서 실수 형을 저장하고 싶어도 DECIMAL을 사용하는 것이 바람직하다.</p>
</blockquote>
<table>
<thead>
<tr>
<th>데이터 형식</th>
<th>바이트 수</th>
<th>숫자 범위</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>BIT(N)</td>
<td>N/B</td>
<td></td>
<td>1~64Bit 표현, b'0000'형식으로 표현</td>
</tr>
<tr>
<td>TINYINT</td>
<td>1</td>
<td>-128 ~ 127</td>
<td>정수</td>
</tr>
<tr>
<td>SMALLINT</td>
<td>2</td>
<td>-32,768 ~ 32,767</td>
<td>정수</td>
</tr>
<tr>
<td>MEDIUMINT</td>
<td>3</td>
<td>-8,388,608 ~ 8,388,607</td>
<td>정수</td>
</tr>
<tr>
<td>INT, INTEGER</td>
<td>4</td>
<td>약-21억 ~ +21억</td>
<td>정수</td>
</tr>
<tr>
<td>BIGINT</td>
<td>8</td>
<td>약 -900경 ~ +900경</td>
<td>정수</td>
</tr>
<tr>
<td>FLOAT</td>
<td>4</td>
<td>3.40E+38 ~ -1.17E-38</td>
<td>소수점 아래 7자리까지 표현</td>
</tr>
<tr>
<td>DOUBLE</td>
<td>8</td>
<td>-1.22E-308 ~ 1.79E+308</td>
<td>소수점 아래 15자리까지 표현</td>
</tr>
<tr>
<td>DECIMAL(m,[d]), NUMBER(m,[d])</td>
<td>5~17</td>
<td>-10^38+1 ~ 10^38-1</td>
<td>전체 자릿수(m)와 소수점 이하 자릿수(d)를 가진</td>
</tr>
</tbody></table>
<hr />
<h2 id="문자-데이터-형식">문자 데이터 형식</h2>
<ul>
<li>CHAR는 고정길이 문자형으로 자릿수가 불변이다.</li>
<li>VARCHAR는 가변길이 문자형으로 자릿수가 가변이다.
(큰 자릿수를 설정해도 저장할 공간을 효율적으로 사용할 수 있다.)</li>
<li>CHAR 형식으로 설정하는 것이 INSERT/UPDATE 시에 일반적으로 더 좋은 성능을 발휘한다.</li>
<li>대용량 데이터는 TEXT계열의 데이터 형식으로 저장한다.</li>
<li>BLOB(Binary Large Object)은 사진 파일, 동영상 파일, 문서 파일 등의 대용량 이진 데이터를 저장한다.</li>
<li>ENUM은 열거형 데이터를 사용(요일이나 카테고리 등) 시 활용되는 방식이다.</li>
<li>SET은 최대 64개를 준비한 후에 2개씩 세트로 데이터를 저장하는 방식을 사용 시 활용되는 방식이다.</li>
<li>LONGTEXT는 대용량 문서 데이터, LONGBLOB은 동영상 파일과 같은 큰 바이너리 파일 저장 시에 활용할 수 있다.</li>
</ul>
<table>
<thead>
<tr>
<th>데이터 형식</th>
<th>데이터 형식</th>
<th>바이트 수</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td></td>
<td>CHAR(n)</td>
<td>1 ~ 255</td>
<td>고정길이 문자형이며 n을 1부터 255까지 지정, 그냥 CHAR만 쓰면 CHAR(1)과 동일</td>
</tr>
<tr>
<td></td>
<td>VARCHAR(n)</td>
<td>1 ~ 65535</td>
<td>가변길이 문자형이며 n을 사용하면 1부터 65535까지 지정</td>
</tr>
<tr>
<td></td>
<td>BINARY(n)</td>
<td>1 ~ 255</td>
<td>고정길이의 이진 데이터 값</td>
</tr>
<tr>
<td></td>
<td>VARBINARY(n)</td>
<td>1 ~ 255</td>
<td>가변길이의 이진 데이터 값</td>
</tr>
<tr>
<td>TEXT</td>
<td>TINYTEXT</td>
<td>1 ~ 255</td>
<td>255 크기의 TEXT 데이터 값</td>
</tr>
<tr>
<td>TEXT</td>
<td>TEXT</td>
<td>1 ~ 65535</td>
<td>N 크기의 TEXT 데이터 값</td>
</tr>
<tr>
<td>TEXT</td>
<td>MEDIUMTEXT</td>
<td>1 ~ 16777215</td>
<td>16777215 크기의 TEXT 데이터 값</td>
</tr>
<tr>
<td>TEXT</td>
<td>LONGTEXT</td>
<td>1 ~ 4294967295</td>
<td>최대 4GB 크기의 TEXT 데이터 값</td>
</tr>
<tr>
<td>BLOB</td>
<td>TINYBLOB</td>
<td>1 ~ 255</td>
<td>255 크기의 BLOB 데이터 값</td>
</tr>
<tr>
<td>BLOB</td>
<td>BLOB</td>
<td>1 ~ 65535</td>
<td>N 크기의 BLOB 데이터 값</td>
</tr>
<tr>
<td>BLOB</td>
<td>MEDIUMBLOB</td>
<td>1 ~ 16777215</td>
<td>16777215 크기의 BLOB 데이터 값</td>
</tr>
<tr>
<td>BLOB</td>
<td>LONGBLOB</td>
<td>1 ~ 4294967295</td>
<td>최대 4GB 크기의 BLOB 데이터 값</td>
</tr>
<tr>
<td></td>
<td>ENUM(값들...)</td>
<td>1 또는 2</td>
<td>최대 65535개의 열거형 데이터 값</td>
</tr>
<tr>
<td></td>
<td>SET(값들...)</td>
<td>1, 2, 3, 4, 8</td>
<td>최대 64개의 서로 다른 데이터 값</td>
</tr>
</tbody></table>
<hr />
<h2 id="날짜와-시간-데이터-형식">날짜와 시간 데이터 형식</h2>
<table>
<thead>
<tr>
<th>데이터 형식</th>
<th>바이트 수</th>
<th>설명</th>
<th></th>
</tr>
</thead>
<tbody><tr>
<td>DATE</td>
<td>3</td>
<td>날짜는 1001-01-01 ~ 9999-12-31 까지 저장되며 날짜 형식만 사용</td>
<td></td>
</tr>
<tr>
<td>'YYYY-MM-DD' 형식으로 사용됨</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>TIME</td>
<td>3</td>
<td>-838:59:59.000000 ~ 838:59:59.000000 까지 저장되며</td>
<td></td>
</tr>
<tr>
<td>'HH:MM:SS' 형식으로 사용</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>DATETIME</td>
<td>8</td>
<td>날짜는 1001-01-01 00:00:00 ~ 9999-12-31 23:59:59 까지 저장되며 형식은</td>
<td></td>
</tr>
<tr>
<td>'YYYY-MM-DD HH:MM:SS' 형식으로 사용</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>TIMESTAMP</td>
<td>4</td>
<td>날짜는 1001-01-01 00:00:00 ~ 9999-12-31 23:59:59 까지 저장되며 형식은</td>
<td></td>
</tr>
<tr>
<td>'YYYY-MM-DD HH:MM:SS' 형식으로 사용</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>time_zone 시스템 변수와 관련이 있고 UTC 시간대 변환하여 저장</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>YEAR</td>
<td>1 (tiny_int?)</td>
<td>1901 ~ 2155까지 저장</td>
<td></td>
</tr>
<tr>
<td>'YYYY' 형식으로 사용</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>
<hr />
<h2 id="기타-데이터-형식">기타 데이터 형식</h2>
<table>
<thead>
<tr>
<th>데이터 형식</th>
<th>바이트 수</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>GEOMETRY</td>
<td>N/A</td>
<td>공간 데이터 형식으로 선, 점 및 다각형 같은 공간 데이터 개체를 저장하고 조작</td>
</tr>
<tr>
<td>JSON</td>
<td>8</td>
<td>JSON 문서를 저장</td>
</tr>
</tbody></table>
<hr />
<blockquote>
<p><strong>Type casting (형변환)</strong> 에 대해 알아보자.</p>
</blockquote>
<ul>
<li>SQL 데이터의 형변환에는 명시적 형변환과 암시적 형변환이 있다.</li>
</ul>
<h1 id="명시적-형변환">명시적 형변환</h1>
<ul>
<li><code>CAST (expression AS 데이터형식 [(길이)])</code></li>
<li><code>CONVERT (expression, 데이터형식 [(길이)])</code></li>
</ul>
<h2 id="숫자형---숫자형">숫자형 -&gt; 숫자형</h2>
<p>간단하게 각 column의 자료형에 대해 보자</p>
<pre><code class="language-sql">DESC tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ef1014eb-de4c-425f-b903-056181157420/image.png" /></p>
<p>형변환을 하지 않은 상태로 소수점을 나타내보았다.</p>
<pre><code class="language-sql">SELECT AVG(menu_price) AS '가격평균'
  FROM tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/99e8e474-87f5-4636-b4de-4db5df6c9449/image.png" /></p>
<p>소수점 아래가 3으로 반복되는 값으로 형변환을 해본다.</p>
<h3 id="unsigned-integer">UNSIGNED INTEGER</h3>
<blockquote>
<p>UNSIGNED INTEGER : 소수점 이하를 자른다.</p>
</blockquote>
<pre><code class="language-sql">SELECT CAST(AVG(menu_price) AS UNSIGNED INTEGER) AS '가격평균'
  FROM tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3a4cb848-aca5-4a86-a8ef-6ee14ba6626c/image.png" /></p>
<hr />
<h3 id="float">FLOAT</h3>
<blockquote>
<p>FLOAT : 소수점 이하 2자리까지 자른다.</p>
</blockquote>
<pre><code class="language-sql">SELECT CAST(AVG(menu_price) AS FLOAT) AS '가격평균'
  FROM tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4d4e80a6-0091-47bf-ba65-4735da038795/image.png" /></p>
<blockquote>
<p> 데이터 형식에 따르면 <code>FLOAT</code> 형은 &quot;소수 7자리까지 나와야 하는거 아닌가?&quot; 싶을 수 있어서 찾아봤는데, <code>FLOAT</code> 유형으로 변환하는 과정에서 표시 방식의 차이가 발생할 수 있고, 그로 인해 소수점 이하 2자리까지만 나온다고 한다.</p>
</blockquote>
<hr />
<h3 id="double">DOUBLE</h3>
<blockquote>
<p>DOUBLE : 소수점 12번째 자리까지 나타낼 수 있다.</p>
</blockquote>
<pre><code class="language-sql">SELECT CAST(AVG(menu_price) AS DOUBLE) AS '가격평균'
  FROM tbl_menu;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d71e4481-5d68-417b-bb68-0c76905e3603/image.png" /></p>
<hr />
<h2 id="문자---날짜">문자 -&gt; 날짜</h2>
<p>오늘 날짜인 2024년 6월 27일을 문자열로 나타내고 date 형으로 변환해보자.</p>
<pre><code class="language-sql">SELECT CAST('2024$6$27' AS DATE);
SELECT CAST('2024/6/27' AS DATE);
SELECT CAST('2024#6#27' AS DATE);</code></pre>
<blockquote>
<p>특수 문자를 구분자로 활용하기에 셋 다 같은 결과가 나온다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7143d189-2adb-4e79-b024-d843c5005741/image.png" /></p>
</blockquote>
<hr />
<h1 id="묵시적-형변환">묵시적 형변환</h1>
<h2 id="숫자---문자">숫자 -&gt; 문자</h2>
<ul>
<li>CONCAT : 2개의 문자열을 붙여준다.</li>
</ul>
<p>CONCAT을 활용해 묵시적 형변환을 할 수 있다.</p>
<pre><code class="language-sql">SELECT CONCAT(1000, '원');

-- 원래 모양
-- SELECT CONCAT(CAST(1000 AS CHAR), '원');</code></pre>
<p>1000 은 INT형, '원'은 문자열인데 위 쿼리를 실행하면 숫자 -&gt; 문자열로 형변환이 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/25229e9e-f821-448e-9c48-be2fe13565e9/image.png" /></p>
<h2 id="문자---숫자">문자 -&gt; 숫자</h2>
<p>MariaDB가 연산 시 치환하기 힘든 문자열은 0으로 치환하여 계산해주는데,</p>
<ul>
<li>제대로 치환되는 경우
```sql
SELECT 1 + '2';</li>
<li><ul>
<li>SELECT 1 + CAST('2' AS INT);<pre><code></code></pre></li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c1c7e61e-a6bb-485a-86d2-20b8a032f431/image.png" /></p>
<hr />
<ul>
<li>0으로 치환하는 경우
```sql
SELECT 1 + '김'; -- 1로 계산</li>
<li><ul>
<li>SELECT 5 &gt; '반가워';</li>
</ul>
</li>
<li><ul>
<li>반가워를 0으로 치환해서 TRUE 로 인해 1이 나온다.<pre><code></code></pre></li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/54f84ad1-6217-4d30-b122-b2ea83f5c4a1/image.png" /></p>
<hr />
<ul>
<li>문자열 속 우선순위</li>
<li><blockquote>
<p>이 경우에는 왼쪽에 있는 것이 우선순위가 된다.
```sql</p>
</blockquote>
</li>
<li><ul>
<li>문자열 속 우선순위 (문자열 속에 2가지 형이 들어있다면 왼쪽에 있는게 우선순위)
SELECT 1 + '2반가워'; -- 3으로 계산
SELECT 1 + '반가워2'; -- 1로 계산
```</li>
</ul>
</li>
</ul>