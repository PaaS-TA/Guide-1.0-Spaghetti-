
## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 참고자료](#4)
2. [API 서비스 선정](#5)
     * [2.1. 데이터 포털 회원가입 및 로그인](#6)
     * [2.2. API 검색](#7)
3. [API 서비스 브로커 개요](#8)
     * [3.1. 개요](#9)
     * [3.2. 서비스 브로커 APIs](#10)
     * [3.3. API 서비스 브로커 동작 구조](#11)
4. [API 서비스 브로커 구현](#12)
     * [4.1. API 서비스 브로커 설정 파일](#13)
         * [4.1.1. 공통 설정 값](#14)
         * [4.1.2. 서비스 설정 값](#15)
         * [4.1.3. 플랜 설정 값](#16)
     * [4.2. 카탈로그](#17)
         * [4.2.1. 요청](#18)
         * [4.2.2. 응답](#19)
     * [4.3. 프로비전](#20)
         * [4.3.1. 요청](#21)
         * [4.3.2. 응답](#22)
     * [4.4. 업데이트](#23)
         * [4.4.1. 요청](#24)
         * [4.4.2. 응답](#25)
     * [4.5. 바인드](#26)
         * [4.5.1. 요청](#27)
         * [4.5.2. 응답](#28)
     * [4.6. 언바인드](#29)
         * [4.6.1. 응답](#30)
         * [4.6.2. 요청](#31)
     * [4.7. 디프로비전](#32)
         * [4.7.1. 요청](#33)
         * [4.7.2. 응답](#34)
5. [API 서비스 브로커 배포](#35)
     * [5.1. 개방형 클라우드 플랫폼 로그인](#36)
     * [5.2. 서비스 브로커 생](#37)
         * [5.2.1. 서비스 브로커 생성](#38)
         * [5.2.2. 서비스 브로커 생성 확인](#39)
         * [5.2.3. 카탈로그(Catalog) 확인](#40)
6. [API 서비스 추가/제거](#41)
     * [6.1. 공공 API 서비스 브로커 설정파일 정의](#42)
     * [6.2. 네이버 API 서비스 브로커 설정파일 정의](#43)
7. [API 서비스 브로커 검증](#44)
     * [7.1. 샘플 어플리케이션 개요](#45)
     * [7.2. 사용 API 서비스](#46)
         * [7.2.1. 사용 API 서비스](#47)
         * [7.2.2. 네이버 Open API 서비스](#48)
     * [7.3. API 서비스키 획득](#49)
         * [7.3.1. 공공 데이터 포털 API 서비스 키 획득](#50)
         * [7.3.2. 네이버 Open API 서비스 키 획득](#51)
     * [7.4. 샘플 어플리케이션 배포](#52)
         * [7.4.1. 개방형 클라우드 플랫폼 로그인](#53)
         * [7.4.2. 어플리케이션 배포](#54)
     * [7.5. 서비스 인스턴스 생성](#55)
         * [7.5.1. 서비스 인스턴스 생성](#56)
         * [7.5.2. 서비스 인스턴스 생성 확인](#57)
     * [7.6. 서비스 바인드](#58)
         * [7.6.1. 서비스 바인드](#59)
         * [7.6.2. 서비스 바인드  확인](#60)
     * [7.7. 샘플 어플리케이션 동작 확](#61)

     


<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
개방형 클라우드 플랫폼(OpenPaas)에 배포되는 어플리케이션은 서비스 브로커를 통해 외부에서 제공하는 서비스들을 사용할 수 있게 된다. 본 문서는 외부 API 서비스들을 개방형 클라우드 플랫폼에서 사용할 수 있도록 서비스 브로커를 구현하고 검증한다. 이를 통해 플랫폼 운영자는 개발자에게 필요한 API 서비스를 개방형 클라우드 플랫폼의 마켓 플레이스에 등록할 수 있고, 이 과정에 대한 이해를 돕는 것이 본 문서의 목적이다.

<div id='3'></div>
### 1.2. 범위 
플랫폼 운영자는 개발자들이 사용하게 될 서비스를 개방형 클라우드 플랫폼의 마켓 플레이스에 노출시킨다. 따라서 본 문서는 API 서비스 브로커의 구현과 배포, API 서비스를 추가하는 방법을 기술한다.(2장~6장) 또한, 어플리케이션에서 API 서비스를 사용하는 방법을 안내하는데(7장), 이는 플랫폼 운영자가 아닌 어플리케이션 개발자의 영역이지만, 검증을 위해 필요하므로 함께 기술한다.
본 문서의 [4장 API 서비스 브로커 구현]을 이해하기 위해서 [OpenPaaS_PaaSTA_ServicePack_develope_guide.docx]문서의 [2장 Service Broker API 개발가이드]를 숙지하여야 하며, 본 문서는 그 중 JAVA 방식 구현에 대해서만 기술하였다.

<div id='4'></div>
### 1.3. 참고자료
- OpenPaaS_PaaSTA_ServicePack_develope_guide.docx
- CF document
- 인천문화예술정보 공공 Open API 센터(**<http://iq.ifac.or.kr/openAPI/look/culture_guide.php>**)
- 네이버 개발자 센터(**<http://developer.naver.com/wiki/pages/Tutorial_JavaScript>**)

<div id='5'></div>
#   2. API 서비스 선정
개방형 클라우드 플랫폼 운영자는 플랫폼 사용자(개발자)들에게 제공할 API를 선정하여, 서비스 브로커를 통해 플랫폼에서 제공한다. 서비스 브로커를 통한 API 서비스의 제공은 플랫폼 운영자의 권한이 필요하기 때문에, 개발자들은 필요한 API 서비스를 제공해줄 것을 운영자에게 요청할 수 있다. 서비스 브로커는 서비스를 제공/소개하는 포털에 따라서 구현방식에 차이가 있을 수 있기 때문에 각각의 API 포털 별로 별도로 구현한다. 예를 들면, 공공 데이터 포털(https://www.data.go.kr/) API 서비스는 공공 데이터 포털 API 서비스를 구현하여 제공하고 네이버(http://www.naver.com/) API 서비스는 네이버 API 서비스 브로커를 구현하여 제공한다.

※ 공공 API 서비스는 각각의 공공 기관에서 서비스를 제공하고 이러한 API 서비스를 데이터 포털에서 통합하여 소개하는 형태로 일반에 공개된다. 대표적인 데이터 포털로는 공공 데이터 포털(https://www.data.go.kr/), 서울 열린 데이터 광장(http://data.seoul.go.kr/) 등이 있다.

※ 본 문서는 공공 데이터 포털(https://www.data.go.kr/)을 기준으로 안내를 기술한다. 각각의 포털이나 API 서비스에 따라 세부적인 내용은 차이가 있을 수 있다.

<div id='6'></div>
### 2.1. 데이터 포털 회원가입 및 로그인
※ 대부분의 데이터 포털은 서비스키 발급을 로그인 된 사용자에게만 허용한다. 서비스키 발급은 플랫폼 운영자가 아닌 개발자의 역할이지만 본 문서의 7장(API 서비스 브로커 검증)을 진행하기 위해서는 서비스키를 발급받아야 하기 때문에 회원가입 및 로그인 절차를 안내한다. 

공공데이터 포털의 API를 사용하기 위해서는 반드시 회원가입이 되어 있어야 한다. 공공데이터포털(https://www.data.go.kr)에 접속하여 상단 [회원가입] 버튼을 눌러 회원가입을 진행한다.

![2-1-0-0]

이동한 회원가입 화면에서 일반회원 또는 기관회원을 선택하여 [가입하기] 버튼을 누른다.

![2-1-0-1]

이름과 이메일 주소를 입력하고 [가입확인] 버튼을 누른다.

![2-1-0-2]

가입여부를 확인하는 절차를 거치고 입력된 정보로 가입된 회원이 없으면 하단에 다음과 같은 화면이 생긴다.

![2-1-0-3]
![2-1-0-4]

'공공데이터포털 이용약관'과 '개인정보 수집 및 이용에 대한 안내'가 소개된다. 내용에 동의한다면 각각의 약관에 동의함을 표시하고 [동의] 버튼을 누른다.

![2-1-0-5]
![2-1-0-6]

가입자 기본정보와 가입자 연락정보를 입력한다. 
(1) 가입자 연락정보에서 이메일 인증을 시도한다. [이메일 인증]버튼을 누르면 인증메일이 발송되고 (2)인증번호 입력 필드가 생긴다. 입력한 이메일로 도착한 인증메일의 인증번호를 입력하고 [확인]버튼을 눌러 인증을 마친다. (3) [완료] 버튼을 눌러 가입완료 화면으로 이동한다.

![2-1-0-7]

회원 가입이 완료되었다.

![2-1-0-8]

공공데이터 포털 메인 화면에서 상단의 [로그인]버튼을 눌러 로그인 한다.

![2-1-0-9]

아이디와 비밀번호를 입력하고 로그인 한다.

<div id='7'></div>
### 2.2. API 검색
API 서비스를 검색하기 위해 데이터 포털에 접속한다. 데이터 포털의 Open API 서비스를 확인한다.

![2-2-0-0]

Open API 카테고리로 이동한다.

![2-2-0-1]

Oepn API 카테고리로 이동하면, ①API 서비스 명으로 검색하거나 ②각종 필터로 분류된 목록을 확인할 수 있다.

![2-2-0-2]

① 전국 단위의 문화행사 관련 정보를 얻기 위해 공연전시정보조회서비스를 검색한다
② 검색 결과에서 API 서비스의 상세정보를 확인하기 위해 API 서비스 목록에서 해당 API를 클릭한다.

![2-2-0-3]

이동한 화면에서 상세정보버튼을 클릭한다.

![2-2-0-4]

창이 확장되면서 API 서비스에 대한 정보를 확인할 수 있다. 
① 해당 API 서비스에 대한 가이드 문서를 다운로드 받을 수 있다.
② 해당 API 서비스가 제공하는 오퍼레이션 별로 요청주소(Endpoint), 요청/응답 필드, 허용 트래픽 등의 정보를 확인할 수 있다. 여기서 제공하는 정보만으로도 API 서비스 브로커에 API 서비스를 추가하고 사용하는데 문제는 없지만, API 서비스에 따라서 가이드 문서를 확인하거나 연관 링크로 이동해야 하는 경우도 있다. 서비스 브로커에 API 서비스를 추가하기 위해 필요한 정보는 다음과 같다.

| 포털 URL      | 해당 서비스를 제공/소개하는 포털의 URL |
|-------------|-----------------------------|
| 서비스 제공자    | 해당 서비스를 실제로 제공하는 기관명이나 URL을 넣는다. |
| 가이드 문서 URL | 플랫폼 사용자(개발자)가 해당 서비스의 가이드 문서를 확인할 수 있는 URL. 플랫폼 사용자는 이 URL의 가이드 문서에서 오퍼레이션이나 요청 파라미터 등을 확인하여 해당 서비스를 사용할 수 있다. |
| 요청주소(Endpoint)   | API 서비스를 사용하기 위한 URL. 서비스 브로커에서는 Endpoint라는 용어를 사용한다. |
| 허용 트래픽    | 서비스 제공자가 허락하는 요청 횟수와 그 단위. 서비스에 따라 일/1,000회, 월/100,000회 등으로 허용 횟수와 단위가 다르다. |

<div id='8'></div>
#   3. API 서비스 브로커 개요
<div id='9'></div>
### 3.1. 개요
서비스 브로커는 개방형 클라우드 플랫폼과 플랫폼 외부의 서비스를 연결하는 역할을 한다. 서비스 브로커는 플랫폼 운영자가 직접 개발하거나 플랫폼 내에 서비스를 제공하고자 하는 외부 서비스의 제공자(provider)가 개발하여 플랫폼 운영자에게 제공할 수 있다. 서비스 브로커 개발은 카탈로그(Catalog), 프로비전(Provision), 업데이트(Update), 바인드(Bind), 언바인드(Unbind), 디프로비전(Deprovision)의 6개 API를 구현함으로써 이루어진다. 서비스 브로커의 개발은 제공하고자 하는 서비스의 특성 또는 서비스 정책에 따라 상이하므로 개발 이전에 제공하고자 하는 서비스에 대한 이해가 바탕이 되어야 한다. 본 문서에서 안내를 제공하는 2개의 API 서비스 브로커는 API 서비스의 특성에 맞게 설계되었다.

<div id='10'></div>
### 3.2. 서비스 브로커 APIs
- 카탈로그(Catalog): 서비스의 목록을 생성한다.
- 프로비전(Provision): 서비스 인스턴스를 생성한다.
- 업데이트(Update): 카탈로그를 업데이트 한다.
- 바인드(Bind): 서비스와 어플리케이션을 바인드한다.
- 언바인드(Unbind): 서비스와 어플리케이션의 바인드를 해제한다.
- 디프로비전(Deprovision): 서비스 인스턴스를 삭제한다.

| <b>서비스 브로커 APIs</b>      | <b>관련 명령어</b> |
|-------------|-----------------------------|
| 카탈로그(Catalog)    | cf create-service-broker [서비스 브로커명] [username] [password] [서비스 브로커 URL] <br>확인: cf service-access |
| 프로비전(Provision) | cf create-service [서비스명] [플랜명] [서비스 인스턴스명] <br>확인: cf services |
| 업데이트(Update)   | cf update-service-broker [서비스 브로커명] [username] [password] [서비스 브로커 URL] <br>확인: cf service-access |
| 바인드(Bind)    | cf bind-service [어플리케이션명] [서비스 인스턴스명] <br>확인: cf env [어플리케이션명] |
| 언바인드(Unbind)    | cf unbind-service [어플리케이션명] [서비스 인스턴스명] <br>확인: cf env |
| 디프로비전(Deprovision)    | cf delete-service [서비스 인스턴스명] <br>확인: cf services |
※ 서비스 브로커 APIs에 대한 상세 정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5 개발가이드]를 참고한다.

<div id='11'></div>
### 3.3. API 서비스 브로커 동작구조
![3-3-0-0]

API 서비스 브로커는 API 포털에 소개된 API 서비스 중, 사용하고자 하는 서비스에 대한 정보를 설정파일에 담고 있는다. 플랫폼 운영자 또는 플랫폼 사용자가 개방형 클라우드 플랫폼에 명령어를 입력하여 API 요청을 서비스 브로커로 보내면, API 서비스 브로커는 설정 파일에 정의된 정보를 이용하여 플랫폼이 요구하는 형태로 응답을 보낸다. 이를 바탕으로 플랫폼은 각각의 명령어에 대한 동작을 실행하게 된다. 서비스 브로커의 구현 플랫폼과 통신하는 6개의 API를 구현함으로써 이루어진다.

<div id='12'></div>
#   4. API 서비스 브로커 구현
<div id='13'></div>
### 4.1. API 서비스 브로커 설정 파일
<div id='14'></div>
##### 4.1.1 공통 설정 값
API 서비스 브로커를 통해 서비스되는 서비스들이 공통적으로 갖게 되는 값을 설정파일에 정의하였다.

| <b>키(Key) 값</b>      | <b>설명</b> | <b>Value 값 예시</b> |
|-------------|-----------------------------|-----------------------------|
| DashboardUrl    | 대시보드 URL. 포털의 URL로 이해할 수 있으며, API를 제공하는 포털에 따라 서비스 브로커를 구분하기 때문에 하나의 서비스 브로커의 서비스들은 공통의 대시보드 URL을 갖는다. | http://www.data.go.kr |
| SupportUrl | 개방형 클라우드 플랫폼의 공식 사이트 주소를 입력한다. | http://www.openpaas.org |

<div id='15'></div>
##### 4.1.2 서비스 설정 값
각각의 서비스마다 별도로 가지는 값을 설정파일에 정의하였다. 키 값에 서비스 번호를 붙여 서로 다른 서비스들을 구분한다. 서비스를 추가함에 따라 서비스 번호를 늘려갈 수 있으며, 서비스 번호는 1번부터 순서대로 부여되어야 한다.

| <b>키(Key) 값</b>      | <b>설명</b> | <b>Value 값 예시</b> |
|-------------|-----------------------------|-----------------------------|
| Service1.Name | 서비스명. 임의로 정할 수 있으나 반드시 고유의(Unique)값이어야 한다. 다른 서비스 브로커에서도 같은 서비스 명을 사용할 수 없다. | PublicPerformance |
| Service1.Description | API 서비스에 대한 간략한 설명을 입력한다. 설정파일에 정의해 놓지 않은 경우에는 "no service description"이라고 입력된다. 개방형 클라우드 플랫폼의 서비스 마켓플레이스에서 사용자에게 노출된다. | Performances, exhibits information display |
| Service1.Provider | API 서비스의 제공 기관의 이름 또는 URL이다. | http://www.culture.go.kr |
| Service1.DocumentUrl | API 서비스에 대한 기술문서, 가이드문서 등을 확인할 수 있는 URL이다. | https://www.data.go.kr/subMain.jsp#/L3B1YnIvd....(생략) |
| Service1.Endpoint | API 서비스의 서비스 URL/URI이다. | http://www.culture.go.kr/openapi/rest/publicperformancedisplays |

<div id='16'></div>
##### 4.1.3 플랜 설정 값
각각의 서비스에 대해서 최소한 한 개 이상의 플랜을 설정파일에 정의해 주어야 한다. 플랜이 정의되어 있지 않으면 개방형 클라우드 플랫폼에서 해당 서비스를 사용할 수 없다. 플랜의 키 값은 서비스번호와 플랜번호를 포함하는데, 예를 들어 키 값이 [Service1.Plan1.Name]이라면 1번 서비스의 첫 번째 플랜의 명칭이라는 의미이다. 플랜번호는 1번부터 순서대로 부여되어야 한다.

| <b>키(Key) 값</b>      | <b>설명</b> | <b>Value 값 예시</b> |
|-------------|-----------------------------|-----------------------------|
| Service1.Plan1.Name | 플랜명. 플랜명은 서비스만 다르다면 고유의(Unique)값일 필요가 없다. | Basic |
| Service1.Plan1.Description | 플랜에 대한 간략한 설명을 입력한다. 설정파일에 정의해 놓지 않은 경우에는 "no plan description"이라고 입력된다. | total 1,000,000 calls |
| Service1.Plan1.Bullet | 플랜의 과금 정보. API 서비스이기 때문에 최대 허용 호출 수를 입력한다. 복수 입력을 하려면 코드의 수정이 필요하다. | 1,000,000 callsr |
| Service1.Plan1.Unit | 최대 허용 호출 수의 단위를 입력한다. 예를 들면, per month, per day, weekly, total등으로 입력할 수 있다. | Total |

<div id='17'></div>
### 4.2. 카탈로그
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.1. Catalog API 가이드]를 참고한다.
<div id='18'></div>
##### 4.2.1 요청
- Route
  ```
  GET /v2/catalog
  ```
  
- cURL
  ```
  curl -H "X-Broker-API-Version: 2.5" http://username:password@broker-url/v2/catalog
  ```
  ※ 'username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.
  
<div id='19'></div>
##### 4.2.2 응답
※{1}은 코드 내에서 설정파일에 정의된 서비스와 플랜의 키(Key) 값을 순서대로 불러오기 위한 변수 값이다.
<br>※ Key값의[ ](대괄호)내의 문자는 설정파일에 정의된 서비스의 키(Key) 값을 의미한다.

- body

  | <b>응답필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | services* | 각각의 서비스 객체를 담은 객체의 리스트 | |
  | &nbsp;&nbsp;id* | 서비스 ID. 고유(Unique)해야 하며, 설정파일에서 읽어 온 값과 지정된 텍스트의 조합으로 생성됨. <br>형태: "Service"+{1}+[Service1.Name]+"ServiceID" | |
  | &nbsp;&nbsp;name* | 서비스명. 설정파일에서 읽어 온 값. <br>Key값: [Service1.Name] | PublicPerformance |
  | &nbsp;&nbsp;description* | 서비스 설명. 설정파일에서 읽어 온 값. <br>Key값: [Service1.Name] | Performances, exhibits information display |
  | &nbsp;&nbsp;bindable* | 어플리케이션과 바인드 가능 여부. boolean 타입. <br>지정값: true | true |
  | &nbsp;&nbsp;tags | 서비스의 분류, 속성, 또는 기반 기술을 노출 <br>지정값: "Public API Service" | Public API Service |
  | &nbsp;&nbsp;metadata | 서비스 제공을 위한 메타 데이터의 목록. 상세 설명은 아래 '서비스 메타데이터' 참고 | |
  | &nbsp;&nbsp;requires* | 사용자가 서비스를 제공 하는 권한 목록. 현재는 syslog_drain 권한만 지원함 <br>지정값: "syslog_drain" | syslog_drain |
  | plan_updateable | 서비스의 플랜 변경 지원 여부. boolean 타입. <br>지정값: false | false |
  | &nbsp;&nbsp;plans* | 서비스에 대한 각각의 플랜 객체를 담은 객체의 리스트 | |
  | &nbsp;&nbsp;&nbsp;&nbsp;id* | 플랜 ID. 고유(Unique)해야 하며, 설정파일에서 읽어 온 값과 지정된 텍스트의 조합으로 생성됨. <br>형태: "Service"+{1}+[Service1.Name]+"Plan"+{1}+[Service1.Plan1.Name]+"PlanID" | Service1 PublicPerformance Plan1 basic PlanID |
  | &nbsp;&nbsp;&nbsp;&nbsp;name* | 플랜명. 설정파일에서 읽어 온 값. <br>Key값: [Service1.Plan1.Name] | basic |
  | &nbsp;&nbsp;&nbsp;&nbsp;description* | 플랜 설명. 설정파일에서 읽어 온 값. <br>Key값: [Service1.Plan1.Description] | total 1,000,000 calls |
  | &nbsp;&nbsp;&nbsp;&nbsp;metadata | 서비스의 플랜을 위한 메타 데이터의 목록. 상세 설명은 아래 '플랜 메타데이터' 참고 | |
  | &nbsp;&nbsp;&nbsp;&nbsp;free | 유/무료 과금 정책을 표시.boolean 타입. 기본값은 true. <br>지정값: true | true |

- 서비스 메타데이터

  | <b>응답필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | metadata.displayName | 그래픽 클라이언트에 표시되는 서비스명 <br>Key값: [Service1.Name] | PublicPerformance |
  | metadata.imageUrl | 서비스에 대한 이미지 URL <br>지정값: "no image" | no image |
  | metadata.longDescription | 서비스 상세 설명 <br>Key값: [Service1.Description] | Performances, exhibits information display |
  | metadata.providerDisplayName | 실제 서비스를 제공하는 기관명 <br>Key값: [Service1.Provider] | Performances, exhibits information display |
  | metadata.documentationUrl | 서비스 관련 문서 URL <br>Key값: [Service1.DocumentationUrl] | https://www.data.go.kr/subMain.jsp#/L3B1YnIvdXNlL3ByaS9Jcm9z...(생략) |
  | metadata.supportUrl | 서비스 지원 URL <br>Key값: [SupportUrl] | http://www.openpaas.org |

- 플랜 메타데이터

  | <b>응답필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | metadata.bullets | 플랜의 과금 정보. API 서비스이기 때문에 최대 호출 수를 입력 <br>Key값: [Service1.Plan1.Bullet] | 1,000,000 calls |
  | metadata.costs | 플랜의 비용 정보.Map타입의 amount와 String타입의 unit으로 구성. <br>amount 지정값: "KRW",0 <br>※KRW는 한국 통화단위 <br>unit Key값: [Service1.Plan1.Unit] | Json 구조 <br>"costs": [{"amount": {"KRW": 0} "unit": "total" }] |
  | metadata.displayName | 그래픽 클라이언트에 표시되는 플랜명 <br>Key값: [Service1.Plan1.Name] | basic |

<div id='20'></div>
### 4.3. 프로비전
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.1  Provision 가이드]를 참고한다.
<div id='21'></div>
##### 4.3.1 요청
- Route
  ```
  PUT /v2/service_instances/:instance_id
  ```
  ※ instance_id는 서비스 인스턴스 생성 명령어를 입력 했을 때, 클라우드 컨트롤러에서 생성하는 고유의(Unique) ID이다.
  
- cURL
  ```
  $ curl http://username:password@broker-url/v2/service_instances/:instance_id -d '{
  "service_id":        "Service1 PublicPerformance ServiceID",
  "plan_id":           "Service1 PublicPerformance Plan1 basic PlanID",
  "organization_guid": "[org-guid-here]",
  "space_guid":        "[space-guid-here]"
}' -X PUT -H "X-Broker-API-Version: 2.5" -H "Content-Type: application/json"
  ```
  ※ ''username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.

- body

  | <b>요청필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | service_id* | 카탈로그에서 생성한 서비스ID | Service1 PublicPerformance ServiceID |
  | plan_id* | 카탈로그에서 생성한 플랜ID | 카탈로그에서 생성한 플랜ID |
  | organization_guid* | 프로비전을 요청한 사용자 Org의 GUID 값 | [클라우드 컨트롤러에서 Org 식별을 위해 사용하는 GUID 값] |
  | space_guid* | 프로비전을 요청한 사용자 Space의 GUID 값 | [클라우드 컨트롤러에서 Space 식별을 위해 사용하는 GUID 값] |
  
<div id='22'></div>
##### 4.3.2 응답
| <b>응답필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
|-------------|-----------------------------|-----------------------------|
| dashboard_url | 공공데이터포털 URL을 사용 <br>Key값: [DashboardUrl] | http://www.data.go.kr |

<div id='23'></div>
### 4.4. 업데이트
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.3  Update Instance API 가이드]를 참고한다.
<div id='24'></div>
##### 4.4.1 요청
- Route
  ```
 PATCH /v2/service_instances/:instance_id
  ```
  ※ instance_id는 프로비전에서 생성된 서비스 인스턴스의 고유(Unique)ID
  
- cURL
  ```
  $ curl http://username:password@broker-url/v2/service_instances/:instance_id -d '{
  "plan_id": "Service1 PublicPerformance Plan2 special PlanID"
}' -X PATCH -H "X-Broker-API-Version: 2.4" -H "Content-Type: application/json"
  ```
  ※ 'username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.

- body

  | <b>요청필드</b>      | <b>설명</b> | <b>샘플데이터(공연전시정보 API)</b> |
  |-------------|-----------------------------|-----------------------------|
  | plan_id | 카탈로그에서 생성된, 변경할 플랜의 ID | Service1 PublicPerformance Plan2 special PlanID |
  | service_id* | 카탈로그에서 생성된, 플랜을 변경하고자 하는 서비스의 ID | Service1 PublicPerformance ServiceID |

<div id='25'></div>
##### 4.4.2 응답
| <b>응답필드</b>      | <b>설명</b> |
|-------------|-----------------------------|
| {} | 업데이트가 성공적으로 진행되었을 경우, "{}"의 형태로 응답된다. |

<div id='26'></div>
### 4.5. 바인드
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.5  Bind API 가이드]를 참고한다.
<div id='27'></div>
##### 4.5.1 요청
- Route
  ```
  PUT /v2/service_instances/:instance_id/service_bindings/:binding_id
  ```
  
- cURL
  ```
  $ curl http://username:password@broker-url/v2/service_instances/
:instance_id/service_bindings/:binding_id -d '{
  "plan_id":       "Service1 PublicPerformance Plan1 basic PlanID",
  "service_id":     "Service1 PublicPerformance ServiceID",
  "app_guid":       "app-guid-here"
}' -X PUT -H "X-Broker-API-Version: 2.5" -H "Content-Type: application/json"
  ```
  ※ 'username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.

- body

  | <b>요청필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | plan_id | 카탈로그에서 생성된, 바인드하는 서비스 인스턴스의 플랜ID | Service1 PublicPerformance Plan1 basic PlanID |
  | service_id | 카탈로그에서 생성된, 바인드하는 서비스 인스턴스의 서비스ID | Service1 PublicPerformance ServiceID |
  | app_guid | 바인드하는 어플리케이션의 GUID | 바인드하는 어플리케이션의 GUID |

<div id='28'></div>
##### 4.5.2 응답
- body

  | <b>응답필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | credentials | Application이 서비스에 접근할수 있는 credentials 정보. 해시 형태로 제공. 자세한 정보는 'Credentials'를 참고 | |
  | syslog_drain_url | 개방형 클라우드 플랫폼에 bound 된 Application에 대한 로그 URL | |
  
- Credentials

  | <b>Credential</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | url | 설정파일에 정의된 API 서비스의 엔드포인트 <br>Key값: [Service1.Endpoint] | http://www.culture.go.kr/openapi/rest/publicperformancedisplays |
  | serviceKey | API 서비스를 사용하기 위해 서비스 제공자로부터 발급받은 인증키 <br>※ 서비스 바인드 시, 입력 | [사용자가 발급받은 키값] |
  | documentUrl | API 서비스의 기술문서, 개발 가이드 등을 확인할 수 있는 URL <br>Key값: [Service1.DocumentationUrl] | https://www.data.go.kr/subMain.jsp#/L3B1YnIvdXNlL3ByaS... (생략) |
  
<div id='29'></div>
### 4.6. 언바인드
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.6  Unbind API 가이드]를 참고한다.
<div id='30'></div>
##### 4.6.1 요청
- Route
  ```
  DELETE /v2/service_instances/:instance_id/service_bindings/:binding_id
  ```
  
- cURL
  ```
  $ curl 'http://username:password@broker-url/v2/service_instances/:instance_id/
service_bindings/:binding_id?service_id=Service1 PublicPerformance ServiceID &plan_id=Service1 PublicPerformance Plan1 basic PlanID' -X DELETE -H "X-Broker-API-Version: 2.4"
  ```
  ※ 'username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.

- body

  | <b>요청필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | service_id | 카탈로그에서 생성된, 언바인드하는 서비스 인스턴스의 서비스ID | Service1 PublicPerformance ServiceID |
  | plan_id | 카탈로그에서 생성된, 언바인드하는 서비스 인스턴스의 플랜ID | Service1 PublicPerformance Plan1 basic PlanID |

<div id='31'></div>
##### 4.6.2 응답
- body

  | <b>응답</b>      | <b>설명</b> |
  |-------------|-----------------------------|
  | {} | 언바인드가 성공적으로 진행되었을 경우, "{}"의 형태로 응답된다. |

<div id='32'></div>
### 4.6. 디프로비전
※ 세부정보는 [OpenPaaS_PaaSTA_ServicePack_develope_guide]문서의 [2.5.4  Deprovision API 가이드]를 참고한다.
<div id='33'></div>
##### 4.7.1 요청
- Route
  ```
  DELETE /v2/service_instances/:instance_id
  ```
  
- cURL
  ```
  $ curl 'http://username:password@broker-url/v2/service_instances/:instance_id?service_id=
Service1 PublicPerformance ServiceID plan_id=Service1 PublicPerformance Plan1 basic PlanID -X DELETE -H "X-Broker-API-Version: 2.5"
  ```
  ※ 'username:password'는 서비스 브로커의 인증ID와 인증Password를 의미한다. 서비스 브로커 구현 시, 라이브러리에 정의된 값이다. 정의되어 있는 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다.

- body

  | <b>요청필드</b>      | <b>설명</b> | <b>샘플데이터</b> |
  |-------------|-----------------------------|-----------------------------|
  | service_id | 카탈로그에서 생성된, 디프로비전하는 서비스 인스턴스의 서비스ID | Service1 PublicPerformance ServiceID |
  | plan_id | 카탈로그에서 생성된, 디프로비전하는 서비스 인스턴스의 플랜ID | Service1 PublicPerformance Plan1 basic PlanID |

<div id='34'></div>
##### 4.7.2 응답
- body

  | <b>응답</b>      | <b>설명</b> |
  |-------------|-----------------------------|
  | {} | 모든 응답은 Body는 JSON Object "{}" 형식으로 한다. |

<div id='35'></div>
#   5. API 서비스 브로커 배포
개방형 클라우드 플랫폼에서 사용하기 위해서 서비스 브로커를 구동한다. 서비스 브로커는 하나의 어플리케이션으로 개방형 클라우드 플랫폼 내에 어플리케이션 형태로 구동하여 사용할 수 있지만, 본 문서는 외부 서버에서 구동하여 개방형 클라우드 플랫폼에서 사용하는 방법을 안내한다. 서비스 브로커가 구동되는 외부 서버는 개방형 클라우드 플랫폼과 통신이 가능한 환경을 구성하여야 한다. 서비스 브로커 구동에 대한 안내는 따로 진행하지 않는다.

<div id='36'></div>
### 5.1. 개방형 클라우드 플랫폼 로그인
개방형 클라우드 플랫폼에서 서비스 브로커를 생성한다. 서비스 브로커 생성을 위해서는 개방형 클라우드 플랫폼의 관리자 권한이 필요하다. 하단의 명령어를 통해 개방형 클라우드 플랫폼에 관리자 계정으로 로그인 한다.
```
  $ cf login
```

예시)
```
  $ cf login
  Email> admin
  Password> 123456
```

<div id='37'></div>
### 5.2. 서비스 브로커 생성
개방형 클라우드 플랫폼에서 서비스 브로커를 생성한다. 아래 명령어와 같은 서비스 브로커 생성 명령어를 입력했을 때, 카탈로그(Catalog)가 진행되면서, 설정 파일의 서비스 정보를 기반으로 서비스를 정의한다.

<div id='38'></div>
##### 5.2.1 서비스 브로커 생성
```
  $ cf create-service-broker [서비스 브로커 명] [인증ID] [인증Password] [서비스 브로커 주소]
```
※[서비스 브로커 명]은 개방형 클라우드 플랫폼 내에서 서비스 브로커를 지칭하는 명칭으로 사용자가 임의로 결정한다. [인증ID]와 [인증Password]는 서비스 브로커 구현 시, 라이브러리에 정의되어 있는 값이다. 제공되는 샘플에 정의된 인증ID는 'admin', 인증Password는 'cluoudfoundry'이다. [서비스 브로커 주소]는 서비스 브로커가 구동되어 있는 주소를 의미한다.

예시)
```
  $ cf create-service-broker public-api-sb admin cloudfoundry http://10.30.60.100:8080
```

<div id='39'></div>
##### 5.2.2 서비스 브로커 생성 확인
서비스 브로커의 목록을 확인하는 명령어를 이용하여 서비스 브로커가 정상적으로 생성된 것을 확인할 수 있다.
```
  $ cf service-brokers
```

![5-2-2-0]

<div id='40'></div>
##### 5.2.3 카탈로그(Catalog) 확인
서비스 브로커가 개방형 클라우드 플랫폼에 생성될 때, 서비스 브로커를 통해 서비스되는 API 서비스의 목록을 생성하는 카탈로그를 진행한다. 카탈로그 된 서비스들은 서비스 접근(service access) 목록에 추가되는데, 이 서비스 접근 목록을 확인함으로써, 정상적으로 카탈로그가 이루어졌음을 확인할 수 있다.
```
  $ cf service-access
```

![5-2-3-0]

<div id='41'></div>
### 5.3. 서비스 접근 허용
[5.2.3. 카탈로그(Catalog)확인]의 서비스 접근목록 확인을 보면 카탈로그 된 서비스들은 접근(access)이 none으로 설정되어 있다. 이 설정을 접근 가능하게 바꿔 주어야 마켓 플레이스에 서비스 목록이 노출된다. 아래 명령어를 이용하여 서비스에 대한 접근을 허용한다.

```
  $ cf enable-service-access [서비스명] -p [플랜명]
```
※ [서비스명]과 [플랜명]은 서비스 브로커 설정 파일에 정의된 서비스 명과 플랜 명을 의미한다. cf service-access 명령어를 통해, 서비스 접근 허용을 할 수 있는 서비스의 서비스명과 플랜명 목록을 확인할 수 있다.

예시)
```
  $ cf enalbe-service-access PublicPerformance -p basic
```

위의 명령어를 이용하여 마켓 플레이스에 노출하고자 하는 서비스들의 접근을 허용한 뒤, 하단의 명령어를 이용하여 서비스 접근 목록을 다시 확인하면, 서비스들의 접근(access)설정이 all로 바뀐 것을 확인할 수 있다.
```
  $ cf service-access
```

![5-3-0-0]

<div id='42'></div>
### 5.4. 마켓 플레이스 확인
접근을 허용한 서비스들은 마켓 플레이스에 등록됨으로써 개발자들이 사용할 수 있는 상태가 된다. 명령어를 이용해 마켓 플레이스를 확인한다.

```
  $ cf marketplace
```

![5-4-0-0]

<div id='43'></div>
#   6. API 서비스 추가/제거
샘플 서비스 브로커에서 API 서비스의 추가 또는 제거는 [4.1 서비스 브로커 설정 파일]에 정보를 삽입하거나 삭제함으로써 이루어진다. 샘플 서비스 브로커는 설정 파일의 키(key)값을 통해, 필요한 정보를 읽어 서비스 카탈로그(catalog)와 바인드(bind)에서 사용하도록 설계되어있다. 설정 파일의 파일명은 'application-mvc.properties'이고 샘플 서비스 브로커에서 요구하는 API 서비스에 대한 정보는 다음과 같다.
※ 샘플 서비스 브로커는 서비스나 플랜을 서비스 번호, 플랜 번호를 순서대로 읽도록 설계되었다.
서비스 번호, 플랜번호가 1번부터 순서대로 입력되지 않으면, 정상적으로 서비스나 플랜을 읽어 올 수 없다. 서비스나 플랜을 추가 또는 제거 한 이후에 서비스 번호나 플랜 번호가 1번부터 빠짐없이 이어질 수 있도록 주의한다.

| <b>설정 파일 키(key) 값</b>      | <b>설명</b> |
|-------------|-----------------------------|
| Service[서비스 번호].Name | API 서비스의 이름을 영문으로 표기 |
| Service[서비스 번호].Description | API 서비스에 대한 설명. 마켓 플레이스에 등록되었을 때, 플랫폼 사용자(개발자)들에게 노출됨 |
| Service[서비스 번호].Provider | API 서비스를 제공하는 기관명이나 URL |
| Service[서비스 번호].DocumentUrl | API 서비스의 기술문서나 명세를 확인할 수 있는 URL |
| Service[서비스 번호].EndPoint | API를 요청하는 URL |
| Service[서비스 번호].Plan[플랜번호].Name | 해당 서비스의 플랜명. <br>서비스에 따라 복수의 플랜을 가질 수 있기 때문에 플랜번호를 부여하지만, 일반적으로 API 서비스는 단일 플랜을 갖는다. |
| Service[서비스 번호].Plan[플랜번호]..Description | 해당 서비스의 해당 플랜에 대한 설명 |
| Service[서비스 번호].Plan[플랜번호].Bullet | 해당 서비스의 해당 플랜의 서비스 제한 형태. 일반적으로 API 서비스는 호출 수에 제한을 두어 서비스를 관리한다. |
| Service[서비스 번호].Plan[플랜번호].Unit | 해당 서비스의 해당 플랜의 제한 단위. 일반적으로 API 서비스는 일별 또는 월별 호출 수에 제한을 둔다. |

<div id='44'></div>
### 6.1. 공공 API 서비스 브로커 설정파일 정의
제공되는 2개의 샘플 서비스 브로커 중 공공 API 서비스 브로커의 서비스 목록은 다음과 같이 정의 되어있다.

| <b>설정 파일 키(key) 값</b>      | <b>Value 값</b> |
|-------------|-----------------------------|
| 서비스 1 - 공연전시 정보 조회 서비스 |
| Service1.Name | PublicPerformance |
| Service1.Description | Performances, exhibits information display |
| Service1.Provider | http://www.culture.go.kr |
| Service1.DocumentUrl | https://www.data.go.kr/subMain.jsp#/L3B1YnIvdXNlL3ByaS9Jcm9zT3BlbkFwaURldGFpbC9vcGVuQXBpTGlzdFBhZ2UkQF4wMTJtMSRAXnB1YmxpY0RhdGFQaz0xNTAwMDEyMCRAXmJybUNkPU9DMDAwOCRAXnJlcXVlc3RDb3VudD0zNDQkQF5vcmdJbmRleD1PUEVOQVBJ |
| Service1.EndPoint | http://www.culture.go.kr/openapi/rest/publicperformancedisplays |
| Service1.Plan1.Name | Basic |
| Service1.Plan1.Description | total 1,000,000 calls |
| Service1.Plan1.Bullet | 1,000,000 calls |
| Service1.Plan1.Unit | Total |
| 서비스 2 - 인천광역시 문화행사 |
| Service2.Name | IncheonCulture |
| Service2.Description | Culture performances information held in Incheon City |
| Service2.Provider | http://iq.ifac.or.kr/ |
| Service2.DocumentUrl | http://iq.ifac.or.kr/openAPI/look/ |
| Service2.EndPoint | http://iq.ifac.or.kr/openAPI/real/search.php |
| Service2.Plan1.Name | Basic |
| Service2.Plan1.Description | 5,000 calls per day |
| Service2.Plan1.Bullet | 5,000 calls |
| Service2.Plan1.Unit | per day |
| 서비스 3 - 대전광역시 문화축제 |
| Service3.Name | DaejeonFestival |
| Service3.Description | Culture Festival information held in Daejeon City |
| Service3.Provider | http://data.daejeon.go.kr/ |
| Service3.DocumentUrl | https://www.data.go.kr/subMain.jsp#/L3B1YnIvcG90L215cC9Jcm9zTXlQYWdlL29wZW5EZXZHdWlkZVBhZ2UkQF4wMTJtMSRAXnB1YmxpY0RhdGFQaz0xNTAwNjk2OSRAXnB1YmxpY0RhdGFEZXRhaWxQaz11ZGRpOjJkZjQ4YzY3LWIxYjAtNGMyOS04YTc3LWQyMmQ2Mjc3MTJmZCRAXm9wcnRpblNlcU5vPTgyNTYkQF5tYWluRmxhZz10cnVl |
| Service3.EndPoint | http://data.daejeon.go.kr/openapi-data/service/rest/festival/festivalDaejeonService/festivalDaejeon |
| Service3.Plan1.Name | Basic |
| Service3.Plan1.Description | 1,000 calls per day |
| Service3.Plan1.Bullet |1,000 calls |
| Service3.Plan1.Unit | per day |
| 서비스 4 - 전시공연/테마파크 정보 |
| Service4.Name | JeonnamPerformanceList |
| Service4.Description | Performances, exhibits information held in Jeonnam |
| Service4.Provider | http://api.namdokorea.com/ |
| Service4.DocumentUrl | http://api.namdokorea.com/main |
| Service4.EndPoint | http://api.namdokorea.com/openapi/performance/list |
| Service4.Plan1.Name | Basic |
| Service4.Plan1.Description | 5,000 calls per day |
| Service4.Plan1.Bullet | 5,000 calls |
| Service4.Plan1.Unit | per day |

<div id='45'></div>
### 6.2. 네이버 API 서비스 브로커 설정파일 정의
제공되는 2개의 샘플 서비스 브로커 중, 네이버 API 서비스 브로커의 서비스 목록은 다음과 같이 정의 되어있다.

| <b>설정 파일 키(key) 값</b>      | <b>Value 값</b> |
|-------------|-----------------------------|
| 서비스 1 - 네이버 지도 서비스 |
| Service1.Name | NaverMap |
| Service1.Description | Naver map API service |
| Service1.Provider | http://www.naver.com |
| Service1.DocumentUrl | http://developer.naver.com/wiki/pages/JavaScript |
| Service1.EndPoint | http://openapi.map.naver.com/openapi/naverMap.naver |
| Service1.Plan1.Name | basic |
| Service1.Plan1.Description | 1,000,000 calls per day |
| Service1.Plan1.Bullet | 1,000,000 calls |
| Service1.Plan1.Unit | per day |
| 서비스 2 - 네이버 주소-좌표 변환 서비스 |
| Service2.Name | NaverAddressToGPS |
| Service2.Description | Convert address into gps coordinate |
| Service2.Provider | http://www.naver.com |
| Service2.DocumentUrl | http://developer.naver.com/wiki/pages/JavaScript#section-JavaScript-_EC_9A_94_EC_B2_ADURLRequestUrl |
| Service2.EndPoint | http://openapi.map.naver.com/api/geocode |
| Service2.Plan1.Name | basic |
| Service2.Plan1.Description | 1,000,000 calls |
| Service2.Plan1.Bullet | 1,000,000 calls |
| Service2.Plan1.Unit | per day |
| 서비스 3 - 네이버 검색 서비스 |
| Service3.Name | NaverSearch |
| Service3.Description | Naver search API(Blog, News, Movie, Local, etc.) |
| Service3.Provider | http://www.naver.com |
| Service3.DocumentUrl | http://developer.naver.com/wiki/pages/SrchAPI |
| Service3.EndPoint | http://openapi.naver.com/search |
| Service3.Plan1.Name | basic |
| Service3.Plan1.Description | 25,000 calls |
| Service3.Plan1.Bullet | 25,000 calls |
| Service3.Plan1.Unit | per day |



[2-1-0-0]:/images/openpaas-service/publicapi/2-1-0-0.png
[2-1-0-1]:/images/openpaas-service/publicapi/2-1-0-1.png
[2-1-0-2]:/images/openpaas-service/publicapi/2-1-0-2.png
[2-1-0-3]:/images/openpaas-service/publicapi/2-1-0-3.png
[2-1-0-4]:/images/openpaas-service/publicapi/2-1-0-4.png
[2-1-0-5]:/images/openpaas-service/publicapi/2-1-0-5.png
[2-1-0-6]:/images/openpaas-service/publicapi/2-1-0-6.png
[2-1-0-7]:/images/openpaas-service/publicapi/2-1-0-7.png
[2-1-0-8]:/images/openpaas-service/publicapi/2-1-0-8.png
[2-1-0-9]:/images/openpaas-service/publicapi/2-1-0-9.png
[2-2-0-0]:/images/openpaas-service/publicapi/2-2-0-0.png
[2-2-0-1]:/images/openpaas-service/publicapi/2-2-0-1.png
[2-2-0-2]:/images/openpaas-service/publicapi/2-2-0-2.png
[2-2-0-3]:/images/openpaas-service/publicapi/2-2-0-3.png
[2-2-0-4]:/images/openpaas-service/publicapi/2-2-0-4.png
[3-3-0-0]:/images/openpaas-service/publicapi/3-3-0-0.png
[5-2-2-0]:/images/openpaas-service/publicapi/5-2-2-0.png
[5-2-3-0]:/images/openpaas-service/publicapi/5-2-3-0.png
[5-3-0-0]:/images/openpaas-service/publicapi/5-3-0-0.png
[5-4-0-0]:/images/openpaas-service/publicapi/5-4-0-0.png


