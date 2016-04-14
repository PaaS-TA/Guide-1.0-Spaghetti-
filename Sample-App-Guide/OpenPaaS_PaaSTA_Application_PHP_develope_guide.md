## Table of Contents
1. [문서 개요](#1--문서-개요)
     * [목적](#11--목적)
     * [범위](#12--범위)
     * [참고자료](#13--제약사항)
     * [참고자료](#14--참고자료)
2. [개발환경 구성](#2--개발환경-구성)
     * [PHP 샘플 소스 받기](#21--PHP-샘플-소스-받기)
     * [XAMP설치](#22--XAMP설치)
     * [PHP 실행 환경설정](#23--PHP-실행-환경설정)
     * [Composer 설치](#24--Composer-설치)
     * [Mongo 드라이버 설치](#25--Mongo-드라이버-설치)
3. [개발](#3--개발)
     * [사용 Package 설명](#31--사용-Package-설명)
     * [디렉토리설명](#32--디렉토리설명)
     * [애플리케이션 환경설정](#33--애플리케이션-환경설정)
     * [VCAP_SERVICES 환경설정 정보](#34--VCAP_SERVICES-환경설정-정보)
     * [Mysql 연동](#32--Mysql-연동)
     * [CUBRID 연동](#32--CUBRID-연동)
     * [MongoDB 연동](#32--MongoDB-연동)
     * [Redis 연동](#32--Redis-연동)
     * [RabbitMQ 연동](#32--RabbitMQ-연동)
     * [GlusterFS 연동](#32--GlusterFS-연동)
4. [배포](#4--배포)
5. [테스트](#5--테스트)


#1.  문서 개요

## 1.1.  목적 

본 문서(PHP 애플리케이션 개발 가이드)는 개발형 플랫폼 프로젝트의 서비스를PHP 애플리케이션과 연동하는 방법을 제공하는데 있습니다.

## 1.2.  범위

연동을 하는 서비스는 MySQL, MongoDB, Redis, GlusterFS 입니다. 데이터 저장에는 MySQL, MongoDB를 사용하고 사용자의 Session은 Redis에 샘플 어플리케이션에서 사용하는 이미지 파일 관리(Upload)를 위해서는 GlusterFS를 사용합니다.

## 1.3.  제약사항

현재 PHP 빌드팩(CloudFoundry의 공식 빌드팩 v4.3.1)의 지원하는 드라이버가 본 사업의 서비스와 정확하게 맞지 않아 일부 서비스(RabbitMQ, CUBRID)는 연동하지 못했습니다. 또한 MongoDB와 연결은 DB인증 절차가 적용이 되지 않아 DB Admin 계정으로 접속하는 방법을 설명하였습니다.
실제 사용시에는 PHP 빌드팩을커스터마이징하여 프로젝트 환경에 맞추어서 개발을 진행하셔야 합니다.


## 1.4.  참고자료

-	PHP 빌드팩 :https://github.com/cloudfoundry/php-buildpack
-	XAMP 사이트 :https://www.apachefriends.org/index.html



#2. 개발환경 구성

개발환경은 개방형 플랫폼의 네트워크의 구성에 따라 로컬에 구성을 하거나 직접 개방형 플랫폼에 Deploy하여 관리할 수 있습니다. 여기서는 Windows 환경에 간단하게 개발환경을 구성하고 개방형 플랫폼에 배포(Deploy)하는 방법을 설명하겠습니다.
PHP 개발환경을 구성하려면 Web Server와 PHP 엔진, Extension 설치등을해야하는데 이를 간편하게 구성 놓은 툴이 있습니다. 본 가이드에서는 XAMP를 이용하여 설치를 하고 구성하도록 하겠습니다.

본 문서 작성을 위해 구성한 시스템은 아래와 같습니다.
-	OS : Windows 8.1 64bit
-	XAMP PHP 5.5.30
-	Mongo 라이브러리 :
-	Composer : 

PHP로 REST/full 서버를 구현하였고 화면(HTML)은 Apache의 Web 서버에서 제공을 합니다. HTML과 PHP는 별도로 돌아가는 구조입니다.

## 2.1.  PHP 샘플 소스 받기

샘플의 위치는 변경될수있느나 개방형 플랙폼 홈페이지에서 찾아볼 수 있습니다. 해당 GIT 위치를 확인하시고 아래와 같은 명령문으로 소스를 다운로드 받습니다. 해당 명령을 위해서는 GIT Client가 설치되어 있어야 합니다.

$ git clone 



## 2.2.  XAMP설치

BOSH는 스템셀을 생성하는 VM을 AWS에 생성하고 관리한다. 스템셀을 생성하기 위해서는 AWS에 계정을 생성하고 스템셀을 생성하기 위한 환경을 구성해야 한다.
  1.	URL (https://www.apachefriends.org/index.html) 에 접속하면 바로 다운로드 화면이 나옵니다. 여기서 "Click here for other version"을 선택합니다. 
    
  ![./image/php/php_develope_guide2.png](./image/php/php_develope_guide2.png)<br>
  XAMP 공식 홈페이지 첫화면
    
  다운로드는 Windows버전 PHP 5.5.30 (32bit)을 다운로드 받습니다.
    
  2.	다운로드 받은 파일을 실행하고 모두 Next를 하면됩니다. 하지만 아래와 같이 디렉토리를 물어보는데 이때 이 위치를 변경하거나 정확하게 기억하고 있어야 합니다. 설치 완료후php 실행 디렉토리를 환경변수(Path)에 넣어 줘야 하기 때문입니다.
    
  ![./image/php/php_develope_guide3.png](./image/php/php_develope_guide3.png)<br>
    XAMP 설치 디렉토리
    
  3.	설치가 정상적으로 이루어지고 있으면 아래와 같이 진행이 될겁니다. 처음 실행할 때 Antivirus 프로그램으로 느려질수 있다는 문구 등이 나올수도 있는데 "확인"을 누르시면 됩니다.
    
  ![./image/php/php_develope_guide4.png](./image/php/php_develope_guide4.png)<br>
    XAMP 설치 진행중
    
  4.	설치가 완료되면 Control Panel을 띄우겠다는 메시지가 나옵니다. 선택이 Default로 되어 있어 완료를 선택하면 아래와 같은 Control Panel이 실행됩니다.
    
  ![./image/php/php_develope_guide5.png](./image/php/php_develope_guide5.png)<br>
    XAMP 관리 패널 창
    
  사용방법은 간단합니다. 원하는 서비스(여기서는 Apache 만 사용할 예정)의 Start를 선택하면 해당 서비스가 실행이 됩니다. 단. 해당서비스가 사용하는 포트(Apache의 경우 80)는 사용하고 있지 않아야 합니다.
    
  5.	Apache의 Config를 선택하고 Apache (httpd.conf)를 선택하여 DocumentRootdhk Directory의 위치를 개발소스가 설치된 곳으로 바꾸면 브라우져에서http://localhost 로 호출시 개발하는 위치로 바로 연결됩니다. 개발소스의 위치는 2.2.1에서 설치한 위치를 지정해 넣습니다.
        
        
      DocumentRoot C:\개발소스위치
      <DirectoryC:\개발소스위치>
      

## 2.3. PHP 실행 환경설정

  1.	XAMP로 설치된 PHP를 어디서나 실행가능하게 환경설정(Path)에 넣어줍니다. 제어판 -> 시스템 -> 고급 시스템 설정을 선택하면 아래와 같이 시스템 속성을 변경하는 창이 나타납니다. 

  ![./image/php/php_develope_guide5.png](./image/php/php_develope_guide6.png)<br>
  시스템 속성 창
  
  2.	여기서 "환경변수"를 선택하고 Path를 편집을 합니다. 변수값의 마지막에 XAMP 설치 디렉토리 아래의 php디렉토리를 추가합니다. 

  ![./image/php/php_develope_guide5.png](./image/php/php_develope_guide7.png)<br>
    Path 환경변수 설정

  3.	정상적으로 구성이 되었는지 확인하려면 "명령 프롬프트"를 실행하고 php-version 을 선택하여 아래와 같이 나오면 정상적으로 환경설정이 완료된 것입니다.
 
  ![./image/php/php_develope_guide5.png](./image/php/php_develope_guide8.png)<br>
    명령 프롬프트에서 PHP 버전 확인


## 2.4.  2.2.4.	Composer 설치

  Composer는 개발시 필요한 라이브러리를 관리하는 툴입니다. 홈페이지는 다음과 같습니다. https://getcomposer.org/
  
  1.	Composer를 다운로드 받아 설치하고 Path에 설정하는 방법이 있지만 여기서는 Manual로 설치를 하겠습니다. Composer.phar 파일을 개방형 플랫폼에서 그대로 사용하고 있어 Manual 설치로 composer.phar파일을 개발 위치에 설치하였습니다. (소스를 Git에서 받았으면 따로 설치할 필요는 없습니다.)
  
  2.	매뉴얼 설치는 간단합니다. 아래와 같이 소스의 루트 디렉토리에서 입력을 하면됩니다. 물론 PHP 명령이 실행이 될 수 있게 2.2.3의 환경변수 설정이 되어 있어야만 합니다.
    php r "readfile('https://getcomposer.org/installer');" | php
  	
  3.	필요한 Package를 composer.json에 구성하고 install하면 PHP에서 사용할 수 있는 Package가 vendor 디렉토리 아래에 설치가 됩니다. 
  
  * 주의사항 *
  개방형 플랫폼에서 사용하기 위해서는 XAMP와는 환경이 틀리기 때문에 PHP 빌드팩의 Release된 Extension을 확인하고 Dependency를 확인해야 합니다. XAMP 환경에서는 동작을 하지만 개방형 플랫폼에서는 동작을 안할 수 있으니 주의해야 합니다.
  PHP 빌드팩Extenstion정보 :https://github.com/cloudfoundry/php-buildpack/releases



## 2.5. Mongo 드라이버 설치

  Mongo 드라이브 설치는 개방형 플랫폼에서 지원하는 Mongo 드라이버를 설치하기 위함입니다. 관련문서는 http://docs.php.net/manual/en/mongo.installation.php#mongo.installation.windows에 있습니다. 라이브러리 파일을 다운받고 config 파일에 정보를 추가하면 됩니다.
  
  1.	먼저 문서에 PECL 홈페이지(http://pecl.php.net/package/mongo)에서 DLL를 다운로드 받아야 합니다. 본 가이드는 1.6.12 버전을 을 선택하였습니다. 
   
  위의 링크중 DLL 부분을 눌러서 선택하고 "5.5 Thread Safe (TS) x86"를 다운로드 받습니다. 압축을 해제하면 php_mongo.dll이 있는데 이 파일만 있으면 됩니다.
  
  ![./image/php/php_develope_guide5.png](./image/php/php_develope_guide7.png)
  
  2.	php_mongo.dll 파일을 XAMP 설치된 디렉토리에서php 아래에 ext에 복사를 합니다.
  
  3.	XAMP 설치 디렉토리의php디렉토리에 php.ini를 선택하고 아래와 같이 추가를 해줍니다.
    extension=php_mongo.dll
  
  4.	php.ini 파일의 셋팅이 끝났으면 Apache서버를 재기동합니다. 만약에 오류가 있으면 XAMP 패널에 빨간색으로 오류가 표시되니 정상적으로 Apache서버가 올라오는지 확인합니다.
  
  5.	모듈이 정상적으로 설치되었는지 확인하려면 소스코드의 루트에 있는 info.php를 실행합니다. 브라우져에서http://localhost/info.php 를 선택하고 내용에 mongo 부분의 설정 정보가 보이면 정상적으로 설치가 된 것입니다.
  


#3.  개발

개발에 필요한 Package의 구성과 소스디렉토리 구성을 설명합니다. 그리고 각각의 서비스와 연동을 하는 부분의 설명합니다

## 3.1.  사용 Package 설명

Composer를 사용하여 Dependency를 관리합니다. Composer.json 파일의 Package구성을 설명하면 아래의 표와 같습니다. 설정정보는 composer.json에 저장되어 있습니다.


|패키지명 |버전 |설명 |
|--------------------------|------|--------------------------------------------|
|slim/slim                 |2.*     |PHP의 REST/full Framewok를 위해 사용합니다. |
|videlalvaro/php-amqplib   |2.5.*   |RabbitMQ 서비스와 연결을 위해 사용합니다. (SSL 접속이 지원되지 않아 현재는 사용하지 못하고 있음)| 
|predis/predis             |1.0.*   |Redis 서비스와 연동을 위해 사용합니다. |
|rackspace/php-opencloud   |1.15.*  |Openstack 연결 SDK로 GlusterFS에 파일을 Upload할 때 사용합니다. |
|guzzlehttp/guzzle         |6.*     |Http client로 GlusterFS의 Container 권한 변경시 사용합니다. |
|phpunit/phpunit           |4.3.*   |PHP의 단위테스트를 위한 프로그램입니다. Vendor\bin\phpunit를 실행하면 test 디렉토리의 Test Case를 실행하고 결과를 화면에 표시합니다. |


  Composer를 이용하여 라이브러리를 설치하려면 아래와 같이 install을 실행하면 됩니다. 이 명령은 개방형 플랫폼에 PHP 빌드팩과 같이 배포될 때 자동으로 수행이 됩니다. 
    phpcomposer.phar install
  
  만약에 로컬 개발환경에서 composer.json파일이 변경되면 install 대신 update를 이용하여 패키지의 구성을 변경하면 됩니다.



## 3.2.  디렉토리설명

개발 편의를 위해 API 서비스를 따로 디렉토리로구성하였으며 resource 디렉토리는 HTML에서 필요한 정적인 파일(js, css, image)이 있습니다.

|파일/폴더  |목적  |
|-----------|------------------------------------------|
|.bp-config|	PHP 빌드팩에서 사용하는 extension을 정의하는 곳입니다.|
|api|	REST API에 대한 소스가 있습니다. 각 서비스별로 구분되어 있습니다.|
|resources|	HTML에서 사용하는 정적인 파일(js, css, image 등)이 있습니다.|
|Test|	PHP 단위테스트를 위한 테스트 케이스가 있습니다.|
|vendor|	Composer로 설치되는 패키지가 있습니다.|
|.cfiignore|	개방형 플랫폼에 배포시 배포 예외를 시키는 파일/디렉토리를 정의합니다.|
|.htaccess|	REST/full 구현을 위해 url에 대한 패턴을 정의한 곳입니다.|
|composer.json, composer.phar, composer.lock|	Composer파일로 패키지의 Dependency를 관리하는 파일입니다.|
||Info.php|	PHP에 설치된 모듈을 확인하기 위한 phpinfo()를 포함한 웹페이지입니다.|
|login.html|	예제 실행을 위한 login 페이지입니다.|
|main.html|	예제의 조직도를 보여주기 위한 main페이지입니다.|
|manage..html|	예제의 데이터를 관리하기 위한 manage 페이지입니다. login이후에 보여지는 화면입니다.|
|manifest.yml|	개방형 플랫폼에 배포(push)할 때 사용하는 설정 파일입니다.|
|phpunit.xml|	PHP 단위테스트를 정의한 설정입니다.|


## 3.3.  애플리케이션 환경설정

REST/full 서비스를 위한 환경설정과 PHP 빌드팩에서 사용할 Extension을 적용해야 합니다. 

1.	REST/full 서비스를 위해 .htaccess 구성
REST/full API 형식인 /api/변수 를 처리하기 위한 설정입니다. 루트디렉토리에 .htaccess를 추가하고 아래와 같이 넣어줍니다.

\`#<IfModulemod_rewrite.c>
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule /(.*)$ /api/api.php?request=$1 [QSA,NC,L]
#</IfModule>\`
	
	
2.	PHP 빌드팩에 Extension 추가
XAMP에서 mongo 드라이브를 추가하였듯이 개방형 플랫폼에도 사용항 Extension 라이브러리를 추가해야합니다. 사용가능한 라이브러리는 여기()에서 확인이 가능합니다.
추가를 위해서는 .bp-config디렉토리에options.json 파일을 만들고 아래와 같이 추가해주면 됩니다.
\`{
    "PHP_EXTENSIONS": ["mysqli", "mongo", "amqp"]
}\`
 	mysqli :mysql과 연결을 위한 extension
 	mongo : mongo 연결을 위한 extension
 	amqp :rabbitmq와 연결을 위한 extension (현재 SSL연동이 안되서 사용을 못하고 있음)


## 3.4.  생성한 기본 OS 이미지의 보관장소 

1.  생성한 기본 OS 이미지 확인

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh remote
		$ ll /tmp


2.  생성한 기본 OS 다운로드
 
		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant scp remote:/tmp/<생성한 기본 OS 이미지명> <다운받을 로컬 경로>


#4.  BOSH 스템셀 생성 

## 4.1.  원격지의 OS 이미지를 사용한 스템셀 생성 

원격지의 OS 이미지를 사용해서 스템셀을 생성하는 절차를 기술한다.

1.  Build 실행

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh -c '
		cd /bosh
		CANDIDATE_BUILD_NUMBER=<current_build> bundle exec rake stemcell:build[vsphere,esxi,centos,7,go,bosh-os-images,bosh-centos-7-os-image.tgz]
		' remote


2.  입력 옵션 정보

	|옵션명                      |필수    |설명                       |예시|
	|--------------------------|------|-----------------------------------------------|------|
	|CANDIDATE_BUILD_NUMBER     |O      |현재 스템셀 버전            |3147|
	|Infrastructure             |O      |인프라 타입                 |Vsphere|
	|Hypervisor                 |O      |하이퍼 바이저 타입          |Esxi|
	|Operating system name      |O      |OS 타입                    |Centos|
	|Operating system version   |O      |OS 버전                    |7|
	|Agent type                 |X      |에이전트 타입               |Go|
	|OS image s3 bucket name    |O      |Bosh용 OS 이미지 버킷명     |Bosh-os-image|
	|OS image key               |O      |OS 이미지명                |Bosh-centos-7-os-image.tgz|


	※ 다른 OS image에 대해서는 다음을 참조한다. 
[http://s3.amazonaws.com/bosh-os-images/](http://s3.amazonaws.com/bosh-os-images/)

	※ Agent type타입이 필수 항목은 아니지만 현재 go 타입 이외의 에이전트는 지원하지 않으므로 go를 입력한다.


3.  설정 가능한 옵션 구성

	|Infrastructure             |Hypervisor                |OS|
	|--------------------------|-------------------------|----------------------------|
	|aws                        |Xen                       |ubuntu|
    |aws                        |Xen                       |centos|
	|openstack                  |Kvm						|ubuntu|                       
	|openstack                  |Kvm						|centos|		
	|vcloud					   |Esxi						 |ubuntu|		
	|vsphere					|Esxi						 |ubuntu|
	|vsphere					|Esxi						 |centos|

	※ 위와 다른 옵션을 지정하고 싶은 경우 Bosh source에서 필요한 부분을 수정하거나 개발 한다.


## 4.2.  로컬의 OS 이미지를 사용한 스템셀 생성 

로컬의 OS 이미지를 사용해서 스템셀을 생성하는 절차를 기술한다.


1.  기본 OS 이미지를 생성 또는 다운로드 받는다.

	|OS 명            |URL|
	|----------------|----------------------------------------------------------------------------|
	|ubuntu           |[https://s3.amazonaws.com/bosh-os-images/bosh-ubuntu-trusty-os-image.tgz](https://s3.amazonaws.com/bosh-os-images/bosh-ubuntu-trusty-os-image.tgz)|
	|centos           |[https://s3.amazonaws.com/bosh-os-images/bosh-centos-7-os-image.tgz](https://s3.amazonaws.com/bosh-os-images/bosh-centos-7-os-image.tgz)|
	|사용자 생성 OS     |[3. 기본 OS 이미지 생성 참조](#3--기본-os-이미지-생성)|



2.  기본 OS 이미지를 다운 받은 경우, 스템셀 생성 VM에 업로드 한다.


3.  build\_with\_local\_os\_image 실행
  
		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh -c '
		cd /bosh
		bundle exec rake STEMCELL_BUILD_NUMBER=<stemcell version> stemcell:build_with_local_os_image[aws,xen,ubuntu,trusty,go,/tmp/ubuntu_base_image.tgz]
		' remote

	※ STEMCELL\_BUILD\_NUMBER을 생략할 경우, 생성되는 스템셀의 버전은 0000으로 고정된다.


4.  입력 옵션 정보

	|옵션명                      |필수    |설명                                   |예시|
	|--------------------------|------|--------------------------------------|------------------------------|
	|Infrastructure             |O      |인프라 타입                             |Aws|
	|Hypervisor                 |O      |하이퍼 바이저 타입                       |Xen|
	|Operating system name      |O      |OS 타입                                |Ubuntu|
	|Operating system version   |O      |OS 버전                                |Trusty|
	|Agent type                 |X      |에이전트 타입                           |Go|
	|Local os image path        |O      |스템셀 생성 VM에 있는 OS 이미지 경로      |/tmp/ubuntu_base_image.tgz|


	※ Agent type타입이 필수 항목은 아니지만 현재 go 타입 이외의 에이전트는 지원하지 않으므로 go를 입력한다


5.  설정 가능한 옵션 구성

	|Infrastructure             |Hypervisor                |OS|
	|--------------------------|-------------------------|----------------------------|
	|aws                        |Xen                       |ubuntu|
    |aws                        |Xen                       |centos|
	|openstack                  |Kvm						|ubuntu|                       
	|openstack                  |Kvm						|centos|		
	|vcloud					   |Esxi						 |ubuntu|		
	|vsphere					|Esxi						 |ubuntu|
	|vsphere					|Esxi						 |centos|


## 4.3.  생성한 스템셀의 보관장소 

1.  생성한 스템셀 확인

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh remote
		$ ll /bosh/tmp


2.  생성한 스템셀 다운로드

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant scp remote:/bosh/tmp/<생성한 스템셀명> <다운받을 로컬 경로>


#5.  BOSH Light 스템셀 생성 

## 5.1.  Bosh Light 스템셀 생성

Bosh light 스템셀은 AWS (N. Virgina region 한정)에서만 사용가능한 경량 스템셀이다. 스템셀을 AWS에 AMI로 등록하고 등록한 이미지 아이디, 스템셀 정보 등을 기록한 파일을 생성하여 tgz로 압축한다.

1.  다운로드 받았거나 생성한 스템셀을 스템셀 생성 VM에 업로드한다.

		$ cd ~/workspace/bosh/bosh-stemcell	
		$ scp <업로드 대상 스템셀> remote:/tmp/bosh-stemcell.tgz


2.  build\_light 실행

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh -c '
		cd /bosh
		export BOSH_AWS_ACCESS_KEY_ID=<YOUR-AWS-ACCESS-KEY>
		export BOSH_AWS_SECRET_ACCESS_KEY=<YOUR-AWS-SECRET-KEY>
		bundle exec rake stemcell:build_light[/tmp/bosh-stemcell.tgz,hvm]
		' remote


3.  입력 옵션 정보

	|옵션명                |필수   |설명                   |예시|
	|---------------------|------|----------------------|------------------------|
	|Local stemcell path   |O      |로컬의 stemcell 경로   |/tmp/bosh-stemcell.tgz|
	|Virtualization type   |X      |가상화 타입            |Hvm|

	※ 필수 항목이 아닌 곳에 대해서는 ‘’을 입력한다.


#6.  스템셀 커스터마이징 

## 6.1.  스템셀 생성 소스 수정 

사용자의 요구사항에 맞는 스템셀을 생성하기 위해서는 스템셀 생성 소스를 수정 해야 할 경우가 있다. 스템셀 생성을 구성하는 대부분의 파일은 아래의 디렉토리에 있다.

	bosh/stemcell_builder/stages/<스템셀 생성 stage>/<폴더 또는 파일>

1.  수정한 내용을 스템셀 생성 VM에 적용한다.

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant provision remote


2.  스템셀을 생성한다. (필요한 경우 기본 OS 이미지부터 생성한다.)


3.  스템셀 생성 중 오류가 발생한 경우, 해당 오류를 조치한 후 오류가 발생한 stage부터 진행 할 수 있다. 이 경우, resume\_from=<스템셀 생성 stage\>를 생성 명령어에 추가한다. ※ 단, resume 옵션으로
    스템셀을 생성할 경우, 이전에 정상적으로 진행된 스테이지에서 오류가 발생하는 경우도 있다. 이런 경우, resume\_from옵션을 사용하지 않는다.

		$ cd ~/workspace/bosh/bosh-stemcell
		$ vagrant ssh -c '
		cd /bosh
		bundle exec rake stemcell:build_with_local_os_image[aws,xen,ubuntu,trusty,go,/tmp/ubuntu_base_image.tgz] resume_from=stemcell_openstack
		' remote