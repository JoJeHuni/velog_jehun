<h1 id="elastic-beanstalk">Elastic Beanstalk</h1>
<blockquote>
<p>AWS에서 제공하는 관리형 서비스로 애플리케이션 배포와 확장성 관리를 간소화 한 것</p>
<p>개발자가 서버 인프라의 세부 설정에 신경 쓰지 않고도 웹 애플리케이션 및 서비스를 쉽게 배포하고 관리할 수 있도록 해주는 PaaS(Platform as a Service)</p>
<p>프로비저닝, 로드 밸런싱, 자동 스케일링, 모니터링 등 다양한 인프라 관리 작업을 자동화 한다.</p>
</blockquote>
<p>코드를 업로드하기만 하면 애플리케이션 배포에 필요한 환경을 자동으로 설정, 구성 및 관리한다.</p>
<p>나는 이전 게시글들을 통해 대략적인 구조를 세팅하면서 Elastic Beanstalk으로 환경을 만든 뒤 배포를 했었다.</p>
<ul>
<li><p><strong>1. 프로비저닝(Provisioning)</strong>
: 필요한 컴퓨팅 리소스(서버, 네트워크, 소프트웨어)를 준비하고 설정하는 과정이다.
필요한 하드웨어를 구성하고 소프트웨어를 설치 및 설정하여 시스템이나 서비스가 작동할 준비를 하는 것을 포함하며 자동화하거나 수동으로 할 수 있다.</p>
</li>
<li><p><strong>2. 로드 밸런싱(Load Balancing)</strong>
: 네트워크 트래픽이나 애플리케이션 요청을 여러 서버에 균등하게 분배하는 기술이다.
단일 서버 과부하 방지 및 자원의 효율적 사용으로 전체 시스템의 가용성과 성능을 향상시킨다.</p>
</li>
<li><p><strong>3. 자동 스케일링(Auto Scaling)</strong>
: 컴퓨팅 리소스(서버의 수)를 확장하거나 축소하는 기능으로 사용량이 증가할 때 자동으로 리소스를 추가하고, 사용량이 감소할 때 리소스를 줄여 비용을 절감한다.</p>
</li>
<li><p><strong>4. 모니터링(Monitoring)</strong>
: 시스템이나 네트워크의 성능을 지속적으로 관찰하고 기록하는 과정이다.
신속하게 문제를 감지하고 대응할 수 있으며, 시스템의 성능을 최적화하고 유지 관리 계획을 수립하는 데 필요한 데이터를 제공한다.</p>
</li>
</ul>
<hr />
<h2 id="장단점">장단점</h2>
<h3 id="장점">장점</h3>
<ul>
<li><strong>간편한 사용</strong>
: 개발자는 코드를 업로드만 하면 되고, 나머지 배포 및 관리는 Elastic Beanstalk가 자동으로 처리한다.</li>
<li><strong>자동 스케일링</strong>
: 애플리케이션의 트래픽 변화에 따라 자동으로 리소스를 확장하거나 축소한다.</li>
<li><strong>다양한 프로그래밍 언어 및 스택 지원</strong>
: Java, .NET, PHP, Node.js, Python, Ruby, Go 및 Docker 등 다양한 프로그래밍 언어와 개발 스택을 지원한다.</li>
<li><strong>비용 효율적</strong>
: 사용한 만큼만 비용을 지불하며, 추가적인 비용 없이 AWS의 다른 리소스와 통합된다.</li>
</ul>
<h3 id="단점">단점</h3>
<ul>
<li><strong>제한된 컨트롤</strong>
: 기본 설정은 사용하기 쉽지만, 매우 특정한 환경 설정이 필요한 경우에는 제한될 수 있다.</li>
<li><strong>비용 예측의 어려움</strong>
: 자동 스케일링은 비용 관리를 어렵게 만들 수 있으며, 트래픽이 많은 애플리케이션은 예상치 못한 비용을 발생시킬 수 있다.</li>
<li><strong>학습 곡선</strong>
: Elastic Beanstalk의 자동화된 기능들을 효과적으로 사용하기 위해서는 일정 수준의 학습이 요구된다.</li>
</ul>
<hr />
<h2 id="주요-개념">주요 개념</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0b0aed0c-f7fc-456a-a306-679d3eebc4af/image.png" /></p>
<p><a href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts-webserver.html?icmpid=docs_elasticbeanstalk_console">Web Server Environments - AWS Elastic Beanstalk</a></p>
<p>위 링크를 통해 더 자세하게 알 수 있다.</p>
<p>Environment를 위해 만든 AWS 리소스 하나에는 Elastic Load Balancer(다이어그램의 ELB), Auto Scaling 그룹, 하나 이상의 Amazon Elastic Compute Cloud(Amazon EC2) 인스턴스가 포함된다는 것과 함께 개념에 대해 알려주니 한 번 읽어보는 것도 좋다.</p>
<blockquote>
<p>간단하게 말하면 애플리케이션을 실행하는 모든 리소스를 환경으로 관리 가능하다.</p>
</blockquote>
<p><strong>Host Manager(HM)</strong>
: 소프트웨어 구성 요소가 각 Amazon EC2 인스턴스에서 실행된다.</p>
<ul>
<li>애플리케이션 배포</li>
<li>콘솔, API 또는 명령줄을 통한 검색을 위해 이벤트 및 메트릭(서버 응답 시간, CPU 사용량, 메모리 사용량, 네트워크 트래픽, 디스크 I/O 연산, 에러 수, 요청 수 등) 집계</li>
<li>인스턴스 수준 이벤트 생성</li>
<li>애플리케이션 로그 파일에서 중요한 오류 모니터링</li>
<li>애플리케이션 서버 모니터링</li>
<li>인스턴스 구성 요소 패치</li>
</ul>
<hr />
<h2 id="동작원리">동작원리</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1d2b663c-c65a-4de8-a0fc-28f02b99c0dc/image.png" /></p>
<ul>
<li>Elastic Beanstalk를 사용하여 애플리케이션을 구성하고, 애플리케이션 코드 번들(WAR나 JAR) 형태로 애플리케이션 버전을 업로드</li>
<li>코드를 실행하는데 필요한 AWS 리소스는 Elastic Beanstalk에서 자동으로 생성 및 구성</li>
<li>환경이 시작되면 환경을 관리하고 새로운 애플리케이션 버전을 배포 가능</li>
</ul>
<ol>
<li><strong>Create Application (애플리케이션 생성)</strong>:<ul>
<li>Elastic Beanstalk에서 첫 번째 단계는 애플리케이션을 생성하는 것이다. 여기서 애플리케이션은 관리할 모든 버전, 환경, 구성 등을 포함하는 컨테이너 역할을 한다.</li>
</ul>
</li>
<li><strong>Upload Version (버전 업로드)</strong>:<ul>
<li>개발자가 애플리케이션의 새 버전을 준비하고 난 후, 이 버전을 Elastic Beanstalk에 업로드한다. 이 버전은 실행 가능한 코드, 라이브러리, 의존성 파일 등을 포함할 수 있다.</li>
</ul>
</li>
<li><strong>Launch Environment (환경 시작)</strong>:<ul>
<li>업로드된 버전을 기반으로 하여 실행 환경을 시작한다. Elastic Beanstalk은 자동으로 AWS 리소스(예: EC2 인스턴스, 데이터베이스 인스턴스)를 프로비저닝하고 설정한다. 이 환경은 특정 버전의 애플리케이션을 호스팅하는 데 필요한 모든 것을 포함한다.</li>
</ul>
</li>
<li><strong>Manage Environment (환경 관리)</strong>:<ul>
<li>애플리케이션을 실행하고 있는 환경의 관리 단계이다. 이 단계에서는 모니터링, 로깅, 알림 설정, 환경 구성 변경, 스케일링 등의 작업을 수행할 수 있다. 또한 문제가 발생하면 진단 및 수정을 진행한다.</li>
</ul>
</li>
<li><strong>Deploy New Version (새 버전 배포)</strong>:<ul>
<li>애플리케이션의 새로운 버전이 준비되면, 이를 다시 업로드하고 기존 환경에 배포하여 업데이트한다. 이 과정은 최소 중단으로 새 버전을 적용할 수 있도록 설계되었다.</li>
</ul>
</li>
<li><strong>Update Version (버전 업데이트)</strong>:<ul>
<li>기존 환경에 설치된 애플리케이션의 버전을 업데이트하는 과정이다. 이는 성능 개선, 버그 수정, 새 기능 추가 등의 이유로 수행될 수 있다.</li>
</ul>
</li>
</ol>
<hr />
<h2 id="실습">실습</h2>
<p>내가 배포할 때 github-action을 활용해서 했었는데 그것과 관련해서 RDS 세팅부터 사용했던 deploy.yml에 대해 설명하겠다.</p>
<p>우선 내가 했던 백엔드 deploy.yml에 대해서 github에 올려둔 것을 함께 확인할 수 있도록 올린다.</p>
<p><a href="https://github.com/LearnsMate/LearnsMateBackend/blob/main/.github/workflows/deploy.yml">deploy.yml</a></p>
<p>위와 같이 deploy.yml을 활용해서 했었는데</p>
<pre><code class="language-yaml">        env:
          RDS_HOSTNAME: ${{ secrets.RDS_HOSTNAME }}
          RDS_PORT: ${{ secrets.RDS_PORT }}
          RDS_DB_NAME: ${{ secrets.RDS_DB_NAME }}
          RDS_USERNAME: ${{ secrets.RDS_USERNAME }}
          RDS_PASSWORD: ${{ secrets.RDS_PASSWORD }}
          SECRET_KEY: ${{ secrets.SECRET_KEY }}
          TOKEN_SECRET: ${{ secrets.JWT_SECRET }}</code></pre>
<p>여기에 <code>secrets.</code> 과 같은 형태로 해서 GitHub에서 Repository -&gt; <code>Settings</code> 에서
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e3de5c7f-7f54-495f-99e2-7393d8da6ee6/image.png" />
Actions에서 <code>New repository secret</code> 버튼을 통해 정의할 수 있다.</p>
<p>등등 여러가지 본인의 프로젝트에서 적용하면서 비밀로 해야 하는 것들을 저장해두고 환경 변수를 통해 나타낼 수 있다.</p>
<p>추가로 Elastic Beanstalk 을 만들 때도 그 변수를 정의해줘야 하는 것들이 있는데 그것 또한 고려해서 작성하겠다.</p>
<hr />
<h3 id="rds-세팅">RDS 세팅</h3>
<p>나는 MariaDB를 사용했었는데 세팅하는 과정을 적어보자면</p>
<ol>
<li><p><code>표준 생성</code></p>
</li>
<li><p><code>MariaDB</code>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0a35faf6-02c0-42ba-97e2-e882209d19b6/image.png" /></p>
</li>
<li><p>인스턴스 식별자는 편한대로 <code>마스터 사용자 이름</code>, <code>마스터 암호</code>는 작성하고 꼭 기억해두기</p>
</li>
<li><p><code>VPC : 새 VPC 생성</code></p>
</li>
<li><p><code>DB 서브넷 그룹 : 새 DB 서브넷 그룹 생성</code></p>
<blockquote>
<p>default vpc보단 vpc를 새로 만들어서 하는 것이 좋다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/879a5e76-cc52-4f3a-a779-39ec1ec713c4/image.png" /></p>
</blockquote>
</li>
<li><p>이후로는 <code>퍼블릭 액세스 : 예</code> 로 바꿔준 뒤 그대로 생성하면 된다.</p>
</li>
<li><p>만들어진 해당 default 보안그룹을 RDS에서 확인할 수 있는데 해당 보안그룹 클릭</p>
</li>
<li><p>인바운드 <code>규칙 추가</code> -&gt; 유형 : <code>MYSQL/Aurora</code> -&gt; 소스 <code>Anywhere-IPv4</code></p>
<blockquote>
<p>위 과정을 추가하면 3306 포트를 추가해주게 된다.</p>
</blockquote>
</li>
</ol>
<p>RDS 세팅은 이게 끝이다.</p>
<p>나는 위의 deploy.yml에서</p>
<ul>
<li><code>RDS_DB_NAME</code> : learnsmatedb</li>
<li><code>RDS_USERNAME</code> : intbyte</li>
</ul>
<p>이런 식으로 github secrets에 넣어뒀었다.</p>
<blockquote>
<p>RDS 계정을 통해 접근해 DB 활용에 어려움을 느낄까봐 추가한다.
우선 본인의 RDS 마스터 사용자 계정, 암호를 통해 접근한 뒤</p>
<pre><code>SHOW databases;

-- mysql 데이터베이스로 계정 정보 확인하기
USE mysql;    -- 기본 적으로 제공되는 mysql database

SELECT * FROM user;    -- mysql database에서 user를 확인해 계정이 추가된 것을 확인한다.

-- 2) 데이터베이스 생성 후 계정에 권한 부여
CREATE DATABASE learnsmatedb;

-- swcamp 계정의 권한 확인하기
SHOW GRANTS FOR 'intbyte'@'%';

GRANT ALL PRIVILEGES ON learnsmatedb.* TO 'intbyte'@'%';    -- learnsmatedb에 대한 모든 권한 부여

SHOW GRANTS FOR 'intbyte'@'%';

USE learnsmatedb;</code></pre><p>이런 형태로 권한이 주어졌는지 일단 확인한 뒤 활용할 수 있다.</p>
</blockquote>
<h3 id="추가로-봐야-할-설정-파일들-링크">추가로 봐야 할 설정 파일들 링크</h3>
<ol>
<li><p><a href="https://github.com/LearnsMate/LearnsMateBackend/blob/main/LearnsMate/.ebextensions/01-app-start.config">01-app-start.config</a></p>
</li>
<li><p><a href="https://github.com/LearnsMate/LearnsMateBackend/blob/main/LearnsMate/.platform/nginx.conf">nginx.conf</a></p>
</li>
<li><p>Procfile</p>
<pre><code>web: appstart</code></pre></li>
<li><p>build.gradle 수정 : 하나의 jar 파일만 생성 가능하도록 하는 설정이다.</p>
<pre><code>jar {
   enabled = false
}</code></pre></li>
</ol>
<blockquote>
<h3 id="유의해야할-점">유의해야할 점</h3>
<p>나는 deploy.yml을 보면 프로젝트 파일과 내가 deploy.yml을 해둔 파일과 위치가 달라서 <code>Learnsmate</code> 라는 프로젝트와 같은 위치에 <code>.github</code> 패키지가 있다.
그래서 프로젝트 안에 없기 때문에 <code>working-directory: LearnsMate</code> 라는 코드를 deploy.yml 곳곳에 작성해둔 것을 확인할 수 있을 것이다.
나와 다르게 프로젝트 안에 넣어둔 경우에는 deploy.yml을 다르게 작성해야 한다.
반드시 <code>Generate deployment package</code> 부분에서 본인의 위치에 맞게 deploy.zip을 생성하게끔 유의하며 사용하자.</p>
</blockquote>
<hr />
<h2 id="elastic-beanstalk-생성하기">Elastic Beanstalk 생성하기</h2>
<h3 id="백엔드">백엔드</h3>
<ol>
<li><p>애플리케이션 이름</p>
</li>
<li><p>환경 이름</p>
<pre><code>application_name: intbyte-env
environment_name: Intbyte-env</code></pre><p>나는 이런 식으로 했었다.</p>
</li>
<li><p>플랫폼 : Java</p>
</li>
<li><p>IAM EC2 역할 생성</p>
</li>
</ol>
<ul>
<li>aws-elasticbeanstalk-service-role</li>
<li>aws-elasticbeanstalk-ec2-role</li>
<li>EC2 키 페어 : 본인의 키 페어</li>
</ul>
<p><strong>aws-elasticbeanstalk-service-role 권한</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e00e2561-66c1-4890-ae13-9c13d9091432/image.png" /></p>
<p><strong>aws-elasticbeanstalk-ec2-role 권한</strong>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1b691cf6-53fe-4cf8-84c4-b11bb7a1987f/image.png" /></p>
<p>위와 같이 권한을 세팅해주면 된다.</p>
<ol start="5">
<li><p>VPC : 이전 게시글에서 만들었던 VPC로 선택</p>
</li>
<li><p>인스턴스 서브넷 : 가용 영역이 <code>ap-northeast-2a</code> 로 된 서브넷 2개 선택</p>
<ul>
<li>각각 public01, private01 서브넷일 것이다.</li>
</ul>
</li>
<li><p>EC2 보안 그룹 : elb 이름으로 만들었던 보안 그룹 선택 (나는 intbyte-sg-elb로 만들었었다.)</p>
</li>
<li><p>인스턴스 유형 : 일단은 <code>t3.micro</code>로 하되 늘리고 싶으면 나중에 늘릴 수 있다.</p>
<blockquote>
<p>근데 바꾸게 되면 대상 그룹에서 해제가 된다.. 난 이것 때문에 프로젝트 발표 직전에 뭐가 문제인지 열심히 찾았었는데 해제가 되니까 다시 세팅해주면 잘 될 것이다.</p>
</blockquote>
</li>
<li><p>환경 속성 : 이건 안 건드려도 된다.</p>
<blockquote>
<p>나중에 smtp로 이메일 인증을 하기 위해서 그 때는 여기에 추가를 해줘야 한다. 내 deploy.yml을 자세히 보면 SMTP 에 관해서도 환경 변수로 정의를 해뒀는데 그것과 마찬가지로 Elastic Beanstalk 만들 때 환경 변수에도 추가해주면 된다.</p>
</blockquote>
</li>
</ol>
<p>위 9가지 과정을 하게 되면 1개를 만든 것이다. 
근데 우리는 가용 영역이 a인 것과 c인 것을 만들 것이기 때문에 6번 과정에서 <code>ap-northeast-2c</code> 인 것들로 하나를 더 만들자.</p>
<p>deploy.yml에도 2개로 만들었었다. 그걸 참고해서 만들면 될 것이다.</p>
<hr />
<h3 id="프론트엔드">프론트엔드</h3>
<p>이제 프론트엔드 관련해서도 Elastic Beanstalk을 만들건데</p>
<p>프론트엔드와 백엔드는 다른 vpc 영역에 속한다. 그래서 vpc부터 새로 만들어야 한다.</p>
<h4 id="vpc">VPC</h4>
<ol>
<li>생성할 리소스 : <code>VPC 등</code> 선택, <code>이름</code>과 <code>Ipv4 CIDR 블록</code> 기입<blockquote>
<p>기존에는 <code>VPC만</code> 을 눌러서 각각 만들어줬었지만 해당 선택을 하면 이 VPC에 맞춰 여러 가지 서브넷, 인터넷 게이트웨이, 라우팅 테이블과 같은 것들을 만들어준다.</p>
</blockquote>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9e2cf57e-7ac0-4be9-82fe-3ebb49d66068/image.png" /></p>
<p>프로젝트 이름에 맞춰서 구분되게끔 위와 같이 해준 뒤</p>
<ol start="2">
<li><p><strong><code>가용 영역 수</code></strong> : <code>2</code>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b8dc8c13-50cf-49b0-93bc-289304a18d4d/image.png" /></p>
<blockquote>
<p>백엔드 VPC도 a, c로 했으니 프론트엔드 또한 a, c로 해야 한다.</p>
</blockquote>
</li>
<li><p><strong><code>퍼블릭 서브넷 수</code></strong> : <code>2</code></p>
</li>
<li><p><strong><code>프라이빗 서브넷 수</code></strong> : 0</p>
</li>
<li><p><strong><code>NAT 게이트웨이</code></strong> : 없음</p>
</li>
</ol>
<p>위 5가지를 한 뒤 생성해주면 프론트엔드 VPC가 생성이 된다.</p>
<hr />
<p>이제 프론트엔드 Elastic Beanstalk을 만들어보자.</p>
<ol>
<li><p>애플리케이션 이름</p>
</li>
<li><p>환경 이름</p>
<pre><code>application_name: intbyte-v-env
environment_name: Intbyte-v-env</code></pre><p>나는 이런 식으로 했었다.</p>
</li>
<li><p>플랫폼 : Node.js</p>
</li>
<li><p>서비스 역할 - 기존 서비스 역할 사용</p>
</li>
</ol>
<ul>
<li>aws-elasticbeanstalk-service-role</li>
<li>aws-elasticbeanstalk-ec2-role</li>
<li>EC2 키 페어 : 본인의 키 페어</li>
</ul>
<ol start="5">
<li>VPC : 위에서 만든 프론트엔드 용 VPC로 선택</li>
<li>인스턴스 서브넷 : <code>ap-northeast-2a</code>, <code>ap-northeast-2c</code> 둘 다 선택</li>
</ol>
<p>나머지는 따로 선택할 것은 없다.</p>
<h3 id="프론트엔드-설정-파일">프론트엔드 설정 파일</h3>
<ol>
<li><p><a href="https://github.com/LearnsMate/LearnsMateFrontend/blob/main/.github/workflows/deploy.yml">deploy.yml</a></p>
</li>
<li><p>Procfile</p>
<pre><code>web: npm install express &amp;&amp; npm start</code></pre></li>
<li><p><a href="https://github.com/LearnsMate/LearnsMateFrontend/blob/main/server.js">server.js</a></p>
</li>
<li><p>package.json 수정 : 주석으로 처리해둔 부분에 추가했다고 표시를 한 것</p>
</li>
</ol>
<p><strong>실행할 때는 주석 부분을 삭제해야 한다.</strong></p>
<pre><code>{
  &quot;name&quot;: &quot;veb_proj&quot;,
  &quot;version&quot;: &quot;0.0.0&quot;,
  &quot;private&quot;: true,
  &quot;type&quot;: &quot;module&quot;,
  &quot;scripts&quot;: {
    &quot;dev&quot;: &quot;vite&quot;,
    &quot;build&quot;: &quot;vite build&quot;,
    &quot;preview&quot;: &quot;vite preview&quot;,
    &quot;start&quot;: &quot;node server.js&quot;   // server.js 웹 서버 실행
  },
  &quot;dependencies&quot;: {
    &quot;express&quot;: &quot;^4.19.2&quot;,       // express dependency 추가
    &quot;vue&quot;: &quot;^3.5.12&quot;
  },
  &quot;devDependencies&quot;: {
    &quot;@vitejs/plugin-vue&quot;: &quot;^5.1.4&quot;,
    &quot;vite&quot;: &quot;^5.4.10&quot;
  }
}</code></pre>