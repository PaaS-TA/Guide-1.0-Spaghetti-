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
     * [3.1. Sample App 구조](#sample-App-구조)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#개방형-클라우드-플랫폼에서-서비스-신청)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#sample-app에-서비스-바인드-신청-및-app-확인)
4. [Redis Client 툴 접속](#redis-client-툴-접속)
     * [4.1. Redis Desktop Manager 설치 & 연결](#redis-desktop-manager-설치-&-연결)


# 문서 개요

### 목적
      
본 문서(Redis서비스팩설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인Redis서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서Redis 서비스를 사용하는 방법을 기술하였다.


### 범위 

설치 범위는 Redis서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

### 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. Redisdedicated-node(5대), Redis 서비스 브로커로 최소사항을 구성하였다.
![시스템 구성도][redis_bosh_lite_02]
### 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

# 	Redis 서비스팩 설치

###	설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH-lite 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH-lite 가 설치 되어 있지 않을 경우 먼저 BOSH-lite 설치 가이드 문서를 참고 하여 BOSH-lite를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

###		Redis 서비스 릴리즈 업로드

-	OpenPaaS-Services.zip 파일 압축을 풀고폴더안에 있는 Redis 서비스 릴리즈openpaas-redis-release-beta-1.0.tgz 파일을복사한다.
- 업로드할 openpaas-redis-release-beta-1.0.tgz 파일을 확인한다.


><div>$ls –all</div>	
![redis_bosh_lite_03]


-	업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$bosh releases</div>
![redis_bosh_lite_04]
><div>Redis 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</div>
>

-	Redis 서비스 릴리즈 파일을업로드한다.

><div>$ bosh upload release {서비스릴리즈 파일 PATH}</div><br>
><div>$ bosh upload release openpaas-redis-release-beta-1.0.tgz</div><br>
><br>
>![redis_bosh_lite_05]
>![redis_bosh_lite_06]
>![redis_bosh_lite_07]
>![redis_bosh_lite_08]

-	업로드 된 Redis릴리즈를 확인한다.

><div>$bosh releases</div><br>
>![redis_bosh_lite_09]
><div>Redis 서비스 릴리즈가 업로드 되어 있는 것을 확인</div>

###		Redis 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고폴더안에 있는lite용 Redis Deployment 화일인openpaas-redis-lite.yml를 복사한다.
- 다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-redis-lite.yml)

><div>$ls –all</div>
>![redis_bosh_lite_10]

-	Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.

><div>$bosh status</div>
>![redis_bosh_lite_11]

 -	Deploy시 사용할 Stemcell을 확인한다. (Stemcell2776 버전 사용)
 
><div>$bosh stemcells</div>
>![redis_bosh_lite_12]
><div>Stemcell목록이 존재 하지 않을 경우 BOSH-lite 설치 가이드 문서를 참고 하셔서 Stemcell2776 버전을 업로드를 해야 한다.</div>

-	openpaas-redis-lite.yml Deployment 파일을 서버 환경에 맞게 수정한다.(빨간색으로 표시된 부분만 수정)

<pre>$vi openpaas-redis-lite.yml

# openpaas-redis-lite 설정 파일 내용
---
name: openpaas-redis-service                             # 서비스 배포이름(필수)
director_uuid: xxxxx#bosh status 에서 확인한 Director UUID을 입력(필수)
releases:
- name: openpaas-redis                       #서비스 릴리즈 이름(필수)
  version: beta-1.0             #서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전
compilation:                 #컴파일시 필요한 가상머신의 속성(필수)
cloud_properties:    # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    name: random
  network: services                   # Networks block에서 선언한 network 이름(필수)
reuse_compilation_vms: true        # 컴파일지 VM 재사용 여부(옵션)
  workers: 2                       # 컴파일 하는 가상머신의 최대수(필수)
update:
  canaries: 1                      # canary 인스턴스 수(필수)
canary_watch_time: 30000-180000  # canary 인스턴스가 수행하기 위한 대기 시간(필수)
update_watch_time: 30000-180000  # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
max_in_flight: 4     # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
networks:     # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, bosh lite 에서는 제공하는 네트워크를 수정 없이 사용
- name: services
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.3.32/30
    reserved:
    - 10.244.3.33
  - cloud_properties:
      name: random
    range: 10.244.3.36/30
    reserved:
    - 10.244.3.37
  - cloud_properties:
      name: random
    range: 10.244.3.40/30
    reserved:
    - 10.244.3.41
  - cloud_properties:
      name: random
    range: 10.244.3.44/30
    reserved:
    - 10.244.3.45
    static:
    - 10.244.3.46
  - cloud_properties:
      name: random
    range: 10.244.3.48/30
    reserved:
    - 10.244.3.49
  - cloud_properties:
      name: random
    range: 10.244.3.52/30
    reserved:
    - 10.244.3.53
    static:
    - 10.244.3.54
  - cloud_properties:
      name: random
    range: 10.244.3.56/30
    reserved:
    - 10.244.3.57
    static:
    - 10.244.3.58
  - cloud_properties:
      name: random
    range: 10.244.3.60/30
    reserved:
    - 10.244.3.61
    static:
    - 10.244.3.62
  - cloud_properties:
      name: random
    range: 10.244.3.64/30
    reserved:
    - 10.244.3.65
    static:
    - 10.244.3.66
  - cloud_properties:
      name: random
    range: 10.244.3.68/30
    reserved:
    - 10.244.3.69
    static:
    - 10.244.3.70
resource_pools:             # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:           # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    ram: 1024
    name: random
  name: services-small
  network: services
stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent#stemcell 이름(필수)
    version: latest                                      # stemcell 버전(필수)
jobs:
- name: dedicated-node              # 작업 이름(필수)
  templates:
  - name: dedicated-node            # job template 이름(필수)
    release: openpaas-redis                  #서비스 릴리즈 이름(필수)
  - name: syslog-configurator
    release: openpaas-redis
  instances: 5                       # 생성하는 인스턴스 수(필수)
resource_pool: services-small
persistent_disk: 4096             # 영구적 디스크 사이즈 정의(옵션): 4G
  networks:
  - name: services
static_ips:                     # 전용 노드 IP 목록(필수)
    - 10.244.3.54
    - 10.244.3.58
    - 10.244.3.62
    - 10.244.3.66
    - 10.244.3.70
- name: openpaas-redis-broker
  templates:
  - name: cf-redis-broker                 # job template 이름(필수)
    release: openpaas-redis                   #서비스 릴리즈 이름(필수)
  - name: syslog-configurator
    release: openpaas-redis
  instances: 1
resource_pool: services-small
persistent_disk: 4096                   # 영구적 디스크 사이즈 정의(옵션): 4G
  networks:
  - name: services
static_ips:
    - 10.244.3.46
- name: broker-registrar                # 작업 이름: 서비스 브로커 등록
  instances: 1
  lifecycle: errand                       # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로실행할때 설정, 주로 테스트 용도에 쓰임
  networks:
  - name: services
resource_pool: services-small
  templates:
  - name: broker-registrar
    release: openpaas-redis
  properties:
    broker:# 서비스 브로커 설정 정보
      name: openpaas-redis-service
      host: redis-broker.10.244.0.34.xip.io
      username: admin
      password: admin
- name: broker-deregistrar
  instances: 1
  lifecycle: errand
  networks:
  - name: services
resource_pool: services-small
  templates:
  - name: broker-deregistrar
    release: openpaas-redis
  properties:
    broker:
      name: openpaas-redis-service
      host: redis-broker.10.244.0.34.xip.io
      username: admin
      password: admin
- name: smoke-tests
  instances: 1
  lifecycle: errand
  networks:
  - name: services
resource_pool: services-small
  templates:
  - name: smoke-tests
    release: openpaas-redis
properties:
syslog_aggregator:
    address: 127.0.0.1
    port: 1234
cf:
api_url: http://api.10.244.0.34.xip.io# CF API url(필수)
apps_domain: 10.244.0.34.xip.io               # CF 도메인(필수)
admin_username: admin                     # CF 어드민 아이디 (필수)
admin_password: admin                    # CF 어드민 아이디 비밀번호(필수)
nats:                               # CF 설치시 설치한 nats 정보 (필수)
      host: 10.244.0.6
      port: 4222
      username: nats
      password: nats
redis:
    host: 10.244.3.46
maxmemory: 262144000             # 최대 메모리
config_command: config                 # config 설정 명령어가 있을 경우(옵션)
save_command: save                    # 저장 명령어가 있을 경우(옵션)
bg_save_command: bgsave
    agent:
backend_port: 54321                    # Redis backend 포트번호(필수)
    broker:
      network: services
backend_host: 10.244.3.46
backend_port: 12345
dedicated_nodes:
      - 10.244.3.54
      - 10.244.3.58
      - 10.244.3.62
      - 10.244.3.66
      - 10.244.3.70
service_instance_limit: 3                     # 서비스 인스턴스 생성 가능 수(필수)
service_name: redis-sb
auth:
        username: admin
        password: admin
      backups:
        path: "/backups"
access_key_id: ZZZZZZZZZZZZZZZ
secret_access_key: ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
endpoint_url: https://s3.amazonaws.com
bucket_name: my-backup-bucket

</pre>

-	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

><div>$bosh deployment {Deployment manifest 파일 PATH}</div>
><div>$bosh deployment openpaas-redis-lite.yml</div>
>![redis_bosh_lite_13]

-	Redis서비스팩을 배포한다.

><div>$bosh deploy</div>
>![redis_bosh_lite_14]
>![redis_bosh_lite_15]

-	배포된 Redis서비스팩을 확인한다.

><div>$bosh vms</div>
>![redis_bosh_lite_16]
>![redis_bosh_lite_17]

###	Redis 서비스 브로커 등록
Redis서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Redis 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시개방형 클라우드 플랫폼에서서비스브로커를등록할수있는사용자로로그인이되어있어야한다.


-	서비스 브로커 목록을 확인한다.

><div>$cf service-brokers</div>
>![redis_bosh_lite_18]

-	Redis 서비스 브로커를 등록한다.

><div>$cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
-	서비스팩이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
-	서비스팩 사용자ID / 비밀번호 :서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
-	서비스팩URL :서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.</div><br>
><div>$ cf create-service-broker redis-service-broker admin adminhttp://10.244.3.46:80</div>
>![redis_bosh_lite_19]

-	등록된 Redis 서비스 브로커를 확인한다.

><div>$cf service-brokers</div>
>![redis_bosh_lite_20]

-	접근 가능한 서비스 목록을 확인한다.

><div>$cf service-access</div>
>![redis_bosh_lite_21]
><div>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.</div>

-	특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

><div>$cf enable-service-access redis-sb</div>
><div>$ cf service-access</div>
>![redis_bosh_lite_22]

#		Redis 연동 Sample App 설명
본 Sample App은 개발형클라우드 플랫폼에 배포되며 redis의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
###	Sample App 구조
Sample App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. 배포 완료 후 정상적으로 App 이 구동되면 curl 명령어로 App url 를 입력하면 redis 환경정보(서비스 연결 정보)를 보여주는 url 페이지를 제공한다.

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

-	OpenPaaS-Sample-Apps.zip 파일 압축을 풀고 Service폴더안에 있는 RedisSample Web App인 redis-example-app을복사한다

><div>$ls -all</div>
>![redis_bosh_lite_23]

###	개방형 클라우드 플랫폼에서 서비스 신청
Sample App에서 Redis 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$cf marketplace</div>
>![redis_bosh_lite_24]

-	Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
-	서비스명 :redis-sb로 Marketplace에서 보여지는 서비스 명칭이다.<br>
-	서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Redis 서비스는 shared-vm 과 dedicated-vmplan을지원한다.<br>
-	내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.</div><br>

><div>$ cf create-service redis-sbshared-vmredis-service-instance</div>
>![redis_bosh_lite_25]

-	생성된 Redis 서비스 인스턴스를 확인한다.

><div>$cf services</div>
>![redis_bosh_lite_26]

###	Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Redis 서비스를 이용한다.
*참고: 서비스 Bind 신청시개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample App 디렉토리로 이동하여 manifest 파일을 확인한다

><div>$cdredis-example-app<br>
$ vi manifest.yml<br><br>
---<br>
applications:<br>
- name: redis-example-app#배포할 App 이름<br>
  memory: 256M#배포시 메모리 크기<br>
  instances: 1#배포시인스턴스 수<br>
path: .#배포시 구동되는 Path<br>
 </div>

-	--no-start 옵션으로 App을 배포한다. 
--no-start: App 배포시 구동은 하지 않는다.

><div>$cf push --no-start<br></div>
>![redis_bosh_lite_27]

-	배포된 Sample App을 확인하고 로그를 수행한다.

><div>$cf apps<br></div>
>![redis_bosh_lite_28]
><div>$ cf logs {배포된 App명}
>$ cf logs redis-example-app</div>
>![redis_bosh_lite_29]

-	Sample App에서 생성한 서비스인스턴스바인드 신청을 한다. 

><div>$cf bind-service redis-example-appredis-service-instance</div>
>![redis_bosh_lite_30]

-	바인드가 적용되기 위해서 App을 재기동한다.

><div>$cf restart redis-example-app</div>
>![redis_bosh_lite_31]
>![redis_bosh_lite_32]

-	App이 정상적으로 Redis 서비스를 사용하는지 확인한다.

><div>-	curl 로 확인<br>
-	Redis Application 을 이용하여 Key 값으로 데이터 저장<br>
$ export APP=redis-example-app.10.244.0.34.xip.io<br>
$ curl -X PUT $APP/foo -d 'data=bar'<br>
><div>
>![redis_bosh_lite_33]
><div>-	Key 값을 이용하여 Redis 에 저장되어 있는 Value 값 Get<br>
$ curl -X GET $APP/foo<br>
></div>
>![redis_bosh_lite_34]
><div>-	Key 값을 이용하여 Redis 에 저장되어 있는 Value 삭제<br>
$ curl -X DELETE $APP/foo<br>
></div>
>![redis_bosh_lite_35]

#	Redis Client 툴 접속
Application에 바인딩된Redis 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Redis Client 툴에서 직접 연결할수 없다. 따라서 Redis Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Redis Client 툴로써는 오픈 소스인 Redis Desktop Manager로 가이드한다. 또한 Bosh lite를 AWS 환경에서 구성 한 경우를 전제로 하였다. AWS에서 Bosh lite를 구성하면 Vagrant VM이 생성되는데 Vagrant VM 에서는 서비스팩의 Private IP 와 해당 포트로 접근이 가능하도록 구성되어 있다.
###	Redis Desktop Manager 설치 & 연결
Redis Desktop Manager 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

-	Redis Desktop Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.
[**http://redisdesktop.com/download**](http://redisdesktop.com/download)<br>
![redis_bosh_lite_36]

- 다운로드한 설치파일을 실행한다.<br>
![redis_bosh_lite_37]

- Redis Desktop Manager 설치를 위한 안내사항이다. Next 버튼을 클릭한다.<br>
![redis_bosh_lite_38]

- 프로그램 라이선스에 관련된 내용이다. I Agree 버튼을 클릭한다.<br>
![redis_bosh_lite_39]

- Redis Desktop Manager를 설치할 경로를 설정 후 Install 버튼을 클릭한다.
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.<br>
![redis_bosh_lite_40]

- 설치 완료 후 Next 버튼을 클릭하여 다음 과정을 진행한다.<br>
![redis_bosh_lite_41]

- Finish 버튼 클릭으로 설치를 완료한다.<br>
![redis_bosh_lite_42]

- Redis Desktop Manager를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect to Redis Server 버튼을 클릭한다.<br>
![redis_bosh_lite_43]

- Connection 탭에서 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.<br>
![redis_bosh_lite_44]

- 서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cfenv<app_name> 명령어로 이용하여 확인한다.
예) $ cfenvredis-example-app<br>
![redis_bosh_lite_45]

-	SSH Tunnel탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력하고 공개키(.pem) 파일을 불러온다. Test Connection 버튼을 클릭하면 Redis 서버에 접속이 되는지 테스트 하고 OK 버튼을 눌러 Redis 서버에 접속한다.
(참고) 만일 공개키 없이 ID/Password로 접속이 가능한 경우에는 공개키 대신 사용자 이름과 암호를 입력한다.<br>
![redis_bosh_lite_46]

-	접속이 완료되고 좌측 서버 정보를 더블 클릭하면 좌측에 스키마 정보가 나타난다.<br>
![redis_bosh_lite_47]

- 신규 키 등록후 확인<br>
![redis_bosh_lite_48]








[redis_bosh_lite_02]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_02.png
[redis_bosh_lite_03]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_03.png
[redis_bosh_lite_04]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_04.png
[redis_bosh_lite_05]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_05.png
[redis_bosh_lite_06]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_06.png
[redis_bosh_lite_07]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_07.png
[redis_bosh_lite_08]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_08.png
[redis_bosh_lite_09]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_09.png
[redis_bosh_lite_10]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_10.png
[redis_bosh_lite_11]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_11.png
[redis_bosh_lite_12]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_12.png
[redis_bosh_lite_13]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_13.png
[redis_bosh_lite_14]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_14.png
[redis_bosh_lite_15]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_15.png
[redis_bosh_lite_16]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_16.png
[redis_bosh_lite_17]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_17.png
[redis_bosh_lite_18]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_18.png
[redis_bosh_lite_19]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_19.png
[redis_bosh_lite_20]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_20.png
[redis_bosh_lite_21]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_21.png
[redis_bosh_lite_22]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_22.png
[redis_bosh_lite_23]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_23.png
[redis_bosh_lite_24]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_24.png
[redis_bosh_lite_25]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_25.png
[redis_bosh_lite_26]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_26.png
[redis_bosh_lite_27]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_27.png
[redis_bosh_lite_28]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_28.png
[redis_bosh_lite_29]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_29.png
[redis_bosh_lite_30]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_30.png
[redis_bosh_lite_31]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_31.png
[redis_bosh_lite_32]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_32.png
[redis_bosh_lite_33]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_33.png
[redis_bosh_lite_34]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_34.png
[redis_bosh_lite_35]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_35.png
[redis_bosh_lite_36]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_36.png
[redis_bosh_lite_37]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_37.png
[redis_bosh_lite_38]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_38.png
[redis_bosh_lite_39]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_39.png
[redis_bosh_lite_40]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_40.png
[redis_bosh_lite_41]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_41.png
[redis_bosh_lite_42]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_42.png
[redis_bosh_lite_43]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_43.png
[redis_bosh_lite_44]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_44.png
[redis_bosh_lite_45]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_45.png
[redis_bosh_lite_46]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_46.png
[redis_bosh_lite_47]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_47.png
[redis_bosh_lite_48]:/images/openpaas-service/redis/redis_bosh_lite/redis_bosh_lite_48.png
