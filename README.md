# Open PaaS PaaSTA 가이드 문서 


## PaaSTA 설치 가이드

- [개인 환경(BOSH-Lite) 설치](./Install-Guide/BOSH%20Lite/OpenPaaS_PaaSTA_BOSH_Lite_install_guide.md)
- BOSH 운영 환경 설치([AWS](./Install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_AWS_install_guide.md), [OpenStack](./Install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_Openstack_install_guide.md))
- Controller 운영 환결 설치([vSphere](./Install-Guide/Controller/Controller_vSphere_install_guide.md),
[AWS](./Install-Guide/Controller/Controller_AWS_install_guide.md), [Openstack](./Install-Guide/Controller/Controller_Openstack_install_guide.md))
- Container 운영 환경 설치([vSphere](./Install-Guide/Container/Container_vSphere_install_guide.md),
[AWS](./Install-Guide/Container/Container_AWS_install_guide.md),
[Openstack](./Install-Guide/Container/Container_Openstack_install_guide.md))

## PaaSTA 서비스 설치 가이드
- DBMS 설치
  - Cubrid (	[vSphere](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_vSphere_install_guide.md), 
[AWS](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_AWS_install_guide.md), 
[OpenStack](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_BOSH-Lite_install_guide.md))
  2. Mysql ([vSphere](./Service-Guide/DBMS/ServicePack_MySQL_vSphere_install_guide.md), 
[AWS](./Service-Guide/DBMS/ServicePack_MySQL_AWS_install_guide.md), 
[OpenStack](./Service-Guide/DBMS/ServicePack_MySQL_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/DBMS/ServicePack_MySQL_BOSH-Lite_install_guide.md))
- NOSQL 설치
  - Mongodb ([vSphere](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_vSphere_install_guide.md), 
[AWS](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_AWS_install_guide.md), 
[OpenStack](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_BOSH-Lite_install_guide.md))
  2. Redis ([vSphere](./Service-Guide/NOSQL/ServicePack_Redis_vSphere_install_guide.md), 
[AWS](./Service-Guide/NOSQL/ServicePack_Redis_AWS_install_guide.md), 
[OpenStack](./Service-Guide/NOSQL/ServicePack_Redis_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/NOSQL/ServicePack_Redis_BOSH-Lite_install_guide.md))
- Storage 설치
  - GlusterFS ([vSphere](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_vSphere_install_guide.md), 
[AWS](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_AWS_install_guide.md), 
[OpenStack](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_BOSH-Lite_install_guide.md))
- MasageQueue 설치
  - RabbitMQ ([vSphere](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_vSphere_install_guide.md), 
[AWS](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_AWS_install_guide.md), 
[OpenStack](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_BOSH-Lite_install_guide.md))
- API Platform 설치
  - WSO2([vSphere](./Service-Guide/ETC/ServiceBroker_APIPlatform_vSphere_install_guide.md), 
[AWS](./Service-Guide/ETC/ServiceBroker_APIPlatform_AWS_install_guide.md), 
[OpenStack](./Service-Guide/ETC/ServiceBroker_APIPlatform_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/ETC/ServiceBroker_APIPlatform_BOSH_Lite_install_guide.md))

## PaaSTA 활용 가이드
- [BOSH CLI(Command Line Interface) 사용](./Use-Guide/OpenPaaS_PaaSTA_BOSH_CLI_guide.md)
- [CF CLI(Command Line Interface) 사용](Use-Guide/OpenPaas%20CLi%20가이드.md)
- [Eclipse plugin 개발도구 사용] (./Use-Guide/Open%20PaaS%20%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%20%EC%82%AC%EC%9A%A9%20%EA%B0%80%EC%9D%B4%EB%93%9C.md)
- [인증인가(UAA) 서비스 관리](./Use-Guide/OpenPaaS_PaaSTA_UAA권한관리_guide.md)
- [외부 인증/인가 서비스 통합](./Use-Guide/OpenPaaS_PaaSTA_외부Identity서비스_integrate_guide.md)
- [Open PaaS 관리자 대시보드 사용](./Use-Guide/Open%20PaaS%20%EA%B4%80%EB%A6%AC%EC%9E%90%20%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C%20%EC%82%AC%EC%9A%A9%EA%B0%80%EC%9D%B4%EB%93%9C.md)

## 개발 언어 별 PaaSTA 배포 어플리케이션 개발 가이드
- [Node.js](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Nodejs_develope_guide.md)
- [PHP](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_PHP_develope_guide.md)
- [Python](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Python_develope_guide.md)
- [Ruby](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Ruby_develope_guide.md)
- [Java](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Java_develope_guide.md)
- [Go](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Go_develope_guide.md)
	
## PaaSTA 플랫폼 개발 가이드
- [스템셀 개발 가이드](./Development-0Guide/OpenPaaS_PaaSTA_Build_Stemcell_guide.md)
- [서비스팩 개발 가이드](./Development-0Guide/ServicePack_develope_guide.md)
- [빌드팩 개발 가이드](./Development-Guide/Buildpack_develope_guide.md)
- [어플리케이션 APIPlatform 도로주소 개발 가이드](./Development-Guide/Application_APIPlatform_dorojuso_devlope_guide.md)
- [퍼블릭 API 개발 가이드](./Development-Guide/PublicAPI_devlope_guide.md)

