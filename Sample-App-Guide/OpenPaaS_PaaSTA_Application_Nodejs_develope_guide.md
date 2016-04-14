## Table of Contents
1. [문서 개요](#1)
     * [1.1. 목적](#2)
     * [1.2. 범위](#3)
     * [1.3. 참고 자료](#4)
2. [개발환경 구성](#5)
     * [2.1. Node.js 및 npm 설치](#6)
3. [개발](#7)
     * [3.1. Node.js Express애플리케이션 생성](#8)
     * [3.2. Node.js 샘플 애플리케이션](#9)
     * [3.3. 애플리케이션 환경설정](#10)
     * [3.4. VCAP_SERVICES 환경설정 정보](#11)
     * [3.5. Mysql 연동](#12)
     * [3.6. Cubrid 연동](#13)
     * [3.7. MongoDB 연동](#14)
     * [3.8. Redis 연동](#15)
     * [3.9. RabbitMQ연동](#16)
     * [3.10. GlusterFS 연동](#17)
4. [배포](#18)
     * [4.1. 개방형 플랫폼 로그인](#19)
     * [4.2. 서비스 생성](#20)
     * [4.3. 애플리케이션 배포](#21)
     * [4.4. 애플리케이션, 서비스 연결](#22)
     * [4.5. 애플리케이션 실행](#23)
5. [테스트](#24)



<div id='1'></div>
# 1. 문서 개요

<div id='2'></div>
### 1.1. 목적

본 문서(node.js 애플리케이션 개발 가이드)는 개방형 플랫폼 프로젝트의 서비스팩(Mysql, Cubrid, MongoDB, RabbitMQ, Radis, GlusterFS)을 Node.js 애플리케이션과 연동하여서비스를 사용하고 애플리케이션을 배포하는 방법에 대해 제시하는 문서이다.

<div id='3'></div>
### 1.2. 범위

본 문서의 범위는 개방형 플랫폼 프로젝트의 Node.js 애플리케이션 개발과 서비스팩 연동에 대한 내용으로 한정되어 있다.

<div id='4'></div>
### 1.3. 참고자료
**<https://docs.cloudfoundry.org/devguide/>**  
**<https://docs.cloudfoundry.org/buildpacks/node/node-tips.html>**  
**<https://nodejs.org/>**  
**<http://expressjs.com/ko/>**  
**<https://github.com/felixge/node-mysql>**  
**<https://github.com/CUBRID/node-cubrid>**  
**<https://github.com/mongodb/node-mongodb-native>**  
**<https://github.com/NodeRedis/node_redis>**  
**<https://github.com/postwait/node-amqp>**  
**<https://github.com/pkgcloud/pkgcloud>**  
**<https://mochajs.org/>**

<div id='5'></div>
# 2. 개발환경 구성

Open PaaS에 등록된 다양한 서비스팩을 Node.js언어로 작성된 애플리케이션과 바인딩하고해당 애플리케이션에 바인딩된 환경정보(VCAP_SERVICES)에서 각 서비스별 접속정보를 획득하여 애플리케이션에 적용하여 이용 할 수 있도록 Windows 환경에서 Node.js 애플리케이션을 작성 할 수 있도록한다.

Node.js 애플리케이션 개발을 위해 다음과 같은 환경으로 개발환경을 구성 한다.

- BuildPack: v1.3.4
- OS : Windows 7 64bit
- Node.js : v0.12.4
- npm : v2.10.1

<div id='6'></div>
### 2.1. Node.js 및 npm 설치

##### 1. Node.js 다운로드

- 아래의 주소로 접속한 후 node-v0.12.4-x64.msi를 다운받는다.

>https://nodejs.org/dist/v0.12.4/x64/node-v0.12.4-x64.msi
>![2-2-1-0]

##### 2. Node.js 설치

- 다운받은 폴더에서 node-v0.12.4-x64.msi를 더블클릭하여 설치를 시작한다.

![2-2-1-1]

- "실행"버튼을 클릭하여 계속 진행한다.

![2-2-1-2]

- "Next"버튼을 클릭하여 계속 진행한다.

![2-2-1-3]

- "I accept the terms in the License Agreement"를 체크하여 라이센스에 동의한 후 "Next"버튼을 클릭하여 계속 진행한다.

![2-2-1-4]

- 설치경로를 입력 혹은 선택한 후 "Next"버튼을 클릭하여 계속 진행한다.
여기서는 C:\Program Files\nodejs\ 를 설치경로로 설정하였다.

![2-2-1-5]

- 설치할 항목을 선택한 후 "Next"버튼을 클릭하여 계속 진행한다.
여기서는 선택하여 Node.js, npm, doc을 설치하고 환경변수 PATH까지 추가하였다.

![2-2-1-6]

- "Install"버튼을 클릭하여 설치한다.

![2-2-1-7]

- "Finish"버튼을 클릭하여 설치를 완료한다.

![2-2-1-8]

- '윈도우키+R' 또는 '시작->실행'아이콘을 클릭하여 실행창을 띄운 후 'cmd'를 입력하고 "확인"버튼을 눌러 커맨드창을 연다.

![2-2-1-9]

- 커맨드창에 아래의 명령어를 입력하여 node.js와 npm의 버젼과 제대로 설치되었는지 여부를 확인한다.

><div>>node -v</div>
><div>>npm -v</div>
![2-2-1-10]

개발도구
Node.js는 javascript기반의 언어로 Notepad++, Sublim Text, EditPlus등 문서편집기를 개발도구로 사용할 수 있다. 또한 Eclipse의 플러그인 Nodeclipse를 설치하여 사용할 수도있다.

<div id='7'></div>
# 3. 개발

샘플 애플리케이션에의 데이터 관리는 MySQL, CubridDB, MongoDB 중에 하나를 이용하기 때문에 API 요청시 요청 본문에 DBType 값을 가지고 결정한다.

<div id='8'></div>
### 3.1. Node.js Express애플리케이션 생성
##### 1. 'express-generator'를 이용하여 Express 애플리케이션을 생성

- 커맨드 창에서 개발을 진행할 경로로 이동후 아래의 명령어를 입력하여 'express-generator' npm을 설치한다.

><div>>npm install express-generator</div>
![2-3-1-0]

- Express 애플리케이션을 생성한다. '-e'옵션은 view enjine을 ejs를 사용한다는 것이고 default view enjin은 jade이다.

><div>>.\node_modules\.bin\express -e</div>
![2-3-1-1]

##### 2. npm 설치

- Express 애플리케이션에 기본적으로 포함되어있는 npm을 설치한다. 설치할 npm에 대한 정의는 package.json에 정의되어있다. 

><div>>npm install</div>
![2-3-1-2]

##### 3. Node.js Express 어플리캐이션 실행

- 아래의 두 명령어중 하나를 이용해 애플리케이션 실행한다.

><div>>npm start</div>
><div>>node bin/www</div>
![2-3-1-3]

- 브라우저로 아래의 주소로 접속하여 애플리케이션이 제대로 동작하는지 확인한다.

><div>http://localhost:3000/</div>
![2-3-1-3]
 
<div id='9'></div>
### 3.2. Node.js 샘플 애플리케이션

##### 1. Node.js 샘플 애플리케이션 다운로드 (현재는 비공개이며 지원예정)

- git clone을 통해 Node.js 샘플 애플리케이션을 다운받는다.

```
git clone https://github.com/OpenPaaSRnD/OpenPaaSSample
```

##### 2. Node.js 샘플 애플리케이션 경로로 이동

- 다운받은 경로아래에 Node.js 샘플 애플리케이션 경로로 이동한다.

><div>>cd node-sample-app</div>
![2-3-2-0]

##### 3. Node.js 샘플 애플리케이션 디렉토리구조

![2-3-2-1]

<table>
  <tr>
    <td>파일/폴더</td>
    <td>목적</td>
  </tr>
  <tr>
    <td>package.json</td>
    <td>node.js 어플리케이션에 필요한 npm의 의존성 정보를 기술하는데 사용 한다.<br>
    npm install 명령을 실행시 install 뒤에 아무런 정보를 입력하지 않으면 이 파일의 정보를 이용하여 npm을 설치한다.</td>
  </tr>
  <tr>
    <td>app.js</td>
    <td>Express에 대한 설정을 한다. http요청에 대한 라우팅 정보 또한 이곳에서 정의한다.</td>
  </tr>
  <tr>
    <td>bin/www</td>
    <td>실질적으로 node.js 샘플 애플리케이션의 시작점이며, http서버를 설정한다. 서버가 구동될 때 사용할 Port를 여기서 설정할 수 있다.</td>
  </tr>
  <tr>
    <td>routes/</td>
    <td>app.js에서 라우팅 후 실제 수행할 내용이 작성되어있다. 서비스 연결에 대한 내용도 이곳에 있다.</td>
  </tr>
  <tr>
    <td>public/</td>
    <td>외부에서 접근가능한 디렉토리이다. css, js등 웹서비스에 필요한 정적파일들이 있다.<br>
    외부에서 접근가능한 디렉토리에 대한 설정은 app.js파일에서 설정한다.</td>
  </tr>
  <tr>
    <td>views/</td>
    <td>ejs파일들이 위치하는 곳이다.<br>
    ejs파일은 html을 좀더 쉽게 작성할 수 있게 도와주는 template enjin 이며 express의 view enjin을 ejs로 설정했을시 사용하는 파일이다.<br>
    render() 메소드를 이용하여 ejs파일을 html로 변환하여 보여준다.</td>
  </tr>
  <tr>
    <td>test/</td>
    <td>mocha test를 작성한 디렉토리이다.</td>
  </tr>
  <tr>
    <td>(node_modules)</td>
    <td>위의 그림에서는 보이지 않지만 npm install로 모듈설치시 이 디렉토리아래에 설치가 된다. 여기에 설치되어있는 npm모듈들을 애플리케이션에서 require로 불러와서 사용할 수 있다.</td>
  </tr>
  <tr>
    <td>Makefile</td>
    <td>linux에서 좀더 쉽게 mocha 테스트를 실행하기위한 파일이다.</td>
  </tr>
  <tr>
    <td>manifest.yml</td>
    <td>개방형 플랫폼에 배포시 애플리케이션에 대한 설정이다. 애플리케이션의 이름, 배보될 경로, 인스턴스 수 등을 정의할 수 있다.</td>
  </tr>
  <tr>
    <td>.cfignore</td>
    <td>개방형 플랫폼에 배포시 포함되지않을 디렉토리, 혹은 파일을 기술한다.</td>
  </tr>
  <tr>
    <td>.gitignore</td>
    <td>git에 배포시 포함되지않을 디렉토리, 혹은 파일을 기술한다.</td>
  </tr>
  <tr>
    <td>README.md</td>
    <td>Node.js 샘플 애플리케이션에 대한 간략한 설명이 기술되어 있다.</td>
  </tr>
</table>

<div id='10'></div>
### 3.3. 애플리케이션 환경설정

이 샘플은 Node.js version 0.12.4, npm version 2.10.1.을 기준으로 각 모듈의 버전을 명시적으로 선택하여 설치하였다.
package.json 수정(설정)시 설치된 Node.js의 버전에 맞는 모듈을 설치하는 것을 권장한다.

1)  ./package.json
- 애플리케이션에서 필요한 모듈을 정의한다.

```json
{
  "name": "node-sample-app",
  "version": "0.8.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "1.13.2",
    "cookie-parser": "1.3.5",
    "debug": "2.2.0",
    "morgan": "1.6.1",
    "serve-favicon": "2.3.0",
    "express": "4.13.1",
    "ejs": "2.3.4",
    "generic-pool": "2.2.1",
    "mysql": "2.9.0",
    "node-cubrid":"2.2.5",
    "mongodb":"2.0.48",
    "redis":"2.4.2",
    "uuid":"2.0.1",
    "amqp":"0.2.4",
    "pkgcloud":"1.2.0-alpha.0",
    "formidable":"1.0.17",
    "mocha":"2.3.4",
    "should":"7.1.1",
    "supertest":"1.1.0"
  },
  "engines": {
    "node": "0.12.4",
    "npm": "2.10.1"
  }
}
```

<table>
  <tr>
    <td>name</td>
    <td>애플리케이션 이름</td>
  </tr>
  <tr>
    <td>version</td>
    <td>애플리케이션 버젼</td>
  </tr>
  <tr>
    <td>private</td>
    <td>npm에 게시할것인지 여부를 설정한다. (true: 게시하지않음)</td>
  </tr>
  <tr>
    <td>scripts.start</td>
    <td>npm start 명령어로 실행될 명령어(애플리케이션 구동 명령어)</td>
  </tr>
</table>
  
- dependencies

<table>
  <tr>
    <td>body-parser</td>
    <td rowspan=7>Express프레임워크에서 기본적으로 사용하는 모듈들.</td>
  </tr>
  <tr>
    <td>cookie-parser</td>
  </tr>
  <tr>
    <td>debug</td>
  </tr>
  <tr>
    <td>morgan</td>
  </tr>
  <tr>
    <td>serve-favicon</td>
  </tr>
  <tr>
    <td>express</td>
  </tr>
  <tr>
    <td>ejs</td>
  </tr>
  <tr>
    <td>generic-pool</td>
    <td>connection pool생성 및 관리 모듈</td>
  </tr>
  <tr>
    <td>mysql</td>
    <td>mysql 모듈</td>
  </tr>
  <tr>
    <td>node-cubrid</td>
    <td>cubrid 모듈</td>
  </tr>
  <tr>
    <td>mongodb</td>
    <td>mongodb 모듈</td>
  </tr>
  <tr>
    <td>redis</td>
    <td>redis 모듈</td>
  </tr>
  <tr>
    <td>uuid</td>
    <td>고유식별자를 생성해주는 모듈</td>
  </tr>
  <tr>
    <td>amqp</td>
    <td>rabbitMQ 사용 모듈</td>
  </tr>
  <tr>
    <td>pkgcloud</td>
    <td>swift, glusterfs 사용 모듈</td>
  </tr>
  <tr>
    <td>formidable</td>
    <td>form data를 파싱해주는 모듈</td>
  </tr>
  <tr>
    <td>mocha</td>
    <td>node.js test 모듈</td>
  </tr>
  <tr>
    <td>should</td>
    <td>mocha test에 사용되는 모듈</td>
  </tr>
  <tr>
    <td>supertes</td>
    <td>rest test에 사용되는 모듈</td>
  </tr>
</table>

- engines

<table>
  <tr>
    <td>node</td>
    <td>애플리케이션에서 사용할 node.js 모듈 버젼.<br>
    개방형 플랫폼에 배포하여 사용시 Node Buildpack에서 지원하는 Node.js 버젼에 따라 사용할 수 있는 버젼에 제약이 있다.<br>
    - https://github.com/cloudfoundry/nodejs-buildpack/blob/master/CHANGELOG</td>
  </tr>
  <tr>
    <td>npm</td>
    <td>애플리케이션에서 사용할 npm 버젼<br>
    Node.js와 마찬가지로 Node Buildpack에서 지원하는 버젼에 따라 사용할 수 있는 버젼에 제약이 있다.</td>
  </tr>
</table>

2) 모듈 설치
- pakage.json에 정의된 모듈을 설치한다. 모듈이름을 지정하지 않으면 package.json의 depencencies의 모든 모듈을 설치한다.
```
>npm install
```

3) ./bin/www
- HTTP서버가 사용할 PORT를 개방형 플랫폼이 제공하는 PORT를 사용하게 설정한다. 개방형 플랫폼은 이 값을 이용하여 애플리케이션이 제대로 동작하고 있는지 감지하는데 사용한다. 이 값 외의 다른 PORT를 사용하면 애플리케이션이 제대로 동작하지 않는다.

```
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var app = require('../app');
var debug = require('debug')('node-sample-app:server');
var http = require('http');

/**
 * Get port from environment and store in Express.
 * port 환경변수를 얻어와서 변수에 담는다.
 * 'process.env.PORT'는 Cloud에서 사용하는 환경변수.
 */

var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);

/**
 * Create HTTP server.
 * HTTP 서버 생성.
 */

var server = http.createServer(app).listen(app.get('port'), function(){
  console.log('Express server listening on port ' + app.get('port'));
});

/**
 * Listen on provided port, on all network interfaces.
 * 서버가 사용할 port를 설정한다.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);
...(생략)
```

4) ./app.js
- Request URL 매핑 설정

```javascript
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');

var routes = require('./routes/index');
var users = require('./routes/users');

var app = express();

// URL 매핑 후 수행할 js 파일들
var org_chart_mysql = require('./routes/rest/org_chart/mysql')
  , org_chart_mongo = require('./routes/rest/org_chart/mongo')
  , org_chart_cubrid = require('./routes/rest/org_chart/cubrid')
  , orgs_mysql = require('./routes/rest/orgs/mysql')
  , orgs_mongo = require('./routes/rest/orgs/mongo')
  , orgs_cubrid = require('./routes/rest/orgs/cubrid')
  , groups_mysql = require('./routes/rest/groups/mysql')
  , groups_mongo = require('./routes/rest/groups/mongo')
  , groups_cubrid = require('./routes/rest/groups/cubrid')
  , login = require('./routes/rest/login/login')
  , image = require('./routes/rest/image/image')
  , page = require('./routes/page/page_processing');

// view engine setup
// 뷰 엔진 설정
app.set('views', path.join(__dirname, 'views/'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
// 정적파일 위치 설정
app.use(express.static(path.join(__dirname, 'public')));

/*
* URL 매핑 설정
*/
app.use(   '/', routes);
app.use(   '/users', users);

// page
app.get(   '/login', page.login);
app.get(   '/manage', page.manage);
app.get(   '/main/:id', page.main);

// org-chart
app.get(   '/org-chart/:org_id/mysql', org_chart_mysql.index);
app.get(   '/org-chart/:org_id/cubrid', org_chart_cubrid.index);
app.get(   '/org-chart/:org_id/mongo', org_chart_mongo.index);
app.get(   '/org-chart/:org_id/status/mysql', org_chart_mysql.status);
app.get(   '/org-chart/:org_id/status/cubrid', org_chart_cubrid.status);
app.get(   '/org-chart/:org_id/status/mongo', org_chart_mongo.status);

//orgs
//  mysql
app.get(   '/orgs/mysql', orgs_mysql.index);
app.post(  '/orgs/mysql', orgs_mysql.create);
app.get(   '/orgs/:org_id/mysql', orgs_mysql.show);
app.put(   '/orgs/:org_id/mysql', orgs_mysql.update);
app.delete('/orgs/:org_id/mysql', orgs_mysql.destroy);
…..(생략)
```

<div id='11'></div>
### 3.4. VCAP_SERVICES 환경설정 정보 
개방형 플랫폼에 배포되는 애플리케이션이 바인딩된서비스별 접속 정보를 얻기 위해서는 애플리케이션별로 등록되어있는 VCAP_SERVICES 환경설정 정보를 읽어들여정보를 획득 할 수 있다.

1)  개방형 플랫폼의 애플리케이션 환경정보
- 서비스를 바인딩하면 JSON 형태로 환경설정 정보가 애플리케이션 별로 등록된다.

```json
{
 "VCAP_SERVICES": {
  "CubridDB": [
   {
    "credentials": {
     "host": "10.30.60.23::",
     "hostname": "10.30.60.23",
     "jdbcurl": "jdbc:cubrid:10.30.60.23::fccf1d7869ff72ce:b2f6b4af1e7bd7d8:45f179c648ee60a5:",
     "name": "fccf1d7869ff72ce",
     "password": "45f179c648ee60a5",
     "port": "",
     "uri": "cubrid:10.30.60.23::fccf1d7869ff72ce:b2f6b4af1e7bd7d8:45f179c648ee60a5:",
     "username": "b2f6b4af1e7bd7d8"
    },
    "label": "CubridDB",
    "name": "sample-cubrid-instance",
    "plan": "utf8",
    "tags": [
     "cubrid",
     "document"
    ]
   }
  ],
…..(이하 생략)…..
```

2)  Node.js에서 개방형 플랫폼의 애플리케이션 환경정보에 접근하는 방법
- 시스템환경변수의 VCAP_SERVICES값을 읽어서 접근 할 수 있다.
<div>process.env.VCAP_SERVICES</div>

<div id='12'></div>
### 3.5. Mysql 연동
1)  ./route/db/mysql/db_pooling.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 mysql Connection Pool을 생성
```javascript
/**
 * generic-pool 연동
 * mysql 풀 모듈 구현
 */

var generic_pool  = require("generic-pool");
var mysql   = require("mysql");

config = {};
if (process.env.VCAP_SERVICES) {
  // cloud env 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
  var cloud_env   = JSON.parse(process.env.VCAP_SERVICES);
  var mysql_env   = cloud_env["Mysql-DB"][0]["credentials"];

  config = {
    host:mysql_env.hostname,
    user:mysql_env.username,
    password:mysql_env.password,
    database:mysql_env.name
  };
} else {
  // local env
  config = {
    host:'10.30.40.63',
    user:'cESTBl9QpxGVF5Xa',
    password:'aVu1ynInBnaEeFY0',
    database:'cf_ea68784e_3de6_439d_afc1_d51b4e95627b'
  };
}

var pooling   = generic_pool.Pool({
  name:"mysql",
  create:function(cb){
    // create Connection
    var conn = mysql.createConnection(config);
    conn.connect(function(err){
      if( err) console.log("mysql 연결오류");
      else {
      //  console.log("mysql 연결성공");
      } cb(err, conn);
      // 콜백함수를 통해 풀링에 커넥션 객체를 던짐
    });
  },
  destroy:function(myConn){
    myConn.end(function(err){
      if( err)  console.log("mysql 연결해제오류");
  //    else    console.log("mysql 연결해제성공");
    });
  },
  min:3,
  max:5,
  idleTimeoutMillis:1000*500,
  log:false

});

process.on("exit", function(){
  pooling.drain(function(){
    pooling.destroyAllNow();
  });
});

module.exports = pooling;
```

<div id='13'></div>
### 3.6. Cubrid 연동
1)  ./route/db/cubrid/db_pooling.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 cubrid Connection Pool을 생성

```javascript
/**
 * generic-pool 연동
 * cubrid 풀 모듈 구현
 */

var generic_pool  = require("generic-pool");
var cubrid    = require("node-cubrid");

var   database
  , port
  , hostname
  , username
  , password;
if (process.env.VCAP_SERVICES) {
  // cloud env 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
  var cloud_env   = JSON.parse(process.env.VCAP_SERVICES);
  var cubrid_env    = cloud_env["CubridDB"][0]["credentials"];

  database  = cubrid_env.name
  port    = cubrid_env.port
  hostname  = cubrid_env.hostname
  username  = cubrid_env.username
  password  = cubrid_env.password;
} else {
  // local env
  database  = 'fccf1d7869ff72ce'
  port    = '' 
  hostname  = '10.30.60.23' 
  username  = 'b2f6b4af1e7bd7d8'
  password  = '45f179c648ee60a5';
}
var pooling   = generic_pool.Pool({
  name:"cubrid",
  create:function(cb){
//    console.log("cubrid_env.uri:" + cubrid_env.uri);
    var conn = cubrid.createCUBRIDConnection(hostname, port, username, password, database);
    // create Connection
    conn.connect(function(err){
      if( err) console.log("cubrid 연결오류");
      else{
//        console.log("cubrid 연결성공");
        cb(err, conn);
      }
      // 콜백함수를 통해 풀링에 커넥션 객체를 던짐
    });
  },
  destroy:function(myConn){
    myConn.end(function(err){
      if( err)  console.log("cubrid 연결해제오류");
//      else    console.log("cubrid 연결해제성공");
    });
  },
  min:3,
  max:5,
  idleTimeoutMillis:1000*50,
  log:false,
  
});

process.on("exit", function(){
  pooling.drain(function(){
    pooling.destroyAllNow();
  });
});

module.exports = pooling;
```
<div id='14'></div>
### 3.7. MongoDB 연동
1)  ./route/db/mongo/db_pooling.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 mongodb Connection Pool을 생성

```javascript
/**
 * generic-pool 연동
 * mongo 풀 모듈 구현
 */

var generic_pool = require("generic-pool");
var mongoClient  = require("mongodb").MongoClient;

var url = '';
if (process.env.VCAP_SERVICES) {
  // cloud env. 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
  var cloud_env = JSON.parse(process.env.VCAP_SERVICES);
  var mongo_env = cloud_env["Mongo-DB"][0]["credentials"];

  url = mongo_env.uri;

} else {
  // local env.
  url = 'mongodb://d3e35ad5-9f49-43ae-bc85-08e39ec1d8eb:fc23791e-b2d8-402d-b070-90bfbdb5dcfa@10.30.60.53:27017/e37e541c-75de-4f01-8196-63e2d902e768';
}

var pooling = generic_pool.Pool({
        name:"mongo",
        create:function(cb){
    // create Connection
    mongoClient.connect(url, function(err, db){
      if (err) console.log("mongo 연결오류");
      else {
        cb(err, db);
      }
    });
        },
        destroy:function(myDb){
                myDb.close(function(err){
                        if( err)        console.log("mysql 연결해제오류");
        //              else            console.log("mysql 연결해제성공");
                });
        },
        min:3,
        max:5,
        idleTimeoutMillis:1000*500,
        log:false

});


module.exports = pooling;
```

<div id='15'></div>
### 3.8. Redis 연동
1)  ./route/redis/redis.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 redis Connection을 생성

```javascript
var redis = require("redis");

var options = {};
if (process.env.VCAP_SERVICES) {
  // cloud env. 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
  var services = JSON.parse(process.env.VCAP_SERVICES);
  var redisConfig = services["redis-sb"];

  if (redisConfig) {
      var node = redisConfig[0];
      options = {
      host: node.credentials.host,
      port: node.credentials.port,
      pass: node.credentials.password,
    };
  }

} else {
  // local env.
    options = {
    host: '10.30.40.71',
    port: '34838',
    pass: 'c239b721-d986-4ee3-8816-b5f5fa9f3ffb',
  };
}

var client = null;
exports.open = function(cb) {
//  console.log(JSON.stringify(options));
  //create Client
  client = redis.createClient(options);
  // get auth.
  client.auth(options.pass);

  cb(client);
}
exports.close = function(){
  client.end();
}
```

<div id='16'></div>
### 3.9. RabbitMQ연동
1)  ./route/rabbitMQ/rabbitMQ.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 rabbirMQ Connection을 생성

```javascript
var amqp = require('amqp');

var url = '';
if (process.env.VCAP_SERVICES) {
  // cloud env. 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
        var services = JSON.parse(process.env.VCAP_SERVICES);
        var rabbitMQConfig = services["p-rabbitmq"];

        if (rabbitMQConfig) {
                var node = rabbitMQConfig[0];
    url = node.credentials.uri;
        }
} else {
  // local env.
  url = 'amqps://14b1ab93-4cdb-46af-8cdd-8d8073bbe282:cl71e9ihgu6gvhj1eiqj9uh4um@10.30.40.82:5671/6ffb4d8a-8748-4f00-a338-80e6eadee822';
}

exports.open = function(cb){
  // create connection.
  var conn = amqp.createConnection({url: url});
  
  // it must be cb(callback) after the 'ready' event.
  conn.on('ready', function(){
    cb(conn);
  });
}

// not used. 
/*
exports.close = function(){
  conn.disconnect();
}
*/
```
<div id='17'></div>
### 3.10. GlusterFS 연동
1)  ./route/glusterfs/glusterfs.js
- 개방형 플랫폼의 애플리케이션 환경정보에 접근하여 glusterfs Connection을 생성

```javascript
var pkgcloud = require('pkgcloud');
var http = require('http');
var url = require('url');

var credentials = {};
var container_name = 'node_container';
if (process.env.VCAP_SERVICES) { 
  // cloud env. 설정. 데이터 구조는 2.3.4 VCAP_SERVICES 환경정보 참고
  var services = JSON.parse(process.env.VCAP_SERVICES); 
  var glusterfsConfig = services["glusterfs"]; 
 
  if (glusterfsConfig) { 
    var config = glusterfsConfig[0]; 
    credentials = {
      provider: 'openstack', // 
            username: config.credentials.username,
            password: config.credentials.password,
      authUrl:  config.credentials.auth_url.substring(0, config.credentials.auth_url.lastIndexOf('/')),
      region: 'RegionOne' //
    };
  }
} else {
  // local env.
    credentials = {
      provider: 'openstack',
            username: 'cf13d551d997458e', 
            password: 'b45cc01d53a4f0e0',
      authUrl:  'http://54.199.136.22:5000/',
      region: 'RegionOne'
    };
}

// create Client
var client = pkgcloud.storage.createClient(credentials);

// delete container for test
/*
client.destroyContainer(container_name, function(err, result){
  if (err) console.log(err);
  else console.log(result);
});
*/

// check container
client.getContainer(container_name, function(err, container){
        if (err)
        {
    // if container not exist
                if (err.statusCode === 404)
                {
      // create container
                        client.createContainer({name:container_name}, function(create_err, create_container){
                                if (create_err) console.log(err);
        else
        {  
          // if container created successfully, setting a readable member(X-Contaner-Read: .r:*)
          // 컨테이너가 성공적으로 생성되었다면 컨테이너를 누구나 읽을 수 있게 설정한다.(X-Contaner-Read: .r:*)
          // There is a bug in the code(pkgcloud). so i used api call.
          // pkgcloud 모듈에서 metadata를 넣을 경우 prefix가 붙는 로직때문에 제대로 위의 값이 입력이 안되므로 api를 통해서 설정.
          var serviceUrl = url.parse(create_container.client._serviceUrl);
          var option = {
            host: serviceUrl.hostname,
            port: serviceUrl.port,
            path: serviceUrl.path+'/'+container_name,
            method: 'POST',
            headers: {
              'X-Auth-Token': create_container.client._identity.token.id,
              'X-Container-Read': '.r:*' // ACL form
            }
          };
          var req = http.request(option, function(res){
          });
          req.end();
        }

                        });
                }
                else    console.log(err);
        }
});

module.exports = client;
```

<div id='18'></div>
# 4. 배포

개방형 플랫폼에 애플리케이션을 배포하면 배포한 애플리케이션과 개방형 플랫폼이 제공하는 서비스를 연결하여 사용할 수 있다. 개방형 플랫폼상에서 실행을 해야만 개방형 플랫폼의 애플리케이션 환경변수에 접근하여 서비스에 접속할 수 있다.

<div id='19'></div>
### 4.1.  개방형 플랫폼 로그인

아래의 과정을 수행하기 위해서 개방형 플랫폼에 로그인

<div>
$ cf api --skip-ssl-validation https://api.cf.open-paas.com # 개방형 플랫폼 TARGET 지정
#cf api [target url]

$ cf login -u testUser -o sample_test -s sample_space # 로그인 요청
 #cf login –u [user name] –o [org name] –s [space name]
</div>


![2-4-1-0]

<div id='20'></div>
### 4.2.  서비스 생성
애플리케이션에서 사용할 서비스를 개방형 플랫폼을 통하여 생성한다. 별도의 서비스 설치과정 없이 생성할 수 있으며, 애플리케이션과 바인딩과정을 통해 접속정보를 얻을 수있다.
- 서비스 생성 (cf marketplace 명령을 통해 서비스 목록과 각 서비스의 플랜을 조회할 수 있다.

<div>
# cf create-service SERVICE PLAN SERVICE_INSTANCE [-c PARAMETERS_AS_JSON] [-t TAGS] 
$ cf create-service p-mysql 100mb node-mysql
$ cf create-service CubridDB utf8 node-cubrid
$ cf create-service Mongo-DB default-plan node-mongodb
$ cf create-service redis-sb shared-vm node-redis
$ cf create-service glusterfs glusterfs-5Mb node-glusterfs
$ cf create-service p-rabbitmq standard node-rabbitmq
</div>


![2-4-2-0]

<div id='21'></div>
### 4.3. 애플리케이션 배포
애플리케이션을 개방형 플랫폼에 배포한다. 배포된 애플리케이션은 생성된 서비스와 바인드하여 서비스를 사용할 수 있다.

1.  manifest.yml 생성

```yaml
---
applications:
- name: node-sample-app # 애플리케이션 이름
  memory: 512M # 애플리케이션 메모리 사이즈
  instances: 1 # 애플리케이션 인스턴스 개수
  command: npm start # 애플리케이션 실행 명령어
  path: ./ # 배포될 애플리케이션의 위치
```

2.  Mysql, Cubrid 테이블 생성 
- Sample App의 조직관리 기능을 위해 DB에 테이블을 생성해 주어야 한다.
- Mysql과 Cubrid에 테이블을 추가하는 방법은 OpenPaaS Mysql, Cubrid 서비스팩 설치 가이드의 'Client 툴 접속'을 참고한다.
- Client 툴을 이용하여 아래의 테이블 생성 sql를 각각 실행한다. (Mysql과 Cubrid 양쪽다 동일한 sql로 생성가능하다.)

```
DROP TABLE IF EXISTS ORG_TBL;
DROP TABLE IF EXISTS GROUP_TBL;


CREATE TABLE ORG_TBL (
  id INT AUTO_INCREMENT PRIMARY KEY
  , label VARCHAR(40) NOT NULL
  , `desc` VARCHAR(150)
  , url VARCHAR(500) DEFAULT '#'
  , created TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL
  , modified TIMESTAMP
);

CREATE TABLE GROUP_TBL (
  id INT AUTO_INCREMENT PRIMARY KEY
  , org_id INTEGER NOT NULL
  , parent_id INTEGER
  , label VARCHAR(40) NOT NULL
  , `desc` VARCHAR(150)
  , thumb_img_name VARCHAR(256)
  , thumb_img_path VARCHAR(512)
  , url VARCHAR(500) DEFAULT '#'
  , created TIMESTAMP  DEFAULT CURRENT_TIMESTAMP  NOT NULL
  , modified TIMESTAMP 
);

ALTER TABLE GROUP_TBL
ADD FOREIGN KEY(org_id)
REFERENCES ORG_TBL(id)
ON DELETE CASCADE;

ALTER TABLE GROUP_TBL
ADD FOREIGN KEY(parent_id)
REFERENCES GROUP_TBL(id)
ON DELETE CASCADE;
```

3. 애플리케이션 배포
- cf push 명령으로 배포한다. 별도의 값을 넣지않으면 manifest.yml의 설정을 사용한다. 아직 서비스를 연결하지 않았기 때문에 --no-start 옵션으로 배포후 실행은 하지않는다.
<div>$ cf push --no-start</td>div>


![2-4-3-0]

<div id='22'></div>
### 4.4. 애플리케이션, 서비스 연결
애플리케이션과 서비스를 연결하는 과정을 '바인드(bind)라고 하며, 이 과정을 통해 서비스에 접근할 수 있는 접속정보를 생성한다.
- 애플리케이션과 서비스 연결

```
cf bind-service APP_NAME SERVICE_INSTANCE [-c PARAMETERS_AS_JSON]
$ cf bind-service node-sample-app node-mysql
$ cf bind-service node-sample-app node-cubrid
$ cf bind-service node-sample-app node-mongodb
$ cf bind-service node-sample-app node-redis
$ cf bind-service node-sample-app node-glusterfs
$ cf bind-service node-sample-app node-rabbitmq
```


![2-4-4-0]

연결확인
<div>$ cf services</div>


![2-4-4-1]

<div id='23'></div>
### 4.5. 애플리케이션 실행
서비스 바인드 과정을 통해 생성된 접속정보 환경변수를 가지고 어플리케이션이 실행된다.
<div>$ cf start node-sample-app</div>


![2-4-5-0]

<div id='24'></div>
# 5. 테스트
샘플 어플리케이션은 REST 서비스로 구현되어있으며 REST 테스트를 위해서 mocha 모듈을 사용하였다. 테스트를 진행하기 위해서는 mocha 모듈을 포함한 package.json 안의 모듈들이 설치 되어 있어야한다. (npm install)

1.  Makefile
- 매번 bin파일에 접근하여 실행하는 불편함을 해결하기 위해 작성. 리눅스 운영체제에서 사용할 수 있다.

```
test:
  @./node_modules/.bin/mocha -u tdd

.PHONY: test
```

2.  테스트 실행
- test디렉토리 아래에 있는 테스트를 실행한다.
2.1.  윈도우
<div>> .\node_modules\.bin\mocha -u tdd test</div>

2.2.  리눅스
<div>$ make test</div>


![2-5-0-0]

[2-2-1-0]:/Sample-App-Guide/image/nodejs/2-2-1-0.png
[2-2-1-1]:/Sample-App-Guide/image/nodejs/2-2-1-1.png
[2-2-1-2]:/Sample-App-Guide/image/nodejs/2-2-1-2.png
[2-2-1-3]:/Sample-App-Guide/image/nodejs/2-2-1-3.png
[2-2-1-4]:/Sample-App-Guide/image/nodejs/2-2-1-4.png
[2-2-1-5]:/Sample-App-Guide/image/nodejs/2-2-1-5.png
[2-2-1-6]:/Sample-App-Guide/image/nodejs/2-2-1-6.png
[2-2-1-7]:/Sample-App-Guide/image/nodejs/2-2-1-7.png
[2-2-1-8]:/Sample-App-Guide/image/nodejs/2-2-1-8.png
[2-2-1-9]:/Sample-App-Guide/image/nodejs/2-2-1-9.png
[2-2-1-10]:/Sample-App-Guide/image/nodejs/2-2-1-10.png
[2-3-1-0]:/Sample-App-Guide/image/nodejs/2-3-1-0.png
[2-3-1-1]:/Sample-App-Guide/image/nodejs/2-3-1-1.png
[2-3-1-2]:/Sample-App-Guide/image/nodejs/2-3-1-2.png
[2-3-1-3]:/Sample-App-Guide/image/nodejs/2-3-1-3.png
[2-3-1-4]:/Sample-App-Guide/image/nodejs/2-3-1-4.png
[2-3-2-0]:/Sample-App-Guide/image/nodejs/2-3-2-0.png
[2-3-2-1]:/Sample-App-Guide/image/nodejs/2-3-2-1.png
[2-4-1-0]:/Sample-App-Guide/image/nodejs/2-4-1-0.png
[2-4-2-0]:/Sample-App-Guide/image/nodejs/2-4-2-0.png
[2-4-3-0]:/Sample-App-Guide/image/nodejs/2-4-3-0.png
[2-4-4-0]:/Sample-App-Guide/image/nodejs/2-4-4-0.png
[2-4-4-1]:/Sample-App-Guide/image/nodejs/2-4-4-1.png
[2-4-5-0]:/Sample-App-Guide/image/nodejs/2-4-5-0.png
[2-5-0-0]:/Sample-App-Guide/image/nodejs/2-5-0-0.png
