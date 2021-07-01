
## Table of Contents
1. [개요](#1)
    * [1.1. 문서 개요](#2)
    * [1.1.1 목적](#3)
    * [1.1.2 범위](#4)
    * [1.1.3 참고자료](#5)
2. [시스템 구성](#6)
     * [2.1. 시스템 구성도](#7)
3. [도로명주소 서비스](#8)
     * [3.1. 도로명 주소 획득](#9)
     * [3.2. 주소 데이터 Table 정의하기](#10)
     * [3.3. 주소 데이터 DB 넣기](#11)
4. [도로명 주소 서비스](#12)
     * [4.1. 도로명 주소 서비스 구조](#13)
     * [4.2. 도로명 주소 서비스 API](#14)
     * [4.2.1. 도로명 주소 검색 서비스](#15)
     * [4.2.2. 도로명 주소 관리 서비스](#16)
     * [4.3. 소스 설명](#17)
     * [4.3.1. Class Diagram](#18)
     * [4.3.2. 소스 리스트 및 설명](#19)
5. [도로명 주소 서비스 등록(API 플랫폼)](#20)
     * [5.1. 도로명 주소 검색 API 등록](#21)
     * [5.2. 도로명 주소 관리 API 등록](#22)
6. [개방형 클라우드 플랫폼 설정](#23)
     * [6.1. API 플랫폼 서비스팩 등록](#24)
     * [6.2. API 플랫폼 서비스팩 Update](#25)
7. [도로명 주소 검색 Sample Web App 설명](#26)
     * [7.1. Sample Web App 구조](#27)
     * [7.2. 개방형 클라우드 플랫폼에서 서비스 신청 및 바인딩 하기](#28)
     * [7.2.1. 서비스 신청(Provision)하기](#29)
     * [7.2.2. 서비스와 Sample App을 연결(Binding) 하기](#30)
     * [7.3. 소스 리스트 및 설명](#31)
     * [7.3.1. Spring 서버 영역](#32)
     * [7.3.2. Web영역 (중요한 파일만 표시 - main/resources 안에 있음)](#33)
8. [별첨A. 도로명 주소 검색 API 정의서](#34)
     * [8.1. 도로명 주소 검색](#35)
9. [별첨B. 도로명 주소 관리 API 정의서](#36)
     * [9.1. 도로명 주소 검색](#37)
     * [9.2. 도로명 주소 검색 (한 개)](#38)
     * [9.3. 도로명 주소 등록](#39)
     * [9.4. 도로명 주소 수정](#40)
     * [9.5. 도로명 주소 삭제](#41)
     
     
     
# <a name="1"/>1. 개요

### <a name="2"/>1.1. 문서 개요

##### <a name="3"/>1.1.1. 목적
전자정부 프레임워크에서 사용자의 편의를 위해 제공하는 “공통 컴포넌트”를 서비스형태로 제공하기 위해 일부 컴포넌트를 서비스화해서 API 플랫폼과 연계하는 방법을 제시합니다.

##### <a name="4"/>1.1.2. 범위
전자정부 프레임워크의 공통 컴포넌트 중 “도로명 주소 검색”기능을 자체 DB로 구축합니다. 데이터는 도로명 주소 안내시스템([***http://www.juso.go.kr***](http://www.juso.go.kr))에서 제공하는 정보를 이용하며 도로명 주소 서비스 Open API의 스펙을 그대로 활용하여 제작합니다. 이렇게 만들어진 API를 인증, 관리하기 위해 API 플랫폼을 활용합니다. 

##### <a name="5"/>1.1.3. 참고 자료
-   [가이드]주소구축(전체주소)활용 방법.pdf’ – 도로명 주소 안내시스템([***http://www.juso.go.kr***](http://www.juso.go.kr)) 에서 다운
-   [가이드]OpenAPI 연계신청 및 활용 방법.pdf’ – 도로명 주소 안내시스템([***http://www.juso.go.kr***](http://www.juso.go.kr)) 에서 다운
-   WSO2 API Manager Document – Getting Started - Quick Start Guide([***https://docs.wso2.com/display/AM180/Quick+Start+Guide***](https://docs.wso2.com/display/AM180/Quick+Start+Guide))



# <a name="6"/>2. 시스템 구성

### <a name="7"/>2.1. 시스템 구성도
도로명 주소 서비스(API)를 만들고 이 서비스를 API 플랫폼에 등록합니다. 그리고 일반 개발자는 도로명 주소 서비스를 “개방형 클라우드 플랫폼”에서 신청하여 사용합니다.
>![api_platform_dorojuso_01]

그림. 도로명 주소 검색 서비스 시스템 구성도


API 플랫폼에서는 도로명 주소 서비스 API에 대한 접근관리와 통계, Life cycle 관리를 합니다. API 플랫폼에 대한 자세한 설명은 별도의 API 플랫폼 가이드를 참고하여 주시기 바랍니다.

도로명 주소 서비스(API)는 어떤 REST/full 형태의 서버로 개발을 하였으며 도로명 주소 정보는 MySQL DB에서 관리합니다.



# <a name="8"/>3. 도로명주소 서비스

### <a name="9"/>3.1. 도로명 주소 획득
도로명주소를 자체 DBMS에 구축하기 위해 먼저 도로명 주소를 가져와야 합니다. 이 정보는 “도로명주소안내 시스템” ([***http://www.juso.go.kr***](http://www.juso.go.kr))에서 다운로드를 받으실 수 있습니다. 

먼저 사이트에 접속을 하면 아래 화면과 같이 메인 화면에서 “주소제공”->”도로명주소 DB제공”을 선택합니다.
>![api_platform_dorojuso_02]

그림. 주소 제공하는 페이지로 이동


주소제공 페이지에서는 주소데이터의 활용방법을 기술한 PDF파일과 가장 최근의 주소 데이터 정보를 다운로드 받으실 수 있습니다. “전체주소 구축가이드 다운로드”와 전체주소(최종안)의 가장 최신의 일자 정보를 받습니다. 아래 화면에서 빨간색 부분으로 표시된 두 영역을 클릭하여 정보를 다운로드 받습니다.
>![api_platform_dorojuso_03]

그림. 구축가이드와 최종 주소 다운로드


다운로드 받은 “[가이드]주소구축(전체주소)활용방법.pdf”에서는 데이터의 구조를 파악할 수 있습니다. 문서의 13페이지에 같이 다운로드 받은 주소 파일의 스펙이 정의되어 있습니다. 중간에 보면 “건물관리번호”가 있는데 이 정보가 Primary Key 입니다.
>![api_platform_dorojuso_04]

그림. “[가이드]주소구축(전체주소)활용방법.pdf”에서 스펙정보


주소 파일은 압축파일로 약 158MB정도가 됩니다. (2015년 6월 기준) 이 파일을 압축을 해제하면 아래와 같이 TXT정보로 된 주소파일을 확인할 수 있습니다. 
>![api_platform_dorojuso_05]

그림. 주소 파일 리스트


압축을 해제하면 1.85GB정도됩니다. 이 TXT 파일은 “|” 구분자로 되어 있는 텍스트 파일로 MySQL에 데이터를 Import해야 합니다.

### <a name="10"/>3.2. 주소 데이터 Table 정의하기
주소 데이터를 저장하기 위해서는 테이블의 구조를 정의해야 합니다. 본문서는 MySQL에 도로명주소 정보를 저장하고 관리하기 때문에 MySQL의 문법에 맞게 정의하였습니다.
가능한 “[가이드]주소구축(전체주소)활용방법.pdf “에 정의된 한글명칭과 비슷하게 구조를 잡았습니다.

````
CREATE TABLE IF NOT EXISTS `egov_common`.`doro_juso` (
  `code` CHAR(10) NULL COMMENT '',
  `sido` VARCHAR(40) NULL COMMENT '',
  `sigungu` VARCHAR(40) NULL COMMENT '',
  `eupmyundong` VARCHAR(40) NULL COMMENT '',
  `ri` VARCHAR(40) NULL COMMENT '',
  `san` CHAR(1) NULL COMMENT '0:대지, 1:산',
  `bunji` SMALLINT(4) NULL COMMENT '',
  `ho` SMALLINT(4) NULL COMMENT '',
  `doro_code` CHAR(12) NULL COMMENT '시군구코드(5)+도로명번호(7)',
  `doro` VARCHAR(80) NULL COMMENT '',
  `jiha` CHAR(1) NULL COMMENT '0:지상, 1:지하, 2:공중',
  `bon` SMALLINT(5) NULL COMMENT '',
  `bu` SMALLINT(5) NULL COMMENT '',
  `gunmul` VARCHAR(40) NULL COMMENT '',
  `gunmul_sangse` VARCHAR(100) NULL COMMENT '',
  `gunmul_no` VARCHAR(25) NOT NULL COMMENT '',
  `eupmyundong_no` SMALLINT(2) NULL COMMENT '',
  `hang_code` CHAR(10) NULL COMMENT '',
  `hang` VARCHAR(20) NULL COMMENT '',
  `zipcode` CHAR(6) NULL COMMENT '',
  `zipno` CHAR(3) NULL COMMENT '',
  `dayaeng` VARCHAR(40) NULL COMMENT '',
  `idong` CHAR(2) NULL COMMENT '31 : 건물번호 부여, 34, 변경, 63 : 건물번호 폐지\n72 : 건물군내 일부 건물 폐지,\n73 : 건물군내 일부 건물 생성',
  `update_date` CHAR(8) NULL COMMENT '',
  `defore_doro` VARCHAR(25) NULL COMMENT '',
  `sigungu_gunmul` VARCHAR(200) NULL COMMENT '',
  `gongdong` CHAR(1) NULL COMMENT '0:비공동주택, 1:공동주택',
  `gicho_no` CHAR(5) NULL COMMENT '',
  `juso_sang` CHAR(1) NULL COMMENT '0:미부여, 1:부여',
  `bigo1` VARCHAR(15) NULL COMMENT '',
  `bigo2` VARCHAR(15) NULL COMMENT '',
  PRIMARY KEY (`gunmul_no`)  COMMENT '')
ENGINE = InnoDB;

CREATE INDEX `IDX_SIGUNGU` ON `egov_common`.`doro_juso` (`sigungu` ASC)  COMMENT '';
CREATE INDEX `IDX_DONG` ON `egov_common`.`doro_juso` (`eupmyundong` ASC)  COMMENT '';
CREATE INDEX `IDX_DORO` ON `egov_common`.`doro_juso` (`doro` ASC)  COMMENT '';
````



### <a name="11"/>3.3. 주소 데이터 DB에 넣기
해당 주소 정보를 DBMS에 Import 하기 전에 하나 점검을 해야 할 것이 있습니다. 먼저 구축하고자하는 DBMS의 언어설정이 어떻게 되어 있는지를 확인해야 합니다. 현재 이 파일은 한글 표준이 ANSI로 되어 있습니다. 요즘 일반적으로 DB를 구성할 때 UTF8로 구성하기 때문에 정확하게 한글이 Import되게 하려면 파일을 하나씩 읽어 문자 인코딩을 변경하여 Insert하는 프로그램을 개발하거나 상용 툴들을 이용해야 합니다.

이 문서는 MySQL 서버를 대상으로 데이터를 Import 하는데 기존의 TXT 파일을 UTF8로 변환하고 변환된 파일을 MySQL Import 기능을 이용하여 DBMS에 넣는 방법을 설명하겠습니다. 물론 DBMS의 DB는 UTF8로 설정되어 있습니다.

파일의 인코딩을 UTF8로 변환하는 방법은 Windows 환경에서 해당 파일을 메모장에서 열고 이 파일을 다시 “다른 이름으로 저장”하기를 할 때 인코딩을 변경하는 방법입니다.
>![api_platform_dorojuso_06]

그림. 메모장에서 다른 이름으로 저장하기


이렇게 파일을 준비하고 MySQL Import를 이용하여 데이터를 Import합니다.
````
mysqlimport -u [사용자ID] -p [데이터베이스명] --fields-terminated-by="|" --lines-terminated-by="\n" [파일명칭]
````

여기서 [파일명칭]은 Table명칭과 같아야 합니다. 그래서 여러 개의 파일을 Import해야 되는 상황에서는 각각을 이름을 변경하고 해당 명령어를 수행해야 합니다.

mysqlimport 명령문을 설명하면
-u [사용자ID] : 해당 데이터베이스에 Insert가 가능한 사용자 ID입니다.
-p : Mysql Import 시 비밀번호를 물어봅니다.
[데이터베이스명] : 도로명 주소가 저장될 데이터베이스명입니다.
--field-terminated-by=“|” : 각각의 필드/컬럼을 구분하는 문자는 | 로 표시하였습니다.
--lines-terminated-by=”\n” : 한줄의 끝이 “\n”으로 구분합니다. (Windows는 끝이 \r\n 입니다.)
[파일명칭] : Import하려는 파일의 위치이며 파일 명칭이 Table 명칭입니다.

텍스트 파일을 모두 Import하였으면 도로명 주소 데이터는 준비가 완료되었습니다.



# <a name="12"/>4. 도로명 주소 서비스

### <a name="13"/>4.1. 도로명 주소 서비스 구조
전자정부 프레임워크의 공통 컴포넌트중 도로명 주소 서비스(OpenAPI)가 있습니다. 이 컴포넌트를 자체 DB로 구축하고 도로명 주소 서비스(Open API)와 같은 구조로 도로명 주소 검색 서비스를 지원할 수 있도록 개발을 합니다.

Spring Framework를 이용하여 개발을 하였으며 사용한 컴포넌트에 대한 정보는 아래와 같습니다.

<table>
  <tr>
    <td>모듈</td>
    <td>버전</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>Java</td>
    <td>1.7</td>
    <td>Java 컴파일러/실행환경</td>
  </tr>
  <tr>
    <td>Spring Boot</td>
    <td>1.3</td>
    <td>Java 만으로 Web App을 구동 시킬 수 있는 모듈</td>
  </tr>
  <tr>
    <td>JSON Path</td>
    <td>2.0.0</td>
    <td>JSON 데이터로 변환하여 관리</td>
  </tr>
    <tr>
      <td>Spring JDBC</td>
      <td>4.0.0</td>
      <td>JDBC를 사용하기 위한 Spring Library</td>
    </tr>
      <tr>
        <td>MySQL connector</td>
        <td>5.1.27</td>
        <td>MySQL과 연결을 위한 Driver 라이브러리</td>
     </tr>
</table>



### <a name="14"/>4.2. 도로명 주소 서비스 API

#### <a name="15"/>4.2.1. 도로명 주소 검색 서비스
<table>
  <tr>
    <td>API</td>
    <td>방식</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>addrLinkApi.do?currentPage={currentPage}&</td>
    <td>GET</td>
    <td>현재 페이지 번호(currentPage), 페이지당 데이터 개수(countPerPage)와 검색할 단어(keyword)를 Paramater로 받아 검색을 수행합니다.</td>
  </tr>
</table>
※ 상세한 API 정의서는 별첨A.를 참조하여 주세요.

#### <a name="16"/>4.2.2. 도로명 주소 관리 서비스
<table>
  <tr>
    <td>API</td>
    <td>방식</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>/dorojuso/manager/{currentPage}/{countPerPage}/{keyword}</td>
    <td>GET</td>
    <td>현재 페이지 번호(currentPage), 페이지당 데이터 개수(countPerPage)와 검색할 단어(keyword)를 Paramater로 받아 검색을 수행합니다. (PATH 방식)</td>
  </tr>
  <tr>
    <td>/dorojuso/manager/{building_code}</td>
    <td>GET</td>
    <td>건물관리번호(PK)를 Path 변수로 받아 하나의 도로명 주소를 조회합니다.</td>
  </tr>    
  <tr>
    <td>/dorojuso/manager</td>
    <td>POST</td>
    <td>Body에 도로명 주소의 정보를 넣어 도로명 주소 등록을 합니다.</td>
  </tr>
  <tr>
    <td>/dorojuso/manager/{building_code}</td>
    <td>PUT</td>
    <td>Body에 도로명 주소의 정보를 넣어 도로명 주소 수정을 합니다.</td>
  </tr>
  <tr>
    <td>/dorojuso/manager/{building_code}</td>
    <td>DELETE</td>
    <td>도로명 주소 데이터를 삭제합니다.</td>
  </tr>
</table>
※ 상세한 API 정의서는 별첨A.를 참조하여 주세요.

### <a name="17"/>4.3. 소스 설명

#### <a name="18"/>4.3.1. Class Diagram
주요 Class에 대한 구성은 아래의 Class Diagram과 같습니다. Model과 Exception, Utility는 제외한 프로세스에서 중요한 Controller, Service, DAO를 표시하였습니다.
>![api_platform_dorojuso_07]

그림. 도로명 주소 서비스의 Class Diagram

BaseController는 각종 Exception을 정의하고 있습니다. 이를 상속받은 DoroJusoController가 도로명 주소 검색에 대한 Control하게 되면 DoroJusoService에서 비즈니스 로직을 담당합니다.
DoroJusomanagerController는 DoroJusoConroller와 같이 BaseController를 상속받으며 도로명 주소를 관리하기 위한 등록, 수정, 삭제를 할 수 있는 서비스(API)를 Control하며 DoroJusoManagerService에서 관리를 위한 비즈니스 로직을 담당합니다.

#### <a name="19"/>4.3.2. 소스 리스트 및 설명
해당 소스의 위치는 “개방형 클라우드 플랫폼”의 Git Hub에 위치하며 일반에게 공개할 위치는 따로 홈페이지를 통해서 공유가 될 예정입니다.
(개발을 위한 Private 위치는 [**https://github.com/OpenPaaSRnD/service_egov_common_juso/tree/master/service_egov_common_juso**](https://github.com/OpenPaaSRnD/service_egov_common_juso/tree/master/service_egov_common_juso) 입니다)

<table>
  <tr>
    <td>Package명/소스명</td>
    <td>설명</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.dorojuso</td>
  </tr>
  <tr>
    <td>Application</td>
    <td>Spring Boot를 시작하는 Main을 포함하는 Class입니다.</td>
  </tr>
  <tr>
    <td>MysqlConfig</td>
    <td>MySQL의 Datasource를 설정하는 Config Class입니다.</td>
  </tr>
  <tr>
    <td>SimpleCORSFilter</td>
    <td>CORS를 처리하기 위한 Filter Class입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.common</td>
  </tr>
  <tr>
    <td>StringUtils</td>
    <td>String의 처리를 위해 공통으로 사용하는 Utility들의 Class입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.controller</td>
  </tr>
  <tr>
    <td>BaseContoller</td>
    <td>Controller들의 상위 Controller로 Error처리, Exception 처리를 정의하였습니다.</td>
  </tr>
  <tr>
    <td>DoroJusoController</td>
    <td>도로명 주소 검색 서비스를 Control하는 Class입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoManagerController</td>
    <td>도로명 주소 관리 서비스를 Control하는 Class입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.dao</td>
  </tr>
  <tr>
    <td>DoroJusoDAO</td>
    <td>도로명 주소 DB와 접속을 위한 DAO 인터페이스입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.dao.impl</td>
  </tr>
  <tr>
    <td>DoroJusoDAOImpl</td>
    <td>DAO 인터페이스를 구현한 Class입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.exception</td>
  </tr>
  <tr>
    <td>DoroJusoBadRequestException</td>
    <td>잘못된 요청에 대한 오류를 처리하기 위한 Exception입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoDoesNotExistException</td>
    <td>수정, 삭제시 존재하지 않는 도로명 정보에 대한 오류를 처리하기 위한 Exception입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoException</td>
    <td>도로명 주소 처리중 발생하는 일반 오류를 처리하는 Exception입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoExistsException</td>
    <td>도로명 주소가 기존에 존재하여 중복되는 오류를 처리하는 Exception입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.model</td>
  </tr>
  <tr>
    <td>Common</td>
    <td>도로명 주소 검색 결과 값에 공통부분(오류코드, 개수 등)을 가지는 Model입니다.</td>
  </tr>
  <tr>
    <td>DoroJuso</td>
    <td>도로명 주소 검색 결과 전체의 Model입니다. 공통(Common)부분과 Juso 리스트를 포함하고 있습니다.</td>
  </tr>
  <tr>
    <td>DoroJusoInfo</td>
    <td>도로명 주소 DB에서 정보를 가져올 때의 Model입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoInfoResult</td>
    <td>도로명 주소 관리의 결과(조회)에서 사용하는 Model입니다.</td>
  </tr> 
  <tr>
    <td>Juso</td>
    <td>도로명 주소 검색 결과 값중 juso 부분의 Model입니다.</td>
  </tr>
  <tr>
    <td>ResultMessage</td>
    <td>도로명 주소 관리의 결과(등록, 수정, 삭제)의 결과 Model입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.service</td>
  </tr>
  <tr>
    <td>DoroJusoManagerService</td>
    <td>도로명 주소 관리의 비즈니스 로직을 담당하는 Service입니다.</td>
  </tr>
  <tr>
    <td>DoroJusoService</td>소
    <td>도로명 주소 검색의 비즈니스 로직을 담당하는 Service입니다.</td>
  </tr>
</table>



# <a name="20"/>5. 도로명 주소 서비스 등록(API 플랫폼)
4장에서 만들어지 도로명 주소 서비스는 인증/API 관리(Life cycle)부분이 없습니다. 단순히 요청에 대해 응답하는 구조입니다. 실제 서비스를 위해서는 사용자 인증, Token 발생/관리, API의 Life cycle 관리, 통계 등의 많은 필수 기능들이 필요합니다. 이를 위해 기존의 전자정부 프레임워크의 공통 컴포넌트를 서비스화 할 때 도움을 주고자 API 플랫폼을 선정하여 이를 통해서 서비스가 이루어지도록 하였습니다.
API 플랫폼에 대한 설치 및 사용 방법의 자세한 매뉴얼은 “분석_API Manager 설치 매뉴얼”를 참조하여 주시기 바랍니다.
본 문서에서는 API 플랫폼에 도로명 주소 서비스 API를 등록하는 방법과 노하우 정도를 간단하게 기술하였습니다.

### <a name="21"/>5.1. 도로명 주소 검색 API 등록
실제 사용자가 사용하게 될 서비스 API로 도로명 주소를 검색하는 기능을 등록합니다. 먼저 API 플랫폼의 Publisher 화면으로 접속을 합니다. (URL은 별도로 공지합니다.)

API 플랫폼 Publisher에서 API를 추가(add) 합니다.
>![api_platform_dorojuso_08]

그림. API의 기본정보를 입력


서비스의 기본정보를 입력합니다. Name은 “개방형 클라우드 플랫폼”의 Market Place에서 보여질 명칭이 될 겁니다. Context는 API 플랫폼에 접속을 위한 최초의 path가 될 겁니다. 기존에 API 플랫폼에 존재하지 않는 명칭을 선택해서 진행을 합니다. 
Description은 API 플랫폼에서는 필수사항은 아니지만 개방형 클라우드 플랫폼에서는 필수 사항이니 꼭 입력을 해야 합니다.

이제 실제 API를 등록합니다. 도로명 주소 서비스([***http://www.juso.go.kr***](http://www.juso.go.kr))에서 제공하는 스펙과 같이 만들기 위해 API는 addrLinkApi.do로 하였고 Parameter 또한 같은 명칭을 사용하였습니다. 먼저 addrLinkApi.do를 URL Param에 넣고 GET, OPTIONS를 선택하고 Add New Resource를 누릅니다. 그럼 GET, OPTIONS의 두 개의 API가 생성됩니다.
>![api_platform_dorojuso_09]

그림. API 등록


Add New Resource버튼을 누르면 각각의 Parameter를 정의할 수 있도록 GET과 OPTIONS가 화면하단에 나타납니다. GET에 Parameter를 입력하기 위해 GET을 클릭하여 화면을 확장시킵니다. currentPage, countPerPage는 Data type을 integer로 넣고 Parameter type은 query를 선택합니다. keyword는 Data type은 String으로 Parameter type은 query로 넣습니다. 모두 Required는 true, 필수로 선택을 합니다.
>![api_platform_dorojuso_10]

그림. Parameter 넣기


OPTIONS을 등록해주는 이유는 https나 기타 Web 통신에서 API가 유효한지 CORS를 체크하기 위해 OPTIONS를 먼저 요청하는 경우가 있어 API 등록 시에는 반드시 OPTIONS를 추가 해줍니다.

API에 대한 등록을 완료하면 Implement 단계로 넘어갑니다. Implement단계에서는 서비스의 Endpoint를 등록합니다. Endpoint type은 Http 통신으로 서비스를 할 것이니 “HTTP Endpoint”를 선택을 하고 도로명 주소 서비스 서버의 URL을 Production, Sandbox Endpoint에 넣습니다. 개발 서버/테스트 서버는 Sandbox endpoint에 넣어주고 실제 운영 서비스는 Production endpoint에 넣어줍니다. 
여기서 입력한 Endpoint가 기본이 되고 뒤에 API, Parameter를 붙여서 실제 요청을 하게 됩니다.
>![api_platform_dorojuso_11]

그림. Endpoint 등록


Endpoint 등록이 완료되면 마지막 단계인 API의 접근설정을 합니다. 
통신방식(http, https)와 접근 제한 설정(Tier Availability)를 설정합니다. 제약사항을 더 추가하기 위해서는 API 플랫폼의 매뉴얼을 참고하여주세요. 여기서는 기본 설정만 하는 것으로 합니다. 
접근 제한 설정(Tier Availability)는 Unlimited로 설정을 합니다. 개방형 클라우드 플랫폼과의 연결시, 제한 설정을 하지 않고 모니터링만 하기로 되어 있기 때문에 두 시스템을 연결하는 서비스(서비스팩)에 Unlimited만 사용할 수 있게 설정되어 있습니다.
>![api_platform_dorojuso_12]

그림. 접속 환경 설정


이제 API에 대한 정보는 입력을 마쳤으며 이 API를 Publishing 을 하면 API플랫폼 Store에 보여지는 것을 확인하면 됩니다. 
>![api_platform_dorojuso_13]

그림. API플랫폼 


위와 같이 Store에서 만든 서비스가 보이고 API Console에서 API를 호출했을 때 결과 값이 정상으로 오면 등록이 성공적으로 된 것입니다.

지금까지 도로명 주소 서비스 서버를 만들었고 해당 서비스를 API 플랫폼에서 관리하게 하였습니다. 이제는 개방형 클라우드 플랫폼에 서비스를 등록하여 플랫폼 내에서 서비스를 이용할 수 있게 하면 준비는 완료됩니다.

### <a name="22"/>5.2. 도로명 주소 관리 API 등록
도로명 주소 서비스의 데이터를 관리하는 기능으로 조회, 등록, 수정, 삭제를 할 수 있게 합니다. 해당 관리 기능도 검색 서비스와 같이 API로 만들고 접근 관리는 API 플랫폼에서 합니다. 
API플랫폼에 등록하는 방법은 거의 유사하고 맨 처음 API의 기본 정보를 입력하는 부분만 다르니 이 부분만 설명하겠습니다.

도로명 주소 API 등록합니다. 등록, 수정, 삭제, 조회에 대해서 다 등록을 해야 합니다. 자세한 사항은 별첨B의 API 정의서로 등록을 하면 됩니다.
>![api_platform_dorojuso_14]

그림. 도로명 주소 관리 API 등록


도로명 주소 관리는 Parameter를 query 방식이 아닌 path 방식으로 설계하였습니다. API 플랫폼에서는 아래의 화면과 같이 URL Pattern에 dorojuso/manager/{building_code}와 같이 { }로 path 변수 명을 입력하면 자동으로 parameter를 설정해 줍니다.
>![api_platform_dorojuso_15]

그림. Path방식의 Parameter 입력


>![api_platform_dorojuso_16]

그림. Path에 변수를 넣었을 경우의 화면


여기서도 주의할 것은 각 API Pattern별로 OPTIONS을 같이 등록해야 합니다. Rest통신시 OPTIONS로 해당 API의 유효성을 체크해주기도 해서 등록을 해줘야 합니다. 나머지 Endpoint, 접속 환경 설정은 도로명 주소 검색 서비스와 동일하기 입력하시면 됩니다. 입력이 완료되고 Publish까지 완료되면 Store에서 API가 잘 보이는 지와 API Console에서 정상동작을 하는지 확인하시면 됩니다.

이렇게 도로명 주소 관리 서비스까지 API 플랫폼에 등록을 완료하였습니다.



# <a name="23"/>6. 개방형 클라우드 플랫폼 설정
개방형 클라우드 플랫폼에서 API 플랫폼의 API를 사용하기 위해서는 서비스팩(Service Broker)을 등록해야 합니다. 만약 한번이라도 API 플랫폼의 서비스팩을 등록을 했을 경우에는 서비스팩 Update를 해줘야 합니다.

개방형 클라우드 플랫폼에서 서비스를 등록하고 관리하는 방법은 따로 매뉴얼에서 설명이 되어 있으니 본 가이드에서는 간략하게 어떤 명령으로 등록하고 권한 설정, Update하는 명령 위주로만 설명하겠습니다.

### <a name="24"/>6.1. API 플랫폼 서비스팩 등록
cf cli 로 서버에 로그인을 합니다. 서비스팩(Service Broker)를 등록할 수 있는 권한을 가진 사용자 이여야합니다.

서비스 팩 등록을 위해서는 아래와 같이 명령을 입력합니다.
````
$ cf create-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 사용자비밀번호} http://{서비스팩 URL}
````
- 서비스팩 이름 : 서비스 팩 관리를 위해 개방형 클라우드 플랫폼에서 보여지는 명칭입니다. 서비스 Marketplace에서는 각각의 API 서비스 명이 보여지니 여기서 명칭은 서비스팩 리스트의 명칭입니다.
- 서비스팩 사용자ID / 비밀번호 : 서비스팩에 접근할 수 있는 사용자 ID입니다. 서비스팩도 하나의 API 서버이기 때문에 아무나 접근을 허용할 수 없어 접근이 가능한 ID/비밀번호를 입력합니다.
- 서비스팩 URL : 서비스팩이 제공하는 API를 사용할 수 있는 URL을 입력합니다.

서비스팩 등록이 완료되었으며 아래와 같이 등록된 정보를 확인할 수 있습니다.
````
$ cf service-brokers
Getting service brokers as admin...Cloud Controller
OK

Name            URL
{서비스팩 이름}   http://{서비스팩 URL}
````
하지만 현재 상태에서는 Marketplace에서 리스트를 확인할 수 없습니다. 이유는 Access할 수 있는 권한이 설정되지 않아서 입니다. 아래와 같이 서비스의 access 권한을 확인할 수 있습니다.
````
$ cf service-access

broker: apiplatform-sb
   service         plan        access   orgs
dorojusodb      Unlimited   none
   dorojusodbMgt   Unlimited   none
````

도로명 주소 검색/관리 서비스의 access가 설정이 안된 것을 확인 할 수 있습니다. 이 access를 enable 시켜야 Marketplace에서 확인이 가능하고 서비스 신청(Provision), 연결(Bind)를 할 수 있습니다.
````
$ cf enable-service-access dorojusodb
$ cf enable-service-access dorojusodbMgt

$ cf service-access

broker: apiplatform-sb
   service         plan        access   orgs
dorojusodb      Unlimited   all
   dorojusodbMgt   Unlimited   all
````
서비스를 Enable해주고 다시 서비스 접근 리스트를 조회하면 access가 all로 되어 있는 것을 확인할 수 있습니다.

이제 개방형 클라우드 플랫폼에서 도로명 주소 서비스를 사용할 준비가 되었습니다.

### <a name="25"/>6.2. API 플랫폼 서비스팩 Update
만약 기존에 서비스팩이 등록되었었으면 서비스팩 Update를 해야합니다. API플랫폼에 새로운 API를 만들었거나 없어졌을 경우에는 이 Update를 해줘서 개방형 클라우드 플랫폼과 정보를 맞춰야 합니다.

명령문은 아래와 같습니다.
````
$ cf update-service-broker {서비스팩 이름} {서비스팩 사용자ID} {서비스팩 비밀번호} http://{서비스팩 URL}
````
새로 생성된 서비스는 접근 권한을 새로 설정해야 하며 전 절에서 설명한 enable-service-access 를 이용하여 접근 권한을 줍니다.



# <a name="26"/>7. 도로명 주소 검색  Sample Web App 설명
본 Sample Web App은 개방형 클라우드 플랫폼에 배포되며 API 플랫폼의 서비스를 Provision과 Bind를 한 상태에서 사용이 가능합니다.

### <a name="27"/>7.1. Sample Web App 구조
Sample Web App은 개방형 클라우드 플랫폼에 App으로 배포가 됩니다. 여기서는 간단하게 Spring Boot를 이용하여 개방형 클라우드 플랫폼에서 환경정보(서비스 연결 정보)를 가져오고 Javascript, AngularJS내에서 그 정보를 활용할 수 있게 되어 있습니다.

<table>
  <tr>
    <td>모듈</td>
    <td>버전</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>Java</td>
    <td>1.7</td>
    <td>Java 컴파일러/샐행환경</td>
  </tr>
  <tr>
    <td>Spring Boot</td>
    <td>1.3</td>
    <td>Java 만으로 Web App을 구동 시킬 수 있는 모듈</td>
  </tr>
  <tr>
    <td>Spring cloud</td>
    <td>1.0</td>
    <td>개방형 클라우드 플랫폼에서 서비스와 바인딩한 정보를 가져오기 위한 라이브러리</td>
  </tr>
    <tr>
      <td>thymeleaf</td>
      <td>1.3</td>
      <td>HTML에 객체를 전달하기 위해 사용한 모듈</td>
    </tr>
      <tr>
        <td>httpclient</td>
        <td>4.4.1</td>
        <td>API 플랫폼과 HTTP 통신을 하기 위함. (token을 가져올때 사용함)</td>
     </tr>
</table>
 
<table>
  <tr>
    <td>모듈</td>
    <td>버전</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>AngularJS</td>
    <td>1.2.18</td>
    <td>HTML에서 MVC를 이용할 수 있게하는 Javascript Framework</td>
  </tr>
  <tr>
    <td>JQuery</td>
    <td>1.11</td>
    <td>Javascript를 잘 사용하기 위한 Library</td>
  </tr>
  <tr>
    <td>Bootstrap</td>
    <td>3.1</td>
    <td></td>
  </tr>
  <tr>
    <td>sb-admin</td>
    <td>1</td>
    <td>Bootstrap 테마로 각종 UI 컴포넌트와 메뉴틀을 제공함.</td>
  </tr>
</table>

### <a name="28"/>7.2. 개방형 클라우드 플랫폼에서 서비스 신청 및 바인딩 하기
6장에서 등록한 서비스를 앱에서 사용하기 위해서는 서비스 신청(Provision)과 연결(Binding)을 해야합니다. 연결을 위해서는 App이 한번은 배포가 되어 있어야 합니다.
개방형 클라우드 플랫폼의 자세한 사용법은 서비스 이용을 참조하여 주시기 바랍니다. 여기서는 간단하게 도로명 주소 서비스 연결하는 부분만 설명하겠습니다.

#### <a name="29"/>7.2.1 서비스 신청(Provision)하기
먼저 개방형 클라우드 플랫폼 Marketplace에서 서비스가 있는지 확인을 합니다.
````
$ cf marketplace

service         plans                                                description
dorojusodb      Unlimited                                            search the doro juso
dorojusodbMgt   Unlimited                                            dorojusodbmgt 

* These service plans have an associated cost. Creating a service instance will incur this cost.

TIP:  Use 'cf marketplace -s SERVICE' to view descriptions of individual plans of a given service.
````

Marketplace에서 원하는 서비스가 있는지 확인하면 서비스 신청(Provision)을 합니다. 
````
$ cf create-service {서비스명} {서비스플랜} {내서비스명}
````
- 서비스명 : dorojusodb, dorojusodbMgt로 Marketplace에서 보여지는 서비스 명칭입니다.
- 서비스플랜 : 서비스에 대한 정책으로 plans에 있는 정보 중 하나를 선택합니다. API 플랫폼은 Unlimited만 지원하므로 Unlimited를 선택하면 됩니다.
- 내서비스명 : 내 서비스에서 보여지는 명칭입니다. 이 명칭을 기준으로 환경설정정보를 가져오기 때문에 서버에 설정되어 있는 명칭으로 꼭 등록을 해야 합니다. (dorojusodb, dorojusomgt)
서비스 생성이 끝났으면 내가 만든 앱과 연결(Binding)을 해야합니다.

#### <a name="30"/>7.2.2. 서비스와 Sample App을 연결(Binding) 하기
먼저 나의 앱이 배포되어 있는 상태를 확인합니다. 
````
$ cf apps

name                                 requested state   instances   memory   disk   urls
service_egov_common_juso_sampleApp   started           1/1         512M     1G     service-egov-common-juso-sampleapp.cf-dev.open-paas.com
````
초기에 앱을 일단 기본 구성만으로 배포를 해놓고 서비스와 연결(Binding)을 해야만 서비스의 정보(접속 URL, 접속 ID, 비밀번호)를 가져올 수 있습니다. 개방형 클라우드 플랫폼의 앱 개발/배포에 대한 부분에서 자세히 설명되어 있습니다.

나의 앱의 이름을 확인한 후에 앱과 서비스를 연결(Binding)합니다.
````
$ cf bind-service {내앱 명칭} {내서비스명}
````
- 내앱 명칭 : 위의 예제로 보면 service_egov_common_juso_sampleApp이 됩니다. 나의 앱의 고유한 명칭을 이야기합니다.
- 내서비스명 : 서비스 신청(Provision)시에 부여한 명칭입니다.

연결이 정상적으로 이루어 졌으면 앱을 restage하라고 나옵니다. 현재 앱은 서비스와 연결이 안되어 있는 상태이고 restage를 해야지 연결(Binding)이 잘되었는지 확인이 가능합니다.
````
$ cf services

name            service          plan        bound apps                           last operation
dorojusodbmgt   dorojusodbMgt  Unlimited   service_egov_common_juso_sampleApp  create succeeded
dorojusodb      dorojusodb      Unlimited   service_egov_common_juso_sampleApp   create succeeded
````
위의 내용을 보면 service_egov_common_juso_sampleApp가 dorojusodbmgt와 dorojusodb완 연결되어 있는 것을 확인할 수 있습니다.

### <a name="31"/>7.3. 소스 리스트 및 설명
해당 소스의 위치는 “개방형 클라우드 플랫폼”의 Git Hub에 위치하며 일반에게 공개할 위치는 따로 홈페이지를 통해서 공유가 될 예정입니다.
(개발을 위한 Private 위치는 https://github.com/OpenPaaSRnD/service_egov_common_juso/tree/master/service_egov_common_juso_sampleApp 입니다.)

#### <a name="32"/>7.3.1. Spring 서버 영역

<table>
  <tr>
    <td>Package명/소스명</td>
    <td>설명</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.dorojuso.sampleApp</td>
  </tr>
  <tr>
    <td>Application</td>
    <td>Spring Boot를 시작하는 Main을 포함하는 Class입니다.</td>
  </tr>
  <tr>
    <td>ApplicationInstanceInfoCreator</td>
    <td>개방형 클라우드 플랫폼에 배포된 앱의 정보를 담는 Class입니다.</td>
  </tr>
  <tr>
    <td>CloudFoundryConnector</td>
    <td>개방형 클라우드 플랫폼에서 환경설정 정보를 가져오기 위한 Class입니다.</td>
  </tr>
  <tr>
    <td>CloudFoundryServiceInfoCreator</td>
    <td>개방형 클라우드 플랫폼에서 서비스와 연결(Binding)된 정보를 담는 Class입니다.</td>
  </tr>
  <tr>
    <td>Tags</td>
    <td>서비스팩에서 설정한 Tag 정보를 담는 Class입니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.sampleApp.controller</td>
  </tr>
  <tr>
    <td>SampleController</td>
    <td>/ 로 오는 요청을 처리하고 API플랫폼과 token 요청을 처리를 Control하고 있습니다.</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.sampleApp.model</td>
  </tr>
  <tr>
    <td>ResultMessage</td>
    <td>요청에 대한 처리 결과(등록, 수정, 삭제) 정보 Class입니다.</td>
  </tr>
  <tr>
    <td>TokenRequestVO</td>
    <td>Token을 요청하기 위한 사용자 정보를 가진 Class입니다.</td>
  </tr>
  <tr>
    <td>TokenVO</td>
    <td>API 플랫폼에서 획득한 Token을 보내기 위한 Class입니다.
</td>
  </tr>
  <tr>
    <td colspan=2>org.openpaas.egovframwork.comcomponent.sampleApp.service</td>
  </tr>
  <tr>
    <td>SampleService</td>
    <td>API 플랫폼에서 SSL을 사용하지 않고 Token을 가져오기 위한 비즈니스 로직을 수행합니다.</td>
  </tr>
</table>

#### <a name="33"/>7.3.2. Web 영역 (중요한 파일만 표시 -  main/resources 안에 있음)
Web 부분의 소스 설명은 개발한 부분만 정리를 하였으며 main/resoureces 안에 있는 파일들 입니다.

<table>
  <tr>
    <td>디렉토리/파일명</td>
    <td>설명</td>
  </tr>
  <tr>
    <td colspan=2>Template</td>
  </tr>
  <tr>
    <td>app.html</td>
    <td>SampleController에서 호출하는 부분은 개방형 클라우드 플랫폼에서 설정된 환경설정(서버 정보, 서비스 연결 정보)를 보냅니다.</td>
  </tr>
  <tr>
    <td colspan=2>public/js</td>
  </tr>
  <tr>
    <td>app.js</td>
    <td>AngularJS가 시작되는 부분은 Route정보를 가지고 있습니다.</td>
  </tr>
  <tr>
    <td>controller.js</td>
    <td>기본 콘트롤러를 선언합니다. 자세한 콘트롤러는 개별적으로 선언합니다.</td>
  </tr>
  <tr>
    <td colspan=2>public/js/controller</td>
  </tr>
  <tr>
    <td>dorojJusoController.js</td>
    <td>도로명 주소 Open API를 이용한 화면의 Controller입니다. (본 가이드에서는 다루지 않음)</td>
  </tr>
  <tr>
    <td>dorojJusoDBController.js</td>
    <td>도로명 주소 검색 서비스를 이용한 화면의 Controller입니다.</td>
  </tr>
  <tr>
    <td>dorojJusoDBMgtController.js</td>
    <td>도로명 주소 관리 서비스를 이용한 화면의 Controller입니다.</td>
  </tr>
  <tr>
    <td colspan=2>public/partials</td>
  </tr>
  <tr>
    <td>doroJusoDBList.html</td>
    <td>도로명 주소 검색 서비스 리스트 화면 HTML 입니다.</td>
  </tr>
  <tr>
    <td>doroJusoDBMgtForm.html</td>
    <td>도로명 주소 관리 등록/수정시 사용하는 화면 HTML 입니다.</td>
  </tr>
  <tr>
    <td>doroJusoDBMgtList.html</td>
    <td>도로명 주소 관리 리스트 화면 HTML 입니다.</td>
  </tr>
  <tr>
    <td>doroJusoList.html</td>
    <td>도로명 주소 Open API 사용 리스트 화면 HTML 입니다.</td>
  </tr>
  <tr>
    <td>main.html</td>
    <td>처음 표시되는 화면입니다. (내용은 없음)</td>
  </tr>
  <tr>
    <td>waitForm.html</td>
    <td>조회시 사용하는 배경에 로딩 이미지를 표시하기 위한 HTML 입니다.</td>
  </tr>    
</table>



# <a name="34"/>8. 별첨 A. 도로명 주소 검색 API 정의서

## <a name="35"/>8.1. 도로명 주소 검색
사용자가 도로명 주소로 검색할 때 사용하는 검색 서비스입니다. 스펙은 도로명 주소 API (http://www.juso.go.kr) 의 스펙을 그대로 사용하였습니다.

### 8.1.1. Route
````
GET/ http://<server & base path>/addrLinkApi.do
````

### 8.1.2. Request

#### 8.1.2.1. Parameters
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>currentPage</td>
    <td>현재 페이지 수</td>
  </tr>
  <tr> 
    <td>countPerPage</td>
    <td>페이지당 데이터 개수</td>
  </tr>
  <tr>
    <td>keyword</td>
    <td>검색할 단어 keyword</td>
  </tr>
</table>

#### 8.1.2.2. Body
N/A

#### 8.1.2.3. Headers
````
Accept : application/json
````

### 8.1.3. Response

#### 8.1.3.1. Status
````
200 OK
````

#### 8.1.3.2. Body
````
{
    "common": {
    "totalCount": 10,
    "currentPage": 10,
    "countPerPage": 10,
    "errorCode": "0",
    "errorMessage": "정상"
    },
    "juso": [
        {
        "roadAddr": "경기도 양평군 강남로 912 (강상면)",
        "roadAddrPart1": "경기도 양평군 강남로 912",
        "roadAddrPart2": "(강상면)",
        "jibunAddr": "경기도 양평군 강상면 66-0",
        "engAddr": "",
        "zipNo": "476-913",
        "admCd": "4183031021",
        "rnMgtSn": "418303217033",
        "bdMgtSn": "4183031021100660000007779"
    },
    {
        "roadAddr": "경기도 양평군 강남로 952 (강상면)",
        "roadAddrPart1": "경기도 양평군 강남로 952",
        "roadAddrPart2": "(강상면)",
        "jibunAddr": "경기도 양평군 강상면 71-1",
        "engAddr": "",
        "zipNo": "476-913",
        "admCd": "4183031021",
        "rnMgtSn": "418303217033",
        "bdMgtSn": "4183031021100710001010017"
    },
    …
    ]
} 
````



# <a name="36"/>9. 별첨 B. 도로명 주소 관리 API 정의서

## <a name="37"/>9.1. 도로명 주소 검색
관리자가 도로명 주소로 검색할 때 사용하는 검색 서비스입니다. DB 구조와 동일하게 정보를 보내줍니다.

### 9.1.1. Route
````
GET/ http://<server & base path>/dorojuso/manager
````

### 9.1.2. Request

#### 9.1.2.1. Parameters (PATH 방식)
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>currentPage</td>
    <td>현재 페이지 수</td>
  </tr>
  <tr>
    <td>countPerPage</td>
    <td>페이지당 데이터 개수</td>
  </tr>
  <tr>
    <td>keyword</td>
    <td>검색할 단어 keyword</td>
  </tr>
</table>

#### 9.1.2.2. Body
N/A

#### 9.1.2.3. Headers
````
Accept : application/json
````

### 9.1.3. Responser

#### 9.1.3.1. Status
````
200 OK
````

#### 9.1.3.2. Body
````
{
    "juso": [
        {
            "code": "4183031021",
            "sido": "경기도",
            "sigungu": "양평군",
            "eupmyundong": "강상면",
            "ri": "병산리",
            "san": "0",
            "bunji": 66,
            "ho": 0,
            "doro_code": "418303217033",
            "doro": "강남로",
            "jiha": "0",
            "bon": 912,
            "bu": 1,
            "gunmul": "",
            "gunmul_sangse": "",
            "gunmul_no": "4183031021100660000007779",
            "eupmyundong_no": 1,
            "hang_code": "4183031000",
            "hang": "강상면",
            "zipcode": "476913",
            "zipno": "011",
            "dayaeng": "",
            "idong": "",
            "update_date": "",
            "defore_doro": "",
            "sigungu_gunmul": "",
            "gongdong": "0",
            "gicho_no": "12571",
            "juso_sang": "0",
            "bigo1": "",
            "bigo2": ""
        },
        …
    ]
}
````

## <a name="38"/>9.2. 도로명 주소 검색 (한 개)
관리자가 도로명 주소로 검색할 때 사용하는 검색 서비스입니다. DB 구조와 동일하게 정보를 보내줍니다.

### 9.2.1. Route
````
GET/ http://<server & base path>/dorojuso/manager
````

### 9.2.2. Request

#### 9.2.2.1. Parameters (PATH 방식)
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>building_code</td>
    <td>건물 관리 번호</td>
  </tr>
</table>

#### 9.2.2.2. Body
N/A

#### 9.2.2.3. Headers
````
Accept : application/json
````

### 9.2.3. Response

#### 9.2.3.1. Status
````
200 OK
````

#### 9.2.3.2. Body
````
{
    "code": "4183031021",
    "sido": "경기도",
    "sigungu": "양평군",
    "eupmyundong": "강상면",
    "ri": "병산리",
    "san": "0",
    "bunji": 66,
    "ho": 0,
    "doro_code": "418303217033",
    "doro": "강남로",
    "jiha": "0",
    "bon": 912,
    "bu": 1,
    "gunmul": "",
    "gunmul_sangse": "",
    "gunmul_no": "4183031021100660000007779",
    "eupmyundong_no": 1,
    "hang_code": "4183031000",
    "hang": "강상면",
    "zipcode": "476913",
    "zipno": "011",
    "dayaeng": "",
    "idong": "",
    "update_date": "",
    "defore_doro": "",
    "sigungu_gunmul": "",
    "gongdong": "0",
    "gicho_no": "12571",
    "juso_sang": "0",
    "bigo1": "",
    "bigo2": ""
}
````
## <a name="39"/>9.3. 도로명 주소 등록
관리자가 도로명 주소를 새로 등록할 때 사용합니다. 건물관리 번호가 Key로 중복시 오류가 발생합니다.

### 9.3.1. Route
````
POST/ http://<server & base path>/dorojuso/manager
````

### 9.3.2. Request

#### 9.3.2.1. Parameters (PATH 방식)
N/A

#### 9.3.2.2. Body
````
{
    "code": "4183031021",
    "sido": "경기도",
    "sigungu": "양평군",
    "eupmyundong": "강상면",
    "ri": "병산리",
    "san": "0",
    "bunji": 66,
    "ho": 0,
    "doro_code": "418303217033",
    "doro": "강남로",
    "jiha": "0",
    "bon": 912,
    "bu": 1,
    "gunmul": "",
    "gunmul_sangse": "",
    "gunmul_no": "4183031021100660000007779",
    "eupmyundong_no": 1,
    "hang_code": "4183031000",
    "hang": "강상면",
    "zipcode": "476913",
    "zipno": "011",
    "dayaeng": "",
    "idong": "",
    "update_date": "",
    "defore_doro": "",
    "sigungu_gunmul": "",
    "gongdong": "0",
    "gicho_no": "12571",
    "juso_sang": "0",
    "bigo1": "",
    "bigo2": ""
}
````

#### 9.3.2.3. Headers
````
Accept : application/json
````

### 9.3.3. Response

#### 9.3.3.1. Status
````
200 OK
````

#### 9.3.3.2. Body
````
{
    "message": "DoroJuso saved. gunmul_no:4183031021100660000007779"
}
````

## <a name="40"/>9.4. 도로명 주소 수정
관리자가 도로명 주소를 새로 등록할 때 사용합니다. 건물관리 번호가 Key로 중복시 오류가 발생합니다.

### 9.4.1. Route
````
PUT/ http://<server & base path>/dorojuso/manager
````

### 9.4.2. Request

#### 9.4.2.1. Parameters (PATH 방식)
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>building_code</td>
    <td>건물 관리 번호</td>
  </tr>
</table>

#### 9.4.2.2. Body
````
{
    "code": "4183031021",
    "sido": "경기도",
    "sigungu": "양평군",
    "eupmyundong": "강상면",
    "ri": "병산리",
    "san": "0",
    "bunji": 66,
    "ho": 0,
    "doro_code": "418303217033",
    "doro": "강남로",
    "jiha": "0",
    "bon": 912,
    "bu": 1,
    "gunmul": "",
    "gunmul_sangse": "",
    "gunmul_no": "4183031021100660000007779",
    "eupmyundong_no": 1,
    "hang_code": "4183031000",
    "hang": "강상면",
    "zipcode": "476913",
    "zipno": "011",
    "dayaeng": "",
    "idong": "",
    "update_date": "",
    "defore_doro": "",
    "sigungu_gunmul": "",
    "gongdong": "0",
    "gicho_no": "12571",
    "juso_sang": "0",
    "bigo1": "",
    "bigo2": ""
}
````

#### 9.4.2.3. Headers
````
Accept : application/json
````

### 9.4.3. Response

#### 9.4.3.1. Status
````
200 OK
````

#### 9.4.3.2. Body
````
{
    "message": "DoroJuso updated. gunmul_no:4183031021100660000007779"
}
````

## <a name="41"/>9.5. 도로명 주소 삭제
관리자가 도로명 주소를 삭제할 때 사용합니다.

### 9.5.1. Route
````
DELETE/ http://<server & base path>/dorojuso/manager
````

### 9.5.2. Request

#### 9.5.2.1. Parameters (PATH 방식)
<table>
  <tr>
    <td>이름</td>
    <td>설명</td>
  </tr>
  <tr>
    <td>building_code</td>
    <td>건물 관리 번호</td>
  </tr>
</table>

#### 9.5.2.2. Body
N/A

#### 9.5.2.3. Headers
````
Accept : application/json
````

### 9.5.3. Response

#### 9.5.3.1. Status
````
200 OK
````

#### 9.5.3.2. Body
````
{
    "message": "DoroJuso deleted. gunmul_no:4183031021100660000007779"
}
````

[api_platform_dorojuso_01]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_01.png
[api_platform_dorojuso_02]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_02.png
[api_platform_dorojuso_03]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_03.png
[api_platform_dorojuso_04]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_04.png
[api_platform_dorojuso_05]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_05.png
[api_platform_dorojuso_06]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_06.png
[api_platform_dorojuso_07]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_07.png
[api_platform_dorojuso_08]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_08.png
[api_platform_dorojuso_09]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_09.png
[api_platform_dorojuso_10]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_10.png
[api_platform_dorojuso_11]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_11.png
[api_platform_dorojuso_12]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_12.png
[api_platform_dorojuso_13]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_13.png
[api_platform_dorojuso_14]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_14.png
[api_platform_dorojuso_15]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_15.png
[api_platform_dorojuso_16]:/images/openpaas-application-apiplatform-dorojuso/api_platform_dorojuso_16.png
