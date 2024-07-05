<p>텍스트 처리 명령어를 들어가기 전 사전 생성을 하자</p>
<pre><code class="language-shell">$ mkdir sortDir

$ touch ./sortDir/number.txt

$ touch sortDir/fruit.txt

$ cd sortDir

$ ls</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f904a72f-81b4-4bc5-8a9f-89479fa3b066/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f9d61742-f52a-4638-a5f9-047a6e39866f/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/340c5825-4aed-4373-af6c-237ce4c45234/image.png" /></p>
<hr />
<h1 id="텍스트-처리-명령어">텍스트 처리 명령어</h1>
<h2 id="wc">wc</h2>
<blockquote>
<p>파일의 행 수, 단어 수, 바이트 수를 출력할 수 있다.</p>
</blockquote>
<pre><code class="language-shell">## 행수, 단어수, 바이트 수, 파일 이름
$ wc fruit.txt

## 행만 표시
$ wc -l fruit.txt

## 단어만 표시
$ wc -w fruit.txt

## 바이트 수만 표시
$ wc -c fruit.txt</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/166af086-189a-48d3-8677-67b2b5603c3a/image.png" /></p>
<p><strong>파이프라인</strong>을 통해 루트 디렉토리의 파일과 디렉토리가 총 몇 개인지도 알 수 있다.</p>
<pre><code class="language-shell">## 루트 디렉토리
$ ls / | wc -l</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/c98eb47f-7fe0-493b-a3d5-44934d857555/image.png" /></p>
<hr />
<h2 id="tr-명령어">tr 명령어</h2>
<pre><code class="language-shell">cat fruit.txt | tr ' ' '\n'</code></pre>
<p>앞의 프로세스의 결과를 뒤의 프로세스로 변경해준다.
즉, 띄어쓰기 되어 있는 공백을 줄바꿈으로 바꿔준다는 것이다.</p>
<pre><code>mango banana pineapple</code></pre><p>형태였던 텍스트를</p>
<pre><code>mango
banana
pineapple</code></pre><p>과 같은 형태로 바꿔줬다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/60eedbb2-4f7f-4fb0-953b-7362b3f613f9/image.png" /></p>
<hr />
<h2 id="sort-명령어">sort 명령어</h2>
<p>정렬을 해주는 명령어
위의 tr과 함께 해서 또 파이프라인을 추가하면</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/65667cca-49e2-46a2-82ec-3f6f0ac463b7/image.png" /></p>
<p>이런 형태로 문자열을 기준으로 정렬할 수 있다.</p>
<blockquote>
<p>-n 옵션을 추가하면 문자열이 아닌 숫자로 판단
<strong>위의 텍스트에는 숫자가 없어서 정렬 불가.</strong></p>
</blockquote>
<blockquote>
<p>-r 옵션을 추가하면 역순으로 정렬됨
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ac3361cb-7417-4445-a3a9-a7061033e5ea/image.png" /></p>
</blockquote>
<h2 id="head-tail-명령어">head, tail 명령어</h2>
<p><code>head</code> : 파일의 처음 부분 출력
<code>tail</code> : 파일의 마지막 부분 출력</p>
<table>
<thead>
<tr>
<th>옵션</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>-n number</td>
<td>number 수 만큼 출력한다.</td>
</tr>
<tr>
<td>-c number</td>
<td>number 바이트 만큼 출력한다.</td>
</tr>
<tr>
<td>-q</td>
<td>여러 개의 파일을 출력할 때 제목을 출력하지 않는다.</td>
</tr>
<tr>
<td>-f</td>
<td>내용이 변경될 때마다 실시간으로 출력해주며 로그파일 모니터링 등에 활용된다.(tail만 해당)</td>
</tr>
</tbody></table>
<pre><code class="language-shell">## 마지막 4행만 보여준다.
$ tail -n 4 /etc/passwd
$ cat /etc/passwd | tail -n 4

## 처음 4행만 보여준다.
$ head -n 4 /etc/passwd
$ cat /etc/passwd | head -n 4</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a29d245d-1768-4200-ae6b-377382de0957/image.png" /></p>
<hr />
<h2 id="uniq-명령어">uniq 명령어</h2>
<p>같은 내용이 연속되어 있는 경우 중복 제거</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/20b37796-b789-4adb-9a01-4b0bb48f5692/image.png" /></p>
<blockquote>
<p>-c 옵션을 주어 중복 데이터 개수를 셀 수도 있다.</p>
</blockquote>
<pre><code class="language-shell">## 연속된 중복 데이터 카운팅
$ uniq -c fruit.txt

## 정렬 후 연속된 중복 데이터 카운팅(정렬 후에 중복을 카운팅하면 중복 값을 모두 확인할 수 있다.)
$ sort fruit.txt | uniq -c</code></pre>
<hr />
<h2 id="diff-명령어">diff 명령어</h2>
<p>fruit.txt
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/21c183be-28f0-4701-86fc-6b4fab526e68/image.png" /></p>
<p>fruit2.txt
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6edb6e55-d754-4224-b3be-9171a4c92c21/image.png" /></p>
<p>비슷해보이지만 차이가 있다.</p>
<pre><code class="language-shell">diff fruit.txt fruit2.txt

## 결과는 fruit.txt의 12번째 줄부터 16번째 행이 fruit2.txt의 12번째 줄부터 14번째 줄로 변경되었음을 나타낸다.</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/107e03d7-cffe-41ff-87a2-9f6fee9e3264/image.png" /></p>