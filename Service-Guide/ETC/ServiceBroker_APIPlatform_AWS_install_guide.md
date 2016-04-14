## Table of Contents
1. [문서 개요](#1-문서-개요)
	-	1.1. [목적](#11-목적)
	-	1.2. [범위](#12-범위)
	-	1.3. [시스템 구성도](#13-시스템-구성도)
	-	1.4. [참고자료](#14-참고자료)
2. [API 플랫폼 서비스팩 설치](#2-API 플랫폼-서비스팩-설치)
	-	2.1. [설치전 준비사항](#21-설치전-준비사항)
	-	2.2. [API 플랫폼 서비스 릴리즈 업로드](#22-API 플랫폼-서비스-릴리즈-업로드)
	-	2.3. [API 플랫폼 서비스 Deployment 파일 수정 및 배포](#23-API-플랫폼-서비스-deployment-파일-수정-및-배포)
	-	2.4. [API 매니저에서 API 생성 및 배포](#24-API-매니저에서-API-생성-및-베포)
	-   2.5. [API 플랫폼 서비스 브로커 등록](#25-API-플랫폼-서비스-브로커-등록)
3. [API 플랫폼 연동 Sample Web App 설명](#3-API 플랫폼-연동-sample-web-app-설명)
	-	3.1. [Sample Web App에 서비스 바인드 신청 및 App 확인](#31-sample-web-app에-서비스-바인드-신청-및-app-확인)

# 1. 문서 개요
### 1.1. 목적




### 1.2. 범위




### 1.3. 시스템 구성도

![IMAGE 1.3.01]

| 구분 | Resource Pool | Instance type/스펙 |
|--------|-------|-------|
| apiplatform-broker | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| apimanager | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| bam | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |
| mariadb | small | m1.small (1vCPU / 1.7GB RAM / 160GB Disk) |

### 1.4. 참고자료
[**http://bosh.io/docs**](http://bosh.io/docs)  
[**http://docs.cloudfoundry.org/**](http://docs.cloudfoundry.org/)
[**https://docs.wso2.com/display/AM180/Quick+Start+Guide**](https://docs.wso2.com/display/AM180/Quick+Start+Guide/)


# 2. API 플랫폼 서비스팩 설치
### 2.1. 설치전 준비사항



### 2.2. API 플랫폼 서비스 릴리즈 업로드




### 2.3. API 플랫폼 서비스 릴리즈 Deployment 파일 수정 및 배포




### 2.4. API 매니저에서 API 생성 및 배포




##### 2.4.1. 터널링 설정
API 플랫폼 서비스팩으로 배포한 API 매니저에는 Public IP가 할당되지 않기 때문에 사용자가 웹 브라우저를 통해 API 매니저에 접속할 수 있도록 터널링 설정이 필요하다. 터널링은 다양한 방법을 사용할 수 있지만, 본 문서에서는 SSH 클라이언트인 Putty와 웹 브라우저 Firefox를 이용한 터널링 방법에 대해서 안내한다.

###### 1. Putty 설정
(1)다운로드한 Putty를 실행하고 Connection 메뉴를 열어 SSH 메뉴에서 Tunnels를 메뉴를 연다.


(2) 터널링 정보를 입력한다.<br>
① Source port에 사용하지 않는 임의의 포트를 입력하고 ②번 Dynamic을 선택한 후, ③ Add 버튼을 눌러 추가한다. ④번 위치에서 D{입력한 포트}의 형태로 추가된 것을 확인한다.


(3) Putty 접속 정보를 입력한다.<br>
① Session 메뉴를 클릭하여 접속정보 설정 화면으로 이동하여 ② 배포한 API 매니저와 내부망으로 연결되어 있는 머신(설치 환경에 따라 상이함)의 Public IP를 입력한다.


(4) Putty 접속 및 로그인<br>
Open 버튼을 클릭하여 해당 머신에 연결한다


로그인 화면에서 로그인을 완료한 채로 접속을 유지해둔다.



###### 2. Firefox 설정
(1) Mozila Firefox 브라우저를 실행하여 첫 화면 하단의 옵션 버튼을 클릭한다.


(2) 설정 창을 연다.<br>
① 고급 탭-  ② 네트워크 탭- ③ 설정 버튼을 차례로 클릭하여 설정 창을 연다.


(3) 연결 설정 정보를 입력하고 저장한다.<br>
① 프록시 수동설정에 체크하고 ② 화면과 같이 설정을 입력한다. 포트는 Putty 터널링 설정에서 입력한 Source Port와 동일한 포트를 입력한다. ③ 설정을 완료하였으면 확인 버튼을 눌러 저장한다.


###### 3. 터널링 설정 확인
터널링 설정이 되어있는 Putty 접속을 유지한 채로, Mozila Firefox 브라우저를 이용하여 API 매니저 관리자 화면에 접속해본다. 하단의 화면이 확인된다면 API 매니저 배포 및 터널링 설정이 정상적으로 이루어진 것이다.


##### 2.4.2. API 생성 및 배포
터널링 설정이 완료 되었다면, API 플랫폼 서비스팩을 통해 배포된 API 매니저에 접속하여 API를 생성하고 배포(Publish)한다.
```
{API매니저 URL}:9443/carbon
예) https://10.0.0.201:9443/carbon

```
