<h1 id="kubernetes">Kubernetes</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/0acc134a-f9e5-469f-af32-613f515d8c99/image.png" /></p>
<p><strong>쿠버네티스(Kubernetes, K8s)</strong>는 자동화된 컨테이너 배포, 스케일링, 관리를 제공하는 오픈소스 플랫폼으로 컨테이너 <strong>오케스레이션 도구</strong>의 일종이다.</p>
<blockquote>
<p><strong>컨테이너 오케스트레이션</strong> : 시스템 전체를 총괄하고 여러 개의 컨테이너를 관리하는 것</p>
</blockquote>
<hr />
<h2 id="등장-배경">등장 배경</h2>
<p>2013년 도커가 등장하면서 많은 서비스들이 리눅스 컨테이너 기술을 도입하며 도커라이징하게 된다.</p>
<p>하지만, 도커 이미지와 관리할 컨테이너가 늘어났고 -&gt; 인스턴스 상태 관리나 자동화 툴의 필요성이 대두되게 되었다.</p>
<p>그래서 쿠버네티스가 탄생했고, 구글 내부에서 사용하던 Borg를 기반으로 2015년에 첫 버전이 출시 되어 현재는 <strong>CNCF(Cloud Native Computing Foundation)에 기부되어 클라우드 네이티브 기술 발전을 위한 프로젝트로 관리되고 있다.</strong></p>
<hr />
<h2 id="도커-컴포즈와-쿠버네티스의-차이">도커 컴포즈와 쿠버네티스의 차이</h2>
<ul>
<li><p><strong>도커 컴포즈</strong>
옵션을 지정해 수동으로 컨테이너의 수를 바꿔야 한다.</p>
</li>
<li><p><strong>쿠버네티스</strong>
yml 파일에 정의한 설정대로 컨테이너를 생성, 삭제하며 <strong>바람직한 상태 유지</strong>
문제가 있는 컨테이너를 삭제하고 새로운 컨테이너를 만들거나, 삭제가 된다면 다시 설정대로 생성하는 등 유지를 한다.</p>
</li>
</ul>
<hr />
<h1 id="쿠버네티스의-클러스터">쿠버네티스의 클러스터</h1>
<h2 id="개념">개념</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/26722cb7-8837-42a4-aa44-b22bc84f0219/image.png" />
노드는 간단하게 컴퓨터라고 생각하면 된다.</p>
<ol>
<li>도커 or 도커 데스크탑이 설치된 채로 그걸 통해 이미지가 있어야 한다. (docker hub에 있는 상태여야 한다.)</li>
<li>manifest 파일로 상태 작성(yml)</li>
</ol>
<p>이 2가지가 선행된 상태여야 쿠버네티스가 효과있다.</p>
<p>kubectl 명령어로 해당 manifest 파일을 주면 master 노드가 worker 노드를 만들어서 관리해준다.</p>
<blockquote>
<p>하나의 worker node 당 manifest 파일 2개가 필요하다. (deployment, service) 에 대한 파일</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/142d4b46-348d-4f2f-a8ec-8daf9d100e28/image.png" />
위 이미지는 <strong>클러스터 아키텍처</strong>이며,</p>
<p><strong>클러스터(Cluster)</strong>란 컨테이너 형태의 애플리케이션을 호스팅하는 물리/가상 환경의 노드들로 이루어진 집합을 말한다.</p>
<p>클러스터는 클러스터 내부 요소들을 관리(제어)하는 <strong>마스터 노드</strong>와 컨테이너화 된 애플리케이션을 실행하는 <strong>워커노드</strong>로 구성되어 있다.</p>
<ul>
<li><strong>Master Node</strong><ul>
<li>컨테이너의 라이프사이클을 정의하고, 배포, 관리를 위한 쿠버네티스의 컴포넌트</li>
<li>etcd 라는 키-값 타입의 DB가 설치된다.</li>
<li>이 노드를 설정하는 관리자 컴퓨터에는 kubectl이 설치돼야 마스터 노드에 로그인하고 설정 관리가 가능하다.</li>
</ul>
</li>
</ul>
<p>| 종류 | 
| --- | --- |
| kube-apiserver | 사용자와 컨트롤 플레인과 통신하는 쿠버네티스 API |
| etcd | 모든 클러스터 데이터를 담아 정보 전반을 관리하는 키-값 타입의 데이터 베이스이다. |
| kube-scheduler | 파드를 워커 노드에 할당한다. |
| kube-controller-manager | 컨트롤러를 통합 관리하고 실행한다. |
| cloud-controller-manager | 클라우드 서비스와 연동해 서비스를 생성한다. |</p>
<ul>
<li><strong>워커 노드</strong> (컴퓨터 1대라는 의미로 보면 된다.)<ul>
<li>모든 클러스터는 최소 1개의 워커 노드를 가진다.</li>
<li>도커 엔진 같은 컨테이너 엔진이 필요하다.</li>
<li>워커노드는 구성요소인 파드를 호스트한다.</li>
</ul>
</li>
</ul>
<p>| 종류 | 
| --- | --- |
| kube-let | 클러스터의 각 노드에서 실행되는 에이전트로 마스터 노드의 kube-scheduler와 연동해 파드를 배치하고 확실하게 실행되도록 관리한다. <br />(쿠버네티스를 통해 생성되지 않은 컨테이너는 관리하지 않는다.) |
| kube-proxy | 각 노드에서 실행되는 네트워크 프록시로 쿠버네티스의 서비스 개념의 구현부이다. 네트워크 통신의 라우팅을 담당한다. <br /> (트래픽이 실제로 전달되어야 하는 파드 내의 포트를 지정, 서비스가 받은 트래픽을 파드의 어떤 포트로 전달할지 결정) |</p>
<hr />
<p>Master Node의 <code>kube-scheduler</code>, Worker Node의 <code>kube-let</code>은 배치를 하는 역할
Master Node의 <code>kube-apiserver</code>, Worker Node의 <code>kube-proxy</code>는 접근에 관련된 역할을 한다.</p>
<p>Worker Node의 <code>kube-proxy</code>는 <strong>load balancer</strong>의 역할과 비슷하다. </p>
<blockquote>
<p>kube-proxy를 활용하기 위해서는 ingress 를 만들고 prefix를 통해 게이트웨이처럼 사용이 가능하다.</p>
<p>하지만 이를 활용하기 위해서는 nginx 설치 및 몇 가지 추가로 해야 할 것이 있어서 추후에 다뤄보도록 한다.</p>
</blockquote>
<hr />
<h1 id="구성-요소">구성 요소</h1>
<h2 id="파드-pod">파드 (Pod)</h2>
<blockquote>
<p>클러스터에서 실행중인 컨테이너의 집합
즉, 컨테이너를 띄우는 곳 (컨테이너는 무조건 파드 안에 속해야 한다.)
파드 하나에는 컨테이너가 1개 이상 구성 가능하다.</p>
</blockquote>
<hr />
<h2 id="서비스-service">서비스 (Service)</h2>
<blockquote>
<p>네트워크 서비스로 파드 집합에서 실행중인 애플리케이션(컨테이너)들을 노출하는 방법</p>
</blockquote>
<ul>
<li>각 서비스는 고정된 IP(Cluster IP)를 부여 받아 외부에서는 이 IP 주소로 접근하며 이 때 서비스가 적절한 파드들로 분배한다.(로드밸런서)</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/8297a094-cc1d-444c-86f9-9351884b5f1c/image.png" /></p>
<p>위 이미지는 Service 관련 아키텍처이다.</p>
<p>위 이미지로 설명을 하면 현재 노드는 3개가 존재하지만 또 하나의 노드가 생기게 된다면 해당 노드로 연결을 하고, 분배하는 과정을 알아두는 것이 좋은데, 그것은 아래에 3가지 개념에 대해 적어두겠다.</p>
<blockquote>
<p><strong>롤링 업데이트, 카나리 배포, 블루그린 배포</strong> 3가지 개념을 공부하자.</p>
</blockquote>
<hr />
<h2 id="deployment-replicaset">Deployment, ReplicaSet</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/b6a77a8a-ab44-4d31-a9ac-be006f4a1b58/image.png" /></p>
<p>디플로이먼트(Deployment)는 파드와 레플리카셋(ReplicaSet)에 대한 선언적 업데이트를 제공한다.</p>
<p>선언적 업데이트를 제공한다는 의미는 파드와 레플리카셋에 대한 의도하는 상태를 설정하고 파드의 디플로이(배포)를 관리하는 요소임을 뜻한다.</p>
<p>ReplicaSet은 레플리카의 수(또는 파드의 수)를 관리한다. 또한 레플리카셋이 관리하는 동일한 구성의 파드를 레플리카(Replica)라고 한다.</p>
<h3 id="구성-요소-정리">구성 요소 정리</h3>
<table>
<thead>
<tr>
<th>리소스 이름</th>
<th>내용</th>
</tr>
</thead>
<tbody><tr>
<td>파드</td>
<td>컨테이너와 볼륨을 합한 것</td>
</tr>
<tr>
<td>서비스</td>
<td>파드에 요청을 배분</td>
</tr>
<tr>
<td>디플로이먼트</td>
<td>파드의 배포를 관리</td>
</tr>
<tr>
<td>레플리카세트</td>
<td>파드의 수를 관리</td>
</tr>
</tbody></table>
<h1 id="manifest">Manifest</h1>
<p>매니페스트(manifest)는 파드나 서비스에 대한 설정을 작성한 파일(정의 파일)이다. </p>
<p>yaml 또는 json 형식으로 작성할 수 있지만 기본적으로 사람이 설정 파일을 읽고 쓰기 용이하도록 yaml 파일을 사용한다.</p>
<blockquote>
<p>매니페스트 파일은 파드, 서비스, 디플로이먼트, 레플리카셋과 같은 리소스 단위로 작성한다. </p>
<p>디플로이먼트를 작성하면 레플리카셋과 파드도 함께 설정되게 된다.</p>
</blockquote>
<ul>
<li>기본적인 매니페스트 파일 항목<pre><code class="language-bash"># API 그룹 및 버전
apiVersion:
</code></pre>
</li>
</ul>
<h1 id="리소스-유형">리소스 유형</h1>
<p>kind:</p>
<h1 id="메타-데이터">메타 데이터</h1>
<p>metadata:</p>
<h1 id="리소스-내용">리소스 내용</h1>
<p>spec:</p>
<pre><code>
---
## 리소스 설정
- 자주 사용되는 리소스의 API그룹 및 리소스 유형
  - 최신 매니페스트 작성 유형
    - kubectl api-resources를 작성한다.
    ![](https://velog.velcdn.com/images/jojehuni_9759/post/f9cea099-4fa1-4e1e-b5b1-ee924b76e4e7/image.png)

| 리소스 | API 그룹/버전 | 리소스 유형 |
| --- | --- | --- |
| 파드 | core/v1(v1으로 축약 가능) | Pod |
| 서비스 | core/v1(v1으로 축약 가능) | Service |
| 디플로이먼트 | apps/v1 | Deployment |
| 레플리카세트 | apps/v1 | ReplicaSet |

---
## Deployment 생성
- **vue001dep.yml** 파일 생성

```yaml
apiVersion: apps/v1 # 사용할 Kubernetes API 버전을 명시한다. 'apps/v1'은 Deployment를 포함한 여러 리소스를 지원한다.
kind: Deployment # 생성하려는 리소스의 종류를 나타낸다. 여기서는 Deployment를 생성하려고 한다.
metadata: # 리소스에 대한 메타데이터를 정의한다. 이름, 네임스페이스, 레이블 등을 포함할 수 있다.
  name: vue001dep # Deployment의 이름을 설정한다. 이 이름으로 Deployment를 식별할 수 있다.

spec: # Deployment의 사양을 정의한다. 여기에는 레플리카 수, 셀렉터, 템플릿 등이 포함된다.

  # 디플로이먼트가 특정한 레이블이 부여된 파드를 관리할 수 있도록 하는 설정
  selector:
    matchLabels:
      app: vue001kube # 이 셀렉터는 'app=vue001kube' 레이블이 있는 파드만을 대상으로 한다.

  # 생성할 파드의 정보를 기재
  template: # 파드 템플릿을 정의한다. 이 템플릿은 파드 생성에 사용된다.

    # 파드 하나의 메타 데이터
    metadata:
      labels:
        app: vue001kube # 생성될 파드에 'app=vue001kube' 레이블을 부여한다.

    # 파드 하나의 스펙
    spec:
      containers:
      - name: vue-container # 컨테이너의 이름을 설정한다.
        image: jojehuni/k8s_vue:latest # 사용할 컨테이너 이미지를 지정한다. 여기서는 'jojehuni/k8s_vue:latest'를 사용한다.
        imagePullPolicy: Always # 이미지 풀 정책을 'Always'로 설정하여 항상 최신 이미지를 사용하도록 한다.
        ports:
        - containerPort: 5173 # 컨테이너가 리스닝할 포트 번호를 지정한다. 여기서는 5173번 포트를 사용한다.</code></pre><ul>
<li><strong>boot001dep.yml</strong> 파일 생성<pre><code class="language-yaml">apiVersion: apps/v1
kind: Deployment
metadata:
name: boot001dep
spec:
selector:
  matchLabels:
    app: boot001kube
replicas: 3
template:
  metadata:
    labels:
      app: boot001kube
  spec:
    containers:
    - name: boot-container
      image: jojehuni/k8s_boot:latest
      imagePullPolicy: Always
      ports:
      - containerPort: 7777</code></pre>
</li>
</ul>
<hr />
<h2 id="서비스-생성">서비스 생성</h2>
<ul>
<li><strong>vue001ser.yml</strong> 파일 생성<pre><code class="language-yaml"># 네트워크 서비스로 파드 집합에서 실행중인 애플리케이션을 노출하는 방법
apiVersion: v1 # 사용할 Kubernetes API 버전을 명시한다. 'v1'은 Service와 같은 기본 리소스를 지원한다.
kind: Service # 생성하려는 리소스의 종류를 나타낸다. 여기서는 Service를 생성하려고 한다.
metadata:
name: vue001ser # Service의 이름을 설정한다. 이 이름으로 Service를 식별할 수 있다.
</code></pre>
</li>
</ul>
<p>spec: # Service의 사양을 정의한다. 여기에는 서비스 유형, 포트 설정, 셀렉터 등이 포함된다.
  type: NodePort # 서비스 유형을 NodePort로 설정한다. 이는 클러스터 외부에서 내부 파드에 접근할 수 있도록 한다.
  ports:</p>
<pre><code># 서비스의 포트</code></pre><ul>
<li><p>port: 8000 </p>
<h1 id="컨테이너-포트파드의-포트">컨테이너 포트(파드의 포트)</h1>
<p>targetPort: 5173 
protocol: TCP </p>
<h1 id="워커-노드의-포트">워커 노드의 포트</h1>
<p>nodePort: 30000 # 클러스터의 모든 노드에서 이 포트를 통해 서비스에 접근할 수 있다.(30000 ~ 32767)
selector:
app: vue001kube # 이 셀렉터는 'app=vue001kube' 레이블이 있는 파드를 서비스의 대상으로 지정한다.</p>
<pre><code>이 YAML 파일은 vue001ser라는 이름의 Service를 생성한다.
</code></pre></li>
</ul>
<p>이 서비스는 NodePort 유형으로 설정되어 있으며, 이는 클러스터 외부의 사용자가 NodePort로 지정된 클러스터 노드의 특정 포트(여기서는 30000번)를 통해 서비스에 접근할 수 있음을 의미한다.</p>
<p>서비스는 port로 지정된 8000번 포트에서 외부 요청을 받아, selector에 의해 선택된 app=vue001kube 레이블을 가진 파드의 targetPort인 5173번 포트로 요청을 전달한다.</p>
<ul>
<li><strong>boot001ser.yml</strong> 파일 생성<pre><code class="language-yaml">apiVersion: v1
kind: Service
metadata:
name: boot001ser
spec:
type: NodePort
ports:
- port: 8001
  targetPort: 7777
  protocol: TCP
  nodePort: 30001
selector:
  app: boot001kube</code></pre>
</li>
</ul>
<p>worker node 안에는 kube-let 으로 배치하고 service로 안내를 하게 된다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d733991e-6096-44b5-9a2c-e9300498cf1e/image.png" /></p>
<p>마스터노드에서 위의 코드에 적힌 것처럼 30001 port로 가고, service는 8001 port이기에 
또 targetPort인 7777 port인 로드밸런싱에 의해 해당 pod를 찾아서 proxy를 거친 뒤 해당 서버에 접근을 하게 되는 것이다.</p>
<hr />
<h2 id="kubectl-명령어들">kubectl 명령어들</h2>
<pre><code># Deployment 적용
kubectl apply -f &lt;deployment yml 파일명(확장자 포함)&gt;
kubectl get deployments
kubectl get pods

# Service 적용
kubectl apply -f &lt;service yml 파일명(확장자 포함)&gt;
kubectl get services

# 기존 것을 지울려면
kubectl delete pods &lt;pod명&gt;
kubectl delete deployments &lt;deployment명&gt;
kubectl delete services &lt;service명&gt;

# 로그 확인하기
kubectl logs &lt;pod명&gt;

# 이미지 변경후 새로운 이미지 가져오기
kubectl rollout restart deployments &lt;deployment명&gt;</code></pre>