## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#1.1)
     * [1.2. 범위](#1.2)
     * [1.3. 시스템 구성도](#1.3)
     * [1.4. 참고자료](#1.4)
2. [MySQL 서비스팩 설치](#2)
     * [2.1. 설치전 준비사항](#2.1)
     * [2.2. MySQL 서비스 릴리즈 업로드](#2.2)
     * [2.3. MySQL 서비스 Deployment 파일 수정 및 배포](#2.3)
     * [2.4. MySQL 서비스 브로커 등록](#2.4)
3. [MySQL 연동 Sample Web App 설명](#3)
     * [3.1. Sample Web App 구조](#3.1)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#3.2)
     * [3.3. Sample Web App에 서비스 바인드 신청 및 App 확인](#3.3)
4. [MySQL Client 툴 접속](#4)
     * [4.1. HeidiSQL 설치 및 연결](#4.1)

<div id='1'>
#1. 문서 개요
</div>

<div id='1.1'>
### 1.1. 목적
</div>
  
본 문서(MySQL 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 MySQL 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 MySQL 서비스를 사용하는 방법을 기술하였다.

<div id='1.2'>
###1.2. 범위
</div>


설치 범위는 MySQL 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

<div id='1.3'>
###1.3. 시스템 구성도
</div>


본 문서의 설치된 시스템 구성도이다. MySQL Server, MySQL 서비스 브로커, Proxy로 최소사항을 구성하였다.
![시스템구성도][mysql_vsphere_1.3.01]

| 구분 | 스펙 |
|--------|-------|
| openpaas-mysql-broker |2vCPU / 2GB RAM / 20GB Disk |
| proxy | 2vCPU / 2GB RAM / 20GB Disk |
| server | 2vCPU / 2GB RAM / 20GB Disk+10GB(영구적 Disk) |

<div id='1.4'>
###1.4. 참고자료
</div>
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

<div id='2'>
#2. MySQL 서비스팩 설치
</div>

<div id='2.1'>
###2.1. 설치전 준비사항
</div>
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.  
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

<div id='2.2'>
###2.2. MySQL 서비스 릴리즈 업로드
</div>
-   OpenPaaS-Services.zip파일 압축을 풀고 폴더안에 있는 MySQL 서비스 릴리즈 openpaas-mysql-release-beta-1.0.tgz 파일을복사한다.
업로드할 openpaas-mysql-release-beta-1.0.tgz 파일을 확인한다.
>$ls –all<br>
>![mysql_vsphere_2.2.01]

-	업로드 되어 있는 릴리즈 목록을 확인한다.
>$bosh releases<br>
>![mysql_vsphere_2.2.02]<br>
>Mysql 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인  

-	MySQL 서비스 릴리즈 파일을 업로드한다.
>$ bosh upload release {서비스 릴리즈 파일 PATH}<br>
>$ bosh upload release openpaas-mysql-release-beta-1.0.tgz<br>
>![mysql_vsphere_2.2.03]<br>
>![mysql_vsphere_2.2.04]<br>
>![mysql_vsphere_2.2.05]<br>
>![mysql_vsphere_2.2.06]<br>
>![mysql_vsphere_2.2.07]

-	업로드 된MySQL 릴리즈를 확인한다.
>$bosh releases<br>
>![mysql_vsphere_2.2.08]<br>
>Mysql 서비스 릴리즈가 업로드 되어 있는 것을 확인

<div id='2.3'>
###2.3. MySQL 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML 파일이다.<br>
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 vSphere용 MySQL Deployment 화일인 openpaas-mysql_vsphere.yml를 복사한다.<br>
다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-mysql_vsphere.yml)
>$ls –all<br>
>![mysql_vsphere_2.3.01]

-	Director UUID를 확인한다.<br>
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할 수 있다.
>$bosh status<br>
>![mysql_vsphere_2.3.02]

-	Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3016 버전 사용)  
>$bosh stemcells<br>
>![mysql_vsphere_2.3.03]<br>
>Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3016 버전을 업로드를 해야 한다.

-	openpaas-mysql-vsphere.yml Deployment 파일을 서버 환경에 맞게 수정한다.(굵은 글씨로 표시된 부분 특히 주의)

><pre>$vi openpaas-mysql-vsphere.yml
 # openpaas-mysql-vsphere 설정 파일 내용
name: openpaas-mysql-service              # 서비스 배포이름(필수)
director_uuid: <b>xxxxx#bosh status 에서 확인한 Director UUID을 입력(필수)</b>
>
releases:
- name: openpaas-mysql            #서비스 릴리즈 이름(필수)
  version: beta-1.0           #서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전
>
update:
  canaries: 1                 # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000          # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                  # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000  # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
>
compilation:                        # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:            # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    cpu: 4
    disk: 20480
    ram: 4096
  network: openpaas_network          # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true     # 컴파일지 VM 재사용 여부(옵션)
  workers: 6                     # 컴파일 하는 가상머신의 최대수(필수)
>
jobs:
- instances: 1                    # job 인스턴스 수(필수)
  name: mysql                   #작업 이름(필수): MySQL 서버
  networks:                      # 네트워크 구성정보
  - name: openpaas_network                     # Networks block에서 선언한 network 이름(필수)
    static_ips: <b>10.30.40.162</b>                 # 사용할 IP addresses 정의(필수): MySQL 서버 IP
  persistent_disk: 100000                   # 영구적 디스크 사이즈 정의(옵션): 100G
  properties:                             # job에 대한 속성을 지정(필수)
    admin_password: <b>admin</b>            # MySQL 어드민 패스워드
    cluster_ips:                           # 클러스터 구성시 IPs(필수)
    - <b>10.30.40.162</b># MySQL 서버 IP
    network_name: openpaas_network        # Networks block에서 선언한 network 이름
    seeded_databases: null
    syslog_aggregator: null
  release: openpaas-mysql                  # 서비스 릴리즈 이름(필수)
  resource_pool: services-small       # Resource Pools block에 정의한 resource pool 이름(필수)
  template: mysql                   # job template 이름(필수)
>
- instances: 1
  name: proxy                          # 작업 이름(필수): proxy
  networks:
  - name: openpaas_network
    static_ips: <b>10.30.40.163</b># 사용할 IP addresses 정의(필수): Proxy IP
  properties:
    cluster_ips:
    - <b>10.30.40.162</b># MySQL 서버 IP
    external_host: <b>controller.open-paas.com</b>    # CF 설치시 설정한 외부 호스트 정보(필수)
    nats:                           # CF 설치시 설치한 nats 정보 (필수)
      machines:
      - <b>10.30.40.111</b># nats 서버 IP
      password: <b>admin</b># nats 유저 비밀번호
      port: <b>4222</b># nats 서버 포트번호
      user: <b>nats</b># nats 서버 유저아이디
    network_name: openpaas_network
    proxy:                          # proxy 정보 (필수)
      api_password: <b>admin</b>       # proxy api 유저 비밀번호(필수)
      api_username: <b>api</b>                  # proxy api 유저아이디
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: services-small
  template: proxy                      # job template 이름(필수)
>
- instances: 1
  name: openpaas-mysql-broker                # 작업 이름(필수): 서비스 브로커
  networks:
  - name: openpaas_network
    static_ips: <b>10.30.40.161</b># 사용할 IP addresses 정의(필수): 서비스 브로커 IP
  properties:
    auth_password: <b>admin</b>       # 브로커 접근 아이디 비밀번호(필수)
    auth_username: <b>admin</b>              # 브로커 접근 아이디(필수)
    cc_api_uri: <b>http://api.controller.open-paas.com</b># CF 설치시 설정한 api uri 정보(필수)
    cookie_secret: <b>admin</b> 
 # external_host: <b>10.30.40.161</b> # 사용할 IP addresses 정의(필수): 서비스 브로커 IP
external_host: <b>p-mysql.controller.open-paas.com</b>   # 사용할 외부 호스트 정의(필수): 서비스 브로커
    max_user_connections_default: 40
    mysql_node:
      admin_password: admin
      host: <b>10.30.40.163</b> # proxy IP(필수)
      persistent_disk: 100000           # 영구적 디스크 사이즈 정의(옵션): 100G
    nats:                # CF 설치시 설정하는 Nats 정보 (필수): 위와 동일
      machines:
      - <b>10.30.40.111</b>
      password: <b>admin</b>
      port: <b>4222</b>
      user: <b>nats</b>
    network_name: openpaas_network
    networks:
      broker_network: openpaas_network
    services:
    - dashboard_client:# 서비스 dashboard client 정보
        id: p-mysql                         # 서비스 dashboard 에 접속할 ID
        secret: admin                # 서비스 dashboard 에 접속시 사용할 secret
      description: MySQL databases on demand       # 서비스 설명
      id: 44b26033-1f54-4087-b7bc-da9652c2a539     # 서비스 아이디(고유식별자)
      metadata:
        displayName: MySQL for Pivotal Cloud Foundry   # 화면에서 보여줄 서비스 설명
        documentationUrl: http://docs.controller.open-paas.com/p-mysql/ # 문서제공되는 Url
        imageUrl: data:image/png;base64,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
        longDescription: Creating a service instance provisions a database. Binding
          applications provisions unique credentials for each application to access
          the database.                  # 서비스 상세 설명
        providerDisplayName: OpenPaas Software     # provider 이름
        supportUrl: https://support.pivotal.io  # 서포트 가능한 Url
      name: p-mysql                       # 서비스 이름
      plan_updateable: true                 # plan 업데이트 가능여부
      plans:                               # plan 정보 
      - description: Shared MySQL Server            # plan 설명
        id: ab08f1bc-e6fc-4b56-a767-ee0fea6e3f20     # plan 아이디
        max_storage_mb: 100                        # plan 정보: 100mb 저장공간
        max_user_connections: 20                    # plan 정보: 사용자 접속수
        metadata:
          bullets:                         # 화면에 제공할 plan 정보 
          - Shared MySQL server
          - 100 MB storage
          - 20 concurrent connections
          costs:                    # 과금 정책 설정
          - amount:
              usd: 0               # US 달러: 0달러
            unit: MONTH          # 월별 징수
          displayName: 100 MB
        name: 100mb               # plan 이름
      - description: Shared MySQL Server
        id: 11d0aa36-dcec-4021-85f5-ea4d9a5c8342
        max_storage_mb: 1000
        max_user_connections: 40
        metadata:
          bullets:
          - Shared MySQL server
          - 1000 MB storage
          - 40 concurrent connections
          costs:
          - amount:
              usd: 0
            unit: MONTH
          displayName: 1 GB
        name: 1gb
      tags:
      - mysql                          # 태그 정보
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: services-small         # Resource Pools block에 정의한 resource pool 이름(필수)
  template: cf-mysql-broker          # job template 이름(필수)
>
- instances: 1
  lifecycle: errand               # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar            # 작업 이름: 서비스 브로커 등록 
  networks:
  - name: openpaas_network
  properties:
    broker:# 서비스 브로커 설정 정보
      host: <b>10.30.40.161</b># 서비스 브로커 IP 
      name: p-mysql
      password: admin
      username: admin
      protocol: http
      port: 80
    cf:
      admin_password: <b>admin</b>
      admin_username: <b>admin</b>
      api_url: http://api.controller.open-paas.com
  release: openpaas-mysql
  resource_pool: services-small
  template: broker-registrar
>
- instances: 1
  lifecycle: errand
  name: broker-deregistrar   # 작업 이름: 서비스 브로커 삭제
  networks:
  - name: openpaas_network
  properties:
    broker:
      name: p-mysql
    cf:
      admin_password: <b>admin</b>
      admin_username: <b>admin</b>
      api_url: <b>http://api.controller.open-paas.com</b>
  release: openpaas-mysql
  resource_pool: services-small
  template: broker-deregistrar
>
- instances: 1
  lifecycle: errand
  name: acceptance-tests                 # 작업이름: 서비스팩이 정상적으로 설치 되었는 테스트
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: <b>10.30.40.161</b># 서비스 브로커 IP
    cf:
      admin_password: <b>admin</b>
      admin_username: <b>admin</b>
      api_url: <b>http://api.controller.open-paas.com</b>
      apps_domain: <b>controller.open-paas.com</b>
    proxy:                            # proxy 정보
      api_password: admin
      api_username: api
      external_host: <b>controller.open-paas.com</b>
    service:
      max_user_connections_default: 40
      name: p-mysql
      plans:
      - max_storage_mb: 100
        max_user_connections: 20
        plan_name: 100mb
      - max_storage_mb: 1000
        max_user_connections: 40
        plan_name: 1gb
  release: openpaas-mysql
  resource_pool: services-small
  template: acceptance-tests
>
meta:
  apps_domain: <b>controller.open-paas.com</b># CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: <b>controller.open-paas.com</b># CF 설치시 설정한 외부 도메인 정보
  nats:# CF 설치시 설정한 nats 정보
    machines:
    - <b>10.30.40.111</b>
    password: <b>admin</b>
    port: <b>4222</b>
    user: <b>nats</b>
  syslog_aggregator: null
networks:                   # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: openpaas_network
  subnets:
  - cloud_properties:
      name: Internal#vsphere 에서 사용하는 network 이름(필수)
    dns:                  # DNS 정보
    - <b>10.30.20.24</b>
    - <b>8.8.8.8</b>
    gateway: <b>10.30.20.23</b>
    name: default_unused
    range: <b>10.30.0.0/16</b>
    #reserved:                        # 설치시 제외할 IP 설정
    #- 10.30.0.1 - 10.30.10.254
    #- 10.30.40.1 - 10.30.40.100
    #- 10.30.40.201 - 10.30.254.254
    static:
    - <b>10.30.40.110 - 10.30.40.200</b>#사용 가능한 IP 설정
  type: manual
properties: {}
resource_pools:     # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:        # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    cpu: 2
    disk: 10480
    ram: 4096
  name: services-small             # 고유한 resource pool 이름 
  #size: 4# resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  network: openpaas_network
  stemcell:
    name: <b>bosh-vsphere-esxi-ubuntu-trusty-go_agent</b>#stemcell 이름(필수)
    version: "<b>3016</b>"                                         # stemcell 버전(필수)
></pre>

-	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.
>$bosh deployment {Deployment manifest 파일 PATH}<br>
>$bosh deployment openpaas-mysql-vsphere.yml<br>
>![mysql_vsphere_2.3.04]

-	MySQL 서비스팩을 배포한다.
>$bosh deploy<br>
>![mysql_vsphere_2.3.05]<br>
>![mysql_vsphere_2.3.06]

-	배포된 MySQL 서비스팩을 확인한다.
>$bosh vms<br>
>![mysql_vsphere_2.3.07]

<div id='2.4'>
###2.4. MySQL 서비스 브로커 등록
Mysql 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 MySQL 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	서비스 브로커 목록을 확인한다.
>$cf service-brokers<br>
>![mysql_vsphere_2.4.01]

-	MySQL 서비스 브로커를 등록한다.
>$cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}
>-	서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.  
>-	서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID이다. 
서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
>-	서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.<br><br>
>
>$ cf create-service-broker mysql-service-broker admin password http://p-mysql.controller.open-paas.com<br>
>![mysql_vsphere_2.4.02]

-	등록된 MySQL 서비스 브로커를 확인한다.
>$cf service-brokers<br>
>![mysql_vsphere_2.4.03]

-	접근 가능한 서비스 목록을 확인한다.
>$cf service-access<br>
>![mysql_vsphere_2.4.04]<br>
>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

-	특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)
>$cf enable-service-access p-mysql<br>
>$cf service-access<br>
>![mysql_vsphere_2.4.05]

<div id='3'>
#3. MySQL 연동 Sample Web App 설명
</div>
본 Sample Web App은 개발형 클라우드 플랫폼에 배포되며 MySQL의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

<div id='3.1'>
###3.1. Sample Web App 구조
</div>
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 MySQL 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 MySQL 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.

| 이름 | 설명
| ---- | ------------
| src | Sample 소스 디렉토리
| manifest | 개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일
| pom.xml | 메이븐 project 설정 파일
| target | 메이븐 빌드시 생성되는 디렉토리(war 파일, classes 폴더 등)

-	OpenPaaS-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 MySQL Sample Web App인 hello-spring-mysql를 복사한다.
	
	>$ls -all<br>
	![mysql_vsphere_3.1.01]

<div id='3.2'>
###3.2. 개방형 클라우드 플랫폼에서 서비스 신청
</div>
Sample Web App에서 MySQL 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.  
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.
>$cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
>-	서비스명 : p-mysql로 Marketplace에서 보여지는 서비스 명칭이다.
>-	서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. MySQL 서비스는 100mb, 1gb를 지원한다.
>-	내 서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.<br><br>
>
>$cf create-service p-mysql 100mb mysql-service-instance<br>
>![mysql_vsphere_3.2.02]

-	생성된 MySQL 서비스 인스턴스를 확인한다.
>$cf services<br>
>![mysql_vsphere_3.2.03]

<div id='3.3'>
###3.3. Sample Web App에 서비스 바인드 신청 및 App 확인
</div>
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 MySQL 서비스를 이용한다.<br>
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.
>$cd hello-spring-mysql<br>
>$vi manifest.yml<br>
	<pre>applications:
		- name: hello-tomcat-mysql <b>#배포할 App 이름</b>
		  instances: 1 <b># 배포 인스턴스 수</b>
		  path: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war <b>#배포하는 App 파일 PATH</b></pre>
>참고: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war파일이 존재 하지 않을 경우 mvn 빌드를 수행 하면 파일이 생성된다.

-	--no-start 옵션으로 App을 배포한다.  
	--no-start: App 배포시 구동은 하지 않는다.
>$cf push --no-start<br>
>![mysql_vsphere_3.3.01]

-	배포된 Sample App을 확인하고 로그를 수행한다.
>$cf apps<br>
>![mysql_vsphere_3.3.02]<br>
>$ cf logs {배포된 App명}<br>
>$ cf logs hello-tomcat-mysql<br>
>![mysql_vsphere_3.3.03]

-	Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다.
>$cf bind-service hello-tomcat-mysql mysql-service-instance<br>
>![mysql_vsphere_3.3.04]

-	바인드가 적용되기 위해서 App을 재기동한다.
>$cf restart hello-tomcat-mysql<br>
>![mysql_vsphere_3.3.05]

-	(참고) 바인드 후 App구동시 Mysql 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.<br>
>-	rule.json 화일을 만들고 아래와 같이 내용을 넣는다.<br>
>$ vi rule.json
><pre>
		[
		   {
		      "protocol": "tcp",
		      "destination": "10.244.7.6",
		      "ports": "3306"
		   }
		]
></pre>
>
>-	보안 그룹을 생성한다.
>$cf create-security-group p-mysql rule.json<br>
>![mysql_vsphere_3.3.06]
>
>-	모든 App에 Mysql 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.
>$cf bind-running-security-group p-mysql<br>
>![mysql_vsphere_3.3.07]
>
>-	App을 리부팅 한다.
>$cf restarthello-tomcat-mysql<br>
>![mysql_vsphere_3.3.08]

-	App이 정상적으로 MySQL 서비스를 사용하는지 확인한다.
>-	curl 로 확인<br>
>$curl hello-tomcat-mysql.controller.open-paas.com<br>
>![mysql_vsphere_3.3.09]

-	브라우져에서 확인
>![mysql_vsphere_3.3.10]

<div id='4'>
#4. MySQL Client 툴 접속
</div>
Application에 바인딩된 MySQL 서비스 연결정보는 Private IP로 구성되어 있기 때문에 MySQL Client 툴에서 직접 연결할수 없다. 따라서 MySQL Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 MySQL Client 툴로써는 오픈 소스인 HeidiSQL로 가이드한다. HeidiSQL 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 vSphere관리자 및 OpenPaaS 운영자에게 문의하여 구성한다.

<div id='4.1'>
###4.1. HeidiSQL 설치 및 연결
</div>
HeidiSQL 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

-	HeidiSQL을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.<br>
[http://www.heidisql.com/download.php](http://www.heidisql.com/download.php)<br>
![mysql_vsphere_4.1.01]<br><br>

-	다운로드한 설치파일을 실행한다.
![mysql_vsphere_4.1.02]<br><br>

-	HeidSQL 설치를 위한 안내사항이다. Next 버튼을 클릭한다.
![mysql_vsphere_4.1.03]<br><br>

-	프로그램 라이선스에 관련된 내용이다. 동의(I accept the agreement)에 체크 후 Next 버튼을 클릭한다.
![mysql_vsphere_4.1.04]<br><br>

-	HeidiSQL을 설치할 경로를 설정 후 Next 버튼을 클릭한다.  
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.
![mysql_vsphere_4.1.05]<br><br>

-	설치 완료 후 시작메뉴에 HeidiSQL 바로가기 아이콘의 이름을 설정하는 과정이다.  
Next 버튼을 클릭하여 다음 과정을 진행한다.  
![mysql_vsphere_4.1.06]<br><br>

-	체크박스가 4개가 있습니다. 아래의 경우를 고려하여 체크 및 해제를 한다.
	
	>바탕화면에 바로가기 아이콘을 생성할 경우  
	>sql확장자를 HeidiSQL 프로그램으로 실행할 경우  
	>heidisql 공식 홈페이지를 통해 자동으로 update check를 할 경우  
	>heidisql 공식 홈페이지로 자동으로 버전을 전송할 경우

	체크박스에 체크 설정/해제를 완료했다면 Next 버튼을 클릭한다.
![mysql_vsphere_4.1.07]<br><br>

-	설치를 위한 모든 설정이 한번에 출력된다.확인 후 Install 버튼을 클릭하여 설치를 진행한다.
![mysql_vsphere_4.1.08]<br><br>

-	Finish 버튼 클릭으로 설치를 완료한다.  
![mysql_vsphere_4.1.09]<br><br>

-	HeidiSQL을 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. 신규 버튼을 클릭한다.  
![mysql_vsphere_4.1.10]<br><br>

-	어떤 Server에 접속하기 위한 Connection 정보인지 별칭을 입력한다.  
![mysql_vsphere_4.1.11]<br><br>

-	네트워크 유형의 목록에서 MySQL(SSH tunel)을 선택한다.  
![mysql_vsphere_4.1.12]<br><br>

-	아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.  
![mysql_vsphere_4.1.13]<br>
서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.<br>
<b>예)</b> $cf env hello-tomcat-song
![mysql_vsphere_4.1.14]<br><br>

-	SSH 터널 탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력한다. plink.exe 위치 입력은 Putty에서 제공하는 plink.exe 실행 위치를 넣어주고 만일 해당 파일이 없을 경우 plink.exe 내려받기 링크를 클릭하여 다운받는다. 로컬 포트 정보는 임의로 넣고 열기 버튼을 클릭하면 Mysql 데이터베이스에 접속한다.<br>
(참고) 만일 개인 키로 접속이 가능한 경우에는 openstack용 Open PaaS Mysql 서비스팩 설치 가이드를 참고한다.<br>
![mysql_vsphere_4.1.15]<br><br>

-	접속이 완료되면 좌측에 스키마 정보가 나타난다.하지만 초기설정은 테이블, 뷰, 프로시져, 함수, 트리거, 이벤트 등 모두 섞여 있어서 한눈에 구분하기가 힘들어서 접속한 DB 별칭에 마우스 오른쪽 클릭 후 "트리 방식 옵션" - "객체를 유형별로 묶기"를 클릭하면 아래 화면과 같이 각 유형별로 구분이된다.  
![mysql_vsphere_4.1.16]<br><br>

-	우측 화면에 쿼리 탭을 클릭하여 Query문을 작성한 후 실행 버튼(삼각형)을 클릭한다.  
쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.  
![mysql_vsphere_4.1.17]<br><br>


[mysql_vsphere_1.3.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_1.3.01.png
[mysql_vsphere_2.2.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.01.png
[mysql_vsphere_2.2.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.02.png
[mysql_vsphere_2.2.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.03.png
[mysql_vsphere_2.2.04]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.04.png
[mysql_vsphere_2.2.05]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.05.png
[mysql_vsphere_2.2.06]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.06.png
[mysql_vsphere_2.2.07]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.07.png
[mysql_vsphere_2.2.08]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.2.08.png
[mysql_vsphere_2.3.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.01.png
[mysql_vsphere_2.3.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.02.png
[mysql_vsphere_2.3.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.03.png
[mysql_vsphere_2.3.04]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.04.png
[mysql_vsphere_2.3.05]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.05.png
[mysql_vsphere_2.3.06]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.06.png
[mysql_vsphere_2.3.07]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.3.07.png

[mysql_vsphere_2.4.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.4.01.png
[mysql_vsphere_2.4.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.4.02.png
[mysql_vsphere_2.4.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.4.03.png
[mysql_vsphere_2.4.04]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.4.04.png
[mysql_vsphere_2.4.05]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_2.4.05.png
[mysql_vsphere_3.1.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.1.01.png
[mysql_vsphere_3.2.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.2.01.png
[mysql_vsphere_3.2.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.2.02.png
[mysql_vsphere_3.2.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.2.03.png
[mysql_vsphere_3.3.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.01.png
[mysql_vsphere_3.3.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.02.png
[mysql_vsphere_3.3.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.03.png
[mysql_vsphere_3.3.04]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.04.png
[mysql_vsphere_3.3.05]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.05.png
[mysql_vsphere_3.3.06]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.06.png
[mysql_vsphere_3.3.07]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.07.png
[mysql_vsphere_3.3.08]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.08.png
[mysql_vsphere_3.3.09]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.09.png
[mysql_vsphere_3.3.10]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_3.3.10.png
[mysql_vsphere_4.1.01]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.01.png
[mysql_vsphere_4.1.02]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.02.png
[mysql_vsphere_4.1.03]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.03.png
[mysql_vsphere_4.1.04]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.04.png
[mysql_vsphere_4.1.05]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.05.png
[mysql_vsphere_4.1.06]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.06.png
[mysql_vsphere_4.1.07]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.07.png
[mysql_vsphere_4.1.08]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.08.png
[mysql_vsphere_4.1.09]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.09.png
[mysql_vsphere_4.1.10]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.10.png
[mysql_vsphere_4.1.11]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.11.png
[mysql_vsphere_4.1.12]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.12.png
[mysql_vsphere_4.1.13]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.13.png
[mysql_vsphere_4.1.14]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.14.png
[mysql_vsphere_4.1.15]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.15.png
[mysql_vsphere_4.1.16]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.16.png
[mysql_vsphere_4.1.17]:/images/openpaas-service/mysql/mysql_vsphere/mysql_vsphere_4.1.17.png
