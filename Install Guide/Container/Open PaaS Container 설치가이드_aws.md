## Table of Contents
1. [개요](#1)
    * [1.1. 목적](#11)
    * [1.2. 범위](#12)
    * [1.3. 참고자료](#13)
2. [Prerequisites](#2)
    * [2.1. OpenPaas Controller 설치확인](#21)
3. [Open Paas Container 설치](#3)
    * [3.1. Release Upload](#31)
    * [3.2. Deployment Manifes](#32)
    * [3.2.1. Name & Release](#321)
    * [3.2.2. Networks](#322)
    * [3.2.3. Compilatio](#323)
    * [3.2.4. Resource Pools](#324)
    * [3.2.5. Update](#325)
    * [3.2.6. Jobs](#326)
    * [3.2.7. Properties](#327)
    * [3.3. Deployment Manifest 지정](#33)
    * [3.4. Bosh Deploy](#34)
    * [3.5. 설치형상 확인](#35)
4. [설치 검증](#4)
    * [4.1. CF Login](#41)
    * [4.2. Application Deploy](#42)
    * [4.3. Application Access](#43)
    
        
# <a name="1"/>1. 개요

### <a name="11"/>1.1. 목적

> 본 문서(설치가이드)는, 현 시점에서 지원되는 IaaS(Infrastructure as a
> Service) 중 하나인 AWS 환경에서 개방형클라우드플랫폼(OpenPaas
> Container) 을 설치하기 위한 가이드를 제공하는데 그 목적이 있다.

### <a name="12"/>1.2. 범위

> 본 문서의 범위는 개방형클라우드플랫폼을 AWS 기반에 설치하기 위한
> 내용으로 한정되어 있다. Openstack/VSphere와 같은 다른 IaaS 환경에서의
> 설치는 그에 맞는 가이드 문서를 참고해야 하며, Bosh/CF release 설치
> 또한 해당 가이드 문서를 별도로 참조해야 한다.

### <a name="13"/>1.3. 참고자료

-   [***https://github.com/cloudfoundry-incubator/diego-release***](https://github.com/cloudfoundry-incubator/diego-release)

# <a name="2"/>2. Prerequisites

### <a name="21"/>2.1. OpenPaas Controller 설치확인

> 개방형클라우드플랫폼 (OpenPaas Container) 를 설치하기 위해서는 사전에
> OpenPaas Controller가 설치되어 있어야 한다.
>
> 확인하는 방법은 bosh deployments를 통해 배포된 리스트 목록으로
> 확인한다.
>
> ![][container_aws_guide_01]

# <a name="3"/>3. Open Paas Container 설치

### <a name="31"/>3.1. Release Upload

> 배포된 설치 패키지의 OpenPaaS-Container 폴더에 있는 Open PaaS
> Container Bosh Release와 의존관계에 있는 garden-linux 및 etcd 를 Bosh
> Server로 아래와 같은 명령으로 Upload 한다.

  ````
  bosh upload release \$INSTALL\_PACKAGE/OpenPaaS-Container/garden-linux-0.329.0.tgz

  bosh upload release \$INSTALL\_PACKAGE/OpenPaaS-Container/etcd -release-20.tgz

  bosh upload release \$INSTALL\_PACKAGE/OpenPaaS-Container/openpaas-container -1.0.tgz
  ````

> Release Upload는 상황에 따라 다소 차이는 있으나 보통 20-30분 정도
> 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.
>
> ![](container_aws_guide_02)
>
> [주의] Release Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가
> 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB
> 이상 Free Size가 있는지를 확인해야 한다.
>
> Bosh Sever에 Release가 정상적으로 Upload 되었는지는 “bosh releases”
> 명령으로 확인한다.
````
  bosh releases
````

> ![](container_aws_guide_03)

### <a name="32"/>3.2. Deployment Manifest 파일 수정하기

> 배포된 설치 패키지에 포함된 Sample Deployment Manifest
> File(\$INSTALL\_PACKAGE/OpenPaaS-Deployment/openpaas-container-aws-1.0.yml)을
> 아래의 순서대로 설치환경에 적합하게 수정한다.

### <a name="321"/>3.2.1. Name & Release

  ````
  name: openpaas-container-aws- 1.0                       # Deployment Name

  director\_uuid: 3d139c62-6669-4804-adb0-990b16446c37    # Bosh Director UUID

  releases:

  - name: openpaas-container                              # container Release Name

  version: latest                                         # container Release Version

  - name: openpaas-controller                             # controller Release Name

  version: latest                                         # controller Release Version

  - name: garden-linux                                    # garden-linux Release Name

  version: latest                                         # garden-linux Release Version

  - name: etcd                                            # etcd Release Name

  version: latest                                         # etcd Release Version
  ````

> Deployment Name은 설치자가 임의로 부여하는데, IaaS와 Version을 표시할
> 것을 권장한다. Bosh Director UUID는 “bosh status” 명령을 실행하면
> 출력되는 UUID 값을 넣는다.
>
> ※ Controller, Container, Garden-linux, etcd의 Release Name과 Version은
> “bosh releases” 명령의 결과로 나오는 값들을 입력하도록 한다. 본
> 가이드에서는 각 하나의 release가 업로드 되어 있으므로 명시적 버전 대신
> 업로드 된 릴리즈 버전 중 최신 버전인 latest 로 지정하여 사용한다.

### <a name="322"/>3.2.2. Networks

  ````
  networks:

  - name: openpaas-container-network      # Platform이 설치될 Network Name

  subnets:

  - cloud\_properties:

  subnet: subnet-4676f47b                 # AWS Subnet ID

  security\_groups:                       # AWS Security Group Name

  - cf-security

  - bosh-security

  dns:

  - 8.8.8.8

  gateway: 10.0.80.1                      # Gateway IP Address

  range: 10.0.80.0/24                     # Network CIDR

  reserved:

  - 10.0.80.2 - 10.0.80.9

  static:

  - 10.0.80.10 - 10.0.80.40               # VM에 할당될 Static IP 주소 대역

  type: manual
  ````

Network Name은 설치자가 임의로 부여 가능하다. Network ID,
Security\_groups, Gateway, DNS Server, Network CIDR은 AWS 구성을 직접
확인하거나 인프라 담당자에게 문의하여 정보를 얻도록 한다. Static IP
주소는 Platform을 설치할 때 개별 VM에 할당될 IP의 주소 대역으로
마찬가지로 인프라 담당자에게 할당을 받아야 한다.

### <a name="323"/>3.2.3. Compilation

  ````
  compilation:

  cloud\_properties:                    # Compile시 사용할 VM 속성

  instance\_type: m1.medium

  network: openpaas-container-network   # 네트워크 설정에 사용된 것과 동일한 이름

  reuse\_compilation\_vms: true         # compilation VMs 재사용 여부

  workers: 6                            # 동시 동작하는 VM 수
  ````

Network Name은 3.2.2에서 정의한 것과 동일한 이름을 줘야 한다. Workers는
동시에 Compile을 수행하는 VM의 개수로 별다른 환경적 특성이 없다면
Default 값을 사용토록 한다.

### <a name="324"/>3.2.4. Resource Pools

  ````
  resource\_pools:

  - name: access\_z1                        # Resource Name

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.small

  network: openpaas-container-network       # Network Name

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent   # Stemcell Name

  version: latest                           # Stemcell Version

  - name: brain\_z1

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.small

  network: openpaas-container-network

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent

  version: latest

  - name: cc\_bridge\_z1

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.small

  network: openpaas-container-network

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent

  version: latest

  - name: cell\_z1

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.medium

  network: openpaas-container-network

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent      # Stemcell Name

  version: latest                           # Stemcell Version

  - name: database\_z1

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.small

  network: openpaas-container-network

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent

  version: 3147

  - name: route\_emitter\_z1

  cloud\_properties:                        # Resource 실행 VM 속성

  instance\_type: m1.small

  network: openpaas-container-network

  stemcell:

  name: bosh-aws-xen-ubuntu-trusty-go\_agent

  version: 3147
  ````

Resource pool 정보는 Jobs 영역에서 각 VM들이 사용하기 위한 Resource를
사전 정의한 영역으로, 각 VM 영역의 이름으로 명명되어 있으며, 필요 크기에
따라 cpu, disk, ram 정보를 수정한다. Stemcell Name과 Version은 “bosh
stemcells” 명령어 결과로 출력되는 값들을 입력하도록 한다.

### <a name="325"/>3.2.5. Update

  ````
  update:

  canaries: 1                           # Canary instance 개수

  canary\_watch\_time: 5000-120000      # Canary instance 의 health 상태 점검 대기 시간

  max\_in\_flight: 1                    # update instance들의 최대 병렬 처리 개수

  serial: false                         # VM 순차적 Update

  update\_watch\_time: 5000-120000      # canary instance 테스트 후 실제 instance update 하면서 health 상태 점검 대기 시간
  ````

Default 값들을 수정 없이 사용한다.

### <a name="326"/>3.2.6. Jobs

아래 Sample Jobs를 참고하여 설치 환경에 맞게 수정한다.

  ````
  jobs:

  - instances: 1                        # VM Instance 개수

  name: database\_z1                    # Job Name

  networks:

  - name: openpaas-container-network    # Network Name

  static\_ips:

  - 10.0.80.10                          # 서버 IP 지정

  persistent\_disk: 1024

  properties:

  consul:

  agent:

  services:

  bbs: {}

  etcd: {}

  metron\_agent:

  zone: z1

  resource\_pool: database\_z1          # Resource Name

  templates:

  - name: etcd                          # database VM에서 실행될 컴포넌트

  release: etcd                         # etcd job template이 존재하는 release 정보

  - name: bbs

  release: openpaas-container

  - name: consul\_agent

  release: openpaas-controller

  - name: metron\_agent

  release: openpaas-controller

  update:

  max\_in\_flight: 1                    # update instance들의 최대 병렬처리 개수

  serial: true                          # 순차적 실행여부

  - instances: 1

  name: brain\_z1

  networks:

  - name: openpaas-container-network

  static\_ips:

  - 10.0.80.11                          # brain 노드 ip 지정

  properties:

  consul:

  agent:

  services:

  auctioneer: {}

  metron\_agent:

  zone: z1                              # Zone 영역(설치되는 VM 그룹)

  resource\_pool: brain\_z1

  templates:

  - name: consul\_agent

  release: openpaas-controller

  - name: auctioneer

  release: openpaas-container

  - name: converger

  release: openpaas-container

  - name: metron\_agent

  release: openpaas-controller

  update:

  max\_in\_flight: 1

  serial: true

  - instances: 1

  name: cell\_z1

  networks:

  - name: openpaas-container-network

  static\_ips:

  - 10.0.80.12                             # cell 노드 ip 지정

  properties:

  diego:

  rep:

  zone: z1

  metron\_agent:

  zone: z1

  resource\_pool: cell\_z1

  templates:

  - name: rep

  release: openpaas-container

  - name: consul\_agent

  release: openpaas-controller

  - name: garden-linux

  release: openpaas-container

  - name: metron\_agent

  release: openpaas-controller

  update:

  max\_in\_flight: 1

  serial: false

  - instances: 1

  name: cc\_bridge\_z1

  networks:

  - name: openpaas-container-network

  static\_ips:

  - 10.0.80.13                              # cc\_bridge 노드 ip 지정

  properties:

  consul:

  agent:

  services:

  cc\_uploader: {}

  nsync: {}

  stager: {}

  tps: {}

  metron\_agent:

  zone: z1

  resource\_pool: cc\_bridge\_z1

  templates:

  - name: stager

  release: openpaas-container

  - name: nsync

  release: openpaas-container

  - name: tps

  release: openpaas-container

  - name: file\_server

  release: openpaas-container

  - name: consul\_agent

  release: openpaas-controller

  - name: metron\_agent

  release: openpaas-controller

  update:

  max\_in\_flight: 1

  serial: false

  - instances: 1

  name: route\_emitter\_z1

  networks:

  - name: openpaas-container-network

  static\_ips:

  - 10.0.80.14                                 # route\_emitter 노드 ip 지정

  properties:

  metron\_agent:

  zone: z1

  resource\_pool: route\_emitter\_z1

  templates:

  - name: route\_emitter

  release: openpaas-container

  - name: consul\_agent

  release: openpaas-controller

  - name: metron\_agent

  release: openpaas-controller

  update:

  max\_in\_flight: 1

  serial: false

  - instances: 1

  name: access\_z1

  networks:

  - name: openpaas-container-network

  static\_ips:

  - 10.0.80.16                                  # access 노드 ip 지정

  properties:

  consul:

  agent:

  services:

  file\_server: {}

  ssh\_proxy: {}

  metron\_agent:

  zone: z1

  resource\_pool: access\_z1

  templates:

  - name: ssh\_proxy

  release: openpaas-container

  - name: consul\_agent

  release: openpaas-controller

  - name: metron\_agent

  release: openpaas-controller

  - name: receptor

  release: openpaas-container

  update:

  max\_in\_flight: 1

  serial: false
  ````

### <a name="327"/>3.2.7. Properties

아래 Sample Manifest를 참조하여 설치 환경에 맞게 값을 수정한다.

  ````
  properties:

  consul:               # consul 속성 정의

  agent:

  datacenter: null

  log\_level: null

  servers:

  lan:

  - 10.0.16.16          # controller에서 정의한 consul의 static\_ips

  agent\_cert: |        # controller에서 정의한 consul의 agent\_cert

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  agent\_key: |         # controller에서 정의한 consul의 agent\_key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  ca\_cert: |           # controller에서 정의한 consul의 ca\_cert

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  encrypt\_keys:        # controller에서 정의한 consul의 encrypt\_keys

  - t66mLrBhJ5kpofLwoJpH5A==

  require\_ssl: true

  server\_cert: |       # controller에서 정의한 consul의 server\_cert

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  server\_key: |        # controller에서 정의한 consul의 server\_key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  diego:

  auctioneer:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+          # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+      # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+       # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null        # TLS client 캐시 크기

  max\_idle\_conns\_per\_host: null         # Idel http 요청 최대 수

  require\_ssl: true                        # bbs 통시 시 ssl 사용

  cell\_state\_timeout: null                # Cell State endpoint 에 HTTP 요청 시 타임아웃 시간(디폴트 1초)

  log\_level: null                          # 로그 레벨(디폴트 info)

  bbs:

  active\_key\_label: key1                  # database 쓰기 시 사용될 보안 키 명

  auctioneer:

  api\_url: http://auctioneer.service.cf.internal:9016      # Auctioneer API 주소

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  encryption\_keys:                         # 사용할 암호화 키

  - label: key1

  passphrase: DtFRfvja56znEDc0HRRTuw==

  etcd:

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # etcd-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # etcd-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  machines:

  - etcd.service.cf.internal

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  log\_level: null

  require\_ssl: true

  server\_cert: |+                          # bbs-certs/server.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  server\_key: |+                           # bbs-certs/server.key 

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  cc\_uploader:

  cc:

  base\_url: https://api.controller.open-paas.com

  basic\_auth\_password: admin

  staging\_upload\_password: admin

  staging\_upload\_user: staging\_upload\_user

  log\_level: null

  converger:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key 

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  log\_level: debug

  executor:

  disk\_capacity\_mb: null

  drain\_timeout\_in\_seconds: 0

  garden:

  address: 127.0.0.1:7777

  network: tcp

  log\_level: debug

  memory\_capacity\_mb: null

  file\_server:

  log\_level: null

  nsync:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  cc:

  base\_url: https://api.controller.open-paas.com

  basic\_auth\_password: admin

  staging\_upload\_password: admin

  staging\_upload\_user: staging\_upload\_user

  log\_level: debug

  rep:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key 

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  evacuation\_timeout\_in\_seconds: 60

  log\_level: debug

  route\_emitter:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  log\_level: debug

  nats:

  machines:

  - 10.0.16.11                              # controller에서 정의한 nats의 static\_ips

  password: admin                           # controller에서 정의한 nats의 password

  port: 4222

  user: nats                                # controller에서 정의한 nats의 user id

  ssh\_proxy:

  bbs:

  api\_location: bbs.service.cf.internal:8889       # BBS server 도메인 주소

  ca\_cert: |+                              # diego-ca.crt 인증서**

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  diego\_credentials: null

  enable\_cf\_auth: true

  enable\_diego\_auth: false

  host\_key: |+                             # ssh\_proxy 인증서. 호스트 식별을 위한 RSA 키

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  servers:

  - 10.0.80.31 **\# ssh\_proxy 서버 ip 지정**

  uaa\_secret: ssh-proxy-secret             # UAA에서 ssh-proxy 인증 시 사용할 OAuth client 비밀키

  uaa\_token\_url: https://uaa.controller.open-paas.com/oauth/token         # UAA 토큰 엔드 포인트

  ssl:

  skip\_cert\_verify: true

  stager:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  cc:

  base\_url: https://api.controller.open-paas.com

  basic\_auth\_password: admin              # controller에서 정의한 cc의 internal\_api\_password

  staging\_upload\_password: admin          # controller에서 정의한 cc의 staging\_upload\_password

  staging\_upload\_user: staging\_upload\_user  # controller에서 정의한 cc의 staging\_upload\_user

  insecure\_docker\_registry\_list: null

  log\_level: null

  tps:

  bbs:

  api\_location: bbs.service.cf.internal:8889

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # bbs-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # bbs-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  client\_session\_cache\_size: null

  max\_idle\_conns\_per\_host: null

  require\_ssl: true

  cc:

  base\_url: https://api.controller.open-paas.com

  basic\_auth\_password: admin

  staging\_upload\_password: admin

  staging\_upload\_user: staging\_upload\_user

  log\_level: null

  traffic\_controller\_url: wss://doppler.controller.open-paas.com:443

  etcd:

  ca\_cert: |+                              # diego-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_cert: |+                          # etcd-certs/client.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  client\_key: |+                           # etcd-certs/client.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  cluster:

  - instances: 1

  name: database\_z1

  - instances: 0

  name: database\_z2

  - instances: 0

  name: database\_z3

  - instances: 0

  name: colocated\_z1

  - instances: 0

  name: colocated\_z2

  - instances: 0

  name: colocated\_z2

  election\_timeout\_in\_milliseconds: null

  heartbeat\_interval\_in\_milliseconds: null

  machines:

  - etcd.service.cf.internal

  peer\_ca\_cert: |+                    # etcd-peer-ca.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  peer\_cert: |+                        # etcd-certs/peer.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  peer\_key: |+                         # etcd-certs/peer.key 

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  peer\_require\_ssl: true

  require\_ssl: true

  server\_cert: |+                      # etcd-certs/server.crt 인증서

  -----BEGIN CERTIFICATE-----

  -----END CERTIFICATE-----

  server\_key: |+                       # etcd-certs/server.key

  -----BEGIN RSA PRIVATE KEY-----

  -----END RSA PRIVATE KEY-----

  garden:

  allow\_host\_access: null

  allow\_networks: null

  deny\_networks:

  - 0.0.0.0/0

  disk\_quota\_enabled: null

  enable\_graph\_cleanup: true

  insecure\_docker\_registry\_list: null

  listen\_address: 0.0.0.0:7777

  listen\_network: tcp

  log\_level: debug

  mount\_btrfs\_loopback: null

  network\_mtu: null

  persistent\_image\_list:

  - /var/vcap/packages/rootfs\_cflinuxfs2/rootfs

  loggregator:

  etcd:

  machines:

  - 10.0.16.24                      # controller에서 정의한 etcd의 static\_ips

  loggregator\_endpoint:

  shared\_secret: admin             # controller에서 정의한 loggregator\_endpoint의 shared\_secret

  metron\_agent:

  deployment: openpaas-aws-1.0

  metron\_endpoint:

  shared\_secret: admin             # changeit: controller에서 정의한 metron\_endpoint의 shared\_secret

  nats:

  machines:

  - 10.0.16.11                      # controller에서 정의한 nats의 static\_ips

  password: admin                   # controller에서 정의한 nats의 password

  port: 4222

  user: nats                        # controller에서 정의한 nats의 user id

  syslog\_daemon\_config:

  address: null

  port: null
  ````

> 기본으로 주어진 인증서 대신 새로 인증서를 생성하기 위해서는 다음과
> 같은 절차를 따른다. Sample Manifest에서 사용한 인증서 생성 방법을 위해
> OpenPaaS-Container 폴더 하위의 diego-cert-gen.tgz 압축을 풀고, 다음
> 명령을 실행한다.

  ````
  \# tar cvzf diego-cert-gen.tgz

  \# cd diego-cert-gen

  \# ./keygen\_certs.sh
  ````

> 위 인증서 생성 명령을 실행 후 최종 인증서의 생성 위치는 다음과 같다.

-   diego-cert/diego-ca.key

-   diego-cert/etcd-peer-ca.crt

-   diego-cert/etcd-certs/peer.cert

-   diego-cert/etcd-certs/peer.key

-   diego-cert/etcd-certs/client.crt

-   diego-cert/etcd-certs/client.key

-   diego-cert/bbs-certs/peer.cert

-   diego-cert/bbs-certs/peer.key

-   diego-cert/bbs-certs/client.crt

-   diego-cert/bbs-certs/client.key

-   diego-certs/ssh\_proxy

> ssh\_proxy 접속을 위한 키(diego-certs/ssh\_proxy.fin)는 아래와 같이
> openpaas-controller의 manifest에 반영해야 한다.

  ````
  properties:

  …

  app\_ssh:

  host\_key\_fingerprint: 17:2b:92:3e:03:7b:6c:3a:31:31:1e:f7:49:63:24:b0       # diego-certs/ssh\_proxy.fin 파일 내용으로 치환

  oauth\_client\_id: ssh-proxy
  ````
### <a name="33"/>3.3. Deployment Manifest 지정

  ````    
  > bosh deployment openpaas-container-aws-1.0.yml
  ````

> “bosh deployment” 명령어로 생성한 Deployment Manifest File을 지정하고,
> 아래의 그림과 같이 동일한 명령어로 정상 지정 되었는지를 확인한다
>
> ![](container_aws_guide_04)

### <a name="34"/>3.4. Bosh Deploy

Diego module에 대한 bosh upload 과정이 끝났으면, deploy 과정을 통해
Diego 관련 VM을 생성한다.

  ````
  $ bosh deploy
  ````

> ![](container_aws_guide_05)
>
> ![](container_aws_guide_06)
>
> **[그림 : bosh deploy 실행 결과]**

### <a name="35"/>3.5. 설치형상 확인

> 설치가 정상적으로 완료된 후 “bosh vms” 명령으로 설치된 Platform의
> 형상을 확인한다.

  ````
  bosh vms
  ````

> 아래 그림과 같이 Deployment Name, Virtual Machine, IP 주소 등의 정보를
> 확인할 수 있다.
>
> ![](container_aws_guide_07)

### <a name="36"/>3.6. Trobleshooting

> Container 파일을 Deploy를 한 후 다음 사진과 같이 “database\_z1 &gt;
> database\_z1/0 (canary). Failed: Volume”이라는 에러가 발생하는 경우
> 사용하고 있는 OpenStack에 접속하여 리소스가 부족하지 않은지 확인해
> 보고 필요하지 않은 것들은 Delete를 해 준다.
>
> ![](container_aws_guide_08){width="6.686111111111111in"
> height="2.9770833333333333in"}
>
> Container 파일을 Deploy를 한 후 다음 사진과 같이 “database\_z1 &gt;
> database\_z1/0 (canary). Failed: ‘database\_z1/0’ is not running after
> update”라는 에러가 발생하는 경우가 있다.
>
> ![](container_aws_guide_09){width="6.697916666666667in"
> height="3.8020833333333335in"}
>
> 다음과 같이 bosh ssh를 통해 database\_z1/0에 접근한다. “Choose an
> instance”에서 database\_z1/0을 선택하면 된다.
>
> ![](container_aws_guide_10){width="6.697915573053368in" height="1.21875in"}
>
> 다음 그림과 같이 sudo su를 통해 접속하면 ‘etcd’가 ‘not monitored’
> 상태인 것을 확인할 수 있다.
>
> ![](container_aws_guide_11){width="6.674305555555556in"
> height="7.988194444444445in"}
>
> monit summary를 통해 먼저 프로세스의 상태를 확인한다. 프로세스를
> 없애고 다시 상태를 확인한다. 다음의 명령어들을 하나씩 실행하면서
> 프로세스의 상태를 확인한다.

  ````
  monit summary

  killall etcd

  ps –ef | grep etcd

  kill -9 pid

  monit quit etcd

  monit summary
  ````
  
> monit quit etcd까지 다 실행하고 monit summary를 실행하면 ‘etcd’가
> running으로 바뀐 것을 볼 수 있다.
>
> ![](container_aws_guide_12){width="6.6875in" height="2.78125in"}
>
> ‘etcd’를 확인한 후 종료하고 OpenStack 서버에서 다음과 같은 명령어를
> 실행하면 database\_z1/0가 running 상태인 것을 확인할 수 있다.
>
> ![](container_aws_guide_13){width="4.825694444444444in"
> height="2.953472222222222in"}

# <a name="4"/>4. 설치 검증

### <a name="41"/>4.1. CF Login

  ````
  cf api [***http://api.115.68.46.29.xip.io***](http://api.115.68.46.29.xip.io) --skip-ssl-validation       # cf target 지정

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
  ````

CF Target을 지정하고, Login을 수행한다. 이 때 계정은 admin/admin을
사용한다.

Application을 Deploy할 ORG와 Space를 생성하고, 해당하는 ORG/Space로
Targetting 한다.

** admin 계정의 패스워드 설정을 바꾸고 싶다면, CF-Release deploy시
manifest 설정 파일에서 변경하야 한다.**

### <a name="42"/>4.2. Application Deploy

> 개방형클라우드플랫폼 패키지와 함께 배포된 Sample Application이
> 위치하는 디렉토리로 이동하고 Application을 Deploy 한다.

  ````
  cd \$PACKAGE\_ROOT/apps/hello-java

  cf push “application-name” –i “instance\_count” –m “memory\_size”
  ````

-   Application 배포시 Disk 관련 옵션 (-k)을 지정하지 않은 경우에는
    기본적으로 6G 크기의 디스크 사용량이 지정된다.

![](container_aws_guide_14)
![](container_aws_guide_15)
![](container_aws_guide_16)

### <a name="43"/>4.3. Application Access

> Deploy한 Application URL을 Browser 또는 curl 명령어로 Access하여 정상
> 접근 되는지를 확인한다.
>
> 사용법) 배포된 App URL이 hello-spring-test.controller.open-paas.com 일
> 경우
>
> curl –L http:// hello-spring-test.controller.open-paas.com

[container_aws_guide_01]:/images/openpaas-container/container_aws_guide_01.png
[container_aws_guide_02]:/images/openpaas-container/container_aws_guide_02.png
[container_aws_guide_03]:/images/openpaas-container/container_aws_guide_03.png
[container_aws_guide_04]:/images/openpaas-container/container_aws_guide_04.png
[container_aws_guide_05]:/images/openpaas-container/container_aws_guide_05.png
[container_aws_guide_06]:/images/openpaas-container/container_aws_guide_06.png
[container_aws_guide_07]:/images/openpaas-container/container_aws_guide_07.png
[container_aws_guide_08]:/images/openpaas-container/container_aws_guide_08.png
[container_aws_guide_09]:/images/openpaas-container/container_aws_guide_09.png
[container_aws_guide_10]:/images/openpaas-container/container_aws_guide_10.png
[container_aws_guide_11]:/images/openpaas-container/container_aws_guide_11.png
[container_aws_guide_12]:/images/openpaas-container/container_aws_guide_12.png
[container_aws_guide_13]:/images/openpaas-container/container_aws_guide_13.png
[container_aws_guide_14]:/images/openpaas-container/container_aws_guide_14.png
[container_aws_guide_15]:/images/openpaas-container/container_aws_guide_15.png
[container_aws_guide_16]:/images/openpaas-container/container_aws_guide_16.png



