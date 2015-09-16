## Table of Contents
1. [문서 개요](#문서-개요)
     * [1.1. 목적](#목적)
     * [1.2. 범위](#범위)
     * [1.3. 시스템 구성도](#시스템-구성도)
     * [1.4. 참고자료](#참고자료)
2. [MySQL 서비스팩 설치](#mysql-서비스팩-설치)
     * [2.1. 설치전 준비사항](#설치전-준비사항)
     * [2.2. MySQL 서비스 릴리즈 업로드](#mysql-서비스-릴리즈-업로드)
     * [2.3. MySQL 서비스 Deployment 파일 수정 및 배포](#mysql-서비스-deployment-파일-수정-및-배포)
     * [2.4. MySQL 서비스 브로커 등록](#mysql-서비스-브로커-등록)
3. [MySQL 연동 Sample Web App 설명](#mysql-연동-sample-web-app-설명)
     * [3.1. Sample Web App 구조](#sample-web-app-구조)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#개방형-클라우드-플랫폼에서-서비스-신청)
     * [3.3. Sample Web App에 서비스 바인드 신청 및 App 확인](#sample-web-app에-서비스-바인드-신청-및-app-확인)
4. [MySQL Client 툴 접속](#mysql-client-툴-접속)
     * [4.1. HeidiSQL 설치 및 연결](#heidisql-설치-및-연결)


# 문서 개요

### 목적
      
본 문서(MySQL 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 MySQL 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 MySQL 서비스를 사용하는 방법을 기술하였다.


### 범위 

설치 범위는 MySQL 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

###시스템 구성도
본 문서의 설치된 시스템 구성도이다. MySQL Server, MySQL 서비스 브로커, Proxy로 최소사항을 구성하였다.
![시스템구성도][mysql_bosh_lite_1.3.01]

### 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)


# MySQL 서비스팩 설치

### 설치전 준비사항 
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH-lite 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH-lite 가 설치 되어 있지 않을 경우 먼저 BOSH-lite 설치 가이드 문서를 참고 하여BOSH-lite를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

### MySQL 서비스 릴리즈 업로드
-   OpenPaaS-Services.zip파일 압축을 풀고 폴더안에 있는 MySQL 서비스 릴리즈 openpaas-mysql-release-beta-1.0.tgz 파일을복사한다.<br>
업로드할 openpaas-mysql-release-beta-1.0.tgz 파일을 확인한다.<br>
>$ls –all<br>
>![mysql_bosh_lite_2.2.01]


-	업로드 되어 있는 릴리즈 목록을 확인한다.
>$bosh releases<br>
>![mysql_bosh_lite_2.2.02]<br>
>Mysql 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인


-	MySQL 서비스 릴리즈 파일을 업로드한다.
>$ bosh upload release {서비스 릴리즈 파일 PATH}<br>
>$ bosh upload release openpaas-mysql-release-beta-1.0.tgz<br>
>![mysql_bosh_lite_2.2.03]<br>
>![mysql_bosh_lite_2.2.04]<br>
>![mysql_bosh_lite_2.2.05]<br>
>![mysql_bosh_lite_2.2.06]<br>
>![mysql_bosh_lite_2.2.07]


-	업로드 된MySQL 릴리즈를 확인한다.
>$bosh releases<br>
>![mysql_bosh_lite_2.2.08]<br>
>Mysql 서비스 릴리즈가 업로드 되어 있는 것을 확인

### MySQL 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML 파일이다.<br>
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 lite용 MySQL Deployment 화일인 openpaas-mysql-lite.yml를복사한다.<br>
다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-mysql-lite.yml)
>$ls –all<br>
>![mysql_bosh_lite_2.3.01]

-	Director UUID를 확인한다.<br>
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할 수 있다.
>$bosh status<br>
>![mysql_bosh_lite_2.3.02]

-	Deploy시 사용할 Stemcell을 확인한다. (Stemcell 2776 버전 사용)
>$bosh stemcells<br>
>![mysql_bosh_lite_2.3.03]<br>
>Stemcell 목록이 존재 하지 않을 경우 BOSH-lite 설치 가이드 문서를 참고 하여 Stemcell 2776 버전을 업로드를 해야 한다.

-	openpaas-mysql-lite.yml Deployment 파일을 서버 환경에 맞게 수정한다.(굵은 글씨로 표시된 부분만 수정)

>$vi openpaas-mysql-lite.yml
><pre>#openpaas-mysql-lite 설정 파일 내용
compilation:           # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:      # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    name: random
  network: services1        # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true        # 컴파일지 VM 재사용 여부(옵션)
  workers: 6         # 컴파일 하는 가상머신의 최대수(필수)
director_uuid: <b>xxxxx#bosh status 에서 확인한 Director UUID을 입력(필수)</b>
disk_pools:
- cloud_properties: {}
  disk_size: 100000               # 디스크 사이즈 100G
  name: mysql-persistent-disk     # 영구적 디스크 pool 이름
jobs:
- instances: 1        # job 인스턴스 수(필수)
  name: mysql_z1      # 작업 이름(필수): MySQL 서버
  networks:        # 네트워크 구성정보
  - name: mysql1
    static_ips:
    - 10.244.7.2
  persistent_disk_pool: mysql-persistent-disk    # 영구적 디스크 pool 정의(옵션)
  properties:
    admin_password: password         # MySQL 어드민 패스워드
    cluster_ips: # 클러스터 구성시 IPs(필수)
    - 10.244.7.2
    - 10.244.8.2
    - 10.244.9.2
    max_heap_table_size: null
    network_name: mysql1
    seeded_databases: null
    syslog_aggregator: null
    tmp_table_size: null
    wsrep_max_ws_rows: null
    wsrep_max_ws_size: null
  release: openpaas-mysql         # 서비스 릴리즈 이름(필수)
  resource_pool: mysql_z1         # Resource Pools block에 정의한 resource pool 이름(필수)
  template: mysql         # job template 이름(필수)
- instances: 1
  name: mysql_z2
  networks:
  - name: mysql2
    static_ips:
    - 10.244.8.2
  persistent_disk_pool: mysql-persistent-disk
  properties:
    admin_password: password
    cluster_ips:
    - 10.244.7.2
    - 10.244.8.2
    - 10.244.9.2
    max_heap_table_size: null
    network_name: mysql2
    seeded_databases: null
    syslog_aggregator: null
    tmp_table_size: null
    wsrep_max_ws_rows: null
    wsrep_max_ws_size: null
  release: openpaas-mysql
  resource_pool: mysql_z2
  template: mysql
- instances: 1
  name: mysql_z3
  networks:
  - name: mysql3
    static_ips:
    - 10.244.9.2
  persistent_disk_pool: mysql-persistent-disk
  properties:
    admin_password: password
    cluster_ips:
    - 10.244.7.2
    - 10.244.8.2
    - 10.244.9.2
    max_heap_table_size: null
    network_name: mysql3
    seeded_databases: null
    syslog_aggregator: null
    tmp_table_size: null
    wsrep_max_ws_rows: null
    wsrep_max_ws_size: null
  release: openpaas-mysql
  resource_pool: mysql_z3
  template: mysql
- instances: 1
  name: proxy_z1       # 작업 이름(필수): proxy
  networks:
  - name: mysql1
    static_ips:
    - 10.244.7.6           # 사용할 IP addresses 정의(필수): Proxy IP
  properties:
    cluster_ips:            # MySQL 서버 IP
    - 10.244.7.2
    - 10.244.8.2
    - 10.244.9.2
    external_host: p-mysql.10.244.0.34.xip.io        # CF 설치시 설정한 외부 호스트 정보(필수)
    nats:             # CF 설치시 설치한 nats 정보 (필수)
      machines:
      - 10.244.0.6
      password: nats
      port: 4222
      user: nats
    network_name: mysql1
    proxy:                   # proxy 정보 (필수)
      api_force_https: null
      api_password: password          # proxy api 유저 비밀번호(필수)
      api_username: username       # proxy api 유저 아이디
      health_port: null
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: proxy_z1
  template: proxy
- instances: 1
  name: proxy_z2
  networks:
  - name: mysql2
    static_ips:
    - 10.244.8.6
  properties:
    cluster_ips:
    - 10.244.7.2
    - 10.244.8.2
    - 10.244.9.2
    external_host: p-mysql.10.244.0.34.xip.io
    nats:
      machines:
      - 10.244.0.6
      password: nats
      port: 4222
      user: nats
    network_name: mysql2
    proxy:
      api_force_https: null
      api_password: password
      api_username: username
      health_port: null
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: proxy_z2
  template: proxy
- instances: 1
  name: openpaas-mysql-broker_z1         # 작업 이름(필수): 서비스 브로커
  networks:
  - name: mysql1
  properties:
    auth_password: password           # 브로커 접근 아이디 비밀번호(필수)
    auth_username: admin             # 브로커 접근 아이디(필수)
    cc_api_uri: https://api.10.244.0.34.xip.io  # CF 설치시 설정한 api uri 정보(필수)
    cookie_secret: 94046872-2602-4ca6-8d07-8b0da9762477
    external_host: p-mysql.10.244.0.34.xip.io
    max_user_connections_default: 40
    mysql_node:
      admin_password: password
      host: 10.244.7.6
      persistent_disk: 100000        # 영구적 디스크 사이즈 정의(옵션): 100G
    nats:           # CF 설치시 설정하는 Nats 정보 (필수)
      machines:
      - 10.244.0.6
      password: nats
      port: 4222
      user: nats
    network_name: mysql1
    networks:
      broker_network: mysql1
    services:
    - dashboard_client:           # 서비스 dashboard client 정보
        id: p-mysql              # 서비스 dashboard 에 접속할 ID
        secret: p-mysql-secret       # 서비스 dashboard 에 접속시 사용할 secret
      description: MySQL databases on demand        # 서비스 설명
      id: 44b26033-1f54-4087-b7bc-da9652c2a539     # 서비스 아이디(고유식별자)
      metadata:
        displayName: MySQL for Pivotal Cloud Foundry        # 화면에서 보여줄 서비스 설명
        documentationUrl: http://docs.pivotal.io/p-mysql/   # 문서제공되는 Url
        imageUrl: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAQAAAAEACAYAAABccqhmAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAJo5JREFUeNrsXQtsVNeZvvPwzBiD3zyNwxgWhEKqmEibR9skdhQtaao2Rqtks4+qthKp1e5KxKhVCg3BbrKOst3lIW2rrUQVR5W2adgKp92ikG1ik2RbkpWCs4WWQomHVyAY8NhgPDOex97v+F4zHs97zrlz7z3/Jw1j/Ji5c+/9vv9x/v8/DoVgK9Rt7/arT37tv7XqozXT7zorK1clotFgYnp6PMtLDquPoPZ1YKxvd4DOsn3goFNgOYK3asRuUx81SQRvM/hQhpIEYlz7f1AViGG6SiQABH6WHMS+U/u61SKHDxGAp/CxJgzDqjAE6aqSABAyE75NI/iD2rPfZh8xoAnDYdfCquEr218coqtOAiC7K9+hEb5N0tMw5PBUHE5EpgcodCABsDvha5MI36HF8YRbQIgw4PR5D8dD4QEKGUgA7BLHg+yPSWzli7sx3e4hxeV8w11bO3B5y7YAnRESAKtZ+i2KdZJ2pobT6x2Oh8N74SGQZ0ACYFbig/Rf18hPEHGzulzqP44BR0XFq1d39A3QGSEBMIOLD0vfSTG94WIQdHi9/a5FC/dSiEACYCgaXtjeEZ+aImtvnnzBQCIa3auGB0N0NkgARFp8WPqdiv3W6O1xM3s9gUQ40qsKQT+dDRIAXqSHa/+M5uqTm2+R8MBZ6dvrXe3fc+HJpylpSAJAxCchICEgASDikxCQEJAAZCF/DxHf/kJwZfuLPXQ2SACSid+pUHJPnpve6wk4fb7eK8/29pMAyE38No34bUQLCW9+t3vI3VDfe3nLtiESAPni/N3KTAEPQe6wQHF4PP2+tau7ZcwPOCQkf6dGforzCXPzAwurumULCxwSER/x/Ssyufuwbs5K35zvOauqsv5NfHJy7v+nQkoiFpOGEE6fd8hVU9MlS3mxQxLy9yg2zO67Fqpkdjox3FMlu3OG7M75pOcFXRxmRCGuPk8pielp9n+bIeiuq907+u2dPSQA1rf6BxSLt+UyS161gBEd5Hb6fGrcWmGqY2SioIkBhMIOnoOWJLS1N+CwMflRzLPTilYf5Haprroak7Jns5G9EFGIh9THjUklpopCIjJtydyAq66md3Trjj0kANYgfq0W63dY6CZTXNWLLE/4XIAAxCYmlBgEYeK6xXIDvgHfujVddlspcNiM/G2ay296q8+sfHW14q5ZlDMxZ1dABGLjE+zZCuGCw+NBbmCzneoGHDYif4/m8pvb0tfVKupNJCxRR2JgiLfWa5cEocMGxDe9yw/33l1fx54JuREdC86KgWmv6aKFA96WVZYPCRwWJ3+rRn7TZfnh4rvr6hR3Y/3MLDtCUTkDiEF0bMyUCUSH2zXsaVrRdekbW4dJAIwnf4dGflPF+1ib1918Al+vIHrlqulqDpAXUK951+i3nh8gATCO/Fji222mYwLh3fW10ib0jAJqDKY/G2UrCabKC9TVdFtxqdBhQfLD6neaifgVS5fYdunOzOHB9GeXmWdgmnuhvq5f9QS6SADEEN9UyT4ivnmEIHL+gmk8AqvVCzgsRP5BxQTJPsT4FUsXk6tPoUFmEahaMOxb09JuBRFwEPnzPFGqpfc2NxHxTQ4sHUY+vVj2VQOIQHzyZrvZtzJzmJz8ID0q+/xlO0Eul+JubGBWn2AdwBvAqkE5i4rUeyfgWblis5mXCR0mJz8sf9nW01C441mxnOJ8K+cHVG+gnAVFWCb0LF/ablYRcBD507v7ID5V7tkoLDh3oWzegJlFwEHknwvd3afqPZt5Ayr59bCARMCkAlBO8oPwnuYmsvrkDUglAg4TkR+kP6qUIeHHYn2V/GT15fEGIAJlyg2A/KZZHXCaiPyD5SC/Z8Uyxeu/jcgvEXCtcc1x7cuAVmfVgsGm1/aZolnEYSLyG7rOz9b1V91GffmSA81F4TNnDa8bMEuxkBk8AMPbeVHN51u7hshPYPcA7gWjcz/xyZutoZOnXyn35y+r36s19jxp5Hsiy++9baXicDrp7ifMeIPqveCurWEOceq+CEJzEdHo+kWPPeq/+dbgG9IJgNbS+x1D4/3mJqViSSPd8YSMnqHD44F1VtmZMCoEaV20+cvjNw8NHpEmB6AN8zhg2IdE0me1n1x+Qv55gU8Chi0VsnkCNdWbyzFUxFEG8hu61g/Se1Y2EfkJhbnnkWmWHDRqAlG5agScBpNf7+k3jPxk+QnFEbLC0HsnEYnURi58+orRy4NGZ8IMy/jr5Kf1fUKpoSPbg9EIEYjGWsMjZwxdGTCMHdrc/m8a8V6Y1uNBpp/ITyhVBLBCoN5PCAmwzZkBnsD6RV/9kuPmfw8N2SYHoO3YM2gY+Zub6M4lcAfKh42YQajNoGg3YgcihwHkR0wzYkTcT+Qn2EYEPJ5g5fq1LaIrBY3IARiS9NMbeggEkcA9ZsSeD0gKGlEpKDRI1op9nhH9IVjCDw09VN1HMACummpWLCS6f4BVCgouEnIIJL9fmWnvrRVOfsr2F4zmqoXKyqrs9e8TkYhyPHiVTlY6csZirFhIdJ2Ael8H3Y0NGy9v2RYQEjYLPPYDRH7jUV3hUTbUNcwSfENtPftetUf9fm1DSa99fvKGcm5ypof+t6OXVIEIqwJxTf3+dfX7N6Q6z/oSoWgRUIWmNnr1GkKBdst4AEZs1U3lvTO4b8nymcfiZSrpF6mkX1iW45iYVr2FsatMGNjz5Yvse7b3BNQwIHTqtPCyYXddrZAtyR0CyG+I6y9rOy8s+6amVYz0eDYzED4cunBWOXT+jK1DCYN6B4IVS5dwDwVEhADCs/7IxMpG/ida1iqP+9cy4lsFCDnw2LphIwsf9p08prw+csp2ngHuxYoVy9gSoUDUxsbHuYcCXD0A1fp3agIgzhVqbCjXKKeyEX/rhrvK5tqLCBX2nTyu7PvjMdsJAaYOY8NSkXDVVHddeba333QCYETBD2qyEffL4urvuvsBS1n8QgCPoPvDd1muwE4IB84KHTaKVYHKDeu5FQjxXDjfLZL8bLOOVc1SkB9u86FNm21LfgAezf72R5mHYyew6dICd5LCqkDo1Ce7uYUvnKx/m/rUKfLEYoCnLMt9P/7iw2zpTgbAy2m2SXijWWh2r4pEIhLpXLL3pTYzeQBCl/wQ88uS9INFXGkjQuQD5DjsBDaERmCeCqsN0avXdppCALTEX5uoD4u4H4k/WZCrOs+u4YDdgHtW5KThRDTa1vjyzs6yC4BI68+265Ik7ifYD6J3m4qHQjvLKgBaxZ/fqieQQBCdDxDZoZoIR/yNfc/1lEUAtGW/LcJcf9V9oo06CVYH7mGRIWx8KrSllDmCpXgAaPMVsuwnWjkJBCMhcrt5LAuGPwk8Y6gAiLb+5PoTKBQwxgso1gMQZv2R9ZfZ9UcnnXSfOXhNilBA1H1dihdQsACItP5MKVfK7fofunDGduWx2YB+gF3HPpLis3pWLBfm2RbrBRTjAQiz/kiWiCyjtAqeev/XUkziAfkff+egFHMDmIFT721RCcFivYBiBGCLqJODZAnhFjHQNWdXwMvZdOiAdCPHWEJQkJGDF1BwaFKg+9+pCNrO29u8UnF6vcR+DeF4TBm6dJ5N2EGtfLNNKgTRBdgzfETpOfqBNJZ/ntX1eJRYcFyAG5DwVd5/35nQe0fy3l+w0IEgQqr+ZE/85bKUj6sPiMBT6+5gvQJWbBTaP3JKeT1wSqr8Rsb7HQlB9Z6P3Zjk70l7PeBof96/X4D1F7alt2+NX3FWVRHb84Q+Duy+JctKHvQp0tKD7G9qSU1ZrX1Gd31yUgmdDojxMCorN1/d0ZfXVuOFeABfF2X9ify3AAs/Homw1YBsXoFuSeENQBAwCRiDQfFcDg8Bx4PlvN8HZwaC5jslGAK2aeUqaVYCZkmq3vOivIDE9DS4mpcA5OUBaIM+R4RYf0mHe2bC1jvuYjP0QKJdx48W7TJDFPQR4ez/i5fN+VkhQKIOewTMfH2NWXN9FHixI8ER0qAN+HFV8FiYM3hQumuNicJTJ04Kee2KpUta8hkgmq8HICTzjy2WiPyZCbxffYAc+9XYGcM0C7XIQDZPolyf6wn/WkZ82cGWBVUOiNhrMHb9BjjbzUsAOoUIgER9/qUQBo+ejfey8dqIqc1G6nzcfBD+kaZV0g07ycNSCxGARDjcyUUAtOQf98IfFvuT9c8bcOdBIt1ysjzA6KU5+QAzER4JSl28ZBlvVqwXICIXgMKghhe2d+RKBubjAQhJ/lHRDx/PQNmwcTZOR+YdMTr6CRCnixYGEB1bjuE4mhcsZNbdzoNMxXkBi8uWDHTnsP6w/B28DwyWnzL//MnIMuppdgtKTuLBayhKcEpIIhJy8KFqxhvmvsdgItEBDo/17Q4W6wF0iPjAFPtnBjbcFCEOZiYv1QjMcIL3zkLaVmXgcH9G8cnxGtyz/3rmk5Aedtw6Kxfs3POQtwConBDRI+D0erNy2JnF/ferT638P2gdsTyHNew5ekSaz4sSYSoPFseNeDjcumTvS/5iPAAx7j9Z/7y8ALQE290TQKETtgcjiOVGNBjsKEYAHuN9IGiCoH7//IC1/nt/+TNmIe2GmVbgAenKf/MJj4U0xcXiGbnsyOD+Q4rGeB8H5qKRB1A49LJZ1MxbeU0dooZ4n1z+LNZ6LChqm/G6dKsBbqPcf4xCIvIXh3PaTrrVRz3KI6oIYKkv3XKfGYElSHgxIH8xPQMyhgHTn17SM/jc4PR5064GZBKAB0W4/4T8rH0moiAngPyA3hegFwNhjd4sy3vHWTfgrQrFfPIY2T6zjABXeJcHx0PhBwsRAO4egKummq5sHni8ZR1rloHFz+Uqp5YBY70f1XjoANxQWz/bDSgibNDfF4VFekdgoa49iI/dgdnnlrAbMBtXBPQHgNNdOXMAavyPpb+jvN3/yg3r6crmAb0dWCdZMZ2A6ZDcGjwjFqpAeLx5Ex0otvU3FakdgbK2A2fD1PET3MMAFRvH+nYP5/IA+Ft/cv+LJgoeSACiCxCxdLFDNFN7A4xOxMHaI28B0pt1ipHdwwCHpwLczikA/ON/cv9LAtz6p9dtYA80/Ohjtsw+aksXsE1NtxHpTRAGJCLTD+aTA2jj7f6TByBGDAB4BL8fu8ae0QlYriU21oxUV6/cXjuTf6CGIVN6zW1ZBUCN/9t4v6OzagFdTdHEw8ANZe0cdx8twfAWzt28Mc/l19uFC7HkqbkDllNQv8a4chryIU4EYhPXub5mY99zbVe2vziUyQPgXvtP7r/x0AeFzlWKjXRirCYAGBTCWQBiNybB8VkBcAqP/6nvvyDIWCVX7IwC+3sAQoznHI47RXoAqG2m2v/CBcBqM/9KAcKUfX88RhfeOP60phUArf3XT9a//Oj+4F0pRACJSxm6Hk3GIb/W6zPPA/DzfifnQhKAYgBCgBiYC2BXcqApCBugyrY5qEk41JpOANoo/jcfSfSWYLsIATwbtALbWdy4CoBPyOTsWa4nrwLcyTV+cbko/ufkDSR3Aj61boPlimoQ52Nj0P0jJ6npp1ABqPQxLnEuC74znQD4eR84ga8Q6J2Aelmtmefu6/sWYDMTcvNL5xLnseH+dALAdQWAxn4XB73CL9ugTFhR/Fz/ndQqPKM3CAXZZ/YkKLwaUW8MotFg2bnEWQBa5wiA1gHIXbUIhQNVdugGfHrdHcqu4x/l1Qk4QzxY2VPzyJX8rG/eke530pE61QMBudn7aZWEhVYUpr4vPud92v6HBGO5BM6jM1D3ALiP6hGUvJAGK7VeeXQCFhs/z/bsm4Rg8EqsmscoJxwVQnJptckhQBv3g6YEIDchgKXEw4rjtXTSW2mMmQwegMb5IV0Aani+sovW/4WAxfobG9hOwee1CTy/Hb2ouuLXTJNoQ4LydrY56HK2QShZen4iwHnrsJpkD4BvDsDppCtmgGeQvFuw7urrHYA84vRcMbz+rHcGGp18lC4M4CsArckCwFmtKumKlQHZevB1MZjzfy2pl9GapyQNqfW3nB5AJffOQGE5AIeLPIBigbl7omLxVIEwS0x+noqDysEpxnkhTKUlwOLx5vkz0pXIYpWDUB5OObUuQM6v6qIrViRAfjTJyCICW/MYf04QA3AfHgB3ASAPoDQgo2/3Tjm94/H1EbL+eXFKTGWtn4J1E4sA20Dz+FHbeQOoZUCXo0yDT8wKJAG5VgGic4nAD9hBFxNznmhZy8qDrZqF15uZfnzyGHUElsAtzl2BtRAAvk1A5P4LIY/e/MMmAKti8EjTKtOLAY4b3YDYx4CsPR9u8W4KctNptV5ocPzoVTZQA1V3M5uDLldur6sve9Wd3hE4s2nJJWoDtkgIQLAo4EqfG5m7d2DyBqF6IQ/PAp7k6kIUEhW7MSiBBIAgykNQH5lcbngNK6sK23VmIhIha04CkGecQoNAzO81UBLOmjkA/oNBFFoGJBBkFhU6BQSC3AJAUxoIBDlRQx4AgSC5B0AVGgSCnBgnD4BAkNwDIBAIJAB8EJ+cpLNKIAiACG6RB0AgkAdAIBBkBPUCWAzYOxDbh6UiWzMO1fITsgnAMNc4he/sckISdt/9wJx9AOZgw8aiX5ftJZBlGrG+10A6YGAJwaAcAH9usb0BgzxfkfPEEoIyM9J7/0OPCuv3X1m1sOh2YaO2/27Wjk/mRiYB3ApSCCA5+UsFPBIMKBEJjEPDRqm6N4IJQwh5aMpQ6XBoY8FHeL6ob42f2oIlIL9OyHv/62fCXh/7ICLvkQkQAgxOtftAEiwBhk4HeL9si3Osb3eAqEbkLyV8EHGM+Pw//uLDWckPYCQakpyEwgHuC1kGpERgaQCh3tq02TI76/IeTorPDfHLZ+syWH8ZVjhEcUrPAQwpHPcHTMTixOISb36r7LKLEIBnLA6LDsufz+cH8WVZhRDAqaFkAeCsVlPEZAnID/Dc1y852ZcLGEr69Pu/lubeEFVirwvAMFcPYHqa2CwB+QFsWsIDWWsc0rn+quWXaUlQwBLgcLIAjFMOoHyA5UO220rkZ7P/Ry+VvG1ZMclOvDc2SZEJAjg1npoD2Mn7gGmXIL5ur2hgzz7dpTdiWQ2kR7xfSBIRgoMdhYn8fHMAQd6vzsIAEgDLkJ+RfvSiYevpyPDvvueBgr0e2Vx/gSE14zxbBhzr2z3M+9UpDLAW+QGjiIW1/Xwz/cnAaoNsrr8oLumcd6ckBbhtFDqTtVxMTE+DrXfcpWwtoXlHFHo23qPt73ctZwyeDfrWYeni/d6N9xaU7Et2/bs/eFfK+0XACsCswU8WgABXASAPIC0KzXYbCcTkeOQswCmDeIH8pSYcyQOYw3VlNgTQ8DHXuCUWIxGwEPnNjEMSby8ODglYAvw4nQAMcT/4EAkAkb80yOz6C+TQ0DwBqFi6JMD94G/QgFDEvUR+cv1NxqHheQJwecu2QHJswAMxySYEI7mX2r2G2nYif3FAxl/2nn8BHAqM9e2eXfZ3p1EGP7c8QGSaPRyeCqlcfFis10dOzcavKLAhESgMWI2QfdyYzh9R1j81BwAc5q5gExO2d/EPbeqYQ3Cs7yeXtnZ/+K7tB1Zwd/0/lNv1F8idwxkFwLWwintBUMzGeQC9gSddHXvq9596/9c0mbcA158EUxh3MnsAV7a/OMRfxa7b8uIgts/WxALPYNc9989Wu+ntq7JbNXL9y8sdNf4fyhYCACQCOYAy3v3tuVt3dQ9BB0ptH3/nIIlAFjxFIimSM/O4PU8AHJ4K/nmAcfvkAdC2W0gNP0Rgd9LvIwzoOXqEmJ4Gsoz3KiNnDucUgERkeoA8gPTAEl+uIZXpgARh8t9hhYBEYC5kGu9VRs7M47Yr9Ruh945cqrz/vmfUL/n18iYSirOyUnF6vZa+KH+aGFfalq9UlvgWFPy3+LvzN2812nx0dVRprlqkbKhroLtdxdcOv6WMhmiUnE7+6Bj3Dv2gGv935/QAMikFhQHaMIoP3is6RkX4kLo8SC4vuf4GcSUtp9MOBXX6vIfjoXAnhQHpXVUk8rD2Xyj08VebDh2Y7b3Ha1lh/n+yCKLdt7RzeG2OiJLrbwhX0ub2HOm+Wbe9u1Z9GuN9BJ7mJsVdV2uLi1TKQA9dRHQSFDsQFHmEXL37qTg/eV3q/fXMDrj+kXMXRLx0XXIJcFYBAOqf//ZgIhpt43kErupFitd/m20uVimDPVDo8vjgwdn/QwTy9Sr0DjnaG89+CAfOivAAhlTyt6f19jOz1fmGCNdGQG1z+WJX1XXdP1LcXHwUEqUuD+Yz7BLkZyEIkd92ADcEuf8ZuZxRANy1tQMijkRAdrOsKCWRl255MNvMO7zPvb/8GSXMbOz+C0JGLrsy/WDyzbeDVQ+3dSRisWVcVS4UUiqWNNrqwv3i7CclLQ8ijj99fWZrhqFL59MuD8LiU5WcvRE5c44tmXPGsOr+v1ywBwDEw+G93N2cWMx2XkCpy4MYj528CrCTJfduWXmEGUR++1t/AaO/gKwczrU78IDD5eL/Ya/Yz4UFYZ/Kc686NLwgCXhey8bry4PJjUOI8/Fz5AW6P3yXGGJ3ARDHiayhfFZ2h947Eqps+0KrEo+v5+oFRKNoPVYcHo+tLiIIi2q/bFN12XCQwYPK/sBMvA+yIwzwqkKLZ4QT4XiMPfDzQpf5CNYDxn5PX74ihPyq+/9qKR6A4qioeFXEkU1/NmrLi5ktkQcvIdWaJ/e+s8ahex4gRkgGgVzIyd2cAnB1Rx/CAO5BO4Yd2HVsOAp00i0PHrpwNu3vY5NNHWzLrLtJBKSx/ioHBA3+QO3/QMkCwLwAr7ffYnFP2ZGayNNj/XRI/T6WB5+gGYIU+5eGvDiblwC4Fi3cK+TDI/Npo8KgZCQn8nSA1Kk1/yB/OrKjzBjFQgR7W3+BK2J5cTavFD9qAha03889GQigLsAu/QGpQCLvt5cvKY+tWs2SfHjga7S9Vnu8yudVgv/wvoeUxb7KeX/LtupWH3gNgj0ROXdelAFE8u9H+fyiO99XRDIwEY128D5SlguYnFScVVW2vMgs8ffBu2w3XN3i52oiQmKQBobY3Pqr97zAgbl5e+x5L/JPDb53YsHDD3YqsRh3c439z+3qBQCo8su1PKgD6/4/+MP/EUPsbv3PfyrK+gfSDf4oKQcwS9RwpFfEEUMJ7To9WAfc+WyNQyxnMHhwdkMRgn2Be12g9S+Io45CX71+x7fGEgK8AOweVLl+ne0vPkKBVE8AiULaN0AeTJ04Kcr6Y+mvrpA/cBb6Ds5Kn5AVAZwQuxYHJQP5gGSi4+u/OHSAyC8JcI8LXPkqmJsFC4B3tX+PiMIgAGuidl0WnOPqa9OAEBJsOjRATT6SAPe2wHV/cHJPoX9UcKfP9f/8RajqkYcq1Q/Txv8MJdSTFFHctTW2vhGwtIea/9cDFO/LBIz6ioeEVb++rLr/bwr3AER7ASxBYvOEIEBz+eSC4Pu6KOtftABcePLpoKhcgK6UgnqjCQTjXX/1XhY06HM29k838FOYAIj2Agw4YQSCoa6/QINWtPUvSQBEewGyhAIEcv3LZf0BR6nvXt/77EgiHPGL+GSYRuRbv1YRMZWIQDDC9Q+dOCXS+qPqr6WUF3CWegROn69X5AmkUIBArn9GlMw9B4+jELGJSDI8K5Yp7kbaRJNgHWC9P/LpJZFvkXGzD0M9AMDdUN8r0k3HibTr9CCC/YB7VTD5uVh/bgJwecu2IYfH0y/y04bPnKWlQYIl4n7cq4LRr1r/IdMIAOBbu7pb1LIgO7GR6ZmNEwgEM8f96j0quJwdHOvm9WLcBIAtCy6s6hb5ydFCaYBrRSAUHaoKbPPV0V3Ksl8qHLyPruF73xmMh8JtIs+AnbYZJ9gDArf1TgaXxF8y3LyP0FVT0xUPXT6qfimMoTjRTp8Prcl056UBxk3diknjRSdQMaPBqT5m3UWbjm0r+Xwj6See/LD6Xbxf1CHiSBd/v7dHVcSdIs8GVh28q/3SiQDiy8R0hN10M+SeUu9A9TkSMayVekYYPKoiONXzX6leCye7Do4KD/uZbOQPfxIwIkHdq1r/HksIACC6NoBZJPWmgwjYtVIQlhw3GCP79LQR8SUfLxDbvlVUsOvDHjb1HFjGXyW/AUvU3F1/YSHA7As31HdFr1w9KmJ8WKr62kEEGNFD6gNTkkMhS9c9zArV2FyxZmGbKg52CN8MJL8Q11+4B8BCgV0vPBO9cm236DNkRU8A7npsYkIbi35TuhoHXCtn1QLmLbiqqy0VOhhIfgBZ/z2WFACg4XvbDqgWrUP0+2BVAKsDpraM6AwbV0mvuvZ2H31WTF7BpYYKrppqVRAWmfpYkfATuKNPMrDBx2ahvBH9CXzr1nRNnTjVlohEhK7b6RfETCIASzFLemptzukRRSPB2esIEdDFwEyenYHkF+r6G+YBAEv2vtQWvXJ10Ag3t9yeAJGeP8wiBgaSH2jnVe6b9dwa8Ukm33w7sPArmxxqKNAm+r2QQIM1QbLJ4XQadnMgY4+Rz9MXLiqx4Dg2USHm8hJV9VwyUb02pn4dVhxulxoyeAwVdVxXA8mPJb9+Q0IvIy9k40s7DsSu3+gw4r2MSAwya6/eFNMSjDM3Y86gorFBcaken+hrbGDCz5C4fw5PjLxo3pZVXap6DxtikQUWaMDawx2cOn6C1X8T+cuTM8C5Z9cA47YnJ+1A/mEj4v6yeQDAsh/tao1c/GxQdFIw2RPwrGzisu4MFxAW3yoFOdLlCrCkqHoEPPpEWHnv+QtGkj+oxf3DthYAYPG/fK8jNj5xwKi171LLhkH86c8uk6W3UniwdEnRQmBgeW8yNqvkHzBcNMtxgW6+NXhiYceXxuM3px4xxl9MKNFrYyxxlK8I4OJHR68yKwCrr8TixCyrQL1WSBpGtevG+hTyTAjjb8IjZ9g9YyC6jUr6mcIDSPIEXlGJ2Wnke2K2IGYMZr0Jrlyd2cTRYtV5n1vepNyxfIVyW139vJ/97uIF5djFT5WzY9fk8whUD7Bi6eKccyWRUxC4d18mYLpPV9nOTbkvjlGVgqmxomdV87zssRVdfZD+m59/QHn09juUGl9lzt+HAPzPyGnlpx/9r/L+yJ/m/fyLLX+m/OLpv+dybPXf3WqJ0IBNnz5zrhy5HUMz/mkNYrkvCioFQ6dH/PHJm62GeYjqhQ6dOq14V93G3ENkkMMY4Wwh4sPK9z3awYhf6N/h8dd3/TkTgn/4+U+l8QrYyoF6nSHy3uYm1qXI4n3MmzT+2hue8TelAGCUWNNr+9pVERg0UgRwwSECTAAs1nkHK/2Tv+vKy+JnA8IFGUMCdu1PB8p57UH+dp6jvYqF0wwXBCKgkr9dOzGGwmrkh+WGi14q+YGDvz+myAzZyW8KD0AHTsiyH+3qMrJGwGpAvN/35czpkvHQFCO1nvDT8YXVa5TPLWtiz8nC8e+/eZdOqrFgDT5mIb+pBAC49I2tw6oItJMIpMe//eWTGS0/yPzy24eYCKRCT/bhb5EzePahTez/EIp88dV9P0ybNCQURH7DC30sJQAkAtldf3gA6YBEHrL6uQBxwO/BS8j0WgR5yG+aHEA6EfAsX9ru8HiCdO/MQLfaqdj+q4G8yJ8qBGTNifymFYBkEVDKkBg0Y+yfrrgHy3gUx5saw2Ymv6kFQBcBnEBn1QKpRSDTWv9/fPQhUYzIb18BAJAx9a1pkVoEsO6fDoW6/gTDyW/6ENZthbM5Wyx08vQrRpcNmwHNtXVp3X8jgaKhfHA2eE3K4qIkoKOvywrkt4wA6CKgPm0uRwNRuZEu/h+fmjL0GLLVHyTj5XcOseVISVHWxh5bhgCpGP3W813uxvpuu+4GlC9+d+mCQjAVuq1GfksKABOBrTv2uGqqN8u8TMijFJjABcwzFbl5BwlAek9ggNUKGDRj0GygQh5TQE/2DVj1A7itfPaxTIjkYHjkzCtGTRsui7t/8cI8wqdLDIo+holQ7uYZiRKAlkr22VIAgNnk4Pd7e2IT13facY+9c2Nj8wQAiUF8r5B6/lLw3V+9QdWDGhyeit5rPf/cY4fP4rTLRRn99s4ed2ODLcuHMxEP/QEEw+P9druQ31YCAFzesm2ocv3aFqfPN2Cnz3XwD8cyCkC6JUKCMJe/xYjtukgASgwJrj7/0mZtqdAW3oA+xy8VWAn4yd920YqAeKuPJb7NVo/3pRCA2ZBg6449akiw0eF220KxMxXXIA9Q6IQgeA4jO/6JhCM3cO9stOoSXz6wdTXN5JtvB6cO/+bVRV/9EjYmxbxBn1U/C0psQfa1i5fM+9nSRdVK1z2fV3wVFcq54FjaoSD6INB9f/U15W/uulvxuSuUd079kb1uut9LBfoOUn/X5lb/ZRT2hN47YutaE4csV3TJ3pf8sfHxV+KhcJtVPwMsNqx9rhoAhAwQAh1YMkyXK0hXtptpLLhEE4Fg9bG8F5Dhw7plEYDLW7bhgrY3vryzM35jcnciFrPctCFY9n/8+Wss7s+W/NNHf+cC5gQS5sX6/TJ9aKdsV/nKs739lRvWtzgrK/ut2E+Adf8H/+1fuXQD5tvhJwFA+hbZyC+lAABspWBHXxerG7BgkhCewFf2/aCkTT0gIPAmJAeuPUp5u+yY4accQJ5gYUEotDMRjvitePyI2zE1CLmBL7SsyZgXwKhwxPGoK8gkHFmTgPYp80U42CujxScByCYEfc/1xKdCW6yYH5gX36tigKQhle/Oi/P3qsTvoVNBApAWTa/tqw1/EnjGLkJAuEV89bFHVlefBICEgIhPIAEgISDiE0gAikLd9u5Oh9dj2WShBAgolNwjATBACNocbveWRDTaQWfDFECn3l67deqRAJgcrLz4+o0tiXC4k8KDsrj5/RrxA3Q6SADKioYXtnckpqe/riQSHXacSmQya/+qlefwkQDYOzyAJ9Dh9Hq3xMPhVjojXIDhm0jqDVBSjwTAUiFCNBjsUGLxxxLRaBudkYKAeP4NjfTk4pMAWN8zcPq8HfFQ+EF4COqDcgbzY3q49YfJ0pMAyCAIrQ5PRUciMg1BkNU7GEoi/DDdFSQA0qKx77m22I1J5AwgCHj22+wjBrRYHoQfpiU7EgBCjpBBEwJ4B3dqgmCVpOKwRviPNSs/TC49CQCBU+ig5Q8gDDVJomB0GDGURPZx7f9BcuVJAAjlFQh/UvhQy8FrAKF16x2gjLy98P8CDADMneNqQ66ddAAAAABJRU5ErkJggg==
        longDescription: Creating a service instance provisions a database. Binding
          applications provisions unique credentials for each application to access
          the database.           # 서비스 상세 설명
        providerDisplayName: Pivotal Software       # provider 이름
        supportUrl: https://support.pivotal.io   # 서포트 가능한 Url
      name: p-mysql            # 서비스 이름
      plan_updateable: true          # plan 업데이트 가능여부
      plans:             # plan 정보
      - description: Shared MySQL Server            # plan 설명
        id: ab08f1bc-e6fc-4b56-a767-ee0fea6e3f20       # plan 아이디
        max_storage_mb: 10           # plan 정보: 10mb 저장공간
        max_user_connections: 20       # plan 정보: 사용자 접속수
        metadata:
          bullets:                # 화면에 제공할 plan 정보
          - Shared MySQL server
          - 100 MB storage
          - 20 concurrent connections
          costs:               # 과금 정책 설정
          - amount:
              usd: 0            # US 달러: 0달러
            unit: MONTH        # 월별 징수
          displayName: 100 MB
        name: 100mb        # plan 이름
      - description: Shared MySQL Server
        id: 11d0aa36-dcec-4021-85f5-ea4d9a5c8342
        max_storage_mb: 20
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
      tags:           # 테그 정보
      - mysql
    skip_ssl_validation: true
    ssl_enabled: null
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: openpaas-mysql-broker_z1      # Resource Pools block에 정의한 resource pool 이름(필수)
  template: cf-mysql-broker     # job template 이름(필수)
- instances: 1
  name: openpaas-mysql-broker_z2
  networks:
  - name: mysql2
  properties:
    auth_password: password
    auth_username: admin
    cc_api_uri: https://api.10.244.0.34.xip.io
    cookie_secret: 94046872-2602-4ca6-8d07-8b0da9762477
    external_host: p-mysql.10.244.0.34.xip.io
    max_user_connections_default: 40
    mysql_node:
      admin_password: password
      host: 10.244.7.6
      persistent_disk: 100000
    nats:
      machines:
      - 10.244.0.6
      password: nats
      port: 4222
      user: nats
    network_name: mysql2
    networks:
      broker_network: mysql2
    services:
    - dashboard_client:
        id: p-mysql
        secret: p-mysql-secret
      description: MySQL databases on demand
      id: 44b26033-1f54-4087-b7bc-da9652c2a539
      metadata:
        displayName: MySQL for Pivotal Cloud Foundry
        documentationUrl: http://docs.pivotal.io/p-mysql/
        imageUrl: data:image/png;base64,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
        longDescription: Creating a service instance provisions a database. Binding
          applications provisions unique credentials for each application to access
          the database.
        providerDisplayName: Pivotal Software
        supportUrl: https://support.pivotal.io
      name: p-mysql
      plan_updateable: true
      plans:
      - description: Shared MySQL Server
        id: ab08f1bc-e6fc-4b56-a767-ee0fea6e3f20
        max_storage_mb: 10
        max_user_connections: 20
        metadata:
          bullets:
          - Shared MySQL server
          - 100 MB storage
          - 20 concurrent connections
          costs:
          - amount:
              usd: 0
            unit: MONTH
          displayName: 100 MB
        name: 100mb
      - description: Shared MySQL Server
        id: 11d0aa36-dcec-4021-85f5-ea4d9a5c8342
        max_storage_mb: 20
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
      - mysql
    skip_ssl_validation: true
    ssl_enabled: null
    syslog_aggregator: null
  release: openpaas-mysql
  resource_pool: openpaas-mysql-broker_z2
  template: cf-mysql-broker
- instances: 1
  lifecycle: errand       # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar         # 작업 이름: 서비스 브로커 등록
  networks:
  - name: mysql1
  properties:
    broker:
      host: p-mysql.10.244.0.34.xip.io
      name: p-mysql
      password: password
      username: admin
    cf:
      admin_password: admin
      admin_username: admin
      api_url: https://api.10.244.0.34.xip.io
      apps_domain: 10.244.0.34.xip.io
      skip_ssl_validation: true
  release: openpaas-mysql
  resource_pool: errands_z1
  template: broker-registrar
- instances: 1
  lifecycle: errand
  name: broker-deregistrar
  networks:
  - name: mysql1
  properties:
    broker:
      name: p-mysql
    cf:
      admin_password: admin
      admin_username: admin
      api_url: https://api.10.244.0.34.xip.io
      apps_domain: 10.244.0.34.xip.io
      skip_ssl_validation: true
  release: openpaas-mysql
  resource_pool: errands_z1
  template: broker-deregistrar
- instances: 1
  lifecycle: errand
  name: acceptance-tests
  networks:
  - name: mysql1
  properties:
    broker:
      host: p-mysql.10.244.0.34.xip.io
    cf:
      admin_password: admin
      admin_username: admin
      api_url: https://api.10.244.0.34.xip.io
      apps_domain: 10.244.0.34.xip.io
      skip_ssl_validation: true
    proxy:
      api_force_https: null
      api_password: password
      api_username: username
      external_host: p-mysql.10.244.0.34.xip.io
      skip_ssl_validation: null
    service:
      max_user_connections_default: 40
      name: p-mysql
      plans:
      - max_storage_mb: 10
        max_user_connections: 20
        plan_name: 100mb
      - max_storage_mb: 20
        max_user_connections: 40
        plan_name: 1gb
    smoke_tests_only: null
  release: openpaas-mysql
  resource_pool: errands_z1
  template: acceptance-tests
meta:
  environment: null
  syslog_aggregator: null
name: openpaas-mysql-service
networks:         # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, bosh lite 에서는 제공하는 네트워크를 수정 없이 사용
- name: services1
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.1.0/30
    reserved:
    - 10.244.1.1
    static:
    - 10.244.1.2
  - cloud_properties:
      name: random
    range: 10.244.1.4/30
    reserved:
    - 10.244.1.5
    static:
    - 10.244.1.6
  - cloud_properties:
      name: random
    range: 10.244.1.8/30
    reserved:
    - 10.244.1.9
    static:
    - 10.244.1.10
  - cloud_properties:
      name: random
    range: 10.244.1.12/30
    reserved:
    - 10.244.1.13
    static:
    - 10.244.1.14
  - cloud_properties:
      name: random
    range: 10.244.1.16/30
    reserved:
    - 10.244.1.17
    static:
    - 10.244.1.18
  - cloud_properties:
      name: random
    range: 10.244.1.20/30
    reserved:
    - 10.244.1.21
    static:
    - 10.244.1.22
  - cloud_properties:
      name: random
    range: 10.244.1.24/30
    reserved:
    - 10.244.1.25
    static:
    - 10.244.1.26
  - cloud_properties:
      name: random
    range: 10.244.1.28/30
    reserved:
    - 10.244.1.29
    static:
    - 10.244.1.30
  - cloud_properties:
      name: random
    range: 10.244.1.32/30
    reserved:
    - 10.244.1.33
    static:
    - 10.244.1.34
  - cloud_properties:
      name: random
    range: 10.244.1.36/30
    reserved:
    - 10.244.1.37
    static:
    - 10.244.1.38
  - cloud_properties:
      name: random
    range: 10.244.1.40/30
    reserved:
    - 10.244.1.41
    static:
    - 10.244.1.42
  - cloud_properties:
      name: random
    range: 10.244.1.44/30
    reserved:
    - 10.244.1.45
    static:
    - 10.244.1.46
  - cloud_properties:
      name: random
    range: 10.244.1.48/30
    reserved:
    - 10.244.1.49
    static:
    - 10.244.1.50
  - cloud_properties:
      name: random
    range: 10.244.1.52/30
    reserved:
    - 10.244.1.53
    static:
    - 10.244.1.54
  - cloud_properties:
      name: random
    range: 10.244.1.56/30
    reserved:
    - 10.244.1.57
    static:
    - 10.244.1.58
  - cloud_properties:
      name: random
    range: 10.244.1.60/30
    reserved:
    - 10.244.1.61
    static:
    - 10.244.1.62
  - cloud_properties:
      name: random
    range: 10.244.1.64/30
    reserved:
    - 10.244.1.65
    static:
    - 10.244.1.66
  - cloud_properties:
      name: random
    range: 10.244.1.68/30
    reserved:
    - 10.244.1.69
    static:
    - 10.244.1.70
  - cloud_properties:
      name: random
    range: 10.244.1.72/30
    reserved:
    - 10.244.1.73
    static:
    - 10.244.1.74
  - cloud_properties:
      name: random
    range: 10.244.1.76/30
    reserved:
    - 10.244.1.77
    static:
    - 10.244.1.78
  - cloud_properties:
      name: random
    range: 10.244.1.80/30
    reserved:
    - 10.244.1.81
    static:
    - 10.244.1.82
  - cloud_properties:
      name: random
    range: 10.244.1.84/30
    reserved:
    - 10.244.1.85
    static:
    - 10.244.1.86
  - cloud_properties:
      name: random
    range: 10.244.1.88/30
    reserved:
    - 10.244.1.89
    static:
    - 10.244.1.90
  - cloud_properties:
      name: random
    range: 10.244.1.92/30
    reserved:
    - 10.244.1.93
    static:
    - 10.244.1.94
  - cloud_properties:
      name: random
    range: 10.244.1.96/30
    reserved:
    - 10.244.1.97
    static:
    - 10.244.1.98
  - cloud_properties:
      name: random
    range: 10.244.1.100/30
    reserved:
    - 10.244.1.101
    static:
    - 10.244.1.102
  - cloud_properties:
      name: random
    range: 10.244.1.104/30
    reserved:
    - 10.244.1.105
    static:
    - 10.244.1.106
  - cloud_properties:
      name: random
    range: 10.244.1.108/30
    reserved:
    - 10.244.1.109
    static:
    - 10.244.1.110
  - cloud_properties:
      name: random
    range: 10.244.1.112/30
    reserved:
    - 10.244.1.113
    static:
    - 10.244.1.114
  - cloud_properties:
      name: random
    range: 10.244.1.116/30
    reserved:
    - 10.244.1.117
    static:
    - 10.244.1.118
  - cloud_properties:
      name: random
    range: 10.244.1.120/30
    reserved:
    - 10.244.1.121
    static:
    - 10.244.1.122
  - cloud_properties:
      name: random
    range: 10.244.1.124/30
    reserved:
    - 10.244.1.125
    static:
    - 10.244.1.126
  - cloud_properties:
      name: random
    range: 10.244.1.128/30
    reserved:
    - 10.244.1.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.132/30
    reserved:
    - 10.244.1.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.136/30
    reserved:
    - 10.244.1.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.140/30
    reserved:
    - 10.244.1.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.144/30
    reserved:
    - 10.244.1.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.148/30
    reserved:
    - 10.244.1.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.152/30
    reserved:
    - 10.244.1.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.156/30
    reserved:
    - 10.244.1.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.160/30
    reserved:
    - 10.244.1.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.164/30
    reserved:
    - 10.244.1.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.168/30
    reserved:
    - 10.244.1.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.172/30
    reserved:
    - 10.244.1.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.176/30
    reserved:
    - 10.244.1.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.180/30
    reserved:
    - 10.244.1.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.184/30
    reserved:
    - 10.244.1.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.188/30
    reserved:
    - 10.244.1.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.192/30
    reserved:
    - 10.244.1.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.196/30
    reserved:
    - 10.244.1.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.200/30
    reserved:
    - 10.244.1.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.204/30
    reserved:
    - 10.244.1.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.208/30
    reserved:
    - 10.244.1.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.212/30
    reserved:
    - 10.244.1.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.216/30
    reserved:
    - 10.244.1.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.220/30
    reserved:
    - 10.244.1.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.224/30
    reserved:
    - 10.244.1.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.228/30
    reserved:
    - 10.244.1.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.232/30
    reserved:
    - 10.244.1.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.236/30
    reserved:
    - 10.244.1.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.240/30
    reserved:
    - 10.244.1.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.244/30
    reserved:
    - 10.244.1.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.248/30
    reserved:
    - 10.244.1.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.1.252/30
    reserved:
    - 10.244.1.253
    static: []
- name: services2
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.3.0/30
    reserved:
    - 10.244.3.1
    static:
    - 10.244.3.2
  - cloud_properties:
      name: random
    range: 10.244.3.4/30
    reserved:
    - 10.244.3.5
    static:
    - 10.244.3.6
  - cloud_properties:
      name: random
    range: 10.244.3.8/30
    reserved:
    - 10.244.3.9
    static:
    - 10.244.3.10
  - cloud_properties:
      name: random
    range: 10.244.3.12/30
    reserved:
    - 10.244.3.13
    static:
    - 10.244.3.14
  - cloud_properties:
      name: random
    range: 10.244.3.16/30
    reserved:
    - 10.244.3.17
    static:
    - 10.244.3.18
  - cloud_properties:
      name: random
    range: 10.244.3.20/30
    reserved:
    - 10.244.3.21
    static:
    - 10.244.3.22
  - cloud_properties:
      name: random
    range: 10.244.3.24/30
    reserved:
    - 10.244.3.25
    static:
    - 10.244.3.26
  - cloud_properties:
      name: random
    range: 10.244.3.28/30
    reserved:
    - 10.244.3.29
    static:
    - 10.244.3.30
  - cloud_properties:
      name: random
    range: 10.244.3.32/30
    reserved:
    - 10.244.3.33
    static:
    - 10.244.3.34
  - cloud_properties:
      name: random
    range: 10.244.3.36/30
    reserved:
    - 10.244.3.37
    static:
    - 10.244.3.38
  - cloud_properties:
      name: random
    range: 10.244.3.40/30
    reserved:
    - 10.244.3.41
    static:
    - 10.244.3.42
  - cloud_properties:
      name: random
    range: 10.244.3.44/30
    reserved:
    - 10.244.3.45
    static:
    - 10.244.3.46
  - cloud_properties:
      name: random
    range: 10.244.3.48/30
    reserved:
    - 10.244.3.49
    static:
    - 10.244.3.50
  - cloud_properties:
      name: random
    range: 10.244.3.52/30
    reserved:
    - 10.244.3.53
    static:
    - 10.244.3.54
  - cloud_properties:
      name: random
    range: 10.244.3.56/30
    reserved:
    - 10.244.3.57
    static:
    - 10.244.3.58
  - cloud_properties:
      name: random
    range: 10.244.3.60/30
    reserved:
    - 10.244.3.61
    static:
    - 10.244.3.62
  - cloud_properties:
      name: random
    range: 10.244.3.64/30
    reserved:
    - 10.244.3.65
    static:
    - 10.244.3.66
  - cloud_properties:
      name: random
    range: 10.244.3.68/30
    reserved:
    - 10.244.3.69
    static:
    - 10.244.3.70
  - cloud_properties:
      name: random
    range: 10.244.3.72/30
    reserved:
    - 10.244.3.73
    static:
    - 10.244.3.74
  - cloud_properties:
      name: random
    range: 10.244.3.76/30
    reserved:
    - 10.244.3.77
    static:
    - 10.244.3.78
  - cloud_properties:
      name: random
    range: 10.244.3.80/30
    reserved:
    - 10.244.3.81
    static:
    - 10.244.3.82
  - cloud_properties:
      name: random
    range: 10.244.3.84/30
    reserved:
    - 10.244.3.85
    static:
    - 10.244.3.86
  - cloud_properties:
      name: random
    range: 10.244.3.88/30
    reserved:
    - 10.244.3.89
    static:
    - 10.244.3.90
  - cloud_properties:
      name: random
    range: 10.244.3.92/30
    reserved:
    - 10.244.3.93
    static:
    - 10.244.3.94
  - cloud_properties:
      name: random
    range: 10.244.3.96/30
    reserved:
    - 10.244.3.97
    static:
    - 10.244.3.98
  - cloud_properties:
      name: random
    range: 10.244.3.100/30
    reserved:
    - 10.244.3.101
    static:
    - 10.244.3.102
  - cloud_properties:
      name: random
    range: 10.244.3.104/30
    reserved:
    - 10.244.3.105
    static:
    - 10.244.3.106
  - cloud_properties:
      name: random
    range: 10.244.3.108/30
    reserved:
    - 10.244.3.109
    static:
    - 10.244.3.110
  - cloud_properties:
      name: random
    range: 10.244.3.112/30
    reserved:
    - 10.244.3.113
    static:
    - 10.244.3.114
  - cloud_properties:
      name: random
    range: 10.244.3.116/30
    reserved:
    - 10.244.3.117
    static:
    - 10.244.3.118
  - cloud_properties:
      name: random
    range: 10.244.3.120/30
    reserved:
    - 10.244.3.121
    static:
    - 10.244.3.122
  - cloud_properties:
      name: random
    range: 10.244.3.124/30
    reserved:
    - 10.244.3.125
    static:
    - 10.244.3.126
  - cloud_properties:
      name: random
    range: 10.244.3.128/30
    reserved:
    - 10.244.3.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.132/30
    reserved:
    - 10.244.3.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.136/30
    reserved:
    - 10.244.3.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.140/30
    reserved:
    - 10.244.3.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.144/30
    reserved:
    - 10.244.3.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.148/30
    reserved:
    - 10.244.3.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.152/30
    reserved:
    - 10.244.3.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.156/30
    reserved:
    - 10.244.3.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.160/30
    reserved:
    - 10.244.3.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.164/30
    reserved:
    - 10.244.3.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.168/30
    reserved:
    - 10.244.3.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.172/30
    reserved:
    - 10.244.3.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.176/30
    reserved:
    - 10.244.3.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.180/30
    reserved:
    - 10.244.3.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.184/30
    reserved:
    - 10.244.3.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.188/30
    reserved:
    - 10.244.3.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.192/30
    reserved:
    - 10.244.3.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.196/30
    reserved:
    - 10.244.3.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.200/30
    reserved:
    - 10.244.3.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.204/30
    reserved:
    - 10.244.3.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.208/30
    reserved:
    - 10.244.3.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.212/30
    reserved:
    - 10.244.3.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.216/30
    reserved:
    - 10.244.3.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.220/30
    reserved:
    - 10.244.3.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.224/30
    reserved:
    - 10.244.3.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.228/30
    reserved:
    - 10.244.3.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.232/30
    reserved:
    - 10.244.3.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.236/30
    reserved:
    - 10.244.3.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.240/30
    reserved:
    - 10.244.3.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.244/30
    reserved:
    - 10.244.3.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.248/30
    reserved:
    - 10.244.3.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.3.252/30
    reserved:
    - 10.244.3.253
    static: []
- name: mysql1
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.7.0/30
    reserved:
    - 10.244.7.1
    static:
    - 10.244.7.2
  - cloud_properties:
      name: random
    range: 10.244.7.4/30
    reserved:
    - 10.244.7.5
    static:
    - 10.244.7.6
  - cloud_properties:
      name: random
    range: 10.244.7.8/30
    reserved:
    - 10.244.7.9
    static:
    - 10.244.7.10
  - cloud_properties:
      name: random
    range: 10.244.7.12/30
    reserved:
    - 10.244.7.13
    static:
    - 10.244.7.14
  - cloud_properties:
      name: random
    range: 10.244.7.16/30
    reserved:
    - 10.244.7.17
    static:
    - 10.244.7.18
  - cloud_properties:
      name: random
    range: 10.244.7.20/30
    reserved:
    - 10.244.7.21
    static:
    - 10.244.7.22
  - cloud_properties:
      name: random
    range: 10.244.7.24/30
    reserved:
    - 10.244.7.25
    static:
    - 10.244.7.26
  - cloud_properties:
      name: random
    range: 10.244.7.28/30
    reserved:
    - 10.244.7.29
    static:
    - 10.244.7.30
  - cloud_properties:
      name: random
    range: 10.244.7.32/30
    reserved:
    - 10.244.7.33
    static:
    - 10.244.7.34
  - cloud_properties:
      name: random
    range: 10.244.7.36/30
    reserved:
    - 10.244.7.37
    static:
    - 10.244.7.38
  - cloud_properties:
      name: random
    range: 10.244.7.40/30
    reserved:
    - 10.244.7.41
    static:
    - 10.244.7.42
  - cloud_properties:
      name: random
    range: 10.244.7.44/30
    reserved:
    - 10.244.7.45
    static:
    - 10.244.7.46
  - cloud_properties:
      name: random
    range: 10.244.7.48/30
    reserved:
    - 10.244.7.49
    static:
    - 10.244.7.50
  - cloud_properties:
      name: random
    range: 10.244.7.52/30
    reserved:
    - 10.244.7.53
    static:
    - 10.244.7.54
  - cloud_properties:
      name: random
    range: 10.244.7.56/30
    reserved:
    - 10.244.7.57
    static:
    - 10.244.7.58
  - cloud_properties:
      name: random
    range: 10.244.7.60/30
    reserved:
    - 10.244.7.61
    static:
    - 10.244.7.62
  - cloud_properties:
      name: random
    range: 10.244.7.64/30
    reserved:
    - 10.244.7.65
    static:
    - 10.244.7.66
  - cloud_properties:
      name: random
    range: 10.244.7.68/30
    reserved:
    - 10.244.7.69
    static:
    - 10.244.7.70
  - cloud_properties:
      name: random
    range: 10.244.7.72/30
    reserved:
    - 10.244.7.73
    static:
    - 10.244.7.74
  - cloud_properties:
      name: random
    range: 10.244.7.76/30
    reserved:
    - 10.244.7.77
    static:
    - 10.244.7.78
  - cloud_properties:
      name: random
    range: 10.244.7.80/30
    reserved:
    - 10.244.7.81
    static:
    - 10.244.7.82
  - cloud_properties:
      name: random
    range: 10.244.7.84/30
    reserved:
    - 10.244.7.85
    static:
    - 10.244.7.86
  - cloud_properties:
      name: random
    range: 10.244.7.88/30
    reserved:
    - 10.244.7.89
    static:
    - 10.244.7.90
  - cloud_properties:
      name: random
    range: 10.244.7.92/30
    reserved:
    - 10.244.7.93
    static:
    - 10.244.7.94
  - cloud_properties:
      name: random
    range: 10.244.7.96/30
    reserved:
    - 10.244.7.97
    static:
    - 10.244.7.98
  - cloud_properties:
      name: random
    range: 10.244.7.100/30
    reserved:
    - 10.244.7.101
    static:
    - 10.244.7.102
  - cloud_properties:
      name: random
    range: 10.244.7.104/30
    reserved:
    - 10.244.7.105
    static:
    - 10.244.7.106
  - cloud_properties:
      name: random
    range: 10.244.7.108/30
    reserved:
    - 10.244.7.109
    static:
    - 10.244.7.110
  - cloud_properties:
      name: random
    range: 10.244.7.112/30
    reserved:
    - 10.244.7.113
    static:
    - 10.244.7.114
  - cloud_properties:
      name: random
    range: 10.244.7.116/30
    reserved:
    - 10.244.7.117
    static:
    - 10.244.7.118
  - cloud_properties:
      name: random
    range: 10.244.7.120/30
    reserved:
    - 10.244.7.121
    static:
    - 10.244.7.122
  - cloud_properties:
      name: random
    range: 10.244.7.124/30
    reserved:
    - 10.244.7.125
    static:
    - 10.244.7.126
  - cloud_properties:
      name: random
    range: 10.244.7.128/30
    reserved:
    - 10.244.7.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.132/30
    reserved:
    - 10.244.7.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.136/30
    reserved:
    - 10.244.7.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.140/30
    reserved:
    - 10.244.7.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.144/30
    reserved:
    - 10.244.7.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.148/30
    reserved:
    - 10.244.7.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.152/30
    reserved:
    - 10.244.7.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.156/30
    reserved:
    - 10.244.7.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.160/30
    reserved:
    - 10.244.7.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.164/30
    reserved:
    - 10.244.7.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.168/30
    reserved:
    - 10.244.7.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.172/30
    reserved:
    - 10.244.7.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.176/30
    reserved:
    - 10.244.7.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.180/30
    reserved:
    - 10.244.7.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.184/30
    reserved:
    - 10.244.7.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.188/30
    reserved:
    - 10.244.7.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.192/30
    reserved:
    - 10.244.7.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.196/30
    reserved:
    - 10.244.7.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.200/30
    reserved:
    - 10.244.7.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.204/30
    reserved:
    - 10.244.7.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.208/30
    reserved:
    - 10.244.7.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.212/30
    reserved:
    - 10.244.7.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.216/30
    reserved:
    - 10.244.7.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.220/30
    reserved:
    - 10.244.7.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.224/30
    reserved:
    - 10.244.7.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.228/30
    reserved:
    - 10.244.7.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.232/30
    reserved:
    - 10.244.7.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.236/30
    reserved:
    - 10.244.7.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.240/30
    reserved:
    - 10.244.7.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.244/30
    reserved:
    - 10.244.7.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.248/30
    reserved:
    - 10.244.7.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.7.252/30
    reserved:
    - 10.244.7.253
    static: []
- name: mysql2
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.8.0/30
    reserved:
    - 10.244.8.1
    static:
    - 10.244.8.2
  - cloud_properties:
      name: random
    range: 10.244.8.4/30
    reserved:
    - 10.244.8.5
    static:
    - 10.244.8.6
  - cloud_properties:
      name: random
    range: 10.244.8.8/30
    reserved:
    - 10.244.8.9
    static:
    - 10.244.8.10
  - cloud_properties:
      name: random
    range: 10.244.8.12/30
    reserved:
    - 10.244.8.13
    static:
    - 10.244.8.14
  - cloud_properties:
      name: random
    range: 10.244.8.16/30
    reserved:
    - 10.244.8.17
    static:
    - 10.244.8.18
  - cloud_properties:
      name: random
    range: 10.244.8.20/30
    reserved:
    - 10.244.8.21
    static:
    - 10.244.8.22
  - cloud_properties:
      name: random
    range: 10.244.8.24/30
    reserved:
    - 10.244.8.25
    static:
    - 10.244.8.26
  - cloud_properties:
      name: random
    range: 10.244.8.28/30
    reserved:
    - 10.244.8.29
    static:
    - 10.244.8.30
  - cloud_properties:
      name: random
    range: 10.244.8.32/30
    reserved:
    - 10.244.8.33
    static:
    - 10.244.8.34
  - cloud_properties:
      name: random
    range: 10.244.8.36/30
    reserved:
    - 10.244.8.37
    static:
    - 10.244.8.38
  - cloud_properties:
      name: random
    range: 10.244.8.40/30
    reserved:
    - 10.244.8.41
    static:
    - 10.244.8.42
  - cloud_properties:
      name: random
    range: 10.244.8.44/30
    reserved:
    - 10.244.8.45
    static:
    - 10.244.8.46
  - cloud_properties:
      name: random
    range: 10.244.8.48/30
    reserved:
    - 10.244.8.49
    static:
    - 10.244.8.50
  - cloud_properties:
      name: random
    range: 10.244.8.52/30
    reserved:
    - 10.244.8.53
    static:
    - 10.244.8.54
  - cloud_properties:
      name: random
    range: 10.244.8.56/30
    reserved:
    - 10.244.8.57
    static:
    - 10.244.8.58
  - cloud_properties:
      name: random
    range: 10.244.8.60/30
    reserved:
    - 10.244.8.61
    static:
    - 10.244.8.62
  - cloud_properties:
      name: random
    range: 10.244.8.64/30
    reserved:
    - 10.244.8.65
    static:
    - 10.244.8.66
  - cloud_properties:
      name: random
    range: 10.244.8.68/30
    reserved:
    - 10.244.8.69
    static:
    - 10.244.8.70
  - cloud_properties:
      name: random
    range: 10.244.8.72/30
    reserved:
    - 10.244.8.73
    static:
    - 10.244.8.74
  - cloud_properties:
      name: random
    range: 10.244.8.76/30
    reserved:
    - 10.244.8.77
    static:
    - 10.244.8.78
  - cloud_properties:
      name: random
    range: 10.244.8.80/30
    reserved:
    - 10.244.8.81
    static:
    - 10.244.8.82
  - cloud_properties:
      name: random
    range: 10.244.8.84/30
    reserved:
    - 10.244.8.85
    static:
    - 10.244.8.86
  - cloud_properties:
      name: random
    range: 10.244.8.88/30
    reserved:
    - 10.244.8.89
    static:
    - 10.244.8.90
  - cloud_properties:
      name: random
    range: 10.244.8.92/30
    reserved:
    - 10.244.8.93
    static:
    - 10.244.8.94
  - cloud_properties:
      name: random
    range: 10.244.8.96/30
    reserved:
    - 10.244.8.97
    static:
    - 10.244.8.98
  - cloud_properties:
      name: random
    range: 10.244.8.100/30
    reserved:
    - 10.244.8.101
    static:
    - 10.244.8.102
  - cloud_properties:
      name: random
    range: 10.244.8.104/30
    reserved:
    - 10.244.8.105
    static:
    - 10.244.8.106
  - cloud_properties:
      name: random
    range: 10.244.8.108/30
    reserved:
    - 10.244.8.109
    static:
    - 10.244.8.110
  - cloud_properties:
      name: random
    range: 10.244.8.112/30
    reserved:
    - 10.244.8.113
    static:
    - 10.244.8.114
  - cloud_properties:
      name: random
    range: 10.244.8.116/30
    reserved:
    - 10.244.8.117
    static:
    - 10.244.8.118
  - cloud_properties:
      name: random
    range: 10.244.8.120/30
    reserved:
    - 10.244.8.121
    static:
    - 10.244.8.122
  - cloud_properties:
      name: random
    range: 10.244.8.124/30
    reserved:
    - 10.244.8.125
    static:
    - 10.244.8.126
  - cloud_properties:
      name: random
    range: 10.244.8.128/30
    reserved:
    - 10.244.8.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.132/30
    reserved:
    - 10.244.8.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.136/30
    reserved:
    - 10.244.8.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.140/30
    reserved:
    - 10.244.8.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.144/30
    reserved:
    - 10.244.8.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.148/30
    reserved:
    - 10.244.8.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.152/30
    reserved:
    - 10.244.8.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.156/30
    reserved:
    - 10.244.8.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.160/30
    reserved:
    - 10.244.8.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.164/30
    reserved:
    - 10.244.8.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.168/30
    reserved:
    - 10.244.8.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.172/30
    reserved:
    - 10.244.8.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.176/30
    reserved:
    - 10.244.8.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.180/30
    reserved:
    - 10.244.8.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.184/30
    reserved:
    - 10.244.8.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.188/30
    reserved:
    - 10.244.8.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.192/30
    reserved:
    - 10.244.8.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.196/30
    reserved:
    - 10.244.8.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.200/30
    reserved:
    - 10.244.8.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.204/30
    reserved:
    - 10.244.8.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.208/30
    reserved:
    - 10.244.8.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.212/30
    reserved:
    - 10.244.8.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.216/30
    reserved:
    - 10.244.8.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.220/30
    reserved:
    - 10.244.8.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.224/30
    reserved:
    - 10.244.8.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.228/30
    reserved:
    - 10.244.8.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.232/30
    reserved:
    - 10.244.8.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.236/30
    reserved:
    - 10.244.8.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.240/30
    reserved:
    - 10.244.8.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.244/30
    reserved:
    - 10.244.8.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.248/30
    reserved:
    - 10.244.8.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.8.252/30
    reserved:
    - 10.244.8.253
    static: []
- name: mysql3
  subnets:
  - cloud_properties:
      name: random
    range: 10.244.9.0/30
    reserved:
    - 10.244.9.1
    static:
    - 10.244.9.2
  - cloud_properties:
      name: random
    range: 10.244.9.4/30
    reserved:
    - 10.244.9.5
    static:
    - 10.244.9.6
  - cloud_properties:
      name: random
    range: 10.244.9.8/30
    reserved:
    - 10.244.9.9
    static:
    - 10.244.9.10
  - cloud_properties:
      name: random
    range: 10.244.9.12/30
    reserved:
    - 10.244.9.13
    static:
    - 10.244.9.14
  - cloud_properties:
      name: random
    range: 10.244.9.16/30
    reserved:
    - 10.244.9.17
    static:
    - 10.244.9.18
  - cloud_properties:
      name: random
    range: 10.244.9.20/30
    reserved:
    - 10.244.9.21
    static:
    - 10.244.9.22
  - cloud_properties:
      name: random
    range: 10.244.9.24/30
    reserved:
    - 10.244.9.25
    static:
    - 10.244.9.26
  - cloud_properties:
      name: random
    range: 10.244.9.28/30
    reserved:
    - 10.244.9.29
    static:
    - 10.244.9.30
  - cloud_properties:
      name: random
    range: 10.244.9.32/30
    reserved:
    - 10.244.9.33
    static:
    - 10.244.9.34
  - cloud_properties:
      name: random
    range: 10.244.9.36/30
    reserved:
    - 10.244.9.37
    static:
    - 10.244.9.38
  - cloud_properties:
      name: random
    range: 10.244.9.40/30
    reserved:
    - 10.244.9.41
    static:
    - 10.244.9.42
  - cloud_properties:
      name: random
    range: 10.244.9.44/30
    reserved:
    - 10.244.9.45
    static:
    - 10.244.9.46
  - cloud_properties:
      name: random
    range: 10.244.9.48/30
    reserved:
    - 10.244.9.49
    static:
    - 10.244.9.50
  - cloud_properties:
      name: random
    range: 10.244.9.52/30
    reserved:
    - 10.244.9.53
    static:
    - 10.244.9.54
  - cloud_properties:
      name: random
    range: 10.244.9.56/30
    reserved:
    - 10.244.9.57
    static:
    - 10.244.9.58
  - cloud_properties:
      name: random
    range: 10.244.9.60/30
    reserved:
    - 10.244.9.61
    static:
    - 10.244.9.62
  - cloud_properties:
      name: random
    range: 10.244.9.64/30
    reserved:
    - 10.244.9.65
    static:
    - 10.244.9.66
  - cloud_properties:
      name: random
    range: 10.244.9.68/30
    reserved:
    - 10.244.9.69
    static:
    - 10.244.9.70
  - cloud_properties:
      name: random
    range: 10.244.9.72/30
    reserved:
    - 10.244.9.73
    static:
    - 10.244.9.74
  - cloud_properties:
      name: random
    range: 10.244.9.76/30
    reserved:
    - 10.244.9.77
    static:
    - 10.244.9.78
  - cloud_properties:
      name: random
    range: 10.244.9.80/30
    reserved:
    - 10.244.9.81
    static:
    - 10.244.9.82
  - cloud_properties:
      name: random
    range: 10.244.9.84/30
    reserved:
    - 10.244.9.85
    static:
    - 10.244.9.86
  - cloud_properties:
      name: random
    range: 10.244.9.88/30
    reserved:
    - 10.244.9.89
    static:
    - 10.244.9.90
  - cloud_properties:
      name: random
    range: 10.244.9.92/30
    reserved:
    - 10.244.9.93
    static:
    - 10.244.9.94
  - cloud_properties:
      name: random
    range: 10.244.9.96/30
    reserved:
    - 10.244.9.97
    static:
    - 10.244.9.98
  - cloud_properties:
      name: random
    range: 10.244.9.100/30
    reserved:
    - 10.244.9.101
    static:
    - 10.244.9.102
  - cloud_properties:
      name: random
    range: 10.244.9.104/30
    reserved:
    - 10.244.9.105
    static:
    - 10.244.9.106
  - cloud_properties:
      name: random
    range: 10.244.9.108/30
    reserved:
    - 10.244.9.109
    static:
    - 10.244.9.110
  - cloud_properties:
      name: random
    range: 10.244.9.112/30
    reserved:
    - 10.244.9.113
    static:
    - 10.244.9.114
  - cloud_properties:
      name: random
    range: 10.244.9.116/30
    reserved:
    - 10.244.9.117
    static:
    - 10.244.9.118
  - cloud_properties:
      name: random
    range: 10.244.9.120/30
    reserved:
    - 10.244.9.121
    static:
    - 10.244.9.122
  - cloud_properties:
      name: random
    range: 10.244.9.124/30
    reserved:
    - 10.244.9.125
    static:
    - 10.244.9.126
  - cloud_properties:
      name: random
    range: 10.244.9.128/30
    reserved:
    - 10.244.9.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.132/30
    reserved:
    - 10.244.9.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.136/30
    reserved:
    - 10.244.9.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.140/30
    reserved:
    - 10.244.9.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.144/30
    reserved:
    - 10.244.9.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.148/30
    reserved:
    - 10.244.9.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.152/30
    reserved:
    - 10.244.9.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.156/30
    reserved:
    - 10.244.9.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.160/30
    reserved:
    - 10.244.9.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.164/30
    reserved:
    - 10.244.9.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.168/30
    reserved:
    - 10.244.9.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.172/30
    reserved:
    - 10.244.9.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.176/30
    reserved:
    - 10.244.9.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.180/30
    reserved:
    - 10.244.9.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.184/30
    reserved:
    - 10.244.9.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.188/30
    reserved:
    - 10.244.9.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.192/30
    reserved:
    - 10.244.9.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.196/30
    reserved:
    - 10.244.9.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.200/30
    reserved:
    - 10.244.9.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.204/30
    reserved:
    - 10.244.9.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.208/30
    reserved:
    - 10.244.9.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.212/30
    reserved:
    - 10.244.9.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.216/30
    reserved:
    - 10.244.9.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.220/30
    reserved:
    - 10.244.9.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.224/30
    reserved:
    - 10.244.9.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.228/30
    reserved:
    - 10.244.9.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.232/30
    reserved:
    - 10.244.9.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.236/30
    reserved:
    - 10.244.9.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.240/30
    reserved:
    - 10.244.9.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.244/30
    reserved:
    - 10.244.9.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.248/30
    reserved:
    - 10.244.9.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.9.252/30
    reserved:
    - 10.244.9.253
    static: []
properties:
  app_domains:
  - 10.244.0.34.xip.io
  cf:
    admin_password: admin
    admin_username: admin
    api_url: https://api.10.244.0.34.xip.io
    apps_domain: 10.244.0.34.xip.io
    skip_ssl_validation: true
  default_mysql_cluster_ips:
  - 10.244.7.2
  - 10.244.8.2
  - 10.244.9.2
  domain: 10.244.0.34.xip.io
  nats:
    machines:
    - 10.244.0.6
    password: nats
    port: 4222
    user: nats
releases:
- name: openpaas-mysql
  version: latest
resource_pools:       # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:     # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    name: random
  name: mysql_z1         # 고유한 resource pool 이름
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent#stemcell 이름(필수)
    version: latest           # stemcell 버전(필수)
- cloud_properties:
    name: random
  name: mysql_z2
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: mysql_z3
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: proxy_z1
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: proxy_z2
  network: mysql2
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: openpaas-mysql-broker_z1
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: openpaas-mysql-broker_z2
  network: mysql2
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
- cloud_properties:
    name: random
  name: errands_z1
  network: mysql1
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest
update:
  canaries: 1         # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000     # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1        # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000    # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
</pre>

-	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.
>$bosh deployment {Deployment manifest 파일 PATH}<br>
>$bosh deployment openpaas-mysql-lite.yml<br>
>![mysql_bosh_lite_2.3.04]

-	MySQL 서비스팩을 배포한다.
>$bosh deploy<br>
>![mysql_bosh_lite_2.3.05]<br>
>![mysql_bosh_lite_2.3.06]<br>
>![mysql_bosh_lite_2.3.07]

-	배포된 MySQL 서비스팩을 확인한다.
>$bosh vms<br>
>![mysql_bosh_lite_2.3.08]<br>
>![mysql_bosh_lite_2.3.09]

###MySQL 서비스 브로커 등록
Mysql 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 MySQL 서비스 브로커를 등록해 주어야 한다.  
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스브로커를 등록할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	서비스 브로커 목록을 확인한다.
>$cf service-brokers<br>
>![mysql_bosh_lite_2.4.01]

-	MySQL 서비스 브로커를 등록한다.
>$cf create-service-broker {서비스팩 이름}{서비스팩 사용자ID}{서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
>-	서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.<br>
>-	서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.<br>
>-	서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.<br><br>
>$ cf create-service-broker mysql-service-broker admin password http://p-mysql.10.244.0.34.xip.io<br>
>![mysql_bosh_lite_2.4.02]

-	등록된 MySQL 서비스 브로커를 확인한다.
>$cf service-brokers<br>
>![mysql_bosh_lite_2.4.03]

-	접근 가능한 서비스 목록을 확인한다.
>$cf service-access<br>
>![mysql_bosh_lite_2.4.04]<br>
>서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

-	특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)
>$cf enable-service-access p-mysql<br>
>$cf service-access<br>
>![mysql_bosh_lite_2.4.05]

#MySQL 연동 Sample Web App 설명
본 Sample Web App은 개발형 클라우드 플랫폼에 배포되며 MySQL의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.


###Sample Web App 구조
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. App을 배포하여 구동시 Bind 된 MySQL 서비스 연결정보로 접속하여 초기 데이터를 생성하게 된다. 배포 완료 후 정상적으로 App 이 구동되면 브라우져나 curl로 해당 App에 접속 하여 MySQL 환경정보(서비스 연결 정보)와 초기 적재된 데이터를 보여준다.

Sample Web App 구조는 다음과 같다.

|  이름  |  설명  |
|--------|:-------|
|src |Sample 소스 디렉토리|
|manifest |개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일|
|pom.xml |메이븐 project 설정 파일|
|target |메이븐 빌드시 생성되는 디렉토리(war 파일, classes 폴더 등)|
<br>
<br>
-	OpenPaaS-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 MySQL Sample Web App인 hello-spring-mysql를 복사한다.
>$ls -all<br>
>![mysql_bosh_lite_3.1.01]

-	OpenPaaS-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 MySQL Sample Web App인 hello-spring-mysql를 복사한다.
>$cf service-access<br>
>![mysql_bosh_lite_3.1.01]

-	접근 가능한 서비스 목록을 확인한다.
>$ls -all<br>
>![mysql_bosh_lite_3.1.01]

###개방형 클라우드 플랫폼에서 서비스 신청
Sample Web App에서 MySQL 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.  
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.<br>
><div>$cf marketplace</div>
>![mysql_bosh_lite_3.2.01]<br><br>

-	Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.<br>
><div>$cf create-service {서비스명} {서비스플랜} {내서비스명}</div>
>-	서비스명 : p-mysql로 Marketplace에서 보여지는 서비스 명칭이다.
>-	서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. MySQL 서비스는 100mb, 1gb를 지원한다.
>-	내 서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.<br>
><div>$cf create-service p-mysql 100mb mysql-service-instance</div>
>![mysql_bosh_lite_3.2.02]<br><br>

-	생성된 MySQL 서비스 인스턴스를 확인한다.<br>
><div>$cf services</div>
>![mysql_bosh_lite_3.2.03]<br><br>

###Sample Web App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample Web App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 MySQL 서비스를 이용한다.  
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	Sample Web App 디렉토리로 이동하여 manifest 파일을 확인한다.<br>
><div>$cd hello-spring-mysql</div>
><div>$vi manifest.yml</div>
	<pre>applications:
		- name: hello-tomcat-mysql <b>#배포할 App 이름</b>
		  instances: 1 <b># 배포 인스턴스 수</b>
		  path: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war <b>#배포하는 App 파일 PATH</b></pre>
>참고: target/hello-spring-mysql-1.0.0-BUILD-SNAPSHOT.war파일이 존재 하지 않을 경우 mvn 빌드를 수행 하면 파일이 생성된다.<br><br>

-	--no-start 옵션으로 App을 배포한다.  
	--no-start: App 배포시 구동은 하지 않는다.<br>
><div>$cf push --no-start</div>
>![mysql_bosh_lite_3.3.01]

-	배포된 Sample App을 확인하고 로그를 수행한다.<br>
><div>$cf apps</div>
>![mysql_bosh_lite_3.3.02]<br><br>
><div>$ cf logs {배포된 App명}<br>
>$ cf logs hello-tomcat-mysql</div>
>![mysql_bosh_lite_3.3.03]<br><br>

-	Sample Web App에서 생성한 서비스 인스턴스 바인드 신청을 한다.<br>
><div>$cf bind-service hello-tomcat-mysql mysql-service-instance</div>
>![mysql_bosh_lite_3.3.04]

-	바인드가 적용되기 위해서 App을 재기동한다.<br>
><div>$cf restart hello-tomcat-mysql</div>
>![mysql_bosh_lite_3.3.05]<br><br>

-	(참고) 바인드 후 App구동시 Mysql 서비스 접속 에러로 App 구동이 안될 경우 보안 그룹을 추가한다.<br>  
>-	rule.json 화일을 만들고 아래와 같이 내용을 넣는다.
><div>
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
></div>
>-	보안 그룹을 생성한다.
><div>$cf create-security-group p-mysql rule.json</div>
>![mysql_bosh_lite_3.3.06]<br><br>
>
>-	모든 App에 Mysql 서비스를 사용할수 있도록 생성한 보안 그룹을 적용한다.
><div>$cf bind-running-security-group p-mysql</div>
>![mysql_bosh_lite_3.3.07]<br><br>
>
>-	App을 리부팅 한다.
><div>$cf restarthello-tomcat-mysql</div>
>![mysql_bosh_lite_3.3.08]<br><br>

-	App이 정상적으로 MySQL 서비스를 사용하는지 확인한다.
>-	curl 로 확인 
><div>$curl hello-tomcat-mysql.10.244.0.34.xip.io</div>
>![mysql_bosh_lite_3.3.09]<br><br>

#MySQL Client 툴 접속
Application에 바인딩된 MySQL 서비스 연결정보는 Private IP로 구성되어 있기 때문에 MySQL Client 툴에서 직접 연결할수 없다. 따라서 MySQL Client 툴에서 SSH 터널, Proxy 터널 등을 제공하는 툴을 사용해서 연결하여야 한다. 본 가이드는 SSH 터널을 이용하여 연결 하는 방법을 제공하며 MySQL Client 툴로써는 오픈 소스인 HeidiSQL로 가이드한다. 또한 Bosh lite를 AWS 환경에서 구성 한 경우를 전제로 하였다. AWS에서 Bosh lite를 구성하면 Vagrant VM이 생성되는데 Vagrant VM 에서는 서비스팩의 Private IP 와 해당 포트로 접근이 가능하도록 구성되어 있다.

###HeidiSQL 설치 및 연결
HeidiSQL 프로그램은 무료로 사용할 수 있는 오픈소스 소프트웨어이다.

-	HeidiSQL을 다운로드 하기 위해 아래 URL로 이동하여 설치파일을 다운로드 한다.<br>
[http://www.heidisql.com/download.php](http://www.heidisql.com/download.php)<br>
![mysql_bosh_lite_4.1.01]<br><br>

-	다운로드한 설치파일을 실행한다.
![mysql_bosh_lite_4.1.02]<br><br>

-	HeidSQL 설치를 위한 안내사항이다. Next 버튼을 클릭한다.
![mysql_bosh_lite_4.1.03]<br><br>

-	프로그램 라이선스에 관련된 내용이다. 동의(I accept the agreement)에 체크 후 Next 버튼을 클릭한다.
![mysql_bosh_lite_4.1.04]<br><br>

-	HeidiSQL을 설치할 경로를 설정 후 Next 버튼을 클릭한다.  
별도의 경로 설정이 필요 없을 경우 default로 C드라이브 Program Files 폴더에 설치가 된다.
![mysql_bosh_lite_4.1.05]<br><br>

-	설치 완료 후 시작메뉴에 HeidiSQL 바로가기 아이콘의 이름을 설정하는 과정이다.  
Next 버튼을 클릭하여 다음 과정을 진행한다.  
![mysql_bosh_lite_4.1.06]<br><br>

-	체크박스가 4개가 있습니다. 아래의 경우를 고려하여 체크 및 해제를 한다.
	
	>바탕화면에 바로가기 아이콘을 생성할 경우  
	>sql확장자를 HeidiSQL 프로그램으로 실행할 경우  
	>heidisql 공식 홈페이지를 통해 자동으로 update check를 할 경우  
	>heidisql 공식 홈페이지로 자동으로 버전을 전송할 경우

	체크박스에 체크 설정/해제를 완료했다면 Next 버튼을 클릭한다.
![mysql_bosh_lite_4.1.07]<br><br>

-	설치를 위한 모든 설정이 한번에 출력된다.확인 후 Install 버튼을 클릭하여 설치를 진행한다.
![mysql_bosh_lite_4.1.08]<br><br>

-	Finish 버튼 클릭으로 설치를 완료한다.  
![mysql_bosh_lite_4.1.09]<br><br>

-	HeidiSQL을 실행했을 때 처음 뜨는 화면이다. 이 화면에서 Server에 접속하기 위한 profile을 설정/저장하여 접속할 수 있다. 신규 버튼을 클릭한다.  
![mysql_bosh_lite_4.1.10]<br><br>

-	어떤 Server에 접속하기 위한 Connection 정보인지 별칭을 입력한다.  
![mysql_bosh_lite_4.1.11]<br><br>

-	네트워크 유형의 목록에서 MySQL(SSH tunel)을 선택한다.
![mysql_bosh_lite_4.1.12]<br><br>

-	아래 붉은색 영역에 접속하려는 서버 정보를 모두 입력한다.  
![mysql_bosh_lite_4.1.13]<br>
서버 정보는 Application에 바인드되어 있는 서버 정보를 입력한다. cf env <app_name> 명령어로 이용하여 확인한다.  
**예)** $cf env hello-tomcat-mysql
![mysql_bosh_lite_4.1.14]<br><br>

-	SSH 터널 탭을 클릭하고 Vagrant VM 정보를 입력하고 개인 키 파일을 불러온다. 개인키는 AWS에서 인스턴스 접속을 위한 공개키(.pem 파일)를puttygen을 이용하여 개인키(.ppk)로 변환한다. plink.exe 위치 입력은 Putty에서 제공하는 plink.exe 실행 위치를 넣어주고 만일 해당 파일이 없을 경우 plink.exe 내려받기 링크를 클릭하여 다운받는다. 로컬 포트 정보는 임의로 넣고 열기 버튼을 클릭하면 Mysql 데이터베이스에 접속한다.  
(참고) 만일 개인 키 없이 ID/Password로 접속이 가능한 경우에는 개인키 대신 사용자 이름과 암호를 입력한다.
![mysql_bosh_lite_4.1.15]<br><br>

-	접속이 완료되면 좌측에 스키마 정보가 나타난다.하지만 초기설정은 테이블, 뷰, 프로시져, 함수, 트리거, 이벤트 등 모두 섞여 있어서 한눈에 구분하기가 힘들어서 접속한 DB 별칭에 마우스 오른쪽 클릭 후 "트리 방식 옵션" - "객체를 유형별로 묶기"를 클릭하면 아래 화면과 같이 각 유형별로 구분이된다.  
![mysql_bosh_lite_4.1.16]<br><br>

-	우측 화면에 쿼리 탭을 클릭하여 Query문을 작성한 후 실행 버튼(삼각형)을 클릭한다.
쿼리문에 이상이 없다면 정상적으로 결과를 얻을 수 있을 것이다.
![mysql_bosh_lite_4.1.17]<br><br>


[mysql_bosh_lite_1.3.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_1.3.01.png
[mysql_bosh_lite_2.2.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.01.png
[mysql_bosh_lite_2.2.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.02.png
[mysql_bosh_lite_2.2.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.03.png
[mysql_bosh_lite_2.2.04]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.04.png
[mysql_bosh_lite_2.2.05]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.05.png
[mysql_bosh_lite_2.2.06]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.06.png
[mysql_bosh_lite_2.2.07]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.07.png
[mysql_bosh_lite_2.2.08]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.08.png
[mysql_bosh_lite_2.3.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.01.png
[mysql_bosh_lite_2.3.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.02.png
[mysql_bosh_lite_2.3.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.03.png
[mysql_bosh_lite_2.3.04]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.04.png
[mysql_bosh_lite_2.3.05]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.05.png
[mysql_bosh_lite_2.3.06]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.06.png
[mysql_bosh_lite_2.3.07]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.07.png
[mysql_bosh_lite_2.3.08]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.08.png
[mysql_bosh_lite_2.3.09]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.3.09.png
[mysql_bosh_lite_2.4.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.4.01.png
[mysql_bosh_lite_2.4.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.4.02.png
[mysql_bosh_lite_2.4.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.4.03.png
[mysql_bosh_lite_2.4.04]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.4.04.png
[mysql_bosh_lite_2.4.05]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.4.05.png
[mysql_bosh_lite_3.1.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.1.01.png
[mysql_bosh_lite_3.2.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.2.01.png
[mysql_bosh_lite_3.2.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.2.02.png
[mysql_bosh_lite_3.2.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.2.03.png
[mysql_bosh_lite_3.3.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.01.png
[mysql_bosh_lite_3.3.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.02.png
[mysql_bosh_lite_3.3.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.03.png
[mysql_bosh_lite_3.3.04]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.04.png
[mysql_bosh_lite_3.3.05]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.05.png
[mysql_bosh_lite_3.3.06]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.06.png
[mysql_bosh_lite_3.3.07]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.07.png
[mysql_bosh_lite_3.3.08]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.08.png
[mysql_bosh_lite_3.3.09]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_3.3.09.png
[mysql_bosh_lite_4.1.01]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.01.png
[mysql_bosh_lite_4.1.02]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.02.png
[mysql_bosh_lite_4.1.03]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.03.png
[mysql_bosh_lite_4.1.04]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.04.png
[mysql_bosh_lite_4.1.05]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.05.png
[mysql_bosh_lite_4.1.06]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.06.png
[mysql_bosh_lite_4.1.07]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.07.png
[mysql_bosh_lite_4.1.08]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.08.png
[mysql_bosh_lite_4.1.09]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.09.png
[mysql_bosh_lite_4.1.10]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.10.png
[mysql_bosh_lite_4.1.11]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.11.png
[mysql_bosh_lite_4.1.12]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.12.png
[mysql_bosh_lite_4.1.13]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.13.png
[mysql_bosh_lite_4.1.14]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.14.png
[mysql_bosh_lite_4.1.15]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.15.png
[mysql_bosh_lite_4.1.16]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.16.png
[mysql_bosh_lite_4.1.17]:/images/openpaas-service/mysql/mysql_bosh_lite/mysql_bosh_lite_4.1.17.png