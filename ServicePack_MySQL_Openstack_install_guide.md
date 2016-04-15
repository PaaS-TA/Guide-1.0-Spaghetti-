## Table of Contents
1. [문서 개요](#1-문서-개요)
	-	1.1. [목적](#11-목적)
	-	1.2. [범위](#12-범위)
	-	1.3. [시스템 구성도](#13-시스템-구성도)
	-	1.4. [참고자료](#14-참고자료)
2. [MySQL 서비스팩 설치](#2-mysql-서비스팩-설치)
	-	2.1. [설치전 준비사항](#21-설치전-준비사항)
	-	2.2. [MySQL 서비스 릴리즈 업로드](#22-mysql-서비스-릴리즈-업로드)
	-	2.3. [MySQL 서비스 Deployment 파일 수정 및 배포](#23-mysql-서비스-deployment-파일-수정-및-배포)
	-	2.4. [MySQL 서비스 브로커 등록](#24-mysql-서비스-브로커-등록)
3. [MySQL 연동 Sample Web App 설명](#3-mysql-연동-sample-web-app-설명)
	-	3.1. [Sample Web App 구조](#31-sample-web-app-구조)
	-	3.2. [개방형 클라우드 플랫폼에서 서비스 신청](#32-개방형-클라우드-플랫폼에서-서비스-신청)
	-	3.3. [Sample Web App에 서비스 바인드 신청 및 App 확인](#33-sample-web-app에-서비스-바인드-신청-및-app-확인)
4. [MySQL Client 툴 접속](#4-mysql-client-툴-접속)
	-	4.1. [HeidiSQL 설치 및 연결](#41-heidisql-설치-및-연결)

# 1. 문서 개요
### 1.1. 목적

본 문서(MySQL 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 MySQL 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 MySQL 서비스를 사용하는 방법을 기술하였다.

### 1.2. 범위
설치 범위는 MySQL 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

### 1.3. 시스템 구성도
본 문서의 설치된 시스템 구성도이다. MySQL Server, MySQL 서비스 브로커, Proxy로 최소사항을 구성하였다.

![시스템구성도][mysql_openstack_1.3.01]

| 구분 | Resource Pool | 스펙 |
|--------|-------|-------|
| openpaas-mysql-broker | services-small | m1.small(1vCPU / 2GB RAM / 20GB Disk) |
| proxy | services-small |  m1.small(1vCPU / 2GB RAM / 20GB Disk) |
| mysql_z1 | services-small |  m1.small(1vCPU / 2GB RAM / 20GB Disk + 8GB 영구적 Disk) |

### 1.4. 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

#2. MySQL 서비스팩 설치

###2.1. 설치전 준비사항

본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다.

OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Services.zip, OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip)


###2.2. MySQL 서비스 릴리즈 업로드

##### OpenPaaS-Services.zip 파일 압축을 풀고 폴더안에 있는 MySQL 서비스 릴리즈 openpaas-mysql-1.0.tgz 파일을 확인한다.

>`$ cd openpaas-service-release`  
>`$ ls –all`  

>![mysql_openstack_2.2.01]

<br>

##### MySQL 서비스 릴리즈 파일을 업로드한다.

>`$ bosh upload release {서비스 릴리즈 파일 PATH}`  
>`$ bosh upload release openpaas-mysql-1.0.tgz`

※ 하단의 화면은 릴리즈 파일을 tarball 형태로 압축하지 않고 릴리즈를 업로드하고 있다. 본 문서에서 안내하는 방법대로 tarball 형태로 릴리즈 파일 압축하여 업로드 할 경우에 출력되는 화면은 하단의 화면과 다소 차이가 있다.

>![mysql_openstack_2.2.03]

>![mysql_openstack_2.2.04]

>![mysql_openstack_2.2.05]

>![mysql_openstack_2.2.06]

>![mysql_openstack_2.2.07]

<br>

##### 업로드된 MySQL 릴리즈를 확인한다.

>`$ bosh releases`

>![mysql_openstack_2.2.08]

>Mysql 서비스 릴리즈가 업로드 되어 있는 것을 확인

<br>

### 2.3. MySQL 서비스 Deployment 파일 수정 및 배포

BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML 파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

##### OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 OpenStack용 MySQL Deployment 화일인 openpaas-mysql-openstack-1.0.yml 를 복사한다.

<br>

##### 다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-mysql-openstack-1.0.yml)

>`$ ls –all`

>![mysql_openstack_2.3.01]

<br>

##### Director UUID를 확인한다.

>BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할 수 있다.

>`$ bosh status`

>![mysql_openstack_2.3.02]

<br>

##### Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3147 버전 사용)

>`$ bosh stemcells`

>![mysql_openstack_2.3.03]

>Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3147 버전을 업로드를 해야 한다.

<br>

##### -	openpaas-mysql-openstack-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다.

>`$ vi openpaas-mysql-openstack.yml`

```yml
# openpaas-mysql-openstack-1.0 설정 파일 내용
name: openpaas-mysql-service      # 서비스 배포이름(필수)
director_uuid: xxxxx       #bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- name: openpaas-mysql       #서비스 릴리즈 이름(필수)
  version: beta-1.0       #서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1       # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000      # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1      # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000     # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

compilation:     # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:     # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)
    instance_type: m1.medium    # 인스턴스 타입: Flavors 타입 (필수)
  network: openpaas_network      # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true     # 컴파일지 VM 재사용 여부(옵션)
  workers: 4   # 컴파일 하는 가상머신의 최대수(필수)

jobs:
- instances: 1     # job 인스턴스 수(필수)
  name: mysql     # 작업 이름(필수): MySQL 서버
  networks:      # 네트워크 구성정보
  - name: openpaas_network     # Networks block에서 선언한 network 이름(필수)
    static_ips: 
    - 10.20.0.90     # 사용할 IP addresses 정의(필수): MySQL 서버 IP
persistent_disk: 10000     # 영구적 디스크 사이즈 정의(옵션): 10G
  properties:      # job에 대한 속성을 지정(필수)
    admin_password: admin     # MySQL 어드민 패스워드
    cluster_ips:     # 클러스터 구성시 IPs(필수)
    - 10.20.0.90      # MySQL 서버 IP
    - 10.20.0.91
    - 10.20.0.92 
    network_name: openpaas_network     # Networks block에서 선언한 network 이름
    seeded_databases: null
    syslog_aggregator: null
    collation_server: utf8_unicode_ci     # Mysql CharSet
    character_set_server: utf8      # Mysql CharSet
  release: openpaas-mysql     # 서비스 릴리즈 이름(필수)
  resource_pool: services-small     # Resource Pools block에 정의한 resource pool 이름(필수)
  template: mysql      # job template 이름(필수)

- instances: 1
  name: mysql_z2
  networks:
  - name: openpass_network
    static_ips: 10.20.0.91
  persistent_disk: 10000
  properties:
    admin_password: admin
    cluster_ips:
    - 10.20.0.90
    - 10.20.0.91
    - 10.20.0.92
    network_name: openpass_network
    seeded_databases: null
    syslog_aggregator: null
    collation_server: utf8_unicode_ci
    character_set_server: utf8
  release: openpaas-mysql
  resource_pool: services-small
  template: mysql

- instances: 1
  name: mysql_z3
  networks:
  - name: openpass_network
    static_ips: 10.20.0.92
  persistent_disk: 10000
  properties:
    admin_password: admin
    cluster_ips:
    - 10.20.0.90
    - 10.20.0.91
    - 10.20.0.92
    network_name: openpass_network
    seeded_databases: null
    syslog_aggregator: null
    collation_server: utf8_unicode_ci
    character_set_server: utf8
  release: openpaas-mysql
  resource_pool: services-small
template: mysql

- instances: 1
  name: proxy      # 작업 이름(필수): proxy
  networks:
  - name: openpaas_network
    static_ips: 10.20.0.93     # 사용할 IP addresses 정의(필수): Proxy IP
  properties:
    cluster_ips:
    - 10.20.0.90
    - 10.20.0.91
    - 10.20.0.92
    external_host: 115.68.151.190.xip.io      # CF 설치시 설정한 외부 호스트 정보(필수)
    nats:      # CF 설치시 설치한 nats 정보 (필수)
      machines:
      - 10.10.3.11     # nats 서버 IP
      password: admin     # nats 유저 비밀번호
      port: 4222     # nats 서버 포트번호
      user: nats     # nats 서버 유저아이디
    network_name: openpaas_network
    proxy:     # proxy 정보 (필수)
      api_password: admin     # proxy api 유저 비밀번호(필수)
      api_username: api     # proxy api 유저아이디
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: services-small
  template: proxy     # job template 이름(필수)

- instances: 1
  name: openpaas-mysql-java-broker     # 작업 이름(필수): 서비스 브로커
  networks:
  - name: openpaas_network
    static_ips: 
    - 10.20.0.94     # 사용할 IP addresses 정의(필수): 서비스 브로커 IP 
  properties:
    jdbc_ip: 10.20.0.93     # Mysql Url
    jdbc_pwd: admin     # Mysql password
    jdbc_port: 3306      # Mysql port
    log_dir: openpaas-mysql-java-broker
    log_file: openpaas-mysql-java-broker
    log_level: INFO 
    release: openpaas-mysql
  resource_pool: services-small      # Resource Pools block에 정의한 resource pool 이름(필수)
  template: op-mysql-java-broker      # job template 이름(필수)

- instances: 1
  lifecycle: errand                  # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar             # 작업 이름: 서비스 브로커 등록
  networks:
  - name: openpaas_network
  properties:
    broker:                 # 서비스 브로커 설정 정보
      host: 10.30.40.195      # 서비스 브로커 IP 
      name: mysql-service      # 서비스 명
      password: cloudfoundry     # 서비스 브로커 인증 패스워드
      username: admin      # 서비스 브러커 인증 아이디
      protocol: http      # 서비스 브로커 프로토콜
      port: 8080       # 서비스 프로커 서비스 포트
    cf:
      admin_password: admin      # CF 사용자 암호
      admin_username: admin      # CF 사용자 아이디
      api_url: https://api.115.68.151.190.xip.io      # CF 주소
      skip_ssl_validation: true      # CF SSL 접속 여부
  release: openpaas-mysql
  resource_pool: services-errand
  template: broker-registrar

- instances: 1
  lifecycle: errand
  name: broker-deregistrar                    # 작업 이름: 서비스 브로커 삭제
  networks:
  - name: openpaas_network
  properties:
 broker:
      name: mysql-service
    cf:
      admin_password: admin
      admin_username: admin
      api_url: https://api.115.68.151.190.xip.io
      skip_ssl_validation: true
 release: openpaas-mysql
  resource_pool: services-errand
  template: broker-deregistrar

meta:
  apps_domain: 115.68.151.190.xip.io               # CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: 115.68.151.190.xip.io               # CF 설치시 설정한 외부 도메인 정보
  nats:                                    # CF 설치시 설정한 nats 정보
    machines:
    - 10.10.3.11
    password: admin
    port: 4222
    user: nats
  syslog_aggregator: null

networks:                       # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: openpaas_network
  subnets:
  - cloud_properties:
      net_id: 7b49e746-161a-4f90-9ed6-c93e27122a1a       # OpenStack에서 사용하는 network 이름 아이디(필수)
      security_groups:      # OpenStack 에서 사용하는 접근 시큐리티 이름 이름(필수)
      - cf-security
      - bosh
      - default 
    dns:   # DNS 정보
    - 10.20.0.3
    - 8.8.8.8
    gateway: 10.20.0.1
range: 10.20.0.0/24
    reserved:      # 설치시 제외할 IP 설정 (OpenStack 에서는 서비스팩 설치 구간 이외에는 IP 제외 설정을 해줘야 오류가 나지 않음. 
    - 10.20.0.2 - 10.20.0.89
    static:
    - 10.20.0.90 - 10.20.0.94       # 사용 가능한 IP 설정
type: manual

properties: {}

resource_pools:      # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:      # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone)
    instance_type: m1. small       #인스턴스 타입: Flovers 타입(필수)
  name: services-small      # 고유한 resource pool 이름
  network: openpaas_network
 # size: 3       # resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent      # stemcell 이름(필수)
    version: "3147"      # stemcell 버전(필수)
```
<br>

##### Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

>`$ bosh deployment {Deployment manifest 파일 PATH}`
>`$ bosh deployment openpaas-mysql-openstack-1.0.yml`

>![mysql_openstack_2.3.04]

<br>

##### MySQL 서비스팩을 배포한다.

>`$ bosh deploy`

>![mysql_openstack_2.3.05]

>![mysql_openstack_2.3.06]

>![mysql_openstack_2.3.07]

<br>

##### 배포된 MySQL 서비스팩을 확인한다.

>`$ bosh vms openpaas-mysql-service`

>![mysql_openstack_2.3.08]

### 2.4. MySQL 서비스 브로커 등록
Mysql 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 MySQL 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어 있어야 한다.

##### 서비스 브로커 목록을 확인한다.

>`$ cf service-brokers`

>![mysql_openstack_2.4.01]

<br>

##### MySQL 서비스 브로커를 등록한다.

>`$ cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL(IP)}`

	서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
	서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
	서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.


>`$cf create-service-broker mysql-service-broker admin cloudfoundry http://10.0.0.95:8080`

>![mysql_openstack_2.4.02]

<br>

##### 등록된 MySQL 서비스 브로커를 확인한다.

>`$ cf service-brokers`

>![mysql_openstack_2.4.03]

<br>

##### 접근 가능한 서비스 목록을 확인한다.

>`$ cf service-access`

>![mysql_openstack_2.4.04]

>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

<br>

##### 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

>`$ cf enable-service-access Mysql-DB`

>`$ cf service-access`

>![mysql_openstack_2.4.05]

# 3. MySQL 연동 Sample Web App 설명
본 Sample Web App은 개방형 클라우드 플랫폼에 배포되며 MySQL의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.

### 3.1. Sample Web App 구조

Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 MySQL 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 MySQL 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.

| 이름 | 설명
| ---- | ------------
| src | Sample 소스 디렉토리
| manifest | 개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일
| pom.xml | 메이븐 project 설정 파일
| target | 메이븐 빌드시 생성되는 디렉토리(war 파일, classes 폴더 등)

##### OpenPaaS-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 MySQL Sample Web App인 hello-spring-mysql를 복사한다.

>`$ls -all`

>![mysql_openstack_3.1.01]

<br>

### 3.2. 개방형 클라우드 플랫폼에서 서비스 신청
Sample Web App에서 MySQL 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.

*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

<br>

##### 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

>`$cf marketplace`

>![mysql_openstack_3.2.01]

<br>

##### Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>`$ cf create-service {서비스명} {서비스플랜} {내서비스명}`

>서비스명 : p-mysql로 Marketplace에서 보여지는 서비스 명칭이다.
>서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. MySQL 서비스는 10 connection, 100 connection 를 지원한다.
>내 서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.

>`$ cf create-service 'Mysql-DB' Mysql-Plan2-100con mysql-service-instance`

>![mysql_openstack_3.2.02]

<br>

##### 생성된 MySQL 서비스 인스턴스를 확인한다.

>`$ cf services`

>![mysql_openstack_3.2.03]

<br>

### 3.3. Sample Web App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 MySQL 서비스를 이용한다. 
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

<br>

##### Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

>`$ cd hello-spring-mysql`

>`$ vi manifest.yml`

```yml
---
applications:
- name: hello-spring-mysql    # 배포할 App 이름
  memory: 512M                # 배포시 메모리 사이즈
  instances: 1                # 배포 인스턴스 수
  path: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war    #배포하는 App 파일 PATH
```

>참고: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war파일이 존재 하지 않을 경우 mvn 빌드를 수행 하면 파일이 생성된다.

<br>

##### --no-start 옵션으로 App을 배포한다.  

>--no-start: App 배포시 구동은 하지 않는다.

>`$ cf push --no-start`

>![mysql_openstack_3.3.01]

<br>

##### 배포된 Sample App을 확인하고 로그를 수행한다.
>`$ cf apps`

>![mysql_openstack_3.3.02]

>`$ cf logs {배포된 App명}`

>`$ cf logs hello-spring-mysql`

>![mysql_openstack_3.3.03]

<br>

##### Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

>`$ cf bind-service hello-spring-mysql mysql-service-instance`

>![mysql_openstack_3.3.04]

<br>

##### 바인드가 적용되기 위해서 App을 재기동한다.

>`$ cf restart hello-spring-mysql`

>![mysql_openstack_3.3.05]

>(참고) 바인드 후 App구동시 Mysql 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.  

<br>

##### rule.json 화일을 만들고 아래와 같이 내용을 넣는다.
>`$ vi rule.json`

```json
[
  {
    "protocol": "tcp",
    "destination": "10.0.0.63",
    "ports": "3306"
  }
]
```
<br>

##### 보안 그룹을 생성한다.

>`$ cf create-security-group p-mysql rule.json`

>![mysql_openstack_3.3.06]

<br>

##### 모든 App에 Mysql 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.

>`$ cf bind-running-security-group p-mysql`

>![mysql_openstack_3.3.07]

<br>

##### App을 리부팅 한다.

>`$ cf restart hello-spring-mysql`

>![mysql_openstack_3.3.08]

<br>

##### App이 정상적으로 MySQL 서비스를 사용하는지 확인한다.

>curl 로 확인

>`$ curl hello-spring-mysql.52.71.64.39.xip.io`

>![mysql_openstack_3.3.09]


> 브라우져에서 확인

??

# 4. MySQL Client 툴 접속

Application에 바인딩된 MySQL 서비스 연결정보는 Private IP로 구성되어 있기 때문에 MySQL Client 툴에서 직접 연결할수 없다. 따라서 MySQL Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 MySQL Client 툴로써는 오픈 소스인 HeidiSQL로 가이드한다. HeidiSQL 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 OpenStack 관리자 및 OpenPaaS 운영자에게 문의하여 구성한다. OpenStack 에서 구성한 인스턴스는 개인키(.ppk) 로 접속을 해야 하므로 개인키는 운영 담당자에게 문의하여 제공받는다.

### 4.1. HeidiSQL 설치 및 연결

HeidiSQL 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

##### HeidiSQL을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.

>[http://www.heidisql.com/download.php](http://www.heidisql.com/download.php)

>![mysql_openstack_4.1.01]

<br>

##### 다운로드한 설치파일을 실행한다.

>![mysql_openstack_4.1.02]

<br>

##### HeidSQL 설치를 위한 안내사항이다. Next 버튼을 클릭한다.

>![mysql_openstack_4.1.03]

<br>

##### 프로그램 라이선스에 관련된 내용이다. 동의(I accept the agreement)에 체크 후 Next 버튼을 클릭한다.

>![mysql_openstack_4.1.04]

<br>

##### HeidiSQL을 설치할 경로를 설정 후 Next 버튼을 클릭한다.

>별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.

>![mysql_openstack_4.1.05]

<br>

##### 설치 완료 후 시작메뉴에 HeidiSQL 바로가기 아이콘의 이름을 설정하는 과정이다.  
>Next 버튼을 클릭하여 다음 과정을 진행한다.

>![mysql_openstack_4.1.06]

<br>

##### 체크박스가 4개가 있다. 아래의 경우를 고려하여 체크 및 해제를 한다.
>	
	바탕화면에 바로가기 아이콘을 생성할 경우  
	sql확장자를 HeidiSQL 프로그램으로 실행할 경우  
	heidisql 공식 홈페이지를 통해 자동으로 update check를 할 경우  
	heidisql 공식 홈페이지로 자동으로 버전을 전송할 경우

> 체크박스에 체크 설정/해제를 완료했다면 Next 버튼을 클릭한다.

>![mysql_openstack_4.1.07]

<br>

##### 설치를 위한 모든 설정이 한번에 출력된다.확인 후 Install 버튼을 클릭하여 설치를 진행한다.

>![mysql_openstack_4.1.08]

<br>

##### Finish 버튼 클릭으로 설치를 완료한다.

>![mysql_openstack_4.1.09]

<br>

##### HeidiSQL을 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. 신규 버튼을 클릭한다.

>![mysql_openstack_4.1.10]

<br>

##### 어떤 Server에 접속하기 위한 Connection 정보인지 별칭을 입력한다.

>![mysql_openstack_4.1.11]

<br>

##### 네트워크 유형의 목록에서 MySQL(SSH tunel)을 선택한다.

>![mysql_openstack_4.1.12]

<br>

##### 아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.

>![mysql_openstack_4.1.13]

>서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.

>**예)** $cf env hello-tomcat-mysql

>![mysql_openstack_4.1.14]

<br>

##### SSH 터널 탭을 클릭하고 Vagrant VM 정보를 입력하고 개인 키 파일을 불러온다. 개인키는 AWS에서 인스턴스 접속을 위한 공개키(.pem 파일)를puttygen을 이용하여 개인키(.ppk)로 변환한다. plink.exe 위치 입력은 Putty에서 제공하는 plink.exe 실행 위치를 넣어주고 만일 해당 파일이 없을 경우 plink.exe 내려받기 링크를 클릭하여 다운받는다. 로컬 포트 정보는 임의로 넣고 열기 버튼을 클릭하면 Mysql 데이터베이스에 접속한다.

>(참고) 만일 개인 키 없이 ID/Password로 접속이 가능한 경우에는 Openstack 용 Open PaaS Mysql 서비스팩 설치 가이드를 참고한다.

>![mysql_openstack_4.1.15]

<br>

##### 접속이 완료되면 좌측에 스키마 정보가 나타난다.하지만 초기설정은 테이블, 뷰, 프로시져, 함수, 트리거, 이벤트 등 모두 섞여 있어서 한눈에 구분하기가 힘들어서 접속한 DB 별칭에 마우스 오른쪽 클릭 후 "트리 방식 옵션" - "객체를 유형별로 묶기"를 클릭하면 아래 화면과 같이 각 유형별로 구분이된다.

>![mysql_openstack_4.1.16]

<br>

##### 우측 화면에 쿼리 탭을 클릭하여 Query문을 작성한 후 실행 버튼(삼각형)을 클릭한다.  

>쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.

>![mysql_openstack_4.1.17]


[mysql_openstack_1.3.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_1.3.01.png
[mysql_openstack_2.2.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.01.png
[mysql_openstack_2.2.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.02.png
[mysql_openstack_2.2.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.03.png
[mysql_openstack_2.2.04]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.04.png
[mysql_openstack_2.2.05]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.05.png
[mysql_openstack_2.2.06]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.06.png
[mysql_openstack_2.2.07]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.07.png
[mysql_openstack_2.2.08]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.2.08.png
[mysql_openstack_2.3.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.01.png
[mysql_openstack_2.3.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.02.png
[mysql_openstack_2.3.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.03.png
[mysql_openstack_2.3.04]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.04.png
[mysql_openstack_2.3.05]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.05.png
[mysql_openstack_2.3.06]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.06.png
[mysql_openstack_2.3.07]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.07.png
[mysql_openstack_2.3.08]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.08.png
[mysql_openstack_2.3.09]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.3.09.png
[mysql_openstack_2.4.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.4.01.png
[mysql_openstack_2.4.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.4.02.png
[mysql_openstack_2.4.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.4.03.png
[mysql_openstack_2.4.04]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.4.04.png
[mysql_openstack_2.4.05]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_2.4.05.png
[mysql_openstack_3.1.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.1.01.png
[mysql_openstack_3.2.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.2.01.png
[mysql_openstack_3.2.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.2.02.png
[mysql_openstack_3.2.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.2.03.png
[mysql_openstack_3.3.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.01.png
[mysql_openstack_3.3.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.02.png
[mysql_openstack_3.3.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.03.png
[mysql_openstack_3.3.04]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.04.png
[mysql_openstack_3.3.05]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.05.png
[mysql_openstack_3.3.06]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.06.png
[mysql_openstack_3.3.07]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.07.png
[mysql_openstack_3.3.08]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.08.png
[mysql_openstack_3.3.09]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_3.3.09.png
[mysql_openstack_4.1.01]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.01.png
[mysql_openstack_4.1.02]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.02.png
[mysql_openstack_4.1.03]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.03.png
[mysql_openstack_4.1.04]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.04.png
[mysql_openstack_4.1.05]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.05.png
[mysql_openstack_4.1.06]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.06.png
[mysql_openstack_4.1.07]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.07.png
[mysql_openstack_4.1.08]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.08.png
[mysql_openstack_4.1.09]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.09.png
[mysql_openstack_4.1.10]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.10.png
[mysql_openstack_4.1.11]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.11.png
[mysql_openstack_4.1.12]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.12.png
[mysql_openstack_4.1.13]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.13.png
[mysql_openstack_4.1.14]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.14.png
[mysql_openstack_4.1.15]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.15.png
[mysql_openstack_4.1.16]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.16.png
[mysql_openstack_4.1.17]:/images/openpaas-service/mysql/mysql_openstack/mysql_openstack_4.1.17.png
