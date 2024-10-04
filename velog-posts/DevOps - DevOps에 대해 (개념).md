<p>몇 주 간의 기획, 모델링, ddd 설계, 기능 설계, 기능 구현, Backend, Frontend 구현까지 합친 프로젝트를 끝내고 DevOps를 공부하기 시작해 블로그 글을 이어 작성하고자 한다.</p>
<h1 id="devops">DevOps</h1>
<blockquote>
<p>단절된 개발과 운영 간의 프로세스를 원활하게 연결하고 자동화 방법을 통해 효율성을 극대화하는 방법</p>
</blockquote>
<p>즉, 서로 다른 업무의 통합이다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/9e741659-c92a-4930-a6d4-f60d9843b43c/image.png" /></p>
<p><strong>DevOps</strong>는 소프트웨어 개발(Dev)과 IT 운영(Ops)의 경계를 허물고, 지속적인 통합(CI), 지속적인 배포(CD), 자동화 등을 통해 더 빠른 소프트웨어 개발과 더 높은 운영 효율을 달성하려는 문화, 움직임, 관행의 집합이다.</p>
<hr />
<h2 id="과거와-현재">과거와 현재</h2>
<p>과거에는 새로운 서비스 출시를 위해 오랜 시간 개발하고 배포하는데 많은 시간을 소요해야 했다.</p>
<ul>
<li>코드개발, 테스트 등</li>
</ul>
<p>모든 팀은 각자의 '완료'의 의미가 다 다르기 때문에 제품 전체의 완료를 책임지지 않았다.</p>
<ul>
<li><strong>개발팀</strong>에게 ‘완료’의 의미는 <strong>요구사항을 구현</strong>하는 것</li>
<li><strong>QA팀</strong>에게 ‘완료’의 의미는 <strong>코드를 테스트</strong>하는 것</li>
<li><strong>운영팀</strong>에게 ‘완료’의 의미는 <strong>코드를 릴리스</strong>하는 것</li>
</ul>
<p>현재는 서비스 출시 속도도 빠르며 업데이트 주기도 빈번한데</p>
<p>현재의 트렌드에 맞게 개발된 소프트웨어가 <strong>안정성을 유지</strong>하면서도 사용자에게 신속하게 제공될 수 있도록 개발, 테스트, 배포, 운영의 <strong>업무 사이클을 자동화된 하나의 프로세스로 통합</strong>할 필요성이 생겼다.</p>
<hr />
<h2 id="devops의-핵심-요소-cams">DevOps의 핵심 요소 (CAMS)</h2>
<p>CAMS는 DevOps의 핵심 원칙을 나타내는 약어이다.</p>
<ol>
<li><p><strong>Culture(문화)</strong>
DevOps 문화는 협업과 의사소통을 중시한다.
개발(Dev) 팀과 운영(Ops) 팀 간의 장벽을 허물고, 모든 이해관계자가 목표 달성을 위해 긴밀하게 협력하도록 장려하는 문화 <strong>즉, silo화 되는 것을 방지</strong>
(본인만의 이익만을 우선시하는 것을 방지)</p>
</li>
<li><p><strong>Automation(자동화)</strong>
DevOps에서는 반복 가능하고 예측 가능한 작업을 자동화함으로써, 수동 작업의 오류를 줄이고 효율성을 높인다. (작업자의 실수를 예방하고 서비스의 일관성을 유지)</p>
</li>
<li><p><strong>Measurement(측정)</strong>
성능, 프로세스, 효과성을 정량적으로 측정하고 분석하는 것을 강조한다.
DevOps에서는 지속적인 측정과 피드백이 중요하며, 시스템의 성능, 응답 시간, 버그 발생 빈도 등을 모니터링하고, 이를 기반으로 지속적인 개선을 도모한다.
(측정할 수 없으면 관리 X, 관리 할 수 없으면 개선 X)</p>
</li>
<li><p><strong>Sharing(공유)</strong>
지식, 아이디어, 성공 사례뿐만 아니라 실패 경험까지 공유하는 것을 중요시한다.
공유 문화는 DevOps의 핵심 요소 중 하나로, 팀 간의 벽을 허물고 조직 전체가 하나의 목표를 향해 나아가도록 한다.</p>
</li>
</ol>
<hr />
<h2 id="cloud-native">Cloud Native</h2>
<p><a href="https://landscape.cncf.io/">CNCF LandScape</a></p>
<blockquote>
<p>Cloud Native는 애플리케이션을 개발하고 운영하기 위한 접근 방식으로, 클라우드 컴퓨팅의 이점을 최대화하도록 설계된 기술과 아키텍처에 중점을 둔다.</p>
</blockquote>
<p>Cloud Native와 DevOps는 서로 상호 보완적인 관계에 있다. </p>
<p>Cloud Native는 DevOps의 원칙과 관행을 클라우드 환경에서 효과적으로 구현할 수 있는 기술적 수단을 제공한다.</p>
<p>DevOps는 Cloud Native 기술을 활용해 개발과 운영의 경계를 없애고, 더 빠른 혁신과 효율적인 운영을 가능케 한다.</p>
<h3 id="역사">역사</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/e86356b9-9a2d-44ba-92ee-5be0bc121f59/image.png" /></p>
<p>1980년대 Waterfall 모델, 애플리케이션은 일체형(모놀리식), 물리적 서버에 배포되고, 데이터센터 환경에서 운영됐다.</p>
<p>1990년대 aisle 개발 방법론, 아키텍처는 N-계층으로 발전해 모듈화된 구조, 가상서버 기술이 등장해 배포와 패키징에 변화를 가져왔다.</p>
<p>2000년대부터 DevOps 운동이 시작됐다. 이는 개발과 운영의 경계를 허물고, CI/CD의 개념을 통해 지속적인 통합과 배포를 중요시했다.</p>
<hr />
<h2 id="msa">MSA</h2>
<p>DevOps를 가능케 해주는게 Cloud Native 기술이며, Cloud Native에 속하는 한 구획이 MSA이다.</p>
<p>MSA의 개념과 장, 단점에 대해서는 아래 링크로 간단하게 볼 겸 링크를 달아본다.</p>
<p><a href="https://velog.io/@jojehuni_9759/MSA%EB%9E%80">MSA란?</a></p>
<hr />
<h2 id="cicd">CI/CD</h2>
<blockquote>
<p>CI (Continuous Integration, 지속적 통합)와 CD (Continuous Delivery 또는 Continuous Deployment, 지속적 제공 또는 지속적 배포)</p>
</blockquote>
<p>CI, CD는 개발의 효율성을 높이고, 소프트웨어의 품질을 개선하며, 배포 과정을 자동화하여 더 빠른 속도로 시장에 출시할 수 있게 돕는다.</p>
<ul>
<li><p><strong>CI (Continuous Integration)</strong>: 개발자들이 작업한 코드를 주기적으로 (보통은 하루에 여러 번) 메인 저장소에 통합(머지)하는 것을 의미한다. 이 과정에서 자동화된 빌드와 테스트가 수행되어, 코드 변경 사항이 시스템에 통합될 때 발생할 수 있는 문제를 조기에 발견하고 수정한다.</p>
</li>
<li><p><strong>CD (Continuous Delivery)</strong>: CI 과정을 통해 테스트된 코드를 자동으로 빌드하고 테스트 환경이나 스테이징 환경에 배포하는 것을 의미한다. 목표는 소프트웨어가 항상 출시 준비 상태를 유지하도록 하는 것이다.</p>
</li>
<li><p><strong>CD (Continuous Deployment)</strong>: Continuous Delivery의 한 단계 더 나아가, 자동화된 테스트를 통과한 코드를 자동으로 프로덕션 환경에 배포하는 것이다. 이 과정은 수동 개입 없이 이루어진다.</p>
</li>
</ul>
<h3 id="장단점">장단점</h3>
<ol>
<li><p><strong>장점</strong></p>
<ul>
<li><strong>개발 속도와 효율성 향상</strong>: 자동화를 통해 개발 과정이 더 빠르고 효율적이게 되며, 개발자는 코드 작성에 더 집중할 수 있다.</li>
<li><strong>품질 개선</strong>: 자동화된 테스트와 지속적인 통합을 통해 소프트웨어의 품질이 개선된다.</li>
<li><strong>배포 속도 증가</strong>: 자동화된 배포 과정을 통해 새로운 기능과 수정 사항을 빠르게 시장에 출시할 수 있다.</li>
</ul>
</li>
<li><p><strong>단점</strong></p>
<ul>
<li><strong>시간과 비용</strong>: CI/CD 파이프라인을 설계하고 구현하는 데 초기에 상당한 시간과 비용이 소요된다. 적절한 도구 선택, 시스템 구성, 그리고 팀 교육에 대한 투자가 필요하다.</li>
<li><strong>학습 곡선</strong>: 개발팀이 CI/CD 도구와 방법론에 익숙해지기까지는 시간이 걸리며, 초기에는 생산성이 저하될 수 있다.</li>
<li><strong>파이프라인 유지 관리</strong>: 다수의 도구와 프로세스를 관리해야 하며, 파이프라인의 복잡성은 시간이 지남에 따라 증가할 수 있다. 이로 인해 유지 관리가 어려워질 수 있다.</li>
</ul>
</li>
</ol>
<hr />
<p>아주 깊게 CI/CD에 대해 알아본다기 보다는 CNCF LandScape 속 현재 가장 잘 알려진 것 중</p>
<ul>
<li>Docker</li>
<li>kubernetes</li>
<li>Jenkins</li>
<li>Terraform</li>
</ul>
<p>위 4가지에 대해 공부해서 블로그 글을 쭉 작성할 예정이다.</p>