
## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 시스템 구성도](#4)
     * [1.4. 참고자료](#5)
2. [GlusterFS 서비스팩 설치](#6)
     * [2.1. 설치전 준비사항](#7)
     * [2.2. GlusterFS 서비스 릴리즈 업로드](#8)
     * [2.3. GlusterFS 서비스 Deployment 파일 수정 및 배포](#9)
     * [2.4. GlusterFS 서비스 브로커 등록](#10)
3. [GlusterFS 연동 Sample App 설명](#11)
     * [3.1. Sample App 구조](#12)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#13)
     * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#14)
4. [GlusterFS Client 툴 접속](#15)
     * [4.1. Putty 다운로드 및 터널링](#16)
     * [4.2. GlusterFS Manager 설치 및 연결](#17)

     


<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
      
본 문서(GlusterFS서비스팩설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인GlusterFS서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서GlusterFS 서비스를 사용하는 방법을 기술하였다.

<div id='3'></div>
### 1.2. 범위 

설치 범위는 GlusterFS서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

<div id='4'></div>
### 1.3. 시스템 구성도
본 문서의 설치된 시스템 구성도입니다. Mysql Server, GlusterFS 서비스 브로커로 최소사항을 구성하였고 서비스 백엔드는 외부에 구성되어 있습니다. 
![시스템 구성도][1-3-0-0]

<table>
  <tr>
    <td>구분</td>
    <td>스펙</td>
  </tr>
  <tr>
    <td>openpaas-glusterfs-broker</td>
    <td>2vCPU / 4GB RAM / 10GB Disk</td>
  </tr>
  <tr>
    <td>mysql</td>
    <td>2vCPU / 4GB RAM / 10GB Disk+10GB(영구적 Disk)</td>
  </tr>
</table>

<div id='5'></div>
### 1.4. 참고자료
**<http://bosh.io/docs>**  
**<http://docs.cloudfoundry.org/>**

<div id='6'></div>
#   2. GlusterFS 서비스팩 설치

<div id='7'></div>
### 2.1. 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.  
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.  
BOSH CLI 가 설치 되어 있지 않을 경우 먼저 BOSH 설치 가이드 문서를 참고 하여BOSH CLI를 설치 해야 한다.

- OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Services.zip, OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip)

<div id='8'></div>
###   2.2. GlusterFS 서비스 릴리즈 업로드

- OpenPaaS-Services.zip 파일 압축을 풀고 폴더안에 있는 GlusterFS 서비스 릴리즈 openpaas-glusterfs- 1.0.tgz 파일을 확인한다.

>$ cd OpenPaaS-Services  
>$ ls –all  
![2-2-0-0]  

- 업로드 되어 있는 릴리즈 목록을 확인한다.

>$ bosh releases  
>
![2-2-1-0]  
>GlusterFS 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인

- GlusterFS 서비스 릴리즈 파일을 업로드한다.

>$ bosh upload release {서비스 릴리즈 파일 PATH}
>
>$ bosh upload release openpaas-glusterfs-1.0.tgz  
![2-2-2-0]  
![2-2-2-1]  
![2-2-2-2]  

- 업로드 된 GlusterFS 릴리즈를 확인한다. 

>$ bosh releases  
![2-2-3-0]  
>GlusterFS 서비스 릴리즈가 업로드 되어 있는 것을 확인

<div id='9'></div>
### 2.3.  GlusterFS 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.  
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params등을 정의가 되어 있다.

- OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는vSphere용 glusterFS Deployment 화일인openpaas-gusterfs-vsphere-1.0.yml를 복사한다.
- 다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-glusterfs-vsphere-1.0.yml)

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

- openpaas-glusterfs-vsphere-1.0.yml Deployment 파일을 서버 환경에 맞게 수정한다.   

>$ vi openpaas-glusterfs-vsphere-1.0.yml 
```yaml
# openpaas-glusterfs-vsphere-1.0 설정 파일 내용
name: openpaas-glusterfs-service# 서비스 배포이름(필수)
director_uuid: xxxxx#bosh status 에서 확인한 Director UUID을 입력(필수)
>
releases:
- name: openpaas-glusterfs#서비스 릴리즈 이름(필수)
  version: 1.0#서비스 릴리즈 버전(필수):latest 시 업로드된 서비스 릴리즈 최신버전
>
update:
  canaries: 1  # canary 인스턴스 수(필수)
canary_watch_time: 30000-600000  # canary 인스턴스가 수행하기 위한 대기 시간(필수)
max_in_flight: 1  # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
update_watch_time: 30000-600000  # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
>
compilation:  #컴파일시 필요한 가상머신의 속성(필수)
cloud_properties:# 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
cpu: 4
    disk: 20480
    ram: 4096
  network: openpaas_network# Networks block에서 선언한 network 이름(필수)
reuse_compilation_vms: true# 컴파일지 VM 재사용 여부(옵션)
  workers: 2  # 컴파일 하는 가상머신의 최대수(필수)
>
jobs:
- instances: 1  # job 인스턴스 수(필수)
  name: mysql  #작업 이름(필수): MySQL 서버
  networks: # 네트워크 구성정보
  - name: openpaas_network# Networks block에서 선언한 network 이름(필수)
static_ips: 10.30.40.196# 사용할 IP addresses 정의(필수): MySQL 서버 IP
persistent_disk: 10000   # 영구적 디스크 사이즈 정의(옵션): 10G, 상황에 맞게 수정
  properties:   # job에 대한 속성을 지정(필수)
admin_username: root# MySQL 어드민 계정
admin_password: admin  # MySQL 어드민 패스워드
  release: openpaas-glusterfs# 서비스 릴리즈 이름(필수)
resource_pool: services-small# Resource Pools block에 정의한 resource pool 이름(필수)
  template: mysql# job template 이름(필수)
>
- instances: 1
  name: openpaas-glusterfs-java-broker
  networks:
  - name: openpaas_network
static_ips: 10.30.40.197
properties:
jdbc_ip: 10.30.40.196   # Mysqlurl(필수)
jdbc_pwd: admin  # Mysql password(필수)
jdbc_port: 3306  # Mysql Port
log_dir: openpaas-glusterfs-java-broker  # Broker Log 저장 디렉토리 명
log_file: openpaas-glusterfs-java-broker  # Broker Log 저장 파일 명
log_level: INFO  # Broker Log 단계
glusterfs_url: 54.199.136.22  # Glusterfs 서비스 주소
glusterfs_tenantname: service  # Glusterfs 서비스 테넌트 이름
glusterfs_username: swift  # Glusterfs 서비스 계정 아이디
glusterfs_password: password  # Glusterfs 서비스 암호
  release: openpaas-glusterfs
resource_pool: services-small
  template: openpaas-glusterfs-java-broker
>
# CF Service Broker Registrar Errand
- instances: 1
  lifecycle: errand  # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: 10.30.40.197  # Broker Url
      name: glusterfs-service  # Broker Name
      password: cloudfoundry  # Broker Auth Password
      username: admin   # Broker Auth Id
      protocol: http  # Broker Http Protocol
      port: 8080  # Broker port
cf:
admin_password: admin  # CF Paasword
admin_username: admin  # CF Id
api_url: https://api.115.68.46.30.xip.io  # CF Target Url
skip_ssl_validation: true # CF SSL 설정
  release: openpaas-glusterfs
resource_pool: services-small
  template: broker-registrar
>
# CF Service Broker Deregistrar Errand
- instances: 1
  lifecycle: errand
  name: broker-deregistrar
  networks:
  - name: openpaas_network
  properties:
    broker:
      name: glusterfs-service
cf:
admin_password: admin
admin_username: admin
api_url: https://api.115.68.46.30.xip.io
skip_ssl_validation: true
  release: openpaas-glusterfs
resource_pool: services-small
  template: broker-deregistrar
>
networks: # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: openpaas_network
  subnets:
  - cloud_properties:
      name: Internal#vsphere 에서 사용하는 network 이름(필수)
dns:
- 10.30.20.24
    - 8.8.8.8
    gateway: 10.30.20.23
    name: default_unused
    range: 10.30.0.0/16
    reserved:   # 설치시 제외할 IP 설정
- 10.30.0.1 - 10.30.0.5
    static:#사용 가능한 IP 설정
- 10.30.40.110 - 10.30.40.200
type: manual   # 네트워크 설정 타입
>
properties: {}
>
resource_pools:  # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:  # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
cpu: 2
    disk: 10480
    ram: 4096
  name: services-small  # 고유한 resource pool 이름
  #size: 5# resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  network: openpaas_network
stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent #stemcell 이름(필수)
    version: "3147"  # stemcell 버전(필수)
```

- Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

>$ bosh deployment {Deployment manifest 파일 PATH}  
>$ bosh deployment openpaas-glusterfs-vsphere-1.0.yml  
![2-3-3-0]

- GlusterFS 서비스팩을 배포한다.

>$ bosh deploy  
![2-3-4-0]  
![2-3-4-1]

- 배포된 GlusterFS 서비스팩을 확인한다.

>$ bosh vms openpaas-glusterfs-service  
![2-3-5-0]  

<div id='10'></div>
### 2.4. GlusterFS 서비스 브로커 등록
GlusterFS 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 GlusterFS 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어있어야 한다.

- 서비스 브로커 목록을 확인한다.

><div>$ cf service-brokers</div>
![2-4-0-0]

- GlusterFS 서비스 브로커를 등록한다.

>$ cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}  
- 서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.  
- 서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.  
- 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.  
>
>$ cf create-service-broker glusterfs-service-broker admin cloudfoundry http://10.30.40.197:8080  
>![2-4-1-0]

- 등록된 GlusterFS 서비스 브로커를 확인한다.

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
#   3. GlusterFS연동 Sample App 설명
본 Sample Web App은 개발형 클라우드 플랫폼에 배포되며 GlusterFS의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
<div id='12'></div>
### 3.1. Sample App 구조
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 GlusterFS 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 GlusterFS 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

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

- OpenPaaS-Sample-Apps.zip 파일 압축을 풀고 Service폴더안에 있는 GlusterFSSample Web App인 hello-spring-glusterfs를복사한다.

><div>$ ls -all</div>
>![3-1-0-0]

<div id='13'></div>
### 3.2. 개방형 클라우드 플랫폼에서 서비스 신청
Sample Web App에서 GlusterFS 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.


- 먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$ cf marketplace</div>
>![3-2-0-0]

- Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

>$ cf create-service {서비스명} {서비스플랜} {내서비스명}  
- 서비스명 : Mongo-DB로 Marketplace에서 보여지는 서비스 명칭이다.  
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. GlusterFS 서비스는 100mb, 1gb를 지원한다.  
- 내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.  
>
>$ cf create-service glusterfs glusterfs-1000Mb glusterfs-service-instance  
>![3-2-1-0]

- 생성된 GlusterFS 서비스 인스턴스를 확인한다.

><div>$ cf services</div>
>![3-2-2-0]

<div id='14'></div>
### 3.3. Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 GlusterFS 서비스를 이용한다.  
*참고: 서비스 Bind 신청시개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.  

- Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.

>$ cd hello-spring-glusterfs  
>$ vi manifest.yml  
```yaml
---
applications:
- name: hello- spring-glustefs #배포할 App 이름
  memory: 512M # 배포시 메모리 사이즈
  instances: 1 # 배포 인스턴스 수
path: target/hello-spring-glusterfs.war #배포하는 App 파일 PATH
```
>참고: ./build/libs/hello-spring-glusterfs.war 파일이 존재 하지 않을 경우 gradle 빌드를 수행 하면 파일이 생성된다.

- --no-start 옵션으로 App을 배포한다.  
--no-start: App 배포시 구동은 하지 않는다.

><div>$ cf push --no-start<br></div>
>![3-3-0-0]

- 배포된 Sample App을 확인하고 로그를 수행한다.

><div>$ cf apps<br></div>
>![3-3-1-0]
><div>$ cf logs {배포된 App명}<br>
>$ cf logs hello-spring-glusterfs</div>
>![3-3-1-1]

- Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다. 

><div>$ cf bind-service hello-spring-glusterfs glusterfs-service-instance</div>
>![3-3-2-0]

- 바인드가 적용되기 위해서 App을 재기동한다.

><div>$ cf restart hello-spring-glusterfs</div>
>![3-3-3-0]  
>![3-3-3-1]  

- App이 정상적으로 GlusterFS 서비스를 사용하는지 확인한다.

><div>- curl 로 확인 <br>
$ curl --form attchFile=@../../../Desert.jpg --form press=OK hello-spring-glusterfs.115.68.46.30.xip.io/swifttest
></div>
>![3-3-4-0]
><div>- 브라우져에서 이미지 확인<br>
></div>
>![3-3-4-1]


[1-3-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/1-3-0-0.png
[2-2-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-0-0.png
[2-2-1-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-1-0.png
[2-2-2-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-2-0.png
[2-2-2-1]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-2-1.png
[2-2-2-2]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-2-2.png
[2-2-3-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-2-3-0.png
[2-3-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-0-0.png
[2-3-1-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-1-0.png
[2-3-2-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-2-0.png
[2-3-3-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-3-0.png
[2-3-4-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-4-0.png
[2-3-4-1]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-4-1.png
[2-3-5-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-3-5-0.png
[2-4-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-4-0-0.png
[2-4-1-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-4-1-0.png
[2-4-2-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-4-2-0.png
[2-4-3-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-4-3-0.png
[2-4-4-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/2-4-4-0.png
[3-1-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-1-0-0.png
[3-2-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-2-0-0.png
[3-2-1-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-2-1-0.png
[3-2-2-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-2-2-0.png
[3-3-0-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-0-0.png
[3-3-1-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-1-0.png
[3-3-1-1]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-1-1.png
[3-3-2-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-2-0.png
[3-3-3-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-3-0.png
[3-3-3-1]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-3-1.png
[3-3-4-0]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-4-0.png
[3-3-4-1]:/images/openpaas-service/glusterfs/glusterfs_vsphere/3-3-4-1.png
