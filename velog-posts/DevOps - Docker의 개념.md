<h1 id="docker">Docker</h1>
<p>도커는 리눅스 응용 프로그램들을 프로세스 격리 기술들을 사용해 컨테이너로 실행하고 관리하는 오픈 소스 프로젝트이다.</p>
<blockquote>
<p>컨테이너 기반 오픈소스 가상화 플랫폼라고 보면 된다.</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a0abaaee-159d-4341-ae8b-b39a0dbbf988/image.png" /></p>
<p>도커 컨테이너는 일종의 소프트웨어를 소프트웨어의 실행에 필요한 모든 것을 포함하는 완전한 파일 시스템 안에 감싼다. </p>
<p>여기에는 코드, 런타임, 시스템도구, 시스템 라이브러리 등 서버에 설치되는 무엇이든 아우른다. 이는 실행 중인 환경에 관계 없이 언제나 동일하게 실행될 것을 보증한다.</p>
<blockquote>
<p>출처 : <a href="https://www.docker.com/why-docker/">도커 공식 홈페이지</a></p>
</blockquote>
<hr />
<h2 id="도커-이미지와-컨테이너">도커 이미지와 컨테이너</h2>
<table>
<thead>
<tr>
<th>구분</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>이미지</td>
<td>서비스 운영에 필요한 서버 프로그램, 소스코드 및 라이브러리, 컴파일 된 실행 파일을 묶는 형태 -&gt; <strong>도커 이미지</strong> 즉, 특정 프로세스를 실행하기 위한 모든 파일과 설정값을 지닌 파일들의 묶음이다.</td>
</tr>
<tr>
<td>컨테이너</td>
<td>이미지를 실행한 상태, 응용 프로그램의 종속성과 함께 응용프로그램 자체를 패키징 or 캡슐화 하여 격리된 공간에서 프로세스를 동작시키는 기술을 의미한다.</td>
</tr>
</tbody></table>
<hr />
<h2 id="사용-이유">사용 이유</h2>
<h3 id="docker의-장점">Docker의 장점</h3>
<ul>
<li>VM을 사용하지 않고 도커 엔진을 이용하여 동작하기 때문에 성능 개선과 동시에 메모리 용량을 적게 요구한다.</li>
<li>컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에, 새로운 환경에서 이것 저것 설치할 필요 없이 새로운 서버에 이미지만 다운받아서 컨테이너를 생성할 수 있다.</li>
<li>개발환경 설정할 때 초기 세팅이 빠르고 실행환경을 강제화할 수 있다.</li>
<li>도커는 개발자가 원하는 환경 세팅을 모듈식 유닛을 조합함으로써 만들수 있게 해준다. 이는 개발 주기, 기능 배포, 버그 수정의 속도를 높여준다.</li>
</ul>
<h2 id="도커의-도구">도커의 도구</h2>
<table>
<thead>
<tr>
<th>명칭</th>
<th>로고</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>도커 컴포즈(Docker compose)</td>
<td><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e9d6dedd-9431-4843-a1d5-b12816827b56/image.png" /></td>
<td>멀티 컨테이너 도커 애플리케이션을 정의하고 실행하는 도구이다. <br /> YAML 파일을 사용하여 애플리케이션의 서비스를 구성하며 하나의 명령을 가지고 모든 컨테이너의 생성 및 시작 프로세스를 수행한다.</td>
</tr>
<tr>
<td>도커 스웜(Docker swarm)</td>
<td><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9f9a0aba-9954-4a6d-9239-93cc09afde0b/image.png" /></td>
<td>도커 컨테이너의 네이티브 클러스터링 기능을 제공하며 도커 엔진을 하나의 가상 도커 엔진으로 탈바꿈시킨다. <br /> 도커 1.12 이상부터 Swarm 모드가 도커 엔진에 통합되어 있다.</td>
</tr>
</tbody></table>