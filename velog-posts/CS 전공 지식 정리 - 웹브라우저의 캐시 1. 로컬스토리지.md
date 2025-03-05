<p><a href="https://velog.io/@jojehuni_9759/CS-%EC%A0%84%EA%B3%B5%EC%A7%80%EC%8B%9D-%EC%A0%95%EB%A6%AC-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-2.-%EB%A9%94%EB%AA%A8%EB%A6%AC">운영체제 2. 메모리</a></p>
<p>위 링크에서도 아주 간단하게 웹 브라우저의 캐시를 알아봤었는데 이번엔 사용법도 같이 알아보고자 한다.</p>
<hr />
<h1 id="로컬스토리지">로컬스토리지</h1>
<blockquote>
<p>Web Storage 객체로 브라우저 내에 키:값 형태로 origin 에 종속돼 저장되는 데이터이다.</p>
</blockquote>
<ul>
<li>키:값 형태이므로 하나의 키에는 오로지 하나의 값만 저장된다.<ul>
<li>추가로 넣고 싶다면 배열 형태로 넣는 수밖에 없다.</li>
</ul>
</li>
<li>웹 브라우저에서 데이터를 <strong>수동</strong>으로 삭제하지 않는 한 로컬 저장소에 저장되며, 만료 날짜가 없다.<ul>
<li>창이나 탭을 닫는다고 만료되는 것이 아니다.</li>
</ul>
</li>
<li>최대 용량은 5MB 이다.</li>
<li>로그인을 유지하기 위한 값, 사용자의 행위를 기억할 때 사용된다.<ul>
<li>예를 들면 1, 2, 3번 총 3개의 탭이 있을 때 2번 탭에서 한 행위는 기억된다.<ul>
<li>ex) 네이버 쇼핑에서 필터링을 걸어두는 행위 -&gt; 새로고침해도 유지됨</li>
</ul>
</li>
</ul>
</li>
<li>데이터는 자동으로 서버로 전송되지 않는다.</li>
</ul>
<blockquote>
<p>추가로 작성할 개념인 <strong>쿠키</strong>는 자동 전송되니까 차이를 알아두자.</p>
</blockquote>
<h2 id="사용법">사용법</h2>
<ul>
<li>설정 : <code>localStorage.setItem(key, value);</code></li>
<li>key에 해당하는 value가져오기 : <code>localStorage.getItem(key);</code></li>
<li>제거 : <code>localStorage.removeItem(key);</code></li>
<li>전체제거 : <code>localStorage.clear()</code></li>
</ul>
<p>개발자 도구 -&gt; <code>console</code>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cb7a683c-0ebf-4da1-8e72-5af8bd21096d/image.png" /></p>
<p>개발자 도구 -&gt; <code>application</code>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ba3b0c33-a80c-46c0-8660-3405008334ad/image.png" /></p>
<p>이런 형태로 설정도 가능하고 제거도 가능하다.</p>
<hr />
<h2 id="로컬스토리지와-오리진">로컬스토리지와 오리진</h2>
<p>위의 개념에서 로컬스토리지는 키:값 형태로 <strong>origin 에 종속돼 저장되는 데이터</strong>라고 했는데 <strong>오리진이란?</strong></p>
<p>오리진은 <code>https://www.naver.com</code> 을 예시를 들어 설명하면</p>
<ol>
<li>https: -&gt; <code>protocol</code></li>
<li><a href="http://www.naver.com">www.naver.com</a> -&gt; <code>hostname</code></li>
<li>443 포트가 있지만 생략돼 있다. -&gt; <code>port</code></li>
</ol>
<blockquote>
<p>참고) <a href="https://www.naver.com:443">https://www.naver.com:443</a> 해도 똑같이 naver가 되는 것을 알 수 있다.
-&gt; https 의 기본 포트 번호이기 때문.</p>
</blockquote>
<p>위에서 말한 <strong>protocol, hostname, port</strong> 를 묶어서 <strong>오리진</strong>이라고 한다.</p>
<p>벨로그를 예를 들면 해당 페이지는</p>
<p><code>https://velog.io</code> 까지가 <strong>오리진</strong>
물음표 이후의 <code>id=983888e3-bb05-4eb6-9aab-b93f4c09a7c5</code> 부분은 <strong>query String</strong> 이다.</p>
<p>그래서 같은 오리진이 같다면 로컬 스토리지에 저장되기 때문에 필터링을 걸어둔 &quot;행위&quot;가 기억되는 것이다.</p>
<hr />
<p>그래서 로그인 유지할 때도 사용한다.</p>
<ul>
<li>토큰 기반 방식</li>
<li>세션 기반 방식</li>
</ul>
<p>2가지가 있는데 토큰 먼저 알아보자.</p>
<p><strong>토큰 기반 방식</strong></p>
<ol>
<li>사용자 아이디 패스워드 입력 -&gt; 서버에 로그인 요청</li>
<li>서버가 <code>DB</code>를 통해 로그인 성공 시 <code>token</code> (반환)</li>
<li>이 <code>token</code> 은 <code>LocalStorage</code>에 저장된다.</li>
<li>이 저장된 <code>token</code>을 보통 <code>header-authorization</code> 이라는 <code>header</code>에 담아서 보내면 로그인이 유지될 수 있는 것이다.</li>
</ol>
<hr />
<h2 id="활용-사례--캐싱">활용 사례 : 캐싱</h2>
<p>로그인 유지뿐 아닌 추가로 더 알아보자.</p>
<p>우리가 이전에 입력했던 것들을 입력하지 않아도 <strong>자동완성</strong>으로 저장돼 있는 경우가 있다.</p>
<p>이것으로 다시 입력할 필요 없이 수고를 덜 수 있다. =&gt; <strong>UX가 좋아진다(개선된다)</strong></p>
<p>텍스트 창을 만든 뒤에 script 에</p>
<pre><code class="language-html">&lt;body&gt;
    &lt;div&gt;
        &lt;input type=&quot;text&quot; id=&quot;field&quot; /&gt;
        &lt;input type=&quot;button&quot; class=&quot;button-62&quot; value=&quot;검색&quot; id=&quot;save&quot; /&gt;
        &lt;input type=&quot;button&quot; class=&quot;button-62&quot; value=&quot;조회&quot; id=&quot;read&quot; /&gt;
        &lt;input type=&quot;button&quot; class=&quot;button-62&quot; value=&quot;삭제&quot; id=&quot;clear&quot; /&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;script&gt;
    window.onload = async () =&gt; {
        const field = document.getElementById(&quot;field&quot;),
            save = document.getElementById(&quot;save&quot;),
            read = document.getElementById(&quot;read&quot;),
            clear = document.getElementById(&quot;clear&quot;)

        save.addEventListener(&quot;click&quot;, e =&gt; localStorage.setItem(&quot;입력값&quot;, field.value))
        read.addEventListener(&quot;click&quot;, e =&gt; alert(window.localStorage[&quot;입력값&quot;]))
        clear.addEventListener(&quot;click&quot;, e =&gt; {
            window.localStorage.clear()
            field.value = &quot;&quot;
        })
        if (window.localStorage[&quot;입력값&quot;]) {
            field.value = window.localStorage[&quot;입력값&quot;]
        }
    }
&lt;/script&gt;</code></pre>
<p>이런 식으로 해서 자동완성에도 사용 가능하다.</p>