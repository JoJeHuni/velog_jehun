<h1 id="dynamic-query">Dynamic Query</h1>
<h2 id="개요">개요</h2>
<p>일반적으로 검색 기능이나 다중 입력 처리 등을 수행할 경우, SQL을 실행하는 DAO를 여러 번 호출하거나 batch 기능을 이용하여 버퍼에 담아서 한번에 실행 시키는 방식으로 쿼리를 구현했다.</p>
<blockquote>
<p>MyBatis에서는 이를 동적으로 제어할 수 있는 구문을 제공하여 좀 더 쉽게 쿼리를 쉽게 구현할 수 있는 기능을 지원한다.</p>
</blockquote>
<h3 id="xml에서-지원-구문-종류">xml에서 지원 구문 종류</h3>
<ol>
<li>if</li>
<li>choose (when, otherwise)</li>
<li>trim (where, set)</li>
<li>foreach</li>
</ol>
<h4 id="ognl-object-graph-navigation-language">OGNL (Object Graph Navigation Language)</h4>
<p>single quatation('') 구간의 값은 리터럴 값으로 보고 그게 아닌 이름은 객체의 필드 또는 변수로 인식하게 작성하는 기법이다.</p>
<ol>
<li>gte (&gt;=) : greater than equal</li>
<li>gt (&gt;) : greater than</li>
<li>lte (&lt;=) : less than equal</li>
<li>lt (&lt;) : less than</li>
<li>eq (==) : equal</li>
<li>neq (!=) : not equal</li>
</ol>
<p>위와 같은 OGNL 기법들이 있으며 용어가 헷갈리지 않게끔 작성해뒀다. 아래부터는 지원 구문에 대해 활용해보자.</p>
<hr />
<h2 id="1-if">1. if</h2>
<ul>
<li>동적 쿼리를 구현할 때 가장 기본적으로 사용되는 구문이다.</li>
<li>(test 속성으로 작성된) <strong>특정 조건을 만족할 경우 내부의 구문을 쿼리에 포함</strong>한다.</li>
</ul>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
     WHERE writer = 'admin'
    &lt;if test=&quot;title != null&quot;&gt;
        AND title like #{title}
    &lt;/if&gt;
&lt;/select&gt;</code></pre>
<p>다중 if 구문도 가능하다.</p>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
     WHERE writer = 'admin'
    &lt;if test=&quot;title != null&quot;&gt;
        AND title like #{title}
    &lt;/if&gt;
    &lt;if test=&quot;location != null&quot;&gt;
        AND location like #{location}
    &lt;/if&gt;
&lt;/select&gt;</code></pre>
<p>이전까지 하던 메뉴 데이터를 가지고 사용한 if 구문도 함께 보자.
메뉴 or 카테고리 명으로 검색해서 메뉴 목록을 보여주는 것을 구현해보았다.</p>
<p><code>condition</code>, <code>value</code> 을 입력받아 아래와 같이 구현된 if 구문 속에서 쿼리를 포함하거나 포함하지 않는 식으로 사용해서 데이터를 출력할 수 있다.</p>
<pre><code class="language-xml">    &lt;select id=&quot;searchMenu&quot; parameterType=&quot;SearchCriteria&quot; resultMap=&quot;menuResultMap&quot;&gt;
        SELECT
               A.MENU_CODE
             , A.MENU_NAME
             , A.MENU_PRICE
             , A.CATEGORY_CODE
             , A.ORDERABLE_STATUS
          FROM TBL_MENU A
        &lt;if test=&quot;condition == 'category'&quot;&gt;
            JOIN TBL_CATEGORY B ON (A.CATEGORY_CODE = B.CATEGORY_CODE)
        &lt;/if&gt;
         WHERE A.ORDERABLE_STATUS = 'Y'
        &lt;if test=&quot;condition == 'category'&quot;&gt;
            AND B.CATEGORY_NAME = #{ value }
        &lt;/if&gt;
        &lt;if test=&quot;condition == 'name'&quot;&gt;
            AND A.MENU_NAME LIKE CONCAT('%', #{ value }, '%')
        &lt;/if&gt;
    &lt;/select&gt;</code></pre>
<p><strong>xml 속 parameterType 에 있는 SearchCreiteria</strong></p>
<pre><code class="language-java">public class SearchCriteria {
    private String condition;
    private String value;

    public SearchCriteria() {
    }

    public SearchCriteria(String condition, String value) {
        this.condition = condition;
        this.value = value;
    }
}</code></pre>
<hr />
<p><strong>출력 결과</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/17b17bbd-feab-4cd6-a3f7-469321300bc6/image.png" /></p>
<p>name과 밥이 각각 condition, value 이다.</p>
<hr />
<h2 id="2-choose-when-otherwise">2. choose (when, otherwise)</h2>
<p>Java의 if-else, switch 문과 비슷한 구문이다.</p>
<h3 id="사용법">사용법</h3>
<p>-<code>&lt;choose&gt;</code> 안의 <code>&lt;when&gt;</code>은 if 또는 case의 역할을, <code>&lt;otherwise&gt;</code>는 else / default의 역할을 한다.</p>
<ul>
<li><code>&lt;when&gt;</code>태그는 <code>&lt;if&gt;</code>태그와 형식이 흡사하며, 다중 사용할 수 있으나 하나의 조건 만족에 대해서만 구문을 추가한다.</li>
<li><code>&lt;otherwise&gt;</code>태그는 <code>&lt;when&gt;</code>태그의 조건 중 어떤 것도 만족하지 않는 경우이다.</li>
</ul>
<pre><code class="language-xml">&lt;choose&gt;
    &lt;when test=&quot; 조건식 &quot;&gt;
        쿼리 구문
    &lt;/when&gt;
    &lt;when test=&quot; 조건식 &quot;&gt;
        쿼리 구문
    &lt;/when&gt;
    . . . 
    &lt;otherwise&gt;
        쿼리 구문
    &lt;/otherwise&gt;
&lt;/choose&gt;</code></pre>
<h3 id="사용-예시">사용 예시</h3>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD 
   WHERE COMMENT != ''
    &lt;choose&gt;
        &lt;when test=&quot;title != null&quot;&gt;
            AND title like #{title}
        &lt;/when&gt;
        &lt;when test=&quot;writer != null&quot;&gt;
            AND writer like #{writer} 
        &lt;/when&gt;
        &lt;otherwise&gt;
            AND featured = 1 
        &lt;/otherwise&gt;
    &lt;/choose&gt;
&lt;/select&gt;</code></pre>
<hr />
<h2 id="3-trim-where-set-태그">3. Trim (where, set 태그)</h2>
<p>Trim에 대해 배우기 전에</p>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
   WHERE
    &lt;if test=&quot;writer != null&quot;&gt;
        writer = #{writer}
    &lt;/if&gt;
    &lt;if test=&quot;title != null&quot;&gt;
        AND title like #{title}
    &lt;/if&gt;
&lt;/select&gt;</code></pre>
<ol>
<li>if 조건 중 <strong>하나의 조건도 만족하지 못했을 때</strong></li>
<li><strong>첫 번째 조건은 만족 X면서, 2번째 조건은 만족할 때</strong></li>
</ol>
<p>위 2가지 다 오류가 생긴다.</p>
<p><strong>1번의 경우</strong></p>
<pre><code class="language-sql">SELECT * FROM BOARD
 WHERE</code></pre>
<p><strong>2번의 경우</strong></p>
<pre><code class="language-sql">SELECT * FROM BOARD
 WHERE
     AND title LIKE ' OOO '</code></pre>
<p>위 2가지 경우로 인해 오류가 발생할 때 <strong>⇒ trim (where, set) 으로 해결할 수 있다.</strong></p>
<hr />
<blockquote>
<p><code>&lt;trim&gt;</code> : 쿼리의 구문의 특정 부분을 없앨 때 쓰인다.</p>
</blockquote>
<p>태그 안의 구문에 처음 시작할 단어와 시작 시 제거해야 할 단어를 명시하여 쿼리를 완성하도록 한다.</p>
<h3 id="trim-태그의-속성">trim 태그의 속성</h3>
<ol>
<li>prefix : 처리 후 엘리먼트의 내용이 있으면 가장 앞에 붙여주는 내용 기술</li>
<li>prefixOverrides : 처리 후 엘리먼트 내용 중 가장 앞에 속성값에 해당하는 문자를 자동 삭제</li>
<li>suffix : 처리 후 엘리먼트의 내용이 있으면 가장 뒤에 붙여주는 내용 기술</li>
<li>suffixOverrides : 처리 후 엘리먼트 내용 중 가장 뒤에 속성값에 해당하는 문자를 자동 삭제</li>
</ol>
<h3 id="사용-예시-1">사용 예시</h3>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
    &lt;trim prefix=&quot;WHERE&quot; prefixOverrides=&quot;AND | OR&quot;&gt;
        &lt;if test=&quot;writer != null&quot;&gt;
            writer LIKE CONCAT('%', #{ writer }, '%')
        &lt;/if&gt;
        &lt;if test=&quot;title != null&quot;&gt;
            AND title LIKE CONCAT('%', #{ title }, '%')
        &lt;/if&gt;
    &lt;/trim&gt; 
&lt;/select&gt;</code></pre>
<p>if 구문의 각각의 조건이 만족한다면 <code>&lt;trim&gt;</code>태그 내의 구문을 생성하고, 내용이 하나라도 있으면 구문의 가장 앞에 WHERE 를 붙인다.</p>
<p>근데 위에 잘 보면 <code>prefixOverrides=&quot;AND | OR&quot;</code> 부분이 있는데 이건 만약 구문의 시작이 AND or OR 인 경우에 해당 문자를 떼고, WHERE을 붙여서 쿼리를 완성한다는 뜻이다.</p>
<p>단, 엘리먼트 내에 작성하는 구문에 WHERE를 작성하지 않아야 한다. 그렇지 않으면 WHERE이 2개 생성(WHERE WHERE)될 수 있다.</p>
<p>엘리먼트 내용이 없으면 생성이 되지 않는다.</p>
<pre><code class="language-sql">WHERE writer LIKE CONCAT('%', #{ writer }, '%')
  AND title LIKE CONCAT('%', #{ title }, '%')</code></pre>
<hr />
<h3 id="trim-에-속하는-where-구문">Trim 에 속하는 WHERE 구문</h3>
<blockquote>
<p><code>&lt;where&gt;</code> : 기존 쿼리의 WHERE 절을 동적으로 구현할 때 사용한다.</p>
</blockquote>
<ul>
<li><code>&lt;where&gt;</code> 태그는 단순히 WHERE만을 추가하지만, 만약 태그 안의 내용이 'AND' 나 'OR'로 시작할 경우 'AND'나 'OR'를 제거한다.
  단, 엘리먼트 내에 작성하는 구문에 WHERE를 작성하지 않아야 한다. 그렇지 않으면 WHERE이 2개 생성(WHERE WHERE)될 수 있다.</li>
</ul>
<p>where엘리먼트 내 모두 쿼리문이 추가되지 않는 상황이면 where를 무시한다.</p>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
    &lt;where&gt;
        &lt;if test=&quot;writer != null&quot;&gt;
            writer = #{writer}
        &lt;/if&gt;
        &lt;if test=&quot;title != null&quot;&gt;
            AND title like #{title}
        &lt;/if&gt;
    &lt;/where&gt;
&lt;/select&gt;</code></pre>
<hr />
<h3 id="trim-에-속하는-set-구문">Trim 에 속하는 SET 구문</h3>
<blockquote>
<p><code>&lt;set&gt;</code> : 기존 쿼리의 UPDATE SET 절을 동적으로 구현할 때 사용한다.</p>
</blockquote>
<ul>
<li><p>고정으로 모든 컬럼을 변경하지 않고, 주어진 일부 값에 대해서만 변경할 수 있는 동적 update 쿼리 구현을 돕는다.</p>
</li>
<li><p>쿼리 실행 시 엘리먼트 내 구문 앞에 SET을 붙이고 마지막에 끝나는 문장의 ',' 를 제거한다.</p>
</li>
</ul>
<pre><code class="language-xml">&lt;update id=&quot;updateUser&quot;&gt;
    update USER
    &lt;set&gt;
        &lt;if test=&quot;username != null&quot;&gt;
            username=#{username},
        &lt;/if&gt;
        &lt;if test=&quot;password != null&quot;&gt;
            password=#{password},
        &lt;/if&gt;
    &lt;/set&gt;
    where id=#{id} 
&lt;/update&gt;</code></pre>
<p>위 예시를 trim 구문으로 변경해보자.</p>
<pre><code class="language-xml">&lt;update id=&quot;updateUser&quot;&gt;
    update USER
    &lt;trim prefix=&quot;SET&quot; surffixOverrides=&quot;,&quot;&gt;
        &lt;if test=&quot;username != null&quot;&gt;
            username=#{username},
        &lt;/if&gt;
        &lt;if test=&quot;password != null&quot;&gt;
            password=#{password},
        &lt;/if&gt;
    &lt;/trim&gt;
    where id=#{id} 
&lt;/update&gt;</code></pre>
<p><code>surffixOverrides</code> 속성을 ','으로 설정해 구문의 마지막에 제거할 값을 명시하면 된다.
set과 마찬가지로 마지막에 ','가 제거된다.</p>
<hr />
<h2 id="foreach">foreach</h2>
<p><strong>Java의 for문과 같은 역할</strong>을 하는 것으로, 동적 쿼리를 구현할 때 collection에 대한 반복 처리를 제공한다.</p>
<table>
<thead>
<tr>
<th>속성명</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>item</td>
<td>반복될 때 접근 가능한 각 객체 변수 (반복을 수행할 때마다 꺼내올 값의 이름)</td>
</tr>
<tr>
<td>index</td>
<td>반복 횟수를 가리키는 변수</td>
</tr>
<tr>
<td>collection</td>
<td>반복에 쓰일 Collection 객체 (반복을 수행할 대상)</td>
</tr>
<tr>
<td>open</td>
<td><strong>첫 반복 시 포함할 여는 문자열</strong> 즉, <code>&lt;foreach&gt;</code> 엘리먼트 구문의 가장 앞에 올 문자 ex) ' <strong>(</strong> ' abc, efg, … )</td>
</tr>
<tr>
<td>separator</td>
<td>반복되는 객체를 나열할 때 item 사이에 사용할 <strong>구분자</strong> ex) ( abc <strong>' ,'</strong> … )</td>
</tr>
<tr>
<td>close</td>
<td><strong>마지막 반복 시 포함할 닫는 문자열</strong> 즉, <code>&lt;foreach&gt;</code> 엘리먼트 구문의 마지막에 올 문자 ex) ( abc, efg, …<strong>' ) '</strong></td>
</tr>
</tbody></table>
<pre><code class="language-xml">&lt;select id=&quot;searchBadwords&quot; resultType=&quot;arraylist&quot;&gt;
    SELECT * FROM BOARD
     WHERE TITLE IN
    &lt;foreach item=&quot;item&quot; index=&quot;index&quot; collection=&quot;list&quot; open=&quot;(&quot; separator=&quot;,&quot; close=&quot;)&quot;&gt;
        #{item}
    &lt;/foreach&gt;
&lt;/select&gt;</code></pre>
<p><strong>결과</strong></p>
<pre><code class="language-sql">SELECT * FROM BOARD
WHERE TITLE IN ( 'XXX' , '사행성', '욕설', … )</code></pre>
<hr />
<ul>
<li>parameter 객체로 값을 받아오지 않고, static 필드 혹은 static 메소드에 접근하여 직접 반환 받아 사용하는 것도 가능하다. </li>
</ul>
<p>이렇게 static 필드 혹은 메소드에 직접 접근해 사용하면 mapper 인터페이스와 service에서 파라미터 넘겨주지 않아도 된다.
    - static 필드 접근 시 collection 속성 : @풀클래스명@필드명
    - static 메소드 접근 시 collection 속성 : @풀클래스명@메소드명()</p>
<p>그냥 말로만 나타내면 헷갈릴 것이다.</p>
<pre><code class="language-xml">&lt;foreach collection=&quot;@com.section01.xml.Application@createRandomMenuCodeList()&quot; item=&quot;menuCode&quot; open=&quot;(&quot; separator=&quot;, &quot; close=&quot;)&quot;&gt;
    #{ menuCode }
&lt;/foreach&gt;</code></pre>
<p>이런 식으로 작성해주면 된다.</p>
<hr />
<h2 id="bind">bind</h2>
<p><strong>특정 문장을 미리 생성하여 쿼리에 적용해야 할 경우</strong> 사용한다.</p>
<p><code>_parameter</code> 를 통해 전달 받은 값에 접근하여 구문을 생성한다.</p>
<pre><code class="language-xml">&lt;select id=&quot;searchBoard&quot; resultType=&quot;arraylist&quot;&gt;
    &lt;bind name=&quot;pattern&quot; value=&quot;'%' + _parameter.getTitle() + '%'&quot; /&gt;
        SELECT * FROM BOARD
         WHERE title LIKE #{pattern}
&lt;/select&gt;</code></pre>