# Open PaaS PaaSTA 가이드 문서 

1. PaaSTA 설치 가이드

	1.1. [BOSH-Lite](./Install-Guide/BOSH%20Lite/OpenPaaS_PaaSTA_BOSH_Lite_install_guide.md)
 
	1.2. BOSH ([AWS](./Install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_AWS_install_guide.md), [OpenStack](./Install-Guide/BOSH/OpenPaaS_PaaSTA_BOSH_Openstack_install_guide.md))

	1.3. Controller ([vSphere](./Install-Guide/Controller/Controller_vSphere_install_guide.md),
	[AWS](./Install-Guide/Controller/Controller_AWS_install_guide.md),
	[Openstack](./Install-Guide/Controller/Controller_Openstack_install_guide.md))

	1.4. Container ([vSphere](./Install-Guide/Container/Container_vSphere_install_guide.md),
	[AWS](./Install-Guide/Container/Container_AWS_install_guide.md),
	[Openstack](./Install-Guide/Container/Container_Openstack_install_guide.md))

2. PaaSTA 서비스 설치 가이드

	2.1. DBMS 

	- 2.1.1. Cubrid (	[vSphere](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_vSphere_install_guide.md), 
	[AWS](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/DBMS/OpenPaaS_PaaSTA_ServicePack_Cubrid_BOSH-Lite_install_guide.md))

	- 2.1.2. Mysql ([vSphere](./Service-Guide/DBMS/ServicePack_MySQL_vSphere_install_guide.md), 
	[AWS](./Service-Guide/DBMS/ServicePack_MySQL_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/DBMS/ServicePack_MySQL_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/DBMS/ServicePack_MySQL_BOSH-Lite_install_guide.md))

	2.2. NOSQL

	- 2.2.1. Mongodb ([vSphere](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_vSphere_install_guide.md), 
	[AWS](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/NOSQL/OpenPaaS_PaaSTA_ServicePack_MongoDB_BOSH-Lite_install_guide.md))

	- 2.2.2. Redis ([vSphere](./Service-Guide/NOSQL/ServicePack_Redis_vSphere_install_guide.md), 
	[AWS](./Service-Guide/NOSQL/ServicePack_Redis_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/NOSQL/ServicePack_Redis_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/NOSQL/ServicePack_Redis_BOSH-Lite_install_guide.md))

	2.3. Storage

	- 2.3.1. GlusterFS ([vSphere](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_vSphere_install_guide.md), 
	[AWS](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/Storage/OpenPaaS_PaaSTA_ServicePack_GlusterFS_BOSH-Lite_install_guide.md))

	2.4. MasageQueue

	- 2.4.1. RabbitMQ ([vSphere](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_vSphere_install_guide.md), 
	[AWS](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/MessageQueue/ServicePack_RabbitMQ_BOSH-Lite_install_guide.md))

	2.5. ETC

	- 2.5.1. API Platform ([vSphere](./Service-Guide/ETC/ServiceBroker_APIPlatform_vSphere_install_guide.md), 
	[AWS](./Service-Guide/ETC/ServiceBroker_APIPlatform_AWS_install_guide.md), 
	[OpenStack](./Service-Guide/ETC/ServiceBroker_APIPlatform_Openstack_install_guide.md), 
	[Bosh-Lite](./Service-Guide/ETC/ServiceBroker_APIPlatform_BOSH_Lite_install_guide.md))

3. PaaSTA 활용 가이드

	3.1. [BOSH CLI](./Use-Guide/OpenPaaS_PaaSTA_BOSH_CLI_guide.md)

	3.2. [외부 Identity 서비스 통합](./Use-Guide/OpenPaaS_PaaSTA_외부Identity서비스_integrate_guide.md)
	
	3.3. [UAA 권한관리](./Use-Guide/OpenPaaS_PaaSTA_UAA권한관리_guide.md)


4. PaaSTA 배포 어플리케이션 개발 가이드

	4.1. [Node.js](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Nodejs_develope_guide.md)

	4.2. [PHP](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_PHP_develope_guide.md)

	4.3. [Python](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Python_develope_guide.md)

	4.4. [Ruby](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Ruby_develope_guide.md)
	
	4.5. [Java](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Java_develope_guide.md)
		
	4.6. [Go](./Sample-App-Guide/OpenPaaS_PaaSTA_Application_Go_develope_guide.md)
	
5. PaaSTA 개발 가이드

	5.1. [스템셀 개발 가이드](./Development-0Guide/OpenPaaS_PaaSTA_Build_Stemcell_guide.md)
	
	5.2. [서비스팩 개발 가이드](./Development-0Guide/ServicePack_develope_guide.md)
	
	5.3. [빌드팩 개발 가이드](./Development-Guide/Buildpack_develope_guide.md)
	
	5.4. [어플리케이션 APIPlatform 도로주소 개발 가이드](./Development-Guide/Application_APIPlatform_dorojuso_devlope_guide.md)
	
	5.5. [퍼블릭 API 개발 가이드](./Development-Guide/PublicAPI_devlope_guide.md)

