## Table of Contents
1. [문서 개요](#문서-개요)
     * [1.1. 목적](#목적)
     * [1.2. 범위](#범위)
     * [1.3. 시스템 구성도](#시스템-구성도)
     * [1.4. 참고자료](#참고자료)
2. [MySQL 서비스팩 설치](#MySQL-서비스팩-설치)
     * [2.1. 설치전 준비사항](#설치전-준비사항)
     * [2.2. MySQL 서비스 릴리즈 업로드](#MySQL-서비스-릴리즈-업로드)
     * [2.3. MySQL 서비스 Deployment 파일 수정 및 배포](#MySQL-서비스-Deployment-파일-수정-및-배포)
     * [2.4. MySQL 서비스 브로커 등록](#MySQL-서비스-브로커-등록)
3. [MySQL 연동 Sample Web App 설명](#MySQL-연동-Sample-Web-App-설명)
     * [3.1. Sample Web App 구조](#Sample-Web-App-구조)
     * [3.2. 개방형 클라우드 플랫폼에서 서비스 신청](#개방형-클라우드-플랫폼에서-서비스-신청)
     * [3.3. Sample Web App에 서비스 바인드 신청 및 App 확인](#Sample-Web-App에-서비스-바인드-신청-및-App-확인)
4. [MySQL Client 툴 접속](#MySQL-Client-툴-접속)
     * [4.1. HeidiSQL 설치&연결](#HeidiSQL-설치&연결)


# 문서 개요

### 목적
      
본 문서(MySQL 서비스팩 설치 가이드)는 전자정부표준프레임워크 기반의 Open PaaS에서 제공되는 서비스팩인 MySQL 서비스팩을 Bosh를 이용하여 설치 하는 방법과 Open PaaS의 SaaS 형태로 제공하는 Application 에서 MySQL 서비스를 사용하는 방법을 기술하였다.


### 범위 

설치 범위는 MySQL 서비스팩을 검증하기 위한 기본 설치를 기준으로 작성하였다. 

###시스템 구성도
본 문서의 설치된 시스템 구성도이다. MySQL Server, MySQL 서비스 브로커, Proxy로 최소사항을 구성하였다.
![시스템-구성도](http://github.com/OpenPaaSRnD/Documents/blob/master/images/openpass-service/mysql/mysql_bosh_lite/mysql_bosh_lite_1.3.01.png)

### 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)


# 2.	MySQL 서비스팩 설치

### 설치전 준비사항 
본 설치 가이드는 Linux 환경에서 설치하는 것을 기준으로 하였다.
서비스팩 설치를 위해서는 먼저 BOSH-lite 가 설치 되어 있어야 하고 BOSH 에 로그인 및 타켓 설정이 되어 있어야 한다.
BOSH-lite 가 설치 되어 있지 않을 경우 먼저 BOSH-lite 설치 가이드 문서를 참고 하여BOSH-lite를 설치 해야 한다.
OpenPaaS 에서 제공하는 압축된 릴리즈 파일들을 다운받는다. (OpenPaaS-Deployment.zip, OpenPaaS-Sample-Apps.zip, OpenPaaS-Services.zip)

### MySQL 서비스 릴리즈 업로드
-   OpenPaaS-Services.zip파일 압축을 풀고 폴더안에 있는 MySQL 서비스 릴리즈 openpaas-mysql-release-beta-1.0.tgz 파일을복사한다.
업로드할 openpaas-mysql-release-beta-1.0.tgz 파일을 확인한다.  

	$ls –all	
![mysql_bosh_lite_2.2.01](http://github.com/OpenPaaSRnD/Documents/blob/master/images/openpass-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.01.png)

	
-	업로드 되어 있는 릴리즈 목록을 확인한다.  

	$bosh releases
![mysql_bosh_lite_2.2.02](http://github.com/OpenPaaSRnD/Documents/blob/master/images/openpass-service/mysql/mysql_bosh_lite/mysql_bosh_lite_2.2.02.png)
Mysql 서비스 릴리즈가 업로드 되어 있지 않은 것을 확인








### OpenPaaS 설치패키지 다운로드 

OpenPaaS 설치패키지를 다운받아 다운로드한 디렉토리에 압축해제 한다.

1.  **OpenPaaS 설치패키지를 다음 사이트에서 다운로드 한다.**

  
		http://openpaas.org
  


2.  **패키지 압축해제**

  
  		# 패키지의 확장자가 tar.gz인 경우
  		$ tar -xvzf <다운로드 디렉토리>/<사이트에서 내려받은 OpenPaaS 설치패키지>.tar.gz

  		# 패키지의 확장자가 zip 인 경우
  		$ cd <다운로드 디렉토리>
  		$ unzip <다운로드 디렉토리>/<사이트에서 내려받은 OpenPaaS 설치패키지>.zip


### RUBY 설치
    
Ruby 설치 절차는 다음과 같다.

1.  **Ruby 설치에 필요한 Dependency들을 설치한다.**


	- **온라인인 경우 ( 권장 )**


			$ sudo apt-get update

			$ sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev  
			sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev


	- 오프라인인 경우
	
			#다음의 패키지를 수동으로 설치한다.
	  		$ sudo dpkg -i <다운로드 디렉토리>/OpenPaaS-Env/bosh-lite-ruby-dependency/<패키지 파일명>