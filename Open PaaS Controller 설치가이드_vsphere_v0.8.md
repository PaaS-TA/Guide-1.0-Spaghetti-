## Table of Contents
1. [개요](#1)
     * [1.1. 문서 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 참고 자료](#4)
2. [Prerequisites](#5)
     * [2.1. 개요](#6)
     * [2.2. vSphere](#7)
     * [2.3. Bosh Server 및 Bosh CLI](#8)
     * [2.4. DNS Server](#9)
     * [2.5. OP CLI](#10)
3. [Open PaaS Controller 설치](#11)
     * [3.1. Release Upload](#12])
     * [3.2. Stemcell Upload](#13)
     * [3.3. Deployment Manifest](#14)
     * [3.4. Bosh Deploy](#15)
     * [3.5. 설치형상 확인](#16)
4. [설치 검증](#17)
     * [4.1. CF Login](#18)
     * [4.2. Application Deploy](#19)
     * [4.3. Application Access](#20)

<div id='1'/>
#문서 개요
<div id='2'/>
###문서 목적
본 문서(설치가이드)는, 현 시점에서 지원되는 IaaS(Infrastructure as a Service) 중 하나인 vSphere 환경에서 개방형클라우드플랫폼을 설치하기 위한 가이드를 제공하는데 그 목적이 있다.

<div id='3'/>
###범위
본 문서의 범위는 개방형클라우드플랫폼을 vSphere에 설치하기 데 대한 내용으로 한정되어 있다. OpenStack/AWS와 같은 다른 IaaS 환경에서의 설치는 그에 맞는 가이드 문서를 참고해야 하며, Inception/Bosh 설치 또한 해당 가이드 문서를 별도로 참조해야 한다.

<div id='4'/>
###참고 자료
[**http://docs.cloudfoundry.org/deploying/vsphere/**](http://docs.cloudfoundry.org/deploying/vsphere/)<br>
[**https://github.com/cloudfoundry/cf-release**](https://github.com/cloudfoundry/cf-release)

<div id='5'/>
#Prerequisites
<div id='6'/>
###개요
개방형클라우드플랫폼을 설치하기 전에 IaaS(vSphere) 환경이 정상적으로 구성되어 있고, Bosh Server와 Bosh/CF CLI가 설치되어 있는지를 확인해야 한다.

<div id='7'/>
###vSphere
####데이터센터/클러스터/리소스풀
![controller_vsphere_image002]<br>
**[그림출처]: Open PaaS 사업단 개발환경**

데이터센터 내에 vSphere Server들로 구성된 클러스터가 있어야 하며, 개방형클라우드플랫폼이 설치될 리소스풀이 만들어져 있어야 한다. 리소스풀의 경우는 필수적인 구성은 아니나, 관리의 용이성을 위해서 사용하는 것을 권장한다.

<div id='8'/>
###Bosh Server 및 Bosh CLI
![controller_vsphere_image003]<br>
**[그림출처]: Open PaaS 사업단 개발환경**

“bosh status” 명령을 실행하여 위와 같이 정상적으로 출력되는 지를 확인한다. 만약 문제 발생 시에는 Bosh 설치가이드를 참조하여 정상적으로 Bosh 환경을 구성한 후 이후 작업을 진행한다.

<div id='9'/>
###DNS Server
개방형클라우드플랫폼은 독자적인 Zone을 DNS에 등록해야 한다. 사용 가능한 DNS Server가 존재하지 않는다면, VM 등에 별도로 구축하여야 한다. 예를 들어 Linux의 경우에는 bind9 Package를 설치하고 아래와 같이 Platform Zone을 등록한다.

>/etc/bind/named.conf.local\
><pre>
zone "controller.open-paas.com" {
        type master;
        file "/etc/bind/db.controller.open-paas.com";
};
</pre>

>/etc/bind/db.cf-dev.open-paas.com
><pre>
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
*       IN      A       10.30.40.113                  <font color="blue"># HA Proxy VM IP 주소</font>
@       IN      AAAA    ::1
</pre>

>NSLOOKUP 등으로 DNS Server에 Platform Domain이 정상 등록 되었는지 확인한다.
>![controller_vsphere_image004]<br>

<div id='10'/>
###OP CLI
Open PaaS 설치 패키지 내에 포함되어 있는 OP CLI 압축 파일을 풀고 명령어 Path Folder에 실행 파일을 복사한다.

>sudo tar -xvzf $INSTALL_PACKAGE/OpenPaaS-Dev-Tools/op-CLI/cf-linux-amd64.tgz<br>
>sudo cp cf /usr/bin

>“cf” 명령어를 입력하면 아래와 같은 Help 화면이 출력됨을 확인한다.
>![controller_vsphere_image005]

<div id='11'/>
#Open PaaS Controller 설치
<div id='12'/>
###Release Upload
배포된 설치 패키지의 OpenPaaS-Controller 폴더에 있는 Open PaaS Controller Bosh Release를 Bosh Server로 아래와 같은 명령으로 212 버전을 Upload 한다.

>bosh upload release $INSTALL_PACKAGE/OpenPaaS-Controller/openpaas-beta-1.0.tgz


>Release Upload는 상황에 따라 다소 차이는 있으나 보통 20-30분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.<br>
>![controller_vsphere_image006]

[주의] Release Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

Bosh Sever에 Release가 정상적으로 Upload 되었는지는 “bosh releases” 명령으로 확인한다.<br>
>bosh releases
>
>![controller_vsphere_image007] 

<div id='13'/>
###Stemcell Upload
배포된 설치 패키지의 OpenPaaS-Stemcells 폴더에 있는 Open PaaS vSphere용 Stemcell 을 Bosh Server로 아래와 같은 명령으로 3016 Version을 Upload 한다.
>bosh upload stemcell $INSALL_PACKAGE/OpenPaaS-Stemcells/bosh-stemcell-3016-vsphere-esxi-ubuntu-trusty-go_agent.tgz

>Stemcell Upload는 상황에 따라 다소 차이는 있으나 보통 5-10분 정도 소요가 되며, 정상 Upload가 되면 아래의 그림과 같은 메시지가 출력된다.
>
>![controller_vsphere_image008]

[주의] Stemcell Upload 과정에서 작업장비의 “/tmp” 폴더의 사이즈가 작을 경우 압축파일을 풀거나 묶을 때 에러가 발생할 수 있으므로, 10GB 이상 Free Size가 있는지를 확인해야 한다.

Bosh Sever에 Stemcell이 정상적으로 Upload 되었는지는 “bosh stemcells” 명령으로 확인한다.
>bosh stemcells

>![controller_vsphere_image009]    

<div id='14'/>
###	Deployment Manifest
배포된 설치 패키지에 포함된 Sample Deployment Manifest File($INSTALL_PACKAGE/OpenPaaS-Deployment/openpaas-vsphere-beta-1.0.yml)을 아래의 순서대로 설치환경에 적합하게 수정한다.

#### Name & Release
<pre>
name: openpaas-vsphere-beta-1.0   <font color="blue"># Deployment Name</font>
director_uuid: 6e0f7c41-2415-4319-98aa-38109597aff4  <font color="blue"># Bosh Director UUID</font>
releases:
- name: openpaas       <font color="blue"> # Bosh Release Name</font>
  version: beta-1.0            <font color="blue"># Bosh Release Version</font>
</pre>
Deployment Name은 설치자가 임의로 부여하는데, IaaS와 Version을 표시할 것을 권장한다. Bosh Director UUID는 “bosh status” 명령을 실행하면 출력되는 UUID 값을 넣고, Release Name과 Version은 “bosh releases” 명령의 결과로 나오는 값들을 입력하도록 한다.

<div id='networks'/>
#### Networks	
<pre>	
networks:
- name: op_network           <font color="blue"># Open PaaS Controller가 설치될 Network Name</font>
  subnets:
  - cloud_properties:
      name: Internal          <font color="blue"># Open PaaS Controller가 설치될 Virtual Switch Name</font>
    dns:
    - 10.30.20.24             <font color="blue"># DNS Server</font>
    - 8.8.8.8
    gateway: 10.30.20.23      <font color="blue"># Gateway IP Address</font>
    name: default_unused
    range: 10.30.0.0/16           <font color="blue"># Network CIDR</font>
    #reserved:
static:
    - 10.30.40.10 - 10.30.40.100   <font color="blue"># VM에 할당될 Static IP 주소 대역</font>
  type: manual
</pre>
Network Name은 설치자가 임의로 부여 가능하다. Virtual Switch, Gateway, DNS Server, Network CIDR은 vSphere 구성을 직접 확인하거나 인프라 담당자에게 문의하여 정보를 얻도록 한다. Static IP 주소는 Open PaaS Controller를 설치할 때 개별 VM에 할당될 IP의 주소 대역으로 마찬가지로 인프라 담당자에게 할당을 받아야 한다.

#### Compilation	
<pre>
compilation:
  cloud_properties:               <font color="blue"># Compile용 VM의 사양</font>
    cpu: 2
    disk: 8192
    ram: 1024
  network: op_network            <font color="blue"># Network Name</font>
  reuse_compilation_vms: true
  workers: 6                     <font color="blue"># 동시 동작하는 VM 수</font>
</pre>
Network Name은 [**Newworks**](#networks)에서 정의한 것과 동일한 이름을 줘야 한다. Workers는 동시에 Compile을 수행하는 VM의 개수로 별다른 환경적 특성이 없다면 Default 값을 사용토록 한다.

#### Resource Pools
<pre>
resource_pools:
- name: small                   <font color="blue"># Resource Name</font>
  cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network           <font color="blue"># Network Name</font>
  size: 3                        <font color="blue"># Small을 사용하는 VM 개수</font>
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent    <font color="blue"># Stemcell Name</font>
    version: 3016                                     <font color="blue"># Stemcell Version</font>
- name: medium
  cloud_properties:
    cpu: 1
    disk: 4096
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  size: 10
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3016

- name: large
  cloud_properties:
    cpu: 1
    disk: 10240
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  size: 1
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3016

- name: runner
  cloud_properties:
    cpu: 2
    disk: 32768
    ram: 16384
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  size: 1
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3016

- name: router
  cloud_properties:
    cpu: 1
    disk: 2048
    ram: 1024
  env:
    bosh:
      password: $6$4gDD3aV0rdqlrKC$2axHCxGKIObs6tAmMTqYCspcdvQXh3JJcvWOY2WGb4SrdXtnCyNaWlrf3WEqvYR2MYizEGp3kMmbpwBC6jsHt0
  network: op_network
  size: 2
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: 3016
</pre>
각 Resource의 Size는 Jobs에서 해당 Resource를 사용하는 VM 개수와 정확하게 일치해야 한다. Stemcell Name과 Version은 “bosh stemcells” 명령어 결과로 출력되는 값들을 입력하도록 한다.

<pre>
update:
  canaries: 1
  canary_watch_time: 30000-600000
  max_in_flight: 1
  serial: true	<font color="blue"># VM의 순차적 Update</font>
  update_watch_time: 5000-600000
</pre>
Default 값들을 수정 없이 사용한다.

####Jobs
아래 Sample Jobs를 참고하여 설치 환경에 맞게 수정한다.
<pre>
jobs:
- name: consul
  instances: 1                  <font color="blue"># VM Instance 개수</font>
  networks:
  - name: op_network       <font color="blue"># VM이 설치될 Network</font>
    static_ips: 10.30.40.150      <font color="blue"># Consul에 할당된 IP 주소</font>
  persistent_disk: 1024
  properties:
    consul:
      agent:
        mode: server
    metron_agent:
      zone: z1
      deployment: openpaas-beta-1.0
  resource_pool: medium
  templates:
  - name: consul_agent
    release: openpaas
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1
    serial: true

- name: ha_proxy
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.113        <font color="blue"># HAProxy IP 주소</font>
  properties:
    ha_proxy:
      disable_http: false
ssl_pem: |                  <font color="blue"># SSL Key</font>
        -----BEGIN CERTIFICATE-----
        MIICyTCCAjICCQD6oXQcZiA41jANBgkqhkiG9w0BAQsFADCBqDELMAkGA1UEBhMC
        S1IxDjAMBgNVBAgMBVNlb3VsMRUwEwYDVQQHDAxZZW9uZ2RldW5ncG8xEjAQBgNV
        BAoMCW9wZW4tcGFhczESMBAGA1UECwwJb3Blbi1wYWFzMR8wHQYDVQQDDBYqLmNm
        LWRldi5vcGVuLXBhYXMuY29tMSkwJwYJKoZIhvcNAQkBFhphZG1pbkBjZi1kZXYu
        b3Blbi1wYWFzLmNvbTAeFw0xNTA4MjYwNjEwNThaFw0xNTA5MjUwNjEwNThaMIGo
        MQswCQYDVQQGEwJLUjEOMAwGA1UECAwFU2VvdWwxFTATBgNVBAcMDFllb25nZGV1
        bmdwbzESMBAGA1UECgwJb3Blbi1wYWFzMRIwEAYDVQQLDAlvcGVuLXBhYXMxHzAd
        BgNVBAMMFiouY2YtZGV2Lm9wZW4tcGFhcy5jb20xKTAnBgkqhkiG9w0BCQEWGmFk
        bWluQGNmLWRldi5vcGVuLXBhYXMuY29tMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCB
        iQKBgQDwguc5oQBT4U2evWMYVh5seX4rRWG696s6XqzQ6fTGiTEQa2kQaSvKCqOb
        h4eptlOTVjqdFUfRB5kX4HPnMqTDXUPy7JLm4ymtLBqEhSAQvtutHSfsy0b5eF54
        nSXryOcfAYyeULLxOlN2xZ5Yvy5x0LZ2fqUm58UuzUFlpCT8jwIDAQABMA0GCSqG
        SIb3DQEBCwUAA4GBADcvW+KAjWkX1GZ22ZCaBkJbID6eH5GdNcVKUlI7Wuy4A/fs
        X/Zd9S0K4GExXBgNKsI8AtpcJb1LrSVL+0qcxhMCoPSVYZeSe/vTgjWHXrmVB5x6
        a/qUhfcQugC0bW2zJL/M+NeC1XNtQyNQVmPaBGzMl9WCUhfqKFsVaKu6JMXJ
        -----END CERTIFICATE-----
        -----BEGIN RSA PRIVATE KEY-----
        MIICXQIBAAKBgQDwguc5oQBT4U2evWMYVh5seX4rRWG696s6XqzQ6fTGiTEQa2kQ
        aSvKCqObh4eptlOTVjqdFUfRB5kX4HPnMqTDXUPy7JLm4ymtLBqEhSAQvtutHSfs
        y0b5eF54nSXryOcfAYyeULLxOlN2xZ5Yvy5x0LZ2fqUm58UuzUFlpCT8jwIDAQAB
        AoGBAMpCq8WRGj9wN+CJEG8E7Wz39xzYqkmbwsnBmL/QIGNeOH7AdYixiXvN2q0P
        Gw2kIUDEwWIc5Vpqc1rbDV2e/2MTClXRHu3Mkwu+Xao5a4yfA/PUk4jY2ZT6CDcm
        hqvDBwgxYWSf6goQudkz8hj7S5nnUBUBy4WZoK11KH8goxuRAkEA+tFZSdEsH6dc
        eYzVO2PVFsZXOuaCBYbiH3AIdclS1e3MsrGDGrcKZFnTLlNPCY99uqs+XflzWxnu
        hubf+00fNQJBAPV7CmX4SzRSJVz1rCI8Z5qqiR1jZuf2e+cBLfNxD8bcsaEN5NAU
        qMHyWMpJzE50X2EklpsW9vEgwM9Ll45IUTMCQQDR6OMRJD4JP8fk8jiCu8pahSP7
        525psbkNFnX+Hb91ys7Fvko60XWhBov2+UHsfmp7D1Xi0CH63HkD7bUGXEUBAkBD
        WtVJkU0hOADkdswaQLS0rWId3C5aKzln8tzm1PjdOaTLagKETYbYYGSYbJNQW4Ho
        J5/jQfPTyYbd2gHlmlYdAkAfKutj2aTiuv9fIDekrU350pFeNOGR00yt7S6bt0cx
        mAIkLL4WmtZiOxysK7xXWOk+aLdgMRCj3kUQynssYLW2
        -----END RSA PRIVATE KEY-----
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
    router:
      servers:
        z1:
        - 10.30.40.115               <font color="blue"># Router IP 주소</font>
  resource_pool: router
  templates:
  - name: haproxy
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: consul_agent
    release: openpaas
  update: {}

- name: nats
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.111             <font color="blue"># NATS IP 주소</font>
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: nats
    release: openpaas
  - name: nats_stream_forwarder
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: etcd
  instances: 1
  networks:
  - name: op_network
    static_ips:
    - 10.30.40.124                  <font color="blue"># ETCD IP 주소</font>
  persistent_disk: 10024
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: etcd
    release: openpaas
  - name: etcd_metrics_server
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: stats
  instances: 1
  networks:
  - name: op_network
    static_ips:
    - 10.30.40.141                <font color="blue"># Stats(Collector) IP 주소</font>
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: small
  templates:
  - name: collector
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: nfs
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.112              <font color="blue"># NFS Server IP 주소</font>
  persistent_disk: 102400
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: debian_nfs_server
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: postgres
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.122            <font color="blue"># DB Server(PostgreSQL) IP 주소</font>
  persistent_disk: 4096
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: postgres
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: uaa
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.142             <font color="blue"># UAA IP 주소</font>
  properties:
    consul:
      agent:
        services:
        - uaa
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: uaa
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: consul_agent
    release: openpaas
  update: {}

- name: login
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.143            <font color="blue"># Login Server IP 주소</font>
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: login
  - name: metron_agent
    release: openpaas
  update: {}

- name: api
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.144               <font color="blue"># Cloud Controller IP 주소</font>
  persistent_disk: 0
  properties:
    consul:
      agent:
        services:
        - cloud_controller_ng
        - routing-api
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
    nfs_server:
      address: 10.30.40.112             <font color="blue"># NFS Server IP 주소</font>
      allow_from_entries:
      - 10.30.0.0/16                    <font color="blue"># 허용 Network CIDR 값</font>
      share: null
  resource_pool: large
  templates:
  - name: cloud_controller_ng
    release: openpaas
  - name: routing-api
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: statsd-injector
    release: openpaas
  - name: consul_agent
    release: openpaas
  - name: nfs_mounter
    release: openpaas
  update: {}

- name: clock_global
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.145             <font color="blue"># Cloud Controller Clock IP 주소</font>
  persistent_disk: 0
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: cloud_controller_clock
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: api_worker
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.146             <font color="blue"># CC Worker IP 주소</font>
  persistent_disk: 0
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
    nfs_server:
      address: 10.30.40.112             <font color="blue"># NFS Server IP 주소</font>
      allow_from_entries:
      - 10.30.0.0/16                    <font color="blue"># 허용 Network CIDR 값</font>
      share: null
  resource_pool: small
  templates:
  - name: cloud_controller_worker
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: consul_agent
    release: openpaas
  - name: nfs_mounter
    release: openpaas
  update: {}

- name: hm9000
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.147            <font color="blue"># Health Manager IP 주소</font>
  properties:
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: medium
  templates:
  - name: hm9000
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: runner
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.148              <font color="blue"># DEA IP 주소</font>
  properties:
    dea_next:
      zone: z1
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: runner
  templates:
  - name: dea_next
    release: openpaas
  - name: dea_logging_agent
    release: openpaas
  - name: metron_agent
    release: openpaas
  update:
    max_in_flight: 1

- name: doppler
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.149            <font color="blue"># Doppler IP 주소</font>
  properties:
    networks:
      apps: op_network
    doppler:
      zone: z1
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
  resource_pool: medium
  templates:
  - name: doppler
    release: openpaas
  - name: syslog_drain_binder
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: loggregator_trafficcontroller
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.151               <font color="blue"># Loggregator Controller IP 주소</font>
  properties:
    networks:
      apps: op_network
    traffic_controller:
      zone: z1
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
  resource_pool: small
  templates:
  - name: loggregator_trafficcontroller
    release: openpaas
  - name: metron_agent
    release: openpaas
  update: {}

- name: router
  instances: 1
  networks:
  - name: op_network
    static_ips: 10.30.40.115              <font color="blue"># Router IP 주소</font>
  properties:
    consul:
      agent:
        services:
        - gorouter
    metron_agent:
      zone: z1
      deployment: openpaas- beta-1.0
    networks:
      apps: op_network
  resource_pool: router
  templates:
  - name: gorouter
    release: openpaas
  - name: metron_agent
    release: openpaas
  - name: consul_agent
    release: openpaas
  update: {}
</pre>

#### Properties
아래 Sample Manifest를 참조하여 설치 환경에 맞게 값을 수정한다.
<pre>
properties:
  acceptance_tests: null
  app_domains:
  - controller.open-paas.com	 <font color="blue"># DNS Server에 등록된 Platform Domain Name</font>
  app_ssh: null
  cc:                                        <font color="blue"># 여기서부터 Cloud Controller Properties</font>
    allow_app_ssh_access: true
    allowed_cors_domains: []
    app_events:
      cutoff_age_in_days: 31
    app_usage_events:
      cutoff_age_in_days: 31
    audit_events:
      cutoff_age_in_days: 31
    billing_event_writing_enabled: false
    broker_client_default_async_poll_interval_seconds: 60
    broker_client_max_async_poll_duration_minutes: 10080
    broker_client_timeout_seconds: 70
    buildpacks:
      buildpack_directory_key: controller-paas.com-cc-buildpacks
      cdn: null
      fog_connection: null
    bulk_api_password: admin          <font color="blue"># Bulk API Password 설정</font>
    client_max_body_size: 2048M
    db_encryption_key: admin          <font color="blue"># DB Encryprion Key 지정</font>
    db_logging_level: debug2
    default_app_disk_in_mb: 1024
    default_app_memory: 1024
    default_buildpacks:
    - name: java_buildpack_offline
      package: buildpack_java_offline
    - name: java_buildpack
package: buildpack_java
    default_health_check_timeout: 60
    default_quota_definition: default
    default_running_security_groups:
    - public_networks
    - dns
    default_staging_security_groups:
    - public_networks
    - dns
    default_to_diego_backend: false
    development_mode: false
    diego_docker: false
    directories: null
    disable_custom_buildpacks: false
droplets:
      cdn: null
      droplet_directory_key: controller.open-paas.com-cc-droplets
      fog_connection: null
    external_host: api
    external_protocol: https             <font color="blue"># HTTP or HTTPS</font>
    install_buildpacks:
- name: java_buildpack_offline
      package: buildpack_java_offline
    - name: java_buildpack
      package: buildpack_java
    internal_api_password: admin            <font color="blue"># Internal API Password</font>
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
      environment_name: null
      license_key: null
      monitor_mode: false
      transaction_tracer:
        enabled: true
        record_sql: obfuscated
    packages:
      app_package_directory_key: controller.open-paas.com-cc-packages
      cdn: null
      fog_connection: null
      max_package_size: 1073741824
    quota_definitions:         <font color="blue"># Application Instance Default Quota 값 지정</font>
      default:
        memory_limit: 10240
        non_basic_services_allowed: true
        total_routes: 1000
        total_services: 100
    resource_pool:
      cdn: null
      fog_connection: null
      resource_directory_key: controller.open-paas.com-cc-resources
security_group_definitions:
    - name: public_networks
      rules:
      - destination: 0.0.0.0-169.253.255.255
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
        protocol: u에
srv_api_uri: https://api.controller.open-paas.com     <font color="blue"># Platform API Target URL</font>
    staging_upload_password: admin          <font color="blue"># Staging Upload Password</font>
    staging_upload_user: staging_upload_user
    system_buildpacks:
    - name: java_buildpack_offline
      package: buildpack_java_offline
    - name: java_buildpack
      package: buildpack_java
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
    users_can_select_backend: true
ccdb:                         <font color="blue"># CCDB Properties</font>
    address: 10.30.40.122         <font color="blue"># DB Server(PostgreSQL) VM IP 주소</font>
    databases:
    - citext: true
      name: ccdb
      tag: cc
    db_scheme: postgres
    port: 5524
    roles:
    - name: ccadmin
      password: admin          <font color="blue"># ccadmin 계정 Password</font>
      tag: admin
  collector: null
  consul:
    agent:
      log_level: null
      servers:
        lan:
        - 10.30.40.150          <font color="blue"># Consul VM IP 주소</font>
databases:
    address: 10.30.40.122	<font color="blue"># DB Server VM IP 주소</font>
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
    directory_server_protocol: http
    disk_mb: 32768
    disk_overcommit_factor: 2
    evacuation_bail_out_time_in_seconds: 600
    heartbeat_interval_in_seconds: 10
    instance_disk_inode_limit: 200000
    kernel_network_tuning_enabled: true
    logging_level: debug
    memory_mb: 16384
    memory_overcommit_factor: 3
    rlimit_core: 0
    staging_disk_inode_limit: 200000
    staging_disk_limit_mb: 6144
    staging_memory_limit_mb: 1024
  description: Open PaaS sponsored by OCP Team
  disk_quota_enabled: true
  domain: controller.open-paas.com
  doppler:
    blacklisted_syslog_ranges: null
    debug: false
    maxRetainedLogMessages: 100
    unmarshaller_count: 5
  doppler_endpoint:
    shared_secret: admin            <font color="blue"># Doppler Endpoint Password</font>
  dropsonde:
    enabled: true
etcd:
    machines:
    - 10.30.40.124                  <font color="blue"># etcd VM IP 주소</font>
  etcd_metrics_server:
    nats:
      machines:
      - 10.30.40.111                <font color="blue"># NATS Server VM IP 주소</font>
      password: admin
      username: nats
  hm9000:
    url: http://hm9000.controller.open-paas.com       <font color="blue"># HM9000 URL(DNS Name 확인)</font>
  logger_endpoint: null
  loggregator:
    blacklisted_syslog_ranges: null
    debug: false
    maxRetainedLogMessages: 100
  loggregator_endpoint:
    shared_secret: admin
login:
    analytics:
      code: null
      domain: null
    asset_base_url: null
    brand: oss
    catalina_opts: -Xmx768m -XX:MaxPermSize=256m
    enabled: true
    invitations_enabled: null
    links:
      home: http://console.controller.open-paas.com        <font color="blue"># Web Console URL(DNS Name 확인)</font>
      network: null
      passwd: http://console.controller.open-paas.com/password_resets/new
      signup: http://console.controller.open-paas.com/register
      signup-network: null
    logout: null
    messages: null
    notifications:
      url: null
    protocol: http
    restricted_ips_regex: 10\.\d{1,3}\.\d{1,3}\.\d{1,3}|192\.168\.\d{1,3}\.\d{1,3}|169\.254\.\d{1,3}\.\d{1,3}|127\.\d{1,3}\.\d{1,3}\.\d{1,3}|172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}|172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}
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
uaa_certificate: null
    url: null
  metron_agent:
    deployment: null
  metron_endpoint:
    shared_secret: admin
  nats:
    address: 10.30.40.111             <font color="blue"># NATS Server VM IP 주소</font>
    debug: false
    machines:
    - 10.30.40.111                   <font color="blue"># NATS Server VM IP 주소</font>
    monitor_port: 0
    password: admin
    port: 4222
    prof_port: 0
    trace: false
    user: nats
  nfs_server:
    address: 10.30.40.112            <font color="blue"># NFS Server VM IP 주소</font>
    allow_from_entries:
    - 10.30.0.0/16                   <font color="blue"># NFS Mount 허용 Range 지정</font>
    share: null
  request_timeout_in_seconds: 900
  router:
    cipher_suites: null
    enable_routing_api: null
    enable_ssl: null
    requested_route_registration_interval_in_seconds: 20
    secure_cookies: false
    ssl_cert: null
    ssl_key: null
    status:
      password: admin
      user: router_status
smoke_tests: null
  ssl:
    skip_cert_verify: true
  support_address: http://support.ocp.com
  syslog_daemon_config: null
  system_domain: controller.open-paas.com            <font color="blue"># DNS Server에 등록한 Platform Domain Name</font>
  system_domain_organization: OCP
  uaa:
    admin:
      client_secret: admin                  <font color="blue"># admin 계정 Password</font>
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
      cloud_controller_username_lookup:
        authorities: scim.userids
        authorized-grant-types: client_credentials
        secret: admin
      doppler:
        authorities: uaa.resource
        override: true
        secret: admin
      gorouter:
        authorities: clients.read,clients.write,clients.admin,route.admin,route.advertise
        authorized-grant-types: client_credentials,refresh_token
        scope: openid,cloud_controller_service_permissions.read
secret: admin
      login:
        authorities: oauth.login,scim.write,clients.read,notifications.write,critical_notifications.write,emails.write,scim.userids,password.write
        authorized-grant-types: authorization_code,client_credentials,refresh_token
        override: true
        redirect-uri: http://login.controller.open-paas.com
        scope: openid,oauth.approvals
        secret: admin
      notifications:
        authorities: cloud_controller.admin,scim.read
        authorized-grant-types: client_credentials
        secret: admin
    database: null
    issuer: http://uaa.controller.open-paas.com
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
    no_ssl: true
    restricted_ips_regex: 10\.\d{1,3}\.\d{1,3}\.\d{1,3}|192\.168\.\d{1,3}\.\d{1,3}|169\.254\.\d{1,3}\.\d{1,3}|127\.\d{1,3}\.\d{1,3}\.\d{1,3}|172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}|172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}
    scim:
      external_groups: null
      userids_enabled: true
      users:
      - admin|admin|scim.write,scim.read,openid,cloud_controller.admin,dashboard.user,console.admin,console.support
    spring_profiles: null
    url: http://uaa.controller.open-paas.com
    user: null
    zones: null
  uaadb:
    address: 10.30.40.122          <font color="blue"># DB Server VM IP 주소</font>
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
</pre>

<div id='15'/>
###Bosh Deploy
지금까지 설치를 위한 준비 과정이 정상적으로 수행되었으면, 지금부터 Open PaaS Controller를 IaaS 환경(vSphere)에 아래의 절차로 설치한다.

####Deployment Manifest 지정
>bosh deployment openpaas-vsphere-beta-1.0.yml
>
>“bosh deployment” 명령어로 생성한 Deployment Manifest File을 지정하고, 아래의 그림과 같이 동일한 명령어로 정상 지정 되었는지를 확인한다.<br>
>![controller_vsphere_image010]
 
####Open PaaS Controller Deploy
“bosh deploy” 명령으로 Open PaaS Controller 설치를 수행한다.
>bosh deploy
>
>보통 설치 과정은 1-2시간 정도가 소요되며 정상적으로 설치가 완료되면 아래 그림과 같은 메세지를 출력하게 된다.
>![controller_vsphere_image011]

<div id='16'/>
###설치형상 확인
설치가 정상적으로 완료된 후 “bosh vms” 명령으로 설치된 Open PaaS Controller의 형상을 확인한다.
>bosh vms
>
>아래 그림과 같이 Deployment Name, Virtual Machine, IP 주소 등의 정보를 확인할 수 있다.
>![controller_vsphere_image012]

<div id='17'/>
#설치 검증
<div id='18'/>
###CF Login
>cf api https://api.controller.open-paas.com –skip-ssl-validation<br>
>…<br>
>cf login<br>
>Email> admin<br>
>Password> admin<br>
>OK<br>
>…<br>
>cf create-space dev<br>
>cf target -o OCP -s dev<br>
>…<br>

CF Target을 지정하고, Login을 수행한다. 이 때 계정은 admin/admin을 사용한다.
Application을 Deploy할 ORG(Default: OCP)와 Space를 생성하고, 해당하는 ORG/Space로 Targetting 한다.

<div id='19'/>
###Application Deploy
설치 패키지와 함께 배포된 Sample Application이 위치하는 디렉토리로 이동하고 Application을 Deploy 한다.
>cd $INSTALL_PACKAGE/OpenPaaS-Sample-Apps/Etc/hello-spring<br>
>cf push
>
>Application이 정상 Deploy가 되면 아래와 같은 메시지가 출력된다.
>![controller_vsphere_image013]

<div id='20'/>
###Application Access
Deploy한 Application URL을 Browser 또는 curl 명령어로 Access하여 정상 접근 되는지를 확인한다.
![controller_vsphere_image014]

[controller_vsphere_image002]:/images/openpaas-controller/controller_vsphere_image002.png
[controller_vsphere_image003]:/images/openpaas-controller/controller_vsphere_image003.png
[controller_vsphere_image004]:/images/openpaas-controller/controller_vsphere_image004.png
[controller_vsphere_image005]:/images/openpaas-controller/controller_vsphere_image005.png
[controller_vsphere_image006]:/images/openpaas-controller/controller_vsphere_image006.png
[controller_vsphere_image007]:/images/openpaas-controller/controller_vsphere_image007.png
[controller_vsphere_image008]:/images/openpaas-controller/controller_vsphere_image008.png
[controller_vsphere_image009]:/images/openpaas-controller/controller_vsphere_image009.png
[controller_vsphere_image010]:/images/openpaas-controller/controller_vsphere_image010.png
[controller_vsphere_image011]:/images/openpaas-controller/controller_vsphere_image011.png
[controller_vsphere_image012]:/images/openpaas-controller/controller_vsphere_image012.png
[controller_vsphere_image013]:/images/openpaas-controller/controller_vsphere_image013.png
[controller_vsphere_image014]:/images/openpaas-controller/controller_vsphere_image014.png

