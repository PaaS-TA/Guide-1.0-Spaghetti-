## Table of Contents
1. [문서 개요](#1)
    * [1.1. 목적](#1.1)
    * [1.2. 범위](#1.2)
    * [1.3. 시스템 구성도](#1.3)
    * [1.4. 참고자료](#1.4)
2. [RabbitMQ 서비스팩 설치] (#2)
    * [2.1. 설치전 준비사항](#2.1)
    * [2.2. RabbitMQ 서비스 릴리즈 업로드](#2.2)
    * [2.3. RabbitMQ 서비스 Deployment 파일 수정 및 배포](#2.3)
    * [2.4. RabbitMQ 서비스 브로커 등록](#2.4)
3. [RabbitMQ 연동 Sample App 설명](#3)
    * [3.1. Sample App 구조](#3.1)
    * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#3.2)
    * [3.3. Sample App에 서비스 바인드 신청 및 App 확인](#3.3)



## <a name="1"/>1. 문서 개요
#### <a name="1.1"/>1.1 목적
본 문서(RabbitMQ 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 RabbitMQ 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 RabbitMQ 서비스를 사용하는 방법을 기술하였다.

#### <a name="1.2"/>1.2 범위
설치 범위는 RabbitMQ 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

#### <a name="1.3"/>1.3 시스템 구성도
본 문서의 설치된 시스템 구성도로써, RabbitMQ(2대), RabbitMQ 서비스 브로커, haproxy로 최소사항을 구성하였다.

![시스템 구성도][rabbitmq_openstack_(1)]

| 구분 | 스펙
| ---- | -----------
| openpaasrmq-broker | 2vCPU / 2GB RAM / 20GB Disk+2GB(영구적 Disk)
| haproxy | 2vCPU / 2GB RAM / 20GB Disk+1GB(영구적 Disk)
| rmq1 | 2vCPU / 2GB RAM / 20GB Disk+2GB(영구적 Disk)
| rmq2 | 2vCPU / 2GB RAM / 20GB Disk+2GB(영구적 Disk)

#### <a name="1.4"/>1.4 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)

[**http://docs.cloudfoundry.org**](http://docs.cloudfoundry.org)

## <a name="2"/>2. RabbitMQ 서비스팩 설치
#### <a name="2.1"/>2.1 설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH CLI 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH CLI가 설치 되어 있지 않을 경우 먼저 BOSH CLI 설치 가이드 문서를 참고 하여 BOSH CLI를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

#### <a name="2.2"/>2.2 RabbitMQ 서비스 릴리즈 업로드

-	OpenPaaS-Services.zip 파일 압축을 풀고 폴더안에 있는 RabbitMQ 서비스 릴리즈 openpaas-rabbitmq-release-beta-1.0.tgz 파일을 복사한다.
업로드할 openpaas-rabbitmq-release-beta-1.0.tgz 파일을 확인한다.

><div>$ ls –all</div>
![rabbitmq_openstack_(2)]

-	업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$ bosh releases</div>
![rabbitmq_openstack_(3)]
<p>RabbitMQ 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</p>

-	RabbitMQ 서비스 릴리즈 파일을 업로드한다.

><div>$ bosh upload release {서비스 릴리즈 파일 PATH}<br>
$ bosh upload release openpaas-rabbitmq-release-beta-1.0.tgz</div>
![rabbitmq_openstack_(4)]<br>
![rabbitmq_openstack_(5)]<br>
![rabbitmq_openstack_(6)]<br>
![rabbitmq_openstack_(7)]

-	업로드 된 RabbitMQ 릴리즈를 확인한다.

><div>$ bosh releases</div>
![rabbitmq_openstack_(8)]
<p>RabbitMQ 서비스 릴리즈가 업로드 되어 있는 것을 확인</p>

#### <a name="2.3"/>2.3 RabbitMQ 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 software를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 OpenStack용 RabbitMQ Deployment 화일인 openpaas-rabbitmq-openstack.yml 를 복사한다.
-   다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-rabbitmq-openstack.yml)

><div>$ ls –all</div>
![rabbitmq_openstack_(9)]

-	Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인 할 수 있다.

><div>$ bosh status</div>
![rabbitmq_openstack_(10)]

-	Deploy시 사용할 Stemcell을 확인한다. (Stemcell 3016 버전 사용)

><div>$ bosh stemcells</div>
![rabbitmq_openstack_(11)]<br><br>
Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 3016 버전을 업로드를 해야 한다.

-	openpaas-rabbitmq-openstack.yml Deployment 파일을 서버 환경에 맞게 수정한다.

<pre>
$ vi openpaas-rabbitmq-openstack.yml

openpaas-rabbitmq-openstack 설정 파일 내용
---
name: openpaas-rabbitmq-service                       # 서비스 배포이름(필수)
director_uuid: xxxxx                                  #bosh status 에서 확인한 Director UUID을 입력(필수)

releases:
- name: openpaas-rabbitmq                             #서비스 릴리즈 이름(필수)
  version: beta-1.0                                   #서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전

update:
  canaries: 1                                         # canary 인스턴스 수(필수)
  canary_watch_time: 30000-600000                     # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 1                                    # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)
  update_watch_time: 30000-600000                     # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)

compilation:                                          # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:                                   # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)
    instance_type: m1.medium                          # 인스턴스 타입: Flavors 타입 (필수)
  network: openpaas_network                           # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true                         # 컴파일지 VM 재사용 여부(옵션)
  workers: 6                                          # 컴파일 하는 가상머신의 최대수(필수)

jobs:
- instances: 1                                        # job 인스턴스 수(필수)
  name: openpaas-rmq-broker                           # 작업 이름(필수): rabbitmq 서비스 브로커
  networks:                                           # 네트워크 구성정보
  - name: openpaas_network                            # Networks block에서 선언한 network 이름(필수)
    static_ips:
    - 10.10.7.81                                      # 사용할 IP addresses 정의(필수): Rabbitmq 브로커 IP
  persistent_disk: 2048                               # 영구적 디스크 사이즈 정의(옵션): 2G
  resource_pool: services-small                       # Resource Pools block에 정의한 resource pool 이름(필수)
  templates:
  - name: rabbitmq-broker                             # job template 이름(필수)
    release: openpaas-rabbitmq                        # 릴리즈 이름(필수)
  syslog_aggregator: null
- instances: 2
  name: rmq                                           # 작업 이름(필수): rabbitmq 서버
  networks:
  - name: openpaas_network
    static_ips:
    - 10.10.7.83
    - 10.10.7.84
  persistent_disk: 2048
  resource_pool: services-small
  templates:
  - name: rabbitmq-server
    release: openpaas-rabbitmq
  syslog_aggregator: null
- instances: 1
  name: haproxy                                       # 작업 이름(필수): Haproxy
  networks:
  - name: openpaas_network
    static_ips:
    - 10.10.7.82
  persistent_disk: 1024
  resource_pool: services-small
  templates:
  - name: rabbitmq-haproxy
    release: openpaas-rabbitmq
  syslog_aggregator: null
- instances: 1
  lifecycle: errand                                   # bosh deploy시 vm에 생성되어 설치 되지 않고 bosh errand 로 실행할때 설정, 주로 테스트 용도에 쓰임
  name: broker-registrar
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: 10.10.7.81
      name: rabbitmq-sb
      password: admin
      username: admin
      protocol: http
      port: 4567
  resource_pool: services-small
  templates:
  - name: broker-registrar
    release: openpaas-rabbitmq
- instances: 1
  lifecycle: errand
  name: broker-deregistrar
  networks:
  - name: openpaas_network
  properties:
    broker:
      host: 10.10.7.81
      name: rabbitmq-sb
      password: admin
      username: admin
      protocol: http
      port: 4567
  resource_pool: services-small
  templates:
  - name: broker-deregistrar
    release: openpaas-rabbitmq
networks:                                           # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, 네트워크 구성은 네트워크 담당자에게 문의 하여 작성 요망
- name: openpaas_network
  subnets:
  - cloud_properties:
      net_id: bfef7fe1-bf86-4e29-842e-84a4d1cff110  # OpenStack에서 사용하는 network 이름 아이디(필수)
      security_groups:
      - cf-security                                 # OpenStack 에서 사용하는 접근 시큐리티 이름 이름(필수)
    dns:                                            # DNS 정보
    - 10.10.5.108
    - 8.8.8.8
    gateway: 10.10.7.1
    range: 10.10.7.0/24
    reserved: # 설치시 제외할 IP 설정 (OpenStack 에서는 서비스팩 설치 구간 이외에는 IP 제외 설정을 해줘야 오류가 나지 않음.
    - 10.10.7.2 - 10.10.7.80
    #- 10.30.40.1 - 10.30.40.100
    #- 10.30.40.201 - 10.30.254.254
    static:
    - 10.10.7.81 - 10.10.7.90                       # 사용 가능한 IP 설정
  type: manual
properties:
  cf:
    admin_password: admin                           # CF 어드민 아이디 비밀번호(필수)
    admin_username: admin                           # CF 어드민 아이디 (필수)
    api_url: http://api.controller.open-paas.com    # CF API url(필수)
    domain: controller.open-paas.com                # CF 도메인(필수)
    nats:                                           # CF 설치시 설치한 nats 정보 (필수)
      host: 10.10.3.11
      port: 4222
      username: nats
      password: admin
  route-registrar:
    target_ip: 10.10.7.82                           # 라우터 타켓 IP (haproxy IP)
  rabbitmq-server:
    plugins:                                        # rabbitmq 플러그인 정보(필수)
    - rabbitmq_management
    - rabbitmq_mqtt
    - rabbitmq_stomp
    administrators:
      broker:
        username: broker                            #브로커에서 rabbitmq 서버에 접근하는 유저 아이디(필수)
        password: CkY26kTuAyZT8r2
    static_ips:                                     # rabbitmq 서버 IP 목록
    - 10.10.7.83
    - 10.10.7.84
    ssl:                                            # SSL 정보(필수)
      cert: |
        -----BEGIN CERTIFICATE-----
        MIIC+zCCAeOgAwIBAgIBAjANBgkqhkiG9w0BAQUFADAnMRUwEwYDVQQDEwxNeVRl
        c3RSb290Q0ExDjAMBgNVBAcTBTY0MTAzMB4XDTE0MDkwNDA3MjIwOFoXDTI0MDkw
        MTA3MjIwOFowKjEXMBUGA1UEAxMObWVyY3VyaW8ubG9jYWwxDzANBgNVBAoTBnNl
        cnZlcjCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANxxSzf958VIm8lp
        qQ4BHSmz1z8yU/KEKSbuEfIqpGwpVx6TZ+ZYiXa0cMV2pE7UKR4OyJiuvtvv9kzu
        6g+HTXmJo2cqVonGCAMp6d9TkCAMaMF76IrbLyGmvXQDcjOmWarvsGHW/w6gJpw9
        svDP9EXyXTBUfaJq3T8+9UQBfMsL4dHwAt79YgvSQLsYiIi2rzQixK/4PHFeHf3G
        I/UDgjG0YG9iCWp2g1Sc3Z6hYB/0pOCBxE7LCrSGS6/M/7c2569yK7NqSSNN7Lqz
        ZoQSF1NYE6KRd2MK2A0QaKrn9v8K5/Lp0fk70bvwtLxTWtp3wq3bYQg8UdqY/6R8
        UATS/aMCAwEAAaMvMC0wCQYDVR0TBAIwADALBgNVHQ8EBAMCBSAwEwYDVR0lBAww
        CgYIKwYBBQUHAwEwDQYJKoZIhvcNAQEFBQADggEBAHC89mK1HJgDqwxjsGpa3V7t
        Nuqe/XxEIUUN3Lm4gBLKq4wed4c6z4csv16f3uL9cypyHPSrQmMPV7CDgWLX4F7g
        YN9PGaVfIp/rGNsDWJEVNU2rfIEDIUfcL+o844jE8CtmzZ4bGVrCHqKW5pAraai1
        o5h3JaU4yDLo49rqPeRft2n/gj+5E3gi/1TsnuLuzB7kK1gaTTOrV3GASiGokCEN
        4v1ZjaqMSGMcwA/esaLv2N6UYJgd5lyJ7PEL4ddE8QCTo2EPhYyltLxRqOjrxa+5
        KONA94PDj14gOSSsoXkoj7gWQsuHT2RXmurYXk4/PkS+k1j0+ZCzKi/ZxF5jt50=
        -----END CERTIFICATE-----
      key: |
        -----BEGIN RSA PRIVATE KEY-----
        MIIEowIBAAKCAQEA3HFLN/3nxUibyWmpDgEdKbPXPzJT8oQpJu4R8iqkbClXHpNn
        5liJdrRwxXakTtQpHg7ImK6+2+/2TO7qD4dNeYmjZypWicYIAynp31OQIAxowXvo
        itsvIaa9dANyM6ZZqu+wYdb/DqAmnD2y8M/0RfJdMFR9omrdPz71RAF8ywvh0fAC
        3v1iC9JAuxiIiLavNCLEr/g8cV4d/cYj9QOCMbRgb2IJanaDVJzdnqFgH/Sk4IHE
        TssKtIZLr8z/tzbnr3Irs2pJI03surNmhBIXU1gTopF3YwrYDRBoquf2/wrn8unR
        +TvRu/C0vFNa2nfCrdthCDxR2pj/pHxQBNL9owIDAQABAoIBAEaVH/h1m9hXP0pm
        QnLxeFz58FmTy71DA8orQCPB85OOSQ7JM+NVRe50KAeRzVpQJDozkZcRnJrfYoWP
        mfIO6IeZwnAt5jeG6Nnaeb/ACc25K3GIMxUCyTyFHvQehlpzURKiXF93b4NHVBWC
        y+C/NP+QcrYe13tBrEWr6uwzbbm1JABlXXbMoK3QaEv+IgbDg6YtW7mqkjQpjW4K
        oLUtmBUnVwT4RzrNEcGmY4jLy2F1HVfIwaqm66V8iOAtv+oGvSv+WTca9ddjfBrr
        ovSJPXMOEt5lf8Yj+j6B00xUDykyzL3emovGYiH0QHRKSYVjnEYkcfvWwa166mE5
        BCQ6IgECgYEA92jrvhpkoQmNV/d61iQvO7VlbWk4WQhXdooKwubqUQ7UTXCIHDHH
        5mDueejqd7lX9yYgZtxxi6p4GVVoK6CZB64VqwfC6yh1UjZvgA8lEPhRwLd77Ii/
        yaNnPpvEzUNxBP9bHg/L41LErwLp2IjPU+EEfkNwr/JjmDPK/StCrAMCgYEA5Bit
        PLbPUVOnHbTM3GXT8E1wtAGrvGLt2EGQ13GrVG7pS4zEgFykszhKe7qaQrZw+CK8
        aAsVZLtKoUPEkjZaP9A5/Ka5EdMbg2fFDGuaVb+CAZCCMQSa4wyAo5dkaeNlslbf
        HlzJdE+38uGW0kF93Ba2YlN5lSro44jbB7ufReECgYEA1vhjXZqxNPuHxJdUNBzc
        rZS/vIFgZHvLA0SOdSsPDb9E2MSef2EBwDsY+vRWRhDVTfnizX1da22DTP/Ys4+n
        Z/Tq/uCPjKRqzDRgTXTbUBNzdW/4TV3Exv8kBlJyfafL2/vEwCoZjjusiiSN64d0
        Hf22Wb9feoGx592Y0sFz/OECgYA8Nhdk84z+luqCePurImY0BP0/R0GiVSsb7xLh
        DGyBuVV0UBB/IfWIKc/foX6fJbHkjqbVgDeBg79LmqwFPLvOJJVqEEjCX8vfgEEP
        6kevYwJaj18OuXx5ew9qH8it+w0Zar0zMqMrRO/1+X7quccTlhtHEWoC6nTXu4S2
        Id0FgQKBgCGNRs4rPJ4MrKn/khm2yVHG0MKKJQRhY4k5CKFq+89WQZiHA2n1z72M
        Pg/ph3WFwppZs/uEUuDmdTpM/9nIkIR4TUH55xi4zJbfWXLbKvUKCOjkze3U8t1S
        x4fDjuJAz2rzIMyfYaCQWB1MPrwk/247HADyqEB7tsRJdKpF0rBe
        -----END RSA PRIVATE KEY-----
      cacert: |
        -----BEGIN CERTIFICATE-----
        MIIC7jCCAdagAwIBAgIJAOVQO4z11jrPMA0GCSqGSIb3DQEBBQUAMCcxFTATBgNV
        BAMTDE15VGVzdFJvb3RDQTEOMAwGA1UEBxMFNjQxMDMwHhcNMTQwOTA0MDcyMjA4
        WhcNMjQwOTAxMDcyMjA4WjAnMRUwEwYDVQQDEwxNeVRlc3RSb290Q0ExDjAMBgNV
        BAcTBTY0MTAzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuXXjHbjm
        LnX8r04/XCQNroV/n483LmhVzZhpR1Fe3iV7aR/QNnMlIoNlHvccxydnO/AOYkN9
        97oGj99oPZnUElYGbQCoFLwthcoEXRkaJCqNRSc4JiZbqkqbeZGUExXcYPNUaMjJ
        2JRmn5V3RofaodPrT2SkXsXwDJlj+Um1yDIOjJ7yGzbrVzT89Nduo7ehQKmb6YIn
        EIf2cP5yviMrPVi4mqNwUjKhnAiENz1t0c53HnjYssG7TsjZiE9fxcqDtisG7QTU
        MNkqv6P2WtOYITi6AxB7B5db/ou0BOLZ7KSUa4b/j9o13nrFQyS0xf7jHmTcOO+r
        4jmHirwcAXzo3wIDAQABox0wGzAMBgNVHRMEBTADAQH/MAsGA1UdDwQEAwIBBjAN
        BgkqhkiG9w0BAQUFAAOCAQEASuQmBRxtMRx1/Oiozh+MiJLrcN9Q53BrQ2tl8QeG
        SUQ5r1jBezBhGDN684TJOgZrgQrukspN0dT9i41rRGhu1IXBgkehChfR7PYt5vww
        MPz2uCYoAQZJgbbSK+X7ZNE4E3Tao+hrUKL3vu9awEptPdkUH4vBBT88GzzqRJxw
        /BmqGdTGCLsZzlO0Jrc6o9NZLPh4SEn3d22Dz2yzN5x6rxWS1yHWRK/a7eEn0gkd
        NqEeEGnW/T0WA/FosIxUEXyP1d252yeeV47LfJsV6qHg0ksRPQFJfW71Nzi3NQTr
        3nizs9vfxcMfGQ88CyUQSvZ4CVdF3lYbw8a96NHJH71ROQ==
        -----END CERTIFICATE-----
  rabbitmq-haproxy:                                   # rabbitmq haproxy 에서 허용하는 포트 목록(필수)
    ports:
    - 5672
    - 5671
    - 1883
    - 8883
    - 61613
    - 61614
    - 15672
    - 15674
    server_ips:                                       # rabbitmq 서버 IP 목록(필수)
    - 10.10.7.83
    - 10.10.7.84
    stats:
      password: admin                                 # 서비스 브로커 비밀번호(필수)
      username: admin                                 # 서비스 브로커 아이디(필수)
  rabbitmq-broker:
    cc_endpoint: http://api.controller.open-paas.com  # CF 설치시 설정한 API endpoint(필수)
    uaa_client:                                       # UAA client 정보(필수)
      client_id: cf                                   # UAA client 아이디
      username: admin                                 # 사용자명
      password: admin                                 # 사용자 비밀번호
    service:                                          # 서비스 브로커 정보(필수)
      username: admin
      password: admin
      url: http://10.10.7.81:4567                     # 서비스 브로커 URL(필수)
    logging:                                          # 로깅 정보(필수)
      level: debug
      print_stack_traces: false
    rabbitmq:
      operator_set_policy:                            # rabbitmq 권한 정책 설정 (필수)
        enabled: true
        policy_name: operator_set_policy
        policy_definition: "{\"ha-mode\":\"exactly\",\"ha-params\":2,\"ha-sync-mode\":\"automatic\"}"
        policy_priority: 50
      management_domain: rabbitmq.controller.open-paas.com       # 관리 도메인
      ssl: |
        -----BEGIN CERTIFICATE-----
        MIIC+zCCAeOgAwIBAgIBAjANBgkqhkiG9w0BAQUFADAnMRUwEwYDVQQDEwxNeVRl
        c3RSb290Q0ExDjAMBgNVBAcTBTY0MTAzMB4XDTE0MDkwNDA3MjIwOFoXDTI0MDkw
        MTA3MjIwOFowKjEXMBUGA1UEAxMObWVyY3VyaW8ubG9jYWwxDzANBgNVBAoTBnNl
        cnZlcjCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANxxSzf958VIm8lp
        qQ4BHSmz1z8yU/KEKSbuEfIqpGwpVx6TZ+ZYiXa0cMV2pE7UKR4OyJiuvtvv9kzu
        6g+HTXmJo2cqVonGCAMp6d9TkCAMaMF76IrbLyGmvXQDcjOmWarvsGHW/w6gJpw9
        svDP9EXyXTBUfaJq3T8+9UQBfMsL4dHwAt79YgvSQLsYiIi2rzQixK/4PHFeHf3G
        I/UDgjG0YG9iCWp2g1Sc3Z6hYB/0pOCBxE7LCrSGS6/M/7c2569yK7NqSSNN7Lqz
        ZoQSF1NYE6KRd2MK2A0QaKrn9v8K5/Lp0fk70bvwtLxTWtp3wq3bYQg8UdqY/6R8
        UATS/aMCAwEAAaMvMC0wCQYDVR0TBAIwADALBgNVHQ8EBAMCBSAwEwYDVR0lBAww
        CgYIKwYBBQUHAwEwDQYJKoZIhvcNAQEFBQADggEBAHC89mK1HJgDqwxjsGpa3V7t
        Nuqe/XxEIUUN3Lm4gBLKq4wed4c6z4csv16f3uL9cypyHPSrQmMPV7CDgWLX4F7g
        YN9PGaVfIp/rGNsDWJEVNU2rfIEDIUfcL+o844jE8CtmzZ4bGVrCHqKW5pAraai1
        o5h3JaU4yDLo49rqPeRft2n/gj+5E3gi/1TsnuLuzB7kK1gaTTOrV3GASiGokCEN
        4v1ZjaqMSGMcwA/esaLv2N6UYJgd5lyJ7PEL4ddE8QCTo2EPhYyltLxRqOjrxa+5
        KONA94PDj14gOSSsoXkoj7gWQsuHT2RXmurYXk4/PkS+k1j0+ZCzKi/ZxF5jt50=
        -----END CERTIFICATE-----
      hosts:
        - 10.10.7.82                                # haproxy IP(필수
      administrator:                                # rabbitmq 서버 관리자 정보(필수)
        username: broker
        password: CkY26kTuAyZT8r2
resource_pools:                           # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:                       # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone)
    instance_type: m1.medium              # 인스턴스 타입: Flovers 타입(필수)
  name: services-small                    # 고유한 resource pool 이름
  env:                                    # 환경 정보(옵션)
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: openpaas_network
 # size: 5                                # resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent                # stemcell 이름(필수)
    version: 3016                                                  # stemcell 버전(필수)
</pre>


-	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

><div>$ bosh deployment {Deployment manifest 파일 PATH}<br>
$ bosh deployment openpaas-rabbitmq-openstack.yml</div>
![rabbitmq_openstack_(12)]

-	RabbitMQ 서비스팩을 배포한다.

><div>$ bosh deploy</div>
![rabbitmq_openstack_(13)]<br>
![rabbitmq_openstack_(14)]

-	배포된 RabbitMQ 서비스팩을 확인한다.

><div>$ bosh vms</div>
![rabbitmq_openstack_(15)]<br>
![rabbitmq_openstack_(16)]

#### <a name="2.4"/>2.4 RabbitMQ 서비스 브로커 등록
RabbitMQ 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 RabbitMQ 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스 브로커를 등록 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	서비스 브로커 목록을 확인한다.

><div>$ cf service-brokers</div>
![rabbitmq_openstack_(17)]

-	RabbitMQ 서비스 브로커를 등록한다.

><div>$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
-	서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
-	서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
-	서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.</div>
$ cf create-service-broker rabbitmq-service-broker admin admin  http://10.10.7.81:4567<br><br>
![rabbitmq_openstack_(18)]

-	등록된 RabbitMQ 서비스 브로커를 확인한다.

><div>$ cf service-brokers</div>
![rabbitmq_openstack_(19)]

-	접근 가능한 서비스 목록을 확인한다.

><div>$ cf service-access</div>
![rabbitmq_openstack_(20)]<br>
서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

-	특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

><div>$ cf enable-service-access p-rabbitmq<br>
$ cf service-access</div>
![rabbitmq_openstack_(21)]


## <a name="3"/>3. RabbitMQ 연동 Sample App 설명
본 Sample App은 개발형 클라우드 플랫폼에 배포되며 RabbitMQ의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
#### <a name="3.1"/>3.1 Sample App 구조
Sample App은 개방형 클라우드 플랫폼에 App으로 배포가 된다. 배포 완료 후 정상적으로 App 이 구동되면 curl 명령어로 App url 를 입력하면 RabbitMQ 환경정보(서비스 연결 정보)를 보여주는 url 페이지를 제공한다.

Sample App 구조는 다음과 같다.

| 이름 | 설명
| ---- | -----------
| lib | Sample 소스 디렉토리
| manifest.yml | 개방형 클라우드 플랫폼에 app 배포시 필요한 설정을 저장하는 파일
| Gemfile | Sample App 구동시 필요한 ruby gem 설정 파일
| config.ru | Sample App 구동 파일

-	OpenPaaS-Sample-Apps.zip 파일 압축을 풀고 Service 폴더안에 있는 RabbitMQ Sample Web App인 rabbit-labrat를 복사한다.

><div>$ ls -all</div>
![rabbitmq_openstack_(22)]

#### <a name="3.2"/>3.2 개방형 클라우드 플랫폼에서 서비스 신청
Sample App에서 RabbitMQ 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$ cf marketplace</div>
![rabbitmq_openstack_(23)]

-	Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

><div>$ cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
-	서비스명 : p-rabbitmq로 Marketplace에서 보여지는 서비스 명칭이다.
-	서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. RabbitMQ 서비스는 standard plan만 지원한다.
-	내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.</div>
$ cf create-service p-rabbitmq standard rabbitmq-service-instance<br><br>
![rabbitmq_openstack_(24)]

-	생성된 RabbitMQ 서비스 인스턴스를 확인한다.

><div>$ cf services</div>
![rabbitmq_openstack_(25)]

#### <a name="3.3"/>3.3 Sample App에 서비스 바인드 신청 및 App 확인
서비스 신청이 완료되었으면 Sample App 에서는 생성된 서비스 인스턴스를 Bind 하여 App에서 RabbitMQ 서비스를 이용한다.
*참고: 서비스 Bind 신청시 개방형 클라우드 플랫폼에서 서비스 Bind 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	Sample App 디렉토리로 이동하여 manifest 파일을 확인한다.

><div>$ cd rabbit-labrat<br>
$ vi manifest.yml</div>
<pre>---
applications:
- name: lab-rat   #배포할 App 이름
command: puma     #배포시 명령어
</pre>

-	--no-start(App 배포시 구동은 하지 않는다.) 옵션으로 App을 배포한다.

><div>$ cf push --no-start</div>
![rabbitmq_openstack_(26)]

-	배포된 Sample App을 확인하고 로그를 수행한다.

><div>$ cf apps</div>
![rabbitmq_openstack_(27)]<br><br>
$ cf logs {배포된 App명}<br>
$ cf logs lab-rat<br><br>
![rabbitmq_openstack_(28)]

-	Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

><div>$ cf bind-service lab-rat rabbitmq-service-instance</div>
![rabbitmq_openstack_(29)]

-	바인드가 적용되기 위해서 App을 재기동한다.

><div>$ cf restart lab-rat</div>
![rabbitmq_openstack_(30)]<br>
![rabbitmq_openstack_(31)]

-	App이 정상적으로 RabbitMQ 서비스를 사용하는지 확인한다. (curl로 확인)

><div>$ curl lab-rat.controller.open-paas.com</div>
![rabbitmq_openstack_(32)]<br>
![rabbitmq_openstack_(33)]<br>
![rabbitmq_openstack_(34)]

<!-- images for this document -->
[rabbitmq_openstack_(1)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(1).png
[rabbitmq_openstack_(2)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(2).png
[rabbitmq_openstack_(3)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(3).png
[rabbitmq_openstack_(4)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(4).png
[rabbitmq_openstack_(5)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(5).png
[rabbitmq_openstack_(6)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(6).png
[rabbitmq_openstack_(7)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(7).png
[rabbitmq_openstack_(8)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(8).png
[rabbitmq_openstack_(9)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(9).png
[rabbitmq_openstack_(10)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(10).png
[rabbitmq_openstack_(11)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(11).png
[rabbitmq_openstack_(12)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(12).png
[rabbitmq_openstack_(13)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(13).png
[rabbitmq_openstack_(14)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(14).png
[rabbitmq_openstack_(15)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(15).png
[rabbitmq_openstack_(16)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(16).png
[rabbitmq_openstack_(17)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(17).png
[rabbitmq_openstack_(18)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(18).png
[rabbitmq_openstack_(19)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(19).png
[rabbitmq_openstack_(20)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(20).png
[rabbitmq_openstack_(21)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(21).png
[rabbitmq_openstack_(22)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(22).png
[rabbitmq_openstack_(23)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(23).png
[rabbitmq_openstack_(24)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(24).png
[rabbitmq_openstack_(25)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(25).png
[rabbitmq_openstack_(26)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(26).png
[rabbitmq_openstack_(27)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(27).png
[rabbitmq_openstack_(28)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(28).png
[rabbitmq_openstack_(29)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(29).png
[rabbitmq_openstack_(30)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(30).png
[rabbitmq_openstack_(31)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(31).png
[rabbitmq_openstack_(32)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(32).png
[rabbitmq_openstack_(33)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(33).png
[rabbitmq_openstack_(34)]: /images/openpaas-service/rabbitmq/openstack/rabbitmq_openstack_(34).png
