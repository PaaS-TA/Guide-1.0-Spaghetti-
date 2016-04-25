
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
     * [5.2. 서비스 브로커 생성](#37)
         * [5.2.1. 서비스 브로커 생성](#38)
         * [5.2.2. 서비스 브로커 생성 확인](#39)
         * [5.2.3. 카탈로그(Catalog) 확인](#40)
     * [5.3. 서비스 접근 허용](#41)
     * [5.4. 마켓 플레이스 확인](#42)
6. [API 서비스 추가/제거](#43)
     * [6.1. 공공 API 서비스 브로커 설정파일 정의](#44)
     * [6.2. 네이버 API 서비스 브로커 설정파일 정의](#45)
7. [API 서비스 브로커 검증](#46)
     * [7.1. 샘플 어플리케이션 개요](#47)
     * [7.2. 사용 API 서비스](#48)
         * [7.2.1. 사용 API 서비스](#49)
         * [7.2.2. 네이버 Open API 서비스](#50)
     * [7.3. API 서비스키 획득](#51)
         * [7.3.1. 공공 데이터 포털 API 서비스 키 획득](#52)
         * [7.3.2. 네이버 Open API 서비스 키 획득](#53)
     * [7.4. 샘플 어플리케이션 배포](#54)
         * [7.4.1. 개방형 클라우드 플랫폼 로그인](#55)
         * [7.4.2. 어플리케이션 배포](#56)
     * [7.5. 서비스 인스턴스 생성](#57)
         * [7.5.1. 서비스 인스턴스 생성](#58)
         * [7.5.2. 서비스 인스턴스 생성 확인](#59)
     * [7.6. 서비스 바인드](#60)
         * [7.6.1. 서비스 바인드](#61)
         * [7.6.2. 서비스 바인드  확인](#62)
     * [7.7. 샘플 어플리케이션 동작 확](#63)


<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적
개방형 클라우드 플랫폼(OpenPaas)에 배포되는 어플리케이션은 서비스 브로커를 통해 외부에서 제공하는 서비스들을 사용할 수 있게 된다. 본 문서는 외부 API 서비스들을 개방형 클라우드 플랫폼에서 사용할 수 있도록 서비스 브로커를 구현하고 검증한다. 이를 통해 플랫폼 운영자는 개발자에게 필요한 API 서비스를 개방형 클라우드 플랫폼의 마켓 플레이스에 등록할 수 있고, 이 과정에 대한 이해를 돕는 것이 본 문서의 목적이다.

<div id='3'></div>
### 1.2. 범위 
플랫폼 운영자는 개발자들이 사용하게 될 서비스를 개방형 클라우드 플랫폼의 마켓 플레이스에 노출시킨다. 따라서 본 문서는 API 서비스 브로커의 구현과 배포, API 서비스를 추가하는 방법을 기술한다.(2장~6장) 또한, 어플리케이션에서 API 서비스를 사용하는 방법을 안내하는데(7장), 이는 플랫폼 운영자가 아닌 어플리케이션 개발자의 영역이지만, 검증을 위해 필요하므로 함께 기술한다.
본 문서의 [4장 API 서비스 브로커 구현]을 이해하기 위해서 [서비스팩 개발 가이드 (./Development-Guide/ServicePack_develope_guide.md)]문서의 [2장 Service Broker API 개발가이드]를 숙지하여야 하며, 본 문서는 그 중 JAVA 방식 구현에 대해서만 기술하였다.

<div id='4'></div>
### 1.3. 참고자료
- 서비스팩 개발 가이드 (./Development-Guide/ServicePack_develope_guide.md)
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

<div id='46'></div>
#   7. API 서비스 브로커 검증
※ 플랫폼 사용자(개발자)가 API 서비스를 어플리케이션에 바인드하여 사용할 수 있는지 확인한다. 이를 위해 API 서비스를 사용하는 샘플 어플리케이션을 제작해 정상 작동 여부를 검증한다.
<br>※ 검증에 필요한 API 서비스들을 사용하기 위해 공공 데이터 포털 서비스 브로커와 네이버 Open API 서비스 브로커의 구현이 완료된 상태에서 검증을 진행한다.


<div id='47'></div>
### 7.1. 샘플 어플리케이션 개요
전국 각지의 지방자치단체별로 제공하는 지역 문화행사 API 서비스를 이용하여 행사 장소를 지도에 표시한다. 네이버 지도 API를 통해 지도를 화면에 출력하고 하단에 셀렉트 박스를 만들어 사용자가 지역을 선택할 수 있도록 한다. 선택된 지역에 따라서 해당 지역에서 제공하는 문화행사 API를 사용하여 행사 위치를 지도에 마커로 표시한다.

<div id='48'></div>
### 7.2. 사용 API 서비스
<div id='49'></div>
##### 7.2.1. 공공 데이터 포털 API 서비스
##### 7.2.1.1 공연전시정보조회 서비스
  1. API 소개<br>
  전국 단위의 공연전시 정보를 제공하는 서비스이다. 문화포털(http://www.culture.go.kr)에서 서비스하고 있으며, 지역별, 기간별, 분야별 검색이 가능하다. 서비스 키는 공공 데이터 포털을 통해 발급한다. 서비스키 신청을 하면, 승인까지 1~2일 가량의 대기기간을 가져야 한다.<br>
  ※ 요청 변수의 샘플 데이터 컬럼은 샘플 어플리케이션에서 사용한 값을 명시한다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://www.culture.go.kr/openapi/rest/publicperformancedisplays |
  | 오퍼레이션 | /area |

  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | serviceKey | 999 | 페이지당 행사 정보의 개수이다. 한 페이지에 전체를 조회하기 위해 '999'를 입력한다. |
  | (sido) | (서울) | 모든 지역을 조회할 경우 변수를 넣지 않고, 서울의 공연전시 정보만 조회할 때, 값을 '서울'로 넣는다. |

  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | title | 행사의 제목이다. |
  | place | 행사가 진행되는 장소이다. |
  | startDate | 행사가 시작되는 날짜이다. |
  | endDate | 행사가 종료되는 날짜이다. |
  | thumbnail | 행사 포스터 이미지의 URL이다. |
  | gpsX | GPS X좌표이다. |
  | gpsY | GPS Y좌표이다. |

##### 7.2.1.2 인천광역시 문화행사
  1. API 소개<br>
  인천문화예술정보 사이트에 등록되어있는 인천 내의 행사정보를 제공하는 서비스이다. 인천문화포털(http://iq.ifac.or.kr)에서 서비스 하고 있다. 공공 데이터 포털에 소개되어 있지만, 서비스키 발급은 인천문화예술정보 공공 Open API 센터(http://iq.ifac.or.kr/openAPI)에서 직접 신청해야 한다. 서비스키는 자동 승인되므로 승인을 기다릴 필요가 없다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://iq.ifac.or.kr/openAPI/real/search.php |
  
  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | apiKey | [서비스키] | 서비스 인증 키이다. 사용자가 직접 발급받아야 하므로 샘플 데이터를 넣지 않는다. |
  | pSize | 999 | 페이지당 행사 정보의 개수이다. 최대의 데이터를  조회하기 위해 최대값인 '999'를 입력한다. |
  | resultType | xml | json과 xml중 응답결과의 출력방식을 지정할 수 있다. |
  | svID | culture | 문화행사정보를 조회하기 위한 서비스 ID이다. |
  | srh_periodType | p | 행사정보를 기간별로 조회하기 위한 값이다. 누락할 경우 현재 실시되지 않는 행사도 함께 조회된다. |
  | srh_sDate | culture | 'srh_periodType' 변수를 입력할 경우 검색 기준이 되는 날짜이다. 샘플 어플리케이션에서는 요청 일을 기준으로 조회하기 때문에 당일의 날짜를 'yyyyMMdd'의 형태로 넣는다. |

  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | title | 행사의 제목이다. |
  | place | 행사가 진행되는 장소이다. 좌표 값을 응답하지 않기 때문에, 네이버 검색 API를 사용하여 장소명을 검색해 좌표 값을 얻는다 |
  | startDate | 행사가 시작되는 날짜이다. |
  | endDate | 행사가 종료되는 날짜이다. |
  | thumbnail | 행사 포스터 이미지의 URL이다. |

##### 7.2.1.3 대전광역시 문화행사
  1. API 소개<br>
  대전광역시 공공정보 Open API Service(http://data.daejeon.go.kr)에서 제공하는 대전광역시 문화축제 API 서비스이다. 대전광역시 내의 문화 축제정보를 제공하며, 서비스키 발급은 공공 데이터 포탈과 대전광역시 공공정보 Open API Service 양쪽에서 모두 가능하다. 서비스키를 신청하면 승인과 발급에 2~3일 가량이 소요된다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://data.daejeon.go.kr/openapi-data/service/rest/festival/festivalDaejeonService/festivalDaejeon |
  
  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | serviceKey | [서비스키] | 서비스 인증 키이다. 사용자가 직접 발급받아야 하므로 샘플 데이터를 넣지 않는다. |
  | numOfRows | 999 | 페이지당 행사 정보의 개수이다. 한 페이지에 전체를 조회하기 위해 '999'를 입력한다. |
  
  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | title | 행사의 제목이다. |
  | place | 행사가 진행되는 장소이다. 좌표 값을 응답하지 않기 때문에, 네이버 검색 API를 사용하여 장소명을 검색해 좌표 값을 얻는다 |
  | startDate | 행사가 시작되는 날짜이다. |
  | endDate | 행사가 종료되는 날짜이다. |
  | thumbnail | 행사 포스터 이미지의 URL이다. |

##### 7.2.1.4 전시공연/테마파크 정보
  1. API 소개<br>
  전남 스마트 관광 문화 Open API(http://api.namdokorea.com)에서 제공하는 API 서비스이다. 전라남도 도내의 전시공연 및 테마파크 정보를 제공하지만, 전시공연정보는 드물고 사실상 관광지 정보를 제공하는 API 서비스에 가깝다. 서비스키 발급 신청은 공공 데이터 포털과 전라남도 공공 데이터 커뮤니티 센터 (http://data.jeonnam.go.kr)에서 가능하며, 승인 및 발급에 약 1~2일의 기간이 소요된다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://api.namdokorea.com/openapi/performance/list |
  
  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | key | [서비스키] | 서비스 인증 키이다. 사용자가 직접 발급받아야 하므로 샘플 데이터를 넣지 않는다. |
  | pageSize | 999 | 페이지당 행사 정보의 개수이다. 한 페이지에 전체를 조회하기 위해 '999'를 입력한다. |
  | format | xml | json과 xml중 응답결과의 출력방식을 지정할 수 있다. |
  
  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | NAME | 관광지명이다. |
  | NEWADDR | 관광지의 주소이다. 좌표 값을 응답하지 않기 때문에, 네이버 주소-좌표 변환 API를 사용하여 해당 주소에 대한 좌표 값을 얻는다 |
  | MASTERIMG | 관광지의 이미지 URL이다. |

<div id='50'></div>
##### 7.2.1. 네이버 Open API 서비스
##### 7.2.1.1 네이버 지도
  1. API 소개<br>
  네이버 개발자센터 Open API(http://developer.naver.com/wiki/pages/OpenAPI)에서 제공하는 지도 API이다. 샘플 어플리케이션에서는 자바스크립트를 이용하여 네이버 지도를 사용한다. 이를 위해서 HTML파일에 다음과 같은 태그를 작성하게 된다.
  
  ```
   <script type="text/javascript" src="[엔드포인트]?ver=2.0&key=[서비스키]"></script>
  ```
  어플리케이션에서는 다음과 같은 형태로 입력하여 사용하게 된다.
  ```
   <script type="text/javascript" src="http://openapi.map.naver.com/openapi/naverMap.naver?ver=2.0&key=f32441ebcd3cc9de474f80s1wf1e54e3"></script>
  ```
  자자세한 사용법은 아래의 URL을 참조한다. 
  네이버 개발자센터 튜토리얼: http://developer.naver.com/wiki/pages/Tutorial_JavaScript

##### 7.2.2.2 네이버 주소-좌표 변환
  1. API 소개<br>
  네이버 개발자센터 Open API(http://developer.naver.com/wiki/pages/OpenAPI)에서 제공하는 주소-좌표 변환 API이다. 요청 변수로 주소를 입력하면, 해당 주소에 대한 좌표 값을 응답한다. 주소가 정확하지 않은 경우 값을 응답하지 않거나 잘못된 값을 응답한다. 서비스키는 네이버 지도 API와 같은 키를 사용하기 때문에 서비스키 당 호출횟수가 함께 집계된다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://openapi.map.naver.com/api/geocode |
  
  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | key | [서비스키] | 서비스 인증 키이다. 사용자가 직접 발급받아야 하므로 샘플 데이터를 넣지 않는다. 네이버 지도 API와 같은 키 값을 사용한다. |
  | query | [주소] | 좌표로 변환하고자 하는 주소를 입력한다. |
  
  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | x | 검색을 요청한 주소에 대한 GPS X좌표 값이다. |
  | y | 검색을 요청한 주소에 대한 GPS Y좌표 값이다. |

##### 7.2.2.3 네이버 검색
  1. API 소개<br>
  네이버 개발자센터 Open API(http://developer.naver.com/wiki/pages/OpenAPI)에서 제공하는 지역 검색 API이다. 네이버 검색 API는 블로그, 뉴스, 책, 지역 등의 검색 API를 제공한다. 이 중 어떤 검색을 사용할지는 요청변수의 taget 값을 통해 결정한다. 네이버 지역검색은 검색어에 대한 주소, 좌표 등의 정보를 응답하는데 여기서 응답되는 좌표 값은 네이버 주소-좌표 변환 API에서 사용하는 경위도 좌표계(WGS84)가 아닌 카텍좌표계를 사용하므로 주의가 필요하다.
  
  2. 명세<br>
  
  | 메소드 | GET |
  |-------------|-----------------------------|
  | 엔드포인트 | http://openapi.naver.com/search |
  
  3. 요청<br>
  
  | <br>변수명 | <br>샘플데이터 | <br>설명 |
  |-------------|-----------------------------|-----------------------------|
  | key | [서비스키] | 서비스 인증 키이다. 사용자가 직접 발급받아야 하므로 샘플 데이터를 넣지 않는다 |
  | query | [주소] | 검색하고자 하는 장소의 명칭을 입력한다. |
  | target | local | 네이버 검색 API 중 지역 검색을 사용하기 위해서 입력한다. |
  | start | 1 | 검색의 시작 위치이다. |
  | display | 1 | 검색결과 출력 건수이다. 네이버 검색 API는 정확도가 높은 순서로 여러 건의 검색 결과를 노출하기 첫 번째 결과 값만 이용한다. |
  
  4. 응답<br>
  
  | <br>변수명 | <br>설명 |
  |-------------|-----------------------------|
  | mapx | 검색을 요청한 주소에 대한 X좌표 값이다. 카텍좌표계 값으로 다른 API와는 다른 좌표계를 사용한다. |
  | mapy | 검색을 요청한 주소에 대한 Y좌표 값이다. 카텍좌표계 값으로 다른 API와는 다른 좌표계를 사용한다. |

<div id='51'></div>
### 7.3. API 서비스키 획득
<div id='52'></div>
##### 7.3.1. 공공 데이터 포털 API 서비스
공공 데이터 포털에 소개된 대부분의 API는 공공 데이터 포털에서 서비스키 발급이 가능하지만 일부는 서비스 제공 기관에 직접 서비스키 신청을 해야 한다. 본 문서에서 설명하는 4개의 공공 데이터 포털 API 중, [7.2.1.1. 공연전시정보조회 서비스], [7.2.1.3. 대전광역시 문화축제], [7.2.1.4 전시공연/테마파크 정보]는 공공 데이터 포털에서 서비스키 발급이 가능하다. 반면, [7.2.1.2. 인천광역시 문화행사]의 경우, 인천문화예술정보 공공 Open API 센터(http://iq.ifac.or.kr/openAPI)에서만 서비스키 발급이 가능하기 때문에 공공 데이터 포털의 서비스키 획득 절차와 인천문화예술정보 공공 Open API 센터의 서비스키 획득 절차를 나누어 기술한다.
##### 7.3.1.1 공공 데이터 포털 서비스키 획득 절차
  1. 회원가입 및 로그인<br>
  ※ 공공 데이터 포털 회원가입과 로그인은 본 문서 2장의 [2.1. 데이터 포털 회원가입 및 로그인]을 참고한다.

  2. 서비스키 신청<br>
  ![7-3-1-0]<br>
  로그인을 완료하고 메인 화면의 상단 중앙의 검색 필드에 서비스키를 발급받고자 하는 API를 검색한다.

  ![7-3-1-1]<br>
  검색결과에서 사용하고자 하는 API를 찾아 클릭한다.
  
  ![7-3-1-2]<br>
  (1) [활용신청] 버튼을 누르면 서비스키 신청화면으로 넘어가며 신청절차를 진행하게 되고<br>
  (2) [상세정보] 버튼을 누르면 해당 API서비스에 관한 상세정보 및 각각의 서비스에 대한 기술문서를 확인할 수 있다.

  ![7-3-1-3]<br>
  시스템 유형을 선택한다.
  
  ![7-3-1-4]<br>
  활용정보를 선택한다.
  
  ![7-3-1-5]<br>
  API서비스의 상세 기능 중 어떤 기능을 사용할지 선택한다.
  
  ![7-3-1-6]<br>
  (1) 라이센스 표시에 동의하고 (2) [신청] 버튼을 클릭한다.
  
  ![7-3-1-7]<br>
  서비스키 신청절차가 완료되었음을 알리는 안내메시지를 확인한다.

##### 7.3.1.2 인천 문화예술 정보 공공 Open API 센터 서비스키 획득 절차
  1. 회원가입 및 로그인<br>
  인천문화예술정보 공공 Open API 센터의 API서비스를 사용하기 위해서는 인천 문화재단 (http://www.ifac.or.kr)패밀리 사이트 통합 회원가입이 되어 있어야 한다. 먼저 인천문화예술정보 공공 Open API 센터(http://iq.ifac.or.kr/openAPI)로 접속한다.

  ![7-3-1-8]<br>
  접속 후, 메인화면 우측 상단의 [회원가입]을 클릭한다.
  
  ![7-3-1-9]<br>
  [회원가입] 버튼을 클릭하면, 인천문화재단 패밀리 사이트 통합 회원가입 화면으로 이동한다. (1) 회원가입 여부를 확인하기 위해 아이핀 또는 휴대폰 인증 중 한가지를 선택하고 (2) 인증을 진행한다.

  ![7-3-1-10]<br>
  [회원가입] 버튼을 눌러 회원가입을 진행한다.
  
  ![7-3-1-11]<br>
  ![7-3-1-12]<br>
  ![7-3-1-13]<br>
  약관 동의화면에서 (1)회원가입약관과 (2)개인정보 수집 및 이용에 관한 안내에 동의를 체크하고 (3)동의함 버튼을 누른다.
  
  ![7-3-1-14]<br>
  기본정보와 정보수신여부를 선택하고 [회원가입] 버튼을 누른다. 기본정보는 우편수령지까지 빠짐없이 입력해야 한다.
  
  ![7-3-1-15]<br>
  회원 가입 완료를 확인한다.
  
  ![7-3-1-16]<br>
  인천문화예술정보 공공 Open API 센터(http://iq.ifac.or.kr/openAPI)메인 화면에 다시 접속하여 [로그인]을 클릭하여 로그인 한다.
  
  2. 서비스키 신청<br>
  ![7-3-1-17]<br>
  API둘러보기 화면에서는 사용할 수 있는 API의 목록을 확인할 수 있다. 사용할 API의 우측 [신청하기] 버튼을 눌러 서비스키 신청절차를 진행한다.

  ![7-3-1-18]<br>
  신청서 작성 화면에서 (1) 필수입력필드를 모두 채우고 (2) 약관에 동의한 뒤, (3) [확인] 버튼을 눌러 서비스키 신청절차를 완료한다.
  
  ![7-3-1-19]<br>
  서비스키 신청 등록이 완료되었다.
  
  3. 서비스키 발급 확인<br>
  ![7-3-1-20]<br>
  인천문화예술정보 공공 Open API 센터(http://iq.ifac.or.kr/openAPI)에 로그인 한 상태로 메인 화면에서 우측 중간 [키 발급/관리] 버튼을 클릭한다.

  ![7-3-1-21]<br>
  인증키 관리 화면으로 이동하면 (1) 서비스키, (2) 트래픽, (3) 승인 상태를 확인할 수 있고 (4) 서비스키 삭제도 가능하다.<br>
  ※ [7.2.1.2. 인천광역시 문화행사]의 경우 API서비스 제공자에게 직접 서비스키 신청을 하게 되기 때문에 공공 데이터 포털에서 서비스키 신청을 하는 경우와 달리 대기기간이 없고 승인 역시 자동승인 되므로 신청 절차만 완료하면 바로 사용할 수 있다.

<div id='53'></div>
##### 7.3.2 네이버 Open API 서비스 키 획득
[2.2.1. 네이버 지도], [2.2.2. 네이버 주소-좌표 변환], [2.2.3. 네이버 검색] API 서비스키를 발급받기 위해서는 네이버 ID로 로그인이 되어 있어야 한다. 네이버 개발자센터의 Open API 페이지(http://developer.naver.com/wiki/pages/OpenAPI)에 접속하여 네이버 ID로 로그인하거나 회원가입을 진행한다.<br>

  1. 회원가입 및 로그인<br>
  ![7-3-2-0]<br>
  네이버 개발자센터의 Open API 페이지(http://developer.naver.com/wiki/pages/OpenAPI)에 접속하여 우측 중간 [회원가입] 버튼을 클릭한다.

  ![7-3-2-1]<br>
  ![7-3-2-2]<br>
  ![7-3-2-3]<br>
  ![7-3-2-4]<br>
  약관 동의 화면에서 필수 동의 항목인 네이버 이용약관과 개인정보 수집 및 이용에 대한 안내에 동의를 체크하고 하단의 [동의] 버튼을 클릭한다.
  
  ![7-3-2-5]<br>
  가입화면에서 휴대전화번호를 포함한 입력항목들을 입력하고 (1) [인증] 버튼을 눌러 휴대전화 인증을 실시한다. 휴대전화를 통해 전달된 인증번호를 입력하고 (2) [확인] 버튼을 눌러 인증을 마치고 (3) [가입하기] 버튼을 눌러 회원가입을 완료한다.
  
  ![7-3-2-6]<br>
  네이버 개발자센터의 Open API 페이지(http://developer.naver.com/wiki/pages/OpenAPI)에 접속하여 우측 상단 [로그인] 버튼을 클릭하여 로그인 한다.
  
  2. 서비스키 신청<br>
  ![7-3-2-7]<br>
  로그인이 완료되고 다시 네이버 개발자센터의 Open API 페이지로 이동하면 우측 중간에 [키 발급/관리] 버튼을 확인할 수 있다.

  ![7-3-2-8]<br>
  키 발급/관리 화면에서 네이버에서 제공하는 API 서비스에 대한 키 관리를 할 수 있다.<br>
  (1) [키 추가]버튼을 눌러 [7.2.2.3. 네이버 검색] API의 서비스 키를 발급받을 수 있고 (2) [키 추가] 버튼을 눌러 [7.2.2.1. 네이버 지도], [7.2.2.2. 네이버 주소-좌표 변환] API의 서비스키를 발급받을 수 있다. 하나의 서비스키로 [7.2.2.1. 네이버 지도]와 [7.2.2.2. 네이버 주소-좌표 변환] API 서비스를 공통으로 사용할 수 있다.<br>
	
  (1) 네이버 검색 API 키 발급<br>
  네이버 검색 API의 [키 추가] 버튼을 클릭한다<br>

  ![7-3-2-9]<br>
  ① 서비스를 사용할 도메인 정보를 입력해야 한다. 정확히 입력하지 않으면, API 서비스를 사용할 수 없다. 이때, Identifier 필드에 입력한 값과 개방형 클라우드 플랫폼에서 어플리케이션이 사용하는 도메인이 일치해야 한다. 어플리케이션의 도메인은 개방형 클라우드 플랫폼에 어플리케이션을 배포하는 시점에 결정한다.<br>
  참고: [7.3. 샘플 어플리케이션 배포]<br>
  ② 네이버 검색 API는 키 발급 과정에서도 휴대폰 인증을 실시해야 한다.<br>
  ③ 좌측의 보안문자를 입력한다. 보안문자 입력에서 대문자와 소문자는 구분하지 않는다.<br>
  ④ 약관의 내용을 숙지하고 동의하면, 체크한다.<br>
  ⑤ [키 발급] 버튼을 눌러 키 발급을 완료한다.<br>

  (2) 네이버 지도 API 키 발급<br>
  네이버 지도 API의 [키 추가] 버튼을 클릭한다<br>
  
  ![7-3-2-10]<br>
  ① 사용환경을 선택한다.<br>
  ② 사용환경에서 '웹'을 선택했기 때문에 URL 정보를 입력해야 한다. 정확히 입력하지 않으면, API 서비스를 사용할 수 없다. 이때, URL 필드에 입력한 값과 개방형 클라우드 플랫폼에서 어플리케이션이 사용하는 도메인이 일치해야 한다. 어플리케이션의 도메인은 개방형 클라우드 플랫폼에 어플리케이션을 배포하는 시점에 결정한다.<br>
  참고: [7.4.2. 어플리케이션 배포]<br>
  ③ 좌측의 보안문자를 입력한다. 보안문자 입력에서 대문자와 소문자는 구분하지 않는다.<br>
  ④ 약관의 내용을 숙지하고 동의하면, 체크한다.<br>
  ⑤ [키 발급] 버튼을 눌러 키 발급을 완료한다.<br>

  3. 서비스키 확인<br>
  ![7-3-2-11]<br>
  네이버 개발자센터의 Open API 페이지(http://developer.naver.com/wiki/pages/OpenAPI)에 접속하고 로그인한 상태에서 우측 중간의 [키 발급/관리] 버튼을 클릭한다.

  ![7-3-2-12]<br>
  네이버에서 제공하는 API 서비스의 서비스키 전체를 관리할 수 있는 화면으로 이동한다. 각각의 API별로 ①발급된 사용자의 서비스 키와 등록 URL과 ②API 호출 수를 확인 할 수 있다. ③키 추가 및 ④수정/삭제 또한 이 화면에서 이루어 진다. [7.2.2.3. 네이버 검색] API 서비스는 상단의 검색 API의 발급 키로 사용이 가능하고 [7.2.2.1. 네이버 지도]와 [7.2.2.2. 네이버 주소-좌표 변환] API 서비스는 지도 API의 발급 키로 사용이 가능하다.

<div id='54'></div>
### 7.4. 샘플 어플리케이션 배포
<div id='55'></div>
##### 7.4.1. 개방형 클라우드 플랫폼 로그인
개방형 클라우드 플랫폼에 개발자 계정으로 로그인 한다. 개발자 계정은 운영자가 계정을 생성하고 개발자 권한을 부여함으로써 사용할 수 있게 된다. 개발자 계정 생성 및 권한 부여에 대한 설명은 기술하지 않는다. 본 문서에 기술된 검증절차는 관리자 계정으로 접속하여 검증하여도 무방하다.

```
  $ cf login
```

예시)
```
  $ cf login
  Email> kimdojun
  Password> asd20kwl
```

<div id='56'></div>
##### 7.4.2. 어플리케이션 배포
개방형 클라우드 플랫폼에 어플리케이션을 배포(Deploy)한다. 어플리케이션 배포는 아래와 같은 명령어를 사용한다.

```
  $ cf push [어플리케이션명] -p [파일 경로] -b [빌드팩] -m [메모리] --no-start
```

※ [어플리케이션 명]은 개방형 클라우드 플랫폼에서 사용하는 어플리케이션의 이름으로 임의의 값을 지정할 수 있다. 배포 시 따로 옵션을 주지 않으면 어플리케이션 명을 바탕으로 어플리케이션의 도메인을 결정하게 된다. 네이버 Open API는 서비스키 발급 시, URL을 입력하도록 되어있다. 어플리케이션의 도메인이 서비스키를 발급 받을 때 입력한 URL과 다를 경우, API를 사용할 수 없다.<br>
※ '-p [파일 경로]'는 빌드 된 파일의 경로를 의미한다.<br>
※ '-b [빌드팩]'은 어플리케이션의 빌드팩을 선택할 수 있는 옵션이다. 입력하지 않을 경우 자동으로 찾게 된다. 선택 가능한 빌드팩 목록은 명령어 'cf buildpacks'로 확인 할 수 있다.<br>
※ '-m [메모리]'는 어플리케이션이 점유할 메모리의 크기를 지정하는 옵션이다. <br>
※ 어플리케이션 배포가 되면 자동으로 어플리케이션을 구동하게 되는데, '--no-start' 옵션은 어플리케이션이 자동으로 구동되지 않도록 한다.<br>

예시)
```
  $ cf push public-naver -p /home/kimdojun/sample/openpaas-service-java-broker-public-naver-api-sample.war -b java_buildpack -m 1024M --no-start
```

<div id='57'></div>
### 7.5. 서비스 인스턴스 생성
마켓 플레이스에 등록된 서비스에 대해서 인스턴스 생성이 가능해진다.<br>
※ 서비스를 마켓 플레이스에 등록하는 것은 플랫폼 운영자의 역할이다. 마켓 플레이스에 서비스를 등록하는 방법은 본 문서의 [5.3. 서비스 접근 허용]을 참고한다.
<div id='58'></div>
##### 7.5.1. 서비스 인스턴스 생성
```
  $ cf create-service [서비스명] [플랜명] [서비스 인스턴스명]
```
※ [서비스명]과 [플랜명]은 서비스 브로커 설정 파일에 정의된 서비스명과 플랜명을 의미한다. [서비스 인스턴스명]은 사용자가 임의로 입력하는 값이지만 제공되는 샘플 어플리케이션은 서비스 인스턴스명으로 서비스를 식별하도록 구현되어 있기 때문에, 임의의 값을 사용할 경우, 샘플 어플리케이션에서 서비스를 식별하지 못하므로 샘플 어플리케이션 소스의 수정이 필요하다.<br><br>

※ 샘플 어플리케이션은 개방형 클라우드 플랫폼에서의 서비스 인스턴스명을 아래의 표를 따르도록 강제한다. 서비스명의 첫 글자를 소문자로 바꾼 형태이다. 임의의 서비스 인스턴스명을 사용하기 위해서는 샘플 어플리케이션의 수정이 필요하다.<br>

| <b>API 서비스</b>      | <b>서비스명</b> | <b>서비스 인스턴스명</b> |
|-------------|-----------------------------|-----------------------------|
| 공연전시정보조회 서비스 | PublicPerformance | publicPerformance |
| 인천광역시 문화행사 | IncheonCulture | incheonCulture |
| 대전광역시 문화축제 | DaejeonFestival | daejeonFestival |
| 전시공연/테마파크 정보 | JeonnamPerformanceList | jeonnamPerformanceList |
| 네이버 지도 | NaverMap | naverMap |
| 네이버 주소-좌표 변환 | NaverAddressToGPS | naverAddressToGPS |
| 네이버 검색 | NaverSearch | naverSearch |

예시)
```
  $ cf create-service PublicPerformance basic publicPerformance
```

<div id='59'></div>
##### 7.5.2. 서비스 인스턴스 확인
서비스 인스턴스의 생성을 통해 개방형 클라우드 플랫폼에서 서비스와 어플리케이션의 바인드가 가능해진다. 서비스 인스턴스의 생성을 확인하기 위해서, 서비스 인스턴스 목록을 노출하는 명령어를 사용한다.

```
  $ cf services
```

생성된 서비스의 인스턴스명과 서비스명, 플랜, 바인드 된 어플리케이션을 확인할 수 있다. bound apps의 값이 바인드 된 어플리케이션을 의미한다.<br>
![7-5-2-0]

<div id='60'></div>
### 7.6. 서비스 바인드
[7.5. 서비스 인스턴스 생성]에서 생성된 서비스 인스턴스를 어플리케이션과 바인드하여 어플리케이션에서 API서비스를 사용할 수 있게 된다.
<div id='61'></div>
##### 7.6.1. 서비스 바인드
생성된 서비스 인스턴스와 어플리케이션을 바인드한다. 바인드 시, -c 옵션을 주어 사용자가 직접 발급받은 해당 서비스의 서비스키를 입력할 수 있도록 한다. 샘플 서비스 브로커의 경우는 서비스 키를 입력하지 않을 경우 바인드가 정상적으로 진행되지 않도록 설계되어 있다.

```
  $ cf bind-service [어플리케이션명] [서비스 인스턴스명] -c '{"serviceKey":"[서비스키]"}'
```
※ [서비스 인스턴스명]은 서비스 인스턴스를 생성할 때, 입력한 명칭으로 서비스 명과 다르다. 서비스명과 서비스 인스턴스명의 차이는 [7.5.1. 서비스 인스턴스 생성]을 참고한다.

예시)
```
  $ cf bind-service public-naver publicPerformance -c '{"serviceKey":"adn2d241aaml%%3D"}'
```
위와 같은 형태로 어플리케이션이 사용할 7개의 API 서비스를 모두 바인드한다.

<div id='62'></div>
##### 7.6.2. 서비스 바인드 확인
어플리케이션과 서비스를 바인드하면 바인드 된 각각의 서비스에 대한 정보는 어플리케이션의 VCAP_SERVICES 정보에서 확인할 수 있다.

```
  $ cf env [어플리케이션 명]
```

예시)
```
  $ cf env public-naver
```
![7-6-2-0]<br>
VCAP_SERVICES 정보를 확인하면 그림과 같이 각각의 서비스 별로 플랜, 엔드포인트, 바인드에서 -c 옵션을 통해 입력한 서비스키 등의 서비스 정보를 확인 할 수 있다. 샘플 어플리케이션은 이 VCAP_SERVICES 정보에서 엔드포인트와 서비스키를 가져와 필요한 API 서비스를 사용한다.

<div id='62'></div>
### 7.7. 샘플 어플리케이션 동작 확인
명령어를 통해 어플리케이션의 URL을 확인할 수 있다.

```
  $ cf apps
```

![7-7-0-0]<br>
어플리케이션이 구동 중임을 확인하고 웹 브라우저에서 URL로 접속한다. 샘플 어플리케이션은 메인화면에서 'Hello World'를 출력하도록 구현되어있다.

![7-7-0-1]<br>
[URL]/main으로 이동하여 지도가 화면에 출력되는 것을 확인한다.

![7-7-0-2]<br>
※ 지도가 화면에 보이지 않는 경우는 네이버 지도 서비스키 신청 시 입력한 URL이 어플리케이션 URL과 동일하지 않아서 생기는 경우가 대부분이다. 샘플 어플리케이션을 예로 들면, 네이버 지도 API 서비스키 발급 시 URL 입력 창에 상단 화면에서 확인되는 URL인 'http://public-naver.10.244.0.34.xip.io'를 입력하여야 한다. 자세한 내용은 [7.3.2 네이버 Open API 서비스 키 획득]의 [2. 서비스키 획득]에서 [(2) 네이버 지도 API 키 발급]과 [7.3. 샘플 어플리케이션 배포]을 참고한다.<br>

화면의 좌측 하단 셀렉트 박스에서 지역을 선택해보면서 각각의 API들이 정상적으로 작동하는 것을 확인한다.

![7-7-0-3]


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
[7-3-1-0]:/images/openpaas-service/publicapi/7-3-1-0.png
[7-3-1-1]:/images/openpaas-service/publicapi/7-3-1-1.png
[7-3-1-2]:/images/openpaas-service/publicapi/7-3-1-2.png
[7-3-1-3]:/images/openpaas-service/publicapi/7-3-1-3.png
[7-3-1-4]:/images/openpaas-service/publicapi/7-3-1-4.png
[7-3-1-5]:/images/openpaas-service/publicapi/7-3-1-5.png
[7-3-1-6]:/images/openpaas-service/publicapi/7-3-1-6.png
[7-3-1-7]:/images/openpaas-service/publicapi/7-3-1-7.png
[7-3-1-8]:/images/openpaas-service/publicapi/7-3-1-8.png
[7-3-1-9]:/images/openpaas-service/publicapi/7-3-1-9.png
[7-3-1-10]:/images/openpaas-service/publicapi/7-3-1-10.png
[7-3-1-11]:/images/openpaas-service/publicapi/7-3-1-11.png
[7-3-1-12]:/images/openpaas-service/publicapi/7-3-1-12.png
[7-3-1-13]:/images/openpaas-service/publicapi/7-3-1-13.png
[7-3-1-14]:/images/openpaas-service/publicapi/7-3-1-14.png
[7-3-1-15]:/images/openpaas-service/publicapi/7-3-1-15.png
[7-3-1-16]:/images/openpaas-service/publicapi/7-3-1-16.png
[7-3-1-17]:/images/openpaas-service/publicapi/7-3-1-17.png
[7-3-1-18]:/images/openpaas-service/publicapi/7-3-1-18.png
[7-3-1-19]:/images/openpaas-service/publicapi/7-3-1-19.png
[7-3-1-20]:/images/openpaas-service/publicapi/7-3-1-20.png
[7-3-1-21]:/images/openpaas-service/publicapi/7-3-1-21.png
[7-3-2-0]:/images/openpaas-service/publicapi/7-3-2-0.png
[7-3-2-1]:/images/openpaas-service/publicapi/7-3-2-1.png
[7-3-2-2]:/images/openpaas-service/publicapi/7-3-2-2.png
[7-3-2-3]:/images/openpaas-service/publicapi/7-3-2-3.png
[7-3-2-4]:/images/openpaas-service/publicapi/7-3-2-4.png
[7-3-2-5]:/images/openpaas-service/publicapi/7-3-2-5.png
[7-3-2-6]:/images/openpaas-service/publicapi/7-3-2-6.png
[7-3-2-7]:/images/openpaas-service/publicapi/7-3-2-7.png
[7-3-2-8]:/images/openpaas-service/publicapi/7-3-2-8.png
[7-3-2-9]:/images/openpaas-service/publicapi/7-3-2-9.png
[7-3-2-10]:/images/openpaas-service/publicapi/7-3-2-10.png
[7-3-2-11]:/images/openpaas-service/publicapi/7-3-2-11.png
[7-3-2-12]:/images/openpaas-service/publicapi/7-3-2-12.png
[7-5-2-0]:/images/openpaas-service/publicapi/7-5-2-0.png
[7-6-2-0]:/images/openpaas-service/publicapi/7-6-2-0.png
[7-7-0-0]:/images/openpaas-service/publicapi/7-7-0-0.png
[7-7-0-1]:/images/openpaas-service/publicapi/7-7-0-1.png
[7-7-0-2]:/images/openpaas-service/publicapi/7-7-0-2.png
[7-7-0-3]:/images/openpaas-service/publicapi/7-7-0-3.png




