## Table of Contents
1. [문서 개요](#문서-개요)
    * [1.1. 목적](#1\.1\.목적)
    * [1.2. 범위](#범위)
    * [1.3. 시스템 구성도](#시스템-구성도)
    * [1.4. 참고자료](#참고자료)
2. [RabbitMQ 서비스팩 설치] (#rabbitmq-서비스팩-설치)
     * [설치전 준비사항](#설치전-준비사항)
     * [RabbitMQ 서비스 릴리즈 업로드](#rabbitmq-서비스-릴리즈-업로드)
     * [RabbitMQ 서비스 Deployment 파일 수정 및 배포](#rabbitmq-서비스-deployment-파일-수정-및-배포)
     * [RabbitMQ 서비스 브로커 등록](#rabbitmq-서비스-브로커-등록)
3. [RabbitMQ 연동 Sample App 설명](#rabbitmq-연동-sample-app-설명)
     * [Sample App 구조](#sample-app-구조)
     * [개방형 클라우드 플랫폼에서 서비스 신청](#개방형-클라우드-플랫폼에서-서비스-신청)
     * [Sample App에 서비스 바인드 신청 및 App 확인](#sample-app에-서비스-바인드-신청-및-app-확인)



##	1. 문서 개요
####	1\.1\. 목적
본 문서(RabbitMQ 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 RabbitMQ 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 RabbitMQ 서비스를 사용하는 방법을 기술하였다.

####	1\.2 범위
설치 범위는 RabbitMQ 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다.

####	1\.3 시스템 구성도
본 문서의 설치된 시스템 구성도로써, RabbitMQ(2대), RabbitMQ 서비스 브로커, haproxy로 최소사항을 구성하였다.

![시스템 구성도][rabbitmq_bosh_lite_(1)]

####	1\.4 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)

[**http://docs.cloudfoundry.org**](http://docs.cloudfoundry.org)

##	RabbitMQ 서비스팩 설치
####	설치전 준비사항
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH-lite 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH-lite 가 설치 되어 있지 않을 경우 먼저 BOSH-lite 설치 가이드 문서를 참고 하여 BOSH-lite를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

####	RabbitMQ 서비스 릴리즈 업로드

-	OpenPaaS-Services.zip 파일 압축을 풀고 폴더안에 있는 RabbitMQ 서비스 릴리즈 openpaas-rabbitmq-release-beta-1.0.tgz 파일을 복사한다.
업로드할 openpaas-rabbitmq-release-beta-1.0.tgz 파일을 확인한다.

><div>$ ls –all</div>
![rabbitmq_bosh_lite_(2)]

-	업로드 되어 있는 릴리즈 목록을 확인한다.

><div>$ bosh releases</div>
![rabbitmq_bosh_lite_(3)]
<p>RabbitMQ 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인</p>

-	RabbitMQ 서비스 릴리즈 파일을 업로드한다.

><div>$ bosh upload release {서비스 릴리즈 파일 PATH}<br>
$ bosh upload release openpaas-rabbitmq-release-beta-1.0.tgz</div>
![rabbitmq_bosh_lite_(4)]<br>
![rabbitmq_bosh_lite_(5)]<br>
![rabbitmq_bosh_lite_(6)]<br>
![rabbitmq_bosh_lite_(7)]

-	업로드 된 RabbitMQ 릴리즈를 확인한다.

><div>$ bosh releases</div>
![rabbitmq_bosh_lite_(8)]
<p>RabbitMQ 서비스 릴리즈가 업로드 되어 있는 것을 확인</p>

####	RabbitMQ 서비스 Deployment 파일 수정 및 배포
BOSH Deployment manifest 는 components 요소 및 배포의 속성을 정의한 YAML  파일이다.
Deployment manifest 에는 software를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의가 되어 있다.

-	OpenPaaS-Deployment.zip 파일 압축을 풀고 폴더안에 있는 lite용 RabbitMQ Deployment 화일인 openpaas-rabbitmq-lite.yml 를 복사한다.
-   다운로드 받은 Deployment Yml 파일을 확인한다. (openpaas-rabbitmq-lite.yml)

><div>$ ls –all</div>
![rabbitmq_bosh_lite_(9)]

-	Director UUID를 확인한다.
BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인 할 수 있다.

><div>$ bosh status</div>
![rabbitmq_bosh_lite_(10)]

-	Deploy시 사용할 Stemcell을 확인한다. (Stemcell 2776 버전 사용)

><div>$ bosh stemcells</div>
![rabbitmq_bosh_lite_(11)]<br><br>
Stemcell 목록이 존재 하지 않을 경우 BOSH 설치 가이드 문서를 참고 하여 Stemcell 2776 버전을 업로드를 해야 한다.

-	openpaas-rabbitmq-lite.yml Deployment 파일을 서버 환경에 맞게 수정한다.

<pre>
$ vi openpaas-rabbitmq-lite.yml

openpaas-rabbitmq-lite 설정 파일 내용
---
director_uuid: xxxxx                            #bosh status 에서 확인한 Director UUID을 입력(필수)
name: openpaas-rabbitmq-service                 # 서비스 배포이름(필수)

releases:
- name: openpaas-rabbitmq                       #서비스 릴리즈 이름(필수)
  version: beta-1.0                             #서비스 릴리즈 버전(필수): latest 시 업로드된 서비스 릴리즈 최신버전

jobs:
- name: rmq                                     # job 이름 (rabbitmq 서버)
  release: openpaas-rabbitmq                    # 릴리즈 이름
  template: rabbitmq-server
  instances: 2                                  # job 인스턴스 수(필수)
  resource_pool: services-small
  persistent_disk: 2048                         # 영구적인 디스크 사이즈 2G
  networks:
  - name: services1                             # 네크워크 블록에서 설정한 이름
    static_ips:
    - 10.244.9.6
    - 10.244.9.18

- name: rmq-broker                              # 서비스 브로커 이름
  release: openpaas-rabbitmq
  template: rabbitmq-broker
  instances: 1
  resource_pool: services-small
  persistent_disk: 512
  networks:
  - name: services1
    static_ips:
    - 10.244.9.14

- name: haproxy                                 # job 이름 : haproxy
  release: openpaas-rabbitmq
  template: rabbitmq-haproxy
  instances: 1
  resource_pool: services-small
  persistent_disk: 256
  networks:
  - name: services1
    static_ips:
    - 10.244.9.50

- name: broker-registrar
  release: openpaas-rabbitmq
  template: broker-registrar
  instances: 1
  lifecycle: errand
  networks:
  - name: services1
  properties:
    broker:
      protocol: http
      host: 10.244.9.14
      port: 4567
      name: p-rabbitmq
      username: "admin"
      password: "admin"
  resource_pool: services-small

- name: broker-deregistrar
  release: openpaas-rabbitmq
  template: broker-deregistrar
  instances: 1
  lifecycle: errand
  networks:
  - name: services1
  properties:
    broker:
      protocol: http
      host: 10.244.9.14
      port: 4567
      name: p-rabbitmq
      username: "admin"
      password: "admin"
  resource_pool: services-small

properties:
  # for broker and route registrars
  cf:
    admin_password: "admin"                     # CF 어드민 아이디 비밀번호
    admin_username: "admin"                     # CF 어드민 아이디
    api_url: "http://api.10.244.0.34.xip.io"    # CF API url
    domain: "10.244.0.34.xip.io"                # CF 도메인
    nats:                                       # CF 설치시 설치한 nats 정보
      host: "10.244.0.6"
      port: "4222"
      username: "nats"
      password: "nats"
  route-registrar:
    target_ip: "10.244.9.50"                    # 라우터 타켓 IP (haproxy IP)
  rabbitmq-server:
    plugins:                                    # rabbitmq 플러그인 정보
    - rabbitmq_management
    - rabbitmq_mqtt
    - rabbitmq_stomp
    administrators:
      broker:
        username: broker                        #브로커에서 rabbitmq 서버에 접근하는 유저 아이디
        password: CkY26kTuAyZT8r2
    static_ips:                                 # rabbitmq 서버 IP 목록
    - 10.244.9.6
    - 10.244.9.18
    ssl:                                        # SSL 정보
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
  rabbitmq-haproxy:                             # rabbitmq haproxy 에서 허용하는 포트 목록
    ports:
    - 5672
    - 5671
    - 1883
    - 8883
    - 61613
    - 61614
    - 15672
    - 15674
    server_ips:                                # rabbitmq 서버 IP 목록
    - 10.244.9.6
    - 10.244.9.10
    stats:
      password: admin                           # 서비스 브로커 비밀번호
      username: admin                           # 서비스 브로커 아이디
  rabbitmq-broker:
    cc_endpoint: http://api.10.244.0.34.xip.io  # CF 설치시 설정한 API endpoint
    uaa_client:                                 # UAA client 정보
      client_id: cf                             # UAA client 아이디
      username: "admin"                         # 사용자명
      password: "admin"                         # 사용자 비밀번호
    service:                                    # 서비스 브로커 정보
      username: "admin"
      password: "admin"
      url: http://10.244.9.14:4567              # 서비스 브로커 URL
    logging:                                    # 로깅 정보
      level: debug
      print_stack_traces: false
    rabbitmq:
      operator_set_policy:                      # rabbitmq 권한 정책 설정
        enabled: true
        policy_name: "operator_set_policy"
        policy_definition: "{\"ha-mode\":\"exactly\",\"ha-params\":2,\"ha-sync-mode\":\"automatic\"}"
        policy_priority: 50
      management_domain: pivotal-rabbitmq.10.244.0.34.xip.io           # 관리 도메인
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
        - 10.244.9.50                          # haproxy IP
      administrator:                           # rabbitmq 서버 관리자 정보
        username: broker
        password: CkY26kTuAyZT8r2


networks:    # 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정, bosh lite 에서는 제공하는 네트워크를 수정 없이 사용
- name: services1
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
    static:
    - 10.244.9.130
  - cloud_properties:
      name: random
    range: 10.244.9.132/30
    reserved:
    - 10.244.9.133
    static:
    - 10.244.9.134
  - cloud_properties:
      name: random
    range: 10.244.9.136/30
    reserved:
    - 10.244.9.137
    static:
    - 10.244.9.138
  - cloud_properties:
      name: random
    range: 10.244.9.140/30
    reserved:
    - 10.244.9.141
    static:
    - 10.244.9.142
  - cloud_properties:
      name: random
    range: 10.244.9.144/30
    reserved:
    - 10.244.9.145
    static:
    - 10.244.9.146
  - cloud_properties:
      name: random
    range: 10.244.9.148/30
    reserved:
    - 10.244.9.149
    static:
    - 10.244.9.150
  - cloud_properties:
      name: random
    range: 10.244.9.152/30
    reserved:
    - 10.244.9.153
    static:
    - 10.244.9.154
  - cloud_properties:
      name: random
    range: 10.244.9.156/30
    reserved:
    - 10.244.9.157
    static:
    - 10.244.9.158
  - cloud_properties:
      name: random
    range: 10.244.9.160/30
    reserved:
    - 10.244.9.161
    static:
    - 10.244.9.162
  - cloud_properties:
      name: random
    range: 10.244.9.164/30
    reserved:
    - 10.244.9.165
    static:
    - 10.244.9.166
  - cloud_properties:
      name: random
    range: 10.244.9.168/30
    reserved:
    - 10.244.9.169
    static:
    - 10.244.9.170
  - cloud_properties:
      name: random
    range: 10.244.9.172/30
    reserved:
    - 10.244.9.173
    static:
    - 10.244.9.174
  - cloud_properties:
      name: random
    range: 10.244.9.176/30
    reserved:
    - 10.244.9.177
    static:
    - 10.244.9.178
  - cloud_properties:
      name: random
    range: 10.244.9.180/30
    reserved:
    - 10.244.9.181
    static:
    - 10.244.9.182
  - cloud_properties:
      name: random
    range: 10.244.9.184/30
    reserved:
    - 10.244.9.185
    static:
    - 10.244.9.186
  - cloud_properties:
      name: random
    range: 10.244.9.188/30
    reserved:
    - 10.244.9.189
    static:
    - 10.244.9.190
  - cloud_properties:
      name: random
    range: 10.244.9.192/30
    reserved:
    - 10.244.9.193
    static:
    - 10.244.9.194
  - cloud_properties:
      name: random
    range: 10.244.9.196/30
    reserved:
    - 10.244.9.197
    static:
    - 10.244.9.198
  - cloud_properties:
      name: random
    range: 10.244.9.200/30
    reserved:
    - 10.244.9.201
    static:
    - 10.244.9.202
  - cloud_properties:
      name: random
    range: 10.244.9.204/30
    reserved:
    - 10.244.9.205
    static:
    - 10.244.9.206
  - cloud_properties:
      name: random
    range: 10.244.9.208/30
    reserved:
    - 10.244.9.209
    static:
    - 10.244.9.210
  - cloud_properties:
      name: random
    range: 10.244.9.212/30
    reserved:
    - 10.244.9.213
    static:
    - 10.244.9.214
  - cloud_properties:
      name: random
    range: 10.244.9.216/30
    reserved:
    - 10.244.9.217
    static:
    - 10.244.9.218
  - cloud_properties:
      name: random
    range: 10.244.9.220/30
    reserved:
    - 10.244.9.221
    static:
    - 10.244.9.222
  - cloud_properties:
      name: random
    range: 10.244.9.224/30
    reserved:
    - 10.244.9.225
    static:
    - 10.244.9.226
  - cloud_properties:
      name: random
    range: 10.244.9.228/30
    reserved:
    - 10.244.9.229
    static:
    - 10.244.9.230
  - cloud_properties:
      name: random
    range: 10.244.9.232/30
    reserved:
    - 10.244.9.233
    static:
    - 10.244.9.234
  - cloud_properties:
      name: random
    range: 10.244.9.236/30
    reserved:
    - 10.244.9.237
    static:
    - 10.244.9.238
  - cloud_properties:
      name: random
    range: 10.244.9.240/30
    reserved:
    - 10.244.9.241
    static:
    - 10.244.9.242
  - cloud_properties:
      name: random
    range: 10.244.9.244/30
    reserved:
    - 10.244.9.245
    static:
    - 10.244.9.246
  - cloud_properties:
      name: random
    range: 10.244.9.248/30
    reserved:
    - 10.244.9.249
    static:
    - 10.244.9.250
  - cloud_properties:
      name: random
    range: 10.244.9.252/30
    reserved:
    - 10.244.9.253
    static:
    - 10.244.9.254
  - cloud_properties:
      name: random
    range: 10.244.10.0/30
    reserved:
    - 10.244.10.1
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.4/30
    reserved:
    - 10.244.10.5
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.8/30
    reserved:
    - 10.244.10.9
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.12/30
    reserved:
    - 10.244.10.13
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.16/30
    reserved:
    - 10.244.10.17
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.20/30
    reserved:
    - 10.244.10.21
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.24/30
    reserved:
    - 10.244.10.25
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.28/30
    reserved:
    - 10.244.10.29
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.32/30
    reserved:
    - 10.244.10.33
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.36/30
    reserved:
    - 10.244.10.37
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.40/30
    reserved:
    - 10.244.10.41
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.44/30
    reserved:
    - 10.244.10.45
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.48/30
    reserved:
    - 10.244.10.49
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.52/30
    reserved:
    - 10.244.10.53
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.56/30
    reserved:
    - 10.244.10.57
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.60/30
    reserved:
    - 10.244.10.61
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.64/30
    reserved:
    - 10.244.10.65
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.68/30
    reserved:
    - 10.244.10.69
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.72/30
    reserved:
    - 10.244.10.73
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.76/30
    reserved:
    - 10.244.10.77
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.80/30
    reserved:
    - 10.244.10.81
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.84/30
    reserved:
    - 10.244.10.85
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.88/30
    reserved:
    - 10.244.10.89
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.92/30
    reserved:
    - 10.244.10.93
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.96/30
    reserved:
    - 10.244.10.97
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.100/30
    reserved:
    - 10.244.10.101
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.104/30
    reserved:
    - 10.244.10.105
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.108/30
    reserved:
    - 10.244.10.109
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.112/30
    reserved:
    - 10.244.10.113
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.116/30
    reserved:
    - 10.244.10.117
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.120/30
    reserved:
    - 10.244.10.121
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.124/30
    reserved:
    - 10.244.10.125
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.128/30
    reserved:
    - 10.244.10.129
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.132/30
    reserved:
    - 10.244.10.133
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.136/30
    reserved:
    - 10.244.10.137
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.140/30
    reserved:
    - 10.244.10.141
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.144/30
    reserved:
    - 10.244.10.145
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.148/30
    reserved:
    - 10.244.10.149
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.152/30
    reserved:
    - 10.244.10.153
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.156/30
    reserved:
    - 10.244.10.157
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.160/30
    reserved:
    - 10.244.10.161
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.164/30
    reserved:
    - 10.244.10.165
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.168/30
    reserved:
    - 10.244.10.169
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.172/30
    reserved:
    - 10.244.10.173
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.176/30
    reserved:
    - 10.244.10.177
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.180/30
    reserved:
    - 10.244.10.181
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.184/30
    reserved:
    - 10.244.10.185
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.188/30
    reserved:
    - 10.244.10.189
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.192/30
    reserved:
    - 10.244.10.193
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.196/30
    reserved:
    - 10.244.10.197
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.200/30
    reserved:
    - 10.244.10.201
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.204/30
    reserved:
    - 10.244.10.205
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.208/30
    reserved:
    - 10.244.10.209
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.212/30
    reserved:
    - 10.244.10.213
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.216/30
    reserved:
    - 10.244.10.217
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.220/30
    reserved:
    - 10.244.10.221
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.224/30
    reserved:
    - 10.244.10.225
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.228/30
    reserved:
    - 10.244.10.229
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.232/30
    reserved:
    - 10.244.10.233
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.236/30
    reserved:
    - 10.244.10.237
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.240/30
    reserved:
    - 10.244.10.241
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.244/30
    reserved:
    - 10.244.10.245
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.248/30
    reserved:
    - 10.244.10.249
    static: []
  - cloud_properties:
      name: random
    range: 10.244.10.252/30
    reserved:
    - 10.244.10.253
    static: []


resource_pools:             # 배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함(필수)
- cloud_properties:         # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone), 직접 cpu, disk, 메모리 설정가능
    name: random
  name: services-small      # 고유한 resource pool 이름
  network: services1
  #size: 4                  # resource pool 안의 가상머신 개수, 주의) jobs 인스턴스 보다 작으면 에러가 남, size 정의하지 않으면 자동으로 가상머신 크기 설정
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent                # stemcell 이름(필수)
    version: latest                                                  # stemcell 버전(필수)

meta:
  stemcell:
    name: bosh-warden-boshlite-ubuntu-trusty-go_agent
    version: latest

update:
  canaries: 1                         # canary 인스턴스 수(필수)
  canary_watch_time: 30000-180000     # canary 인스턴스가 수행하기 위한 대기 시간(필수)
  update_watch_time: 30000-180000     # non-canary 인스턴스가 수행하기 위한 대기 시간(필수)
  max_in_flight: 4                    # non-canary 인스턴스가 병렬로 update 하는 최대 개수(필수)

compilation:                          # 컴파일시 필요한 가상머신의 속성(필수)
  cloud_properties:                   # 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone), 직접 cpu,disk,ram 사이즈를 넣어도 됨
    name: random
  network: services1                  # Networks block에서 선언한 network 이름(필수)
  reuse_compilation_vms: true         # 컴파일지 VM 재사용 여부(옵션)
  workers: 3                          # 컴파일 하는 가상머신의 최대수(필수)
</pre>


-	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.

><div>$ bosh deployment {Deployment manifest 파일 PATH}<br>
$ bosh deployment openpaas-rabbitmq-lite.yml</div>
![rabbitmq_bosh_lite_(12)]

-	RabbitMQ 서비스팩을 배포한다.

><div>$ bosh deploy</div>
![rabbitmq_bosh_lite_(13)]<br>
![rabbitmq_bosh_lite_(14)]

-	배포된 RabbitMQ 서비스팩을 확인한다.

><div>$ bosh vms</div>
![rabbitmq_bosh_lite_(15)]<br>
![rabbitmq_bosh_lite_(16)]

####	RabbitMQ 서비스 브로커 등록
RabbitMQ 서비스팩 배포가 완료 되었으면 Application에서 서비스 팩을 사용하기 위해서 먼저 RabbitMQ 서비스 브로커를 등록해 주어야 한다.
서비스 브로커 등록시 개방형 클라우드 플랫폼에서 서비스 브로커를 등록 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	서비스 브로커 목록을 확인한다.

><div>$ cf service-brokers</div>
![rabbitmq_bosh_lite_(17)]

-	RabbitMQ 서비스 브로커를 등록한다.

><div>$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}<br>
-	서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭이다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭이다.
-	서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID이다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력한다.
-	서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력한다.</div>
$ cf create-service-broker rabbitmq-service-broker admin admin http://10.244.9.14:4567<br><br>
![rabbitmq_bosh_lite_(18)]

-	등록된 RabbitMQ 서비스 브로커를 확인한다.

><div>$ cf service-brokers</div>
![rabbitmq_bosh_lite_(19)]

-	접근 가능한 서비스 목록을 확인한다.

><div>$ cf service-access</div>
![rabbitmq_bosh_lite_(20)]<br>
서비스 브로커 생성시 디폴트로 접근을 허용하지 않는다.

-	특정 조직에 해당 서비스 접근 허용을 할당하고 접근 서비스 목록을 다시 확인한다. (전체 조직)

><div>$ cf enable-service-access p-rabbitmq<br>
$ cf service-access</div>
![rabbitmq_bosh_lite_(21)]


##	RabbitMQ 연동 Sample App 설명
본 Sample App은 개발형 클라우드 플랫폼에 배포되며 RabbitMQ의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능하다.
####	Sample App 구조
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
![rabbitmq_bosh_lite_(22)]

####	개방형 클라우드 플랫폼에서 서비스 신청
Sample App에서 RabbitMQ 서비스를 사용하기 위해서는 서비스 신청(Provision)을 해야 한다.
*참고: 서비스 신청시 개방형 클라우드 플랫폼에서 서비스를 신청 할 수 있는 사용자로 로그인이 되어 있어야 한다.

-	먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 한다.

><div>$ cf marketplace</div>
![rabbitmq_bosh_lite_(23)]

-	Marketplace에서 원하는 서비스가 있으면 서비스 신청(Provision)을 한다.

><div>$ cf create-service {서비스명} {서비스플랜} {내서비스명}<br>
-	서비스명 : p-rabbitmq로 Marketplace에서 보여지는 서비스 명칭이다.
-	서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택한다. RabbitMQ 서비스는 standard plan만 지원한다.
-	내서비스명 : 내 서비스에서 보여지는 명칭이다. 이 명칭을 기준으로 환경설정정보를 가져온다.</div>
$ cf create-service p-rabbitmq standard rabbitmq-service-instance<br><br>
![rabbitmq_bosh_lite_(24)]

-	생성된 RabbitMQ 서비스 인스턴스를 확인한다.

><div>$ cf services</div>
![rabbitmq_bosh_lite_(25)]

####	Sample App에 서비스 바인드 신청 및 App 확인
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
![rabbitmq_bosh_lite_(26)]

-	배포된 Sample App을 확인하고 로그를 수행한다.

><div>$ cf apps</div>
![rabbitmq_bosh_lite_(27)]<br><br>
$ cf logs {배포된 App명}<br>
$ cf logs lab-rat<br><br>
![rabbitmq_bosh_lite_(28)]

-	Sample App에서 생성한 서비스 인스턴스 바인드 신청을 한다.

><div>$ cf bind-service lab-rat rabbitmq-service-instance</div>
![rabbitmq_bosh_lite_(29)]

-	바인드가 적용되기 위해서 App을 재기동한다.

><div>$ cf restart lab-rat</div>
![rabbitmq_bosh_lite_(30)]<br>
![rabbitmq_bosh_lite_(31)]

-	App이 정상적으로 RabbitMQ 서비스를 사용하는지 확인한다. (curl로 확인)

><div>$ curl lab-rat.10.244.0.34.xip.io</div>
![rabbitmq_bosh_lite_(32)]<br>
![rabbitmq_bosh_lite_(33)]<br>
![rabbitmq_bosh_lite_(34)]

<!-- images for this document -->
[rabbitmq_bosh_lite_(1)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(1).png
[rabbitmq_bosh_lite_(2)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(2).png
[rabbitmq_bosh_lite_(3)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(3).png
[rabbitmq_bosh_lite_(4)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(4).png
[rabbitmq_bosh_lite_(5)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(5).png
[rabbitmq_bosh_lite_(6)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(6).png
[rabbitmq_bosh_lite_(7)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(7).png
[rabbitmq_bosh_lite_(8)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(8).png
[rabbitmq_bosh_lite_(9)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(9).png
[rabbitmq_bosh_lite_(10)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(10).png
[rabbitmq_bosh_lite_(11)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(11).png
[rabbitmq_bosh_lite_(12)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(12).png
[rabbitmq_bosh_lite_(13)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(13).png
[rabbitmq_bosh_lite_(14)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(14).png
[rabbitmq_bosh_lite_(15)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(15).png
[rabbitmq_bosh_lite_(16)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(16).png
[rabbitmq_bosh_lite_(17)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(17).png
[rabbitmq_bosh_lite_(18)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(18).png
[rabbitmq_bosh_lite_(19)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(19).png
[rabbitmq_bosh_lite_(20)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(20).png
[rabbitmq_bosh_lite_(21)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(21).png
[rabbitmq_bosh_lite_(22)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(22).png
[rabbitmq_bosh_lite_(23)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(23).png
[rabbitmq_bosh_lite_(24)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(24).png
[rabbitmq_bosh_lite_(25)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(25).png
[rabbitmq_bosh_lite_(26)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(26).png
[rabbitmq_bosh_lite_(27)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(27).png
[rabbitmq_bosh_lite_(28)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(28).png
[rabbitmq_bosh_lite_(29)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(29).png
[rabbitmq_bosh_lite_(30)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(30).png
[rabbitmq_bosh_lite_(31)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(31).png
[rabbitmq_bosh_lite_(32)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(32).png
[rabbitmq_bosh_lite_(33)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(33).png
[rabbitmq_bosh_lite_(34)]: /images/openpaas-service/rabbitmq/bosh_lite/rabbitmq_bosh_lite_(34).png