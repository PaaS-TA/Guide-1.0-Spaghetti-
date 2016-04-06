
## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 시스템 구성도](#4)
     * [1.4. 참고자료](#5)
2. [Cubrid 서비스팩 설치](#6)
     * [2.1. 설치전 준비사항](#7)
     * [2.2. Cubrid 서비스 릴리즈 업로드](#8)
     * [2.3. Cubrid 서비스 Deployment 파일 수정 및 배포](#9)
     * [2.4. Cubrid 서비스 브로커 등록](#10)
3. [Cubrid 연동 Sample App 설명](#11)
     * [3.1. Sample App 구조](#12)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#13)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#14)
4. [Cubrid Client 툴 접속](#15)
     * [4.1. Putty 다운로드 및 터널링](#16)
     * [4.2. Cubrid Manager 설치 및 연결](#17)

     

<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
      
본 문서(Cubrid 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 Cubrid 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 Cubrid 서비스를 사용하는 방법을 기술하였다.

<div id='3'></div>
### 1.2. 범위 

설치 범위는 Cubrid 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

<div id='4'></div>
### 1.3. 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. Cubrid Server, Cubrid 서비스 브로커로 최소사항을 구성하였다.  
![시스템 구성도][1-3-0-0]

<table>
  <tr>
    <td>구분</td>
    <td>스펙</td>
  </tr>
  <tr>
    <td>openpaas-cubrid-broker</td>
    <td>1vCPU / 4GB RAM / 8GB Disk</td>
  </tr>
  <tr>
    <td>Cubrid</td>
    <td>1vCPU / 4GB RAM / 8GB Disk+16GB(영구적 Disk)</td>
  </tr>
</table>

<div id='5'></div>
### 1.4. 참고자료
**<http://bosh.io/docs>**  
**<http://docs.cloudfoundry.org/>**

<div id='6'></div>
#   2. Cubrid 서비스팩 설치

<div id='7'></div>
### 2.1. 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.  
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.  
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.

- OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip)

- OpenPaaS 에서 제공하는Github에서 git clone 명령을 통해 릴리즈 파일들을 다운로드 받는다.
>$ git clone https://github.com/OpenPaaSRnD/openpaas-service-release.git  
![2-1-0-0]  
>
>$ ls –all  
![2-1-1-0]

<div id='8'></div>
###   2.2. Cubrid 서비스 릴리즈 업로드

- 다운받은 Release 디렉토리안의 open-cubrid-release 디렉토리로 이동한다.

>$ cd openpaas-service-release  
>$ ls –all  
![2-2-0-0]  
>
>$ cd open-cubrid-release  
>$ ls -all  
![2-2-1-0]  

- release를 생성하기 위해 src/cubrid 디렉토리에 Cubrid 소스파일을 다운받는다.

>$ cd src  
>$ mkdir cubrid  
>$ ls -all  
![2-2-2-0]  
>src/cubrid 디렉토리가 생성된 것을 확인  
>
>$ cd cubrid  
>$ wget http://ftp.cubrid.org/CUBRID_Engine/9.3.2/CUBRID-9.3.2.0016-linux.x86_64.tar.gz  
>$ ls -all  
![2-2-3-0]  
>Cubrid 소스파일이 다운로드된 것을 확인

- Release Root 디렉토리로 이동하여 업로드 되어 있는 릴리즈 목록을 확인한다.

>$ cd ../../  
>$ bosh releases  
![2-2-4-0]  
>Cubrid 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인

- Cubrid 서비스 릴리즈를 생성한다. 

>$ bosh create release --force --final --version 1.0  
![2-2-5-0]  
![2-2-5-1]  

- Cubrid 서비스 릴리즈를 업로드한다.

>$ bosh upload release {서비스 릴리즈 파일 PATH}
>
>$ bosh upload release releases/openpaas-cubrid/openpaas-cubrid-1.0.yml  
![2-2-6-0]

- 업로드 된 Cubrid 릴리즈를 확인한다. 

>$ bosh releases  
![2-2-7-0]  
>Cubrid 서비스 릴리즈가 업로드 되어 있는 것을 확인

<div id='9'></div>
### 2.3.  Cubrid 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

- OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 AWS용 Cubrid Deployment 화일인 openpaas-cubrid-aws-1.0.yml를 복사한다.
- 다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-cubrid-aws-1.0.yml)

><div>$ ls –all</div>
>![2-3-0-0]

- Director UUID를 확인한다.
- BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.

><div>$ bosh status</div>
>![2-3-1-0]

- Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3147 버전 사용)
 
><div>$ bosh stemcells</div>
>![2-3-2-0]
><div>Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3147 버전을 업로드를 해야 한다.</div>

- openpaas-cubrid-aws-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다.

>$ vi openpaas-cubrid-aws-1.0.yml 
```yaml
# openpaas-cubrid-aws 설정 파일 내용
---
name: openpaas-cubrid-service  # 서비스 배포이름(필수)
director_uuid: xxxxxxx #bosh status 에서 확인한 Director UUID을 입력(필수)
>
release:
  name: openpaas-cubrid  #서비스 릴리즈 이름(필수)
  version: 1.0   #서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전
>
compilation:          # 컴파일시 필요한 가상머신의 속성(필수)
  workers: 2          # 컴파일 하는 가상머신의 최대수(필수)
  network: default    # Networks block에서 선언한 network 이름(필수)
  cloud_properties:   # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    instance_type: m1.medium
  reuse_compilation_vms: true
>
# this section describes how updates are handled
update:
  canaries: 1                 # canary 인스턴스 수(필수)
  canary_watch_time: 30000    # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 30000    # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 4            # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
>
networks:                     # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: default
  subnets:
  - cloud_properties:
      subnet: subnet-e300eabb
      security_groups:
      - cf-security
      - bosh-security
    dns:                      # DNS 정보
    - 10.0.0.6
    - 8.8.8.8
    gateway: 10.0.0.1
    range: 10.0.0.0/24
    reserved:                 # 설치시 제외할 IP 설정
    - 10.0.0.2 - 10.0.0.19
    static:
    - 10.0.0.22 #사용 가능한 IP 설정
    - 10.0.0.23
  type: manual
>
resource_pools:               # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
  - name: cubrid_resource_pools # 고유한 resource pool 이름
    network: default
    stemcell:
      name: bosh-aws-xen-ubuntu-trusty-go_agent  # stemcell 이름(필수)
      version: 3147 # stemcell 버전(필수)
    cloud_properties:         # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
      instance_type: m1.small
>
jobs:
  - name: cubrid  #작업 이름(필수): Cubrid 서버
    template: cubrid  # job template 이름(필수)
    instances: 1  # job 인스턴스 수(필수)
    resource_pool: cubrid_resource_pools  # resource_pools block에 정의한 resource pool 이름(필수)
    persistent_disk: 16384  # 영구적 디스크 사이즈 정의(옵션): 16G
    networks:     # 네트워크 구성정보
    - name: default   # Networks block에서 선언한 network 이름(필수)
      static_ips:
      - 10.0.0.23   # 사용할 IP addresses 정의(필수): MySQL 서버 IP
  - name: cubrid_broker   #작업 이름(필수): Cubrid 서비스 브로커
    template: cubrid_broker  # job template 이름(필수)
    instances: 1  # job 인스턴스 수(필수)
    resource_pool: cubrid_resource_pools  # resource_pools block에 정의한 resource pool 이름(필수)
    networks: # 네트워크 구성정보
    - name: default   # Networks block에서 선언한 network 이름(필수)
      static_ips:
      - 10.0.0.22   # 사용할 IP addresses 정의(필수)
  - name : cubrid_broker_registrar  # 작업 이름: 서비스 브로커 등록
    template : cubrid_broker_registrar
    instances: 1
    lifecycle: errand   # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
    resource_pool: cubrid_resource_pools
    networks:
    - name: default
    properties:
      broker: # 서비스 브로커 설정 정보
        host: 10.0.0.22 # 서비스 브로커 IP 
        name: CubridDB  # CF에서 서비스 브로커를 생성시 생기는 서비스 이름 브로커에 고정되어있는 값
        password: cloudfoundry  # 브로커 접근 아이디 비밀번호(필수)
        username: admin   # 브로커 접근 아이디(필수)
        protocol: http
        port: 8088  # 브로커 포트
      cf:
        admin_password: admin   # CF 사용자의 패스워드
        admin_username: admin   # CF 사용자 이름
        api_url: https://api.115.68.46.30.xip.io   # CF 설치시 설정한 api uri 정보(필수)
    release: openpaas-cubrid
  - name : cubrid_broker_deregistrar  # 작업 이름: 서비스 브로커 삭제
    template : cubrid_broker_deregistrar
    instances: 1
    lifecycle: errand
    resource_pool: cubrid_resource_pools
    networks:
    - name: default
    properties:
      broker:
        host: 10.0.0.22
        name: CubridDB
        password: cloudfoundry
        username: admin
        protocol: http
        port: 8088
      cf:
        admin_password: admin
        admin_username: admin
        api_url: https://api.115.68.46.30.xip.io
    release: openpaas-cubrid
>
meta:
  apps_domain: 115.68.46.30.xip.io   # CF 설치시 설정한 apps 도메인 정보
  environment: null
  external_domain: 115.68.46.30.xip.io   # CF 설치시 설정한 외부 도메인 정보
  nats: # CF 설치시 설정한 nats 정보
    machines:
    - 10.0.0.199
    password: admin
    port: 4222
    user: nats
  syslog_aggregator: null
>
properties:
  cubrid:   # Cubrid 설정 정보
    max_clients: 200
  cubrid_broker:  # Cubrid Servcice Broker 설정 정보
    cubrid_ip: 10.0.0.23 # Cubrid IP
    cubrid_db_port: 30000 # Cubrid Port
    cubrid_db_name: cubrid_broker   # Cubrid service 관리를 위한 데이터베이스 이름
    cubrid_db_user: dba   # 브로커 관리용 데이터베이스 접근 사용자이름
    cubrid_db_passwd: openpaas  # 브로커 관리용 데이터베이스 접근 사용자 비밀번호
    cubrid_ssh_port: 22   # Cubrid가 설치된 서버 SSH 접속 포트
    cubrid_ssh_user: vcap # Cubrid가 설치된 서버 SSH 접속 사용자 이름
    #cubrid_ssh_passwd: c1oudc0w # Cubrid가 설치된 서버 SSH 접속 사용자 비밀번호
    cubrid_ssh_sudo_passwd: cloudc0w # Cubrid가 설치된 서버 sudo 비밀번호
    cubrid_ssh_identity: /var/vcap/jobs/cubrid_broker/config/bosh.pem # Cubrid가 설치된 서버 SSH 접속 인증 키
    cubrid_ssh_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIIEpQIBAAKCAQEAjzJVGaO1AHCiY51mqUd9uGGnwX9A8YZKawAHNTga3uEIPCTyDUVzfhc05qq1
      fKP5F6xfhHpWsAG5rn3pjJ8U65qrDgq31iotypsyuoyevolAHWgJggGkoviLxhTHgHDrjtXfkvMw
      pMd36yQjar+UO+/+mbQZDk1hY+9EDmdbB4cjjezyBN35RRZ2huzoOzr+LQaaCdNdvtWLFMy73BZi
      p6uHbNKc26DtEbbf5jxwpM1AoUQtB6E9bins6sFE9oddz4k/zAbySV0kr5Kded4ro4ODEN+N5Ulm
      WbRV3tWqHPFIWNHrMGawWrqEjlJQnoDxFfiUmt5F4JEL+BrGq0ICvQIDAQABAoIBAQCIIfGFTnzk
      zi+TRDA0/iInB6/T5IZ5FpHo5z3y+kORX53Uj8sNktkZJjbEk8W1mK1WNCkaaLPA3+MGX+2IIjRh
      R8kWsFnU4IRn+ONhNg40VYb+bcmcLrO1u1QmG9sN4EwQnoLN0SSNgpQel1yUP01NLft9ZBnt3uKX
      fv/UMa34mKqBqA4bBP2wBTi/o9LWyG2l0sp8u2RpJp7ywc4pTAaFyEh/L2tbxJUJsg1F1Ym3c5/g
      x4qpSNrIkC3l1pT0j8m60J1UkscCWu8tMB/JKF2RxqK/awRmvrhLjxlKONX75Sc7HrEG6ipxsUwV
      9Y1x1XH9k3QrGnH0ysmUCpNrVrABAoGBAPKxVPL9XCW+deHHp1C06HQi5IOaNEjbmlonCxKT0n5A
      gaox884LWm41N/Twu8Q+VI8k/syKpKtNu80fjrdbuxxz++ATtGzIvGB3ksKqL08u7fXWQBdD01H5
      FnSOb0+Oz18bm+ncYNG4C6wa3GQpGfX49J0E4jUzFm6BtlL+s1ldAoGBAJcMYLVBBNCMAZSYf4Ak
      hHws190LiaUbdhAVuYFlnC9GoEXcXcdF42s2I5LELziCW2htg6dKBaBjDDUinbv5DXqCfq4QOBx9
      ujPQyX+Wt4LwSy2/GkELKW2c8Am1Ctb8Vhj3f/sgGzr0Wi3BKUEaqoxTS9DvdyWFY75WnzHXgtjh
      AoGBAN+JTsFxdAqf31xS1V2VA/bUnL5KrpXdD9Kx25UE6wC20be9wt6pvmV8R0oo4gnjj2c5oCcu
      HHZcZvqMcpuYtkOC4SCHhcwdKMWB3X/H2UOJ6kxhRy8mneIHGBPLjeMtmvvzJizFe7gvWTDwnMb0
      xKC26jgmY2S05ZhriU/woalRAoGAICWmP21DFQ93FqUOHbun5rW0i3r1JIqfqfS1snBpuTYSzkuE
      OfEJ4bt3/rh6RFCaG7OTHXuHjwcFhhmD8Imk8LntNCaHiqjHhFefSuRM0jVKCsZtVAKIIqgP/kDl
      ubOir1Wdi1jd8sImny5JFW4TdiIUNLeCaB3niI7u+rCfTIECgYEAmiRl2iQdOJsGLjZtDHYoO/Cp
      OJVdo4nVjMpcQ/+yoCm9BmJL1C26IyXVPeSek4aBm3e6BgKbmwxRW9qYc8us8ivMFFBCoFRvKIK+
      MfcRFQZiSv9g0VUgMQEDJKxb2PPsU/ioX9gSM/VCrHzKexeNrXpdIEzWxrpMivnJP3u7Ack=
      -----END RSA PRIVATE KEY-----
```

- Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

>$ bosh deployment {Deployment manifest 파일 PATH}  
>$ bosh deployment openpaas-cubrid-aws-1.0.yml  
![2-3-3-0]

- Cubrid 서비스팩을 배포한다.

>$ bosh deploy  
![2-3-4-0]  
![2-3-4-1]

- 배포된 Cubrid 서비스팩을 확인한다.

>$ bosh vms  
![2-3-5-0]  
![2-3-5-1]  

<div id='10'></div>
### 2.4. Cubrid 서비스 브로커 등록
Cubrid 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 Cubrid 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

- 서비스 브로커 목록을 확인한다.

><div>$ cf service-brokers</div>
![2-4-0-0]

- Cubrid 서비스 브로커를 등록한다.

>$ cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}  
- 서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.  
- 서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.  
- 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.  
>
>$ cf create-service-broker cubrid-service-broker admin cloudfoundry http://10.30.60.22:8080  
>![2-4-1-0]

- 등록된 Cubrid 서비스 브로커를 확인한다.

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
#   3. Cubrid연동 Sample App 설명
본 Sample Web App은 개발형 클라우드 플랫폼에 배포되며 Cubrid의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
<div id='12'></div>
### 3.1. Sample App 구조
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 Cubrid 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 Cubrid 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>src</td>
    <td>Sample 소스 디렉토리</td>
  </tr>
  <tr>
    <td>manifest.yml</td>
    <td>개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일</td>
  </tr>
  <tr>
    <td>pom.xml</td>
    <td>메이븐 project 설정 파일</td>
  </tr>
  <tr>
    <td>target</td>
    <td>메이븐 빌드시 생성되는 디렉토리(war 파일, classes 폴더 등)</td>
  </tr>
</table>

- OpenPaaS-Sample-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 Cubrid Sample Web App인 hello-spring-cubrid를 복사한다.

><div>$ ls -all</div>
>![3-1-0-0]

<div id='13'></div>
### 3.2. 개방형 클라우드 플랫폼에서 서비스 신청
Sample Web App에서 Cubrid 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.


- 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$ cf marketplace</div>
>![3-2-0-0]

- Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$ cf create-service {서비스명} {서비스플랜} {내서비스명}  
- 서비스명 : Mongo-DB로 Marketplace에서 보여지는 서비스 명칭이다.  
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. Cubrid 서비스는 100mb, 1gb를 지원한다.  
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.  
>
>$ cf create-service CubridDB utf8 cubrid-service-instance  
>![3-2-1-0]

- 생성된 Cubrid 서비스 인스턴스를 확인한다.

><div>$ cf services</div>
>![3-2-2-0]

<div id='14'></div>
### 3.3. Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 Cubrid 서비스를 이용한다.  
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

>$ cd hello-spring-cubrid  
>$ vi manifest.yml  
```yaml
---
applications:
- name: hello-spring-cubrid #배포할 App 이름
  memory: 512M # 배포시 메모리 사이즈
  instances: 1 # 배포 인스턴스 수
  path: target/hello-spring-cubrid-1.0.0-BUILD-SNAPSHOT.war #배포하는 App 파일 PATH
```
>참고: ./build/libs/hello-spring-cubrid.war 파일이 존재 하지 않을 경우 gradle 빌드를 수행 하면 파일이 생성된다.

- --no-start 옵션으로 App을 배포한다.  
--no-start: App 배포시 구동은 하지 않는다.

><div>$ cf push --no-start<br></div>
>![3-3-0-0]

- 배포된 Sample App을 확인하고 로그를 수행한다.

><div>$ cf apps<br></div>
>![3-3-1-0]
><div>$ cf logs {배포된 App명}<br>
>$ cf logs hello-spring-cubrid</div>
>![3-3-2-0]

- Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다. 

><div>$ cf bind-service hello-spring-cubrid cubrid-service-instance</div>
>![3-3-3-0]

- 바인드가 적용되기 위해서 App을 재기동한다.

><div>$ cf restart hello-spring-cubrid</div>
>![3-3-4-0]  
>![3-3-4-1]  

- (참고) 바인드 후 App구동시 Cubrid 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.

><div>-  rule.json 화일을 만들고 아래와 같이 내용을 넣는다.<br></div>
><div>$ vi rule.json</div>
```json
[
  {
    "protocol": "tcp",
    "destination": "10.30.60.23",
    "ports": "30000"
  }
]
```
><div>- 보안 그룹을 생성한다.<br></div>
><div>$ cf create-security-group CubridDB rule.json</div>
>![3-3-5-0]
><div>- 모든 App에 Cubrid 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.<br></div>
><div>$ cf bind-running-security-group CubridDB</div>
>![3-3-6-0]
><div>- App을 리부팅 한다.<br></div>
><div>$ cf restart hello-spring-cubrid</div>
>![3-3-7-0]

- App이 정상적으로 Cubrid 서비스를 사용하는지 확인한다.

><div>- curl 로 확인 <br>
$ curl hello-spring-cubrid.115.68.46.30.xip.io
></div>
>![3-3-8-0]
><div>- 브라우져에서 확인<br>
></div>
>![3-3-8-1]

<div id='15'></div>
# 4. Cubrid Client 툴 접속
Application에 바인딩된 Cubrid 서비스 연결정보는 Private IP로 구성되어 있기 때문에 Cubrid Client 툴에서 직접 연결할수 없다. 따라서 Cubrid Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 무료 SSH 및 텔넷 접속 툴인 Putty를 이용하여 SSH 터널을 통해 연결 하는 방법을 제공하며 Cubrid Client 툴로써는 Cubrid에서 제공하는 Cubrid Manager로 가이드한다. Cubrid Manager 에서 접속하기 위해서 먼저 SSH 터널링 할수 있는 VM 인스턴스를 생성해야한다. 이 인스턴스는 SSH로 접속이 가능해야 하고 접속 후 Open PaaS 에 설치한 서비스팩에 Private IP 와 해당 포트로 접근이 가능하도록 시큐리티 그룹을 구성해야 한다. 이 부분은 AWS관리자 및 OpenPaaS 운영자에게 문의하여 구성한다.

<div id='16'></div>
### 4.1.  Putty 다운로드 및 터널링
Putty 프로그램은 SSH 및 텔넷 접속을 할 수 있는 무료 소프트웨어이다.

- Putty를 다운로드 하기 위해 아래 URL로 이동하여 파일을 다운로드 한다. 별도의 설치과정없이 사용할 수 있다.
**<http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>**  
![4-1-0-0]

- 다운받은 putty.exe.파일을 더블클릭하여 실행한다.  
![4-1-1-0]

- Session 탭의 Host name과 Port란에. OpenPaaS 운영 관리자에게 제공받은 SSH 터널링 가능한 서버 정보를 입력한다.  
![4-1-2-0]

- Connection->SSH->Tunnels 탭에서 Source port(내 로컬에서 접근할 포트), Destination(터널링으로 연결할 서버정보)를 입력하고 Local, Auto를 선택 후 Add를 클릭한다.   
![4-1-3-0]  
![4-1-3-1]  
서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.  
예) $ cf env hello-spring-cubrid  
![4-1-4-0]

- Session 탭에서 Saved Sessions에 저장할 이름을 입력하고 Save를 눌러 저장한 후 Open버튼을 누른다.  
![4-1-5-0]

- 서버 접속정보를 입력하여 접속하여 터널링을 완료한다.  
만약 ssh 인증이 Password방식이 아닌 Key인증 방식일 경우, Connection->SSH->인증탭의 '인증 개인키 파일'에 key 파일을 등록하여 인증한다.  
Key파일의 확장자가 .pem이라면 putty설치시 같이 설치된 puttygen을 사용하여 ppk파일로 변환한뒤 사용한다.  
![4-1-6-0]

<div id='17'></div>
### 4.2.  Cubrid Manager 설치 & 연결
Cubrid Manager 프로그램은 Cubrid에서 제공하는 무료로 사용할 수 있는 소프트웨어이다.

- Cubrid Manager를 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.  
**<http://ftp.cubrid.org/CUBRID_Tools/CUBRID_Manager/>**  
![4-2-0-0]

- 다운받은 파일을 더블클릭하여 실행한다.  
![4-2-1-0]

- 한국어를 선택하고 OK를 누른다.  
![4-2-2-0]

- 다음을 눌러 계속 진행한다.  
![4-2-3-0]

- 동의함을 눌러 계속 진행한다.  
![4-2-4-0]

- 바로가기 옵션을 선택 후 다음을 눌러 계속 진행한다.  
![4-2-5-0]

- 설치 경로를 입력하고 설치를 눌러 설치를 시작한다.  
![4-2-6-0]

- 설치가 완료되면 다음을 눌러 계속 진행한다.  
![4-2-7-0]

- 마침을 눌러 설치를 완료한다.  
![4-2-8-0]

- 설치된 Cubrid Manager를 실행하면 처음 나오는 화면이다. Workspace를 선택 후 OK를 눌러 실행한다. 만약 이 창을 다시보기를 원치않는다면 '기본적으로 이것을 사용하고 다시 물어 보지 않기' 옵션을 선택한다.  
![4-2-9-0]

- 관리 모드, 질의 모드 둘중 목적에 맞게 선택 후 확인을 눌러 실행한다.  
여기서는 질의 모드로 실행한다.  
![4-2-10-0]

- 연결정보를 입력하기 위해서 연결 정보 등록을 누른다.  
![4-2-11-0]

- Server에 접속하기 위한 Connection 정보를 입력한다.  
![4-2-12-0]  
서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.  
예) $ cf env hello-spring-cubrid  
![4-2-13-0]

- 연결 테스트 버튼을 클릭하여 접속 테스트를 한다.  
![4-2-14-0]  
정보가 정상적으로 입력되었다면 '연결이 성공하였습니다.'라는 메시지가 나온다.  
확인 버튼을 눌러 창을 닫는다.  
![4-2-15-0]

- 연결 버튼을 클릭하여 접속한다  
![4-2-16-0]

- 접속이 완료되면 좌측에 스키마 정보가 나타난다.  
![4-2-17-0]

- 질의 편집기 버튼을 클릭하면 오른쪽 창에 query를 입력할 수 있는 창이 열린다.  
![4-2-18-0]

- 우측 화면에 쿼리 항목에 Query문을 작성한 후 실행 버튼(삼각형)을 클릭한다.  
쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.  
![4-2-19-0]


[1-3-0-0]:/images/openpaas-service/cubrid/cubrid_aws/1-3-0-0.png
[2-1-0-0]:/images/openpaas-service/cubrid/cubrid_aws/2-1-0-0.png
[2-1-1-0]:/images/openpaas-service/cubrid/cubrid_aws/2-1-1-0.png
[2-2-0-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-0-0.png
[2-2-1-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-1-0.png
[2-2-2-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-2-0.png
[2-2-3-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-3-0.png
[2-2-4-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-4-0.png
[2-2-5-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-5-0.png
[2-2-5-1]:/images/openpaas-service/cubrid/cubrid_aws/2-2-5-1.png
[2-2-6-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-6-0.png
[2-2-7-0]:/images/openpaas-service/cubrid/cubrid_aws/2-2-7-0.png
[2-3-0-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-0-0.png
[2-3-1-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-1-0.png
[2-3-2-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-2-0.png
[2-3-3-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-3-0.png
[2-3-4-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-4-0.png
[2-3-4-1]:/images/openpaas-service/cubrid/cubrid_aws/2-3-4-1.png
[2-3-5-0]:/images/openpaas-service/cubrid/cubrid_aws/2-3-5-0.png
[2-3-5-1]:/images/openpaas-service/cubrid/cubrid_aws/2-3-5-1.png
[2-4-0-0]:/images/openpaas-service/cubrid/cubrid_aws/2-4-0-0.png
[2-4-1-0]:/images/openpaas-service/cubrid/cubrid_aws/2-4-1-0.png
[2-4-2-0]:/images/openpaas-service/cubrid/cubrid_aws/2-4-2-0.png
[2-4-3-0]:/images/openpaas-service/cubrid/cubrid_aws/2-4-3-0.png
[2-4-4-0]:/images/openpaas-service/cubrid/cubrid_aws/2-4-4-0.png
[3-1-0-0]:/images/openpaas-service/cubrid/cubrid_aws/3-1-0-0.png
[3-2-0-0]:/images/openpaas-service/cubrid/cubrid_aws/3-2-0-0.png
[3-2-1-0]:/images/openpaas-service/cubrid/cubrid_aws/3-2-1-0.png
[3-2-2-0]:/images/openpaas-service/cubrid/cubrid_aws/3-2-2-0.png
[3-3-0-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-0-0.png
[3-3-1-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-1-0.png
[3-3-2-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-2-0.png
[3-3-3-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-3-0.png
[3-3-4-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-4-0.png
[3-3-4-1]:/images/openpaas-service/cubrid/cubrid_aws/3-3-4-1.png
[3-3-5-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-5-0.png
[3-3-6-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-6-0.png
[3-3-7-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-7-0.png
[3-3-8-0]:/images/openpaas-service/cubrid/cubrid_aws/3-3-8-0.png
[3-3-8-1]:/images/openpaas-service/cubrid/cubrid_aws/3-3-8-1.png
[4-1-0-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-0-0.png
[4-1-1-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-1-0.png
[4-1-2-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-2-0.png
[4-1-3-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-3-0.png
[4-1-3-1]:/images/openpaas-service/cubrid/cubrid_aws/4-1-3-1.png
[4-1-4-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-4-0.png
[4-1-5-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-5-0.png
[4-1-6-0]:/images/openpaas-service/cubrid/cubrid_aws/4-1-6-0.png
[4-2-0-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-0-0.png
[4-2-1-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-1-0.png
[4-2-2-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-2-0.png
[4-2-3-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-3-0.png
[4-2-4-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-4-0.png
[4-2-5-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-5-0.png
[4-2-6-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-6-0.png
[4-2-7-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-7-0.png
[4-2-8-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-8-0.png
[4-2-9-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-9-0.png
[4-2-10-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-10-0.png
[4-2-11-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-11-0.png
[4-2-12-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-12-0.png
[4-2-13-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-13-0.png
[4-2-14-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-14-0.png
[4-2-15-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-15-0.png
[4-2-16-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-16-0.png
[4-2-17-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-17-0.png
[4-2-18-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-18-0.png
[4-2-19-0]:/images/openpaas-service/cubrid/cubrid_aws/4-2-19-0.png
