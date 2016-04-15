## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 시스템 구성도](#4)
     * [1.4. 참고자료](#5)
2. [Redis 서비스팩 설치](#6)
     * [2.1. 설치전 준비사항](#7)
     * [2.2. Redis 서비스 릴리즈 업로드](#8)
     * [2.3. Redis 서비스 Deployment 파일 수정 및 배포](#9)
     * [2.4. Redis 서비스 브로커 등록](#10)
3. [Redis 연동 Sample App 설명](#11)
     * [3.1. Sample App 구조](#12)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#13)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#14)
4. [Redis Client 툴 접속](#15)
     * [4.1. Redis Desktop Manager 설치 및 연결](#16)

<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
      
본 문서(Redis 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 Redis 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application에서 Redis 서비스를 사용하는 방법을 기술하였다.

<div id='3'></div>
### 1.2. 범위 

설치 범위는 Redis 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

<div id='4'></div>
### 1.3. 시스템 구성도

본 문서의 설치된 시스템 구성도입니다. Redis dedicated-node(2대), Redis 서비스 브로커로 최소사항을 구성하였다.

![redis_aws1]

<table>
  <tr>
    <td>구분</td>
    <td>Resource Pool</td>
    <td>스펙</td>
  </tr>
  <tr>
    <td>open-redis-broker</td>
    <td>m1.small</td>
    <td>1vCPU / 1.7GB RAM / 8GB Disk+2GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node1</td>
    <td>m1.small</td>
    <td>1vCPU / 1.7GB RAM / 8GB Disk+2GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>dedicated-node2</td>
    <td>m1.small</td>
    <td>1vCPU / 1.7GB RAM / 8GB Disk+2GB(영구적 Disk)</td>
  </tr> 
</table>

<div id='5'></div>
### 1.4. 참고자료

[**http://bosh.io/docs**](http://bosh.io/docs)
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

<div id='6'></div>
#   2. Redis서비스팩설치

<div id='7'></div>
### 2.1. 설치전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

<div id='8'></div>
###   2.2. Redis 서비스 릴리즈 업로드

-	OpenPaaS-Services.zip 파일 압축을 풀고 폴더안에 있는 Redis 서비스 릴리즈 openpaas-redis-release-1.0.tgz 파일을 복사한다.
업로드할 openpaas-redis-release-1.0.tgz 파일을 확인한다.


><div>$ls -all</div>  
![redis_aws2]

- 업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$bosh releases</div>
![redis_aws3]
><div>Redis 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</div>
>

- Redis 서비스 릴리즈 파일을업로드한다.

><div>$ bosh upload release {서비스릴리즈 파일 PATH}</div><br>
><div>$ bosh upload release openpaas-redis-release-beta-1.0.tgz</div><br>
><br>※	본 샘플은 직접 릴리즈 파일을 생성하도록 안내하고 있으므로 'bosh upload release'만 입력한다.릴리즈 파일을 직접 생성하지 않고 다운로드 받거나 릴리즈 파일 다운로드 URL이 존재하는 경우는 파일 경로 또는 다운로드 URL을 아래와 같이 입력한다. 
>$ bosh upload release {서비스 릴리즈 파일 PATH 또는 URL}<br>
>![redis_aws4]
>![redis_aws5]
>![redis_aws6]
>![redis_aws7]
>![redis_aws8]
>![redis_aws9]

- 업로드 된 Redis릴리즈를 확인한다.

><div>$bosh releases</div>
>![redis_aws10]
><div>Redis 서비스 릴리즈가 업로드 되어 있는 것을 확인</div>





<div id='9'></div>
###   2.3. Redis 서비스 Deployment 파일 수정 및 배포

BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 AWS용 Redis Deployment 화일인 openpaas-redis-aws-1.0.yml를 복사한다.
다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-redis-aws.yml-1.0)


><div>$ls -all</div>
>![redis_aws11]

-	Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.


><div>$bosh status</div>
>![redis_aws12]

-	Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3147 버전 사용)
 
><div>$bosh stemcells</div>
>![redis_aws13]
><div>Stemcell목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여Stemcell 3147  버전을 업로드를 해야 한다.</div>

-	openpaas-redis-aws-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다. (빨간색으로 표시된 부분 특히 주의)

>$vi openpaas-redis-vsphere.yml
```yaml
# openpaas-redis-aws-1.0 설정 파일 내용
name: openpaas-redis-service                            # 서비스 배포 이름(필수)
director_uuid: 3475c880-8836-4a73-9309-c65bc9ac20c6     # bosh status 에서 확인한 Director UUID을 입력(필수)
releases:
- name: cf-redis                  # 서비스 릴리즈 이름(필수)
  version: 1.0               # 서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전
update:
  canaries: 1                          # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000      # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                     # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000      # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
compilation::                        # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:                  # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)
    instance_type: m1.small          # 인스턴스 타입: Flavors 타입 (필수)
  network: openpaas_network          # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true        # 컴파일지 VM 재사용 여부(옵션)
  workers: 6                         # 컴파일 하는 가상머신의 최대수(필수)
jobs:
- instances: 1
  name: openpaas-redis-broker       # 작업 이름(필수)
  networks:                         # 네트워크 구성정보
  - name: openpaas_network          # Networks block에서 선언한 network 이름(필수)
    static_ips:
    - 10.20.0.86                    # 사용할 IP addresses 정의(필수)
  persistent_disk: 2048                 # 영구적 디스크 사이즈 정의(옵션): 2G
  properties:
    broker:                         # 브로커에 대한 설정(필수): 사용자, 패스워드
      password: admin
      username: admin
    host: 10.20.0.86
    syslog_aggregator: null
  resource_pool: services-small     # Resource Pools block에 정의한 resource pool 이름(필수)
  templates:
  - name: cf-redis-broker           # job template 이름(필수)
    release: cf-redis               # 서비스 릴리즈 이름(필수)
- instances: 5                      # 생성하는 인스턴스 수(필수)
  name: dedicated-node              # 작업 이름(필수)
  networks:
  - name: openpaas_network          # Networks block에서 선언한 network 이름(필수)
    static_ips:                     # 전용 노드 IP 목록(필수)
    - 10.20.0.81
    - 10.20.0.82
    - 10.20.0.83
    - 10.20.0.84
    - 10.20.0.85
  persistent_disk: 2048                 # 영구적 디스크 사이즈 정의(옵션): 2G
  resource_pool: services-small
  templates:
  - name: dedicated-node            # job template 이름(필수)
    release: cf-redis
  syslog_aggregator: null
- instances: 1
  lifecycle: errand                # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar           # 작업 이름: 서비스 브로커 등록
  networks:
  - name: openpaas_network
  properties:
    broker:                          # 서비스 브로커 설정 정보
      host: 10.20.0.86               # 서비스 브로커 IP
      name: openpaas-redis-service
      password: admin
      username: admin
      protocol: http
      port: 80
    cf:                            # CF 설정 정보
      admin_password: admin
      admin_username: admin
      api_url: http://api.52.71.64.39.xip.io
    redis:
      broker:
        enable_serive_access: true              # 서비스 접속 허용 여부(필수)
        service_name: redis-sb                  # 서비스 이름
  resource_pool: services-small
  templates:
  - name: broker-registrar                # job template 이름(필수)
    release: cf-redis
- instances: 1
  lifecycle: errand
  name: broker-deregistrar               # 작업이름: 서비스 브로커 삭제
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: 10.20.0.81
      name: openpaas-redis-service
      password: admin
      username: admin
  resource_pool: services-small
  templates:
  - name: broker-deregistrar
    release: cf-redis
- instances: 1
  lifecycle: errand
  name: smoke-tests                       # 작업이름: 서비스팩이 정상적으로 설치 되었는 테스트
  networks:
  - name: openpaas_network
  resource_pool: services-small
  templates:
  - name: smoke-tests
    release: cf-redis
meta:
  apps_domain: 52.71.64.39.xip.io             # CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: 52.71.64.39.xip.io         # CF 설치시 설정한 외부 도메인 정보
  nats:                                             # CF 설치시 설치한 nats 정보 (필수)
    host: 10.20.0.11                        # nats 서버 IP
    password: admin                         # nats 유저 비밀번호
    port: 4222                              # nats 서버 포트번호
    username: nats                          # nats 서버 유저아이디
  syslog_aggregator: null
networks:
- name: openpaas_network
  subnets:
  - cloud_properties:
      net_id: 7b49e746-161a-4f90-9ed6-c93e27122a1a        # aws에서 사용하는 network 이름 아이디(필수)
      security_groups:                                    # aws 에서 사용하는 접근 시큐리티 이름 이름(필수)
      - cf-security
      - bosh
      - default     
    dns:                                        # DNS 정보
    - 10.20.0.3
    - 8.8.8.8
    gateway: 10.20.0.1
    range: 10.20.0.0/24
    reserved:                            # 설치시 제외할 IP 설정 (OpenStack 에서는 서비스팩 설치 구간 이외에는 IP 제외 설정을 해줘야 오류가 나지 않음.
    - 10.20.0.2 - 10.20.0.80
    static:                              # 사용 가능한 IP 설정
    - 10.20.0.81 - 10.20.0.86
  type: manual
properties:
  cf:
    admin_password: admin                                # CF 어드민 아이디 비밀번호(필수)
    admin_username: admin                                # CF 어드민 아이디 (필수)
    api_url: http://api.52.71.64.39.xip.io         # CF API url(필수)
    apps_domain: 52.71.64.39.xip.io                # CF 도메인(필수)
    nats:                                        # CF 설치시 설치한 nats 정보 (필수)
      host: 10.20.0.11
      password: admin
      port: 4222
      username: nats
  redis:
    agent:
      backend_port: 54321                         # Redis backend 포트번호(필수)
    bg_save_command: yetanotherrandomstring
    broker:
      auth:
        password: admin
        username: admin
      backend_host: 10.20.0.86
      backend_port: 12345
      dedicated_nodes:
      - 10.20.0.81
      - 10.20.0.82
      - 10.20.0.83
      - 10.20.0.84
      - 10.20.0.85
      enable_service_access: true
      name: openpaas-redis-service
      network: openpaas_network
      service_instance_limit: 5                 # 서비스 인스턴스 생성 가능 수(필수)
      service_name: redis-sb
    config_command: configalias                 # config 설정 명령어가 있을 경우(옵션)
    host: 10.20.0.86
    maxmemory: 262144000                        # 최대 메모리
    save_command: anotherrandomstring           # 저장 명령어가 있을 경우(옵션)
  syslog_aggregator: null
resource_pools:                  # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:              # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone)
    instance_type: m1.small      # 인스턴스 타입: Flovers 타입(필수)
  name: services-small           # 고유한 resource pool 이름
  env:                           # 환경 정보(옵션)
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: openpaas_network
    #size: 7  # resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent     #stemcell 이름(필수)
    version: 3147                                        #stemcell 버전(필수)
```

Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

><div>$bosh deployment {Deployment manifest 파일 PATH}</div>
><div>$bosh deployment openpaas-redis-aws-1.0.yml</div>
>![redis_aws14]

- Redis서비스팩을 배포한다.

><div>$bosh deploy</div>
>![redis_aws15]
>![redis_aws16]
>![redis_aws17]

- 배포된 Redis서비스팩을 확인한다.

><div>$bosh vms</div>
>![redis_aws18]

<div id='10'></div>
### 2.4. Redis 서비스 브로커 등록

Redis서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Redis 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시개방형 클라우드 플랫폼에서서비스브로커를등록할수있는사용자로로그인이되어있어야한다.

- 서비스 브로커 목록을 확인한다.

><div>$cf service-brokers</div>
>![redis_aws19]

- Redis 서비스 브로커를 등록한다.

><div>cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
- 서비스팩이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
- 서비스팩 사용자ID / 비밀번호 :서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
- 서비스팩URL :서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.</div><br>
><div>$ cf create-service-broker redis-service-broker admin adminhttp://10.30.40.171:80</div>
>![redis_aws20]

- 등록된 Redis 서비스 브로커를 확인한다.

><div>$cf service-brokers</div>
>![redis_aws21]

- 접근 가능한 서비스 목록을 확인한다.

><div>$cf service-access</div>
>![redis_aws22]
><div>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.</div>

- 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

><div>$cf enable-service-access redis-sb</div>
><div>$ cf service-access</div>
>![redis_aws23]

<div id='11'></div>
#   3. Redis연동 Sample App 설명

본 Sample App은 개발형클라우드 플랫폼에 배포되며 Redis의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

<div id='12'></div>
### 3.1. Sample App 구조

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
>![redis_aws24]

<div id='13'></div>
### 3.2. 개방형 클라우드 플랫폼에서 서비스 신청

Sample App에서 Redis 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$cf marketplace</div>
>![redis_aws25]

- Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
- 서비스명 :redis-sb로 Marketplace에서 보여지는 서비스 명칭이다.<br>
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Redis 서비스는 shared-vm 과 dedicated-vmplan을지원한다.<br>
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.</div><br>

><div>$ cf create-service redis-sbshared-vmredis-service-instance</div>
>![redis_aws26]

- 생성된 Redis 서비스 인스턴스를 확인한다.

><div>$cf services</div>
>![redis_aws27]

<div id='14'></div>
### 3.3. Sample App에 서비스 바인드 신청 및 App 확인

서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Redis 서비스를 이용한다.
*참고: 서비스 Bind 신청시개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample App 디렉토리로 이동하여 manifest 파일을 확인한다

><div>$cd redis-example-app<br>
$ vi manifest.yml<br><br></div>
```yaml
---
applications:
- name: redis-example-app #배포할 App 이름
  memory: 256M #배포시 메모리 크기
  instances: 1 #배포시인스턴스 수
path: . #배포시 구동되는 Path
```
 

- --no-start 옵션으로 App을 배포한다. 
--no-start: App 배포시 구동은 하지 않는다.

><div>$cf push --no-start<br></div>
>![redis_aws28]

- 배포된 Sample App을 확인하고 로그를 수행한다.

><div>$cf apps<br></div>
>![redis_aws29]
><div>$ cf logs {배포된 App명}
>$ cf logs redis-example-app</div>
>![redis_aws30]

- Sample App에서 생성한 서비스인스턴스바인드 신청을 한다. 

><div>$cf bind-service redis-example-appredis-service-instance</div>
>![redis_aws31]

- 바인드가 적용되기 위해서 App을 재기동한다.

><div>$cf restart redis-example-app</div>
>![redis_aws32]

- App이 정상적으로 Redis 서비스를 사용하는지 확인한다.

><div>- curl 로 확인<br>
- Redis Application 을 이용하여 Key 값으로 데이터 저장<br>
$ export APP=redis-example-app.52.71.64.39.xip.io<br>
$ curl -X PUT $APP/foo -d 'data=bar'<br>
><div>
>![redis_aws33]
><div>- Key 값을 이용하여 Redis 에 저장되어 있는 Value 값 Get<br>
$ curl -X GET $APP/foo<br>
></div>
>![redis_aws34]
><div>- Key 값을 이용하여 Redis 에 저장되어 있는 Value 삭제<br>
$ curl -X DELETE $APP/foo<br>
></div>
>![redis_aws35]

<div id='15'></div>
# 4. Redis Client 툴 접속

Application에 바인딩된 Redis 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Redis Client 툴에서 직접 연결할수 없다. 따라서 Redis Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Redis Client 툴로써는 오픈 소스인 Redis Desktop Manager로 가이드한다. Redis Desktop Manager 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 AWS 관리자 및 OpenPaaS 운영자에게 문의하여 구성한다. 

<div id='16'></div>
### 4.1. Redis Desktop Manager 설치 및 연결
Redis Desktop Manager 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

- Redis Desktop Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.
[**http://redisdesktop.com/download**](http://redisdesktop.com/download)<br>
![redis_aws36]

- 다운로드한 설치파일을 실행한다.<br>
![redis_aws37]

- Redis Desktop Manager 설치를 위한 안내사항이다. Next 버튼을 클릭한다.<br>
![redis_aws38]

- 프로그램 라이선스에 관련된 내용이다. I Agree 버튼을 클릭한다.<br>
![redis_aws39]

- Redis Desktop Manager를 설치할 경로를 설정 후 Install 버튼을 클릭한다.
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.<br>
![redis_aws40]

- 설치 완료 후 Next 버튼을 클릭하여 다음 과정을 진행한다.<br>
![redis_aws41]

- Finish 버튼 클릭으로 설치를 완료한다.<br>
![redis_aws42]

- Redis Desktop Manager를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect to Redis Server 버튼을 클릭한다.<br>
![redis_aws43]

- Connection 탭에서 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.<br>
![redis_aws44]

- 서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cfenv<app_name> 명령어로 이용하여 확인한다.
예) $ cfenvredis-example-app<br>
![redis_aws45]

- SSH Tunnel탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력하고 공개키(.pem) 파일을 불러온다. Test Connection 버튼을 클릭하면 Redis 서버에 접속이 되는지 테스트 하고 OK 버튼을 눌러 Redis 서버에 접속한다.
(참고) 만일 공개키 없이 ID/Password로 접속이 가능한 경우에는 공개키 대신 사용자 이름과 암호를 입력한다.<br>
![redis_aws46]

- 접속이 완료되고 좌측 서버 정보를 더블 클릭하면 좌측에 스키마 정보가 나타난다.<br>
![redis_aws47]

- 신규 키 등록후 확인<br>
![redis_aws48]





[redis_aws1]:./image/redis_aws/redis_aws1.png
[redis_aws2]:./image/redis_aws/redis_aws2.png
[redis_aws3]:./image/redis_aws/redis_aws3.png
[redis_aws4]:./image/redis_aws/redis_aws4.png
[redis_aws5]:./image/redis_aws/redis_aws5.png
[redis_aws6]:./image/redis_aws/redis_aws6.png
[redis_aws7]:./image/redis_aws/redis_aws7.png
[redis_aws8]:./image/redis_aws/redis_aws8.png
[redis_aws9]:./image/redis_aws/redis_aws9.png
[redis_aws10]:./image/redis_aws/redis_aws10.png
[redis_aws11]:./image/redis_aws/redis_aws11.png
[redis_aws12]:./image/redis_aws/redis_aws12.png
[redis_aws13]:./image/redis_aws/redis_aws13.png
[redis_aws14]:./image/redis_aws/redis_aws14.png
[redis_aws15]:./image/redis_aws/redis_aws15.png
[redis_aws16]:./image/redis_aws/redis_aws16.png
[redis_aws17]:./image/redis_aws/redis_aws17.png
[redis_aws18]:./image/redis_aws/redis_aws18.png
[redis_aws19]:./image/redis_aws/redis_aws19.png
[redis_aws20]:./image/redis_aws/redis_aws20.png
[redis_aws21]:./image/redis_aws/redis_aws21.png
[redis_aws22]:./image/redis_aws/redis_aws22.png
[redis_aws23]:./image/redis_aws/redis_aws23.png
[redis_aws24]:./image/redis_aws/redis_aws24.png
[redis_aws25]:./image/redis_aws/redis_aws25.png
[redis_aws26]:./image/redis_aws/redis_aws26.png
[redis_aws27]:./image/redis_aws/redis_aws27.png
[redis_aws28]:./image/redis_aws/redis_aws28.png
[redis_aws29]:./image/redis_aws/redis_aws29.png
[redis_aws30]:./image/redis_aws/redis_aws30.png
[redis_aws31]:./image/redis_aws/redis_aws31.png
[redis_aws32]:./image/redis_aws/redis_aws32.png
[redis_aws33]:./image/redis_aws/redis_aws33.png
[redis_aws34]:./image/redis_aws/redis_aws34.png
[redis_aws35]:./image/redis_aws/redis_aws35.png
[redis_aws36]:./image/redis_aws/redis_aws36.png
[redis_aws37]:./image/redis_aws/redis_aws37.png
[redis_aws38]:./image/redis_aws/redis_aws38.png
[redis_aws39]:./image/redis_aws/redis_aws39.png
[redis_aws40]:./image/redis_aws/redis_aws40.png
[redis_aws41]:./image/redis_aws/redis_aws41.png
[redis_aws42]:./image/redis_aws/redis_aws42.png
[redis_aws43]:./image/redis_aws/redis_aws43.png
[redis_aws44]:./image/redis_aws/redis_aws44.png
[redis_aws45]:./image/redis_aws/redis_aws45.png
[redis_aws46]:./image/redis_aws/redis_aws46.png
[redis_aws47]:./image/redis_aws/redis_aws47.png
[redis_aws48]:./image/redis_aws/redis_aws48.png
