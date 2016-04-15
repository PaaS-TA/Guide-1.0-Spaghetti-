## Table of Contents
1. [문서 개요](#1-문서-개요)
  - 1.1. [목적](#11-목적)
  - 1.2. [범위](#12-범위)
  - 1.3. [시스템 구성도](#13-시스템-구성도)
  - 1.4. [참고자료](#14-참고자료)
2. [API 플랫폼 서비스팩 설치](#2-API-플랫폼-서비스팩-설치)
  - 2.1. [설치전 준비사항](#21-설치전-준비사항)
  - 2.2. [API 플랫폼 서비스 릴리즈 업로드](#22-API-플랫폼-서비스-릴리즈-업로드)
  - 2.3. [API 플랫폼 서비스 Deployment 파일 수정 및 배포](#23-API-플랫폼-서비스-deployment-파일-수정-및-배포)
  - 2.4. [API 매니저에서 API 생성 및 배포](#24-API-매니저에서-API-생성-및-베포)
  -   2.5. [API 플랫폼 서비스 브로커 등록](#25-API-플랫폼-서비스-브로커-등록)
3. [API 플랫폼 연동 Sample Web App 설명](#3-API-플랫폼-연동-sample-web-app-설명)
  - 3.1. [Sample Web App에 서비스 바인드 신청 및 App 확인](#31-sample-web-app에-서비스-바인드-신청-및-app-확인)


# 1. 문서 개요
### 1.1. 목적
본 문서(API 플랫폼 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 API 플랫폼 서비스팩을 Bosh를 이용하여 설치하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 API 플랫폼 서비스를 사용하는 방법을 기술하였다.


### 1.2. 범위
설치 범위는 API 플랫폼 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 


### 1.3. 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. API 플랫폼(apimanager), API 플랫폼 서비스 브로커(apiplatform-broker), Business Activity Monitor(bam), MariaDB(mariadb)로 최소사항을 구성하였다.

![apiplatform_vsphere_1.3.01]

| 구분 | Resource Pool | Instance type/스펙 |
|--------|-------|-------|
| apiplatform-broker | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| apimanager | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| bam | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| mariadb | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |

### 1.4. 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)
[**https://docs.wso2.com/display/AM180/Quick+Start+Guide**](https://docs.wso2.com/display/AM180/Quick+Start+Guide/)

# 2. API 플랫폼 서비스팩 설치
### 2.1. 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다. 서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다. BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다.

- OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Services.zip, OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip)

### 2.2. API 플랫폼 서비스 릴리즈 업로드

- OpenPaaS-Services.zip 파일 압축을 풀고 폴더 안에 있는 API Platform 서비스 릴리즈 파일 openpaas-apiplatform-1.0.tgz 을 확인한다.

>`$ cd openpaas-service-release`

>`$ ls –all`

>![apiplatform_vsphere_2.2.01]

<br>

- API 플랫폼 서비스 릴리즈 파일을 업로드한다.

>`$ bosh upload release {서비스 릴리즈 파일 PATH}`

>`$ bosh upload release openpaas-apiplatform-1.0.tgz`

> ※ 하단의 화면은 릴리즈 파일을 tarball 형태로 압축하지 않고 릴리즈를 업로드하고 있다. 본 문서에서 안내하는 방법대로 tarball 형태로 릴리즈 파일 압축하여 업로드 할 경우에 출력되는 화면은 하단의 화면과 다소 차이가 있다.

>![apiplatform_vsphere_2.2.02]

>![apiplatform_vsphere_2.2.03]

<br>

- 업로드 된API 플랫폼 릴리즈를 확인한다.

>`$ bosh releases`

>![apiplatform_vsphere_2.2.04]

>API 플랫폼 릴리즈가 업로드 되어 있는 것을 확인


### 2.3. API 플랫폼 서비스 릴리즈 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML 파일이다. Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할 것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

- OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더 안에 있는 vSphere용 API 플랫폼 Deployment 화일인 openpaas-apiplatform-vsphere-1.0.yml을 확인한다.

>`$ ls -all`

>![apiplatform_vsphere_2.3.01]

<br>

- Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기 위한 현재 대상 BOSH Director의 UUID와 일치해야 한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할 수 있다.

>`$ bosh status`

>![apiplatform_vsphere_2.3.02]

<br>

- Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3147 버전 사용)

>`$ bosh stemcells`

>![apiplatform_vsphere_2.3.03]

>Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3147 버전을 업로드를 해야 한다.

<br>

- openpaas-apiplatform-vsphere-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다.

```yml
# openpaas-apiplatform-vsphere 설정파일
---
name: openpaas-apiplatform-service    # 서비스 배포이름(필수)

director_uuid: 0bc8d3c2-e032-4c7e-a99c-e23eea7091fc  # bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- {name: openpaas-apiplatform, version: 1.0} 
#서비스 릴리즈 이름(필수), 서비스 릴리즈 버전(필수): latest 시 업로드 된 서비스 릴리즈 최신버전

update:
  canaries: 1                      # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000 # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                  # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000 # 컴파일 시 필요한 가상머신의 속성(필수)

networks:         
- name: default          # network 이름(필수)

subnets:
  - cloud_properties:
      name: Internal                # vsphere 에서 사용하는 network 이름(필수)
    dns:                           # DNS 정보
    - 10.30.20.24
    gateway: 10.30.20.23
    name: default_unused
    range: 10.30.0.0/16
    reserved:                       # 설치시 제외할 IP 설정
    - 10.30.0.1 - 10.30.0.255
    - 10.30.40.1 - 10.30.40.70
    - 10.30.40.101 - 10.30.60.199
    - 10.30.60.211 - 10.30.255.254
    static:                        
    - 10.30.60.200 - 10.30.60.210    # 사용 가능한 IP 설정
type: manual

resource_pools:    #배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- name: apiplatform-servers    # 고유한 resource pool 이름
  network: default             # Networks block에서 선언한 network 이름(필수)

  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent     # stemcell 이름(필수)
    version: 3147                                      # stemcell 버전(필수)
  
  cloud_properties: # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    cpu: 2
    disk: 8192
    ram: 4096


compilation: # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
  workers: 2                    # 컴파일 하는 가상머신의 최대수(필수)
  network: default              # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true   # 컴파일 시 VM 재사용 여부(옵션)
  cloud_properties:            # 컴파일 VM을 만드는 데 필요한 IaaS의 속성(instance_type, availability_zone)
    ram: 4096
    disk: 8192
    cpu: 2

jobs:
- name: mariadb                           # 작업 이름(필수): DB 서버
  instances: 1                              # job 인스턴스 수(필수)
  template: mariadb                        # job template 이름(필수) 
  resource_pool: apiplatform-servers         # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                                # 네트워크 구성정보
  - name: default                           # Networks block에서 선언한 network 이름
    static_ips: 10.30.60.210                   # 사용할 IP addresses 정의(필수): db서버
  properties:                                # job에 대한 속성을 지정(필수)
    admin_username: root                    # DB 어드민 유저이름 
    admin_password: openpaas               # DB 어드민 패스워드
    apiplatform_username: wso2              # API Platform(API Manager)의 DB 접속 유저이름
    apiplatform_password: openpaas          # API Platform(API Manager)의 DB 접속 패스워드
    apiplatform_broker_username: apiplatform  # API Platform Service Broker의 DB 접속 유저이름
    apiplatform_broker_password: openpaas    # API Platform Service Broker의 DB 접속 패스워드

- name: bam                      # 작업 이름(필수): BAM
  instances: 1                      # job 인스턴스 수(필수)
  template: bam                    # job template 이름(필수)
  resource_pool: apiplatform-servers  # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                         # 네트워크 구성정보
  - name: default                    # Networks block에서 선언한 network 이름
    static_ips: 10.30.60.203           # 사용할 IP addresses 정의(필수): BAM

- name: apimanager                # 작업 이름(필수): API Platform(API Manager)
  instances: 1                        # job 인스턴스 수(필수)
  template: apimanager               # job template 이름(필수)
  resource_pool: apiplatform-servers   # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                          # 네트워크 구성정보
  - name: default                     # Networks block에서 선언한 network 이름
    static_ips: 10.30.60.201          # 사용할 IP addresses 정의(필수): API Platform(API Manager)

- name: apiplatform-broker     # 작업 이름(필수): API Platform Service Broker
  instances: 1                 # job 인스턴스 수(필수)
  template: apiplatform-broker  # job template 이름(필수)
  resource_pool: apiplatform-servers  # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                     # 네트워크 구성정보
  - name: default                # Networks block에서 선언한 network 이름
    static_ips: 10.30.60.200       # 사용할 IP addresses 정의(필수): API Platform Service Broker
 
  properties:                   # job에 대한 속성을 지정(필수)
    catalog_login_id: admin     # 카탈로그 API를 사용하기 위한 API플랫폼 유저 아이디
    catalog_login_password: admin  # 카탈로그 API를 사용하기 위한 API플랫폼 유저 패스워드
    apimanager_url: http://10.30.60.201          # API Platform(API Manager) URL
    database_ip: 10.30.60.210                   # DB 접속 URL
    apiplatform_broker_username: apiplatform   # API Platform Service Broker의 DB 접속 유저이름
    apiplatform_broker_password: openpaas     # API Platform Service Broker의 DB 접속 패스워드

- name: broker-registrar      # 작업 이름(필수): 서비스 브로커 등록
  template: broker-registrar   # job template 이름(필수)
  instances: 1               # job 인스턴스 수(필수)
  lifecycle: errand           # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할 때 설정, 주로 테스트 용도에 쓰임
  resource_pool: apiplatform-servers  # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                         # 네트워크 구성정보
  - name: default                    # Networks block에서 선언한 network 이름
  properties:                        # job에 대한 속성을 지정(필수)
    cf:                            # 개방형 클라우드 플랫폼 접속 정보
      api_url: http://api.115.68.46.30.xip.io   # 개방형 클라우드 플랫폼 접속 URL
      admin_username: "admin"   # 개방형 클라우드 플랫폼 접속 유저이름
      admin_password: "admin"    # 개방형 클라우드 플랫폼 접속 패스워드
      skip_ssl_validation: "true"     # 개방형 클라우드 플랫폼 접속 옵션의 일부
    broker:                            # API Platform Service Broker 설정정보
      protocol: http                    # API Platform Service Broker 접속 프로토콜
      host: 10.30.60.200                # API Platform Service Broker 접속 URL
      port: 8080                       # API Platform Service Broker 접속 포트
      name: apiplatform-serivce-broker  # API Platform Service Broker 생성명
      username: "admin"               # API Platform Service Broker auth 유저이름
      password: "cloudfoundry"         # API Platform Service Broker auth 패스워드

- name: broker-deregistrar         #작업 이름(필수): 서비스 브로커 삭제
  template: broker-deregistrar      # job template 이름(필수)
  instances: 1                     # job 인스턴스 수(필수)
  lifecycle: errand                 # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할 때 설정, 주로 테스트 용도에 쓰임
  resource_pool: apiplatform-servers   # Resource Pools block에 정의한 resource pool 이름(필수)
  networks:                       # 네트워크 구성정보
  - name: default                 # Networks block에서 선언한 network 이름
  properties:                      # job에 대한 속성을 지정(필수)
    cf:                           # 개방형 클라우드 플랫폼 접속 정보    
      api_url: http://api.115.68.46.30.xip.io   # 개방형 클라우드 플랫폼 접속 URL
      admin_username: "admin"                  # 개방형 클라우드 플랫폼 접속 유저이름
      admin_password: "admin"                  # 개방형 클라우드 플랫폼 접속 패스워드
      skip_ssl_validation: "true"                   # 개방형 클라우드 플랫폼 접속 옵션의 일부
    broker:                                     # API Platform Service Broker 설정정보
      name: apiplatform-service-broker           # API Platform Service Broker 생성명
```

<br>

- Deploy할 deployment manifest 파일을 BOSH에 지정한다.

>`$ bosh deployment openpaas-apiplatform-vsphere-1.0.yml`

>![apiplatform_vsphere_2.3.04]

<br>

- API 플랫폼 서비스팩을 배포한다. 환경에 따라 다르지만 API 플랫폼 서비스팩 배포는 일반적으로 30분에서 50분 가량이 소요된다. 

>`$ bosh deploy`

>![apiplatform_vsphere_2.3.05]

>![apiplatform_vsphere_2.3.06]

<br>

- 배포된 API 플랫폼 서비스팩을 확인한다.

>`$ bosh vms`

>![apiplatform_vsphere_2.3.07]


### 2.4. API 매니저에서 API 생성 및 배포
API 플랫폼 서비스팩에는 API 매니저(API 플랫폼) 서비스 브로커가 포함되어 있다. API 매니저에 등록된 API 서비스를 개방형 클라우드 플랫폼에서 사용하기 위해서는 서비스 브로커를 생성하여야 한다. 이때, API 매니저에 API 서비스가 존재하지 않으면, 개방형 클라우드 플랫폼에서 서비스 브로커를 생성할 수 없기 때문에 서비스 브로커를 생성하기 전에 API 매니저에서 API 서비스를 등록한다.

>※ 본 문서에서 생성 및 배포하는 API 서비스는 WSO2 API Manager의 공식 문서에서 안내하는 샘플 API이다.

##### 2.4.1. 터널링 설정
API 플랫폼 서비스팩으로 배포한 API 매니저에는 Public IP가 할당되지 않기 때문에 사용자가 웹 브라우저를 통해 API 매니저에 접속할 수 있도록 터널링 설정이 필요하다. 터널링은 다양한 방법을 사용할 수 있지만, 본 문서에서는 SSH 클라이언트인 Putty와 웹 브라우저 Firefox를 이용한 터널링 방법에 대해서 안내한다.

>※ Putty 다운로드: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

>※ Firefox 다운로드: https://www.mozilla.org/ko/firefox/new/

>※ 설치는 별도로 안내하지 않는다.



###### 1. Putty 설정
(1)다운로드한 Putty를 실행하고 Connection 메뉴를 열어 SSH 메뉴에서 Tunnels를 메뉴를 연다.

>![apiplatform_vsphere_2.4.1.01]

<br>

(2) 터널링 정보를 입력한다.<br>
① Source port에 사용하지 않는 임의의 포트를 입력하고 ②번 Dynamic을 선택한 후, ③ Add 버튼을 눌러 추가한다. ④번 위치에서 D{입력한 포트}의 형태로 추가된 것을 확인한다.

>![apiplatform_vsphere_2.4.1.02]

<br>

(3) Putty 접속 정보를 입력한다.<br>
① Session 메뉴를 클릭하여 접속정보 설정 화면으로 이동하여 ② 배포한 API 매니저와 내부망으로 연결되어 있는 머신(설치 환경에 따라 상이함)의 Public IP를 입력한다.

>![apiplatform_vsphere_2.4.1.03]

<br>

(4) Putty 접속 및 로그인<br>
Open 버튼을 클릭하여 해당 머신에 연결한다

>![apiplatform_vsphere_2.4.1.04]

<br>

로그인 화면에서 로그인을 완료한 채로 접속을 유지해둔다.

>![apiplatform_vsphere_2.4.1.05]


###### 2. Firefox 설정
(1) Mozila Firefox 브라우저를 실행하여 첫 화면 하단의 옵션 버튼을 클릭한다.

>![apiplatform_vsphere_2.4.1.06]

<br>

(2) 설정 창을 연다.<br>
① 고급 탭-  ② 네트워크 탭- ③ 설정 버튼을 차례로 클릭하여 설정 창을 연다.

>![apiplatform_vsphere_2.4.1.07]

<br>

(3) 연결 설정 정보를 입력하고 저장한다.<br>
① 프록시 수동설정에 체크하고 ② 화면과 같이 설정을 입력한다. 포트는 Putty 터널링 설정에서 입력한 Source Port와 동일한 포트를 입력한다. ③ 설정을 완료하였으면 확인 버튼을 눌러 저장한다.

>![apiplatform_vsphere_2.4.1.08]

<br>

###### 3. 터널링 설정 확인
터널링 설정이 되어있는 Putty 접속을 유지한 채로, Mozila Firefox 브라우저를 이용하여 API 매니저 관리자 화면에 접속해본다. 하단의 화면이 확인된다면 API 매니저 배포 및 터널링 설정이 정상적으로 이루어진 것이다.

<div id=APICreatePublish></div>
##### 2.4.2. API 생성 및 배포
터널링 설정이 완료 되었다면, API 플랫폼 서비스팩을 통해 배포된 API 매니저에 접속하여 API를 생성하고 배포(Publish)한다.
```
{API매니저 URL}:9443/carbon
예) https://10.0.0.201:9443/carbon
```

>![apiplatform_vsphere_2.4.2.01]

###### 1. API 매니저 접속 및 로그인
① API 플랫폼 서비스팩을 통해 배포된 API 매니저의 publisher 대시보드에 접속한다.

```
{API매니저 URL}:{API매니저 포트}/publisher
예) https://10.30.60.201:9443/publisher
```
※ http가 아닌 https임에 주의한다.


② 관리자 계정으로 로그인한다. 관리자 계정의 Username과 Password는 admin/admin이다.
※ API 매니저 관리자 대시보드({API매니저 URL}:{API매니저 포트}/carbon)에서 계정을 추가하고 권한을 설정하여 사용할 수도 있지만, 그에 대한 설명은 본 문서에서는 기술하지 않는다.

>![apiplatform_vsphere_2.4.2.02]

<br>

###### 2. API 생성
① 로그인이 완료되면 다음과 같은 화면을 확인할 수 있다. 최초 배포가 완료되면 API가 생성되지 않은 상태이므로 ②번의 New API 버튼이 화면에 보여진다. New API 버튼을 클릭하여 API 생성화면으로 이동한다.

>![apiplatform_vsphere_2.4.2.03]

<br>

<div id=DefineGeneralDetails></div>
###### 3. General Details 정의
① API 생성 화면으로 이동하였다.<br>
② API 생성의 첫 단계인 Design 단계이다.<br>
③ General Details에 하단의 화면과 동일하게 다음과 같이 값을 입력한다.<br>

>
```
Name: Phoneverification
Context: /phoneverify
Version: 1.0.0
```
>

※ ④번 Edit Swagger Definition 버튼을 클릭하여 다음의 [[4. Resources 정의]](#DefineResources) 과정을 생략 할 수 있다. 이에 대한 설명은 [[4. Resources 정의]](#DefineResources) 하단에 [[Swagger 정의]](#DefineSwagger)로 첨부한다.

>![apiplatform_vsphere_2.4.2.04]

<div id=DefineResources></div>
###### 4. Resources 정의
① General Details 하단에 Resources 입력란이 있다. URL Pattern에 대소문자 구분에 유의하여 CheckPhoneNumber 값을 입력하고 GET, POST, OPTIONS 메소드를 선택한다. Resource Name의 값은 URL Pattern을 입력하면 같은 값이 자동으로 입력되는데 사용자 필요에 따라 변경할 수 있다.<br>
② 입력이 완료되었다면, Add New Resource 버튼을 클릭하여 Resource를 추가한다.<br>

>![apiplatform_vsphere_2.4.2.05]

<br>

리소스를 추가하면 하단의 그림처럼 추가된 Resource가 화면에 나타난다. 그 중 GET 메소드의 리소스를 클릭하여 파라미터 세부사항을 정의한다.

>![apiplatform_vsphere_2.4.2.06]

<br>

리소스를 클릭하면 파라미터 세부사항 입력란이 노출된다.<br>
① 추가하고자 하는 파라미터 명을 입력한다.<br>
② Add Parameter 버튼을 클릭하여 파라미터를 추가 한다. Prameter Name 항목에서 입력한 파라미터명을 확인할 수 있다.<br>
③ 파라미터의 세부사항을 정의하기 위해서 각각의 파라미터의 값(값이 정의 되어있지 않다면 [+Empty]로 표시됨)을 클릭하여 다음과 같이 파라미터를 정의한다.<br>

| Parameter Name | Description | Parameter Type | Data Type | Required |
|--------|-------|-------|-------|-------|
| PhoneNumber | Give the phone number to be validated | query | string | True |
| LicenseKey | Give the license key. If you don't have any, enter 0 | query | string | True |

④ 하단의 버튼 중, 좌측의 Save버튼을 클릭하여 저장하고 가운데 Implement 버튼을 클릭하여 다음단계인 Implement 단계로 진행한다.<br>

>![apiplatform_vsphere_2.4.2.07]

<br>

<div id=DefineSwagger></div>
※ Swagger 정의<br>
상단의 [[3. General Details 정의]](#DefineGeneralDetails)에서 푸른색 ④번 박스로 표시된 Edit Swagger Definition 버튼을 클릭하면 Swagger 수정이 가능하다. 다음과 같이 수정하고 Save버튼을 클릭하여 Resources를 정의한다. 이 방법을 통해 상단에 기술된 [[4. Resources 정의]](#DefineResources) 절차를 생략할 수 있다.

>
```yml
apiVersion: 1.0.0
swaggerVersion: "1.2"
apis:
  - path: /checkphonenumber
    description: ""
    file:
      apiVersion: 1.0.0
      swaggerVersion: "1.2"
      basePath: "http://localhost:8280/phoneverify/1.0.0"
      resourcePath: /checkphonenumber
      apis:
        - path: /CheckPhoneNumber
          operations:
            - method: GET
              parameters:
                - description: Give the phone number to be validated
                  name: PhoneNumber
                  type: string
                  required: "True"
                  paramType: query
                - description: "Give the license key. If you don't have any, enter 0"
                  name: LicenseKey
                  type: string
                  required: "True"
                  paramType: query
              nickname: get_CheckPhoneNumber
            - method: POST
              parameters:
                - name: body
                  description: Request Body
                  allowMultiple: false
                  required: true
                  paramType: body
                  type: string
              nickname: post_CheckPhoneNumber
            - method: OPTIONS
              parameters:
                - name: body
                  description: Request Body
                  allowMultiple: false
                  required: true
                  paramType: body
                  type: string
              nickname: options_CheckPhoneNumber
info:
  title: PhoneVerification
  description: ""
  termsOfServiceUrl: ""
  contact: ""
  license: ""
  licenseUrl: ""
authorizations:
  oauth2:
    type: oauth2
    scopes: []
```
>

###### 5. Endpoint 입력
① Endpoint를 입력하고 Endpoint 설정과 보안 설정 등을 입력하는 Implement 화면이다.<br>
② Production Endpoint 입력란과 Sandbox Endpoint 입력란에 다음과 같이 Endpoint를 입력한다.<br>
>Production Endpoint: http://ws.cdyne.com/phoneverify/phoneverify.asmx<br>
>Sandbox Endpoint: http://ws.cdyne.com/phoneverify/phoneverify.asmx

③ 추가된 사항을 저장한다.<br>
④ 다음 단계인 Manage 화면으로 이동한다.<br>

>![apiplatform_vsphere_2.4.2.08]

<div id=TierChoose></div>
###### 6. Tier 선택
① API의 Tier 및 추가적인 설정을 입력하는 Manage화면이다.<br>
② Tier Availability는 해당 API 서비스의 호출 횟수를 제한하는 설정으로 개방형 클라우드 플랫폼에서는 플랜으로 표시된다. API 매니저에 설정된 4가지 기본 Tier 중, 사용자 필요에 맞게 선택할 수 있고 API 매니저 관리자 대시보드({API매니저 URL}:{API매니저 포트}/carbon)에서 Tier명과 호출 횟수 제한을 변경할 수 있다. 다만, 현재는 API 서비스 브로커 설계상 API Tier는 Unlimited Tier를 선택하여야 한다. 따라서 Unlimited Tier만을 선택한다. Unlimited Tier만 선택하여 API 서비스를 배포하더라도 개방형 클라우드 플랫폼에서는 Unlimited와 Bronze의 두 가지 플랜을 선택할 수 있다.<br>
③ Save & Publish 버튼을 클릭하여 저장하고 API를 배포한다.<br>

>![apiplatform_vsphere_2.4.2.09]

<br>

###### 7. 배포 확인
① API 매니저의 Store 대시보드에 접속한다.

>
```
{API매니저 URL}:{API매니저 포트}/store
예) https://10.30.60.201:9443/store
```
※ http가 아닌 https임에 주의한다.
>

② 생성한 API가 배포되어 있음을 확인한다. 배포되지 않은 API는 Store 대시보드에 노출되지 않는다.

>![apiplatform_vsphere_2.4.2.10]


### 2.5. 플랫폼 서비스 브로커 등록
API 매니저(API 플랫폼)에 API 서비스가 정상적으로 등록 및 배포가 완료되었다면, 등록된 API 서비스를 개방형 클라우드 플랫폼의 서비스 형태로 제공하기 위해 API 플랫폼 서비스 브로커를 등록해 주어야 한다. 서비스 브로커 등록 시, 개방형 클라우드 플랫폼에서 서비스 브로커를 등록할 수 있는 사용자로 로그인이 되어 있어야 한다.

- 서비스 브로커 목록을 확인한다.

>`$ cf service-brokers`

>![apiplatform_vsphere_2.5.01]

- API 플랫폼 서비스 브로커를 등록한다.

>`$ cf create-service-broker {서비스 브로커 이름} {서비스 브로커 사용자ID} {서비스 브로커 사용자 비밀번호} {서비스 브로커 URL}`

>서비스 브로커 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
>서비스 브로커 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스 팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
>서비스브로커 URL : 서비스 팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.

>`$ cf create-service-broker apiplatform-service-broker admin cloudfoundry http://10.30.60.200:8080`

>![apiplatform_vsphere_2.5.02]

<br>

- 등록된 API 플랫폼 서비스 브로커를 확인한다.

>`$ cf service-brokers`

>![apiplatform_vsphere_2.5.03]

<br>

- 접근 가능한 서비스 목록을 확인한다.

>`$ cf service-access`

>![apiplatform_vsphere_2.5.04]

>서비스 브로커 생성시 기본적으로 접근을 허용하지 않는다. access가 none인 것을 확인할 수 있다.

※ [[2.4 API 매니저에서 API 생성 및 배포]](#APICreatePublish) 장의 [[6. Tier 선택]](#TierChoose)을 보면 tier는 Unlimited 한 가지만 선택하도록 되어있다. 그럼에도 서비스 목록에 Unlimited와 Bronze 두 개의 플랜으로 서비스가 생기는 것은 서비스 브로커가 두 개의 플랜으로 서비스를 생성하도록 설계되어 있기 때문이다. 두개의 플랜 중 선택에 따라 API 호출 횟수가 제한된다.

<br>

- 특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

>`$ cf enable-service-access Phoneverification`

>`$ cf service-access`

>![apiplatform_vsphere_2.5.05]

<br>

- 서비스가 Marketplace에 등록된 것을 확인한다.

>`$ cf marketplace`

>![apiplatform_vsphere_2.5.06]

<br>

# 3. API 플랫폼 연동
[[2.4 API 매니저에서 API 생성 및 배포]](#APICreatePublish)에서 생성한 API 서비스를 샘플 App에 바인드하여, Vcap 환경설정 정보를 정상적으로 획득할 수 있는지를 확인함으로써 연동여부를 확인한다. 단순히 서비스 바인드만 진행하기 때문에 샘플 어플리케이션은 어떤 어플리케이션을 사용해도 무방하다.

### 3.1. Sample Web App에 서비스 바인드 신청 및 App 확인
샘플 App을 배포하고 API 서비스와 바인드 신청을 한다. 먼저 샘플 App을 배포한다.
>※ 참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind를 할 수 있는 사용자로 로그인이 되어 있어야 한다.

- Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

>`$ cd {샘플앱 경로}`

>`$ vi manifest.yml`

>
```yml
# manifest.yml
---
applications:
- name: hello-servlet-tomcat      # 배포할 App 이름
  memory: 512M                 # 배포시 메모리 사이즈
  instances: 1                     # 배포 인스턴스 수
path: target/hello-java-1.0.war    #배포하는 App의 war 파일 경로
```
>

<br>

- '--no-start' 옵션으로 App을 배포한다. '--no-start'는 App 배포시 구동을 하지 않도록 하는 옵션이다.

>`$ cf push --no-start`

>![apiplatform_vsphere_3.1.01]

<br>

- 배포된 Sample App을 확인하고 로그를 수행한다.

>`$ cf apps`

>`$ cf logs hello-servlet-tomcat`

>![apiplatform_vsphere_3.1.02]

<br>

- 바인드할 서비스의 서비스 인스턴스를 생성한다.

>`$ cf create-service {서비스명} {서비스플랜} {내서비스명}`

>서비스명 : p-mysql로 Marketplace에서 보여지는 서비스 명칭이다.
>서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. MySQL 서비스는 100mb, 1gb를 지원한다.
>내 서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.

>`$ cf create-service PhoneVerification Unlimited phoneverification-instance-unlimited`

>![apiplatform_vsphere_3.1.03]

<br>

- 생성된 API 플랫폼 서비스 인스턴스를 확인한다.

>`$ cf services`

>![apiplatform_vsphere_3.1.04]

<br>

- 샘플앱과 생성한 서비스 인스턴스를 바인드 한다.

>`$ cf bind-service hello-servlet-tomcat phoneverification-instance-unlimited`

>![apiplatform_vsphere_3.1.05]


### 3.2. 서비스 바인드 확인
샘플 앱에 대한 VCAP_SERVICES 정보를 확인하여 샘플 앱과 서비스의 정상적인 바인드 여부를 판단한다. VCAP_SERVICES 정보를 확인하는 명령어는 다음과 같다. VCAP_SERVICES에서 앱에서 API를 사용하는데 필요한 정보를 확인할 수 있고 따라서 앱은 이 값을 읽을 수 있도록 작성되어야 한다.

>`$ cf env hello-servlet-tomcat`

>![apiplatform_vsphere_3.2.01]


[apiplatform_vsphere_1.3.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image3.png
[apiplatform_vsphere_2.2.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image4.png
[apiplatform_vsphere_2.2.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image5.png
[apiplatform_vsphere_2.2.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image6.png
[apiplatform_vsphere_2.2.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image7.png
[apiplatform_vsphere_2.3.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image8.png
[apiplatform_vsphere_2.3.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image9.png
[apiplatform_vsphere_2.3.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image10.png
[apiplatform_vsphere_2.3.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image11.png
[apiplatform_vsphere_2.3.05]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image12.png
[apiplatform_vsphere_2.3.06]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image13.png
[apiplatform_vsphere_2.3.07]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image14.png
[apiplatform_vsphere_2.4.1.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image15.png
[apiplatform_vsphere_2.4.1.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image16.png
[apiplatform_vsphere_2.4.1.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image17.png
[apiplatform_vsphere_2.4.1.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image18.png
[apiplatform_vsphere_2.4.1.05]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image19.png
[apiplatform_vsphere_2.4.1.06]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image20.jpeg
[apiplatform_vsphere_2.4.1.07]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image21.png
[apiplatform_vsphere_2.4.1.08]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image22.png
[apiplatform_vsphere_2.4.2.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image23.png
[apiplatform_vsphere_2.4.2.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image24.png
[apiplatform_vsphere_2.4.2.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image25.png
[apiplatform_vsphere_2.4.2.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image26.png
[apiplatform_vsphere_2.4.2.05]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image27.png
[apiplatform_vsphere_2.4.2.06]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image28.png
[apiplatform_vsphere_2.4.2.07]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image29.png
[apiplatform_vsphere_2.4.2.08]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image30.png
[apiplatform_vsphere_2.4.2.09]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image31.png
[apiplatform_vsphere_2.4.2.10]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image32.png
[apiplatform_vsphere_2.5.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image33.png
[apiplatform_vsphere_2.5.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image34.png
[apiplatform_vsphere_2.5.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image35.png
[apiplatform_vsphere_2.5.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image36.png
[apiplatform_vsphere_2.5.05]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image37.png
[apiplatform_vsphere_2.5.06]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image38.png
[apiplatform_vsphere_3.1.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image39.png
[apiplatform_vsphere_3.1.02]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image40.png
[apiplatform_vsphere_3.1.03]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image41.png
[apiplatform_vsphere_3.1.04]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image42.png
[apiplatform_vsphere_3.1.05]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image43.png
[apiplatform_vsphere_3.2.01]:/images/openpaas-service/apiplatform/apiplatform_vsphere/image44.png
