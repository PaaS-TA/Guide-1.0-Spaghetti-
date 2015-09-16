## Table of Contents
1. [문서 개요](#문서-개요)
     * [1.1. 목적](#목적)
     * [1.2. 범위](#범위)
     * [1.3. 시스템 구성도](#시스템-구성도)
     * [1.4. 참고자료](#참고자료)
2. [Redis 서비스팩 설치](#redis-서비스팩-설치)
     * [2.1. 설치전 준비사항](#설치전-준비사항)
     * [2.2. Redis 서비스 릴리즈 업로드](#redis-서비스-릴리즈-업로드)
     * [2.3. Redis 서비스 Deployment 파일 수정 및 배포](#redis-서비스-deployment-파일-수정-및-배포)
     * [2.4. Redis 서비스 브로커 등록](#redis-서비스-브로커-등록)
3. [Redis 연동 Sample App 설명](#redis-연동-sample-app-설명)
     * [3.1. Sample App 구조](#sample-app-구조)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#개방형-클라우드-플랫폼에서-서비스-신청)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#sample-app에-서비스-바인드-신청-및-app-확인)
4. [Redis Client 툴 접속](#redis-client-툴-접속)
     * [4.1. Redis Desktop Manager 설치 및 연결](#redis-desktop-manager-설치-및-연결)


# 문서 개요

### 목적
      
본 문서(Redis서비스팩설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인Redis서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서Redis 서비스를 사용하는 방법을 기술하였다.


### 범위 

설치 범위는 Redis서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

### 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. Redisdedicated-node(5대), Redis 서비스 브로커로 최소사항을 구성하였다.
![시스템 구성도][redis_openstack_02]

<table>
  <tr>
    <td>구분</td>
    <td>스펙</td>
  </tr>
  <tr>
    <td>openpaas-redis-broker</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node1</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node2</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node3</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node4</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node5</td>
    <td>2vCPU / 2GB RAM / 20GB Disk+8GB(영구적 Disk)</td>
  </tr>
</table>

### 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

#   Redis 서비스팩 설치

### 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)


###   Redis 서비스 릴리즈 업로드

- OpenPaaS-Services.zip 파일 압축을 풀고폴더안에 있는Redis 서비스 릴리즈openpaas-redis-release-beta-1.0.tgz 파일을복사한다.
- 업로드할openpaas-redis-release-beta-1.0.tgz 파일을 확인한다.



><div>$ls –all</div>  
![redis_openstack_03]


- 업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$bosh releases</div>
![redis_openstack_04]
><div>Redis 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</div>
>

- Redis 서비스 릴리즈 파일을업로드한다.

><div>$ bosh upload release {서비스릴리즈 파일 PATH}</div><br>
><div>$ bosh upload release openpaas-redis-release-beta-1.0.tgz</div><br>
><br>
>![redis_openstack_05]
>![redis_openstack_06]
>![redis_openstack_07]
>![redis_openstack_08]

- 업로드 된 Redis릴리즈를 확인한다.

><div>$bosh releases</div><br>
>![redis_openstack_09]
><div>Redis 서비스 릴리즈가 업로드 되어 있는 것을 확인</div>

###   Redis 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params등을 정의가 되어 있다.


- OpenPaaS-Deployment.zip 파일 압축을 풀고폴더안에 있는OpenStack용 Redis Deployment 화일인openpaas-redis-openstack.yml를 복사한다.
다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-redis-openstack.yml)

><div>$ls –all</div>
>![redis_openstack_10]

- Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.

><div>$bosh status</div>
>![redis_openstack_11]

- Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3016 버전 사용)
 
><div>$bosh stemcells</div>
>![redis_openstack_12]
><div>Stemcell목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여Stemcell 3016 버전을 업로드를 해야 한다.</div>

- openpaas-redis-openstack.yml Deployment 파일을 서버 환경에 맞게 수정한다. (빨간색으로 표시된 부분 특히 주의)

<pre>$vi openpaas-redis-openstack.yml

# openpaas-redis-openstack 설정 파일 내용
name: openpaas-redis-service                       # 서비스 배포이름(필수)
director_uuid: xxxxx#bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- name: openpaas-redis                          #서비스 릴리즈 이름(필수)
  version: beta-1.0          #서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                     # canary 인스턴스 수(필수)
canary_watch_time: 30000-600000  # canary 인스턴스가 수행하기 위한 대기 시간(필수)
max_in_flight: 1   # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
update_watch_time: 30000-600000       # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

compilation:                           #컴파일시 필요한 가상머신의 속성(필수)
cloud_properties: # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)
instance_type: m1.medium          # 인스턴스 타입: Flavors 타입 (필수)
  network: openpaas_network    # Networks block에서 선언한 network 이름(필수)
reuse_compilation_vms: true     # 컴파일지 VM 재사용 여부(옵션)
  workers: 6              # 컴파일 하는 가상머신의 최대수(필수)

jobs:
- instances: 1
  name: openpaas-redis-broker           # 작업 이름(필수)
  networks:# 네트워크 구성정보
  - name: openpaas_network         # Networks block에서 선언한 network 이름(필수)
static_ips:
    - 10.10.7.71# 사용할 IP addresses 정의(필수)
persistent_disk: 8192               # 영구적 디스크 사이즈 정의(옵션): 8G
  properties:
    broker:# 브로커에 대한 설정(필수): 사용자, 패스워드
      password: admin
      username: admin
   # host: redis-broker.controlleropen-paas.com
    host: 10.10.7.71
syslog_aggregator: null
resource_pool: services-small          # Resource Pools block에 정의한 resource pool 이름(필수)
  templates:
  - name: cf-redis-broker              # job template 이름(필수)
    release: openpaas-redis            #서비스 릴리즈 이름(필수)
- instances: 5                         # 생성하는 인스턴스 수(필수)
  name: dedicated-node               # 작업 이름(필수)
  networks:
  - name: openpaas_network           # Networks block에서 선언한 network 이름(필수)
static_ips:                         # 전용 노드 IP 목록(필수)
    - 10.10.7.72
    - 10.10.7.73
    - 10.10.7.74
    - 10.10.7.75
    - 10.10.7.76
persistent_disk: 8192                 # 영구적 디스크 사이즈 정의(옵션): 8G
resource_pool: services-small
  templates:
  - name: dedicated-node             # job template 이름(필수)
    release: openpaas-redis
syslog_aggregator: null
- instances: 1
  lifecycle: errand                   # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar              # 작업 이름: 서비스 브로커 등록
networks:
  - name: openpaas_network
  properties:
    broker:# 서비스 브로커 설정 정보
      host: 10.10.7.71# 서비스 브로커 IP
      name: openpaas-redis-service
      password: admin
      username: admin
      protocol: http
      port: 80
cf:# CF 설정 정보
admin_password: admin
admin_username: admin
api_url: http://api.controller.open-paas.com
redis:
      broker:
enable_serive_access: true            # 서비스 접속 허용 여부(필수)
service_name: redis-sb               # 서비스 이름
resource_pool: services-small
  templates:
  - name: broker-registrar               # job template 이름(필수)
    release: openpaas-redis
- instances: 1
  lifecycle: errand
  name: broker-deregistrar                    # 작업이름: 서비스 브로커 삭제
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: 10.10.7.71
      name: openpaas-redis-service
      password: admin
      username: admin
resource_pool: services-small
  templates:
  - name: broker-deregistrar
    release: openpaas-redis
- instances: 1
  lifecycle: errand
  name: smoke-tests                   # 작업이름: 서비스팩이 정상적으로 설치 되었는 테스트
  networks:
  - name: openpaas_network
resource_pool: services-small
  templates:
  - name: smoke-tests
    release: openpaas-redis
meta:
apps_domain: controller.open-paas.com# CF 설치시 설정한 apps 도메인 정보
  environment: null
external_domain: controller.open-paas.com# CF 설치시 설정한 외부 도메인 정보
nats:                            # CF 설치시 설치한 nats 정보 (필수)
    host: 10.10.3.11# nats 서버 IP
    password: admin# nats 유저 비밀번호
    port: 4222# nats 서버 포트번호
    username: nats# nats 서버 유저아이디
syslog_aggregator: null
networks:
- name: openpaas_network
  subnets:
  - cloud_properties:
net_id: bfef7fe1-bf86-4e29-842e-84a4d1cff110# OpenStack에서 사용하는 network 이름 아이디(필수)
security_groups:
      - cf-security# OpenStack 에서 사용하는 접근 시큐리티 이름 이름(필수)
dns:                       # DNS 정보
    - 10.10.5.108
    - 8.8.8.8
    gateway: 10.10.7.1
    range: 10.10.7.0/24
    reserved:             # 설치시 제외할 IP 설정 (OpenStack 에서는 서비스팩 설치 구간 이외에는 IP 제외 설정을 해줘야 오류가 나지 않음.
    - 10.10.7.2 - 10.10.7.70
    static:사용 가능한 IP 설정
    - 10.10.7.71 - 10.10.7.80
  type: manual
properties:
cf:
admin_password: admin               # CF 어드민 아이디 비밀번호(필수)
admin_username: admin              # CF 어드민 아이디 (필수)
api_url: http://api.controller.open-paas.com# CF API url(필수)
apps_domain: controller.open-paas.com# CF 도메인(필수)
nats:                  # CF 설치시 설치한 nats 정보 (필수)
      host: 10.10.3.11
      password: admin
      port: 4222
      username: nats
redis:
    agent:
backend_port: 54321          # Redis backend 포트번호(필수)
bg_save_command: yetanotherrandomstring
    broker:
auth:
        password: admin
        username: admin
backend_host: 10.10.7.71
backend_port: 12345
dedicated_nodes:
      - 10.10.7.72
      - 10.10.7.73
      - 10.10.7.74
      - 10.10.7.75
      - 10.10.7.76
enable_service_access: true
      name: openpaas-redis-service
      network: openpaas_network
service_instance_limit: 5                     # 서비스 인스턴스 생성 가능 수(필수)
service_name: redis-sb
config_command: configalias       # config 설정 명령어가 있을 경우(옵션)
    host: 10.10.7.71
maxmemory: 262144000             # 최대 메모리
save_command: anotherrandomstring   # 저장 명령어가 있을 경우(옵션)
syslog_aggregator: null
resource_pools:         # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:         # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone)
instance_type: m1.medium           #인스턴스 타입: Flovers 타입(필수)
  name: services-small              # 고유한 resource pool 이름
env:                          # 환경 정보(옵션)
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: openpaas_network
 # size: 7# resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent#stemcell 이름(필수)
    version: 3016# stemcell 버전(필수)
</pre>

- Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

><div>$bosh deployment {Deployment manifest 파일 PATH}</div>
><div>$bosh deployment openpaas-redis-openstack.yml</div>
>![redis_openstack_13]

- Redis서비스팩을 배포한다.

><div>$bosh deploy</div>
>![redis_openstack_14]
>![redis_openstack_15]

- 배포된 Redis서비스팩을 확인한다.

><div>$bosh vms</div>
>![redis_openstack_16]
>![redis_openstack_17]

### Redis 서비스 브로커 등록
Redis서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Redis 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시개방형 클라우드 플랫폼에서서비스브로커를등록할수있는사용자로로그인이되어있어야한다.

- 서비스 브로커 목록을 확인한다.

><div>$cf service-brokers</div>
>![redis_openstack_18]

- Redis 서비스 브로커를 등록한다.

><div>cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
- 서비스팩이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
- 서비스팩 사용자ID / 비밀번호 :서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
- 서비스팩URL :서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.</div><br>
><div>$ cf create-service-broker redis-service-broker admin adminhttp://10.10.7.71:80</div>
>![redis_openstack_19]

- 등록된 Redis 서비스 브로커를 확인한다.

><div>$cf service-brokers</div>
>![redis_openstack_20]

- 접근 가능한 서비스 목록을 확인한다.

><div>$cf service-access</div>
>![redis_openstack_21]
><div>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.</div>

- 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

><div>$cf enable-service-access redis-sb</div>
><div>$ cf service-access</div>
>![redis_openstack_22]

#   Redis연동 Sample App 설명
본 Sample App은 개발형클라우드 플랫폼에 배포되며 Redis의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
### Sample App 구조
Sample App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. 배포 완료 후 정상적으로 App 이 구동되면 curl 명령어로 배포된 도메인 정보를 이용하여 Redis에 특정 key 에 값을 저장/조회/삭제를 한다.

Sample App 구조는 다음과 같다.
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>lib</td>
    <td>Sample 소스디렉토리</td>
  </tr>
  <tr>
    <td>manifest.yml</td>
    <td>개방형클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일</td>
  </tr>
  <tr>
    <td>Gemfile</td>
    <td>Sample App 구동시 필요한 ruby gem 설정 파일</td>
  </tr>
  <tr>
    <td>config.ru</td>
    <td>Sample App 구동 파일</td>
  </tr>
</table>

- OpenPaaS-Sample-Apps.zip 파일 압축을 풀고 Service폴더안에 있는 RedisSample Web App인 redis-example-app을복사한다

><div>$ls -all</div>
>![redis_openstack_23]

### 개방형 클라우드 플랫폼에서 서비스 신청
Sample App에서 Redis 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$cf marketplace</div>
>![redis_openstack_24]

- Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
- 서비스명 :redis-sb로 Marketplace에서 보여지는 서비스 명칭이다.<br>
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Redis 서비스는 shared-vm 과 dedicated-vmplan을지원한다.<br>
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.</div><br>

><div>$ cf create-service redis-sbshared-vmredis-service-instance</div>
>![redis_openstack_25]

- 생성된 Redis 서비스 인스턴스를 확인한다.

><div>$cf services</div>
>![redis_openstack_26]

### Sample App에 서비스 바인드신청 및 App 확인
서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Redis 서비스를 이용한다.
*참고: 서비스 Bind 신청시개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample App 디렉토리로 이동하여 manifest 파일을 확인한다

><div>$cd redis-example-app<br>
$ vi manifest.yml<br><br>
---<br>
applications:<br>
- name: redis-example-app#배포할 App 이름<br>
  memory: 256M#배포시 메모리 크기<br>
  instances: 1#배포시인스턴스 수<br>
path: .#배포시 구동되는 Path<br>
 </div>

- --no-start 옵션으로 App을 배포한다. 
--no-start: App 배포시 구동은 하지 않는다.

><div>$cf push --no-start<br></div>
>![redis_openstack_27]

- 배포된 Sample App을 확인하고 로그를 수행한다.

><div>$cf apps<br></div>
>![redis_openstack_28]
><div>$ cf logs {배포된 App명}
>$ cf logs redis-example-app</div>
>![redis_openstack_29]

- Sample App에서 생성한 서비스인스턴스바인드 신청을 한다. 

><div>$cf bind-service redis-example-appredis-service-instance</div>
>![redis_openstack_30]

- 바인드가 적용되기 위해서 App을 재기동한다.

><div>$cf restart redis-example-app</div>
>![redis_openstack_31]

- App이 정상적으로 Redis 서비스를 사용하는지 확인한다.

><div>- curl 로 확인<br>
- Redis Application 을 이용하여 Key 값으로 데이터 저장<br>
$ export APP=redis-example-app.controller.open-paas.com<br>
$ curl -X PUT $APP/foo -d 'data=bar'<br>
><div>
>![redis_openstack_32]
><div>- Key 값을 이용하여 Redis 에 저장되어 있는 Value 값 Get<br>
$ curl -X GET $APP/foo<br>
></div>
>![redis_openstack_33]
><div>- Key 값을 이용하여 Redis 에 저장되어 있는 Value 삭제<br>
$ curl -X DELETE $APP/foo<br>
></div>
>![redis_openstack_34]

# Redis Client 툴 접속
Application에 바인딩된Redis 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Redis Client 툴에서 직접 연결할수 없다. 따라서 Redis Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Redis Client 툴로써는 오픈 소스인 Redis Desktop Manager로 가이드한다. Redis Desktop Manager 에서 접속하기 위해서 먼저 SSH 터널링할수 있는 VM 인스턴스를생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 OpenStack 관리자 및 OpenPaaS 운영자에게 문의하여 구성한다. OpenStack 에서 구성한 인스턴스는공개키(.pem) 로 접속을 해야 하므로 공개키는 운영 담당자에게 문의하여 제공받는다. 참고) 개인키(.ppk)로는 접속이 되지 않는다.

### Redis Desktop Manager 설치 및 연결
Redis Desktop Manager 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

- Redis Desktop Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.
[**http://redisdesktop.com/download**](http://redisdesktop.com/download)<br>
![redis_openstack_35]

- 다운로드한 설치파일을 실행한다.<br>
![redis_openstack_36]

- Redis Desktop Manager 설치를 위한 안내사항이다. Next 버튼을 클릭한다.<br>
![redis_openstack_37]

- 프로그램 라이선스에 관련된 내용이다. I Agree 버튼을 클릭한다.<br>
![redis_openstack_38]

- Redis Desktop Manager를 설치할 경로를 설정 후 Install 버튼을 클릭한다.
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.<br>
![redis_openstack_39]

- 설치 완료 후 Next 버튼을 클릭하여 다음 과정을 진행한다.<br>
![redis_openstack_40]

- Finish 버튼 클릭으로 설치를 완료한다.<br>
![redis_openstack_41]

- Redis Desktop Manager를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect to Redis Server 버튼을 클릭한다.<br>
![redis_openstack_42]

- Connection 탭에서 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.<br>
![redis_openstack_43]

- 서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cfenv<app_name> 명령어로 이용하여 확인한다.
예) $ cfenvredis-example-app<br>
![redis_openstack_44]

- SSH Tunnel탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력하고 공개키(.pem) 파일을 불러온다. Test Connection 버튼을 클릭하면 Redis 서버에 접속이 되는지 테스트 하고 OK 버튼을 눌러 Redis 서버에 접속한다.
(참고) 만일 공개키 없이 ID/Password로 접속이 가능한 경우에는 공개키 대신 사용자 이름과 암호를 입력한다.<br>
![redis_openstack_45]

- 접속이 완료되고 좌측 서버 정보를 더블 클릭하면 좌측에 스키마 정보가 나타난다.<br>
![redis_openstack_46]

- 신규 키 등록후 확인<br>
![redis_openstack_47]








[redis_openstack_02]:/images/openpaas-service/redis/redis_openstack/redis_openstack_02.png
[redis_openstack_03]:/images/openpaas-service/redis/redis_openstack/redis_openstack_03.png
[redis_openstack_04]:/images/openpaas-service/redis/redis_openstack/redis_openstack_04.png
[redis_openstack_05]:/images/openpaas-service/redis/redis_openstack/redis_openstack_05.png
[redis_openstack_06]:/images/openpaas-service/redis/redis_openstack/redis_openstack_06.png
[redis_openstack_07]:/images/openpaas-service/redis/redis_openstack/redis_openstack_07.png
[redis_openstack_08]:/images/openpaas-service/redis/redis_openstack/redis_openstack_08.png
[redis_openstack_09]:/images/openpaas-service/redis/redis_openstack/redis_openstack_09.png
[redis_openstack_10]:/images/openpaas-service/redis/redis_openstack/redis_openstack_10.png
[redis_openstack_11]:/images/openpaas-service/redis/redis_openstack/redis_openstack_11.png
[redis_openstack_12]:/images/openpaas-service/redis/redis_openstack/redis_openstack_12.png
[redis_openstack_13]:/images/openpaas-service/redis/redis_openstack/redis_openstack_13.png
[redis_openstack_14]:/images/openpaas-service/redis/redis_openstack/redis_openstack_14.png
[redis_openstack_15]:/images/openpaas-service/redis/redis_openstack/redis_openstack_15.png
[redis_openstack_16]:/images/openpaas-service/redis/redis_openstack/redis_openstack_16.png
[redis_openstack_17]:/images/openpaas-service/redis/redis_openstack/redis_openstack_17.png
[redis_openstack_18]:/images/openpaas-service/redis/redis_openstack/redis_openstack_18.png
[redis_openstack_19]:/images/openpaas-service/redis/redis_openstack/redis_openstack_19.png
[redis_openstack_20]:/images/openpaas-service/redis/redis_openstack/redis_openstack_20.png
[redis_openstack_21]:/images/openpaas-service/redis/redis_openstack/redis_openstack_21.png
[redis_openstack_22]:/images/openpaas-service/redis/redis_openstack/redis_openstack_22.png
[redis_openstack_23]:/images/openpaas-service/redis/redis_openstack/redis_openstack_23.png
[redis_openstack_24]:/images/openpaas-service/redis/redis_openstack/redis_openstack_24.png
[redis_openstack_25]:/images/openpaas-service/redis/redis_openstack/redis_openstack_25.png
[redis_openstack_26]:/images/openpaas-service/redis/redis_openstack/redis_openstack_26.png
[redis_openstack_27]:/images/openpaas-service/redis/redis_openstack/redis_openstack_27.png
[redis_openstack_28]:/images/openpaas-service/redis/redis_openstack/redis_openstack_28.png
[redis_openstack_29]:/images/openpaas-service/redis/redis_openstack/redis_openstack_29.png
[redis_openstack_30]:/images/openpaas-service/redis/redis_openstack/redis_openstack_30.png
[redis_openstack_31]:/images/openpaas-service/redis/redis_openstack/redis_openstack_31.png
[redis_openstack_32]:/images/openpaas-service/redis/redis_openstack/redis_openstack_32.png
[redis_openstack_33]:/images/openpaas-service/redis/redis_openstack/redis_openstack_33.png
[redis_openstack_34]:/images/openpaas-service/redis/redis_openstack/redis_openstack_34.png
[redis_openstack_35]:/images/openpaas-service/redis/redis_openstack/redis_openstack_35.png
[redis_openstack_36]:/images/openpaas-service/redis/redis_openstack/redis_openstack_36.png
[redis_openstack_37]:/images/openpaas-service/redis/redis_openstack/redis_openstack_37.png
[redis_openstack_38]:/images/openpaas-service/redis/redis_openstack/redis_openstack_38.png
[redis_openstack_39]:/images/openpaas-service/redis/redis_openstack/redis_openstack_39.png
[redis_openstack_40]:/images/openpaas-service/redis/redis_openstack/redis_openstack_40.png
[redis_openstack_41]:/images/openpaas-service/redis/redis_openstack/redis_openstack_41.png
[redis_openstack_42]:/images/openpaas-service/redis/redis_openstack/redis_openstack_42.png
[redis_openstack_43]:/images/openpaas-service/redis/redis_openstack/redis_openstack_43.png
[redis_openstack_44]:/images/openpaas-service/redis/redis_openstack/redis_openstack_44.png
[redis_openstack_45]:/images/openpaas-service/redis/redis_openstack/redis_openstack_45.png
[redis_openstack_46]:/images/openpaas-service/redis/redis_openstack/redis_openstack_46.png
[redis_openstack_47]:/images/openpaas-service/redis/redis_openstack/redis_openstack_47.png
