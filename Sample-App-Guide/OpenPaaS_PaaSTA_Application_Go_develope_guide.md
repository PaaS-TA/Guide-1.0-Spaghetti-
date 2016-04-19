### 1. 개요

#### 1.1. 문서 개요

##### 1.1.1. 목적
본 문서(Go 애플리케이션 개발 가이드)는 Open PaaS 프로젝트의 서비스팩(Mysql, MongoDB, RabbitMQ, Radis, GlusterFS)을 Go 애플리케이션과 연동하여 서비스를 사용하고 애플리케이션을 배포하는 방법에 대해 제시하는 문서이다.

##### 1.1.2. 범위
본 문서의 범위는 Open PaaS 프로젝트의 Go 애플리케이션 개발과 서비스팩 연동에 대한 내용으로 한정되어 있다.

##### 1.1.3. 참고 자료
-   [***https://code.google.com/p/golang-korea/wiki/EffectiveGo***](https://code.google.com/p/golang-korea/wiki/EffectiveGo)
-   [***https://github.com/brianmario/mysql2***](https://github.com/brianmario/mysql2)
-   [***https://github.com/redis/redis-rb***](https://github.com/redis/redis-rb)
-   [***https://github.com/fog/fog***](https://github.com/fog/fog)
-   [***https://www.jetbrains.com/idea/***](https://www.jetbrains.com/idea/)

### 2. Go 애플리케이션 개발가이드

#### 2.1. 개요
Open PaaS에 등록된 다양한 서비스팩을 Go언어로 작성된 애플리케이션과 바인딩하고 해당 애플리케이션에 바인딩된 환경정보(VCAP\_SERVICES)에서 각 서비스별 접속정보를 획득하여 애플리케이션에 적용하여 이용 할 수 있도록 Windows 환경에서 Go 애플리케이션을 작성 할 수 있도록한다.

#### 2.2. 개발환경 구성
Go 애플리케이션 개발을 위해 다음과 같은 환경으로 개발환경을 구성 한다.
-   OS : Windows 7 64bit
-   Go : 1.5.2
-   IDE : Intellij IDEA  
Intellij IDEA 는 Commnuity와 Ultimate 버전이 있는데, Community 버전은 Free이고, Ultimate 버전은 은 30-day trial버전이다.

##### 2.2.1. Go SDK설치
1.  Go SDK 다운로드

| <span id="__DdeLink__2953_294360055" class="anchor"></span>https://golang.org/dl/ |
|-----------------------------------------------------------------------------------|

<img src="./image/go/image2.png" width="612" height="151" />

-   다운로드

Go SDK : go1.5.2.windows-amd64.msi

1.  Go SDK 설치

-   go1.5.2.windows-amd64.msi 더블클릭하여 설치를 실행한다.

<img src="./image/go/image3.png" width="297" height="170" />

-   “실행” 버튼 클릭

<img src="./image/go/image4.png" width="351" height="275" />

-   “Next” 버튼을 클릭

<img src="./image/go/image5.png" width="330" height="258" />

-   “Next” 버튼을 클릭

<img src="./image/go/image6.png" width="324" height="254" />

-   Go SDK가 설치될 디렉토리 선택 후 “Next” 버튼 클릭

<img src="./image/go/image7.png" width="330" height="258" />

-   “Install” 버튼 클릭

<img src="./image/go/image8.png" width="317" height="247" />

<img src="./image/go/image9.png" width="323" height="252" />

-   “Finish” 버튼 클릭

##### 2.2.2. IntelliJ IDEA설치
1.  IDEA 다운로드

| http://www.jetbrains.com/idea/ |
|--------------------------------|

<img src="./image/go/image10.png" width="601" height="272" />

<img src="./image/go/image11.png" width="499" height="400" />

<img src="./image/go/image12.png" width="559" height="138" />

-   다운로드 : ideaIC-15.0.2.exe

1.  IntelliJ IDEA 설치

-   ideaIC-15.0.2.exe 더블클릭하여 설치를 실행한다.

<img src="./image/go/image13.png" width="290" height="225" />

-   “Next” 버튼 클릭

<img src="./image/go/image14.png" width="315" height="245" />

-   설치위치 지정 후 “Next” 버튼 클릭

<img src="./image/go/image15.png" width="315" height="245" />

-   “Next” 버튼 클릭

<img src="./image/go/image16.png" width="315" height="245" />

-   “Install” 버튼 클릭

<img src="./image/go/image17.png" width="308" height="240" />

<img src="./image/go/image18.png" width="309" height="240" />

-   “Run IntelliJ IDEA Community Edition” 체크 선택

-   “Finish” 버튼 클릭

#### 2.3. 개발

샘플 애플리케이션에의 데이터 관리는 MySQL, MongoDB 중에 하나를 이용하기 때문에 API 요청시 요청 본문에 DBType 값을 가지고 결정한다.

##### 2.3.1. 샘플 애플리케이션 연동

1.  Go 샘플 프로젝트 연결

<img src="./image/go/image19.png" width="315" height="219" />

-   “Import Project” 선택

<img src="./image/go/image20.png" width="315" height="418" />

-   다운로드 받은 Go 샘플 어플리케이션의 위치를 지정

-   “Ok” 버튼을 클릭

<img src="./image/go/image21.png" width="275" height="269" /> <img src="./image/go/image22.png" width="284" height="278" />

-   “Next” 버튼 클릭 - “Next” 버튼 클릭

<img src="./image/go/image23.png" width="353" height="345" />

-   “Next” 버튼 클릭

<img src="./image/go/image24.png" width="339" height="331" />

-   “Finish” 버튼 클릭

##### 2.3.2. 샘플 애플리케이션 환경설정

-   IntelliJ IDEA 환경에 Go Plugin 설치 및 Go 샘플 어플리케이션의 환결설정

<img src="./image/go/image25.png" width="535" height="359" />

-   File &gt; Settings 선택

<img src="./image/go/image26.png" width="541" height="366" />

-   Plugins &gt; Browse repositories 선택

<img src="./image/go/image27.png" width="549" height="459" />

-   “Go” 를 검색 후, 조회된 결과에서 Go 를 선택 후 Install 버튼을 클릭

<img src="./image/go/image28.png" width="556" height="466" />

-   “Restart IntelliJ IDEA” 선택

<img src="./image/go/image29.png" width="366" height="135" />

-   “Restart” 버튼 클릭

<img src="./image/go/image30.png" width="459" height="308" />

-   File &gt; Project Structure... 선택

<img src="./image/go/image31.png" width="462" height="384" />

-   Project SDK 영역의 “New” 버튼 클릭

<img src="./image/go/image32.png" width="451" height="375" />

-   “Go SDK” 선택

<img src="./image/go/image33.png" width="321" height="368" />

-   “Go SDK” 설치된 디렉토리 선택 후 “OK” 버튼 클릭

<img src="./image/go/image34.png" width="442" height="368" />

-   “OK” 선택

<img src="./image/go/image35.png" width="573" height="312" />

-   go-sample-app 프로젝트에서 “main.go” 파일을 선택

-   오른쪽 상단에 “Configure Go Libraries” 선택

<img src="./image/go/image36.png" width="266" height="445" /> <img src="./image/go/image37.png" width="264" height="443" />

-   Global libraries 영역에는 Go SDK 디렉토리 선택

-   Project libraries 영역에는 Go 샘플 어플리케이션이 위치한 디렉토리 선택

-   “OK” 버튼 클릭

<img src="./image/go/image38.png" width="571" height="310" />

-   오른쪽 상단의 “Change module type to Go and reload project” 선택

<img src="./image/go/image39.png" width="347" height="126" />

-   “Reload project” 선택

<img src="./image/go/image40.png" width="560" height="305" />

##### 2.3.3. VCAP\_SERVICES 환경설정 정보를 통한 연동 

개방형 플랫폼에 배포되는 애플리케이션이 바인딩된 서비스별 접속 정보를 얻기 위해서는 애플리케이션별로 등록되어있는 VCAP\_SERVICES 환경설정 정보를 읽어들여 정보를 획득 할 수 있다.

1).  개방형 플랫폼의 애플리케이션 환경정보

-   서비스를 바인딩하면 JSON 형태로 환경설정 정보가 애플리케이션 별로 등록된다.

~~~
{
 "VCAP_SERVICES": {
  "p-rabbitmq": [
   {
    "credentials": {
     "dashboard_url": "https://pivotal-rabbitmq.10.244.0.34.xip.io/#/login/f8b12fcd-df98-4745-a6b2-61f01d20fe24/5lpn72rufsivfgnf3ft4l1p797",
     "hostname": "10.244.9.50",
     "hostnames": [
      "10.244.9.50"
     ],

…..(중간 생략)…..
     "ssl": true,
     "uri": "amqps://f8b12fcd-df98-4745-a6b2-61f01d20fe24:5lpn72rufsivfgnf3ft4l1p797@10.244.9.50/a1aec425-d1ec-40b7-865f-4eaba371b9a2",
     "uris": [
      "amqps://f8b12fcd-df98-4745-a6b2-61f01d20fe24:5lpn72rufsivfgnf3ft4l1p797@10.244.9.50/a1aec425-d1ec-40b7-865f-4eaba371b9a2"
     ],
     "username": "f8b12fcd-df98-4745-a6b2-61f01d20fe24",
     "vhost": "a1aec425-d1ec-40b7-865f-4eaba371b9a2"
    },
    "label": "p-rabbitmq",
    "name": "rabbitmq-service-instance",
    "plan": "standard",
    "tags": [
     "rabbitmq",
     "messaging",
     "message-queue",
     "amqp",
     "stomp",
     "mqtt",
     "pivotal"
    ]
   }
  ]
…..(이하 생략)…..
~~~

-   VCAP\_SERVICES 정보 구조

2).  VCAP\_SERVICES 정보 추출 방법

-   VCAP\_SERVICE 정보 중 “uri” 정보를 추출하여 리턴한다.

-   부모 엘리먼트 (VCAP\_SERVICE)
    -   첫번째 엘리먼트 (p-rabbitmq) 서비스 이름 정보
    -   두번째 엘리먼트 (credentials) 정보
    -   세번째 엘리먼트 (uri) 정보

~~~
func sample_function_name() string {

   args := os.Getenv("VCAP_SERVICES")     //VCAP_SERVICES 엘리먼트 정보 (부모 엘리먼트)
   var amqp_uri string
   if args != "" {
      var vcap_env map[string]interface{}
      if err := json.Unmarshal([]byte(args), &vcap_env); err != nil {
         log.Panic(err.Error())
      }

      //Service instance (not name) - for example : p-mysql - type check !!! []interface{}
      if vcap_env["p-rabbitmq"] != nil {    //첫번째 엘리먼트(서비스 이름) 정보
         sub_env := vcap_env["p-rabbitmq"].([]interface{})
         credentials := (sub_env[0].(map[string]interface{}))["credentials"].(map[string]interface{})
//두번째 엘리먼트 “credentials” 정보
         amqp_uri = credentials["uri"].(string)    //세번째 엘리먼트 “uri”정보
      }
   }
   return amqp_uri
}
~~~

-   VCAP\_SERVICES 구조 참조

##### 2.3.4. config.ini 파일을 통한 Database, Redis, RabbitMQ 연동

1).  config.ini

-   mysql 연결정보 설정

~~~
#Go Sample Web Server port
server.port = 8080
#Mysql DB Info
mysql.dburl=”mysql\_server\_ip”:”mysql\_server\_port”/”database\_name”
mysql.userid=”user\_id”
mysql.userpwd=”user\_password”
mysql.maxconn=”max\_connection\_number”

MongoB Info
mongodb.dburl=”mongodb\_server\_ip”: ”mongodb\_server\_port”/”collection\_name”
mongodb.userid=”user\_id”
mongodb.userpwd=”user\_password”

# Redis Info
redis.addr=”redis\_server\_ip”: ”redis\_server\_port”

# RabbitMQ Info
rabbitmq.user=”rabbitmq\_id”
rabbitmq.pass=”rabbitmq\_password”
rabbitmq.addr=”rabbitmq\_server\_ip”: ”rabbitmq\_server\_port”
~~~

2).  연동 샘플

~~~
import (
   "bufio"
   "fmt"
   "io"
   "log"
   "os"
   "strconv"
   "strings"

   "net/http"

   "encoding/json"
   "org/openpaas/sample/Godeps/_workspace/src/gopkg.in/redis.v3"

   "org/openpaas/sample/datasource"
   "org/openpaas/sample/handler"
   "org/openpaas/sample/message"
)

type Config map[string]string

func main() {
   // Datasource - MySql, Cubrid, MongoDB 타입별 처리
   var dbconfig *datasource.DBConfig
   var mgodbconfig *datasource.MgoDBConfig
   var handlers http.Handler

   fmt.Println("##### Go Sample Application start!!!")
   //============================================
   // dbtype 정보는 시스템 프로퍼티에서 가져온다.
   dbtype := os.Getenv("dbtype")
   //============================================

   //============================================
   // Sample VCAP_SERVICE INFO Parsing
   os_args := os.Getenv("VCAP_SERVICES")
   readOSEnvironment(os_args)
   //============================================

   //============================================
   // 기본적인 프로퍼티 설정 정보 읽어오기
   config, err := ReadConfig(`config.ini`)
   if err != nil {
      fmt.Println(err)
   }
   //============================================

   // Default Redis Client Create - For Login & Logout
   redis_client := initRedis(config["redis.addr"])
   defer redis_client.Close()

   // default dbytype = "mysql"
   if dbtype == "" {
      dbtype = "mysql"
   }

   // RabbitMQ
   //endpoint := "amqps://929b5a98-6521-4da5-86c1-0fcaa2450a86:55dgq7ocf1gtiamqum0hb9p8be@10.244.9.50:5671/3c14abdc-c922-428c-af70-d61eb91ef818"
   endpoint := readOSEnvironment_mq()
   fmt.Println("########## credentials-uri:", endpoint)
   mq := message.NewRabbitMQ(endpoint)
   if mq != nil {
      defer message.CloseMQ(mq)
   }

   // DB Initialize -
   // DB 타입별로 Connection 처리
   if dbtype == "mysql" {
      maxConnection, err := strconv.Atoi(config["mysql.maxconn"])
      if err != nil {
         log.Println(err)
         os.Exit(-1)
      }

      dbconfig = datasource.NewDBConfig(config["mysql.userid"], config["mysql.userpwd"], config["mysql.dburl"], maxConnection)
      defer dbconfig.CloseDb()

      fmt.Println("dbmap:", dbconfig.DBMAP)
      if dbconfig.DBMAP == nil {
         log.Panic("Couldn't create Database Connection properly - MySQL!!!")
         os.Exit(-1)
      }

      // Route Path 정보와 처리 서비스 연결
      handlers = handler.NewHandler(dbconfig.DBMAP, redis_client, mq.Ch)

   } else if dbtype == "mongodb" {
      mgodbconfig = datasource.NewMgoDBConfig(config["mongodb.userid"], config["mongodb.userpwd"], config["mongodb.dburl"])
      defer mgodbconfig.CloseDb()

      fmt.Println("session:", mgodbconfig.SESSION)
      if mgodbconfig.SESSION == nil {
         log.Panic("Couldn't create Database Connection properly - MongoDB!!!")
         os.Exit(-1)
      }

      // Route Path 정보와 처리 서비스 연결
      handlers = handler.NewMgoHandler(mgodbconfig.SESSION, redis_client, mq.Ch)

   } else {
      fmt.Println("No database type found.")
      os.Exit(-1)
   }

   if err := http.ListenAndServe(fmt.Sprintf(":%v", config["server.port"]), handlers); err != nil {
      log.Fatalln(err)
   }
}

/*
   Read Config file
*/
func ReadConfig(filename string) (Config, error) {
   // init with some bogus data
   config := Config{
      "server.ip":     "127.0.0.1",
      "server.port":   "8080",
      "mysql.dburl":   "",
      "mysql.userid":  "",
      "mysql.userpwd": "",
      "mysql.maxconn": "",
   }

   if len(filename) == 0 {
      return config, nil
   }
   file, err := os.Open(filename)
   if err != nil {
      return nil, err
   }
   defer file.Close()

   reader := bufio.NewReader(file)
   for {
      line, err := reader.ReadString('\n')
      // check if the line has = sign
      // and process the line. Ignore the rest.
      if equal := strings.Index(line, "="); equal >= 0 {
         if key := strings.TrimSpace(line[:equal]); len(key) > 0 {
            value := ""
            if len(line) > equal {
               value = strings.TrimSpace(line[equal+1:])
            }
            // assign the config map
            config[key] = value
         }
      }
      if err == io.EOF {
         break
      }
      if err != nil {
         return nil, err
      }
   }
   return config, nil
}


func initRedis(addr string) *redis.Client {
   client := redis.NewClient(&redis.Options{
      Addr: addr,
   })
   return client
}

~~~

##### 2.3.5. GlusterFS 연동

1).  파일 Upload

-   클라이언트에서 보낸 요청에서 파일을 읽어들여 GlusterFS Server로 전송

~~~
func (h *GlusterFSService) Upload(w http.ResponseWriter, r *http.Request) {
   if err := r.ParseMultipartForm(MAX_MEMORY); err != nil {
      log.Println(err)
      http.Error(w, err.Error(), http.StatusForbidden)
   }
   index := 0
   path := make([]string, len(r.MultipartForm.File))
   for _, fileHeaders := range r.MultipartForm.File {
      for _, fileHeader := range fileHeaders {
         file, _ := fileHeader.Open()
         buf, _ := ioutil.ReadAll(file)
         thumb_img_path, err := ObjectPut(buf, fileHeader.Filename)
         path[index] = "{\"thumb_img_path\":\"" + thumb_img_path + "\"}"
         if err != nil {
            log.Fatalln("!!! ObjectPut error:", err)
         }
      }
      index++
   }
   js, err := json.Marshal(path)
   if err != nil {
      log.Fatalln("Error writing JSON:", err)
   }
   w.Header().Set("Content-Type", "application/json")
   w.WriteHeader(http.StatusOK)
   w.Write([]byte(js))
   return
}
~~~

2).  GlusterFS Server 연결 및 파일전송

-   GlusterFS Server로 연결을 맺고 파일을 전송

~~~
func ObjectPut(buf []byte, fileName string) (string, error) {
   userName, password, authUrl, tenantName := readOSEnvironment_gf()
   c := swift.Connection{
      UserName: userName,
      ApiKey:   password,
      AuthUrl:  authUrl,
      Tenant:   tenantName,
   }
   // Authenticate
   err := c.Authenticate()
   if err != nil {
      panic(err)
   }
   // Container
   _, _, err = c.Container(CONTAINER_NAME)
   if err != nil {
      var meta = swift.Headers{"X-Container-Read": ".r:*"}
      err = c.ContainerCreate(CONTAINER_NAME, meta)
      if err != nil {
         panic(err)
      }
   }
   // Put object
   headers := swift.Headers{}
   headers["Content-Length"] = strconv.FormatInt(int64(len(buf)), 10)
   err = c.ObjectPutBytes(CONTAINER_NAME, fileName, buf, "application/octet-stream")
   if err != nil {
      log.Fatalln("!!! ObjectPut error:", err)
   }
   // Fetch object info and compare
   info, _, err := c.Object(CONTAINER_NAME, fileName)
   if err != nil {
      log.Println(err)
   }
   if info.Bytes != int64(len(buf)) {
      log.Println("Bad length")
   }
   thumb_img_path := c.StorageUrl + "/" + CONTAINER_NAME + "/" + fileName
   return thumb_img_path, err
}
~~~

3).  GlusterFS 연결정보

-   VCAP\_SERVICES에서 정보 추출

~~~
func readOSEnvironment_gf() (string, string, string, string) {
   args := os.Getenv("VCAP_SERVICES")
   var userName, password, authUrl, tenantName string
   if args != "" {
      var vcap_env map[string]interface{}
      if err := json.Unmarshal([]byte(args), &vcap_env); err != nil {
         log.Panic(err.Error())
      }
      //Service instance (not name) - for example : p-mysql - type check !!! []interface{}
      if vcap_env["glusterfs"] != nil {
         sub_env := vcap_env["glusterfs"].([]interface{})
         credentials := (sub_env[0].(map[string]interface{}))["credentials"].(map[string]interface{})
         userName = credentials["username"].(string)
         password = credentials["password"].(string)
         authUrl = credentials["auth_url"].(string)
         tenantName = credentials["tenantname"].(string)
      }
   }
   return userName, password, authUrl, tenantName
}
~~~

#### 2.4. 배포

-   cf cli 명령어를 이용하여 Go 샘플 어플리케이션을 배포한다.

-   cf cli가 설치되어 있고, cf login이 이미 되어 있다는 가정하에 진행한다.

~~~
### Go 샘플 어플리케이션 배포
$ cd “Go 샘플 어플리케이션” 디렉토리
$ source .envrc                 # GO_PATH를 지정하기 위해 

$ cd src/org/openpaas/sample

$ cf push –f manifest.yml        # Go 샘플 어플리케이션 배포
Using manifest file manifest.yml
Creating app go-sample in org org / space space as admin...
OK
Using route go-sample.os.paasxpert.com
Binding go-sample.os.paasxpert.com to go-sample...
OK
Uploading go-sample...
Uploading app files from: /home/ihocho/git/OpenPaaSSample/GoSample/go-sample-app/src/org/openpaas/sample
Uploading 854.2K, 185 files
Done uploading               
OK
Binding service rabbitmq-service-instance to app go-sample in org org / space space as admin...
OK
Starting app go-sample in org org / space space as admin...
Creating container
Successfully created container
Downloading app package...
Downloaded app package (321.1K)
Downloading buildpacks (https://github.com/cloudfoundry/go-buildpack.git)...
Downloaded buildpacks
Staging...
-------> Buildpack version 1.7.0
https://pivotal-buildpacks.s3.amazonaws.com/concourse-binaries/godep/godep-v17-linux-x64.tgz
-----> Checking Godeps/Godeps.json file.
-----> Installing go1.4.2... done
Downloaded [https://storage.googleapis.com/golang/go1.4.2.linux-amd64.tar.gz]
-----> Running: godep go install -tags cloudfoundry ./...
Exit status 0
Staging complete
Uploading droplet, build artifacts cache...
Uploading droplet...
Uploading build artifacts cache...
Uploaded build artifacts cache (60M)
Uploaded droplet (2.8M)
Uploading complete
1 of 1 instances running
App started
OK
App go-sample was started using this command `sample`
Showing health and status for app go-sample in org org / space space as admin...
OK
requested state: started
instances: 1/1
usage: 256M x 1 instances
urls: go-sample.os.paasxpert.com
last uploaded: Mon Dec 14 04:39:52 UTC 2015
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/go-buildpack.git
     state     since                    cpu    memory         disk      details   
#0   running   2015-12-14 01:41:20 PM   0.0%   2.1M of 256M   0 of 1G      
~~~

Go 샘플 애플리케이션에의 데이터 관리는 MySQL, MongoDB 중에 하나를 이용하기 때문에 배포시 환경변수 정보(dbtype)에 데이터베이스 이름(mysql or mongodb)를 설정하여 배포한다..

#### 2.5. 테스트

curl 명령어를 통해 command 창에서 직접 테스트 할 수 있다.

~~~
### 모든 조직정보 조회
curl -v http://”depolyed_sample_app_name”/orgs

### 특정 조직정보 조회
curl -v http:// ”depolyed_sample_app_name”/orgs/{org_id}

### 조직 생성
curl -X POST -v 
–d '{"id":”org_id”,"label":"org_lable_info","desc":"org_description_info","url":"org_uri_info"}' 
http:// ”depolyed_sample_app_name”/orgs

### 조직 수정
curl -X PUT -v
-d '{"label":" org_lable_info ","desc":"org_description_info","url":"org_url_info"}'
http:// ”depolyed_sample_app_name”/orgs/{org_id}

### 조직 삭제
curl -X DELETE -v http:// ”depolyed_sample_app_name”/orgs/{org_id}
~~~
