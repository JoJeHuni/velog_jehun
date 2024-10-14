<h1 id="ingress">Ingress</h1>
<p>이전에는 frontend, backend 를 vue 와 springboot를 활용해서</p>
<p>각각 dev, ser 컨테이너를 쿠버네티스를 활용해서 manifest 형태로 정의한 뒤 적용을 해뒀다.</p>
<p>그 상태에서 게이트 웨이와 비슷한 역할을 하는 Kuberenetes Ingress에 대해 알아볼 것이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/077114fb-fa80-4418-aa25-07c0683d0b16/image.png" /></p>
<blockquote>
<p><strong>Kubernetes Ingress</strong> : 클러스터 외부에서 내부 서비스로의 HTTP 및 HTTPS 트래픽을 관리하는 API 객체</p>
<p>로드밸런싱, SSL 종단, 이름 기반의 가상 호스팅 제공, 복잡한 라우팅 규칙을 정의하는 역할로 단일 IP의 단일 진입점 역할을 하고 URL 경로나 호스트 이름을 기반으로 적절한 서비스로 라우팅한다.</p>
</blockquote>
<p><del>예전에 적용해봤던 게이트웨이와 비슷한 역할을 하는 것 같다.</del></p>
<p>그럼 Ingress 관련 파일을 작성해보자.</p>
<hr />
<h2 id="ingress-용-파일">Ingress 용 파일</h2>
<p>우선 Vue 프로젝트 파일을 선택한 뒤 터미널을 열자.</p>
<p><code>npm run build</code> 입력 시 정적 리소스로 현재 vue 프로젝트에 관한 js, css 파일을 담는다.</p>
<ul>
<li>dist 폴더에서 확인 가능하다.</li>
</ul>
<p>그 다음 아래 코드를 보자.
전체적으로는 Docker.file을 수정하여 이후 docker image를 만들어 docker hub에 push까지 진행할 것이다.</p>
<p>우선 Docker.file을 수정해보자.</p>
<pre><code># FROM node:lts-alpine

# # curl 설치(client-url): 필수는 아님(postman 같은 것)
# RUN apk add --no-cache curl

# WORKDIR /app
# COPY . ./
# RUN npm install

# # 컨테이너가 시작될 때 실행할 명령어를 지정한다.
# # npm run dev: Vite 개발 서버를 실행한다.
# # --: npm 명령어의 옵션과 Vite 명령어의 옵션을 구분한다.
# # --host 0.0.0.0: Vite 서버를 모든 네트워크 인터페이스에서 접근 가능하도록 설정한다.
# #                 이는 Docker 컨테이너 외부에서 애플리케이션에 접근할 수 있게 한다.
# CMD [&quot;npm&quot;, &quot;run&quot;, &quot;dev&quot;, &quot;--&quot;, &quot;--host&quot;, &quot;0.0.0.0&quot;]

FROM node:lts-alpine AS build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# nginx 이미지를 사용하여 프로덕션 스테이지를 구성한다.
FROM nginx:stable-alpine AS production-stage
# 빌드 스테이지에서 생성된 정적 파일들을 Nginx의 기본 웹 서버 디렉토리로 복사한다.
# 이렇게 하면 Nginx가 Vue.js 애플리케이션의 빌드된 파일들을 서빙할 수 있게 된다.
COPY --from=build-stage /app/dist /usr/share/nginx/html

# 로컬 디렉토리의 nginx.conf 파일을 Nginx의 설정 디렉토리로 복사한다.
# 이 설정 파일은 Nginx의 동작을 커스터마이즈하는데 사용된다.
# 예를 들어, 라우팅 규칙, SSL 설정, 로깅 등을 정의할 수 있다.
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD [&quot;nginx&quot;, &quot;-g&quot;, &quot;daemon off;&quot;]</code></pre><p>Vue 프로젝트에 있던 도커 파일을 수정한다.</p>
<p>그리고 위의 파일을 보면 <code>nginx.conf</code> 도 작성해줘야한다는 것을 볼 수 있는데 Kubernetes 클러스터에서 Nginx 컨테이너는 Pod의 일부로 실행되며 build 된 vue의 정적 파일을 배포하는 웹서버 역할을 하고 있다.</p>
<p><strong>nginx.conf</strong> 파일도 vue 프로젝트에 추가</p>
<pre><code># Nginx 서버 설정 블록
server {
    # 80번 포트에서 HTTP 요청을 수신
    listen 80;
    # 서버 이름을 localhost로 설정
    server_name localhost;

    # 모든 요청 처리
    location / {
        # '/usr/share/nginx/html/' 디렉토리를 루트로 설정
        root /usr/share/nginx/html/;
        # 요청된 URI에 해당하는 파일을 찾고, 없으면 '/index.html'로 폴백
        try_files $uri $uri/ /index.html;
    }
}</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9c1dcd2a-d452-4677-ba6c-059b7158416c/image.png" /></p>
<p>그럼 기존에 했던 5173이 아닌, 80 포트에서 HTTP 요청을 수신하는 것을 알 수 있어서 새로운 dep, ser가 필요하다.</p>
<blockquote>
<p>추가해준 뒤 변동 사항이 생겼으니 <code>docker build -t jojehuni/nine_v_proj .</code> 을 해줘야 한다. (근데 어차피 아래에서 App.vue를 수정할 것이라서 또 해줘야 한다.)</p>
<blockquote>
</blockquote>
<p>위의 명령을 터미널 창에서 해주면 <code>npm num build</code> 또한 해주는데
그 때는 vue 프로젝트를 각각 하나의 js 파일, css 파일로 <code>uglyfy</code> 형태로 바꿔서 프로젝트에 대한 정보를 백엔드에서 jar 파일 만들듯이 <strong>dist 폴더 속에 만들어준다.</strong></p>
<blockquote>
</blockquote>
<p>그 내용도 COPY 명령어에 있으니 확인할 수 있을 것이다.</p>
</blockquote>
<hr />
<p><strong>vue002dep.yml</strong></p>
<pre><code class="language-yaml">apiVersion: apps/v1
kind: Deployment
metadata:
  name: vue002dep
spec:
  selector:
    matchLabels:
      app: vue002kube
  template:
    metadata:
      labels:
        app: vue002kube
    spec:
      containers:
      - name: vue-container
        image: jojehuni/nine_v_proj:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 80      # 5173이 아니라 80으로 바꿔야 한다.</code></pre>
<hr />
<p><strong>vue002ser.yml</strong></p>
<pre><code class="language-yaml">apiVersion: v1
kind: Service
metadata:
  name: vue002ser
spec:
  type: ClusterIP
  ports:
  - port: 8000
    targetPort: 80      # 5173이 아니라 80으로 바꿔야 한다.
  selector:
    app: vue002kube</code></pre>
<hr />
<p>Vue 프로젝트에 002 버전으로 만들었으니 Spring 프로젝트에도 필요할 것이다.</p>
<p><strong>boot002dep.yml</strong></p>
<pre><code class="language-yaml">apiVersion: apps/v1
kind: Deployment
metadata:
  name: boot002dep
spec:
  selector:
    matchLabels:
      app: boot002kube
  # replicas: 3
  replicas: 1
  template:
    metadata:
      labels:
        app: boot002kube
    spec:
      containers:
      - name: boot-container
        image: jojehuni/nine_b_proj:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 7777</code></pre>
<hr />
<p><strong>boot002ser.yml</strong></p>
<pre><code class="language-yaml">apiVersion: v1
kind: Service
metadata:
  name: boot002ser
spec:
  type: ClusterIP
  ports:
  - port: 8001
    targetPort: 7777
  selector:
    app: boot002kube</code></pre>
<hr />
<p><strong>ingress02.yml</strong></p>
<pre><code class="language-yaml">apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sw-camp-ingress
  annotations:
    # 기본적으로 NGINX Ingress 컨트롤러는 모든 HTTP 트래픽을 HTTPS로 리다이렉트한다.
    # 이 설정을 &quot;false&quot;로 지정하면 HTTP 요청을 HTTPS로 자동 리다이렉트 하지 않는다.
    # 개발 환경이나 SSL/TLS가 필요 없는 상황에서 유용하다.
    nginx.ingress.kubernetes.io/ssl-redirect: &quot;false&quot;
    # 매칭된 경로에서 /boot 부분을 제거하고 나머지 부분만 백엔드 서비스로 전달한다.
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  # 사용할 Ingress 컨트롤러의 클래스를 지정. 여기서는 NGINX Ingress 컨트롤러를 사용하도록 설정
  ingressClassName: nginx  
  rules:
  - http:
      paths:
      - path: /()(.*)
        # rewriter와 관련된 사이트 주소: https://kubernetes.github.io/ingress-nginx/examples/rewrite/
        # ImplementationSpecific은 Ingress 컨트롤러의 구현에 따라 경로 매칭 방식이 결정된다.
        # NGINX Ingress Controller의 경우, 정규 표현식을 포함한 더 유연한 라우팅 규칙을 적용할 수 있다.
        # 이는 Prefix나 Exact보다 더 복잡한 경로 매칭을 가능하게 한다.
        # 예: /, /about, /users 등
        pathType: ImplementationSpecific
        backend:
          service:
            name: vue002ser
            port: 
              number: 8000
      - path: /boot(/|$)(.*)
        # 여기서도 ImplementationSpecific을 사용하여 /boot로 시작하는 모든 경로를 매칭한다.
        # (/|$)는 /boot 다음에 /가 오거나 문자열이 끝나는 경우를 모두 포함한다.
        # (.*)는 그 뒤의 모든 문자열을 캡처한다.
        # 예: /boot, /boot/, /boot/plus, /boot/users 등
        pathType: ImplementationSpecific
        backend:
          service:
            name: boot002ser
            port: 
              number: 8001</code></pre>
<hr />
<p>폴더 구조는 아래 이미지와 같다.
<img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0204cfb2-7b61-4e73-bd24-da7e9a234c94/image.png" /></p>
<hr />
<h3 id="ingress-설정-후-기존-코드와-달라져야-할-부분">Ingress 설정 후 기존 코드와 달라져야 할 부분</h3>
<p>ingress 설정 후에는 프론트의 워커노드를 통해서 접근하는 것이 아니라 클러스터 내부에서의 Vue 서비스 접근을 통해서 접근하게 된다.
따라서 <strong>CORS 처리를 하지 않아도 된다.</strong></p>
<p>그 외에도 이런 방식을 사용하는 이유</p>
<ol>
<li><strong>유연성</strong>: 전체 URL을 하드코딩하는 대신 다른 도메인이나 환경(개발, 스테이징, 프로덕션)에서 실행될 때 쉽게 적응</li>
<li><strong>보안:</strong> 전체 URL을 노출하지 않음으로써, 서버의 실제 주소가 숨겨짐</li>
<li><strong>리버스 프록시 호환성:</strong> 프록시(요청을 적절한 백엔드 서버로 라우팅) 뒤에서 실행될 때 유용</li>
<li><strong>Ingress 설정과의 호환성</strong>: K8S의 Ingress나 다른 유사한 시스템에서는 특정 경로(<code>/boot</code>)를 특정 서비스로 라우팅하도록 설정할 수 있음</li>
<li><strong>환경 독립성</strong>: 프론트엔드 코드가 특정 백엔드 URL에 의존하지 않아 다양한 환경에서 동일한 코드를 사용할 수 있음</li>
</ol>
<p>그래서 프론트엔드에서 백엔드의 워커노드로 요청하는 부분을 수정할 것이다. 
즉, 이전에 했던 30001 포트가 아닌 절대 경로로 통신한다.</p>
<p><strong>App.vue</strong></p>
<pre><code class="language-vue">&lt;template&gt;
  &lt;div class=&quot;plus&quot;&gt;
    &lt;h1&gt;덧셈 기능 만들기&lt;/h1&gt;
    &lt;label&gt;num1: &lt;/label&gt;&lt;input type=&quot;text&quot; v-model=&quot;num1&quot;&gt;&amp;nbsp;
    &lt;label&gt;num2: &lt;/label&gt;&lt;input type=&quot;text&quot; v-model=&quot;num2&quot;&gt;&amp;nbsp;
    &lt;button @click=&quot;sendPlus&quot;&gt;더하기&lt;/button&gt;
    &lt;hr&gt;
    &lt;p&gt;`{{ num1 }} + {{ num2 }} = {{  result }}`&lt;/p&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;script setup&gt;
  import { ref } from 'vue';

  const num1 = ref(0);
  const num2 = ref(0);
  const result = ref(0);

  const sendPlus = async() =&gt; {
    /* Ingress 적용 이전 30001번의 백엔드 워커노드와 통신 */
    // const response = await fetch(`http://localhost:30001/plus?num1=${num1.value}&amp;num2=${num2.value}`);

    /* Ingress를 활용한 절대 경로로 통신 */
    const response = await fetch(`/boot/plus?num1=${num1.value}&amp;num2=${num2.value}`);

    const data = await response.json();
    result.value = data.sum;
  }

&lt;/script&gt;

&lt;style scoped&gt;
  div.plus {
    align-items: center;
  }
&lt;/style&gt;</code></pre>
<p>위에서 말한대로 <code>docker build -t jojehuni/nine_v_proj .</code> 한 번 더 하자. (이번에 한 번만 해도 되긴 한다.)</p>
<hr />
<p>백엔드 프로젝트도 WebConfig 수정한 뒤 build를 다시 하자.
WebConfig</p>
<pre><code class="language-java">import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping(&quot;/**&quot;)
//                .allowedOrigins(&quot;http://localhost:5173&quot;)

                // Ingress 적용 이전 프론트 워커노드 포트에 대한 CORS 처리
//                .allowedOrigins(&quot;http://localhost:30000&quot;)

                // Ingress 적용 이후 CORS 불필요로 인한 경로 제거
                .allowedOrigins()
                .allowedMethods(&quot;GET&quot;, &quot;POST&quot;, &quot;PUT&quot;, &quot;DELETE&quot;);
    }
}</code></pre>
<p>해당 백엔드 프로젝트의 도커 파일이 존재하는 경로로 온 뒤 터미널을 열고</p>
<ul>
<li><code>docker build -t jojehuni/nine_b_proj .</code> 실행</li>
</ul>
<blockquote>
<p>이후 </p>
<ul>
<li>docker push jojehuni/nine_v_proj</li>
<li>docker push jojehuni/nine_b_proj
2가지 실행</li>
</ul>
</blockquote>
<hr />
<h1 id="ingress-활용">Ingress 활용</h1>
<p>Nginx 를 위한 Ingress 컨트롤러 설치 및 적용 코드</p>
<pre><code>kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/cloud/deploy.yaml</code></pre><p>위의 코드를 백엔드 프로젝트, 프론트엔드 프로젝트가 둘다 있는 곳에서 터미널을 열어서 한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/cb460fca-4898-4434-8873-0471a97d83af/image.png" /></p>
<p><code>kubectl get ingressclass</code> : 현재 클러스터에 설정된 Ingress 클래스 확인 명령어</p>
<p>다음은 Manifest 파일들이 있는 곳으로 가서 리소스를 배포한다.</p>
<p><code>PS C:\lecture\14_devops\chap02_k8s_manifests\section02&gt; kubectl apply -f ingress002.yml</code></p>
<p>이제 나머지 4개 dev, ser도 해주면 된다.</p>
<pre><code>kubectl apply -f vue002ser.yml
kubectl apply -f boot002ser.yml
kubectl apply -f vue002dep.yml
kubectl apply -f boot002dep.yml</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6d3b9f64-fef1-4966-91c7-f24ea70032bb/image.png" /></p>
<p>적용한 뒤 <code>kubectl get ingress</code> 를 했을 때 변화도 확인 가능하다.</p>
<p>이제 브라우저에서도 확인이 가능하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/96f366d7-bdb2-458e-9460-2339c3866e74/image.png" /></p>
<p>localhost:80 port로 해뒀는데 그냥 localhost만 적어도 되는 이유는 기본 포트가 80이기 때문이다.</p>
<blockquote>
<p>CORS도 필요 없어졌고, Ingress라는 단일 진입점이 생겨서 좋아진 것을 알 수 있다.
하지만 nginx에 대해 알고서 적용하는 것이 좋아서 단순하게만 생각하지는 말자.</p>
</blockquote>
<hr />
<p>이후에는 Jenkins를 통해서 도커 허브에 자동으로 업데이트되게끔도 해보자.</p>