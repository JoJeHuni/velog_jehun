<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3a59a30e-f5ab-4808-984a-2c4808f6a1a2/image.png" /></p>
<p><strong>디스패처 서블릿</strong> : 요청을 해결할 수 있는 적당한 메소드를 연결해주거나, View Resolver라는 적당한 View를 찾아주는 것과 연결하는 역할</p>
<ul>
<li>디스페처 서블릿은 'Rest api' 라고 하여 View Resolver를 들르지 않고 바로 응답할 수 있다.</li>
</ul>
<p><strong>핸들러 매핑</strong> : 어느 컨트롤러 속 메소드가 요청을 해결할 수 있는지 찾아서 매핑해주는 역할</p>
<ul>
<li>M(Model): View와 Controller를 제외한 나머지(비즈니스 로직, 도메인 관련)</li>
<li>V(View): 화면과 관련된 부분</li>
<li>C(Controller):  화면에서 넘어온 값을 가공 처리하고 응답 시 화면을 선택하는 부분</li>
</ul>
<p>위의 개념을 간단하게 알고 처리 과정을 확인해보자.</p>
<ol>
<li>Client 에게 요청이 들어온다.</li>
<li>디스패처 서블릿 - 핸들러 매핑이 연결돼 들어온 요청을 해결할 수 있는 <strong>Controller</strong>의 메소드를 매핑한다.</li>
<li>3번 과정에서 매핑했던 컨트롤러와 연결한 뒤</li>
<li>Service -&gt; DAO -&gt; DB 등 원하는 요청을 해결하는 비즈니스 로직이 실행된다.</li>
<li>해당 응답의 데이터가 담긴 것 (<strong>Model</strong>) 을 다시 디스패처 서블릿이 받는다.</li>
<li>응답에 맞는 화면을 찾기 위해 View Resolver와 연결되고, <strong>View</strong>를 찾은 뒤</li>
<li>Client 에게 응답을 보낸다.</li>
</ol>