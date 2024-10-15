<h1 id="jenkins">Jenkins</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/02d94b52-4e0f-4746-a2ab-75ad52b8761b/image.png" /></p>
<p>Jenkins는 소프트웨어 개발의 지속적 통합(CI)과 지속적 배포(CD) 프로세스를 자동화하는 데 널리 쓰이는 오픈 소스 자동화 서비스이다.</p>
<p>Java로 개발된 이 도구는 버전 컨트롤 시스템(SVC)과 통합해 소프트웨어 개발 프로젝트의 빌드, 테스트, 배포 작업을 자동으로 실행한다.</p>
<hr />
<h2 id="등장-배경">등장 배경</h2>
<blockquote>
<p>2004년 카와구치 코스케(Kohsuke Kawagunchi)에 의해 처음 개발되었다. 
당시 소프트웨어 개발 프로세스에서 반복적이고 수동적인 작업들을 자동화하며 개발 주기를 단축하고 품질을 향상시키려는 필요성이 컸다.
이러한 배경 속에서 지속적 통합과 배포의 중요성이 대두되었고, 젠킨스는 이러한 요구를 충족시키는 툴로 발전했다.</p>
<p>(추가로, Jenkins는 Hudson이라는 이름으로 시작되었다가, 2011년에 Oracle과의 상표권 분쟁 후 Jenkins로 이름이 바뀌었다)</p>
</blockquote>
<hr />
<h2 id="장단점">장단점</h2>
<h3 id="장점">장점</h3>
<ul>
<li><strong>유연성:</strong> 플로그인으로 확장 가능한 구조로, 다양한 도구와의 통합을 지원한다.</li>
<li><strong>강력한 커뮤니티:</strong> 오픈 소스 프로젝트로서 활발한 커뮤니티 지원과 광범위한 플러그인 생태계를 자랑한다.</li>
<li><strong>자동화된 빌드/테스트/배포:</strong> 소프트웨어 개발 과정에서의 반복적인 작업을 자동화함으로써 생산성을 향상시킨다.</li>
</ul>
<h3 id="단점">단점</h3>
<ul>
<li><strong>학습 곡선:</strong> 젠킨스와 플러그인 시스템은 초기 설정과 관리에 있어 상당한 학습이 필요할 수 있다.</li>
<li><strong>리소스 요구량:</strong> 복잡한 프로젝트에서는 상당한 시스템 리소스를 소모할 수 있다.</li>
<li><strong>성능 이슈:</strong> 대규모 프로젝트(높은 동시성 요구)나 빠른 속도의 CI/CD 파이프라인을 요구하는 환경에서는 Jenkins를 다루기 어려울 수 있다.</li>
</ul>
<hr />
<h1 id="구성-요소">구성 요소</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/4bedddad-70a1-48d8-b588-584483a296c5/image.png" /></p>
<h2 id="마스터--에이전드-구조">마스터 &amp; 에이전드 구조</h2>
<ul>
<li><strong>마스터:</strong> 젠킨스 서버의 핵심이 되는 구성 요소로, 작업의 스케줄링(주기적 작업 설정, 이벤트 기반 또는 병렬 실행, 작업 실행 시간 등), 작업 분배, 플러그인 관리, 사용자 인터페이스 제공 등의 역할을 한다.</li>
<li><strong>에이전트:</strong> 마스터에 의해 할당된 작업을 실행하는 시스템. 다양한 환경에서 빌드를 실행할 수 있도록 도와준다.</li>
</ul>
<hr />
<h2 id="플러그인">플러그인</h2>
<p>Jenkins가 거의 모든 개발, 테스트, 배포 도구와 통합할 수 있게 도와주는 플러그인들이다.</p>
<table>
<thead>
<tr>
<th><strong>플러그인 이름</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td>Git Plugin</td>
<td>Git 저장소와의 통합을 지원하며, 소스 코드 체크아웃 등의 기능을 제공</td>
</tr>
<tr>
<td>Pipeline</td>
<td>복잡한 CI/CD 프로세스를 스크립트로 정의할 수 있는 플러그인</td>
</tr>
<tr>
<td>Maven Integration Plugin</td>
<td>Maven 빌드 시스템과의 통합을 지원</td>
</tr>
<tr>
<td>Docker Pipeline</td>
<td>Docker 명령을 Jenkinsfile 내에서 실행할 수 있게 하는 플러그인</td>
</tr>
<tr>
<td>JUnit Plugin</td>
<td>JUnit 테스트 결과를 보고하고 시각화하는 기능 제공</td>
</tr>
<tr>
<td>Blue Ocean</td>
<td>젠킨스의 사용자 인터페이스를 현대적으로 재설계한 플러그인</td>
</tr>
<tr>
<td>Credentials Binding Plugin</td>
<td>민감한 정보(비밀번호, SSH 키 등)를 안전하게 관리할 수 있는 플러그인</td>
</tr>
<tr>
<td>Slack Notification Plugin</td>
<td>빌드 상태에 대한 알림을 Slack으로 전송할 수 있게 해주는 플러그인</td>
</tr>
<tr>
<td>Gradle Plugin</td>
<td>Gradle 빌드와 통합을 지원, Jenkins 내에서 직접 Gradle 작업을 관리할 수 있게 해주는 플러그인</td>
</tr>
<tr>
<td>Publish Over SSH</td>
<td>Jenkins 빌드 프로세스 중에 생성된 파일을 SSH를 통해 원격 서버에 전송할 수 있게 해주는 플러그인</td>
</tr>
</tbody></table>
<h2 id="작업과-빌드">작업과 빌드</h2>
<ul>
<li><strong>작업(Job):</strong> 실행할 작업의 설정을 포함한다. 소스 코드 체크 아웃, 빌드 실행, 테스트 수행 등의 작업이 포함될 수 있다.</li>
<li><strong>빌드(Build):</strong> 작업의 실행 인스턴스로, 하나의 작업이 여러 번 실행될 수 있으며, 각 실행은 각기 다른 빌드 번호를 갖는다.</li>
<li><strong>빌드 파이프라인(Pipeline):</strong> Jenkins의 핵심 기능 중 하나는 빌드 파이프라인을 정의하고 관리할 수 있다는 것이다. Groovy 기반의 Jenkinsfile을 작성하여 복잡한 빌드, 테스트, 배포 작업을 코드로 정의할 수 있다.(IaC 접근 방식 채택)</li>
</ul>
<h3 id="소스-코드-관리">소스 코드 관리</h3>
<p>Git, SVN 등 다양한 소스 코드 관리 시스템과 통합할 수 있으며, 코드 변경을 감지하고 자동으로 빌드를 시작할 수 있다.</p>
<p>젠킨스는 지속적 통합과 지속적 배포를 위한 강력하고 유연한 자동화 도구로서의 역할을 수행한다.</p>
<hr />
<p>우선 Jenkins 공식사이트로 가서 stable 버전으로 설치를 했다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/eee9cafe-8434-4c12-b6ab-fa29c3494380/image.png" /></p>
<p>port 번호를 치니 저 경로로 가서 나오는 password key를 넣어줬다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b0c07e92-a4b1-420e-9f31-641225ec7c13/image.png" /></p>
<p>플러그인도 설치한 뒤 추가 설정을 해줘야 한다.</p>
<p>다 설치되면 우선 아래 사진처럼 화면이 바뀐다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5db5cac7-d8ef-4f8c-8598-30ae860031d2/image.png" /></p>
<p>여기서 mac 이라면 터미널을 열고 윈도우라면 VSCode를 열고 터미널에서 git bash를 열고 아래 명령어를 차례대로 입력하자.</p>
<ol>
<li><strong>git bash 를 활용한 RSA 키 생성하기</strong><pre><code class="language-bash"># my-jenkins 디렉토리 생성 
mkdir ./ssh-jenkins 
</code></pre>
</li>
</ol>
<h1 id="my-jenkins-디렉토리로-이동">my-jenkins 디렉토리로 이동</h1>
<p>cd ./ssh-jenkins </p>
<h1 id="ssh-디렉토리-생성">.ssh 디렉토리 생성</h1>
<p>mkdir ./.ssh </p>
<h1 id="ls--al숨어-있어--al-옵션으로-확인">ls -al(숨어 있어 -al 옵션으로 확인.)</h1>
<p>ssh-keygen -t rsa -f .ssh/ssh-jenkins-github--key </p>
<h1 id="비밀번호는-10자-이상-추천">비밀번호는 10자 이상 추천</h1>
<h1 id="ssh-디렉토리로-이동">.ssh 디렉토리로 이동</h1>
<p>cd ./.ssh </p>
<h1 id="private-key">private key</h1>
<p>cat ssh-jenkins-github--key </p>
<h1 id="public-key">public key</h1>
<p>cat ssh-jenkins-github--key.pub</p>
<pre><code>
&gt; 여기서 얻는 private key, public key는 메모장에 복사를 해두자

나머지는 저 화면에서 여러 가지 계정명 등 정보를 입력하고 젠킨스 등록을 하자.

&gt;**SSH(Secure Shell)란?**
&gt;- 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 파일을 전송할 수 있게 해주는 암호화된 네트워크 프로토콜이다.
&gt;
&gt;**RSA키란?**
&gt;- 공개키 암호화 시스템으로 SSH 연결에서 인증에 사용된다.
  1. 개인 키(Private Key): 사용자만 가지고 있는 비밀 키
  2. 공개 키(Public Key): 누구나 알 수 있는 키

---
2. `Locale` : Jenkins 관리의 Appearance에 들어가서 Defalut Language가 한국어로 되는 것을 확인할 수 있을 것이다.
![](https://velog.velcdn.com/images/jojehuni_9759/post/d4dc54f8-e1f2-4745-ae84-a92ef5fb2375/image.png)

---
3. `Publish Over SSH`
![](https://velog.velcdn.com/images/jojehuni_9759/post/76ead7ca-5d90-4cf0-918d-6a606ba3b1a5/image.png)

---
### Tools
1. **JDK installations**
![](https://velog.velcdn.com/images/jojehuni_9759/post/9469d13f-b0fc-4f9c-85f4-d1f8b6638f1a/image.png)

DashBoard 에서 tools로 가서

![](https://velog.velcdn.com/images/jojehuni_9759/post/e9dbfa4b-8a01-47fd-8f82-e60c5f603390/image.png)

이미 jdk가 있으므로 automatically를 취소하고 했다.

2. **Gradle installations**
![](https://velog.velcdn.com/images/jojehuni_9759/post/120741f8-88d4-4326-9db8-235ca8d6e524/image.png)

이건 automatically 로 했다

---
### Secutiry
이건 RSA 키 생성 이후 해야 한다.

- security 에 public key 추가
![](https://velog.velcdn.com/images/jojehuni_9759/post/526d5636-837e-45f2-addb-1fb7d73ab9fb/image.png)

```bash
security:
  gitHostKeyVerificationConfiguration:
    sshHostKeyVerificationStrategy:
      manuallyProvidedKeyVerificationStrategy:
        approvedHostKeys: |-
          github.com (public key)</code></pre><p>위의 코드에서 public key 부분에 그대로 복사 붙여넣기를 하면 된다. 사진처럼 한 칸 띄운 뒤 붙이면 된다.</p>
<hr />
<h3 id="credentials">Credentials</h3>
<ol>
<li>private 키 추가<ul>
<li>ID는 ssh key의 아이디를 활용
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/998c0410-7a73-4987-9d45-e00fa48a8d9b/image.png" /></li>
</ul>
</li>
</ol>
<p>아래의 userName은 편한대로 작성하면 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/70735abb-ee2c-4c17-bf0b-8686e764990e/image.png" /></p>
<p>이런 식으로 Add를 통해 직접 private key를 추가해줬다.</p>
<p>아래는 아까 만들었던 비밀번호를 넣었다. </p>
<hr />
<ol start="2">
<li>pipeline에서 쓸 도커 관련 설정 추가
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/377c5d3c-f688-4514-9a78-122e81b6eb18/image.png" /></li>
</ol>
<p>밑줄 그은 2곳에는 실제 Docker Hub Id, password 입력이 필요하다.</p>
<blockquote>
<p><strong>주의할 점</strong>
이거 Docker Hub에서 가입할 때 구글 로그인과 같이 oauth로 하는 사람은 Password가 저장이 안 되는건지 로그인을 하려고 하면 구글 로그인일 때만 되고, 이메일은 저장이 돼서 구글 로그인을 사용하지 않고 로그인을 할 때 비밀번호가 옳지 않다고 뜬다.
내 구글 계정의 비밀번호인데 말이다..
이것도 새로 reset해서 Docker Hub의 계정에 비밀번호를 꼭 입력해줘야 한다.. 난 이걸로 2시간을 버렸다.</p>
</blockquote>
<hr />
<p>위 2가지를 해서 만들면
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6377189e-9fbd-43a1-a39e-b9f45f41943f/image.png" /></p>
<p>이렇게 나오게 될 것이다.</p>
<blockquote>
<p>여기서도 DOCKERHUB_PASSWORD 인데 오타가 나서 다시 만들었으니 따라하는 사람이 있다면 오타를 내지 말도록.. 이거 찾기 힘듦</p>
</blockquote>
<hr />
<h2 id="webhook-및-ngrok-설정">Webhook 및 ngrok 설정</h2>
<p>이전처럼 젠킨스 설정을 한 뒤에 이제 Webhook을 하기 위해서 github에 새로운 repo를 생성할 것이다.</p>
<p><a href="https://github.com/JoJeHuni/JENKINS-TEST">Repo 링크</a>
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5649f8bc-ea3d-4ee5-ac70-b418e334897e/image.png" /></p>
<p><code>MIT-license</code> 를 선택한 뒤 만들었다. 여기서 repo 주소를 복사하자.</p>
<hr />
<p>IntelliJ에서 Spring Project를 만든다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b236db8b-ce9a-414c-922f-2207a8a9ca13/image.png" /></p>
<ul>
<li>lombok</li>
<li>spring devtools</li>
<li>spring web</li>
</ul>
<p>3개 라이브러리를 선택한 뒤 create를 한다.</p>
<hr />
<pre><code class="language-bash"># springboot 프로젝트 생성 후 해당 프로젝트 폴더에서.. 
# (.gitignore를 수정할 것(아래에 작성 된 .gitignore 활용) 
1. git init 

# git bash에서 한다면 github remote repo 주소를 shift + insert 키로 복붙 할 것 
2. git remote add origin &lt;github remote repo 주소&gt; 
3. git pull origin main 
4. git status(add 할 것들 확인) 
5. git add * 
6. git commit -m 'first commit' 
7. git push origin main</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0fa1774f-eb14-4e28-baa6-be99bfaabf75/image.png" /></p>
<p>사진은 3번까지만 돼 있지만 7번까지 쭉 했다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6cf7de06-a0de-436f-816e-1e8a5b7be4b4/image.png" /></p>
<p>이제 이전에 했던 계산기 코드와 dockerfile을 추가해주면 된다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a7a22958-a7e1-4d58-ae62-dffeff414de4/image.png" /></p>
<blockquote>
<p>주의할 점
코드에 @NonNull 어노테이션, @RequiredArgsConstructor 어노테이션 이 2가지를 꼭 지워야 한다.</p>
</blockquote>
<hr />
<h3 id="ngrok-띄우기">ngrok 띄우기</h3>
<blockquote>
<p><a href="https://ngrok.com/download">https://ngrok.com/download</a>
위 링크로 가서 본인에 맞는 버전 설치</p>
</blockquote>
<p>이후에 로그인 한 뒤 Your Authtoken으로 가</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/3bcef821-18be-4828-bc74-ef38549530b3/image.png" /></p>
<p>복사한 것을 가지고 설치했던 ngrok 실행해서 붙여서 실행해서 인증한 뒤,</p>
<pre><code>ngrok http 18080</code></pre><p>위 명령어 사용</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5418e84d-b866-429d-9cf4-5e6b221a0d87/image.png" /></p>
<hr />
<h2 id="github-repo가서-deploy-keys에-public-키-추가">github repo가서 Deploy keys에 public 키 추가</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f8b20b19-97a6-41c6-9b7e-6b7d7098b734/image.png" /></p>
<hr />
<h2 id="webhooks-설정">Webhooks 설정</h2>
<blockquote>
<p>(ngrok으로 접속하는 jenkins 경로 뒤에 반드시 /github-webhook/ 추가할 것)</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/aef8b742-116a-4093-b0fb-0d9e4f764bae/image.png" /></p>
<hr />
<h2 id="jenkins-pipe-line-script작성">jenkins pipe-line script작성</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2f51b37a-8bcf-420a-a599-fece8d77d908/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/fdd6a05b-3ef3-4752-b599-ca2d7119f989/image.png" /></p>
<p><strong>pipeline-script</strong></p>
<pre><code>pipeline {
    agent any

    tools {
        gradle 'gradle'
        jdk 'openJDK17'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_PASSWORD')
        DOCKERHUB_USERNAME = 'JoJeHuni'
        GITHUB_URL = 'https://github.com/JoJeHuni/JENKINS-TEST.git'
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker --version'
                    } else {
                        bat 'docker --version'
                    }
                }
            }
        }
        stage('Source Build') {
            steps {
                git branch: 'main', url: &quot;${env.GITHUB_URL}&quot;
                script {
                    if (isUnix()) {
                        sh &quot;chmod +x ./gradlew&quot;
                        sh &quot;./gradlew clean build&quot;
                    } else {
                        bat &quot;gradlew.bat clean build&quot;
                    }
                }
            }
        }
        stage('Container Build and Push') {
            steps {    
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        if (isUnix()) {
                            sh &quot;cp ./build/libs/*.jar .&quot;
                            sh &quot;docker build -t ${DOCKERHUB_USERNAME}/first-pipe:latest .&quot;
                            sh &quot;docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}&quot;
                            sh &quot;docker push ${DOCKERHUB_USERNAME}/first-pipe:latest&quot;
                        } else {
                            bat &quot;copy .\\build\\libs\\*.jar .&quot;
                            bat &quot;docker build -t ${DOCKERHUB_USERNAME}/first-pipe:latest .&quot;
                            bat &quot;docker login -u %DOCKER_USER% -p %DOCKER_PASS%&quot;
                            bat &quot;docker push ${DOCKERHUB_USERNAME}/first-pipe:latest&quot;
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                if (isUnix()) {
                    sh 'docker logout'
                } else {
                    bat 'docker logout'
                }
            }
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}</code></pre><p>이것까지 하면 이제 되는 것을 볼 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/7d1fc7a6-fef8-4299-9d84-ff49586d6588/image.png" /></p>
<p>12번의 실패 기록은 위에서 주의할 점들 때문에 안 됐던 것들이어서 13번째에 해낸걸로 다행이라고 생각한다..</p>
<p>그리고 수동으로 빌드를 꼭 해줘야 한다. <code>지금 빌드</code> 버튼을 꼭 누르도록.</p>