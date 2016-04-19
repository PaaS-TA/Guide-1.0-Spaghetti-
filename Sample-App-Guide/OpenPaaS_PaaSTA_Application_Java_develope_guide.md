### 1. 개요

#### 1.1. 문서 개요

##### 1.1.1. 목적
본 문서(Java 애플리케이션 개발 가이드)는 Open PaaS프로젝트의 서비스팩(Mysql, Cubrid, MongoDB, RabbitMQ, Radis, GlusterFS)을 Java애플리케이션과 연동하여서비스를 사용하고 애플리케이션을 배포하는 방법에 대해 제시하는 문서이다.

##### 1.1.2. 범위
본 문서의 범위는 Open PaaS 프로젝트의 Java 애플리케이션 개발과 서비스팩 연동에 대한 내용으로 한정되어 있다.

##### 1.1.3. 참고 자료
-   [***http://java.sun.com](http://java.sun.com)
-   [***http://spring.io](http://spring.io)


### 2. JAVA 애플리케이션 개발가이드

#### 2.1. 개요
Open PaaS에 등록된 다양한 서비스팩을 Java언어로 작성된 애플리케이션과 바인딩하고해당 애플리케이션에 바인딩된 환경정보(VCAP_SERVICES)에서 각 서비스별 접속정보를 획득하여 애플리케이션에 적용하여 이용 할 수 있도록 Windows 환경에서 Java 애플리케이션을 작성 할 수 있도록 한다.
 Java Sample Application은 REST 기반의 서비스를 UI를 제공한다. 사용하는 서비스로는 Mysql, Cubrid, Mongodb, Redis, RabbitMq, GlusterFS를 제공하며, Local 및 OpenPaas 환경에서 사용가능하다. 단, GlusterFs는 Local환경에서 Connection정보를 제공하지 않는다.

#### 2.2. 개발환경 구성
Java 애플리케이션 개발을 위해 다음과 같은 환경으로 개발환경을 구성 한다.
-   OS : Windows 7 64bit
-   Java : 1.8
-   Framwork : Spring-Boot
-   IDE : Eclipse
-   의존성: Maven

##### 2.2.1. JDK설치
1.  JDK 다운로드(1.8)

| <span id="__DdeLink__2953_294360055" class="anchor"></span>http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html |
|-----------------------------------------------------------------------------------|

1.   다운로드 JDK1.8

    <img src="./image/java/image1.png" width="612" height="151" />

2. JDK 설치

  -  jdk-8u65-windows-x64.exe 더블클릭하여 설치를 실행한다.

<img src="./image/java/image2.png" width="612" height="300" />

  -  next 버튼 클릭하여 진행한다.

<img src="./image/java/image3.png" width="400" height="250" />

  -  설치가 진행된다.

<img src="./image/java/image4.png" width="400" height="300" />
  -   Close 버튼 클릭하여 설치를 종료한다.

3. JDK  환경설정

  -   내컴퓨터에서 우측 버튼을 클릭하여 속성을 클릭한다.

<img src="./image/java/image5.png" width="400" height="150" />

  -   고급 시스템설정>고급 텝 > 환경변수 버튼 클릭
  -   시스템 변수 영역 > path 클릭 > jdk설치된 디렉토리의 bin을 path로 추가한다.
<img src="./image/java/image6.png" width="400" height="150" />

4.  eclipse 설치

  - eclipse공식 사이트에서 Eclipse IDE for Java EE Developers 를 다운로드 받는다.

| <span id="__DdeLink__2953_294360055" class="anchor"></span>http://www.eclipse.org/downloads/ |
|-----------------------------------------------------------------------------------|

  - eclipse-jee-mars-1-win32-x86_64.zip을 선택하여 압축해제한다.

<img src="./image/java/image7.png" width="400" height="150" />

  - eclipse.exe 를 클릭한다.  
<img src="./image/java/image9.png" width="400" height="150" />

  - eclipse 초기화면이 나타난다.  
<img src="./image/java/image10.png" width="400" height="150" />

  - 좌측 Project Explore.에서 import를 클릭한다.  
<img src="./image/java/image11.png" width="400" height="150" />

  - Existing Maven Project를 클릭한다.  
<img src="./image/java/image12.png" width="400" height="150" />

  - Browse  버튼을 클릭하여 java-sample 프로제트가있는 폴더를 선택한다. Finish 버튼을 클릭한다.
<img src="./image/java/image13.png" width="400" height="150" />

  - java sample Application이 추가 된것을 확인한다.  
<img src="./image/java/image14.png" width="400" height="150" />


#### 2.3. 개발
  - OpenPaas Java Sample Application은 다음과 같은 구조를 지닌다. Spring-Boot library를 사용하여
별도의 Tomcat설정 없이 구동이 가능합니다. 샘플 애플리케이션에의 데이터 관리는 MySQL, CubridDB, MongoDB 중에 하나를 이용하기 때문에 API 요청시 요청 본문에 DBType 값을 가지고 결정합니다.

<img src="./image/java/image15.png" width="500" height="500" />



  - **OpenPaas Java Sample Application Package 구조**

|**패키지/파일**   |**설명**                 |
|----------|-------------------------|
|Java/sample/biz                  |서비스 영역으로 비지니스 영역입니다.  |
|Java/sample/Config/data          |Mysql, mongodb, redis, glusterFs,cubrid,RabbitMq 등 Persistence영역 접속 설정을 하는 Package입니다.  Local 환경/Cloud 환경을 인식하여 접속한다. Local환경에서는 GlusterFs 설정을 지원하지 않습니다.(Cloud Only)|
|Java/sample /Config/web          |MVC, Interceptor, MultiPart관련 WEB 설정 정보를 Sample Application에 설정합니다.              |
|Java/sample /controller          |UI에서 요청한 정보를 수신하는 컨트롤러 영역 입니다.            |
|Java/sample/dao                  |Persistence 영역의 데이터를 입력/수정/삭제/조회하는 영역입니다.               |
|Java/sample /model               |VO 영역입니다.     |
|Java/sample /Application.java    |Spring Boot을 활용하여 Sample Application을 Start합니다.   |
|Resource/tables                  |Sample Application이 사용할 DB Table DDL 및 기초데이터 DDL이 포함되어 있습니다.       |
|Resource/ application-cloud.properties  |Sample Application이 Cloud 환경에서 사용할 Property입니다. |
|Resource/ application.properties        |Sample Application이 Local환경에서 사용할 Property입니다.  |
|Resource/logback.xml                    |Logback을 활용한 logging 설정 입니다.   |
|Manifest.yml                            |클라우드 환경에서 Application이 최초 구성될때 사용하는 파일로, Sample Application의 환경정보, 서비스 정보(mysql, rabbitmq등의 서비스), application명, 인스턴스 갯수, 인스턴스 메모리 설정등이 포함되어 있습니다.     |
|pom.xml                                 |Sample Application의 Maven의존성을 정의 합니다.     |
|webapp                                 |UI관련 리소스 위치     |


##### 2.3.1. 애플리케이션 환경 설정
  - 본 문서는 Cloud환경에서 Java Application이 구동 될 수 있는 방법을 제시합니다. Mysql, Cubrid, Redis등의 설치관련 정보는 제공하지 않습니다.

1)   manifest.yml
  - Cloud  
<img src="./image/java/image1-1.png" width="350" height="300" />

2) resource/application-cloud.properties (application.properties는 Local환경용)
  - Cloud환경에서 적용될 프로퍼티로 Application이 사용할 서비스 명이 설정되어 있습니다.
cubrid 와 GlusterFs는 Spring-cloud-connector에서 정보를 제공하지 않아 Application의 VCAP SERVICE 환경정보를 활용하여 서비스 Connection을 합니다.

```
db.mysql.servicename: mysql-service-instance
mongodb.service.name: mongo-service-instance
redis.service.name:   redis-service-instance
rabbitmq.service.name: rabbitmq-service-instance
```

3) sample/config/data/CloudConfigData.java
  - 애플리케이션의 환경설정  

  ```
  @Profile("cloud") ==> Cloud환경 인식하는 Annotation(manifest.yml의 Env정보와 매팽)
@Configuration    ==> Spring Config
@ServiceScan      ==> Cloud Config Service Scan Annotation
public class CloudDataConfig extends AbstractCloudConfig {

	@Value("${db.mysql.servicename}")
	private String mysqlServiceName; ==> application-cloud.properties의 mysql
                                                서비스 인스턴스명 읽어온다.
	private String cubridJdbcUrl;

	@Value("${mongodb.service.name}")  
	private String mongoServiceName; ==> application-cloud.properties의 mongodb
                                                서비스 인스턴스명 읽어온다.
	@Value("${redis.service.name}")
	private String redisServiceName; ==> application-cloud.properties의 redis
                                                 서비스 인스턴스명 읽어온다
	@Value("${rabbitmq.service.name}")
	private String rabbitServiceName; ==> application-cloud.properties의 redis
                                                 서비스 인스턴스명 읽어온다


	@Bean(name = "dsMysql")
	@Primary
	DataSource mysqlDataSource() {

		CloudFactory cloudFactory = new CloudFactory();
		Cloud cloud = cloudFactory.getCloud();
		ServiceInfo serviceInfo = cloud.getServiceInfo(mysqlServiceName);
		String serviceId = serviceInfo.getId();
		return cloud.getServiceConnector(serviceId, DataSource.class, null);

	}

	@Bean(name = "jdbcMysql")  ==> MySql 서비스 Connection설정
	@Autowired
	public JdbcTemplate mysqlJdbcTemplate(@Qualifier("dsMysql") DataSource dsSlave) {
		return new JdbcTemplate(dsSlave);
	}

	@Bean(name = "dsCubrid")
	public DataSource cubridDataSource() {
		try {

			String vcap_services = System.getenv("VCAP_SERVICES");
			JSONObject jsonObj = JSONObject.fromObject(vcap_services);
			JSONArray userPro = jsonObj.getJSONArray("CubridDB");
			jsonObj = JSONObject.fromObject(userPro.get(0));
			jsonObj = jsonObj.getJSONObject("credentials");
			cubridJdbcUrl = jsonObj.getString("jdbcurl");

			return new SimpleDriverDataSource(cubrid.jdbc.driver.CUBRIDDriver.class.newInstance(), cubridJdbcUrl);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}

	@Bean(name = "jdbcCubrid")  ==> Cubrid 서비스 Connection설정
	@Autowired
	public JdbcTemplate cubridJdbcTemplate(@Qualifier("dsCubrid") DataSource dsSlave) {
		return new JdbcTemplate(dsSlave);
	}

	/**
	 * MongoDBFactory
	 */
	@Autowired
	MongoDbFactory mongoDbFactory;


	@Bean(name = "mongoTemplate")   ==> MongoDB 서비스 Connection설정
	public MongoTemplate mongoTemplate() throws UnknownHostException {

		CloudFactory cloudFactory = new CloudFactory();
		Cloud cloud = cloudFactory.getCloud();
		MongoServiceInfo serviceInfo = (MongoServiceInfo) cloud.getServiceInfo(mongoServiceName);

		// MongoDB 인증 처리
		mongoDbFactory.getDb().authenticate(serviceInfo.getUserName(), serviceInfo.getPassword().toCharArray());

		MongoTemplate mongoTemplate = new MongoTemplate(mongoDbFactory);

		return mongoTemplate;
	}

	@Bean  ==> Redis 서비스 Connection설정
	public JedisPool redisTemplate() {

		CloudFactory cloudFactory = new CloudFactory();
		Cloud cloud = cloudFactory.getCloud();
		RedisServiceInfo serviceInfo = (RedisServiceInfo) cloud.getServiceInfo(redisServiceName);

		JedisPoolConfig poolConfig = new JedisPoolConfig();
		poolConfig.setMaxTotal(128);
		return new JedisPool(poolConfig, serviceInfo.getHost(), serviceInfo.getPort(), 5, serviceInfo.getPassword());
	}

	@Bean
	@Primary
	public CachingConnectionFactory rabbitmqConnectionFactory() throws IOException {

		CloudFactory cloudFactory = new CloudFactory();
		Cloud cloud = cloudFactory.getCloud();

		AmqpServiceInfo serviceInfo = (AmqpServiceInfo) cloud.getServiceInfo(rabbitServiceName);

		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost(serviceInfo.getHost());
		connectionFactory.setPort(serviceInfo.getPort());
		connectionFactory.setUsername(serviceInfo.getUserName());
		connectionFactory.setPassword(serviceInfo.getPassword());
		connectionFactory.setVirtualHost(serviceInfo.getVirtualHost());

		try {
			// SslProtocol 사용 설정
			connectionFactory.useSslProtocol("TLS");

		} catch (KeyManagementException e) {
			e.printStackTrace();
		} catch (NoSuchAlgorithmException e) {
			e.printStackTrace();
		}

		CachingConnectionFactory cachingConnectionFactory = new CachingConnectionFactory(connectionFactory);

		return cachingConnectionFactory;
	}

	@Bean  ==> RabbitMQ 서비스 Connection설정
	public RabbitTemplate amqpTemplate(
			@Qualifier("rabbitmqConnectionFactory") CachingConnectionFactory connectionFactory) {

		RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);

		rabbitTemplate.setQueue("sampleQueue");
		rabbitTemplate.setRoutingKey("sampleQueue");
		rabbitTemplate.setConnectionFactory(connectionFactory);
		return rabbitTemplate;
	}

	@Bean
	public RabbitAdmin rabbitAdmin(@Qualifier("rabbitmqConnectionFactory") CachingConnectionFactory connectionFactory) {

		RabbitAdmin admin = new RabbitAdmin(connectionFactory);
		return admin;
	}

	@Autowired
	Gson gson;

	@Bean  ==> GlusterFs 서비스 Connection설정
	public AccountConfig accountConfig(){

		String vcap_services = System.getenv("VCAP_SERVICES");
        JsonObject jsonObj = gson.fromJson(vcap_services, JsonElement.class).getAsJsonObject();

        JsonArray userPro = jsonObj.getAsJsonArray("glusterfs");
        jsonObj = userPro.get(0).getAsJsonObject().getAsJsonObject("credentials");
//        String provider = jsonObj.get("provider").getAsString();
        String tenantName = jsonObj.get("tenantname").getAsString();
        String username = jsonObj.get("username").getAsString();
        String password = jsonObj.get("password").getAsString();
        String authUrl = jsonObj.get("auth_url").getAsString();

		AccountConfig config = new AccountConfig();
		config.setUsername(username);
		config.setTenantName(tenantName);
		config.setPassword(password);
		config.setAuthUrl(authUrl + "/tokens");
		config.setAuthenticationMethod(AuthenticationMethod.KEYSTONE);
		return config;
	}

}
  ```

4) pom.xml
   - Sample Application에서 사용하는 Maven Dependency


   ```
      <parent>
     		<groupId>org.springframework.boot</groupId>
     		<artifactId>spring-boot-starter-parent</artifactId>
     		<version>1.2.3.RELEASE</version>
     		<relativePath />
     	</parent>

     <dependency>
     			<groupId>org.springframework.cloud</groupId>
     			<artifactId>spring-cloud-starter-config</artifactId>
     		</dependency>

     		<dependency>
     			<groupId>org.springframework.boot</groupId>
     			<artifactId>spring-boot-starter-web</artifactId>
     		</dependency>
     		<dependency>
     			<groupId>org.springframework.boot</groupId>
     			<artifactId>spring-boot-starter-data-jpa</artifactId>
     		</dependency>

     		<dependency>
     			<groupId>org.springframework.boot</groupId>
     			<artifactId>spring-boot-configuration-processor</artifactId>
     			<optional>true</optional>
     		</dependency>
 ==>Spring Boot 의존성


    <!-- Spring Cloud Connector Start -->
 		<dependency>
 			<groupId>org.springframework.cloud</groupId>
 			<artifactId>spring-cloud-spring-service-connector</artifactId>
 		</dependency>

 		<dependency>
 			<groupId>org.springframework.cloud</groupId>
 			<artifactId>spring-cloud-core</artifactId>
 		</dependency>

 		<dependency>
 			<groupId>org.springframework.cloud</groupId>
 			<artifactId>spring-cloud-cloudfoundry-connector</artifactId>
 		</dependency>
 		<!-- Spring Cloud Connector End -->

 ==> Spring Cloud Connector 의존성


 		<!-- MongoDB Dependency Start -->
 		<dependency>
 			<groupId>org.mongodb</groupId>
 			<artifactId>mongo-java-driver</artifactId>
 			<version>2.13.3</version>
 		</dependency>

 		<dependency>
 			<groupId>org.springframework.boot</groupId>
 			<artifactId>spring-boot-starter-data-mongodb</artifactId>
 			<exclusions>
 				<exclusion>
 					<groupId>org.mongodb</groupId>
 					<artifactId>mongo-java-driver</artifactId>
 				</exclusion>
 			</exclusions>
 		</dependency>
 		<!-- MongoDB Dependency End -->

 	<!-- MongoDB Dependency Start -->
 		<dependency>
 			<groupId>org.mongodb</groupId>
 			<artifactId>mongo-java-driver</artifactId>
 			<version>2.13.3</version>
 		</dependency>

 		<dependency>
 			<groupId>org.springframework.boot</groupId>
 			<artifactId>spring-boot-starter-data-mongodb</artifactId>
 			<exclusions>
 				<exclusion>
 					<groupId>org.mongodb</groupId>
 					<artifactId>mongo-java-driver</artifactId>
 				</exclusion>
 			</exclusions>
 		</dependency>
 		<!-- MongoDB Dependency End -->

 		<!-- MySql Connector Start -->
 		<dependency>
 			<groupId>mysql</groupId>
 			<artifactId>mysql-connector-java</artifactId>
 		</dependency>
 		<!-- MySql Connector End -->

 		<!-- Redis Start -->
 		<dependency>
 			<groupId>org.springframework.boot</groupId>
 			<artifactId>spring-boot-starter-redis</artifactId>
 		</dependency>

 		<dependency>
 			<groupId>org.apache.commons</groupId>
 			<artifactId>commons-pool2</artifactId>
 		</dependency>
 		<!-- Redis End -->

 		<!-- RabbitMQ Start -->
 		<dependency>
 			<groupId>org.springframework.boot</groupId>
 			<artifactId>spring-boot-starter-amqp</artifactId>
 		</dependency>

 		<!-- RabbitMQ End -->

 		<!-- Java client for OpenStack Storage Swift(GlusterFS) 설정 -->
 		<!-- GlusterFS Start -->
 		<dependency>
 			<groupId>org.javaswift</groupId>
 			<artifactId>joss</artifactId>
 			<version>0.9.8</version>
 		</dependency>
 		<!-- GlusterFS End -->
   ```
#### 2.3. 개발

  - Java Sample Application은 Spring Boot으로 구성되어 있어 별도의 Tomcat 설정이 필요하지 않습니다.
java package Root에 있는 Application.java 우측 버튼을 클릭 --> Run As --> Java Application실행하면 Sample Application이 실행됩니다.

<img src="./image/java/image17.png" width="350" height="400" />


#### 2.4. 배포
  - cf cli 명령어를 이용하여 Java 샘플 어플리케이션을 배포한다
  - java-sample application이 maven install을 실행하여 war가 존재하여야 한다.
  - cf cli가 설치되어 있고, cf login이 이미 되어 있다는 가정하에 진행한다.
 ```
 ### Java 샘플 어플리케이션 배포
 $ cd “Java 샘플 어플리케이션” 디렉토리

 $cd  /home/csupshin/git/OpenPaaSSample/java-sample-app
 $ cf push
 Using manifest file /home/csupshin/git/OpenPaaSSample/java-sample-app/manifest.yml

Updating app java-sample in org OCP / space dev as admin...
OK

Using route java-sample.52.71.31.153.xip.io
Uploading java-sample...
Uploading app files from: /home/csupshin/git/OpenPaaSSample/java-sample-app/lib/java-sample.war
Uploading 1.6M, 227 files
Done uploading               
OK
Binding service mysql-service-instance to app java-sample in org OCP / space dev as admin...
OK
Binding service cubrid-service-instance to app java-sample in org OCP / space dev as admin...
OK
Binding service redis-service-instance to app java-sample in org OCP / space dev as admin...
OK
Binding service rabbitmq-service-instance to app java-sample in org OCP / space dev as admin...
OK
Binding service glusterfs-service-instance to app java-sample in org OCP / space dev as admin...
OK

Stopping app java-sample in org OCP / space dev as admin...
OK

Starting app java-sample in org OCP / space dev as admin...
Creating container
Successfully created container
Downloading app package...
Downloaded app package (30.5M)
Downloading buildpacks (java_buildpack), build artifacts cache...
Downloading java_buildpack...
Downloading build artifacts cache...
Downloaded java_buildpack
Downloaded build artifacts cache (44.7M)
Downloaded buildpacks
Staging...
-----> Java Buildpack Version: v3.3.1 | https://github.com/cloudfoundry/java-buildpack.git#063836b
-----> Downloading Open Jdk JRE 1.8.0_73 from https://download.run.pivotal.io/openjdk/trusty/x86_64/openjdk-1.8.0_73.tar.gz (found in cache)
       Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (2.1s)
-----> Downloading Open JDK Like Memory Calculator 2.0.1_RELEASE from https://download.run.pivotal.io/memory-calculator/trusty/x86_64/memory-calculator-2.0.1_RELEASE.tar.gz (found in cache)
       Memory Settings: -Xss995K -Xmx382293K -XX:MaxMetaspaceSize=64M -Xms382293K -XX:MetaspaceSize=64M
-----> Downloading Spring Auto Reconfiguration 1.10.0_RELEASE from https://download.run.pivotal.io/auto-reconfiguration/auto-reconfiguration-1.10.0_RELEASE.jar (found in cache)
       Modifying /WEB-INF/web.xml for Auto Reconfiguration
Exit status 0
Staging complete
Uploading droplet, build artifacts cache...
Uploading droplet...
Uploading build artifacts cache...
Uploaded build artifacts cache (44.7M)
Uploaded droplet (75.4M)
Uploading complete

0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
0 of 1 instances running, 1 starting
1 of 1 instances running

App started


OK

App java-sample was started using this command `CALCULATED_MEMORY=$($PWD/.java-buildpack/open_jdk_jre/bin/java-buildpack-memory-calculator-2.0.1_RELEASE -memorySizes=metaspace:64m.. -memoryWeights=heap:75,metaspace:10,native:10,stack:5 -memoryInitials=heap:100%,metaspace:100% -totMemory=$MEMORY_LIMIT) && SERVER_PORT=$PORT $PWD/.java-buildpack/open_jdk_jre/bin/java -cp $PWD/.:$PWD/.java-buildpack/spring_auto_reconfiguration/spring_auto_reconfiguration-1.10.0_RELEASE.jar -Djava.io.tmpdir=$TMPDIR -XX:OnOutOfMemoryError=$PWD/.java-buildpack/open_jdk_jre/bin/killjava.sh $CALCULATED_MEMORY org.springframework.boot.loader.WarLauncher`

Showing health and status for app java-sample in org OCP / space dev as admin...
OK

requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: java-sample.52.71.31.153.xip.io
last uploaded: Mon Feb 15 06:47:21 UTC 2016
stack: cflinuxfs2
buildpack: java_buildpack

     state     since                    cpu     memory         disk           details   
#0   running   2016-02-15 03:49:33 PM   88.2%   341M of 512M   185.2M of 1G
 ```

#### 2.5. Sample App 배포 확인

1) cf cli 명령어를 이용하여 Java 샘플 어플리케이션의 배포 상태 및 URL을 확인한다.

```
$ cf apps
Getting apps in org OCP / space dev as admin...
OK

name                     requested state   instances   memory   disk   urls   
java-sample              started           1/1         512M     1G     java-sample.52.71.31.153.xip.io   
```

#### 2.6. Sample App 로그인
1) 브라우저에서 java-sample.52.71.31.153.xip.io 에 접속하면 로그인 화면이 나타납니다.
admin/admin 입력후 로그인 합니다.

<img src="./image/java/image18.png" width="350" height="300" />

2) 로그인이 완료되면 다음 조직 화면이 나타납니다. 화면 하단에 집 아이콘을 클릭하면 해당 조직의 상세 조직 화면이 나타납니다. 조직명을 클릭하면 조직의 에하 조직 목록을 조회 할 수 있습니다.

<img src="./image/java/image19.png" width="350" height="150" />
