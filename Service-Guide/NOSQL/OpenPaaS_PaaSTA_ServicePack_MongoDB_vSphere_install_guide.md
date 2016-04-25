
## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 시스템 구성도](#4)
     * [1.4. 참고자료](#5)
2. [Mongodb 서비스팩 설치](#6)
     * [2.1. 설치전 준비사항](#7)
     * [2.2. Mongodb 서비스 릴리즈 업로드](#8)
     * [2.3. Mongodb 서비스 Deployment 파일 수정 및 배포](#9)
     * [2.4. Mongodb 서비스 브로커 등록](#10)
3. [Mongodb 연동 Sample App 설명](#11)
     * [3.1. Sample App 구조](#12)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#13)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#14)
4. [Mongodb Client 툴 접속](#15)
     * [4.1. MongoChef 설치 및 연결](#16)

<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
      
본 문서(Mongodb 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 Mongodb 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 Mongodb 서비스를 사용하는 방법을 기술하였다.

<div id='3'></div>
### 1.2. 범위 

설치 범위는 Mongodb 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.  

<div id='4'></div>
### 1.3. 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. Mongodb Server, Mongodb 서비스 브로커로 최소사항을 구성하였다.  
![시스템 구성도][1-3-0-0]

<table>
  <tr>
    <td>구분</td>
    <td>스펙</td>
  </tr>
  <tr>
    <td>openpaas-mongodb-broker</td>
    <td>2vCPU / 4GB RAM / 8GB Disk</td>
  </tr>
  <tr>
    <td>Mongos</td>
    <td>2vCPU / 4GB RAM / 8GB Disk</td>
  </tr>
  <tr>
    <td>Mongo Config</td>
    <td>2vCPU / 4GB RAM / 8GB Disk+16GB(영구적 Disk)</td>
  </tr>
  <tr>
    <td>Mongod</td>
    <td>2vCPU / 4GB RAM / 8GB Disk+16GB(영구적 Disk)</td>
  </tr>
</table>

<div id='5'></div>
### 1.4. 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)

<div id='6'></div>
#   2. Mongodb 서비스팩 설치

<div id='7'></div>
### 2.1. 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.  
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.  
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.  

- OpenPaaS 에서 제공하는 릴리즈 파일들을 다운받는다. (OpenPaaS-Services, OpenPaaS-Deployment, OpenPaaS-Sample-Apps)

- 다운로드 위치

>OpenPaaS-Services : **<http://extdisk.hancom.com:8080/share.cgi?ssid=0IgH8sM>**  
>OpenPaaS-Deployment : **<http://extdisk.hancom.com:8080/share.cgi?ssid=0YWXQzq>**  
>OpenPaaS-Sample-Apps : **<http://extdisk.hancom.com:8080/share.cgi?ssid=0icB5ZW>**

<div id='8'></div>
###   2.2. Mongodb 서비스 릴리즈 업로드

- OpenPaaS-Services을 다운로드 받고 폴더안에 있는 Mongodb 서비스 릴리즈 openpaas-mongodb-shard-1.0.tgz 파일을 확인한다.

>cd OpenPaaS-Services  
>$ ls -all  
![2-2-0-0-1]  

- 업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$ bosh releases</div>  
![2-2-4-0-1]  
><div>Mongodb 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</div>  

- Mongodb 서비스 릴리즈를 업로드한다.

>$ bosh upload release {서비스 릴리즈 파일 PATH}  
>$ bosh upload release openpaas-mongodb-shard-1.0.tgz  
![2-2-6-0-1]  
![2-2-6-1-1]  

- 업로드 된 Mongodb 릴리즈를 확인한다.

>$ bosh releases  
![2-2-7-0-1]  
>Mongodb 서비스 릴리즈가 업로드 되어 있는 것을 확인  

<div id='9'></div>
###   2.3. Mongodb 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.


- OpenPaaS-Deployment을 다운로드 받고 폴더안에 있는vSphere용 Mongodb Deployment 화일인 openpaas-mongodb-shard-vsphere-1.0.yml 를 복사한다.
- 다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-mongodb-shard-vsphere-1.0.yml)

><div>$ ls –all</div>
>![2-3-0-0]

- Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.

><div>$ bosh status</div>
>![2-3-1-0]

- Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3147 버전 사용)
 
><div>$ bosh stemcells</div>
>![2-3-2-0]
><div>Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3147 버전을 업로드를 해야 한다.</div>

- openpaas-mongodb-shard-vsphere-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다.

>$ vi openpaas-mongodb-shard-vsphere-1.0.yml  
```yaml
# openpaas-mongodb-shard-vsphere 설정 파일 내용
---
name: op-mongodb-shard-dev  # 서비스 배포이름(필수)
director_uuid: xxxxxx   #bosh status 에서 확인한 Director UUID을 입력(필수)
>
release:
  name: op-mongodb-shard-dev  #서비스 릴리즈 이름(필수)
  version: latest   #서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전
>
compilation:  # 컴파일시 필요한 가상머신의 속성(필수)
  workers: 4  # 컴파일 하는 가상머신의 최대수(필수)
  network: default  # Networks block에서 선언한 network 이름(필수)
  cloud_properties:   # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    ram: 2048
    disk: 8096
    cpu: 2
>
# this section describes how updates are handled
update:
  canaries: 1   # canary 인스턴스 수(필수)
  canary_watch_time: 30000  # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 30000  # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  max_in_flight: 4
networks:     # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: default
  subnets:
  - cloud_properties:
      name: Internal    # vsphere 에서 사용하는 network 이름(필수)
    dns:  # DNS 정보
    - 10.30.20.24
    - 8.8.8.8
    gateway: 10.30.20.23
    name: default_unused
    range: 10.30.0.0/16
    reserved:   # 설치시 제외할 IP 설정
    - 10.30.0.1 - 10.30.0.10
    - 10.30.60.5 - 10.30.60.40
    - 10.30.60.71 - 10.30.60.100
    static:
    - 10.30.60.41 - 10.30.60.70  #사용 가능한 IP 설정
>
resource_pools:   # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
  - name: mongodb_shard_resource_pools  # 고유한 resource pool 이름
    network: default
    stemcell:
      name: bosh-vsphere-esxi-ubuntu-trusty-go_agent  # stemcell 이름(필수)
      version: 3147   # stemcell 버전(필수)
    cloud_properties: # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
      cpu: 2
      disk: 8192
      ram: 4096
>
jobs:
  - name: mongodb_slave1  #작업 이름(필수): mongodb replica set의 slave 서버
    template: mongodb_slave   # job template 이름(필수)
    instances: 2  # job 인스턴스 수(필수)
    resource_pool: mongodb_shard_resource_pools   # resource_pools block에 정의한 resource pool 이름(필수)
    persistent_disk: 16384  # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:   # 네트워크 구성정보
    - name: default   # Networks block에서 선언한 network 이름(필수)
      static_ips:   # 사용할 IP addresses 정의(필수)
      - 10.30.60.42
      - 10.30.60.43
    properties:
      replSetName: op1 # replicaSet1 의 이름
>
  - name: mongodb_slave2
    template: mongodb_slave
    instances: 2
    resource_pool: mongodb_shard_resource_pools
    persistent_disk: 16384
    networks:
    - name: default
      static_ips:
      - 10.30.60.45
      - 10.30.60.46
    properties:
      replSetName: op2 # replicaSet2 의 이름
>
  - name: mongodb_slave3
    template: mongodb_slave
    instances: 2
    resource_pool: mongodb_shard_resource_pools
    persistent_disk: 16384
    networks:
    - name: default
      static_ips:
      - 10.30.60.48
      - 10.30.60.49
    properties:
      replSetName: op3 # replicaSet3 의 이름
>
  - name: mongodb_master1   #작업 이름(필수): mongodb replica set의 master 서버
    template: mongodb_master  # job template 이름(필수)
    instances: 1  # job 인스턴스 수(필수)
    resource_pool: mongodb_shard_resource_pools   # resource_pools block에 정의한 resource pool 이름(필수)
    persistent_disk: 16384  # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:   # 네트워크 구성정보
    - name: default   # Networks block에서 선언한 network 이름(필수)
      static_ips:
      - 10.30.60.41   # 사용할 IP addresses 정의(필수)
    properties:
      replSet_hosts: ["10.30.60.41","10.30.60.42","10.30.60.43"] # 첫번째 Host는 replicaSet1의 master
      replSetName: op1 # replicaSet1 의 이름
>
  - name: mongodb_master2
    template: mongodb_master
    instances: 1
    resource_pool: mongodb_shard_resource_pools
    persistent_disk: 16384
    networks:
    - name: default
      static_ips:
      - 10.30.60.44
    properties:
      replSet_hosts: ["10.30.60.44","10.30.60.45","10.30.60.46"] # 첫번째 Host는 replicaSet2의 master
      replSetName: op2  # replicaSet2 의 이름
>
  - name: mongodb_master3
    template: mongodb_master
    instances: 1
    resource_pool: mongodb_shard_resource_pools
    persistent_disk: 16384
    networks:
    - name: default
      static_ips:
      - 10.30.60.47
    properties:
      replSet_hosts: ["10.30.60.47","10.30.60.48","10.30.60.49"] # 첫번째 Host는 replicaSet2의 master
      replSetName: op3  # replicaSet3 의 이름
>
  - name: mongodb_config
    template: mongodb_config
    instances: 3
    resource_pool: mongodb_shard_resource_pools
    persistent_disk: 16384
    networks:
    - name: default
      static_ips:
      - 10.30.60.50 - 10.30.60.52
>
  - name: mongodb_shard
    template: mongodb_shard
    instances: 1
    resource_pool: mongodb_shard_resource_pools
    networks:
    - name: default
      static_ips:
      - 10.30.60.53
    properties:
      bindIp: 0.0.0.0
      configsvr_hosts: # mongodb_config hosts
      - 10.30.60.50
      - 10.30.60.51
      - 10.30.60.52
      repl_name_host_list: # mongodb_master properties
      - op1/10.30.60.41 # replicaSet1 의 이름/host
      - op2/10.30.60.44 # replicaSet2 의 이름/host
      - op3/10.30.60.47 # replicaSet3 의 이름/host
>
  - name: mongodb_broker  #작업 이름(필수): mongodb 서비스 브로커
    template: mongodb_broker  # job template 이름(필수)
    instances: 1  # job 인스턴스 수(필수)
    resource_pool: mongodb_shard_resource_pools   # resource_pools block에 정의한 resource pool 이름(필수)
    networks:   # 네트워크 구성정보
    - name: default   # Networks block에서 선언한 network 이름(필수)
      static_ips:   # 사용할 IP addresses 정의(필수)
      - 10.30.60.54
>
  - name : mongodb_broker_registrar   # 작업 이름: 서비스 브로커 등록
    template : mongodb_broker_registrar
    instances: 1
    lifecycle: errand   # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
    resource_pool: mongodb_shard_resource_pools
    networks:
    - name: default
    properties:
      broker:   # 서비스 브로커 설정 정보
        host: 10.30.60.54   # 서비스 브로커 IP 
        name: Mongo-DB  # CF에서 서비스 브로커를 생성시 생기는 서비스 이름 브로커에 고정되어있는 값
        password: cloudfoundry  # 브로커 접근 아이디 비밀번호(필수)
        username: admin   # 브로커 접근 아이디(필수)
        protocol: http
        port: 8080  # 브로커 포트
      cf:
        admin_password: admin   # CF 사용자의 패스워드
        admin_username: admin   # CF 사용자 이름
        api_url: https://api.115.68.46.30.xip.io   # CF 설치시 설정한 api uri 정보(필수)
    release: op-mongodb-shard-dev
>
  - name : mongodb_broker_deregistrar   # 작업 이름: 서비스 브로커 삭제
    template : mongodb_broker_deregistrar
    instances: 1
    lifecycle: errand
    resource_pool: mongodb_shard_resource_pools
    networks:
    - name: default
    properties:
      broker:
        host: 10.30.60.54
        name: Mongo-DB
        password: cloudfoundry
        username: admin
        protocol: http
        port: 8080
      cf:
        admin_password: admin
        admin_username: admin
        api_url: https://api.115.68.46.30.xip.io
    release: op-mongodb-shard-dev
>
meta:
  apps_domain: 115.68.46.30.xip.io   # CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: 115.68.46.30.xip.io   # CF 설치시 설정한 외부 도메인 정보
  nats:   # CF 설치시 설정한 nats 정보
    machines:
    - 10.30.40.11
    password: admin
    port: 4222
    user: nats
  syslog_aggregator: null
>
properties:
  mongodb:  # mongodb shard release의 여러 job에서 공통적으로 하용하는 properties
    # key는 shard를 구성할 때 mongos와 각 replicaSet의 인증을 하기위해 사용
    key: |
      +Qy+1icfeV8D2WXIfCojRjvYlryMVI2Ry+dAi8mYZ1H1Z9pDstRkOC0/oJYs0L/i
      +Dj/3PurWo8MJuqBYrWVGsRnsx31um0SVAgFZM2GQEKvHIByX5hq/MuHlulSLM0h
      GKkMT19zqDwFBFIN53jN0PLuuOnJ6FxZSb4cTLymfWM543WGpYx/31b8ehPYyeRp
      T7P2o2vUd9hecb8mQFxcjsBN7PTLwuPb5lK0BRL4Ze7rh6qeC8j7M3zimV8lX2X5
      9EtWlQP0ORYIlFpqijatZhS8Bf5AfI1EW6kZgfqwycl2ghxmSIbeleiqyQgYZNKQ
      yBXV9disuBXcKy4tsOjSFvKw7y61kjjQOn8KXElefokefdLbcrpeARP6LR9WwR1v
      ZTHcChfzWA4apHo6gJZkoqGVPjF4ArXTYxZfC+hHrsa5oe3XZjNapwV6XQfBNCuQ
      EihT3Td/B7iAUWJnGQvugFJwYKJ5EYOYubhk8QtO9QIvoZxQPDq9tgUsVgiQ6gty
      ZT83oxFAIgm3vky9l3uPwYi6jQ2FvsEJvDyiZl7gulOaC5UD/BdcM4Y5n/dxy/6Y
      qphWWuPsJwnYBXLJgwtTZ/NkYDYyX/tL9gyzXGPkpMMD7DofFjWEpJvHlVRKIxp1
      /zlxbVOMAmASgZDaqFperSQQyrfQqpuvAA8pRkWgorROyrsiRYYWlJZWWa4qHlI9
      OZ1dDp8o71l3v0SqsKbEtxINpdiUNx4OdafsMNN/KVxw9oGdrPXnDl5DomtmAoKZ
      uaCf3AQ3RsDeymgVX3j5EpLCHBhcPj+0B5tv4Yln652HAzDissOUKPyDf+PJaVRo
      OfDOkUvmuqnwl45DOoOtZ0BMw7hXGdgm6Xfv5jEmtSjJzQ1pfwHOOfiY+zZWhHAi
      ow/WNvLtUgNUhobi+OQb11bMMNNtmGWe+cZft6QzBsnd2xa/tAYTZDfAJ8OCvYQK
      e46UrHd54ZJFzdzicRZ8DeuU9G4K
    user: root  # admin 권한 사용자이름
    passwd: openpaas  # admin 권한 사용자 비밀번호
#    replSetName: op
#    bindIp: 10.30.60.53 # shard server ip
    port: 27017   # mongodb port
>
  mongodb_broker:
    db_name: mongodb-broker # mongodb broker 관리용 데이터베이스
    authsource: admin   # mongodb broker 관리용 데이터베이스에 접근할 때 인증정보가 있는 데이터베이스
    hosts: 10.30.60.53  # mongodb Host
```

- Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

>$ bosh deployment {Deployment manifest 파일 PATH}  
>$ bosh deployment openpaas-mongodb-shard-vsphere-1.0.yml  
>![2-3-3-0]

- Mongodb 서비스팩을 배포한다.

>$ bosh deploy  
>![2-3-4-0]  
>![2-3-4-1]  
>![2-3-4-2]  

- 배포된 Mongodb 서비스팩을 확인한다.

><div>$ bosh vms</div>
>![2-3-5-0]  
>![2-3-5-1]  

<div id='10'></div>
### 2.4. Mongodb 서비스 브로커 등록
Mongodb 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Mongodb 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.


- 서비스 브로커 목록을 확인한다.

><div>$ cf service-brokers</div>
>![2-4-0-0]

- Mongodb 서비스 브로커를 등록한다.

>$ cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}  
- 서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.  
- 서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.  
- 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.  
>
>$ cf create-service-broker mongodb-shard-service-broker admin cloudfoundry http://10.30.60.54:8080  
>![2-4-1-0]

- 등록된 Mongodb 서비스 브로커를 확인한다.

><div>$ cf service-brokers</div>
>![2-4-2-0]

- 접근 가능한 서비스 목록을 확인한다.

><div>$ cf service-access</div>
>![2-4-3-0]
><div>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.</div>

- 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

>$ cf enable-service-access Mongo-DB  
>$ cf service-access  
>![2-4-4-0]

<div id='11'></div>
#   3. Mongodb연동 Sample App 설명
본 Sample Web App은 개발형 클라우드 플랫폼에 배포되며 Mongodb의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
<div id='12'></div>
### 3.1. Sample App 구조
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 Mongodb 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 Mongodb 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>src</td>
    <td>Sample 소스 디렉토리/td>
  </tr>
  <tr>
    <td>manifest.yml</td>
    <td>개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일</td>
  </tr>
  <tr>
    <td>build.gradle</td>
    <td>gradle project 설정 파일</td>
  </tr>
  <tr>
    <td>build</td>
    <td>gradle 빌드시 생성되는 디렉토리(war 파일, classes 폴더 등)</td>
  </tr>
</table>

- OpenPaaS-Sample-Apps을 다운로드 받고 Service 폴더안에 있는 Mongodb Sample Web App인 hello-spring-mongodb 를복사한다.

><div>$ ls -all</div>
>![3-1-0-0]

<div id='13'></div>
### 3.2. 개방형 클라우드 플랫폼에서 서비스 신청
Sample Web App에서 Mongodb 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.  
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.


- 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$ cf marketplace</div>
>![3-2-0-0]

- Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$ cf create-service {서비스명} {서비스플랜} {내서비스명}  
- 서비스명 : Mongo-DB로 Marketplace에서 보여지는 서비스 명칭이다.  
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Mongodb 서비스는 100mb, 1gb를 지원한다.  
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.  
>
>$ cf create-service Mongo-DB default-plan mongodb-service-instance  
>![3-2-1-0]

- 생성된 Mongodb 서비스 인스턴스를 확인한다.

><div>$ cf services</div>
>![3-2-2-0]

<div id='14'></div>
### 3.3. Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Mongodb 서비스를 이용한다.  
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

>$ cd hello-spring-mongodb  
>$ vi manifest.yml  
```yaml
---
applications:
- name: hello-spring-mongodb#배포할 App 이름
  memory: 512M# 배포시 메모리 사이즈
  instances: 1# 배포 인스턴스 수
  path: ./build/libs/hello-spring-mongodb.war #배포하는 App 파일 PATH
```
>참고: ./build/libs/hello-spring-mongodb.war 파일이 존재 하지 않을 경우 gradle 빌드를 수행 하면 파일이 생성된다.

- --no-start 옵션으로 App을 배포한다.  
--no-start: App 배포시 구동은 하지 않는다.

><div>$ cf push --no-start<br></div>
>![3-3-0-0]

- 배포된 Sample App을 확인하고 로그를 수행한다.

>$ cf apps  
![3-3-1-0]  
$ cf logs {배포된 App명}  
$ cf logs hello-spring-mongodb  
![3-3-2-0]

- Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다. 

><div>$ cf bind-service hello-spring-mongodb mongodb-service-instance</div>
>![3-3-3-0]

- 바인드가 적용되기 위해서 App을 재기동한다.

><div>$ cf restart hello-spring-mongodb</div>
>![3-3-4-0]  
>![3-3-4-1]  

- (참고) 바인드 후 App구동시 Mongodb 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.

><div>-  rule.json 화일을 만들고 아래와 같이 내용을 넣는다.<br></div>
><div>$ vi rule.json</div>
```json
[
  {
    "protocol": "tcp",
    "destination": "10.30.60.53",
    "ports": "27017"
  }
]
```
><div>- 보안 그룹을 생성한다.<br></div>
><div>$ cf create-security-group Mongo-DB rule.json</div>
>![3-3-5-0]
><div>- 모든 App에 Mongodb 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.<br></div>
><div>$ cf bind-running-security-group Mongo-DB</div>
>![3-3-6-0]
><div>- App을 리부팅 한다.<br></div>
><div>$ cf restart hello-spring-mongodb</div>
>![3-3-7-0]

- App이 정상적으로 Mongodb 서비스를 사용하는지 확인한다.

><div>- curl 로 확인 <br>
$ curl hello-spring-mongodb.115.68.46.30.xip.io
></div>
>![3-3-8-0]
><div>- 브라우져에서 확인<br>
></div>
>![3-3-8-1]

<div id='15'></div>
# 4. Mongodb Client 툴 접속
Application에 바인딩된 Mongodb 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Mongodb Client 툴에서 직접 연결할수 없다. 따라서 SSH 터널, Proxy 터널 등을 제공하는 Mongodb Client 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 Mongodb Client 툴로써는 MongoChef 로 가이드한다. MongoChef 에서 접속하기 위해서 먼저 SSH 터널링 할 수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 vSphere관리자 및 OpenPaaS 운영자에게 문의하여 구성한다.

<div id='16'></div>
### 4.1.  MongoChef 설치 & 연결
MongoChef 프로그램은 무료로 사용할 수 있는 소프트웨어이다.

-  MongoChef을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.
[**http://3t.io/mongochef/download/platform/**](http://3t.io/mongochef/download/platform/)<br>
![4-1-0-0]

- 다운로드한 설치파일을 실행한다.  
![4-1-1-0]

- MongoChef 설치를 위한 안내사항이다. Next 버튼을 클릭한다.  
![4-1-2-0]

- 프로그램 라이선스에 관련된 내용이다. 동의(I accept the terms in the License Agreement)에 체크 후 Next 버튼을 클릭한다.  
![4-1-3-0]

- MongoChef 을 설치할 경로를 설정 후 Next 버튼을 클릭한다.  
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.  
![4-1-4-0]

- Install 버튼을 클릭하여 설치를 진행한다.  
![4-1-5-0]

- Finish 버튼 클릭으로 설치를 완료한다.  
![4-1-6-0]

- MongoChef를 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. Connect버튼을 클릭한다.  
![4-1-7-0]

- 새로운 접속 정보를 작성하기 위해 New Connection 버튼을 클릭한다.  
![4-1-8-0]

- Server에 접속하기 위한 Connection 정보를 입력한다.  
![4-1-9-0]  
서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.  
예) $ cf env hello-spring-mongodb  
![4-1-10-0]

- Authentication탭으로 이동하여 mongodb 의 인증정보를 입력한다.  
![4-1-11-0]

- SSH 터널 탭을 클릭하고 OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력한다.  
![4-1-12-0]  

- 모든 정보를 입력했으면 Test Connection 버튼을 눌러 접속 테스트를 한다.  
![4-1-13-0]  
모두 OK 결과가 나오면 정상적으로 접속이 된다는 것이다. OK 버튼을 눌러 빠져나온다.  

- Save 버튼을 눌러 작성한 접속정보를 저장한다.  
![4-1-14-0]

- 방금 저장한 접속정보를 선택하고 Connect 버튼을 클릭하여 접속한다.  
![4-1-15-0]

- 접속이 완료되면 좌측에 스키마 정보가 나타난다. 컬럼을 더블클릭 해보면 우측에 적재되어있는 데이터가 출력된다.  
![4-1-16-0]

- 우측 화면에 쿼리 항목에 Query문을 작성한 후 실행 버튼(삼각형)을 클릭한다.  
쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.  
![4-1-17-0]


[1-3-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/1-3-0-0.png
[2-1-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-1-0-0.png
[2-1-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-1-1-0.png
[2-2-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-0-0.png
[2-2-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-1-0.png
[2-2-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-2-0.png
[2-2-3-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-3-0.png
[2-2-4-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-4-0.png
[2-2-5-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-5-0.png
[2-2-5-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-5-1.png
[2-2-5-2]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-5-2.png
[2-2-6-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-6-0.png
[2-2-6-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-6-1.png
[2-2-7-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-7-0.png
[2-3-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-0-0.png
[2-3-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-1-0.png
[2-3-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-2-0.png
[2-3-3-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-3-0.png
[2-3-4-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-4-0.png
[2-3-4-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-4-1.png
[2-3-4-2]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-4-2.png
[2-3-5-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-5-0.png
[2-3-5-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-3-5-1.png
[2-4-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-4-0-0.png
[2-4-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-4-1-0.png
[2-4-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-4-2-0.png
[2-4-3-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-4-3-0.png
[2-4-4-0]:/images/openpaas-service/mongodb/mongodb_vsphere/2-4-4-0.png
[3-1-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-1-0-0.png
[3-2-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-2-0-0.png
[3-2-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-2-1-0.png
[3-2-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-2-2-0.png
[3-3-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-0-0.png
[3-3-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-1-0.png
[3-3-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-2-0.png
[3-3-3-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-3-0.png
[3-3-4-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-4-0.png
[3-3-4-1]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-4-1.png
[3-3-5-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-5-0.png
[3-3-6-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-6-0.png
[3-3-7-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-7-0.png
[3-3-8-0]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-8-0.png
[3-3-8-1]:/images/openpaas-service/mongodb/mongodb_vsphere/3-3-8-1.png
[4-1-0-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-0-0.png
[4-1-1-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-1-0.png
[4-1-2-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-2-0.png
[4-1-3-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-3-0.png
[4-1-4-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-4-0.png
[4-1-5-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-5-0.png
[4-1-6-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-6-0.png
[4-1-7-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-7-0.png
[4-1-8-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-8-0.png
[4-1-9-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-9-0.png
[4-1-10-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-10-0.png
[4-1-11-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-11-0.png
[4-1-12-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-12-0.png
[4-1-13-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-13-0.png
[4-1-14-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-14-0.png
[4-1-15-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-15-0.png
[4-1-16-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-16-0.png
[4-1-17-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-17-0.png
[4-1-18-0]:/images/openpaas-service/mongodb/mongodb_vsphere/4-1-18-0.png
[2-2-0-0-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-0-0-1.png
[2-2-4-0-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-4-0-1.png
[2-2-6-0-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-6-0-1.png
[2-2-6-1-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-6-1-1.png
[2-2-7-0-1]:/images/openpaas-service/mongodb/mongodb_vsphere/2-2-7-0-1.png
