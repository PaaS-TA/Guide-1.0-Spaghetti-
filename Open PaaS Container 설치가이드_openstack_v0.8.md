## Table of Contents
1. [개요](#개요)
     * [문서 목적](#문서-목적)
     * [범위](#범위)
     * [참고 자료](#참고-자료)
2. [Prerequisites](#prerequisites)
    * [OpenPaas Controller 설치확인](#openpaas-controller-설치확인)
3. [Open Paas Container 설치](#open-paas-container-설치)
   * [Release Upload](#release-upload)
4. [Deployment Manifest 파일 수정하기](#deployment-manifest-파일-수정하기). 
   * [Name & Release](#name-release)
   * [Compilation](#compilation)
   * [Resource Pools](#resource-pools)
   * [Update](#update)
   * [Jobs](#jobs)
   * [Properties](#properties)
5. [Deployment Manifest 지정](#deployment-manifest-지정)
6. [Bosh Deploy](#bosh-deploy)
7. [설치형상 확인](#설치형상-확인)
7. [설치 검증](#설치-검증)
   * [CF Login](#cf-login)
   * [Application Deploy](#application-deploy)
   * [Application Access](#application-access)

## 개요
---

#### 문서 목적

 본 문서(설치가이드)는, 현 시점에서 지원되는 IaaS(Infrastructure as a Service) 중 하나인 Openstack 환경에서 개방형클라우드플랫폼(OpenPaas Container) 을 설치하기 위한 가이드를 제공하는데 그 목적이 있다.

#### 범위

 본 문서의 범위는 개방형클라우드플랫폼을 Openstack 기반에 설치하기 위한 내용으로 한정되어 있다. VMWare/AWS와 같은 다른 IaaS 환경에서의 설치는 그에 맞는 가이드 문서를 참고해야 하며, Bosh/CF release 설치 또한 해당 가이드 문서를 별도로 참조해야 한다.

#### 참고 자료

 [**https://github.com/cloudfoundry-incubator/diego-release**](https://github.com/cloudfoundry-incubator/diego-release)

 [**http://docs.cloudfoundry.org/deploying/openstack/**](http://docs.cloudfoundry.org/deploying/openstack/)


## Prerequisites

#### OpenPaas Controller 설치확인

 개방형클라우드플랫폼 (OpenPaas Container) 를 설치하기 위해서는 사전에 OpenPaas Controller가 설치되어 있어야 한다.

 확인하는 방법은 bosh deployments를 통해 배포된 리스트 목록으로 확인한다.
 
 ![cf push](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/bosh_deployments_openstack.png)

## Open Paas Container 설치

#### Release Upload

 배포된 설치 패키지의 OpenPaaS-Controller 폴더에 있는 Open PaaS Container Bosh Release를 Bosh Server로 아래와 같은 명령으로 Beta-1.0 버전을 Upload 한다.

| bosh upload release $INSTALL\_PACKAGE/OpenPaaS-Container/ openpaas-container-beta-1.0.tgz |
|-------------------------------------------------------------------------------------------|

 Release Upload는 상황에 따라 다소 차이는 있으나 보통 20-30분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.
 
 ![OpenPaas Container Upload Release](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/bosh_upload_release.png)

 \[주의\] Release Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

 Bosh Sever에 Release가 정상적으로 Upload 되었는지는 “bosh releases” 명령으로 확인한다.


| bosh releases |
|---------------|

 ![OpenPaas Container Releases](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/bosh_releases.png)

## Deployment Manifest 파일 수정하기

 배포된 설치 패키지에 포함된 Sample Deployment Manifest File($INSTALL\_PACKAGE/OpenPaaS-Deployment/openpaas-container-openstack-beta-1.0.yml)을 아래의 순서대로 설치환경에 적합하게 수정한다.

#### Name & Release

```
 name: openpaas-container-openstack                    # Deployment Name
 director\_uuid: 6e0f7c41-2415-4319-98aa-38109597aff4  # Bosh Director UUID
 releases:
   - name: openpaas-container         # container Release Name
     version: latest                  # container Release Version
   - name: openpaas                   # controller Release Name
     version: latest                  # controller Release Version 

```
 Deployment Name은 설치자가 임의로 부여하는데, IaaS와 Version을 표시할 것을 권장한다. Bosh Director UUID는 “bosh status” 명령을 실행하면 출력되는 UUID 값을 넣는다.
 
 ※ container & controller Release Name과 Version은 “bosh releases” 명령의 결과로 나오는 값들을 입력하도록 한다.

#### Networks

```
 networks:
   - name: openpaas-container-network                    # Platform이 설치될 Network Name
   subnets:
   - cloud_properties:
       net_id: 06c2b5f8-55a7-4893-9901-861ed2d03ad4      # 네트워크 ID
       security_groups:
       - bosh_security                                   # Security_Group
   dns:
   - 10.10.5.108                                         # DNS Server
   - 8.8.8.8
   gateway: 10.10.9.1                                    # Gateway IP Address
   range: 10.10.9.0/24                                   # Network CIDR
   static:
   - 10.10.9.10 - 10.10.9.30                             # VM에 할당될 Static IP 주소 대역
   type: manual 

```

 Network Name은 설치자가 임의로 부여 가능하다. Network ID, Security\_groups, Gateway, DNS Server, Network CIDR은 Openstack 구성을 직접 확인하거나 인프라 담당자에게 문의하여 정보를 얻도록 한다. Static IP 주소는 Platform을 설치할 때 개별 VM에 할당될 IP의 주소 대역으로 마찬가지로 인프라 담당자에게 할당을 받아야 한다.

#### Compilation:

```
compilation:
  cloud_properties:
    instance_type: m1.medium              # Openstack에서 설정한 Flavor 정보
  network: openpaas-container-network     # 네트워크 설정에 사용된 것과 동일한 이름 
  reuse_compilation_vms: true             # compilation VMs 재사용 여부
workers: 6                              

```

Network Name은 3.3.2에서 정의한 것과 동일한 이름을 줘야 한다. Workers는 동시에 Compile을 수행하는 VM의 개수로 별다른 환경적 특성이 없다면 Default 값을 사용토록 한다.

#### Resource Pools

```
resource_pools:
- cloud_properties:
    instance_type: m1.small                             # Openstack Flavor 정보
  name: access                                          # Resource Name
  network: openpaas-container-network                   # Network Name 
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent     # Stemcell Name
    version: latest                                     # Stemcell Version
- cloud_properties: 
    instance_type: m1.small
  name: brain
  network: openpaas-container-network
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest
- cloud_properties: 
    instance_type: m1.small
  name: cc_bridge
  network: openpaas-container-network
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest
- cloud_properties: 
    instance_type: m1.medium                            # Cell 영역의 VM은 기본적으로 5G Disk
  name: cell                                            # 를 필요로하기 때문에 App 배포 영역을
  network: openpaas-container-network                   # 고려하여 최소 10G 이상의 Flavor 를
  stemcell:                                             # 선택해야 한다. 
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest
- cloud_properties: 
    instance_type: m1.medium
  name: etcd
  network: openpaas-container-network
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest
- cloud_properties: 
    instance_type: m1.small
  name: route_emitter
  network: openpaas-container-network
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest

```

Resource pool 정보는 Jobs 영역에서 각 VM들이 사용하기 위한 Resource를 사전 정의한 영역으로, 각 VM 영역의 이름으로 명명되어 있으며, 필요 크기에 따라 instance\_type에 설정된 Openstack Flavor 정보를 수정한다. Stemcell Name과 Version은 “bosh stemcells” 명령어 결과로 출력되는 값들을 입력하도록 한다.


#### Update

```
update:
  canaries: 1                              # Canary instance 개수
canary_watch_time: 30000-600000            # Canary instance 의 healthy 여부 선언까지 대기하는 시간 
  max_in_flight: 1                         # update instance들의 최대 병렬처리 개수 
  serial: true	                           # VM의 순차적 Update
update_watch_time: 5000-600000             # canary instance 테스트 후 실제 instance update 하면서 healthy 여부 선언까지 대기하는 시간

```
  Default 값들을 수정 없이 사용한다.

#### Jobs
  아래 Sample Jobs를 참고하여 설치 환경에 맞게 수정한다.

```
jobs:
- instances: 1                                      # VM Instance 개수
  name: etcd                                        # Job Name
  networks:
  - name: openpaas-container-network                # Network Name
    static_ips:
    - 10.10.9.12                                    # Job(etcd) VM에 할당할 IP 주소
  persistent_disk: 1024
  resource_pool: etcd                               # Resource Name
  templates:
  - name: etcd                                      # etcd VM에 실행될 컴포넌트 (job template정보)
    release: openpaas-container                     # etcd job template이 존재하는 release 정보
  update:
    max_in_flight: 1                                # update instance들의 최대 병렬처리 개수
    serial: true                                    # 순차적 실행여부
- instances: 1
  name: brain
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.10.9.16
  properties:
    metron_agent:
      zone: z1                                      # Zone 영역(설치되는 VM 그룹)
  resource_pool: brain
  templates:
  - name: consul_agent
    release: openpaas
  - name: auctioneer
    release: openpaas-container
  - name: converger
    release: openpaas-container
  - name: runtime_metrics_server
    release: openpaas-container
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1
    serial: true
- instances: 1
  name: cell
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.10.9.18
  properties:
    diego:
      rep:
        zone: z1
    metron_agent:
      zone: z1
  resource_pool: cell
  templates:
  - name: rep
    release: openpaas-container
  - name: consul_agent
    release: openpaas
  - name: garden-linux
    release: openpaas-container
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1
    serial: false
- instances: 1
  name: cc_bridge
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.10.9.20
  properties:
    consul:
      agent:
        services:
        - file_server
        - nsync
        - stager
        - tps
    metron_agent:
      zone: z1
  resource_pool: cc_bridge
  templates:
  - name: stager
    release: openpaas-container
  - name: nsync
    release: openpaas-container
  - name: tps
    release: openpaas-container
  - name: file_server
    release: openpaas-container
  - name: consul_agent
    release: openpaas
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1
    serial: false
- instances: 1
  name: route_emitter
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.10.9.22
  properties:
    metron_agent:
      zone: z1
  resource_pool: route_emitter
  templates:
  - name: route_emitter
    release: openpaas-container
  - name: consul_agent
    release: openpaas
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1
    serial: false
- instances: 1
  name: access
  networks:
  - name: openpaas-container-network
    static_ips:
    - 10.10.9.14
  properties:
    consul:
      agent:
        services:
        - receptor
        - ssh_proxy
    metron_agent:
      zone: z1
  resource_pool: access
  templates:
  - name: ssh_proxy
    release: openpaas-container
  - name: consul_agent
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: receptor
    release: openpaas-container
  update:
    max_in_flight: 1
    serial: false

```

#### Properties

아래 Sample Manifest를 참조하여 설치 환경에 맞게 값을 수정한다.

```
properties:
  consul:                                   # consul 속성 정의
    agent:
      log_level: null
      servers:
        lan:
        - 10.10.3.50                        # openpaas controller에 생성된 consul server IP
  diego:
    auctioneer:                             # auctioneer 속성 정의
      etcd:
        machines:
        - 10.10.9.12                        # etcd 서버 정보
      log_level: null
    converger:                              # converger 속성 정의
      etcd:
        machines:
        - 10.10.9.12         
      log_level: debug
    etcd:                                   # etcd 속성 정의
      machines:
      - 10.10.9.12
    executor:                               # executor 속성 정의
      allow_privileged: null                # privilege 속성
      drain_timeout_in_seconds: 0           # deprecated property 
      garden:
        address: 127.0.0.1:7777             # garden server 정보
        network: tcp
      log_level: debug
    file_server:
      cc:
        base_url: http://api.controller.open-paas.com      # cloud controller url 정보
        basic_auth_password: admin                         # cloud controller 접근 패스워드 
        external_port: 9022                                # cloud controller 접근 포트 
        staging_upload_password: admin                     # staging upload 시 접근 패스워드
        staging_upload_user: staging_upload_user           # staging upload 시 접근 계정
      log_level: null
    garden-linux:
      allow_networks: null                                 # 접근 허용할 CIDR 목록
      disk_quota_enabled: false                            # container에 대한 disk 한도 적용여부
      insecure_docker_registry_list: null                  # private docker registry url 정보
      kernel_network_tuning_enabled: false          
      listen_address: 0.0.0.0:7777                         # garden 서버 정보
      listen_network: tcp
    nsync:
      cc:
        base_url: http://api.controller.open-paas.com      # file_server 속성 참조
        basic_auth_password: admin
        external_port: 9022
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      diego_api_url: http://:@receptor.service.consul:8888 # receptor 서버 정보
      etcd:
        machines:
        - 10.10.9.12
      log_level: debug
    receptor:
      cors_enabled: null
      domain_names:
      - receptor.cf.open-pass.com.xip.io                   # receptor 도메인 정보
      etcd:
        machines:
        - 10.10.9.12                                       # etcd 서버 정보
      log_level: debug
      nats:
        machines:
        - 10.10.3.11                                       # openpaas controller에 생성된 nats vm ip
        password: admin                                    # nats 서버 접근 패스워드
        port: 4222                                         # nats 서버 접근 포트
        username: nats                                     # nats 서버 접근 계정
      password: ""                                         # receptor 서버 접근 패스워드
      register_with_router: true                           # receptor 서버 router에 등록여부
      username: ""                                         # receptor 서버 접근 계정
    rep:
      etcd:
        machines:
        - 10.10.9.12
      log_level: debug
    route_emitter:
      diego_api_url: http://:@receptor.service.consul:8888
      log_level: debug
      nats:
        machines:
        - 10.10.3.11                                       # openpaas controller에 생성된 nats vm ip
        password: admin
        port: 4222
        username: nats
    runtime_metrics_server:
      diego_api_url: http://:@receptor.service.consul:8888
      etcd:
        machines:
        - 10.10.9.12
      log_level: null
      nats:
        machines:
        - 10.10.3.11                                       # openpaas controller에 생성된 nats vm ip
        password: admin
        port: 4222
        username: nats
    ssh_proxy:
      cc:
        external_port: 9022
      diego_api_url: http://:@receptor.service.consul:8888
      enable_cf_auth: true                                 # openpaas controller application에 ssh 접근 허용 여부
      enable_diego_auth: true                              # openpaas container application에 ssh 접근 허용 여부
      host_key: |+                                         # ssh_proxy host key
        -----BEGIN RSA PRIVATE KEY-----
        MIIEhgIBAAKB/DMF5qOW+fh608KhX7qBLNHHmfzCfOONd176Oaf8rGht5KdnoNge
        TYSGqBFuYB1r1RbYEVhWAkH/8mW14XRVNmQ4C9eQDFqeWmmaOoSBG5GdP5GUfhI/
        z5vprQw+rnV4gt4InCA7QaR86pLj5sMiUij5OE/CW0dw29+z5E0p5WnQX5+utRmw
        ioQJD8jUDvzFrvzKIdE0HVOEl0agbeXq8U2e9E1de4iR+NiDc1zeiQmDNCIhFJb4
        FL7WqqokL+49SwSWGmOFKAlpj4Dlhx5dDwJWpcDe0XBXCkfcXn8xXNOT+4YBxJUG
        idNMPpLKpDUphZRj8CNBSMkjehIKVwIDAQABAoH8MiCAAQQYvXfeh36HT/IMmGSi
        8mIY1G5tclAfSNzCfS5Jz/XNXcYXnjW09LsdjoocJX9NOx30xeawvCA+SU5WS4uM
        htEscfLVHJ67EubMsPhuNZZPbZpnWuPucPM77ojg+UY4LKpKyVE4G+vvEJKtaTe/
        jQyDJOLKATL4/p5DtbDH7hVZcJVHU94csiE9a9OtyAvSwZLmNxGIBHshFntjcI+/
        hmQSFl3d1iduYGx7oeq3wX0sQ1mk/QksUTHRrlLfSQhLi5ZmH9Hnn/Qw2WeXKVdk
        BvXAUBiHG7Y0qGHXl5FOkB1BSlmk/EOkBk6gWl1a1Kx4A6oyNL4+HsuBAn572PqW
        IDutj4shf8ysI5fLJnvGCygZmk8LPZIlZZqLpDGo+l4iF3VCsd8CU2jKfWqel8+Q
        axdmu/BrQ7xyuWpxoHtKICv+CitI1ivzeYQwRCmjIN84jeGP9Pty4AJzhySegf/h
        n3irIp07wEzdedoj4A3RWWObX+AeubyUqfcCfml3scNb2oBK24RDVGYaUSWkSHBe
        OEU0QlOaJXZ2kCK2rIK/IVI7cD12WpkWTGY782VBmipEXwtMTprQzMrnK25shS+z
        AjCDGXtqr0GjxJh73WRurs1dVk6sqslSp1M/R9fmjGU4vdYL2JfMczEH4+57aOpR
        sW+H0FEYDayKoQJ+Eo8gdjDcYJT7N4jsRfuLesEImVQArV2HbNrMNNh2AWkYnAbw
        5lD3nIgFMFcJhBapTJzZWP4DYrzVOW3MJrEMd3yiHSiXDxm9BMw7h9/05DrCtpRt
        fw8b9zOyHrPdCiz9WteGXexE6/hi8ZpOqn3hJ7EiwPWRTK5gappQ3UJfAn4Tr0t2
        cwZtO4uNPCPcirzqkacTkgJeqEpY4ERtv+NXF1FLdfD6MC3ayuRN/mN0EWx0UbI8
        gVZb/XoOWzpeBJeOnKKfLIIUG+P9rQPY9IAVFclUnXPy0KDzPjcCLHMejokSOu2p
        VtXXxY4/huFZHWflcxM56NV9Q5QWDq8+rQECfjQTbNbd4ehbC/Q5EZ1SIzeaSLrn
        0ICmiRajnISbje5vPntqPXjBkbiVGx31qOaZ+DlGGLOyzW/GP5X4NOUwza2bYh3q
        nnzwBhoGLZfvoes5Nw06leOdVqcvIjLIDhb+XbiiEeAnONUp+BAKzDYOIp7K+LPe
        1rHeshh0P/QfCQ==
        -----END RSA PRIVATE KEY-----
      servers:
      - 10.10.9.14                                         # ssh_proxy 서버 정보
    ssl:
      skip_cert_verify: true                  
    stager:
      cc:
        base_url: http://api.controller.open-paas.com
        basic_auth_password: admin
        external_port: 9022
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      diego_api_url: http://:@receptor.service.consul:8888
      insecure_docker_registry: false
      log_level: null
    tps:
      cc:
        base_url: http://api.controller.open-paas.com
        basic_auth_password: admin
        external_port: 9022
        staging_upload_password: admin
        staging_upload_user: staging_upload_user
      diego_api_url: http://:@receptor.service.consul:8888
      log_level: null
      traffic_controller_url: wss://doppler.cf.open-paas.com:443
  etcd:
    machines:
    - 10.10.3.24                                           # openpaas controller에 생성된 etcd vm ip
  loggregator_endpoint:
    shared_secret: admin
  metron_agent:
    deployment: openpaas-container-openstack
  nats:
    machines:
    - 10.10.3.11                                           # openpaas controller에 생성된 nats vm ip
    password: admin
    port: 4222
    user: nats
  syslog_daemon_config:
    address: null
    port: null

```

## Deployment Manifest 지정

| bosh deployment openpaas-container-openstack-beta-1.0.yml |
|-----------------------------------------------------------|

 “bosh deployment” 명령어로 생성한 Deployment Manifest File을 지정하고, 아래의 그림과 같이 동일한 명령어로 정상 지정 되었는지를 확인한다
 
  ![OpenPaas Container deployment manifest file](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container//bosh_deployment_openstack.png)

## Bosh Deploy

  Diego module에 대한 bosh upload 과정이 끝났으면, deploy 과정을 통해 Diego 관련 VM을 생성한다.

| bosh deploy |
|-------------|

 ![OpenPaas Container deploy](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/bosh_deploy_openstack.png)

 \[그림 : bosh deploy 실행 결과\]


## 설치형상 확인

 설치가 정상적으로 완료된 후 “bosh vms” 명령으로 설치된 Platform의 형상을 확인한다.

| bosh vms |
|----------|

 아래 그림과 같이 Deployment Name, Virtual Machine, IP 주소 등의 정보를 확인할 수 있다.

 ![OpenPaas Container vms](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/bosh_vms_openstack.png)

##  설치 검증

#### CF Login

```
cf target api.controller.open-paas.com

…

cf login

Email&gt; admin

Password&gt; admin

OK

…

cf create-org open-paas

cf target -o open-paas

cf create-space dev

cf target -o open-paas -s dev

… 

```


 CF Target을 지정하고, Login을 수행한다. 이 때 계정은 admin/admin을 사용한다.
 Application을 Deploy할 ORG와 Space를 생성하고, 해당하는 ORG/Space로 Targetting 한다.

 ※ admin 계정의 패스워드 설정을 바꾸고 싶다면, CF-Release deploy시 manifest 설정 파일에서 변경하야 한다.

#### Application Deploy

 개방형클라우드플랫폼 패키지와 함께 배포된 Sample Application이 위치하는 디렉토리로 이동하고 Application을 Deploy 한다.

```
cd $PACKAGE\_ROOT/apps/hello-java

cf push “application-name” –i “instance\_count” –m “memory\_size”

```

 Application 배포시 Disk 관련 옵션 (-k)을 지정하지 않은 경우에는 기본적으로 1G 크기의 디스크 사용량이 지정된다.
 
  ![cf push](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/cf_push_openstack.png)
  
  ![cf apps](https://github.com/OpenPaaSRnD/Documents/blob/master/images/openpaas-container/cf_apps_openstack.png)

#### Application Access

 Deploy한 Application URL을 Browser 또는 curl 명령어로 Access하여 정상 접근 되는지를 확인한다.
 
 사용법) 배포된 App URL이 spring-music.controller.open-paas.com 일 경우

```
curl –L http://spring-music.controller.open-paas.com

```
