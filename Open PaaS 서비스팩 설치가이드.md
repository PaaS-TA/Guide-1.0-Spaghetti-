### 1. 개요
#### 1.1. 문서 개요
##### 1.1.1. 목적

본 문서(서비스팩 가이드)는 전자정부표준프레임워크 기반의 Open PaaS 프로젝트의 서비스팩개발 및 운영시 품질목표를 달성하기 위하여 적용되어야 할 각종 표준 및 척도를 정의할 수 있도록 가이드로 제시하는 문서이다.
이 개발 표준 가이드를 근거로 하여 개발시스템의 품질을 높이고, 서비스팩 개발자들이 상호간의 소스코드에 대한 가독성 및 이해도를 높이며, 해당 표준에 따라 개발함으로써 프로젝트 품질의 일관성을 유지하여 프로젝트 완료 이후의 원활한 시스템 유지보수를 지원할 수 있도록 하는데 그 목적이 있다.

##### 1.1.2. 범위
본 문서의 범위는 전자정부표준프레임워크 기반의 Open PaaS 프로젝트의 서비스팩개발에 대한 내용으로 한정되어 있다.

##### 1.1.3. 참고 자료
http://docs.cloudfoundry.org/services/
http://bosh.io/docs/create-release.html
https://github.com/cloudfoundry/bosh-sample-release
https://github.com/cloudfoundry/cf-mysql-release/
https://github.com/cloudfoundry-community/cf-mysql-java-broker-boshrelease
https://github.com/cloudfoundry-community/cf-mysql-java-broker
http://rubykr.github.io/rails_guides/getting_started.html
http://www.appdirect.com

### 2. Service Broker API 개발 가이드
#### 2.1. 개요
개방형 클라우드 플랫폼 Service API는 Cloud Controller  와 Service Broker  사이의 규약을 정의한다. Broker는 HTTP (or HTTPS) endpoints URI 형식으로 구현된다. 하나 이상의 Service가 하나의 Broker 에 의해 제공 될 수 있고, 로드 밸런싱이 가능하게 수평 확장성 있게 제공 될 수 있다.

#### 2.2. Service Architecture
 
[그림출처]: http://docs.cloudfoundry.org/services/overview.html

Services 는 Service Broker API 라고 불리우는 cloud controller 클라이언트 API를 구현하여 개방형 클라우드 플랫폼에서 사용된다. Services API는 독립적인 cloud controller API의 버전이다.
이는 플랫폼에서 외부 application을 이용 가능하게 한다. (database, message queue, rest endpoint , etc)
2.3.	Service Broker API Architecture
 
[그림출처]: http://docs.cloudfoundry.org/services/api.html

개방형 클라우드 플랫폼 Service API는 Cloud Controller 와 Service Broker 사이의 규약 (catalog, provision, deprovision, update provision plan, bind, unbind)이고 Service Broker 는 RESTful API 로 구현하고 Cloud Controller 에 등록한다.

#### 2.4. Pivotal(Cloud Foundry) Marketplace Model
 
[그림출처]: http://www.slideshare.net/platformcf/cloud-foundry-marketplacepowered-by-appdirect

AppDirect: 클라우드 서비스 marketplace 및 관리 솔루션의 선두 업체이고 많은 글로벌 회사의 marketplace를 구축하였다. (삼성, Cloud Foundry, ETC)
AppDirect는 Cloud Foundry 서비스 중개(brokerage) 기능과 부가 서비스를 제공한다. 

Service Provider 및 Cloud Foundry 통합에 관련 설명
 
[그림출처]: http://www.slideshare.net/platformcf/cloud-foundry-marketplacepowered-by-appdirect
#### 2.5. 개발 가이드
서비스의 구현 방법은 서비스 제공자(Provider) 와 개발자(developer)의 몫이다. 개방형 클라우드 플랫폼은 서비스 제공자가 6가지의 Service Broker API를 구현해야 한다. 이때 2.4 Pivotal Marketplace Model를 이용해서 AppDirect 에서 제공중인 서비스 제공자와 협의 하여 AppDirect 의 중개 기능을 이용해서 제공할수도 있다. 또한 Broker 는 별도의 애플리케이션으로 구현하든지 기존 서비스에 필요한 HTTP endpoint를 추가함으로써 구현 될 수 있다.

본 개발 가이드는 Service Broker 에서 service back-end를 제어하는 방식을 가이드 한다.AppDirect를 사용하는 경우에는 http://go.appdirect.com/request-more-information를 참고하여 개발한다.

Service Broker는 6개의 기본 API 기능이 필요하다. (상세 설명은 각 API 가이드 참고)
Routes(API)	Method	Description
/v2/catalog	GET	서비스 및 서비스 plans 정보 조회
/v2/service_instances/:id	PUT	서비스를 위한 인스턴스 생성
/v2/service_instances/:id	DELETE	이전에 생성된 서비스 인스턴스 삭제
/v2/service_instances/:id	PATCH	이전에 생성된 서비스 인스턴스 Plan 수정
/v2/service_instances/:id/service_bindings/:id	PUT	서비스 사용에 관련 사용자 생성 및 권한 등 설정 정보 생성
/v2/service_instances/:id/service_bindings/:id	DELETE	서비스 사용 설정 정보 삭제

Service Broker API의 두 가지 주요 버전은 현재 개방형 클라우드 플랫폼 v1 및 v2를 지원한다. v1은 사용 되지 않으며 개방형 클라우드 플랫폼의 다음 버전에서 제거 될 수 있으니 Service Broker는 v2로 구현하는 것을 권장한다.
-	버전 정보 (가이드 문서는 2.5 버전을 기준으로 작성함)
CURRENT VERSION	PAST VERSIONS
2.5 (cf-release 209 버전 이상, CLI version 6.12.1)	2.4
	2.3
	2.2
	2.1
	2.0
	v1 (unversioned)
API Version Header 필드: (X-Broker-Api-Version: 2.4)

-	인증
Cloud Controller 는 모든 요청에 HTTP 기본 인증(인증 헤더)을 사용하여 Broker와 인증하여 사용자 이름과 암호를 포함하지 않는 모든 Broker 등록을 거부한다. Broker는 사용자 이름과 암호를 확인하고 자격 증명이 유효하지 않은 경우 401 Unauthorized 메시지를 반환 한다. Cloud Controller에서 추가 보안이 요구되는 경우 SSL을 사용하여 브로커에 접속 지원을 한다.

##### 2.5.1. Catalog API 가이드
서비스 Catalog는 서비스 및 서비스 Plan의 정보를 조회한다. Cloud Controller는 처음에 모든 Broker에서 endpoint를 취득해서 Cloud Controller 데이터베이스에 저장되어 있는 user-facing service catalog를 조회한다. 또한 Cloud Controller는 Broker가 업데이트 될 때 마다 catalog를 업데이트한다.Catalog API를 구현하면 CF CLI를 통해서 Service Broker를 등록 할 수 있다.

1.	Request
1.1.	Route
GET /v2/catalog

1.2.	cURL
curl -H "X-Broker-API-Version: 2.4" http://username:password@broker-url/v2/catalog
예)
curl -H "X-Broker-API-Version: 2.4" http://admin:eaa139af583c@10.30.40.61/v2/catalog

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
200 OK	Response body 정보는 아래에 제공

2.2.	Body (* 필드는 필수)
RESPONSE FIELD	TYPE	DESCRIPTION
services*	array-of-objects	서비스 객체들의 스키마를 정의
   id*	string	카탈로그에 요청시 서비스의 상관 관계를 구별하는 식별자이다.개방형 클라우드 플랫폼에서 unique 해야 하기 때문에 GUID를 사용하기를 권장
   name*	string	카탈로그에 표시되는 service 이름(소문자, 공백없음)
   description*	string	카탈로그에 표시되는 서비스의 설명
   bindable*	boolean	Application에 서비스를 바인드 할수 있는지의 여부
   tags	array-of-strings	Tags는 빌드팩 또는 다른 서비스에서 로직을 변경하지 않고 교체 할 수 있게 서비스를 가능하게하며 서비스의 분류, 속성, 또는 기반 기술을 노출 할 수있는 유연한 메커니즘을 제공(MySQL, relation, redis, key-value, 캐싱, 메시징, AMQP)
   metadata	object	서비스 제공을 위한 메타 데이터의 목록. 상세 설명은 아래 2.3 Service Metadata 참고
   requires	array-of-strings	사용자가 서비스를 제공 하는 권한 목록. 현재는 syslog_drain 권한만 지원함
   plan_updateable	boolean	서비스plans의 다운 그레이드/업그레이드 지원 여부
   plans*	array-of-objects	서비스에 대한 Plans의 스키마를정의
      id*	string	카탈로그에 요청시 Plan의 상관 관계를 구별하는 식별자이고개방형 클라우드 플랫폼에서 unique 해야 하기 때문에 GUID를 사용하기를 권장
      name*	string	카탈로그에 표시되는 plan 이름(소문자, 공백없음)
      description*	string	카탈로그에 표시되는 plan의 설명
      metadata	object	서비스 Plan을 위한 메타 데이터의 목록. 상세 설명은 아래 2.4 Plan Metadata 참고
      free	boolean	유료 서비스를 제공할지 설정.Default는 true
   dashboard_client	object	서비스에 대한 대시 보드 SSO 기능을 활성화하는 데 필요한 데이터를 포함
      id	string	서비스를 이용하고자하는 Oauth2 클라이언트의 ID
      secret	string	대시 보드 토큰 서버 인증에 사용하는공유 secret 정보
      redirect_uri	string	UAA SSO 인증을 위한 서비스 도메인 URL 정보

2.3.	Service Metadata
BROKER API FIELD	TYPE	DESCRIPTION	Cloud Controller API FIELD
name	string	카탈로그에 표시되는 서비스의 이름	label
description	string	서비스에 대한 설명	description
metadata.displayName	string	그래픽 클라이언트에 표시되는 서비스 이름	extra.displayName
metadata.imageUrl	string	이미지 URL	extra.imageUrl
metadata.longDescription	string	서비스 상세 설명	extra.longDescription
metadata.providerDisplayName	string	실제 서비스를 제공하는 기업 이름	extra.providerDisplayName
metadata.documentationUrl	string	서비스를 위한 문서 링크 URL	extra.documentationUrl
metadata.supportUrl	string	서비스 지원 URL	extra.supportUrl

2.4.	Plan Metadata
BROKER API FIELD	TYPE	DESCRIPTION	Cloud Controller API FIELD
name	string	카탈로그에 표시되는 서비스 plan의 이름	name
description	string	카탈로그에 표시되는 서비스 plan 의 설명	description
metadata.bullets	array-of-strings	서비스 plan 정보 목록	extra.bullets
metadata.costs	cost object	통화나 측정 단위 같은 서비스 비용을 설명. 여러 비용이 있다면 사용자에게 해당 비용을 청구(월별청구 + 단건 비용). 아래의 형식 처럼 제공
"costs":[
      {
"amount":{
"usd":99.0,
"eur":49.0
},
"unit":"MONTHLY"
                     },
                     {
"amount":{
"usd":0.99,
"eur":0.49
},
"unit":"1GB of messages over 20GB"
   }
]	metadata.costs
metadata.displayName	string	그래픽 클라이언트에 표시되는 서비스 plan 이름.	extra.displayName

2.5.	Quota Plan
NAME	DESCRIPTION	VALID VALUES	EXAMPLE VALUE
name	Plan을 식별하는데 사용	‘숫자’,’_’ 및 ‘문자’ 형식. Quota plan 이름은 계정내에서 고유해야 함.	silver_quota
memory_limit	허용되는 최대 메모리 사용량 (MB)	integer	2048
non_basic_services_allowed	유료 서비스를 제공할지 설정. false 로 설정하면 marketplace 에는 표시 되지만 인스턴스는 제공되지 않음	true/false	true
total_routes	허용되는 최대 라우터 개수	integer	500
total_services	허용되는 서비스 개수	integer	25
trial_db_allowed	기존 필드. 값은 무시	true/false	true

# sample body response message
{
  "services": [
    {
      "id": "44b26033-1f54-4087-b7bc-da9652c2a539",
      "name": "p-mysql",
      "description": "A MySQL service for application development and testing",
      "tags": [
        "mysql"
      ],
      "metadata": {
        "displayName": "MySQL for Pivotal CF",
        "imageUrl": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAABoxJREFUeNrkW01sG0UUnkQ90FNNOfVHilvKBQS2iASHVtiCHkAqxI3ECZG4KOJGnOZQxCVxeuHnEJz0BlVjF3FCyg9UooeKOogeqBTkOEov/DlS056a2hfCASm8b3izXY/XZr07Xq/hSSs7jr2775v3vu+92Zm+vb098X+2fR2/wAeZCL0k6YjzEeHXA9pXa3SU+H2R35f++nCu0sn76+tEBJDTcDDNjsd8nm6LjmU68gRGKdQAkONwesKA063AyDEY1dAAwI5n6RgIKHVrDETOLxC+ACDHEeL5AB13AiJNICwHCgATG0Y8ExIyX2Egqh0HgAku38E89xMNKQKh2M6P+tt0PsUSFTbnBcvqTeYj8wDwiZcc9DtstkD3mjWaAuz8Qo8VeXOUDhO+AeCwX+rRSvccgZD3DAATXrEHwr6VnW0lk00BYKkrdVHjTapDvFlP0aoZygXhfOzQETH0zHMi+vhBecAqD3fExRvfyldD6pDnvsRdBHCFd9PE1SOP7Rexw0esv1d/+8V6f+XNt8TQ08+KlTsbYv3etijdvyu/j8+rf+6KJz+eMYn1eYqCnFsAKn5GHyM5dfo1kTh+QgxEDorvf//H6QH6HH/Dyve35evLn12SzupRsTZ+Qbzy+aU6wAykQlSvFvc5OO+5qcHowfHRwRdEYe22GL56Wayzo05R0cy5GgNi0HmVCjlu050jgImv4oX14dR3774nR/Odr770lb+Zk0kJEM7TATtmJ0S9Ekx5lbzFkTHK4W0Z0n7Jq/LwgUwfRYqGbaJVKZz1csbRwRdlfk9+s2imtSNSXNnckDyAcxu2NEd6PQBc9HjKfTD5xRvXG8jMj01eWxTDX1wWs2fOSlVAihnkgpRTBKS9nvENSNlm2fed6WEPEoQUxkkVwC8GQXAEIOXnjCZGHw6PUMjPnhm2gMB5wSuw2deHTQEwVAcAhX/Ua/hj1Mqa1GGk4AT0vF1DBTh/qygW3x6TkmoHAdcyxQlc7FkRkPQjWUVNrzFS4ycTYoFy15sK7Ijn5z+RUXCFz6HkFZxgKBXqAIh7LXowKhi1OgIjNUD1d86njqs6QIGAFEFhZSgKvAEQY0L69f1pOUKDNFJ6/quQdaoCvYIApZGzHD8UxfiphAkAonYAIq5D/lRSlKhxgeN+Kz7XkkgRheuqGgHRZ6BIGrAD4HqSExdeuVMOxHF7RM1QmilSRSokjj9lhgzb/QEI71Ni+Llbq7JkdWtod5tJJXhEOrq72zRt4LQCACW3F4VxUoK2AQDh1ehGkZOR/e7ZGDeMokYHAXKZJADwuWqXIasAuLD2Yz2IDM76vbtWSgQeAZKISKdxtGNg8nG6aV0x4tT1naey1976AtxpUpgMSanTfIHJkrs/qDwGe8MhnbzAJfFDRxuaIdQBCHUoTictMAAQvpgkQYWnA3CgSSpBZfC/DnSE1uUVAFtBgIAOT84dEAiqmkNEgFOac851qwYwbZgYUQBUgooE5DTIcy1zwZoU1cmunv1/tlTikRQ/IRXDwByhRYIAIBEEAKpKhOOxw0cdSa4hRbTaH2pR8l9lluwA4I9REaDJWR86/q3TdBppyCZI1SQARREim2IJxMhvVXfE1Z9u1/3/pWMn5IyzMQCw+oqqopoIwTNAtNeYVkchNE+jrKeHmn8wUAss64XQctBp4GTo9CB/hp8JNKiyekDSryMSBmvF8OgpQII+a4N8QyHEj5Br3XZeFksjY9JBXf6UiiA9pk6/6jv8nSrBfLcBQK+A/h8yia7zwfRHDaMNbkAh5QSQGwGyPxnSmyE8O+v60je7RMJJVI7V3T+szxAFPkgw17QXYGQKYZJEkOHktSU50arKZ/kkitrmrfYnZVb1ZXROzVA2DFxQFxGbZek85iHhPPL/a4oGD7NSWf2DZusD8MXpMIGg1hzIKbnNjbbnIxDZNPppVwAwCKiUwrgg0mvjE3VaSttqPiAdtlTwYalm64j7W/TKiICJ/4DzM63WD7tZKBkKafRaVznlvdsUUJEwETZpdFvvu4lgV3OCjGKhx5xPutk/4HpStIdAKLh13hUHOHACgAjrynEQXradH3jdMhPnjios64g97x3y9FyAJRIgzIUk5KNeN0753jbHy2vQRicCdnwVtX27e4SMA2ADIsnV42gvOG4cAC0iUgyGqV5CbZ/Nmd5L3NfJ3eMMRlI82jjtNk2g4+AZjHKxkxuo+7qxfV4tUXOwkqk9wW7tbwEGAJwbJQSR3aMDAAAAAElFTkSuQmCC",
        "longDescription": "Provisioning a service instance creates a MySQL database. Binding applications to the instance creates unique credentials for each application to access the database.",
        "providerDisplayName": "Pivotal Software",
        "documentationUrl": "http://docs.gopivotal.com/",
        "supportUrl": "http://gopivotal.com/support/"
      },
      "plans": [
        {
          "id": "ab08f1bc-e6fc-4b56-a767-ee0fea6e3f20",
          "name": "100mb-dev",
          "description": "Shared MySQL Server",
          "metadata": {
            "costs": [
              {
                "amount": {
                  "usd": 0
                },
                "unit": "MONTH"
              }
            ],
            "bullets": [
              "Not for production use - server is not replicated",
              "Shared MySQL server",
              "100 MB storage",
              "40 concurrent connections"
            ],
            "displayName": "(( merge || \"100 MB Dev\" ))"
          }
        }
      ],
      "bindable": true,
      "dashboard_client": {
        "id": "p-mysql",
        "secret": "eaa139af583c",
        "redirect_uri": "http://10.30.40.61/"
      }
    }
  ]
}

3.	Catalog Rest API 구현
3.1.	JAVA 방식
-- CatalogRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/catalog")         // Spring 어노테이션을 사용
class CatalogRestController {
  def settings;

@RequestMapping(method=RequestMethod.GET)
  @ResponseBody
  synchronized Map getCatalog() {
    if (!settings) {
      Yaml yaml = new Yaml(); 
      // settings.yml 파일 안에 서비스 정보와 plan 정보가 들어 있음
      settings = yaml.load(this.class.getClassLoader().getResourceAsStream("settings.yml"));
    }

    return settings;
  }

}

-- settings.yml 파일

services:
- name: p-mysql
  id: 3101b971-1044-4816-a7ac-9ded2e028079
  description: MySQL service for application development and testing
  tags:
    - mysql
    - relational
  max_db_per_node: 250
  metadata:
    provider:
      name:
    listing:
      imageUrl: ~
      blurb: MySQL service for application development and testing
  plans:
  - name: 5mb
    id: 2451fa22-df16-4c10-ba6e-1f682d3dcdc9
    description: Shared MySQL Server, 5mb persistent disk, 40 max concurrent connections
    max_storage_mb: 5 # in MB
    metadata:
      cost: 0.0
      bullets:
      - content: Shared MySQL server
      - content: 5 MB storage
      - content: 40 concurrent connections

3.2.	Ruby 방식(Ruby on Rails)
-- 어플리케이션을 만들 때 레일즈(rails)을 이용 해서 새로운 어플리케이션을 위한 기본 생성 구조를 만든다. 아래 표 참고
$ rails new<broker_name>

파일/폴더	목적
Gemfile	레일즈 어플리케이션에게 필요한 Gem의 의존성 정보를 기술하는데 사용
README	어플리케이션을 위한 설명 (설치, 사용 방법 기술에 쓰임)
Rakefile	터미널에서 실행할 수 있는 배치잡들을 포함
app/	어플리케이션을 위한 컨트롤러, 모델, 뷰를 포함
config/	어플리케이션의 실행 시간의 규칙, 라우팅, 데이터베이스 등 설정을 저장
config.ru	랙(Rack) 기반의 서버들이 시작할때 필요한 설정
db/	현재 데이터베이스의 스키마를 포함(데이터베이스 마이그레이션으로 잘 알려져 있음)
doc/	어플리케이션에 대한 자세한 설명 문서
lib/	어플리케이션을 위한 확장 모듈
log/	어플리케이션의 로그 파일
public/	외부에서 볼수 있는 유일한 폴더(이미지, 자바스크립트, 스타일시트나 그외 정적인 파일)
script/	레일즈 스크립트 및 어플리케이션을 실행시키거나, 배포, 실행 관련한 스크립트 파일
test/	유닛 테스트, 픽스쳐, 그외 다른 테스트 도구
tmp/	임시 파일
vendor/	서드 파티 코드들을 위한 공간. 일반적인 레일즈 어플리케이션은 루비 젬과 레일즈 소스, 프로젝트 내에 설치시와 미리 패키징된 추가 플러그인들이 위치

-- config/routes.rb : posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
    patch 'service_instances/:id' => 'service_instances#set_plan'
    resources :service_instances, only: [:update, :destroy] do
      resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/catalogs_controller.rb)

class V2::CatalogsController < V2::BaseController
  def show
    render json: {
      services: services.map {|service| service.to_hash }
    }
  end

  private

  def services
    (Settings['services'] || []).map {|attrs| Service.build(attrs)}
  end
end

3.3.	Node.js 방식
-- express 라는 Node.js 에서 가장 많이 사용하는 웹 프레임워크 모듈을 이용해서 Rest API 를 만든다.

# sample (app.js)

var express = require('express')
  , http = require('http')
  , app = express()
  , server = http.createServer(app);

app.get('/v2/catalog ', function (req, res) {
// catalog 기능 구현
});

server.listen(8000, function() {   // 포트 설정
  console.log('Express server listening on port ' + server.address().port);
});

4.	서비스 별 Catalog API 개발 명세
Catalog API 경우에는 서비스의 종류와 관계없이 Service 및 Plan 정보를 저장되어 있는 settings.yml 파일이나 기타 메타 파일 또는 소스 안에 정보를 저장한 후 제공한다. 만일 AppDirect 를 이용하는 경우는 Catalog 정보를 조회해오는 AppDirect API를 호출하여 그 결과를 제공한다. 샘플 settings.yml 파일은 3. Catalog Rest API 구현 참고.

# Pivotal 서비스 Plan 예시
- clearDB plan 예
[그림출처] :http://run.pivotal.io/
 

 

 

 


# Pivotal 서비스 Dashboard 예시
- clearDB Dashboard 예
[그림출처] :https://www.cleardb.com/
 
 


2.5.2.	Provision API 가이드
Broker가 Cloud Controller로 부터 provision 요구를 수신하면 개발자를 위한 새로운 서비스 인스턴스를 생성한다. provision 시 서비스들의 종류에 따라 provision 결과는 다르다.
Mysql DataBase 인 경우에는 새로운 DATABASE 스키마를 생성한다. 또한 non-data 서비스 인 경우의 provision은 기존 시스템에 계정을 얻는 의미 일 수도 있다. 자세한 내용은 아래에 각 서비스별 provision을 참고한다.

1.	Request
1.1.	Route
PUT /v2/service_instances/:instance_id
참고: 서비스 인스턴스의 instance_id는 Cloud Controller에 의해 제공된다. 이 ID는 인스턴스 삭제, 바인드 및 바인드 해지에 사용된다.

1.2.	cURL
$ curl http://username:password@broker-url/v2/service_instances/:instance_id -d '{
  "service_id":        "service-guid-here",
  "plan_id":           "plan-guid-here",
  "organization_guid": "org-guid-here",
  "space_guid":        "space-guid-here"
}' -X PUT -H "X-Broker-API-Version: 2.4" -H "Content-Type: application/json"

1.3.	Body
REQUEST FIELD	TYPE	DESCRIPTION
service_id*	string	카탈로그 내의 서비스의 ID는 카탈로그 endpoint 에서 사용자가 provision 할 때 필요한 고유 식별자
plan_id*	string	서비스 내의 plan ID 는 카탈로그 endpoint 에서 사용자가 provision 할 때 필요한 고유 식별자
organization_guid*	string	Cloud Controller 에서 사용자가 사용하는 ORG GUID 값
space_guid*	string	organization_guid 필드와 같이SPACE GUID 값
parameters	JSON object	JSON 형태의 파라미터 값을 제공

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
201 Created	서비스 인스턴스 생성(Response body 정보는 아래에 제공)
200 OK	서비스 인스턴스가 이미 존재하고 요청 된 매개 변수가 기존의 서비스 인스턴스와 동일한 경우 (Response body 정보는 아래에 제공)
409 Conflict	요청 된 서비스의 인스턴스가 이미 존재하는 경우 반환. 에러 메시지 형태는 {} 안에 description 필드를 사용
예) 
{
  "description": "Something went wrong. Please contact support at http://support.example.com."
}

2.2.	Body 
모든 응답 bodies 는 JSON Object ({}) 형식으로 한다.
RESPONSE FIELD	TYPE	DESCRIPTION
dashboard_url	string	서비스 인스턴스에 대한 웹 기반 대시보드 유저 인터페이스의 URL이다. 사용자가 SSO를 통해 서비스 대시보드 인증 할 수있는 방법에 대한 내용은 2.3 Dashboard Single Sign-On 참고
예)
{
 "dashboard_url": "http://mongomgmthost/databases/9189kdfsk0vfnku"
}

2.3.	Dashboard Single Sign-On.
Single Sign-On (SSO)는 개방형 클라우드 플랫폼 사용자들이 개방형 클라우드 플랫폼 자격 증명을 사용하여 third-party 서비스의 대시 보드에 접근한다. 서비스 대시 보드는 서비스가 제공하는 기능의 일부 또는 전부를 사용할 수 있는 웹 인터페이스이다. SSO는 반복되는 로그인과 여러 서비스의 계정을 통합 관리한다. OAuth2 프로토콜 인증을 처리하기 때문에 사용자의 자격 증명은 직접 서비스로 전송하지 않는다. SSO 기능을 사용하려면 Cloud Controller UAA client 에 서비스 브로커의 생성 및 삭제 할 수 있는 권한이 있어야 한다. 이 클라이언트는 개방형 클라우드 플랫폼 설치시 구성한다. (설치 문서 참고)

# CF 설치시 Dashboard SSO 설정 예)
properties:
    uaa:
      clients:
        cc-service-dashboards:
          secret: cc-broker-secret
          scope: openid,cloud_controller_service_permissions.read
          authorities: clients.read,clients.write,clients.admin
          authorized-grant-types: client_credentials

3.	Provision Rest API 구현
3.1.	JAVA 방식
-- ServiceInstanceRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/service_instances/{id}")
class ServiceInstanceRestController {
  @Autowired
private ServiceInstanceService service;

  @RequestMapping(method = RequestMethod.PUT)
  @ResponseBody
  Map update(@PathVariable String id) {
ServiceInstance instance = service.findById(id);   // Spring 프레임워크 사용으로 서비스 구현
    if (!service.isExists(instance)) {
service.create(instance);        // 서비스 인스턴스를 생성하는 부분 (개발 명세 내용 구현)
    }
    return [:];

  }
}

3.2.	Ruby 방식(Ruby on Rails)
-- config/routes.rb : 라우팅 정보를 담은 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
patch 'service_instances/:id' => 'service_instances#set_plan'
    resources :service_instances, only: [:update, :destroy] do
      resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/service_instances_controller.rb)

class V2::ServiceInstancesController < V2::BaseController

  # This is actually the create
  def update
// 서비스 instance 생성 기능 구현 (개발 명세 내용 구현)
  end

end

3.3.	Node.js 방식
# sample (app.js) : Catalog API 참고

var router = express.Router();

router.route('/v2/service_instances/:id’)

.put(function(req, res, next) {
// 서비스 instance 생성 기능 구현 (개발 명세 내용 구현)

})


4.	서비스 별 Provision API 개발 명세
-	인스턴스 생성시 unique 한 이름으로 만든다.
-	생성 요청한 인스턴스 ID 가 이미 존재 하는지 체크한다.
-	선택 한 plan 정보로 인스턴스가 생성 가능한지 체크 하고 가능할 경우 해당 인스턴스를 만든다.
-	인스턴스 생성이 완료 되면 위에서 기술된 JSON Object 형식으로 Cloud Controller 에 전송한다.

4.1.	RDBMS
1. Mysql 경우

- 생성할 데이터 베이스가 존재 하는지 체크 
SHOW DATABASES LIKE '${instance.database}'

- 새로운 데이터 베이스 생성
CREATE DATABASE IF NOT EXISTS ${instance.database}

- 생성 후 Dashboard 정보를 JSON Object 형식으로 Cloud Controller 에 전송.

2. Cubrid DB 경우 

- 데이터 베이스 생성할 디렉토리 생성 및 이동
$ mkdir <databasename>
$ cd <databasename>

- 데이터 베이스 생성
$ cubrid created--db-volume-size=100M --log-volume-size=100M <databasename> en_US

- 데이터 베이스 실행
$ cubrid service start
$ cubrid server start <databasename>

4.2.	대용량 저장소
1. GlusterFS 경우
# GlusterFS 로 파일을 업로드 하기 위해서는 먼저 GlusterFS 와 OpenStack swift 로 service back-end로 구성하여 Object Storage 방식으로 파일을 업로드 다운로드를 할수 있게 제공한다. (아마존 S3 방식과 유사)

- 새로운 Swift Account 를 생성

Method  : PUT 

Req URL : http(s)://[IP Address OR HostName]/auth/v2/[AccountID]

Header  : X-Auth-Admin-User: .super_admin
	  X-Auth-Admin-Key: swauthkey
          X-Account-Meta-Quota-Bytes: [Size(Byte)]


4.3.	NoSQL DB
1. mongoDB 경우

- 새로운 데이터 베이스를 생성
>use <databasename>
switched to db <databasename>

2.5.3.	Update Instance API 가이드
Update Instance API는 기존의 서비스 인스턴스의 plan를 수정 한다. 즉 서비스 인스턴스의 plan을 업그레이드나 다운그레이드 한다.
이 기능을 사용하려면 브로커는 카탈로그 endpoint 에서 “plan_updateable: true”설정 해주어야 한다. 이 옵션 필드가 포함되어 있지 않은 경우에는 service plan 변경 요청에 대해 의미 있는 오류를 반환하고 브로커는 API 호출을 하지 않는다. 이 필드가 포함된 경우 개방형 클라우드 플랫폼의 모든 plan 변경 요청에 브로커로 API 호출을 수행하며 브로커에서는 plan 지원 여부를 확인한다.

1.	Request
1.1.	Route
PATCH /v2/service_instances/:instance_id
참고: instance_id는 이전에 provision 서비스 인스턴스의 GUID

1.2.	cURL
$ curl http://username:password@broker-url/v2/service_instances/:instance_id -d '{
  "plan_id": "plan-guid-here"
}' -X PATCH -H "X-Broker-API-Version: 2.4" -H "Content-Type: application/json"

1.3.	Body
REQUEST FIELD	TYPE	DESCRIPTION
plan_id	string	변경할 새로운 plan ID
service_id*	string	카탈로그 내의 서비스의 ID는 카탈로그 endpoint 에서 사용자가 provision 할 때 필요한 고유 식별자
parameters	JSON object	JSON 형태의 파라미터 값을 제공
previous_values	object	업데이트하기 전 인스턴스에 대한 정보
previous_values.plan_id	string	업데이트하기 전 plan ID
previous_values.service_id	string	업데이트하기 전 서비스 ID
previous_values.organization_id	string	업데이트하기 전 조직 ID
previous_values.space_id	string	업데이트하기 전 스페이스 ID

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
200 OK	요청한 plan 으로 변경하고 응답 본문은 "{}" 으로 전송
422 Unprocessable entity	요구 된 특정 plan 변경이 지원되지 않는 경우. (예. 인스턴스 사용률이 요청한 계획의 할당량을 초과)
에러 메시지 형태는 {} 안에 description 필드를 사용.
예) 
{
  "description": "Something went wrong. Please contact support at http://support.example.com."
}

2.2.	Body 
모든 응답 bodies 는 JSON Object ({}) 형식으로 한다.

3.	Update Service instance Rest API 구현
3.1.	JAVA 방식
-- ServiceInstanceRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/service_instances/{id}")
class ServiceInstanceRestController {
  @Autowired
  private ServiceInstanceService service;

  @RequestMapping(method = RequestMethod.PATCH)
  @ResponseBody
  Map updateInstance(@PathVariable String id) {
    ServiceInstance instance = service.findById(id);   // Spring 프레임워크 사용으로 서비스 구현
    if (!service.isExists(instance)) {
      service.update(instance);        // 서비스 인스턴스를 정보 수정하는 부분 (개발 명세 내용 구현)
    }
    return [:];

  }
}

3.2.	Ruby 방식(Ruby on Rails)
-- config/routes.rb : posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
patch 'service_instances/:id' => 'service_instances#set_plan'
resources :service_instances, only: [:update, :destroy] do
      resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/service_instances_controller.rb)

class V2::ServiceInstancesController < V2::BaseController

def set_plan
// 서비스 인스턴스 plan 정보 업데이트
  end

end

3.3.	Node.js 방식
# sample (app.js) : Catalog API 참고

var router = express.Router();

router.route('/v2/service_instances/:id’)

.patch(function(req, res, next) {
  // 서비스 instance 수정 기능 구현 (개발 명세 내용 구현)

})

4.	서비스 별 Update Service instance API 개발 명세
4.1.	공통
1.	현재 제공 중인 plan 정보와 변경 요청 받은 plan 정보가 다른지 체크한다.

2.	다운 그레이드 할 경우 이미 사용하는 용량이 다운 그레이드 할 용량보다 클 경우 에러를 발생시킨다.

3.	업 그레이드 할 경우 plan 정보를 업데이트 한다. (예: DBMS 서비스 경우 connection 수, storage 용량)

4.	변경된 내용을 Cloud Controller 전달 한다.

2.5.4.	Deprovision API 가이드
브로커가 개방형 클라우드 플랫폼으로부터 deprovision 요청을 수신 할 때 provision 생성시 제공했던 모든 리소스를 삭제한다.

1.	Request
1.1.	Route
DELETE /v2/service_instances/:instance_id

1.2.	Parameters
QUERY-STRING FIELD	TYPE	DESCRIPTION
service_id*	string	카탈로그에 있는 서비스 ID.
plan_id*	string	카탈로그에 있는 Plan ID

1.3.	cURL
$ curl 'http://username:password@broker-url/v2/service_instances/:instance_id?service_id=
    service-id-here&plan_id=plan-id-here' -X DELETE -H "X-Broker-API-Version: 2.4"

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
200 OK	서비스 인스턴스를 삭제. “{}” 메시지를 응답
410 Gone	서비스 인스턴스가 존재 하지 않을 경우. “{}” 메시지를 응답

2.2.	Body 
모든 응답 bodies 는 JSON Object ({}) 형식으로 한다.
성공시 “{}” 값을 전송받는다.

3.	Deprovision Rest API 구현
3.1.	JAVA 방식
-- ServiceInstanceRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/service_instances/{id}")
class ServiceInstanceRestController {
  @Autowired
private ServiceInstanceService service;

@RequestMapping(method = RequestMethod.DELETE)
  @ResponseBody
  Map destroy(@PathVariable String id) {
    ServiceInstance instance = service.findById(id);
    if (service.isExists(instance)) {
      service.delete(instance); // 서비스 instance 삭제 기능 구현 (개발 명세 내용 구현)
    }
    return [:]
  }
}

3.2.	Ruby 방식(Ruby on Rails)
-- config/routes.rb : posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
patch 'service_instances/:id' => 'service_instances#set_plan'
    resources :service_instances, only: [:update, :destroy] do
      resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/service_instances_controller.rb)

class V2::ServiceInstancesController < V2::BaseController

  def destroy
// 서비스 instance 삭제 기능 구현 (개발 명세 내용 구현)
  end

end

3.3.	Node.js 방식
# sample (app.js) : Catalog API 참고

var router = express.Router();

router.route('/v2/service_instances/:id’)

.delete(function(req, res, next) {
// 서비스 instance 삭제 기능 구현 (개발 명세 내용 구현)

})


4.	서비스 별 Deprovision API 개발 명세
-	삭제 요청하는 서비스 인스턴스가 존재 하는지 체크한다.
-	인스턴스가 존재 하면 삭제할 인스턴스에 Application 이 bind 되어 있는지 체크 한다.
-	만일 bind 되어 있는 Application 이 존재 할 경우 Error 를 Cloud Controller 에 전송한다.
-	bind 되어 있는 Application 이 존재 하지 않을 경우 해당 서비스 인스턴스를 삭제한다.

4.1.	RDBMS
1. Mysql 경우

- 데이터 베이스 삭제
DROP DATABASE IF EXISTS #{connection.quote_table_name(database_name)}

2. Cubrid DB 경우 

- 서비스 종료 후 데이터베이스 제거
$ cubrid service stop
$ cubrid deletedb <databasename>

- 제거한 데이터베이스 디렉터리 제거
$ rm –rf <database 설치 path>/<databasename>


4.2.	대용량 저장소
1. GlusterFS 경우

- Swift Account 를 삭제

Method  : DELETE 

Req URL : http(s)://[IP Address OR HostName]/auth/v2/[AccountID]

Header  : X-Auth-Admin-User: .super_admin
	  X-Auth-Admin-Key: swauthkey

4.3.	NoSQL DB
1. mongoDB 경우

- 데이터 베이스 삭제
>use <databasename>
switched to db <databasename>
>db.dropDatabase()
>{ "dropped" : "<databasename>", "ok" : 1 }
>


2.5.5.	Bind API 가이드
Provision만으로 서비스를 사용할 수 있을 경우에는 bind 기능 구현은 필요 없고 결과 성공 메시지만 개방형 클라우드 플랫폼에 전송하면 된다. 브로커가 개방형 클라우드 플랫폼으로부터 바인딩 요청을 수신 할 때 프로비저닝 된 자원을 활용하는데 필요한 정보를 반환한다. 해당 정보는 credentials(자격증명)안에 제공된다. Applicatoin에 고유한 credentials(자격증명)을 발급하여다른  Application에는 영향을 주어서는 안된다.

1.	Request
1.1.	Route
PUT /v2/service_instances/:instance_id/service_bindings/:binding_id
참고: binding_id는 서비스 바인딩을 하기 위해 Cloud Controller에 의해 제공된다. binding_id는 향후 바인딩 해제 요청에 사용된다.

1.2.	cURL
$ curl http://username:password@broker-url/v2/service_instances/
:instance_id/service_bindings/:binding_id -d '{
  "plan_id":        "plan-guid-here",
  "service_id":     "service-guid-here",
  "app_guid":       "app-guid-here"
}' -X PUT -H "X-Broker-API-Version: 2.4" -H "Content-Type: application/json"

1.3.	Body
REQUEST FIELD	TYPE	DESCRIPTION
service_id*	string	카탈로그 내의 서비스의 ID
plan_id*	string	서비스 내의 plan ID
app_guid*	string	서비스 바인드를 할 Application 의 GUID
parameters	JSON object	JSON 형태의 파라미터 값을 제공

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
201 Created	바인딩 생성.Response body 정보는 아래에 제공
200 OK	서비스 바인딩이 이미 존재하고 요청 된 매개 변수가 기존의 바인딩과 동일한 경우. Response body 정보는 아래에 제공
409 Conflict	요청 된 바인딩이 이미 존재하는 경우 반환. 에러 메시지 형태는 {} 안에 description 필드를 사용
예) 
{
  "description": "Something went wrong. Please contact support at http://support.example.com."
}
422 Unprocessable Entity	브로커가 요청 본문에 app_guid를 포함 할 것을 요구하는 경우
예) 
{ 
"error": "RequiresApp", "description": "This service supports generation of credentials through binding an application only." 
}
다른 상태코드(Status Code) 응답은 실패를 의미한다.

2.2.	Body 
모든 응답 bodies 는 JSON Object ({}) 형식으로 한다.
RESPONSE FIELD	TYPE	DESCRIPTION
credentials	object	Application이 서비스에 접근할수 있는 credentials 정보. 해시 형태로 제공. 자세한 정보는 2.3 Binding Credentials를 참고
syslog_drain_url	string	개방형 클라우드 플랫폼에 bound 된 Application에 대한 로그 URL. 자세한 정보는 2.4 Application Log Streaming 을 참고

2.3.	Binding Credentials 
서비스 바인딩 경우 바인드 API 호출에 응답하여 사용자가 Application에서 사용 할 수있는 인증 정보를 반환한다. 개방형 클라우드 플랫폼 환경 변수 VCAP_SERVICES에 이러한 자격 증명을 제공한다. 가능하면 credentials(자격증명) 필드 목록에서 사용하기를 권장한다. 필요에 따라 추가 필드를 제공 할 수 있지만 제공되는 필드로 사용자의 요구 사항을 충족하는 경우 해당 필드를 사용한다.

중요: 연결 문자열(connection string)을 지원하는 서비스를 제공하는 경우 적어도 uri 키를 제공해야한다.위에서 언급 한 바와 같이 또한 별도의 자격 증명 필드를 제공 할 수있다. Buildpacks 및 Application 라이브러리는 uri 키를 사용한다.
CREDENTIALS	DESCRIPTION
uri	dbtype 같은 연결 문자열(Connection string)의 형태는 아래와 같다. dbtype://username:password@hostname:port/name
dbtype: mysql, postgres, mongodb, amqp, etc.
hostname	서버 호스트의 FQDN(Full Qualified Domain Name)
port	서버 호스트의 포트 번호
name	서비스 인스턴스 이름: 예) database name
vhost	메시징 서버의 가상 호스트의 이름 (AMQP 공급자에서 특정 이름 교체)
username	서버 사용자
password	서버 사용자 비밀번호

# Example VCAP_SERVICES 결과
VCAP_SERVICES=
{
  cleardb: [
    {
      name: "cleardb-1",
      label: "cleardb",
      plan: "spark",
      credentials: {
        name: "ad_c6f4446532610ab",
        hostname: "us-cdbr-east-03.cleardb.com",
        port: "3306",
        username: "b5d435f40dd2b2",
        password: "ebfc00ac",
        uri: "mysql://b5d435f40dd2b2:ebfc00ac@us-cdbr-east-03.cleardb.com:3306/ad_c6f4446532610ab",
        jdbcUrl: "jdbc:mysql://b5d435f40dd2b2:ebfc00ac@us-cdbr-east-03.cleardb.com:3306/ad_c6f4446532610ab"
      }
    }
  ],
  cloudamqp: [
    {
      name: "cloudamqp-6",
      label: "cloudamqp",
      plan: "lemur",
      credentials: {
        uri: "amqp://ksvyjmiv:IwN6dCdZmeQD4O0ZPKpu1YOaLx1he8wo@lemur.cloudamqp.com/ksvyjmiv"
      }
    }
    {
      name: "cloudamqp-9dbc6",
      label: "cloudamqp",
      plan: "lemur",
      credentials: {
        uri: "amqp://vhuklnxa:9lNFxpTuJsAdTts98vQIdKHW3MojyMyV@lemur.cloudamqp.com/vhuklnxa"
      }
    }
  ],
  rediscloud: [
    {
      name: "rediscloud-1",
      label: "rediscloud",
      plan: "20mb",
      credentials: {
        port: "6379",
        host: "pub-redis-6379.us-east-1-2.3.ec2.redislabs.com",
        password: "1M5zd3QfWi9nUyya"
      }
    },
  ],
}

2.4.	Application Log Streaming 
개방형 클라우드 플랫폼은 서비스 인스턴스에 바인딩 된 Application에 대한 로그를 스트리밍 한다. 서비스 인스턴스에 바인딩 된 모든 Application에 대한 로그는 해당 인스턴스로 스트리밍된다.

동작하는 방법은 아래와 같다.
1.	브로커는 바인드에 대한 응답으로 syslog_drain_url에 대한 값을 반환한다.
2.	Application이 재구동 할 때 VCAP_SERVICES 안의 syslog_drain_url 의 key와 value 를 갱신한다.
3.	DEAs는 지속적으로 Loggregator에서 Application 로그를 스트리밍한다.
4.	VCAP_SERVICES 안에 syslog_drain_url이 존재하면 DEA는 그 로그에 그 필드를 태그한다.
5.	Loggregator는 값으로 지정된 위치에 이 key를 태그하여 로그 스트리밍한다.


3.	Bind Rest API 구현
3.1.	JAVA 방식
-- ServiceBindingRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/service_instances/{instanceId}/service_bindings/{bindingId}")
class ServiceBindingRestController {
  @Autowired ServiceBindingService bindingService;

@RequestMapping(method = RequestMethod.PUT)
  @ResponseBody
  ServiceBinding update(@PathVariable String instanceId, @PathVariable String bindingId) {
    ServiceBinding binding = bindingService.findById(bindingId, instanceId);
    bindingService.save(binding);     // 서비스 바인드 기능 구현 (개발 명세 내용 구현)
    return binding;
  }

}

3.2.	Ruby 방식(Ruby on Rails)
-- config/routes.rb : posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
    patch 'service_instances/:id' => 'service_instances#set_plan'
    resources :service_instances, only: [:update, :destroy] do
resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/service_bindings_controller.rb)

class V2::ServiceBindingsController< V2::BaseController

  def update
// 서비스 bind 기능 구현 (개발 명세 내용 구현)
  end

end

3.3.	Node.js 방식
# sample (app.js) : Catalog API 참고

var router = express.Router();

router.route('/v2/service_instances/:instanceId/service_bindings/:bindingId’)

.put(function(req, res, next) {
// 서비스 bind 기능 구현 (개발 명세 내용 구현)

})


4.	서비스 별 Bind API 개발 명세
-	Bind 할 서비스 인스턴스가 존재 하는지 체크 한다.
-	Application 에 bind 할 정보를 생성하여 Application 에 전달한다.
-	이때 bind 정보는 랜덤하게 생성하고 base64 인코딩해서 보낸다.

4.1.	RDBMS
1. Mysql 경우

- 데이터 베이스에 접속할 사용자를 생성한다.
CREATE USER #{username} IDENTIFIED BY #{password}

- provision 시 생성한 데이터 베이스에 생성한 사용자가 사용 가능하게 권한을 주고 plan에 해당하는 connection 수를 제공한다.

GRANT ALL PRIVILEGES ON #{databasename}.* TO #{username}@'%'WITH MAX_USER_CONNECTIONS #{max_user_connections}

- 서버에 권한 테이블을 재배치한다.
FLUSH PRIVILEGES

2. Cubrid DB 경우 

- 데이터 베이스에 접속할 사용자를 생성한다.
CREATE USER #{username};

참고: Cubrid DB에서 권한 부여의 최소 단위는 테이블이다. 자신이 만든 테이블은 모든 접근을 허용한다.

4.2.	대용량 저장소
1. GlusterFS 경우

- 새로운 Swift User 를 생성

Method  : PUT 

Req URL : http(s)://[IP Address OR HostName]:[PORT]/auth/v2/[AccountID]/[UserId]

Header  : X-Auth-Admin-User: .super_admin
	  X-Auth-Admin-Key: swauthkey
	  X-Auth-User-Key: [Password]
	  X-Auth-User-Admin: [true OR false]

4.3.	NoSQL DB
1. mongoDB 경우

- 데이터 베이스에 접속할 사용자를 생성하고 접근 role(read, Write)을 부여한다.
>use <databasename>
switched to db <databasename>
>db.getSiblingDB("<databasename>").runCommand(
 { createUser: "<username>",
 pwd: "<password>",
roles: [
"readWrite"
         ]
   }
 )

2.5.6.	Unbind API 가이드
참고: 바인딩 서비스를 제공하지 않는 브로커는 Unbind API를 구현할 필요가 없다.
브로커가 개방형 클라우드 플랫폼으로부터 unbind 요청을 받으면 바인드(bind)에서 만든 모든 자원(resource)을 삭제한다. 삭제 되면 service에 접근 할수 없다.

1.	Request
1.1.	Route
DELETE /v2/service_instances/:instance_id/service_bindings/:binding_id

1.2.	Parameters
QUERY-STRING FIELD	TYPE	DESCRIPTION
service_id*	string	카탈로그에 있는 서비스 ID
plan_id*	string	카탈로그에 있는 Plan ID

1.3.	cURL
$ curl 'http://username:password@broker-url/v2/service_instances/:instance_id/
service_bindings/:binding_id?service_id=service-id-here&plan_id=plan-id-here' -X DELETE -H "X-Broker-API-Version: 2.4"

2.	Response
2.1.	Status Code
STATUS CODE	DESCRIPTION
200 OK	서비스 바인딩이 삭제. “{}” 메시지를 응답
410 Gone	서비스 바인딩이 존재 하지 않을 경우. “{}” 메시지를 응답

2.2.	Body 
모든 응답 bodies 는 JSON Object ({}) 형식으로 한다.
성공시 “{}” 값을 전송받는다.

3.	Unbind Rest API 구현
3.1.	JAVA 방식
-- ServiceBindingRestController.java (Spring 프레임워크 사용)

@Controller
@RequestMapping("/v2/service_instances/{instanceId}/service_bindings/{bindingId}")
class ServiceBindingRestController {
  @Autowired ServiceBindingService bindingService;

@RequestMapping(method = RequestMethod.DELETE)
@ResponseBody
Map destroy(@PathVariable String instanceId, @PathVariable String bindingId) {
    ServiceBinding binding = bindingService.findById(bindingId, instanceId); 
    bindingService.destroy(binding);  // 서비스 unbind 기능 구현 (개발 명세 내용 구현)
    return [:];
  }
}

3.2.	Ruby 방식(Ruby on Rails)
-- config/routes.rb : posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일

CfMysqlBroker::Application.routes.draw do
  resource :preview, only: [:show]

namespace :v2 do
resource :catalog, only: [:show] // 접속 라우팅 설정 (V2/catalog)
    patch 'service_instances/:id' => 'service_instances#set_plan'
    resources :service_instances, only: [:update, :destroy] do
resources :service_bindings, only: [:update, :destroy]
    end
  end

end

-- RestController 구현 (app/controllers/v2/service_bindings_controller.rb)

class V2::ServiceBindingsController< V2::BaseController

  def destroy
// 서비스 unbind 기능 구현 (개발 명세 내용 구현)
  end

end

3.3.	Node.js 방식
# sample (app.js): Catalog API 참고

var router = express.Router();

router.route('/v2/service_instances/:instanceId/service_bindings/:bindingId’)

.delete(function(req, res, next) {
// 서비스 unbind 기능 구현 (개발 명세 내용 구현)

})


4.	서비스 별 Unbind API 개발 명세
-	Unbind 할 bind 인스턴스가 존재 하는지 체크 한다.
-	Application 에 bind 된 정보를 삭제하고 결과를 Application 에 전달한다.

4.1.	RDBMS
1. Mysql 경우

- unbind 할 사용자가 존재 하는지 체크
SHOW GRANTS FOR #{username)}

- 생성된 사용자를 삭제한다.
DROP USER #{username}

- 서버에 권한 테이블을 재배치한다.
FLUSH PRIVILEGES

2. Cubrid DB 경우 

- bind 시 생성한 사용자를 삭제한다.
DROP USER #{username};

4.2.	대용량 저장소
1. GlusterFS 경우

- Swift User 를 삭제

Method  : DELETE

Req URL : http(s)://[IP Address OR HostName]/auth/v2/[AccountID]/[UserId]

Header  : X-Auth-Admin-User: .super_admin
	  X-Auth-Admin-Key: swauthkey	  X-Auth-User-Key: [Password]
	  X-Auth-User-Admin: [true OR false]

4.3.	NoSQL DB
1. mongoDB 경우

- bind 시 생성한 사용자를 삭제한다.
>use <databasename>
switched to db <databasename>
>db.runCommand( { dropUser: "<username>"
                } )

 
3.	Service release 개발 가이드
3.1.	개요
BOSH release 는 jobs(packages 구동 스크립트, monit 스크립트 등), packages, 소스 코드 및 관련 자료의 메타 데이터로 구성되어 있다.BOSH를 통해서 소프트웨어(service back-end, broker 및 etc)를 설치한다. 패키징에 필요한 바이너리 파일 (일명 "blobs")은 release 저장소 내부에 보관하는 필요성을 없애고 release 내 Blob 저장소에 저장되어 외부에서 참조 할 수있다.또한 BOSH release를 활용하기 위해 release yml 를 작성 한다.

참고: service back-end (서비스 provider 가 제공하는 소프트웨어: 2.2 Service Architecture 참고) 가 외부 서비스로 이미 제공하는 provider 인 경우에는 BOSH release 로 service back-end 를 설치할 필요는 없고 해당 브로커만 개발하여 외부 서비스와 연결할수 있다. 이런 경우에는 Borker만 Bosh release 로 설치 한다. 하지만 외부에서 제공하는 서비스가 CF 가 설치 되는 IaaS 에 포함(단절된 네트워크에서 cloud 운영인 경우)되는 경우에는 해당 service back-end 를 BOSH release로 배포한다. 또한 외부 서비스를 사용하고 Broker가 개방형 클라우드 플랫폼의 Application으로 (cf push) 제공 하려는 경우에는 BOSH release 는 생략 하고 2. Service Borker Guide 를 진행하면된다.

3.2.	Bosh Architecture
 
[그림출처]: http://www.cloudsofchange.com/2012/05/fork-in-road-to-cloud.html

-	개방형 클라우드 플랫폼 아키텍쳐와 흡사함 (Message Bus, Health Monitor, Blobstore 등)
-	Director 는 Cloud Controller 와 유사한 기능
-	IaaS 종류에 따라 CPIs 구현 내용이 달라짐. (CPI : Cloud Provider Interface)
-	Worker 들은 Director에 의해 결정된 task 들을 실행시키는 역할을 함

 
[그림출처]: https://www.ibm.com/developerworks/community/blogs/fe313521-2e95-46f2-817d-44a4f27eba32/entry/porting_cloud_foundry_on_power8_ubuntu_le?lang=en

3.3.	Release Directory 구조
디랙토리 구조는 Bosh release 로 구성 할수 있게 되어 있다. Bosh는 릴리스 엔지니어링, 배포 및 대규모 분산 서비스의 라이프 사이클 관리를위한 오픈 소스 도구이다.

3.3.1.	packages
packages에는 Boshrelease 설치를 위한 바이너리에 대한 종속성을 준비하는데 필요한 정보를 제공한다. (packaging, pre_packaging, spec 파일)
 

3.3.2.	jobs
설치되는 package 들의 jobs(processes) 의 구동 및 정지 script 들과 모니터링(monit) script 로 구성한다.
 

3.3.3.	src
service release 에서 사용하는 컨포넌트 소스 코드 또는 pre-compiled software 파일로 구성한다.
 




3.3.4.	shared
ruby 및 lib 와 같은 공통 컴포넌트 소스를 관리한다. (옵션)
 

3.3.5.	releases
-	버전별 서비스 release yml 파일들을 관리한다.(yaml 설치 방식)
-	버전별 서비스 release tgz 압축 파일들을 관리한다. (tarball 설치 방식)
-	Yaml 및 tarball 설치 방식은 아래 개발 가이드 참고한다.
 

3.3.6.	config
최종 release를 저장하기 위한 Bosh blobstore에 URL 및 액세스 자격 증명을 위한 설정 파일로 구성한다.
 

3.3.7.	.final_builds
최종 jobs 및 packages 에 대한 public blobstore 정보를 제공한다.
 

3.3.8.	deployments
IaaS 별 service 배포 manifest 파일 들을 관리한다.
 

3.3.9.	content_migrations
이번버전으로 부터의 마이그레이션 정보 파일 들을 관리한다. (옵션)
 

3.4.	개발 가이드
service를 Bosh release를 통해 배포 해야 하기 때문에 Bosh release 개발 방식에 따라
작성되어야한다.Bosh release 는 packages 와 jobs 관련 스크립트로 구성되어 있다.
Bosh 는 software를 release 할 때 두가지 방식을 제공한다.
Boshupload release CLI명령어 및 프로세스는 다음과 같다.

# bosh upload release CLI 
boshupload release [<release_file>] [--rebase] [--skip-if-exists]
release_file: 로컬 파일 또는 원격 URI 정보
    --rebase:최신 버전으로 Director에 설정
    --skip-if-exists:릴리즈 가 존재 하면 업로드 하지 않음

1.	Yaml 파일을 이용한 설치 프로세스 [<release_file> 파라미터가 yml 파일일 경우]: releases 디랙토리안에 cf-<service_name>-<version>.yml 파일을 읽어서 sha1 값으로 .final_builds 폴더의 해당 packages 또는 jobs 폴더안의 index.yml 의 blobstore_id 로 config/final.yml 의 blobstore 에 접근하여 설치하는 방식이다.
2.	tarball(설치할 release 파일을 모두 포함한 압축 파일: tgz 형식) 을 이용한 설치 프로세스 [<release_file> 파라미터가 tgz 파일일 경우]: blobstore 를 이용하지 않고 설치할 모든 packages 와 jobs 파일 및 release(release.MF), job메타 파일이 tgz 압축 파일 안에 있어서 blobstore에서 다운 받지 않고 설치하는 방식이다. (releases 디랙토리 안에 .tgz 파일로 압축)

3.4.1.	packages 가이드
Service software 설치 관련하여 packaging, pre_packaging 와 spec 파일로 구성 되어 있다.
3.4.1.1.	packaging
packaging 파일은 software 를 설치 하는 script 를 제공한다.

# packaging 파일 설명
1	“bosh generate package PACKAGE_NAME” 명령어로 packaging script file 를 자동생성한다.
1.1	예) $ bosh generate package test (service release 폴더에서 실행)
1.2	packages 폴더 안에 test package 폴더가 생성되고 해당 폴더에 packaging, pre_packaging, spec 파일 생성
1.3	bosh generate package 명령어로 하지 않고 수동으로 디랙토리 생성하여 파일을 만들어도 됨
2	컴파일시에 Bosh는 패키지 사양에 참조 된 소스 파일을 받아 배포 된 작업이 필요한 실행 가능한 바이너리 및 스크립트로 구성된다.
3	packaging scripts 작성시아래와 같은 내용을 포함한다.
3.1	Ruby 어플리케이션 경우 BOSH는 Ruby gems를 설치하고 소스파일을 복사해야 한다. (RubyGems은 루비 프로그램과 라이브러리를 배포하는 표준 형식을 제공하는 루비 프로그래밍 언어의 패키지 관리자)
3.2	Ruby 자체에 대해서는 BOSH는 binary 로 소스를 컴파일 해야 한다.
3.3	Python 어플리케이션 경우 BOSH는 Python eggs를 설치하고 소스파일을 복사해야 한다.
4	packaging script를 작성할 때 이러한 원칙을 준수한다.
4.1	“set -e –x” 으로 스크립트를 시작한다. 이는 에러가 발생하는 경우 즉시 종료 스크립트시킴으로써 컴파일시에 디버깅을 돕는다.
4.2	복사, 설치 또는 컴파일이 (BOSH_INSTALL_TARGET 환경 변수로 표현) 설치 대상 디렉토리에 코드가 생성 되는지 확인한다.“make” commands에 대한 use configure 또는 이에 상응하는 작업을 수행한다.
4.3	BOSH 패키지 사양 파일의 dependencies배포 된 바이너리를 사용할 수 있는지 확인해야 한다.

# Example libyaml packaging script
set -e -x

tar xzf libyaml_0.1.4/yaml-0.1.4.tar.gz
pushd yaml-0.1.4
  ./configure --prefix=${BOSH_INSTALL_TARGET}

  make
  make install
popd

# Example Ruby packaging script
set -e -x

tar xzf ruby_1.9.3/ruby-1.9.3-p484.tar.gz
pushd ruby-1.9.3-p484
  ./configure \
    --prefix=${BOSH_INSTALL_TARGET} \
    --disable-install-doc \
    --with-opt-dir=/var/vcap/packages/libyaml_0.1.4

  make
  make install
popd

tar zxvf ruby_1.9.3/rubygems-1.8.24.tgz
pushd rubygems-1.8.24
  ${BOSH_INSTALL_TARGET}/bin/ruby setup.rb
popd

${BOSH_INSTALL_TARGET}/bin/gem install ruby_1.9.3/bundler-1.2.1.gem --no-ri --no-rdoc

# Example ruby_app packaging script
set -e -x

cp -a ruby_app/* ${BOSH_INSTALL_TARGET}

cd ${BOSH_INSTALL_TARGET}

/var/vcap/packages/ruby_1.9.3/bin/bundle install \
  --local \
  --deployment \
  --without development test

3.4.1.2.	pre_packaging
pre_packaging 파일은 software 를 미리 packaging 하는 script 를 제공한다. (옵션)
bosh document 에서는 pre_packaging 파일의 사용은 권장되지 않으며 이 튜토리얼에서 논의되지 않는다.(Use of the pre_packaging file is not recommended, and is not discussed in this tutorial.) https://bosh.io/docs/create-release.html#dev-release-release 문서에 명시되어 있음

# mysql-service-broker pre_packaging Sample
# abort script on any command that exits with a non zero value
set -e

(
  cd ${BUILD_DIR}/cf-mysql-broker

  # cache gems
  bundle package --all

  RAILS_ENV=assets bundle exec rake assets:precompile

  # remove unneeded files
  rm -rf spec
)

3.4.1.3.	spec
설치할 package 의 메터 정보인 이름, dependencies 및 설치 파일 정보가 제공된다.

# spec 파일 설명
1	name: package 이름을 정의 
2	dependencies: (옵션) 패키지에 의존하는 다른 패키지의 목록을 정의
3	files: 패키지에 포함 된 파일의 목록을 정의하거나 명시적 또는 패턴 매칭을 통해 파일 목록을 정의 할 수 있음
4	패키지 spec 파일을 편집 절차
4.1	모든 컴파일 시간 종속성을 확인한다. 패키지가 다른 패키지에 의존하는 경우 컴파일시 의존성이 발생한다. (4.2 spec 파일 생성후 dependeny 가 있을 경우에 dependencies 내용을 추가)
예) dependency 그래프
[그림출처]: https://bosh.io/docs/create-release.html
 
4.2	“bosh generate package PACKAGE_NAME” 명령어로 spec script file 를 자동생성한다.
4.2.1	예) $ bosh generate package test (service release 폴더에서 실행)
4.2.2	packages 폴더 안에 test package 폴더가 생성되고 해당 폴더에 packaging, pre_packaging, spec 파일 생성
4.2.3	bosh generate package 명령어로 하지 않고 수동으로 디랙토리 생성하여 파일을 만들어도 됨
4.3	package 가 필요로 하는 모든 파일들을 src 디렉토리에 복사한다. 일반적으로 파일은 소스 코드이다. 만일 pre-compiled software(예: ruby-1.9.3-p484.tar.gz)를 포함하는 경우, pre-compiled binary가 포함 된 압축 파일을 복사한다.
4.4	spec 파일 작성
4.4.1	해당 패키지이름(name) 과파일의 이름(files)을 추가
4.4.2	spec 파일에 어떤 compile-time dependencie의 이름을 추가. 패키지에는 compile-time dependencie가 없는 경우 빈 배열을 나타 내기 위해 []를 사용
4.4.3	files 부분은 먼저 src 디랙토리에서 해당 파일을 찾고 없을 경우 blobstore 의 blobs 에서 찾는다.
4.4.4	files 해당 파일이 소스로 구성되어 있을 경우에는 일반적으로 globbing pattern(<package_name>/**/*) 을 사용한다.


# Example Ruby package spec file
---
name: ruby_1.9.3

  dependencies:
  - libyaml_0.1.4

  files:
  - ruby_1.9.3/ruby-1.9.3-p484.tar.gz
  - ruby_1.9.3/rubygems-1.8.24.tgz
  - ruby_1.9.3/bundler-1.2.1.gem

3.4.2.	jobs 가이드
모든 job은 시작 및 중지하는 방법이 제공되어야 한다. 따라서 제어 스크립트를 작성하고 MONIT 파일을 작성하여해당 실행 되는 jobs(processes)를 모니터링 한다.

3.4.2.1.	templates
설치된 package를 구동 및 정지 및 관련 설정 파일을 구성하는 템플릿 파일.

# control script 설명 : *.erb 화일
1	시작 명령과 중지 명령이 포함되어 있다.
2	관련 job에 대한 templates 디렉토리에 ERb template 형식으로 구성한다. (shell script로 구성)
3	각 job 에 대해 “/var/vcap/sys/log/JOB_NAME” 안에 로그 작업을 구성하는 제어 스크립트를 만든다.

# Example mariadb_ctl.erb
#!/bin/bash -e

set -e

export MARIADB_JOB_DIR=/var/vcap/jobs/mysql
RUN_DIR=/var/vcap/sys/run/mysql
datadir=/var/vcap/store/mysql
LOG_DIR=/var/vcap/sys/log/mysql
LOG_FILE=$LOG_DIR/maria-ctl.log
CONFIG_DIR=/etc/mysql
export JOB_INDEX=<%= index %>
STATE_FILE=/var/vcap/store/mysql/state.txt
MYSQL_DAEMON_FILE=/var/vcap/packages/mariadb_ctrl/mysql_daemon.sh
MYSQL_CLIENT_FILE=/var/vcap/packages/mariadb/bin/mysql
MYSQL_SERVER_FILE=/var/vcap/packages/mariadb/support-files/mysql.server
DB_SEED_SCRIPT_FILE=$MARIADB_JOB_DIR/bin/mysql_database_seed.sh
package_dir=/var/vcap/packages/mariadb_ctrl
executable_name=mariadb_ctrl-executable
MYSQL_UPGRADE_SCRIPT_FILE=$package_dir/mysql_upgrade.sh
MYSQL_SHOW_DATABASES_SCRIPT_FILE=$package_dir/show_databases.sh

export MY_NAME=mariadb_ctl
export RUN_DIR=/var/vcap/sys/run/$MY_NAME
export PIDFILE=$RUN_DIR/$MY_NAME.pid

DATABASE_SEED_ATTEMPTS=30

source /var/vcap/packages/common/utils.sh

# add mysql to path
if [ ! -f /usr/local/bin/mysql ]; then
  log "Adding mysql to path"
  ln -s /var/vcap/packages/mariadb/bin/mysql /usr/local/bin
fi

# add xtrabackup to path
export PATH=$PATH:/var/vcap/packages/xtrabackup/bin

# add perl libraries to perl env
export PERL5LIB=$PERL5LIB:/var/vcap/packages/xtrabackup/lib/perl/5.18.2

case $1 in

  # The start script must always exit 0 if there's a chance Maria could start successfully,
  # as if monit sees a pid for Maria but this script exited with error, it will assume
  # someone else started the process in the background.
  # This will latch the status to "Execution failed" until someone manually calls
  # monit unmonitor && monit monitor, at which point monit would find the pid and reset the status to 'running'

  start)

if [[ ! -d "$RUN_DIR" ]]; then
      log "start script: directory $RUN_DIR does not exist, creating it now"
      mkdir -p $RUN_DIR
    fi

    log "start script: checking for existing instance of $MY_NAME"
    set +e
    $(source /var/vcap/packages/common/utils.sh; pid_guard $PIDFILE $MY_NAME)
    pg_exit_code=$?
    log "start script: pg_exit_code: $pg_exit_code"
    if [ $pg_exit_code -eq 1 ]; then
log "start script: $MY_NAME already running. Exiting 0 so that monit doesn't think that execution failed"
      exit 0
    fi
    set -e

    log "start script: writing pid $$ for $MY_NAME to $PIDFILE"
    echo $$ > $PIDFILE

log "start script: checking if mysqld_safe is already running... "
    set +e
    pgrep -f /var/vcap/packages/mariadb/bin/mysqld_safe
    result_code=$?
    set -e
    # Exit code of 0 means we did find a process, so we should exit.
    if [ $result_code -eq 0 ]; then
      log "start script: mysqld_safe already running - exiting with 0 so that monit doesn't think that execution failed"
      exit 0
    else
      log "start script: mysqld_safe not already running - continue"
    fi

    # Start syslog forwarding
    /var/vcap/packages/syslog_aggregator/setup_syslog_forwarder.sh $MARIADB_JOB_DIR/config

    mkdir -p $LOG_DIR
    touch $LOG_FILE
    chown vcap:vcap $LOG_FILE
    date >> $LOG_FILE 2>> $LOG_FILE

    # It is surprisingly hard to get the config file location passed in
    # on the command line to the mysql.server script. This is easier.
    mkdir -p $CONFIG_DIR
    rm -f /etc/my.cnf
    rm -f $CONFIG_DIR/my.cnf
    ln -sf $MARIADB_JOB_DIR/config/my.cnf $CONFIG_DIR/my.cnf

if ! test -d ${datadir}; then
      log "start script: making ${datadir} and running /var/vcap/packages/mariadb/scripts/mysql_install_db"
      mkdir -p ${datadir}
      /var/vcap/packages/mariadb/scripts/mysql_install_db \
             --basedir=/var/vcap/packages/mariadb --user=vcap \
             --datadir=${datadir} >> $LOG_FILE 2>> $LOG_FILE
    fi
    chown -R vcap:vcap ${datadir}

    cd $package_dir

<% node_ip = spec.networks.send(p('network_name')).ip %>

log "start script: starting mariadb_ctrl..."
    $package_dir/$executable_name \
             -logFile=$LOG_FILE \
             -stateFile=$STATE_FILE \
             -mysqlDaemon=$MYSQL_DAEMON_FILE \
             -mysqlClient=$MYSQL_CLIENT_FILE \
             -mysqlUser=<%= p('admin_username')%> \
             -mysqlPassword=<%= p('admin_password')%> \
             -jobIndex=$JOB_INDEX \
             -dbSeedScript=$DB_SEED_SCRIPT_FILE \
             -upgradeScriptPath=$MYSQL_UPGRADE_SCRIPT_FILE \
             -showDatabasesScriptPath=$MYSQL_SHOW_DATABASES_SCRIPT_FILE \
             -numberOfNodes=<%= p('cluster_ips').length %> \
             -clusterIps=<%= (p('cluster_ips') - [node_ip]).join(',') %> \
             -maxDatabaseSeedTries=$DATABASE_SEED_ATTEMPTS
>> $LOG_FILE 2>> $LOG_FILE


log "start script: completed starting mariadb_ctrl."
    ;;

  stop)
log "stop script: stopping mariadb_ctrl..."
    mkdir -p $LOG_DIR
    date >> $LOG_FILE 2>> $LOG_FILE
    log "stop script: stopping node $JOB_INDEX" >> $LOG_FILE
    /var/vcap/packages/mariadb/support-files/mysql.server stop >> $LOG_FILE 2>> $LOG_FILE

    log "stop script: completed stopping mariadb_ctrl"
    ;;

  *)
    echo "Usage: mysql_ctl {start|stop}"
    ;;

esac

3.4.2.2.	monit
배포 된 release 에서 BOSH Agent가 job VM에서 실행된다. BOSH는 차례로 제어 스크립트의 명령을 실행하여 에이전트와 통신한다. Agent는 Monit 라는 오픈 소스 process 모니터링 software 를 사용한다.

# monit 파일 설명
1	작업 프로세스 ID (PID) 파일을 지정한다.
2	job이 vcap 그룹에 속하도록 지정

# Example mariadb monit file
check process mariadb_ctrl-executable
  with pidfile /var/vcap/sys/run/mysql/mysql.pid
  start program "/var/vcap/jobs/mysql/bin/mariadb_ctl start" with timeout 300 seconds
  stop program "/var/vcap/jobs/mysql/bin/mariadb_ctl stop" with timeout 10 seconds
  group vcap
  depends on galera-healthcheck-executable

check process galera-healthcheck-executable
  with pidfile /var/vcap/sys/run/galera-healthcheck/galera-healthcheck.pid
  start program "/var/vcap/jobs/mysql/bin/galera-healthcheck_ctl start" with timeout 10 seconds
  stop program "/var/vcap/jobs/mysql/bin/galera-healthcheck_ctl stop" with timeout 10 seconds
  group vcap
  depends on gra-log-purger-executable

check process gra-log-purger-executable
  with pidfile /var/vcap/sys/run/gra-log-purger/gra-log-purger.pid
  start program "/var/vcap/jobs/mysql/bin/gra-log-purger_ctl start" with timeout 10 seconds
  stop program "/var/vcap/jobs/mysql/bin/gra-log-purger_ctl stop" with timeout 10 seconds
  group vcap

3.4.2.3.	spec
설치할 job 의 메타 정보인 이름, templates 및 설정 properties 정보가 제공된다.

# spec 파일 설명
1	name: job이름을 정의 
2	templates: key/value 형식으로 존재
2.1	각 key는 template 이름
2.2	각 value는 job VM에 해당 파일의 경로
2.3	파일 경로는 “/var/vcap/jobs/<job_name>” 디렉토리를 기준으로 함
2.4	예) bin/mariadb_ctl ->/var/vcap/jobs/<job_name>/bin/ mariadb_ctl
3	packages: 설치되는 package의 목록
4	properties: template 파일에서 사용되는 변수들을 정의

# Example mysql job spec file
---
name: mysql

templates:
  mariadb_ctl.erb:  bin/mariadb_ctl
  my.cnf.erb:     config/my.cnf
  mariadb_init.erb: config/mariadb_init
  galera-healthcheck_ctl.erb: bin/galera-healthcheck_ctl
  gra-log-purger_ctl.erb: bin/gra-log-purger_ctl
  gra-log-purger.sh.erb: bin/gra-log-purger.sh
  mysql_database_seed.sh.erb: bin/mysql_database_seed.sh
  syslog_forwarder.conf.erb: config/syslog_forwarder.conf

packages:
- xtrabackup
- mariadb
- mariadb_ctrl
- galera
- galera-healthcheck
- gra-log-purger
- golang
- common
- syslog_aggregator

properties:
  admin_username:
    description: 'Username for the MySQL server admin user'
    default: 'root'
  admin_password:
    description: 'Password for the MySQL server admin user'
  port:
    description: 'Port the mysql server should bind to'
    default: 3306
  max_connections:
    description: 'Maximum total number of database connections for the node'
    default: 1500
  innodb_buffer_pool_size:
    description: 'The size in bytes of the memory buffer InnoDB uses to cache data and indexes of its tables'
  cluster_ips:
description: 'List of nodes.  Must have the same number of ips as there are nodes in the cluster'
  haproxy_mysql_user:
    description: 'A user for HAProxy health check'
  haproxy_ips:
    description: 'List of haproxy node ip addresses'

  #these two properties are also used by the Broker
  gcache_size:
    description: 'Cache size used by galera (maximum amount of data possible in an IST), in MB'
    default: 512
  ib_log_file_size:
    description: 'Size of the ib_log_file used by innodb, in MB'
    default: 1024
  seeded_databases:
    description: 'Set of databases to seed'
    default: {}

  network_name:
    description: "The name of the network (needed for the syslog aggregator)"
  syslog_aggregator.address:
    description: "IP address for syslog aggregator"
  syslog_aggregator.port:
    description: "TCP port of syslog aggregator"
  syslog_aggregator.all:
description: "Define whether forwarders should also send non-mysql syslog activity to the aggregator."
    default: false
  syslog_aggregator.transport:
description: "Transport to be used when forwarding logs (tcp|udp|relp)."
    default: "tcp"

 
4.	Deployment Guide
BOSH Deploymentmanifest 는 components 요소 및 배포의 속성을 정의한YAML  파일이다.
Deployment manifest 에는 sotfware를 설치 하기 위해서 어떤 Stemcell (OS, BOSH agent) 을 사용할것이며 Release (Software packages, Config templates, Scripts) 이름과 버전, VMs 용량, Jobs params 등을 정의하여 Bosh deploy CLI 을 이용하여 software(여기서는 서비스팩)를 설치 한다. (3.2 Bosh Architecture의 Modules components 그림 참고)

BOSH Deplyment manifest 의 내용은 아래와 같다.
	Deployment Identification: 배포 이름과 배포를 관리하는 BOSH Director의 UUID 정보
	Releases Block: deployment 안의 각 release 의 이름 및 버전 정보
	Networks Block: 네트워크 구성 정보
	Resource Pools Block: BOSH 로 생성하고 관리하는 가상 머신의 속성
	Disk Pools Block: BOSH 로 생성하고 관리하는 디스크의 속성
	Compilation Block: 컴파일 시 필요한 가상 머신의 속성
	Update Block: BOSH 가 배포 중에 작업 인스턴스를 업데이트 하는 방법을 정의
	Jobs Block: 작업(jobs)에 대한 구성 및 자원 정보
	Properties Block: 글로벌 속성과 일반화된 구성 정보를 설명

1.	Deployment Identification
name [String, required]: 배포의 이름. 단일 BOSH Director는 다수의 배포를 관리하고 그들의 이름으로 구별 한다.
director_uuid [String, required]:BOSH CLI가 배포에 대한 모든 작업을 허용하기위한 현재 대상 BOSH Director의 UUID와 일치해야한다. ‘bosh status’ CLI 을 통해서 현재 BOSH Director 에 target 되어 있는 UUID를 확인할수 있다.

# bosh status CLI
 

# Example
name: my-redis-deployment
director_uuid: 8b701af8-d658-48ee-893e-9d299622e332

2.	Releases Block
releases [Array, required]:deployment 안의 각 release 의 이름 및 버전 정보
name [String, required]: release 에서 사용하는 이름
version [String, required]: release 에서 사용하는 버전. ‘latest’ 를 넣을 경우 최신 버전 사용

# Example
releases:
- {name: redis, version: 12}

3.	Networks Block
networks [Array, required]: 네트워크 블록에 나열된 각 서브 블록이 참조 할 수있는 작업이 네트워크 구성을 지정한다. 네트워크는 manual, dynamic, vip 세 개의 종류가 있다.

# AWS Example
Dynamic network 또는 manual network 서브넷에서 사용하는 ‘cloud_properties’ 스키마
subnet [String, required]: AWS에서 생성한 subnet ID

#Example of manual network:

networks:
- name: default
  type: manual

  subnets:
  - range:   10.10.0.0/24
    gateway: 10.10.0.1
    cloud_properties:
      subnet: subnet-9be6c3f7

#Example of dynamic network:

networks:
- name: default
  type: dynamic
  cloud_properties:
    subnet: subnet-9be6c6gh

#Example of vip network:

networks:
- name: default
  type: vip
  cloud_properties: {}

# OpenStack Example
Dynamic network 또는 manual network 서브넷에서 사용하는 ‘cloud_properties’ 스키마
net_id [String, required]: OpenStack에서 생성한 subnet ID. 예) net-b98ab66e-6fae-4c6a-81af-566e630d21d1
security_groups [Array, optional]: security groups 이 네크워크 구성에 적용.


#Example of manual network:

networks:
- name: default
  type: manual

  subnets:
  - range:   10.10.0.0/24
    gateway: 10.10.0.1
    cloud_properties:
      net_id: net-b98ab66e-6fae-4c6a-81af-566e630d21d1
      security_groups: [my-sec-group]

#Example of dynamic network:

networks:
- name: default
  type: dynamic
  cloud_properties:
    net_id: net-b98ab66e-6fae-4c6a-81af-566e630d21d1

#Example of vip network:

networks:
- name: default
  type: vip
  cloud_properties: {}

# vSphere Example
manual network 서브넷에서 사용하는 ‘cloud_properties’ 스키마
name [String, required]: vSphere 에서 사용하는 network 이름

#Example of manual network:

networks:
- name: default
  type: manual

  subnets:
  - range:   10.10.0.0/24
    gateway: 10.10.0.1
    cloud_properties:
      name: VM Network

참고 :vSphere CPI does not support dynamic or vip networks.

# vCloud Example
manual network 서브넷에서 사용하는 ‘cloud_properties’ 스키마
name [String, required]: vApp 에서 생성된network 이름

#Example of manual network:

networks:
- name: default
  type: manual

  subnets:
  - range:   10.10.0.0/24
    gateway: 10.10.0.1
    cloud_properties:
      name: VPC_BOSH

참고 :vCloud CPI does not support dynamic or vip networks.

4.	Resource Pools Block
resource_pools [Array, required]:배포시 사용하는 resource pools를 명시하며 여러 개의 resource pools 을 사용할 경우 name 은 unique 해야함
Resource pools: 같은 stemcell 부터 생성한 가상머신 모음
name [String, required]:고유한 resource pool 이름
network [String, required]: Networks block에서 선언한 network 이름
size [Integer, optional]: resource pool 안의 가상머신 개수이고 이 값을 생략하면 jobs 인스턴스의 총 개수가 된다. 이 값을 넣을 경우 jobs 인스턴스 보다 작으면 에러가 남
stemcell [Hash, required]: resource pool 가상머신에서 생성한 stemcell 정보
- name [String, required]: stemcell 이름
- version [String, required]: stemcell 버전
cloud_properties [Hash, required]: 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성을 설명 (instance_type, availability_zone)
env [Hash, optional]: CPI (cloud provider interface) 에서 create_stemcell 호출할때 가상머신 환경 변수이고 env 데이터는 가상머신에 세팅 되어 있는 BOSH Agents 에서 사용할수 있다. 디폴트는 {}

# AWS Example
instance_type [String, required]: 인스턴스 종류. 예) m1.small
availability_zone [String, required]:인스턴스를 생성하기 위한 availability zone 예) us-east-1a
key_name [String, optional]: key pair 이름. 디폴트 key pair 이름은 global CPI 설정안의 default_key_name 예) bosh
spot_bid_price [Float, optional]:Bid price in dollars 예) 0.03
ephemeral_disk [Hash, optional]:EBS backed ephemeral disk of custom size for when instance storage is not large enough or not available for selected instance type
- size [Integer, required]: megabytes 디스크 사이즈
- type [String, optional]: 디스크 타입(standard, gp2). 디폴트 standard
  - standard stands for EBS magnetic drives
  - gp2 stands for EBS general purpose drives (SSD)

resource_pools:
- name: default
  network: default
  stemcell:
    name: bosh-aws-xen-ubuntu-trusty-go_agent
    version: latest
  cloud_properties:
    instance_type: m1.small
    availability_zone: us-east-1a

# OpenStack Example
instance_type [String, required]: 인스턴스 종류 예) m1.small
availability_zone [String, required]:인스턴스를 생성하기 위한 availability zone 예) us-east-1a
key_name [String, optional]: key pair 이름. 디폴트 key pair 이름은 global CPI 설정안의 default_key_name 예) bosh
scheduler_hints [Hash, optional]:Data passed to the OpenStack Filter scheduler to influence its decision where new VMs can be placed 예) { group: af09abf2-2283... }


resource_pools:
- name: default
  network: default
  stemcell:
    name: bosh-openstack-kvm-ubuntu-trusty-go_agent
    version: latest
  cloud_properties:
    instance_type: m1.small
    availability_zone: us-east-1a

# vSphere Example
cpu [Integer, required]: CPUs 수
ram [Integer, required]:RAM in megabytes
disk [Integer, required]:Ephemeral disk size in megabytes.
datacenters [Array, optional]:datacenters의 배열을 VM 위치에 상관 없이 사용. 하나를 가지고 있어야하며 글로벌 CPI 옵션에 구성된 데이터 센터와 일치해야한다.
-	name [String, required]: Datacenter 이름
-	clusters [Array, required]: clusters의 배열을 VM 위치에 상관 없이 사용 가능
-	<cluster name> [String, required]: Cluster 이름
-	drs_rulers [Array, optional]: DRS 규칙의 배열은 VM 배치를 제한하기 위해 적용.하나만 존재
-	name [String, required]: Director 가 만들 DRS 규칙 이름
-	type [String, required]: DRS rule종류. 현재는separate_vms 만 제공


resource_pools:
- name: default
  network: default
  stemcell:
    name: bosh-vsphere-esxi-ubuntu-trusty-go_agent
    version: latest
  cloud_properties:
    cpu: 2
    ram: 1_024
    disk: 10_240

# vCloud Example
cpu [Integer, required]: CPUs 수
ram [Integer, required]:RAM in megabytes
disk [Integer, required]:Ephemeral disk size in megabytes


resource_pools:
- name: default
  network: default
  stemcell:
    name: bosh-vcloud-esxi-ubuntu-trusty-go_agent
    version: latest
  cloud_properties:
    cpu: 2
    ram: 1_024
    disk: 10_240

5.	Disk Pools Block
disk_pools [Array, required]: 배포시 사용하는 disk pools를 명시하며 여러 개의 disk pools 을 사용할 경우 name 은 고유 식별자이어야 함
disk pools: disk의 논리적 모음. 같은 설정으로 생성됨
name [String, required]:고유한 disk pool 이름
disk_size [Integer, required]:disk 사이즈 명시하며 integer 값으로 표시
cloud_properties [Hash, required]: disk 를 만드는 데 필요한 IaaS의 특정 속성 (type, iops)

# AWS Example
type [String, optional]: disk 종류(standard, gp2). 디폴트는 standard
- standard stands for EBS magnetic drives
- gp2 stands for EBS general purpose drives (SSD)
encrypted [Boolean, optional]: 이 영구 디스크에 대한 EBS 볼륨 암호화를 켠다. root 및 임시 디스크는 암호화 되지 않는다. 디폴트는 false.



disk_pools:
- name: default
  disk_size: 10_240
  cloud_properties:
    type: m1.small

# OpenStack Example
type [String, optional]: OpenStack 설정 볼륨 종류 예) SSD


disk_pools:
- name: default
  disk_size: 10_240
  cloud_properties:
    type: SSD

# vSphere Example
현재는 disk를 위한 cloud properties 는 제공하지 않음

disk_pools:
- name: default
  disk_size: 10_240
  cloud_properties: {}

# vCloud Example
현재는 disk를 위한 cloud properties 는 제공하지 않음

disk_pools:
- name: default
  disk_size: 10_240
  cloud_properties: {}

6.	Compilation Block
compilation [Hash, required]: 컴파일시 필요한 가상머신의 속성
workers [Integer, required]: 컴파일 하는 가상머신의 최대수
network [String, required]: Networks block에서 선언한 network 이름
reuse_compilation_vms [Boolean, optional]: false 경우에는 BOSH는 각각의 새로운 패키지 컴파일을위한 새로운 컴파일 VM을 생성하고 편집이 완료되면 VM을 삭제한다. true 경우는 재사용한다. 디폴트는 false
cloud_properties [Hash, required]: 컴파일 VM을 만드는 데 필요한 IaaS의 특정 속성 (instance_type, availability_zone)

# Example
compilation:
workers: 2
network: default
  reuse_compilation_vms: true
  cloud_properties:
  instance_type: c1.medium
  availability_zone: us-east-1c

7.	Update Block
update [Hash, required]: 업데이트 속성을 정의하며 이러한 속성은 BOSH가 배포 중에 작업 인스턴스를 업데이트하는 방법을 제어
canaries [Integer, required]: canary 인스턴스 수
canary: canary 인스턴스에 먼서 인스턴스를 업데이트를 수행하고 에러가 날 경우 배포가 중지 됨
canary_watch_time [Integer or Range, required]: canary 인스턴스가 수행하기 위한 대기 시간
update_watch_time [Integer or Range, required]: non-canary 인스턴스가 수행하기 위한 대기 시간
max_in_flight [Integer, required]: non-canary 인스턴스가 병렬로 update 하는 최대 개수

# Example
update:
canaries: 1
max_in_flight: 10
canary_watch_time: 1000-30000
  update_watch_time: 1000-30000

8.	Jobs Block
jobs [Array, required]: BOSH release jobs 과 cloud 인스턴스 사이의 맵핑을 명시하며 Jobs는 BOSH release 에 명시 되어 있고 Jobs block 은 BOSH가 IaaS에 의해 가상 머신을 어떤 방법으로 생성하고 구동하는지를 정의
name [String, required]: unique 이름
templates [Array, required]: release 의 job template 정보
- name [String, required]: job template 이름
- release [String, required]: job template 이 존재하는 release 이름
instances [Integer, required]: job 인스턴스 개수. 각 인스턴스는이 특정 job을 실행하는 VM
resource_pool [String, required]:Resource Pools block에 정의한 resource pool 이름
networks [Array, required]: 네트워크 정의
- name [String, required]:Networks block에 정의한 network 이름
- static_ips [Array, optional]: 사용할 IP addresses 정의
- default [Array, optional]: 네트워크 구성요소 명시(DNS, Gateway)
persistent_disk [Integer, optional]:영구적 디스크 사이즈 정의
update [Hash, optional]: 이 job에 대한 특정 업데이트 설정
properties [Hash, optional]: job 속성을 지정


# Example
- name: redis-master
  instances: 1
  templates:
  - {name: redis-server, release: redis}
  persistent_disk: 10_240
  resource_pool: redis-servers
  networks:
  - name: default

- name: redis-slave
  instances: 2
  templates:
  - {name: redis-server, release: redis}
  persistent_disk: 10_240
  resource_pool: redis-servers
  networks:
  - name: default


9.	Properties Block
글로벌 속성과 일반 구성 정보를 설명
글로벌 속성은 제한 없이 사용가능
  - Passwords, Account names, Shared secrets, Host names, IP addresses, Port numbers, max_connections , etc.

# Example
properties:
  redis:
    max_connections: 10

 
5.	Deploy Guide
BOSH deploy CLI 로 Software를 배포 하기 위해서 선행조건으로 deployment manifest yml 에서
사용할 stemcell 과release가 먼저 upload 되어 있어야 하고 deployment manifest yml 파일이 작성 
되어 있어야 한다. 
stemcell 과 release 가 upload 되어 있는지 확인한다. 확인하는 BOSH CLI 는 각각 ‘bosh stemcells’, 
‘bosh releases’ 이다.
Software(서비스팩 software)를 배포하는 bosh deploy CLI 명령어를 제공한다. 

아래의 단계로 배포를 진행한다.
1.	Deploy 할 deployment manifest 파일을 BOSH 에 지정한다.(bosh deployment CLI)

 


2.	Software 를 배포한다. (bosh deploy CLI)

 


3.	배포된 Software 를 확인한다. (bosh vms)

 

