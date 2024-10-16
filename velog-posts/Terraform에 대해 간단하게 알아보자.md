<h1 id="terraform">Terraform</h1>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/5bd131a1-8bd5-4cec-b5f4-05086cee9bcb/image.png" /></p>
<p>Terraform은 Infra Structure를 코드로 관리(Iac)하는 오픈 소스로 리소스와 서비스를 선언적 구성 파일로 정의하고 Provisioning한다.</p>
<blockquote>
<p>Provisioning : 사용자가 요청한 IT 자원을 사용할 수 있는 상태로 준비하는 것</p>
</blockquote>
<hr />
<h2 id="등장배경">등장배경</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d1ad0a5a-2f11-4f0b-b093-be794939fb10/image.png" /></p>
<p>2014년 HashiCorp의 Mitchell Hashimoto와 Armon Dadgar에 의해 처음 개발되었다.</p>
<p>클라우드 Infra Structure의 복잡성이 증가하면서 수동 관리의 한계와 일관성 유지의 어려움이 대두되었고, Terraform은 이러한 문제를 해결하기 위해 등장했다.</p>
<p>결국 인프라를 코드로 정의하고 버전 관리할 수 있는 도구를 통해 이러한 요구를 충족하는 툴로 빠르게 채택되어 현재는 다양한 클라우드 제공업체를 지원하는 유연성과 선언적 구문의 간결함이 주요 장점이 되었다.</p>
<hr />
<h2 id="장단점">장단점</h2>
<h3 id="장점">장점</h3>
<ul>
<li><strong>선언적 구성:</strong> 인프라를 코드로 정의하여 가독성과 유지보수성이 높다.</li>
<li><strong>멀티 클라우드 지원:</strong> 다양한 클라우드 제공업체와 서비스를 단일 도구로 관리할 수 있다.</li>
<li><strong>상태 관리:</strong> 인프라의 현재 상태를 추적하고 관리하여 일관성을 유지한다.</li>
</ul>
<h3 id="단점">단점</h3>
<ul>
<li><strong>학습 곡선:</strong> 테라폼 문법과 개념을 익히는 데 초기 시간 투자가 필요하다.</li>
<li><strong>제한적인 동적 구성:</strong> 일부 동적 구성이나 복잡한 로직 구현에 제한이 있을 수 있다.</li>
</ul>
<hr />
<h1 id="주요-개념-및-workflow">주요 개념 및 WorkFlow</h1>
<h2 id="주요-개념">주요 개념</h2>
<ul>
<li><strong>HCL(Hashicorp Configuration Language):</strong> 테라폼의 설정 언어이며 .tf를 사용한다.</li>
<li><strong>프로바이더(Provider):</strong> 테라폼과 클라우드 서비스를 연결하는 플러그인이다.</li>
</ul>
<table>
<thead>
<tr>
<th><strong>클라우드 서비스 모델</strong></th>
<th><strong>프로바이더</strong></th>
</tr>
</thead>
<tbody><tr>
<td>IaaS</td>
<td>AWS, Azure, GCP, Alibaba, Oracle Cloud</td>
</tr>
<tr>
<td>PaaS</td>
<td>Kubernetes, Docker AWS Elastic Beanstalk</td>
</tr>
<tr>
<td>SaaS</td>
<td>Github, GitLab, Atlassian(Jira, Confluence)</td>
</tr>
</tbody></table>
<blockquote>
<p>Iaas : Infrastructure as a Service
Paas : Platform as a Service
Saas : Software as a Service</p>
</blockquote>
<ul>
<li>Resource : Provider를 통해 생성/관리되는 인프라 요소
ex. 쿠버네티스 객체 (Manifest 파일과 유사하지만 HCL 문법으로 작성), EC2 Instance, 보안 그룹 등</li>
</ul>
<h2 id="workflow">WorkFlow</h2>
<blockquote>
<ol>
<li>Write (작성) : 인프라를 코드로 정의</li>
<li>Plan (계획) : 변경 사항 미리 확인</li>
<li>Apply (적용) : 인프라에 변경 사항 적용</li>
</ol>
<p>실제 코드로 작성하고 적용할 때 -&gt; tf config 파일 작성 이후 init -&gt; plan -&gt; apply</p>
</blockquote>
<ul>
<li><strong>초기화(Init)</strong><ul>
<li>명령어: transform init</li>
<li>역할: 프로젝트 디렉토리 초기화, 필요한 프로바이더 다운로드</li>
<li>중요성: 모든 테라폼 프로젝트의 첫 단계</li>
</ul>
</li>
<li><strong>계획(Plan)</strong><ul>
<li>명령어: transform plan</li>
<li>역할: 설정 파일 분석, 변경 예정 사항 미리 보기를 통해 해당 디렉토리 이하 모든 .tf 파일 적용 가능 확인</li>
<li>결과: 생성/수정/삭제 될 리소스 목록 제공</li>
</ul>
</li>
<li><strong>적용(Apply)</strong><ul>
<li>명령어: transform apply<ul>
<li>역할: 계획된 변경 사항을 실제 인프라에 적용(생성, 수정, 삭제)</li>
</ul>
</li>
<li>특징: 실행 전 최종 확인 단계 제공</li>
</ul>
</li>
</ul>
<hr />
<h1 id="활용해보기">활용해보기</h1>
<p>이전 게시글에 했던 것들을 활용해 이어서 해보자.</p>
<p>우선 해야 할 것이 환경변수 설정을 해야 한다.</p>
<p><a href="https://developer.hashicorp.com/terraform/install">https://developer.hashicorp.com/terraform/install</a></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e8bd346f-e3f1-4648-8264-0e5e731bf087/image.png" /></p>
<p>눌러서 압축을 푼 뒤, C드라이브에 'terraform' 이름으로 폴더를 만든 뒤 라이센스와 실행 파일을 옮기자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/23ac1211-c6c9-44f5-817f-fbe1dbfcb6a4/image.png" /></p>
<p>확인을 눌러준 뒤 cmd 창에서 <code>terraform -version</code></p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/a0d01ab1-436e-4ee9-a8c2-65384a8f685b/image.png" /></p>
<p>잘 된 것을 볼 수 있다.</p>
<hr />
<p>쿠버네티스했던 폴더로 가서 <code>terra_prac</code> 폴더 만들기</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/2ac82c10-5a3c-4984-8741-1a9a19055319/image.png" /></p>
<p>VSCode로 해당 폴더를 연 뒤 <code>main.tf</code> 파일 만들고 아래 코드 적용</p>
<pre><code class="language-tf"># Kubernetes 프로바이더 설정
provider &quot;kubernetes&quot; {
  config_path = &quot;~/.kube/config&quot;  # 로컬 Kubernetes 설정 파일 경로
}

# Spring Boot Deployment
resource &quot;kubernetes_deployment&quot; &quot;boot002dep&quot; {
  metadata {
    name = &quot;boot002dep&quot;
  }

  spec {
    replicas = 1

    selector {
      match_labels = {
        app = &quot;boot002kube&quot;
      }
    }

    template {
      metadata {
        labels = {
          app = &quot;boot002kube&quot;
        }
      }

      spec {
        container {
          image = &quot;jojehuni/nine_b_proj:latest&quot;
          name  = &quot;boot-container&quot;
          image_pull_policy = &quot;Always&quot;

          port {
            container_port = 7777
          }
        }
      }
    }
  }
}

# Spring Boot Service
resource &quot;kubernetes_service&quot; &quot;boot002ser&quot; {
  metadata {
    name = &quot;boot002ser&quot;
  }

  spec {
    selector = {
      app = &quot;boot002kube&quot;
    }

    port {
      port        = 8001
      target_port = 7777
    }

    type = &quot;ClusterIP&quot;
  }
}

# Vue.js Deployment
resource &quot;kubernetes_deployment&quot; &quot;vue002dep&quot; {
  metadata {
    name = &quot;vue002dep&quot;
  }

  spec {
    replicas = 1

    selector {
      match_labels = {
        app = &quot;vue002kube&quot;
      }
    }

    template {
      metadata {
        labels = {
          app = &quot;vue002kube&quot;
        }
      }

      spec {
        container {
          image = &quot;jojehuni/nine_v_proj:latest&quot;
          name  = &quot;vue-container&quot;
          image_pull_policy = &quot;Always&quot;

          port {
            container_port = 80
          }
        }
      }
    }
  }
}

# Vue.js Service
resource &quot;kubernetes_service&quot; &quot;vue002ser&quot; {
  metadata {
    name = &quot;vue002ser&quot;
  }

  spec {
    selector = {
      app = &quot;vue002kube&quot;
    }

    port {
      port        = 8000
      target_port = 80
    }

    type = &quot;ClusterIP&quot;
  }
}

# Ingress
resource &quot;kubernetes_ingress_v1&quot; &quot;sw_camp_ingress&quot; {
  metadata {
    name = &quot;sw-camp-ingress&quot;
    annotations = {
      &quot;nginx.ingress.kubernetes.io/ssl-redirect&quot; = &quot;false&quot;
      &quot;nginx.ingress.kubernetes.io/rewrite-target&quot; = &quot;/$2&quot;
    }
  }

  spec {
    ingress_class_name = &quot;nginx&quot;

    rule {
      http {
        path {
          path = &quot;/()(.*)$&quot;
          path_type = &quot;ImplementationSpecific&quot;
          backend {
            service {
              name = &quot;vue002ser&quot;
              port {
                number = 8000
              }
            }
          }
        }
        path {
          path = &quot;/boot(/|$)(.*)$&quot;
          path_type = &quot;ImplementationSpecific&quot;
          backend {
            service {
              name = &quot;boot002ser&quot;
              port {
                number = 8001
              }
            }
          }
        }
      }
    }
  }
}</code></pre>
<hr />
<p>이제 git bash 터미널 창을 열고 아래 사진과 같이 명령어를 입력해 켜져 있는 deployment와 service, ingress 들을 삭제하자.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1c1cd92f-9a7d-42a6-b009-97b4cea431c9/image.png" /></p>
<blockquote>
<p>Terraform으로 한 번에 띄우는 것을 확인하기 위해 삭제한 것이다.</p>
</blockquote>
<hr />
<p>terra_prac 폴더 위치에서 <code>terraform init</code> 명령어 실행</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/50d632e5-8909-439a-81a5-7f54ae99a60d/image.png" /></p>
<blockquote>
<p>주의할 점
계속 terrform command not found 라는 오류가 떠서 왜 그러나 찾아봐도 방법이 나오지 않았다..</p>
<p>그럴 땐 VSCode 열린걸 다 끄고 다시 키면 된다.. main.tf도 꼭 저장을 하자. (이건 이미 돼 있었는데.. 안 꺼서 안 됐던 것)</p>
</blockquote>
<p><code>terraform plan</code> 명령어로 뷰를 확인할 수 있다.</p>
<p><code>terraform apply</code> 를 하면 끝난다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/f955592a-c7e0-49c2-b62f-27ee69bd72c6/image.png" /></p>
<p>tf 파일 남기고 나머지를 지우는 방법은</p>
<ul>
<li><code>rm -rf .terraform*</code></li>
<li><code>rm terraform.tfstate*</code> 2개 명령어를 순차적으로 사용해 지울 수 있다.</li>
</ul>