# Open PaaS(PaaSTA) 가이드 문서 


## 플랫폼 설치 가이드
- [설치 파일 다운로드 받기](./Download_Page.md)
- 개인 환경 설치
  - [BOSH-Lite](./install-Guide/install-guide/BOSH-LITE/OpenPaaS_PaaSTA_BOSH_Lite_install_guide.md)
- 운영 환경 설치
  - BOSH 설치([AWS](./install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_AWS_install_guide.md), [OpenStack](./install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_Openstack_install_guide.md))
  - Controller 설치([vSphere](./install-Guide/Controller/Controller_vSphere_install_guide.md),
[AWS](./install-Guide/Controller/Controller_AWS_install_guide.md), [Openstack](./install-Guide/Controller/Controller_Openstack_install_guide.md))
  - Container 설치([vSphere](./install-Guide/Container/Container_vSphere_install_guide.md),
[AWS](./install-Guide/Container/Container/Container_AWS_install_guide.md),
[Openstack](./install-Guide/Container/Container_Openstack_install_guide.md))
- 플랫폼 설치
  - [플랫폼 설치 자동화](./install-Guide/Platform-Install-System-1/OpenPaaS_PaaSTA_Platform_Install_System_install_guide.md)
 
## 서비스 설치 가이드
- DBMS 설치
  - Cubrid ( [vSphere](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_vSphere_install_guide.md), 
[AWS](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_AWS_install_guide.md), 
[OpenStack](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_BOSH-Lite_install_guide.md))
  - Mysql ([vSphere](./Service-Guide/DBMS/ServicePack_MySQL_vSphere_install_guide.md), 
[AWS](./Service-Guide/DBMS/ServicePack_MySQL_AWS_install_guide.md), 
[OpenStack](./Service-Guide/DBMS/ServicePack_MySQL_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/DBMS/ServicePack_MySQL_BOSH-Lite_install_guide.md))
- NOSQL 설치
  - Mongodb ([vSphere](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_vSphere_install_guide.md), 
[AWS](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_AWS_install_guide.md), 
[OpenStack](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_Openstack_install_guide.md), 
[Bosh-Lite](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_BOSH-Lite_install_guide.md))
  - Redis ([vSphere](./Service-Guide/NOSQL/ServicePack_Redis_vSphere_install_guide.md), 
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

## 활용 가이드
- [BOSH CLI(Command Line Interface) 사용](./Use-Guide/OpenPaaS_PaaSTA_BOSH_CLI_guide.md)
- [CF CLI(Command Line Interface) 사용](./Use-Guide/OpenPaas-CLi-guide.md)
- [Eclipse plugin 개발도구 사용] (./Use-Guide/Open_PaaS_develop_guide.md)


## 개발 언어 별 어플리케이션 가이드
- [Node.js](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Nodejs_develope_guide.md)
- [PHP](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_PHP_develope_guide.md)
- [Python](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Python_develope_guide.md)
- [Ruby](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Ruby_develope_guide.md)
- [Java](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Java_develope_guide.md)
- [Go](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Go_develope_guide.md)
	
## 플랫폼 개발 가이드
- [스템셀 개발 가이드](./Development-Guide/OpenPaaS_PaaSTA_Build_Stemcell_guide.md)
- [서비스팩 개발 가이드](./Development-Guide/ServicePack_develope_guide.md)
- [빌드팩 개발 가이드](./Development-Guide/Buildpack_develope_guide.md)
- [어플리케이션 APIPlatform 도로주소 개발 가이드](./Development-Guide/Application_APIPlatform_dorojuso_devlope_guide.md)
- [퍼블릭 API 개발 가이드](./Development-Guide/PublicAPI_devlope_guide.md)
