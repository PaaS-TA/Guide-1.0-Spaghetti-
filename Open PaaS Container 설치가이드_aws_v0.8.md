# Table of Contents

-	1. [개요](#1-개요)	
	-	1.1. [문서목적](#11-문서목적)	
	-	1.2. [범위](#12-범위)	
	-	1.3. [참고자료](#13-참고자료)	
-	2. [Prerequisites](#2-prerequisites)	
	-	2.1. [개요](#21-개요)		
	-	2.2. [AWS](#22-aws)		
		-	2.2.1. [Dashboard(Console)](#221-dashboad(consol))	
		-	2.2.2. [Security Group](#222-security-group)	
	-	2.3. [Bosh Server 및 Bosh CLI](#23-bosh-server-및-bosh-cli)	
	-	2.4. [DNS Server](#24-dns-server)	
	-	2.5. [OP CLI](#25-op-cli)	
-	3. [Open PaaS Controller 설치](#3-open-paas-controller-설치)	
	-	3.1. [Release Upload](#31-release-upload)	
	-	3.2. [Stemcell Upload](#32-stemcell-upload)		
	-	3.3. [Deployment Manifest](#33-depoyment-manifest)	
		-	3.3.1. [Name & Release](#331-name-&-release)		
		-	3.3.2. [Networks](#332-networks)	
		-	3.3.3. [Compilation](#333-compliation)	
		-	3.3.4. [Resource Pools](#334-resource-pools)	
		-	3.3.5. [Update](#335-update)	
		-	3.3.6. [Jobs](#336-jobs)	
		-	3.3.7. [Properties](#337-properties)	
	-	3.4. [Bosh Deploy](#34-bosh-deploy)		
		-	3.4.1. [Deployment Manifest 지정](#341-deployment-manifest-지정)	
		-	3.4.2. [Open PaaS Controller Deploy](#342-open-paas-controller-deploy)	
	-	3.5. [설치형상확인](#35-설치형상확인)	








## 1.	개요
---

#### 1.1.	문서 목적
본 문서(설치가이드)는, 현 시점에서 지원되는 IaaS(Infrastructure as aService) 중 하나인 AWS 환경에서 Open PaaS Controller를 설치하기 위한 가이드를제공하는데 그 목적이 있다.

#### 1.2.	범위
본 문서의 범위는 Open PaaS Controller를AWS에 설치하기 데 대한 내용으로 한정되어 있다.Sphere/OpenStack과 같은 다른 IaaS 환경에서의 설치는 그에 맞는 가이드 문서를 참고해야 하며, Bosh 설치 또한 해당 가이드 문서를 별도로 참조해야 한다.

#### 1.3.	참고 자료
http://docs.cloudfoundry.org/deploying/aws/
https://github.com/cloudfoundry/cf-release

 
2.	Prerequisites
#### 2.1.	개요
Open PaaS Controller를 설치하기 전에 IaaS(AWS) 환경이 정상적으로 구성되어 있고, Bosh Server와 Bosh/OP CLI가 설치되어 있는지를 확인해야 한다.

#### 2.2.	AWS
##### 2.2.1.	Dashboard(Console)
 
[그림출처]: Open PaaS 사업단 개발환경

AWS Dashboard(Console)으로 정상 접속되어야 하고, Open PaaS Controller가 설치될 Subnet이 구성되어 있어야 한다. 별도 Subnet 은 필수적인 구성은 아니나, 관리의 용이성을 위해서 사용하는 것을 권장한다.
##### 2.2.2 Security Group
   

SSH, HTTP, HTTPS, DNS Protocol을 받을 수 있고, 모든 통신 Protocol을 엑세스 할 수 있도록 Security Group을 설정한다.(주의: 내부 네트워크 구간에서는 모든 Procotol이 사용 가능하도록 구성해야 한다.)
#### 2.3.	Bosh Server 및 Bosh CLI
 
[그림출처]: Open PaaS 사업단 개발환경

“bosh status” 명령을 실행하여 위와 같이 정상적으로 출력되는 지를 확인한다. 만약 문제 발생 시에는 Bosh 설치가이드를 참조하여 정상적으로 Bosh 환경을 구성한 후 이후 작업을 진행한다.

#### 2.4 DNS Server
	Open PaaS Controller는 독자적인 Zone을 DNS에 등록해야 한다. 사용 가능한 DNS Server가 존재하지 않는다면, VM 등에 별도로 구축하여야 한다. 예를 들어 Linux의 경우에는 bind9 Package를 설치하고 아래와 같이 Platform Zone을 등록한다.

	/etc/bind/named.conf.local	
zone "controller.open-paas.com" {
        type master;
        file "/etc/bind/db.controller.open-paas.com";
};


/etc/resolv.conf  파일 수정 	
nameserver 10.0.0.6
nameserver 8.8.8.8


	/etc/bind/db.controller.open-paas.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.controller.open-paas.com. root.controller.open-paas.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.controller.open-paas.com.
*       IN      A       10.0.16.13# HA Proxy VM IP 주소
@       IN      AAAA    ::1

	NSLOOKUP 등으로 DNS Server에 Platform Domain이 정상 등록 되었는지 확인한다.
	 
#### 2.5 OP CLI
	Open PaaS 설치 패키지 내에 포함되어 있는 OP CLI 압축 파일을 풀고 명령어 Path Folder에 실행 파일을 복사한다.
sudo tar -xvzf$INSTALL_PACKAGE/OpenPaaS-Dev-Tools/op-CLI/cf-linux-amd64.tgz
sudo cp cf /usr/bin

	“cf” 명령어를 입력하면 아래와 같은 Help 화면이 출력됨을 확인한다.
 
 
## 3.	Open PaaS Controller 설치
#### 3.1	Release Upload
배포된 설치 패키지의 OpenPaaS-Controller 폴더에 있는 Open PaaS Controller Bosh Release를 Bosh Server로 아래와 같은 명령으로 1.0 버전을 Upload 한다.
bosh upload release $INSTALL_PACKAGE/OpenPaaS-Controller/openpaas-controller-1.0.tgz

Release Upload는 상황에 따라 다소 차이는 있으나 보통 20-30분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.
 

[주의] Release Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

Bosh Sever에 Release가 정상적으로 Upload 되었는지는 “bosh releases” 명령으로 확인한다.
bosh releases

 

#### 3.2	Stemcell Upload
배포된 설치 패키지의 OpenPaaS-Stemcells 폴더에 있는 Open PaaS AWS용 Stemcell 을 Bosh Server로 아래와 같은 명령으로 3147 Version을 Upload 한다.
bosh upload stemcell$INSALL_PACKAGE/OpenPaaS-Stemcells/bosh-stemcell-3147-aws-xen-ubuntu-trusty-go_agent.tgz

Stemcell Upload는 상황에 따라 다소 차이는 있으나 보통 5-10분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.
 

[주의] Stemcell Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

Bosh Sever에 Stemcell이 정상적으로 Upload 되었는지는 “bosh stemcells” 명령으로 확인한다.
bosh stemcells

 

#### 3.3	Deployment Manifest
배포된 설치 패키지에 포함된 Sample Deployment Manifest File($INSTALL_PACKAGE/OpenPaaS-Deployment/openpaas-controller-aws-1.0.yml)을 아래의 순서대로 설치환경에 적합하게 수정한다.
###### 3.3.1 Name & Release
name: openpaas-controller-1.0# Deployment Name
director_uuid: 3d139c62-6669-4804-adb0-990b16446c37# Bosh Director UUID
releases:
- name: openpaas-controller# BoshRelease Name
  version: latest# BoshRelease Version
Deployment Name은 설치자가 임의로 부여하는데, IaaS와 Version을 표시할 것을 권장한다. Bosh Director UUID는 “bosh status” 명령을 실행하면 출력되는 UUID 값을 넣고, Release Name과 Version은 “bosh releases” 명령의 결과로 나오는 값들을 입력하도록 한다.
##### 3.3.2 Networks		
networks:
- name: op_network# Open PaaS Controller가 설치될 Network Name
  subnets:
  - cloud_properties:
subnet: subnet-71ff185b# AWS Subnet ID
security_groups:
- cf-security
     - bosh
    dns:
    - 10.0.0.6# DNS Server
    - 8.8.8.8
    gateway: 10.0.16.1# Gateway IP Address
    name: default_unused
    range: 10.0.16.0/24# Network CIDR
    reserved:
- 10.0.16.2 - 10.0.16.9
static:
    - 10.0.16.10 - 10.0.16.40# VM에 할당될 Static IP 주소 대역
  type: manual
	Network Name은 설치자가 임의로 부여 가능하다. Neutron Subnet ID, Gateway, DNS Server, Network CIDR은 AWS 구성을 직접 확인하거나 인프라 담당자에게 문의하여 정보를 얻도록 한다. Static IP 주소는 Open PaaS Controller를 설치할 때 개별 VM에 할당될 IP의 주소 대역으로 마찬가지로 인프라 담당자에게 할당을 받아야 한다.
##### 3.3.3 Compilation	
compilation:
  cloud_properties:# Compile용 VM의 사양
instance_type: m1.medium
  network: op_network# Network Name
  reuse_compilation_vms: true
  workers: 6# 동시 동작하는 VM 수
	Network Name은 3.3.2에서 정의한 것과 동일한 이름을 줘야 한다. Workers는 동시에 Compile을 수행하는 VM의 개수로 별다른 환경적 특성이 없다면 Default 값을 사용토록 한다.
###### 3.3.4 Resource Pools	
resource_pools:
- name: small# Resource Name
  cloud_properties:
instance_type: m1.small
env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0 network: op_network# Network Name
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent# Stemcell Name
    version: 3147# Stemcell Version
- name: medium
  cloud_properties:
instance_type: m1.medium
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent
    version: 3147
- name: large
  cloud_properties:
instance_type: m1.large
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent
    version: 3147
- name: runner
  cloud_properties:
instance_type: m1.large
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent
    version: 3147
- name: router
  cloud_properties:
instance_type: m1.medium
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent
    version: 3147

	Stemcell Name과 Version은 “bosh stemcells” 명령어 결과로 출력되는 값들을 입력하도록 한다.
##### 3.3.5 Update
update:
  canaries: 1
  canary_watch_time: 30000-600000
  max_in_flight: 1
  serial: true	# VM의 순차적 Update
  update_watch_time: 5000-600000
	Default 값들을 수정 없이 사용한다.
###### 3.3.6 Jobs
	아래 Sample Jobs를 참고하여 설치 환경에 맞게 수정한다.
jobs:
- instances: 1# VM Instance 개수
  name: consul
  networks:
  - name: op_network# VM이 설치될 Network
    static_ips:
    - 10.0.16.16# Consul에 할당된 IP 주소
  persistent_disk: 1024
  properties:
    consul:
      agent:
        mode: server
    metron_agent:
      zone: z1
      deployment: openpaas-controller
  resource_pool: medium
  templates:
  - name: consul_agent
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update:
    max_in_flight: 1
    serial: true
- name: ha_proxy
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.0.16.13# HAProxy IP 주소
  properties:
    ha_proxy:
      ssl_pem: |# SSL Key
        -----BEGIN CERTIFICATE-----
        MIICzTCCAjYCCQC4Lzsbx+krOjANBgkqhkiG9w0BAQsFADCBqjELMAkGA1UEBhMC
        S1IxDjAMBgNVBAgMBVNlb3VsMQ8wDQYDVQQHDAZKb25nUm8xEjAQBgNVBAoMCW9w
        ZW4tcGFhczESMBAGA1UECwwJb3Blbi1wYWFzMSMwIQYDVQQDDBoqLmNvbnRyb2xs
        ZXIub3Blbi1wYWFzLmNvbTEtMCsGCSqGSIb3DQEJARYeYWRtaW5AY29udHJvbGxl
        ci5vcGVuLXBhYXMuY29tMB4XDTE1MTIxODAyMzgyNVoXDTE2MDExNzAyMzgyNVow
        gaoxCzAJBgNVBAYTAktSMQ4wDAYDVQQIDAVTZW91bDEPMA0GA1UEBwwGSm9uZ1Jv
        MRIwEAYDVQQKDAlvcGVuLXBhYXMxEjAQBgNVBAsMCW9wZW4tcGFhczEjMCEGA1UE
        AwwaKi5jb250cm9sbGVyLm9wZW4tcGFhcy5jb20xLTArBgkqhkiG9w0BCQEWHmFk
        bWluQGNvbnRyb2xsZXIub3Blbi1wYWFzLmNvbTCBnzANBgkqhkiG9w0BAQEFAAOB
        jQAwgYkCgYEAs3yC/6FzVq2WSoZUWAYCwPrAOJ3VpN7JMpJ3fulim6MIzzXjhIeq
        Zl2E10uM9mD0WKWUwTmZcp/a3s+EQZYydgQEY0uQQ1ol/rqnev89PRGu0eAOBKZx
        /GEYxIkDRDHNNcfGo1lj2Of2sTEFR1FvTPy6X784qqW7afqIpo/86yMCAwEAATAN
        BgkqhkiG9w0BAQsFAAOBgQAKU8paqctRObRoI+e2I4G7FPev6GVm3otYi/SEs17q
        LmvMD63QPXEI7r+49FZzaXQtZKALb2NoMJKPO0mhzMJE5GR16f+E8ct1pA6L11t/
        fqce0/oPC+LcX0D36/J1Bw+PL/qJq5NeCOY1ba6JcBPBtckVfwu8Vm+pm+DKGX3i
        hw==
        -----END CERTIFICATE-----
        -----BEGIN RSA PRIVATE KEY-----
        MIICXAIBAAKBgQCzfIL/oXNWrZZKhlRYBgLA+sA4ndWk3skyknd+6WKbowjPNeOE
        h6pmXYTXS4z2YPRYpZTBOZlyn9rez4RBljJ2BARjS5BDWiX+uqd6/z09Ea7R4A4E
        pnH8YRjEiQNEMc01x8ajWWPY5/axMQVHUW9M/Lpfvziqpbtp+oimj/zrIwIDAQAB
        AoGAfHDxakcH7qq/rr/frn/MXPv9VcOoonyMRnHiQ62QXpP0waV9Lx/YdsyUE6kf
        /JpQDz4OGsHSr/RBDYYXDloSdTKx0bBp6xL22SNf0fAkk14biXNc3olc2r5lccPz
        fbKGEDHAGwcOTNb2zFYCWrn0IDuMjHsX+TejLA0mwOhcxgECQQDiyZfw9cuumDGg
        rgMqxlX3DxIeOl/XHo0vOobijLnuG7CP7SS/Em38AEu+kTDUItm9SeRYzB2oDWto
        B1+WSWejAkEAypslTk0Db0gBh9sJbN+4rPJNORvq/2BkXtDGR4WvuDIYvb8q+cYf
        Og1upgjyVBBYV2b4udbXS4R8B42xwcSmgQJAbHtMD/ozbRfmHVs/rpVjB6QQ4Z7A
        u5EkrePMI9B3G/vo0F/6hN+W9sVZdhXTipYFG7Od5A/3W6zXpNJqGeSRCwJAK/0V
        U3PLHB0hH/MBj97fBMWy2IRkOEAgaqmfcyXmafKOhpv747ENVJhX/rqQionl9EwK
        Eqc/pUjFeQpdnrlogQJBAMrDC4bQZ5igTPEXddDA8VN6qRLFDHFlTo2ulVyQ413Y
        HmFUIf4BNcRKD3GO24x63L8xK0ArzW4iLlrkwW8A2IE=
        -----END RSA PRIVATE KEY-----
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
    router:
      servers:
        z1:
        - 10.0.16.15# Router IP 주소
  resource_pool: router
  templates:
  - name: haproxy
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: consul_agent
    release: openpaas-controller
  update: {}
- instances: 1
  name: nats
  networks:
  - name: op_network
    static_ips: 10.0.16.11# NATS IP 주소
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
       apps: op_network
  resource_pool: medium
  templates:
  - name: nats
    release: openpaas-controller
  - name: nats_stream_forwarder
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}
- instances: 1
  name: etcd
  networks:
  - name: op_network
    static_ips:
    - 10.0.16.24# ETCD IP 주소
  persistent_disk: 10024
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: etcd
    release: openpaas-controller
  - name: etcd_metrics_server
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update:
    max_in_flight: 1

- instances: 1
  name: stats
  networks:
  - name: op_network
    static_ips:
    - 10.0.16.31# Stats(Collector) IP 주소
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: small
  templates:
  - name: collector
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}

- instances: 1
  name: nfs
  networks:
  - name: op_network
    static_ips: 10.0.16/24# NFS Server IP 주소
  persistent_disk: 102400
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: debian_nfs_server
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}

- instances: 1
  name: postgres
  networks:
  - name: op_network
    static_ips: 10.0.16.22# DB Server(PostgreSQL) IP 주소
  persistent_disk: 4096
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: postgres
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}

- instances: 1
  name: uaa
  networks:
  - name: op_network
    static_ips: 10.0.16.32# UAA IP 주소
  properties:
    consul:
      agent:
        services:
          uaa: {}
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
    route_registrar:
      routes:
      - name: uaa
        port: 8080
        tags:
          component: uaa
        uris:
        - uaa.controller.open-paas.com
        - '*.uaa.controller.open-paas.com'
        - login.controller.open-paas.com
        - '*.login.controller.open-paas.com'
    uaa:
      proxy:
        servers:
        - 10.0.16.15
  resource_pool: medium
  templates:
  - name: uaa
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: consul_agent
    release: openpaas-controller
  - name: route_registrar
    release: openpaas-controller
  - name: statsd-injector
    release: openpaas-controller
  update: {}

- instances: 1
  name: api
  networks:
  - name: op_network
    static_ips: 10.0.16.33# Cloud Controller IP 주소
  persistent_disk: 8192
  properties:
    consul:
      agent:
        services:
          cloud_controller_ng: {}
          routing-api: {}
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
    nfs_server:
      address: 10.0.16.12# NFS Server IP 주소
      allow_from_entries:
      - 10.0.16.0/24# 허용 Network CIDR 값
      share: null
    route_registrar:
      routes:
      - name: api
        port: 9022
        tags:
          component: CloudController
        uris:
        - api.controller.open-paas.com
  resource_pool: large
  templates:
  - name: cloud_controller_ng
    release: openpaas-controller
  - name: cloud_controller_clock
    release: openpaas-controller
  - name: cloud_controller_worker
  - name: routing-api
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: statsd-injector
    release: openpaas-controller
  - name: consul_agent
    release: openpaas-controller
  - name: nfs_mounter
    release: openpaas-controller
  - name: route_registrar
    release: openpaas-controller
  update: {}

- instances: 1
  name: clock_global
  networks:
  - name: op_network
    static_ips: 10.0.16.34# Cloud Controller Clock IP 주소
  persistent_disk: 4096
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: cloud_controller_clock
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}

- instances: 1
  name: api_worker
  networks:
  - name: op_network
    static_ips: 10.0.16.35# CC Worker IP 주소
  persistent_disk: 0
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
    nfs_server:
      address: 10.0.16.12# NFS Server IP 주소
      allow_from_entries:
      - 10.0.16.0/24# 허용 Network CIDR 값
      share: null
  resource_pool: small
  templates:
  - name: cloud_controller_worker
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: consul_agent
    release: openpaas-controller
  - name: nfs_mounter
    release: openpaas-controller
  update: {}

- instances: 1
  name: doppler
  networks:
  - name: op_network
    static_ips: 10.0.16.38# Doppler IP 주소
  properties:
    doppler:
      zone: z1
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: doppler
    release: openpaas-controller
  - name: syslog_drain_binder
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  update: {}

- instances: 1
  name: loggregator_trafficcontroller
  networks:
  - name: op_network
    static_ips: 10.0.16.39# Loggregator Controller IP 주소
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
    route_registrar:
      routes:
      - name: doppler
        port: 8081
        uris:
        - doppler.controller.open-paas.com
      - name: loggregator
        port: 8080
        uris:
        - loggregator.controller.open-paas.com
    traffic_controller:
      zone: z1
  resource_pool: small
  templates:
  - name: loggregator_trafficcontroller
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: route_registrar
    release: openpaas-controller
  update: {}

- instances: 1
  name: router
  networks:
  - name: op_network
    static_ips: 10.0.16.15# Router IP 주소
  properties:
    consul:
      agent:
        services:
          gorouter: {}
    metron_agent:
      zone: z1
      deployment: openpaas-controller
    networks:
      apps: op_network
  resource_pool: router
  templates:
  - name: gorouter
    release: openpaas-controller
  - name: metron_agent
    release: openpaas-controller
  - name: consul_agent
    release: openpaas-controller
  update: {}

##### 3.3.7 Properties
	아래 Sample Manifest를 참조하여 설치 환경에 맞게 값을 수정한다.
properties:
  acceptance_tests: null
  app_domains:
  - controller.open-paas.com# DNS Server에 등록된 Platform Domain Name
  app_ssh:# App에 ssh 로 접근하기 위한 정보
   host_key_fingerprint: 89:d3:73:01:f3:10:c4:a7:87:53:54:31:63:ee:ef:51
   oauth_client_id: ssh-proxy
  cc:# 여기서부터 Cloud Controller Properties
    allow_app_ssh_access: true
    allowed_cors_domains: []
    app_events:
      cutoff_age_in_days: 31
    app_usage_events:
      cutoff_age_in_days: 31
    audit_events:
      cutoff_age_in_days: 31
    broker_client_default_async_poll_interval_seconds: null
    broker_client_max_async_poll_duration_minutes: null
    broker_client_timeout_seconds: 70
    buildpacks:
      buildpack_directory_key: controller.open-paas.com-cc-buildpacks
      cdn: null
      fog_connection:
        local_root: /var/vcap/store
        provider: Local
    bulk_api_password: admin# Bulk API Password 설정
    client_max_body_size: 2048M
    db_encryption_key: db-encryption-key# DB Encryprion Key 지정
    db_logging_level: debug2
    default_app_disk_in_mb: 1024
    default_app_memory: 1024
    default_buildpacks:
    - name: java_buildpack_offline
      package: buildpack_java_offline
    - name: egov_buildpack
      package: buildpack_egov
    - name: staticfile_buildpack
      package: buildpack_staticfile
    - name: java_buildpack
      package: buildpack_java
    - name: ruby_buildpack
      package: buildpack_ruby
    - name: nodejs_buildpack
      package: buildpack_nodejs
    - name: go_buildpack
      package: buildpack_go
    - name: python_buildpack
      package: buildpack_python
    - name: php_buildpack
      package: buildpack_php
    - name: binary_buildpack
      package: buildpack_binary
    default_health_check_timeout: 60
    default_quota_definition: default
    default_running_security_groups:
    - public_networks
    - dns
    - services
    default_staging_security_groups:
    - public_networks
    - dns
    default_to_diego_backend: true
    development_mode: false
    directories: null
    disable_custom_buildpacks: false
    droplets:
      cdn: null
      droplet_directory_key: controller.open-paas.com-cc-droplets
      fog_connection:
        local_root: /var/vcap/store
        provider: Local
      max_staged_droplets_stored: null
    external_host: api
    external_port: 9022
    external_protocol: null
    install_buildpacks:
    - name: java_buildpack_offline
      package: buildpack_java_offline
    - name: egov_buildpack
      package: buildpack_egov
    - name: staticfile_buildpack
      package: buildpack_staticfile
    - name: java_buildpack
      package: buildpack_java
    - name: ruby_buildpack
      package: buildpack_ruby
    - name: nodejs_buildpack
      package: buildpack_nodejs
    - name: go_buildpack
      package: buildpack_go
    - name: python_buildpack
      package: buildpack_python
    - name: php_buildpack
      package: buildpack_php
    - name: binary_buildpack
      package: buildpack_binary
    internal_api_password: admin# Internal API Password
    internal_api_user: internal_user
    jobs:
      app_bits_packer:
        timeout_in_seconds: null
      app_events_cleanup:
        timeout_in_seconds: null
      app_usage_events_cleanup:
        timeout_in_seconds: null
      blobstore_delete:
        timeout_in_seconds: null
      blobstore_upload:
        timeout_in_seconds: null
      droplet_deletion:
        timeout_in_seconds: null
      droplet_upload:
        timeout_in_seconds: null
      generic:
        number_of_workers: null
      global:
        timeout_in_seconds: 14400
      model_deletion:
        timeout_in_seconds: null
    logging_level: debug2
    maximum_app_disk_in_mb: 2048
    maximum_health_check_timeout: 180
    min_cli_version: null
    min_recommended_cli_version: null
    newrelic:
      capture_params: false
      developer_mode: false
      environment_name: openpaas-controller
      license_key: null
      monitor_mode: false
      transaction_tracer:
        enabled: true
        record_sql: obfuscated
    packages:
      app_package_directory_key: controller.open-paas.com-cc-packages
      cdn: null
      fog_connection:
        local_root: /var/vcap/store
        provider: Local
      max_package_size: 1073741824
      max_valid_packages_stored: null
    quota_definitions:# Application Instance Default Quota 값 지정
      default:
        memory_limit: 10240
        non_basic_services_allowed: true
        total_routes: 1000
        total_services: 100
    resource_pool:
      cdn: null
      fog_connection:
        local_root: /var/vcap/store
        provider: Local
      resource_directory_key: controller.open-paas.com-cc-resources
    security_group_definitions:
    - name: public_networks
      rules:
      - destination: 0.0.0.0-9.255.255.255
        protocol: all
      - destination: 11.0.0.0-169.253.255.255
        protocol: all
      - destination: 169.255.0.0-172.15.255.255
        protocol: all
      - destination: 172.32.0.0-192.167.255.255
        protocol: all
      - destination: 192.169.0.0-255.255.255.255
        protocol: all
    - name: dns
      rules:
      - destination: 0.0.0.0/0
        ports: "53"
        protocol: tcp
      - destination: 0.0.0.0/0
        ports: "53"
        protocol: udp
    - name: services
      rules:
      - destination: 10.0.16.0/24
        protocol: all
service_usage_events:
      cutoff_age_in_days: 31
    srv_api_uri: https://api.controller.open-paas.com# Platform API Target URL
    stacks: null
    staging_upload_password: admin# Staging Upload Password
    staging_upload_user: staging_upload_user
    system_buildpacks:
    - name: java_buildpack_offline
      package: buildpack_java_offline
    - name: egov_buildpack
      package: buildpack_egov
    - name: staticfile_buildpack
      package: buildpack_staticfile
    - name: java_buildpack
      package: buildpack_java
    - name: ruby_buildpack
      package: buildpack_ruby
    - name: nodejs_buildpack
      package: buildpack_nodejs
    - name: go_buildpack
      package: buildpack_go
    - name: python_buildpack
      package: buildpack_python
    - name: php_buildpack
      package: buildpack_php
    - name: binary_buildpack
      package: buildpack_binary
    thresholds:
      api:
        alert_if_above_mb: null
        restart_if_above_mb: null
        restart_if_consistently_above_mb: null
      worker:
        alert_if_above_mb: null
        restart_if_above_mb: null
        restart_if_consistently_above_mb: null
    user_buildpacks: []
    users_can_select_backend: false
  ccdb:
    address: 10.0.16.22# DB Server(PostgreSQL) VM IP 주소
    databases:
    - citext: true
      name: ccdb
      tag: cc
    db_scheme: postgres
    port: 5524
    roles:
    - name: ccadmin
      password: admin# ccadmin 계정 Password
      tag: admin
  collector: null
  consul:
    agent:
      log_level: null
      servers:
        lan:
        - 10.0.16.16# Consul VM IP 주소
    agent_cert: |# Consul agent cert 키 값
     -----BEGIN CERTIFICATE-----
     MIIEIjCCAgygAwIBAgIRANVNoOk6A4WIpnRmprN6Ft4wCwYJKoZIhvcNAQELMBMx
     ETAPBgNVBAMTCGNvbnN1bENBMB4XDTE1MTIxNjA3MjcyN1oXDTE3MTIxNjA3Mjcy
     OFowFzEVMBMGA1UEAxMMY29uc3VsIGFnZW50MIIBIjANBgkqhkiG9w0BAQEFAAOC
     AQ8AMIIBCgKCAQEAvJSw9vW2VCtTbMQ02SQ9H+XpJOA6Pm5B/qsWFFc7YePp2FRg
     CUFO48/SOhKJ99GxdC2io91jmicTMUyHpHjbreohpRlxBpXxlKevPQHY8jVZ1MQ2
     IRGX4V1bi/cpe0rttEeWEy315xjPDdsEevgyUwJ5/gEYzr0PneAzrDkmnXMGZaAv
     /EEM/5cj34HVFrkv3WQ/cY2zOOqzs0wsR02SWgt2H6ne4qukOIDgL4QV3/PjDSiE
     zW7Yta+yxNikwm85BhyGCaucQncOXlNTjJ/a2XDK2FYDzSLm2nNFKLiGqPz6VPgD
     VpH5G/3/875EoJqDQwaEvLzo2TNU5RIyewmfRwIDAQABo3EwbzAOBgNVHQ8BAf8E
     BAMCALgwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCMB0GA1UdDgQWBBQr
     M995lPKvjs4giWC2lz+UksWGoTAfBgNVHSMEGDAWgBRoGRUwOPPgLrI5WCtaB3uj
     sdjkMDALBgkqhkiG9w0BAQsDggIBACmcL4wkwYU1pjp1/fFuesP4xOvxmrs7VKFY
     8eT7IIGv7bk4PMH8xR6y57IAq8VQP4iIe91jlyz5APqP86EhnDHemYimaR4V02R5
     BD5PzN0bDdqpGGAB4oU6OJD5XObEj4yC+0Miy3Mdz5sSmZZ1Tn0o710L4Y+ncjwD
     vEG3wFkCfe/SU8Fd5vVfM1d6CzFDo+szrcoXR56bGrDKAocH641Z4ofCSDs2pCri
     BvJ/OWbekktvqvsA2BX6d78k9FX3RIQZzGUciQtIyiWoJMFT7Gf5D+yK1m8F/Ad8
     ROWL1APXFb/IOjL6K+7E1YIhOpthOYZRtWBp9idhY3/Bzac/cYthOG/mu4YzDf4R
     eX25OO7C2G818xzVk9zxKKnlWJkpRWa1uYLTb/trW6GT/hFaA0BTX0hVzgdnYs/l
     NnhVlPk3wTXWhTBzuvWPVEIyblzt7GszMiYrX9Un1meQxDyVs89dLANAr1tocAo9
     eC98fNpO55RS/mCCzmwtuGO6FL7kGz91BtJRzx+LD9tMyZWWAmjUzR8Ak6iwrHoX
     wyLWcXomI2INoGkaj2/j/KywyDq6zOhb5nA98nQOIZb4FkHgX5H6jajN4DfurZp1
     mq6LVQrV5B7yxf4ul3MfOq/HV2eMzyH6uiKXIuwrFU7poUyn4EdXfUHB5Imjlx46
     ZGkvJ5oM
     -----END CERTIFICATE-----
    agent_key: |# Consul agent 키 값
     -----BEGIN RSA PRIVATE KEY-----
     MIIEpQIBAAKCAQEAvJSw9vW2VCtTbMQ02SQ9H+XpJOA6Pm5B/qsWFFc7YePp2FRg
     CUFO48/SOhKJ99GxdC2io91jmicTMUyHpHjbreohpRlxBpXxlKevPQHY8jVZ1MQ2
     IRGX4V1bi/cpe0rttEeWEy315xjPDdsEevgyUwJ5/gEYzr0PneAzrDkmnXMGZaAv
     /EEM/5cj34HVFrkv3WQ/cY2zOOqzs0wsR02SWgt2H6ne4qukOIDgL4QV3/PjDSiE
     zW7Yta+yxNikwm85BhyGCaucQncOXlNTjJ/a2XDK2FYDzSLm2nNFKLiGqPz6VPgD
     VpH5G/3/875EoJqDQwaEvLzo2TNU5RIyewmfRwIDAQABAoIBAQCM1/TPjbZuaKl3
     G5do8eLhFwKo8vstM4YAoWDtMjV8qA837dSINQmppcaabswylZ+WaJsHDctdD4AL
     GuBX25ge7TXRrA25YHW8k/yScPXJ+ESuXxkaW5x2yhYclC2cEts0AR86FeKJhgLm
     k0RmX0aersPUDJetmJC4VgHhyBFqF301o2BpLQn8nDC5awSqoY6poThELQYkX0s5
     2mNS1GunESaVtpo/qwbb2sdAhVv1M6HKNkcWBE6DOVMR+QwxoIMVj0mosEJT6jsd
     NhZE3aRP49Gv82m6aWAuVtEAiZbIsF/xdsw4pRG2BXwwbiAtjUVMK1nmt2BUjXmK
     ppZbE9ehAoGBAOMn83e3o4IdPhj6Cqs8PPJGOCHRt5hcq66Pkfk2U3fJCONO3ROO
     GkMY6HMrqvTcUpBeHMm1EZjmvKfvK6wjiVs1KNJXFZvrbOF3KMkqFkxev+38zb7L
     VF94OPhgvKDLx9KRiMo9m2013npNx9iimXlGQ0pdi6zil87LbfacL7yXAoGBANSG
     y1eQoMTHS+xxeP5chBxErYaaD40bCCRqQgUuXHxMteltdSR2ChVMIHTHvXgtR6xO
     8l5I4IXucjjmdr0pLecdc40+5UDvxy1UXYgwQVJzOLZ4L2U1wdu7VXFOgr6/Ehl9
     vn4ncEgRlFNpxbyIbZIReHkgxjhOzJ0N+50ogZjRAoGAUs+5vqdAAKtQfCKLySlI
     vrpCtHGUEQOXwyer+8KGY2Dy0ItrpTlk8ZkfBP2icWnw35ivvgk4xRk5Ja/XqAW8
     iXezzhrZUeJd60RZphylzGmuZsCG8UuHEtbtTf9WRPiFfIp55+DVzNaaqNO6S4vb
     j47B8VZxGTHyTf1ztTfzXzkCgYEAsO4HGz8smKXSb2WIdTpQQbhrPkPD7pUykh5k
     GCwgktrKFyso+tHKUzCtVIt1ETehE7Il1JiXUujP7s7uy0wdCutZ550U/pqgFvzF
     YTvLJfkGneIwkvHOEkBDQbE659HqH46vqBbtQxJfiZHlLK/niFNDGJRQcVAoyBd8
     AbpXKgECgYEApqgxW3C7CJStKRdvMQeg50PS5oyXSglU8JoACEYDOUoWQZTDUqci
     CYFJNfdEv/K3AmZH/0hSVuYXhzvdotpWhHvdtmH/YT1bkeoYF5NULyB4VyKOnpyR
     MS/cy+MIiSuLeKK8dNRy62t5Ugo+mgaxuNt3nTlGW0pIathZ9BZJ4Kc=
     -----END RSA PRIVATE KEY-----
    ca_cert: |# Consul ca cert 키 값
     -----BEGIN CERTIFICATE-----
     MIIFAzCCAu2gAwIBAgIBATALBgkqhkiG9w0BAQswEzERMA8GA1UEAxMIY29uc3Vs
     Q0EwHhcNMTUxMjE2MDcyNzA2WhcNMjUxMjE2MDcyNzI1WjATMREwDwYDVQQDEwhj
     b25zdWxDQTCCAiIwDQYJKoZIhvcNAQEBBQADggIPADCCAgoCggIBAKqk/0Lg6fvy
     PMHs/W1yj9lLxdsbJs7GFdoGOLi99DD3zJxN/t2pskagHYqgdSddGqM/ZVIZ+eB/
     tvzUcFczvjG36o+5Bsca31Ez/pCS+1sU88tb5XM5UpGZ0hU2ZB5pdZ08hyKsGqgQ
     dkRLlcq7pCfRs1AHxwUMG34AyvrKwR/uufIDlHm3ChMx7plykeObK5hw6H/ZZDn5
     uvF6ROtSPEvVvm6i7DC45D+hJSbsZF5U4PnGr6Ez8TWvlhJDln3bFDpKZN8NUig+
     nOk05JuxmGjWtpFogtsWfrM5iZzo3Wq9ku8CuYw3F0cZl4L6xycCuzVn2oja/FN8
     wU+0ND2DIntcUbM+CgCrcua+0l1J5ob4JN3TS98mEvO8MeuXIRXVf9EprbGur0YY
     2rVVQ+DMIsOmbD7MzewBnjy0mrU9fuiTX29BoBTk0y+CZM5VpTUJXvnQ54sDfHSI
     Ch1DgvmNgYhPBOkzG8Ecm9oaNXdfl1ZTr0EHycI5aNLeZHOI55SkD2UtFEogmcDc
     wPitTmpVrBNnCnBkWmiqePausRQBJWDvGRKJqWKQmt/hj7X4PrBGE7inPQEFr63A
     d9N7gceZPfPPYDUiKRtbXOaEK20jzyg5RDK6RPKuwO6tQPwhNKzwmgeVKI1TCfow
     OOIGl13s48m7JYul6LRaqpla26Y89siLAgMBAAGjZjBkMA4GA1UdDwEB/wQEAwIA
     BjASBgNVHRMBAf8ECDAGAQH/AgEAMB0GA1UdDgQWBBRoGRUwOPPgLrI5WCtaB3uj
     sdjkMDAfBgNVHSMEGDAWgBRoGRUwOPPgLrI5WCtaB3ujsdjkMDALBgkqhkiG9w0B
     AQsDggIBAKpQwk1sRYfkq5pvVRwnzqSyrdt3tQ9TrtWi15hzucLGQbaPCoCWKmkB
     N9VkeOWsP/f/q9ptRUBBB+qT1270fmCXiTYMdJAnLers32gomqQmv7H/eI0d3Mzr
     1PA8P8W5066hqywsBLd4D0lcrCaC5VkvvUIG6RE3sOh5wW1UZxUR52LPLPqiiS8d
     oU9NXb8BtxW+juj+VypnwisEMXtV4K7Wf0g+05w8PND3PQbzj665em+cCt7sG9Fd
     EOvxhmqFQhJKfiFZe/uNORrlZBVX8w0M+OLRi99feH93BLnklpH2QYVw9XDUu6gV
     zzcAMuOuwd75ngHbW5dRiP6hE8Qn10NQf+K6Hq5gXgnGI7r1yic63JeXYqU9N981
     9krNGikmRYoEAF5Mbind2u8c4ce7/b6TjvsgX2Ddj3nRuPTbg1+9Vl8v7/kQrPaP
     3jR+X+OJ4qOZH//lIAy99Ifs2EqQh2EdqJXLH3cddWhUtf7UdsMciyYNotswlZxs
     CNoRpYD795IrQeAmgx3avhdihCUeZRyy5zZgpufkyLJe0CDvH0XRChTZhOSjptDb
     3LeZoWKxn2G0f9ZL0SasOyU+uLtGSQLpavW22CVwDPvWrxj5BpG3Ulddgh/ysIwN
     4PFVkjXsY1Ca5mC7mMu0+XSaALaNlXlJ7GuRmf+CU2sWCQOxGvp+
     -----END CERTIFICATE-----
    encrypt_keys:# Consul encrypt 키 값
     - t66mLrBhJ5kpofLwoJpH5A==
    require_ssl: true# Consul ssl 접속 여부
    server_cert: |# Consul server cert 키 값
     -----BEGIN CERTIFICATE-----
     MIIEKzCCAhWgAwIBAgIQMkaGpfb7hSNQcutiGf4ERjALBgkqhkiG9w0BAQswEzER
     MA8GA1UEAxMIY29uc3VsQ0EwHhcNMTUxMjE2MDcyNzI2WhcNMTcxMjE2MDcyNzI3
     WjAhMR8wHQYDVQQDExZzZXJ2ZXIuZGMxLmNmLmludGVybmFsMIIBIjANBgkqhkiG
     9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqZvu3TU4dQeh4veVGti3hhYP/7YCbKI8y7q6
     Qyz7FCvQJSsD0bUXnG6+U7tNHOTRNdBTnQrmZFeWoXXGscqZLwSRnTlG6q28Bfwc
     imMbnqM1xz/1TlAXsC4NSw4quvoxW+KlWVNM9/srga5+aWJCXOx4ozF770eb82pe
     8EAGhAHZ2zx1xZ3x8NdQRsOc77pYdXrNjWcxicQzONZ7+DKrhk3Enw7DWZkkUITv
     1DtHjD6HN3v847ZLS5hk6c8bn2EHGqzvXHFSLXM1hJrom3lhDiaO6tN0lC2EPwSz
     ogQ8obPy5VHhST3LgpsondqTlZkAVX8UUsmhn7M+vuXdxuD4iQIDAQABo3EwbzAO
     BgNVHQ8BAf8EBAMCALgwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCMB0G
     A1UdDgQWBBTnXp73N5Gb7qFeRa66ESaCYCj5azAfBgNVHSMEGDAWgBRoGRUwOPPg
     LrI5WCtaB3ujsdjkMDALBgkqhkiG9w0BAQsDggIBAFhuxqoVQ66uk07TwJoet8Jr
     kNtLDPadOlKPBgnOrER8GpNhxg8cet5tWyKD6vPveuzecGKGoXO1EamLdLSmx73m
     HD/e0uISZ1s/yCVYzRfsCmaFw2hsFQvlUIp0J3KwEpA71sP6jyN+il9cwfR0v9Z+
     KhrgCVlYlxk+GpXG1mNx908omLydss81OJRuw/VUCi96htH8naur7JCSYUYNmXTU
     hAPxF3C/uZULGDs/ktJ6orEjybAn0lLFJSJr7FTb2bhmJcxDXnm1B3KU9ggCA9za
     oRmi53xFVZBa9arYMvnD97T7zwioJVww8u+VVBAuOaeZggliSZL3SfloJ6u8LhWO
     MObMsNEgl9/IziNEbC3XcjT6rwcepWb4mNCaoEsm3fKAAwjUv2qHy7FslM2O0GDx
     wgpCCVuG0sN3zYJYr4zCvP3nnir49jZ1gJ1vKikK+Qk72crPEcsWh7/iuWIb0Ir2
     h2LbwCUsXo1DDptJiDn7FfEX4WW9OStaI4WCgvlcKMlMwcnZIZ3tN7NlNzon8fAV
     LR0GHcKqvPmPp49zvjXt4BroQPdUgM3WZPpCNtZDqDc9obfS5ap7FAVQWBaPqOu0
     aMgvXar2LxBuM7JWCorYzztm5sv4NJSjxjDkkybwOImTxZkpLZb2j58TdXQiE1v+
     fns7uQ9yHVtyl941hRlf
     -----END CERTIFICATE-----
    server_key: |# Consul server 키 값
     -----BEGIN RSA PRIVATE KEY-----
     MIIEowIBAAKCAQEAqZvu3TU4dQeh4veVGti3hhYP/7YCbKI8y7q6Qyz7FCvQJSsD
     0bUXnG6+U7tNHOTRNdBTnQrmZFeWoXXGscqZLwSRnTlG6q28BfwcimMbnqM1xz/1
     TlAXsC4NSw4quvoxW+KlWVNM9/srga5+aWJCXOx4ozF770eb82pe8EAGhAHZ2zx1
     xZ3x8NdQRsOc77pYdXrNjWcxicQzONZ7+DKrhk3Enw7DWZkkUITv1DtHjD6HN3v8
     47ZLS5hk6c8bn2EHGqzvXHFSLXM1hJrom3lhDiaO6tN0lC2EPwSzogQ8obPy5VHh
     ST3LgpsondqTlZkAVX8UUsmhn7M+vuXdxuD4iQIDAQABAoIBAA+UZ0iijuERJmm2
     3Fdu+My0UhvnGCnqbTM5n8pz40xjCeegH+WvgM+5zHnchqTUz/LlhAi065zj/s4u
     B2ZledS70rRigoUkGFVkZyq/F9Zmn5YB5gKfkM16LXZrgrlSKh6Kny2pXTSaOdDb
     unjrsbTfc+Vpsjy0kzq01quy9eZCLvClKjSNG4zc4P1Yxo6ptvkYq6zTv/+CA5dM
     AKfzB2NHcEu4lAnWS29ps2gDg4mIDs5tUaC5nE4KRXZHo/o7iHBR3KzyR4mFD/n+
     a5eyjkQFWLOVqBCfseLri8ESmkFZeZJ32td3mUIxokEVuwBdZ7Zb8KKI7QhweVv2
     WQzzUEECgYEAxkdvVE/sJAeDHs176QquLhCZJ83YFjgxU+FWO9JJuANm3j0CWEqK
     Rw6mlparltr8lzJhs/lVEhOmfNEMDw3RcF8TBxj9gsZn12To8yh8PzSz0DpjiDAO
     Osw7iNWy95PvgqQy11+OizUpS5zaVwjc2YByhec0OxzDAUO9D4SLuP0CgYEA2vvl
     2BLlVxkbz73s36JbXMtL7UvqakbFmbcdQSu07czGbdFtNxNhphKIVBCH4ny2yBxm
     qEEpuis/fkVzhD02V81dI63PIhRZuFEXnGWhMeY44VmWCmbaoUdZd9Ozna2AnVVN
     a5Qw1hWHcmfpcILaFgCb/j2XHqveTZ87pMjdyX0CgYA5cbq8V4dXjOGdC/VJN/Hs
     oJxunsFq9o67+X3NSQhYiovD+TLzt2zGV2VGHZLK2tjxSQRrauINoanLYZk3x04V
     W0Yc+U2BFNBC5BZlVCZi/XbW7gOmEh4dRMw+wYLfHXn3hHDCWwnmJNm48VGEg6nQ
     TdlgF/LW6WdJt4FPvJvqVQKBgFs3YFdwD44HTHltcJT7CTmPCVKQM9YPItJT32C9
     NwFzMhieivLNJPjLcXQq6p9iObUDd5OQiTQePbV4cpTb9p3+UlTBWq2kcnb/eGlS
     QCIL9xePfJtamqlhkhgC3CfLFO70kGpGcU1L7H6wYCHYr8VIfbIar688Aj6tHGgY
     r6H1AoGBAK81Evrf729tMezOMoGQKGfZrP9pkh6Bh5ProZFqK5R9GItXdYU4ATvL
     1/dmdLjPwDpprC90gfjDnvT+rxBJUQmlklRV4YvutaOO25UJQtnLEA7oBmASYGjh
     QsflMYJj0ytvop7ReDVV6+p6OymS2SBZrdO2AvwRNy6cVSuPjgLn
     -----END RSA PRIVATE KEY-----
  databases:
    additional_config: null
    address: 10.0.16.22# DB Server VM IP 주소
    collect_statement_statistics: null
    databases:
    - citext: true
      name: ccdb
      tag: cc
    - citext: true
      name: uaadb
      tag: uaa
    db_scheme: postgres
    port: 5524
    roles:
    - name: ccadmin
      password: admin
      tag: admin
    - name: uaaadmin
      password: admin
      tag: admin
  dea_next:
    advertise_interval_in_seconds: 5
    allow_host_access: null
    allow_networks: []
    default_health_check_timeout: 60
    deny_networks: []
    directory_server_protocol: https
    disk_mb: 32768
    disk_overcommit_factor: 3
    evacuation_bail_out_time_in_seconds: 0
    heartbeat_interval_in_seconds: 10
    instance_bandwidth_limit: null
    instance_disk_inode_limit: 200000
    kernel_network_tuning_enabled: false
    logging_level: debug
    memory_mb: 8192
    memory_overcommit_factor: null
    mtu: null
    rlimit_core: 0
    staging_bandwidth_limit: null
    staging_disk_inode_limit: 200000
    staging_disk_limit_mb: 6144
    staging_memory_limit_mb: 1024
  description: Open PaaS sponsored by OCP Team
  disk_quota_enabled: false
  domain: controller.open-paas.com
  doppler:
    blacklisted_syslog_ranges: null
    debug: false
    enable_tls_transport: null
    maxRetainedLogMessages: 100
    port: 4443# Doppler port 번호
    tls_server:
      cert: null
      key: null
      port: null
    unmarshaller_count: 5
  doppler_endpoint:
    shared_secret: admin# Doppler Endpoint Password
  dropsonde:
    enabled: true
  etcd:
    machines:
    - 10.0.16.24# etcd VM IP 주소
    peer_require_ssl: false
    require_ssl: false
  etcd_metrics_server:
    nats:
      machines:
      - 10.0.16.11# NATS Server VM IP 주소
      password: admin
      username: nats
  hm9000:
    url: https://hm9000.controller.open-paas.com
  logger_endpoint:
    port: 443
    use_ssl: true
  loggregator:
    blacklisted_syslog_ranges: null
    debug: false
    etcd:
      machines:
      - 10.0.16.24
    maxRetainedLogMessages: 100
    outgoing_dropsonde_port: 8081
    tls:
      ca: null
  loggregator_endpoint:
    shared_secret: admin
  login:
    analytics:
      code: null
      domain: null
    asset_base_url: null
    brand: oss
    catalina_opts: null
    enabled: true
    invitations_enabled: null
    links:
      passwd: https://console.controller.open-paas.com/password_resets/new
      signup: https://console.controller.open-paas.com/register
    logout: null
    messages: null
    notifications:
      url: null
    protocol: null
    restricted_ips_regex: null
    saml: null
    self_service_links_enabled: null
    signups_enabled: null
    smtp:
      host: null
      password: null
      port: null
      user: null
    spring_profiles: null
    tiles: null
    uaa_base: null
    url: null
  metron_agent:
    deployment: openpaas-controller
    preferred_protocol: null
    tls_client:
      cert: null
      key: null
  metron_endpoint:
    shared_secret: admin
  nats:
    address: 10.0.16.11# NATS Server VM IP 주소
    debug: false
    machines:
    - 10.0.16.11# NATS Server VM IP 주소
    monitor_port: 4221
    password: admin
    port: 4222
    prof_port: 0
    trace: false
    user: nats
  nfs_server:
    address: null
    allow_from_entries:
    - 10.0.16.0/24# NFS Mount 허용 Range 지정
    share: null
  request_timeout_in_seconds: 900
  router:
    cipher_suites: null
    debug_addr: null
    enable_ssl: null
    extra_headers_to_log: null
    logrotate: null
    port: null
    requested_route_registration_interval_in_seconds: null
    route_service_timeout: null
    route_services_secret: admin
    route_services_secret_decrypt_only: null
    secure_cookies: null
    ssl_cert: null
    ssl_key: null
    ssl_skip_validation: true
    status:
      password: admin
      port: null
      user: router
  smoke_tests: null
  ssl:
    skip_cert_verify: true
  support_address: http://support.cloudfoundry.com
  syslog_daemon_config: null
  system_domain: controller.open-paas.com# DNS Server에 등록한 Platform Domain Name
  system_domain_organization: OCP
  traffic_controller:
    outgoing_port: 8080
  uaa:
    admin:
      client_secret: admin# admin 계정 Password
    authentication:
      policy:
        countFailuresWithinSeconds: null
        lockoutAfterFailures: null
        lockoutPeriodSeconds: null
    batch:
      password: admin
      username: batchuser
    catalina_opts: -Xmx768m -XX:MaxPermSize=256m
    cc:
      client_secret: admin
    clients:
      cc-service-dashboards:
        authorities: clients.read,clients.write,clients.admin
        authorized-grant-types: client_credentials
        scope: openid,cloud_controller_service_permissions.read
        secret: admin
      cc_routing:
        authorities: routing.router_groups.read
        authorized-grant-types: client_credentials
        secret: admin
      cloud_controller_username_lookup:
        authorities: scim.userids
        authorized-grant-types: client_credentials
        secret: admin
      doppler:
        authorities: uaa.resource
        override: true
        secret: admin
      gorouter:
        authorities: clients.read,clients.write,clients.admin,routing.routes.write,routing.routes.read
        authorized-grant-types: client_credentials,refresh_token
        scope: openid,cloud_controller_service_permissions.read
        secret: admin
      login:
        authorities: oauth.login,scim.write,clients.read,notifications.write,critical_notifications.write,emails.write,scim.userids,password.write
        authorized-grant-types: authorization_code,client_credentials,refresh_token
        override: true
        redirect-uri: https://login.controller.open-paas.com
        scope: openid,oauth.approvals
        secret: admin
      notifications:
        authorities: cloud_controller.admin,scim.read
        authorized-grant-types: client_credentials
        secret: admin
      ssh-proxy:
        authorized-grant-types: authorization_code
        autoapprove: true
        override: true
        redirect-uri: /login
        scope: openid,cloud_controller.read,cloud_controller.write
        secret: admin
    database: null
    issuer: https://uaa.controller.open-paas.com
    jwt:
      signing_key: |
        -----BEGIN RSA PRIVATE KEY-----
        MIICXAIBAAKBgQDHFr+KICms+tuT1OXJwhCUmR2dKVy7psa8xzElSyzqx7oJyfJ1
        JZyOzToj9T5SfTIq396agbHJWVfYphNahvZ/7uMXqHxf+ZH9BL1gk9Y6kCnbM5R6
        0gfwjyW1/dQPjOzn9N394zd2FJoFHwdq9Qs0wBugspULZVNRxq7veq/fzwIDAQAB
        AoGBAJ8dRTQFhIllbHx4GLbpTQsWXJ6w4hZvskJKCLM/o8R4n+0W45pQ1xEiYKdA
        Z/DRcnjltylRImBD8XuLL8iYOQSZXNMb1h3g5/UGbUXLmCgQLOUUlnYt34QOQm+0
        KvUqfMSFBbKMsYBAoQmNdTHBaz3dZa8ON9hh/f5TT8u0OWNRAkEA5opzsIXv+52J
        duc1VGyX3SwlxiE2dStW8wZqGiuLH142n6MKnkLU4ctNLiclw6BZePXFZYIK+AkE
        xQ+k16je5QJBAN0TIKMPWIbbHVr5rkdUqOyezlFFWYOwnMmw/BKa1d3zp54VP/P8
        +5aQ2d4sMoKEOfdWH7UqMe3FszfYFvSu5KMCQFMYeFaaEEP7Jn8rGzfQ5HQd44ek
        lQJqmq6CE2BXbY/i34FuvPcKU70HEEygY6Y9d8J3o6zQ0K9SYNu+pcXt4lkCQA3h
        jJQQe5uEGJTExqed7jllQ0khFJzLMx0K6tj0NeeIzAaGCQz13oo2sCdeGRHO4aDh
        HH6Qlq/6UOV5wP8+GAcCQFgRCcB+hrje8hfEEefHcFpyKH+5g1Eu1k0mLrxK2zd+
        4SlotYRHgPCEubokb2S1zfZDWIXW3HmggnGgM949TlY=
        -----END RSA PRIVATE KEY-----
      verification_key: |
        -----BEGIN PUBLIC KEY-----
        MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDHFr+KICms+tuT1OXJwhCUmR2d
        KVy7psa8xzElSyzqx7oJyfJ1JZyOzToj9T5SfTIq396agbHJWVfYphNahvZ/7uMX
        qHxf+ZH9BL1gk9Y6kCnbM5R60gfwjyW1/dQPjOzn9N394zd2FJoFHwdq9Qs0wBug
        spULZVNRxq7veq/fzwIDAQAB
        -----END PUBLIC KEY-----

    ldap: null
    login: null
    newrelic: null
    no_ssl: null
    port: 8080
    require_https: null
    restricted_ips_regex: 10\.\d{1,3}\.\d{1,3}\.\d{1,3}|192\.168\.\d{1,3}\.\d{1,3}|169\.254\.\d{1,3}\.\d{1,3}|127\.\d{1,3}\.\d{1,3}\.\d{1,3}|172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}|172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}
    scim:
      external_groups: null
      groups: null
      userids_enabled: true
      users:
      #- admin|admin|scim.write,scim.read,openid,cloud_controller.admin,doppler.firehose,routing.router_groups.read,dashboard.user,console.admin,console.support
       - admin|admin|scim.write,scim.read,openid,cloud_controller.admin,clients.read,clients.write,doppler.firehose,routing.router_groups.read
    spring_profiles: null
    url: https://uaa.controller.open-paas.com
    user: null
    zones: null
  uaadb:
    address: 10.0.16.22# DB Server VM IP 주소
    databases:
    - citext: true
      name: uaadb
      tag: uaa
    db_scheme: postgresql
    port: 5524
    roles:
    - name: uaaadmin
      password: admin
      tag: admin
#### 3.4	Bosh Deploy
지금까지 설치를 위한 준비 과정이 정상적으로 수행되었으면, 지금부터 Open PaaS Controller를 IaaS 환경(AWS)에 아래의 절차로 설치한다.
##### 3.4.1	Deployment Manifest 지정
bosh deployment openpaas-controller-aws-1.0.yml
	“bosh deployment” 명령어로 생성한 Deployment Manifest File을 지정하고, 아래의 그림과 같이 동일한 명령어로 정상 지정 되었는지를 확인한다.
 
##### 3.4.2	Open PaaS Controller Deploy
“bosh deploy” 명령으로 Open PaaS Controller 설치를 수행한다.
bosh deploy
보통 설치 과정은 1-2시간 정도가 소요되며 정상적으로 설치가 완료되면 아래 그림과 같은 메세지를 출력하게 된다.
 
#### 3.5	설치형상 확인
설치가 정상적으로 완료된 후 “bosh vms” 명령으로 설치된 Open PaaS Controller의 형상을 확인한다.
bosh vms
아래 그림과 같이 Deployment Name, Virtual Machine, IP 주소 등의 정보를 확인할 수 있다.
 
