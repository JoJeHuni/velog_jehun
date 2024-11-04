<h1 id="클라우드-서버-및-aws">클라우드 서버 및 AWS</h1>
<h2 id="클라우드-서버">클라우드 서버</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/d554d751-0289-4a4f-91a3-dfd32ee3b0a7/image.png" /></p>
<p><strong>클라우드 서버</strong> : 인터넷 상의 가상화된 서버로 필요할 때마다 접근해서 사용할 수 있는 서버</p>
<p>클라우드(cloud)라는 용어 자체는 에릭 슈미트(전 구글 CEO)가 처음 정의했고 제프 베조스가 처음으로 클라우드 서비스를 만들게 되었다.</p>
<p>인터넷 통신망에만 접근할 수 있다면 컴퓨터 자원(CPU, 메모리, 디스크 등)들을 원하는 대로 가져다가 사용할 수 있어 ‘소유’에서 ‘대여 또는 사용’으로의 패러다임 변화를 가져왔다.</p>
<hr />
<h3 id="클라우드-서비스-모델">클라우드 서비스 모델</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/1558a68c-379e-4820-9eae-41047ca4a8ab/image.png" /></p>
<p><strong>하이퍼바이저</strong>라는 관리 소프트웨어가 물리적 서버에 설치되어 서버를 연결하고 가상화를 하면, 결합되어 있는 자원들을 추상화하여 가상의 서버를 만들어 낸다.</p>
<p>클라우드 특성에 따라 IaaS, PaaS, SaaS로 구분된다.</p>
<blockquote>
<p><strong>하이퍼바이저(Hypervisor)란?</strong>
물리적 하드웨어 위에서 여러 개의 가상 머신(VM)을 생성하고 실행할 수 있게 해 주는 소프트웨어 계층이다.</p>
<ol>
<li><strong>Type1(베어메탈 하이퍼바이저)</strong></li>
</ol>
</blockquote>
<ul>
<li>하드웨어 위에 직접 설치</li>
<li>예: VMware ESXi, Microsoft Hyper-V, Xen<blockquote>
<ol start="2">
<li><strong>Type2(호스트 하이퍼바이저)</strong></li>
</ol>
</blockquote>
</li>
<li>호스트 운영 체제 위에서 실행</li>
<li>예: VMware Workstation, Oracle VirtualBox</li>
</ul>
<ul>
<li><strong>클라우드 컴퓨팅 서비스 모델</strong><ul>
<li>IaaS : 인프라만 제공해주는 것<ul>
<li>사용자는 물리적인 자원을 직접 관리하지 않아도 되며, 제공자가 서비스하는 페이지에서 간단하게 조작이 가능하다.</li>
<li>사용한 만큼만 비용을 지불하면 된다.</li>
<li>AWS, 마이크로소프트 애저, 등이 있다.</li>
</ul>
</li>
<li>PaaS : 소프트웨어(애플리케이션, 데이터)를 제외한 나머지들을 제공해주는 것<ul>
<li>하드웨어, OS, 미들웨어와 같은 관리는 서비스 제공자가 하며, 사용자는 제공된 미들웨어만 사용 가능하다.</li>
<li>하드웨어나 운영체제에 신경쓰지 않고 애플리케이션을 실행할 수 있다.</li>
</ul>
</li>
<li>SaaS : 소프트웨어 포함해서 제공해주는 것<ul>
<li>네이버 클라우드, 웹 메일, 등과 같은 서비스를 사용할 수 있다.</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
<p>대략 자바에서는 런타임이 JRE, 미들웨어가 JVM 으로 생각하면 된다.</p>
</blockquote>
<hr />
<h3 id="클라우드-서버의-장단점">클라우드 서버의 장단점</h3>
<ul>
<li><strong>경제성</strong>
: 클라우드 서버를 사용하면 자체 인프라를 구매하여 관리할 때보다 기업이 부담하는 비용이 감소한다. 또한 유동적으로 서버의 갯수나 자원을 변경할 수 있으며 사용하는 리소스에 대해서만 비용을 지불하면 된다.</li>
<li><strong>편의성</strong>
: 서버 구축을 몇 분 안에 할 수 있으며, 서버 자원 관리 또한 클라우드 서버 회사에서 담당하기 때문에 관리 리소스 부담도 줄어든다.</li>
<li><strong>확장성</strong>
: 서버 저장소나 자원에 변화가 필요할 때 확장 또는 축소를 통해 수요에 대한 공급을 빠르게 대처할 수 있다.</li>
<li><strong>안정성</strong>
: 클라우드 서버는 물리 서버와 동일한 성능을 제공하며, 공유 환경에서 여러 서버가 돌아가고 있어서 한 서버에 문제가 발생해도 빠르게 복구 및 서비스를 지속할 수 있다.</li>
</ul>
<hr />
<h2 id="클라우드-서버-제공-회사">클라우드 서버 제공 회사</h2>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/039c1e4c-014a-4b5f-b563-c476f8cd1294/image.png" /></p>
<p>위 사진 속 회사들이 제공하고 있고, 간단하게 알아본 뒤 AWS 속 서비스들을 알아보자.</p>
<h3 id="아마존웹서비스-aws">아마존웹서비스 (AWS)</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/ad29058c-e4cb-412a-9462-c2d72b7000de/image.png" /></p>
<ul>
<li>2004년 11월 심플 큐 서비스(SQS)를 공개하면서 퍼블릭 클라우드 서비스를 시작했다.</li>
<li>2006년에는 클라우드 스토리지, S3, 컴퓨팅 자원 EC2, 큐 서비스 SQS를 통합적으로 제공했다.</li>
<li>Amazon Web Service는 네트워킹을 기반으로 가상 서버를 제공할 뿐 아니라 네트워크 인프라, 등 다양한 서비스를 제공한다.</li>
<li>전 세계에 175개가 넘는 데이터 센터를 보유하고 있으며, 전세계 점유율 1위 서비스이다.</li>
</ul>
<h3 id="마이크로소프트-애저microsoft-azure">마이크로소프트 애저(Microsoft Azure)</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/6743d748-ed51-4eb4-96cb-2d67b13ab34e/image.png" /></p>
<ul>
<li>2010년에 개시한 마이크로소프트의 클라우드 컴퓨팅 플랫폼이다.</li>
<li>전세계 클라우드 시장 2위로, 1위인 AWS를 추격하고 있다.</li>
<li>AWS에 비해 후발주자로 부족한 부분을 가격 경쟁력으로 따라잡으려고 하고 있다.</li>
<li>크레딧을 이용해 결제를 할 수 있으며, 직접 사거나 MSDN 구독, 비즈스파크 등을 이용해 얻을 수 있다.</li>
</ul>
<h3 id="구글-클라우드google-cloud-platform-gcp">구글 클라우드(Google Cloud Platform, GCP)</h3>
<p><img alt="" src="https://velog.velcdn.com/images/jojehuni_9759/post/796532cc-6da5-42f8-b8e6-460f2fbd25b6/image.png" /></p>
<ul>
<li>2011년 10월에 개시한 구글사의 클라우드 플랫폼이다.</li>
<li>2018년 기준으로 전 세계 클라우드 컴퓨팅 시장에서 3위를 차지하고 있다.</li>
<li>구글 계정이 있는경우 해당 계정을 이용하여 구글 클라우드를 사용할 수 있다는 장점이 있다.</li>
</ul>
<hr />
<h1 id="아마존-웹-서비스-aws">아마존 웹 서비스 (AWS)</h1>
<h2 id="aws-주요-서비스-소개">AWS 주요 서비스 소개</h2>
<h3 id="aws-ec2elastic-compute-cloud">AWS EC2(Elastic Compute Cloud)</h3>
<ul>
<li>AWS에서 가장 기본적이고 많이 쓰이는 인프라이며, 네트워크 망에서 가상 서버를 제공한다.</li>
<li>EC2는 한마디로 가상 머신이라고 볼 수 있으며 하나의 서버(가상머신)을 인스턴스라고 한다.</li>
<li>EC2는 AWS에서 제공하는 인터페이스를 통해 유동적인 스펙의 서버를 빠르게 구축할 수 있다.</li>
</ul>
<h3 id="aws-s3simple-storage-service">AWS S3(Simple Storage Service)</h3>
<ul>
<li>S3는 AWS에서 제공하는 오브젝트 스토리지 서비스이다.(사진, 비디오, 문서 등)</li>
<li>파일 저장은 bucket(또는 컨테이너)을 통해 운용되며 다른 유저들의 액세스도 컨트롤 할 수 있다.</li>
<li>무제한적인 확장성과 높은 가용성과 내구성을 보장하며 단일 파일을 최대 5TB까지 업로드할 수 있도록 지원한다.</li>
</ul>
<h3 id="vpcvirtual-private-cloud">VPC(Virtual Private Cloud)</h3>
<ul>
<li>아마존 VPC는 계정별로 격리된 네트워크 환경을 구성할 수 있는 서비스이다.</li>
<li>서브넷, Route Table, ACL, 보안 그룹 등을 이용해 논리적인 네트워크 분할 작업을 할 수 있다.</li>
<li>계정 안, 또는 계정 간 격리된 네트워크를 연결할 수 있는 옵션도 제공한다.</li>
<li>VPC는 AWS 리소스간 허용을 최소화하고 그룹별로 네트워크를 구성하기 쉽게 하기 위해 사용한다.</li>
</ul>
<h3 id="iamidentity-and-access-management">IAM(Identity and Access Management)</h3>
<ul>
<li>권한 관리를 지원해주는 서비스이다.</li>
<li>AWS에선 일반적으로 계정을 생성 시 Root계정이며, 이 계정은 모든 권한을 가지고 있으며 보안적으로 문제가 발생할 수 있기 때문에, IAM은 사용자 계정 및 그룹을 생성하여 권한을 제어하는 기능을 제공한다.</li>
</ul>
<h3 id="route-53">Route 53</h3>
<ul>
<li>Domain Name System, 도메인 관리/설정 서비스이다.</li>
<li>EC2 인스턴스, Elastic 로드 밸런서, S3 저장소 등 AWS 서비스 인프라에 연결하여 사용한다.</li>
<li>Route53을 통해 자신이 가지고 있는 도메인과 연결하거나, Route53에서 도메인을 구입할 수도 있다.</li>
</ul>
<h3 id="ecselastic-container-service">ECS(Elastic Container Service)</h3>
<ul>
<li>AWS의 매니지드 컨테이너 오케스트레이션 서비스이다.</li>
<li>EC2에 클래스터를 생성하고 EC2 인스턴스를 등록할 수 있다. ECS에서는 서비스와 Task로 도커 컨테이너를 운영하며, 클러스터에 등록된 EC2 인스턴스 위에 도커 컨테이너를 스케줄 한다.</li>
</ul>
<h3 id="ekselastic-kubernetes-service">EKS(Elastic Kubernetes Service)</h3>
<ul>
<li>관리형 쿠버네티스 서비스이다. 해당 서비스를 이용하면 쿠버네티스 마스터노드 구성을 하지 않아도 AWS에서 관리를 해주며, 빠르게 쿠버네티스를 사용할 수 있다.</li>
<li>해당 서비스를 구축하기 위해서는 EKS 클러스터 생성과 노드 그룹을 생성해야 한다.</li>
</ul>
<h3 id="sqssimple-queue-service">SQS(Simple Queue Service)</h3>
<ul>
<li>관리형 메시지 큐 서비스로 AWS의 첫 번째 서비스이다.</li>
<li>애플리케이션간 메시지를 전달하기 위한 Queue이다.</li>
<li>SQS는 메시지가 전달된 순서대로 정확히 처리하는 FIFO 대기열을 지원한다.</li>
</ul>
<h3 id="lambda">Lambda</h3>
<ul>
<li>서버리스 컴퓨팅 서비스이다. 해당 서비스는 애플리케이션을 실행하기 위한 별도 서버 없이 코드를 바로 실행해주는 서비스이다.</li>
<li>고정 비용 없이 사용 시간에 대해서만 비용이 발생한다.</li>
<li>EC2는 초 단위로 비용이 발생하는 반면 람다는 1ms당 요금을 계산하여 사용한 만큼 좀 더 정교한 비용이 발생된다.</li>
</ul>
<hr />
<p>우선은 이론에 관해서만 알아보고 적용하는 것을 배포하기 전에 직접 만들면서 차례대로 정리해서 작성할 예정이다. aws 계정으로 배포하는 것에 대한 내용은 아마 3주..? 뒤에 작성할 수 있을 것 같고, 그 사이에는 쿠폰 발송을 위해 SQS에 대한 글이나 프로젝트 중 알게된 점들에 대해 작성할 것 같다.</p>