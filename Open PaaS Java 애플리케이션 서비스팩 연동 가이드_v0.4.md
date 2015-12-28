# Table Of Contents

1. [개요](#1-개요)
  1.1. [문서 개요](#11-문서-개요)
  1.1.1. [목적](#111-목적)
  1.1.2. [범위](#112-범위)
  1.1.3. [참고 자료](#113-참고-자료)
2. [Java 애플리케이션 개발가이드](#2-java-애플리케이션-개발가이드)
  2.1. [개요](#21-개요)
  2.2. [개발환경 구성](#22-개발환경-구성)
  2.2.1. [자바 개발 킷 설치](#221-자바-개발-킷-설치)
  2.2.2. [STS 설치](#222-sts-설치)
  2.3. [개발](#23-개발)
  2.3.1. [공통](#231-공통)
  2.3.2. [VCAP_SERVICES 환경설정 정보](#232-vcap_services-환경설정-정보)
  2.3.3. [바인딩된 서비스 설정만 활성화](#233-바인딩된-서비스-설정만-활성화)
  2.3.4. [Mysql 연동](#234-mysql-연동)
  2.3.5. [Cubrid 연동](#235-cubrid-연동)
  2.3.6. [MongoDB 연동](#236-mongodb-연동)
  2.3.7. [Redis 연동](#237-redis-연동)
  2.3.8. [RabbitMQ 연동](#238-rabbitmq-연동)
  2.3.9. [GlusterFS 연동](#239-glusterfs-연동)
  2.4. [배포](#24-배포)
  2.4.1. [CLI에서 배포](#241-cli에서-배포)
  2.4.2. [OpenPaas 개발 환경에서 배포](#242-openpaas-개발-환경에서-배포)
  2.5. [테스트](#25-테스트)
  2.5.1. [단위 테스트](#251-단위-테스트)
  2.5.2. [통합 테스트](#252-통합-테스트)



# 1. 개요

### 1.1. 문서 개요

##### 1.1.1. 목적
본 문서(Java 애플리케이션 개발 가이드)는 Open PaaS 프로젝트의 서비스팩(Mysql, Cubrid, MongoDB, RabbitMQ, Radis, GlusterFS)을 Java 애플리케이션과 연동하여 서비스를 사용하고 애플리케이션을 배포하는 방법에 대해 제시하는 문서이다.

##### 1.1.2. 범위
본 문서의 범위는 Open PaaS 프로젝트의 Java 애플리케이션 개발과 서비스팩 연동에 대한 내용으로 한정되어 있다.

##### 1.1.3. 참고 자료
-   [***http://cloud.spring.io/spring-cloud-connectors/***](http://cloud.spring.io/spring-cloud-connectors/)
-   [***http://projects.spring.io/spring-data-jpa/***](http://projects.spring.io/spring-data-jpa/)
-   [***http://projects.spring.io/spring-data-mongodb/***](http://projects.spring.io/spring-data-mongodb/)
-   [***http://projects.spring.io/spring-data-redis/***](http://projects.spring.io/spring-data-redis/)
-   [***http://projects.spring.io/spring-amqp/***](http://projects.spring.io/spring-amqp/)
-   [***http://modelmapper.org/***](http://modelmapper.org/)
-   [***http://joss.javaswift.org/***](http://joss.javaswift.org/)
-   [***http://www.cubrid.org/manual/93/ko/api/jdbc.html***](http://www.cubrid.org/manual/93/ko/api/jdbc.html)
-   [***https://github.com/google/gson***](https://github.com/google/gson)


# 2. Java 애플리케이션 개발가이드

### 2.1. 개요
Open PaaS에 등록된 다양한 서비스팩을 Java 언어로 작성된 애플리케이션과 바인딩하고 해당 애플리케이션에 바인딩된 환경정보(VCAP_SERVICES)에서 각 서비스별 접속정보를 획득하여 애플리케이션에 적용하여 이용 할 수 있도록 Windows 환경에서 Java 애플리케이션을 작성 할 수 있도록한다.

### 2.2. 개발환경 구성
Java 애플리케이션 개발을 위해 다음과 같은 환경으로 개발환경을 구성한다.
-   OS : Windows 7 64bit
-   Java : jdk1.7
-   Framwork : Spring 4.1.8.RELEASE
-   Build : Maven 3.x.x
-   IDE : STS(Spring Tool Suite) 3.7.0.RELEASE


##### 2.2.1. 자바 개발 킷 설치

1. 아래 링크에 접속하여 “JavaSE Download” 페이지가 표시되면 중앙의 “JDK Download” 링크를 클릭하여 라이선스 동의 페이지로 이동한다.
[***http://www.oracle.com/technetwork/java/javase/downloads/index.html***](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
2. 라이선스에 동의한 후 개발환경에 맞는 자바 개발 킷 설치 파일을 다운로드 받는다.
3. 설치 파일을 실행하여 화면에 표시되는 절차에 따라 설치를 완료한다.
  ![java install](images/openpaas-sample-java-connector/image1.jpg)

4. 명령 프롬프트에서 자바 명령어 실행을 위해 시스템 속성의 환경
    변수를 설정한다.
  ![env var1](images/openpaas-sample-java-connector/image2.jpg)\
  ![env var2](images/openpaas-sample-java-connector/image3.jpg)\
  ![env var3](images/openpaas-sample-java-connector/image4.jpg)\

5. 명령 프롬프트에서 “java –version” 명령을 실행하여 설치된 자바 버전이
    정상적으로 출력되는지 확인한다.
  ![java version](images/openpaas-sample-java-connector/image5.jpg)


##### 2.2.2. STS 설치
Spring으로 개발을 하기 위해서 쉬운 환경 세팅 방법은 STS(Spring Tool
Suite)을 설치하는 것이다

1. Spring 홈페이지의 tools 페이지(**https://spring.io/tools**)에
    접속하여 Download STS를 클릭하여 다운로드 받는다.
    ![](images/openpaas-sample-java-connector/image6.png)

2. 다운로드 받은 파일의 압축을 해제하고 “STS.exe” 파일을 실행하여 별도의 설치 없이 STS를 사용할 수 있다.

### 2.3. 개발
샘플 애플리케이션을 사용하기 위해서는 각각 서비스마다 설정을 해주어야
한다.

##### 2.3.1. 공통
각 클라우드 서비스를 이용하기 위해 공통적으로 설정해야 하는 부분을
기술한다.
1)  pom.xml 수정
    -   Maven을 설정하는 pom.xml파일에서 Spring Cloud Connector
    의존성을 설정한다.
    -   Spring Cloud Connector는 서비스 연결과 클라우드 환경 인식을
    단순화 시킨다.

```xml
<!-- Spring Cloud Connector 설정 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-spring-service-connector</artifactId>
    <version>1.2.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-cloudfoundry-connector</artifactId>
    <version>1.2.0.RELEASE</version>
</dependency>
```

##### 2.3.2. VCAP_SERVICES 환경설정 정보
개방형 플랫폼에 배포되는 애플리케이션이 바인딩된 서비스별 접속 정보를
얻기 위해서는 애플리케이션별로 등록되어있는 VCAP_SERVICES 환경변수
정보를 읽어들여 정보를 획득 할 수 있다.

1)  개방형 플랫폼의 애플리케이션 환경정보
    -   서비스를 바인딩하면 JSON 형태로 환경설정 정보가 아래와 같이
        애플리케이션 별로 등록된다.

```json
 {
  "VCAP_SERVICES": {
    "p-mysql": [
      {
        "credentials": {
          "hostname": "10.30.40.63",
          "jdbcUrl": "jdbc:mysql://10.30.40.63:3306/cf_ea68784e_3de6_439d_afc1_d51b4e95627b?user=ZwCFnQRiT3KANqHZ\u0026password=qs7oqi4nSvWq6UQa",
          "name": "cf_ea68784e_3de6_439d_afc1_d51b4e95627b",
          "password": "qs7oqi4nSvWq6UQa",
          "port": 3306,
          "uri": "mysql://ZwCFnQRiT3KANqHZ:qs7oqi4nSvWq6UQa@10.30.40.63:3306/cf_ea68784e_3de6_439d_afc1_d51b4e95627b?reconnect=true",
          "username": "ZwCFnQRiT3KANqHZ"
        },
        "label": "p-mysql",
        "name": "sample-mysql-instance",
        "plan": "100mb",
        "tags": [
          "mysql"
        ]
      }
    ],
  (이하 생략)
```


##### 2.3.3. 바인딩된 서비스 설정만 활성화
1. SpringApplicationContextInitializer.java 설정
    - Cloud 객체에서 바인딩된 서비스 정보를 받아올 수 있다. 해당 서비스
    설정만 활성화 시켜준다.

```java
@Override
public void initialize(ConfigurableApplicationContext applicationContext) {
  ConfigurableEnvironment appEnvironment = applicationContext.getEnvironment();
  String[] persistenceProfiles = getCloudServicesId();

  //서비스 아이디로 프로필 활성화
  for (String persistenceProfile : persistenceProfiles)
    appEnvironment.addActiveProfile(persistenceProfile);
}

/**
 * 바인딩된 서비스들의 아이디배열을 반환
 * @return
 */
public String[] getCloudServicesId() {
  Cloud cloud = getCloud();
  if (cloud == null)
    return null;

  List<String> ids = new ArrayList<String>();
  //바인딩된 서비스 정보를 가져옴.
  List<ServiceInfo> serviceInfos = cloud.getServiceInfos();

  //바인딩된 서비스의 아이디를 id리스트에 저장.
  for (ServiceInfo serviceInfo : serviceInfos)
    ids.add(serviceInfo.getId());

  if (ids.size() > 0)
    return ids.toArray(new String[ids.size()]);

  return null;
}

/**
 * 현재 환경에 적합한 클라우드 객체를 반환.
 * @return
 */
private Cloud getCloud() {
  try {
    CloudFactory cloudFactory = new CloudFactory();
    return cloudFactory.getCloud();
  } catch (CloudException ce) {
    return null;
  }
}
```


##### 2.3.4 Mysql 연동

Mysql 서비스를 연동 및 설정을 한다.

###### pom.xml 수정
- pom.xml파일에서 MariaDB JDBC Driver 의존성을 설정한다.

```xml
<!-- MariaDB JDBC Driver -->
<dependency>
    <groupId>org.mariadb.jdbc</groupId>
    <artifactId>mariadb-java-client</artifactId>
    <version>1.2.3</version>
</dependency>
```

###### Mysql 설정
- 설정 클래스는 AbstractCloudConfig 클래스를 상속받아 바인딩된 서비스에 접근할 수 있다..
- 아래와 같이 Spring Cloud Connector를 이용하여 서비스 아이디로 dataSource객체를 가져올 수 있다.

```java
@Profile("Mysql Service ID")
@Configuration
public class CloudMysqlConfig extends AbstractCloudConfig {
  @Bean
  public DataSource mysqlDataSource(){
    return connectionFactory().dataSource("Mysql Service ID");
  }
}
```

##### 2.3.5. Cubrid 연동
Cubrid 서비스를 연동 및 설정을 한다..

###### pom.xml 수정
- pom.xml파일에 Cubrid JDBC Driver 의존성을 설정한다.

```xml
<!-- Cubrid JDBC Driver -->
<dependency>
    <groupId>cubrid</groupId>
    <artifactId>cubrid-jdbc</artifactId>
    <version>9.3.2.0016</version>
</dependency>
```

###### Cubrid 설정
- Cubrid는 Spring Cloud Connector에서 서비스 바인딩을 제공하지 않는다. 따라서 VCAP_SERVICES 환경변수를 읽어와 직접 DataSource를 설정한다.
- VCAP_SERVICES는 JSON 형태로 되어 있으므로 Gson을 사용하여 파싱하였다.

```java
@Profile("Cubrid Service ID")
@Configuration
public class CloudCubridConfig {
  @Autowired
  Gson gson;

  @Bean
  public DataSource cubridDataSource() throws Exception{
    String vcap_services = System.getenv("VCAP_SERVICES");

    JsonObject jsonObj = gson.fromJson(vcap_services, JsonElement.class).getAsJsonObject();
    JsonArray userPro = jsonObj.getAsJsonArray("CubridDB");
    jsonObj = userPro.get(0).getAsJsonObject().getAsJsonObject("credentials");
    String jdbcurl = jsonObj.get("jdbcurl").getAsString();

    CUBRIDDataSource dataSource = new CUBRIDDataSource();
    dataSource.setUrl(jdbcurl);
    return dataSource;
  }
}
```

##### 2.3.6. MongoDB 연동
MongoDB 서비스를 연동 및 설정을 한다.

###### pom.xml 수정
- pom.xml파일에 spring-data-mongodb 의존성을 설정한다.

```xml
<!-- Spring data mongodb -->
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-mongodb</artifactId>
    <version>1.7.2.RELEASE</version>
</dependency>
```

###### MongoDB 설정
- 설정 클래스는 AbstractCloudConfig 클래스를 상속받아 바인딩된 서비스에 접근할 수 있다..
- Spring Cloud Connector에서 MongoDB 정보를 가져와 MongoTemplate을 설정한다.

```java
@Profile("MongoDB Service ID")
@Configuration
@EnableMongoRepositories(basePackageClasses={MongoGroupRepository.class})
@ComponentScan(basePackageClasses={MongoDBPopulator.class, MongoGroupServiceImpl.class})
public class CloudMongoConfig extends AbstractCloudConfig {
  @Bean
  public MongoDbFactory mongoDbFactory() throws Exception{
    return connectionFactory().mongoDbFactory();
  }

  @Bean
  public MongoTemplate mongoTemplate(MongoDbFactory mongoDbFactory) {
    return new MongoTemplate(mongoDbFactory);
  }
}
```

##### 2.3.7 Redis 연동
Redis 서비스를 연동 및 설정을 한다.

###### pom.xml 수정
- pom.xml파일에서 jedis와 spring-data-redis 의존성을 설정한다.
```xml
<!-- jedis Connector -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.7.2</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>1.6.1.RELEASE</version>
</dependency>
```
###### Redis 설정
- Spring Cloud Connector에서 Redis 정보를 가져와 RedisTemplate을 설정한다.

```java
@Profile("Redis Service ID")
@Configuration
public class CloudRedisConfig extends AbstractCloudConfig {
  @Bean
  public RedisConnectionFactory redisConnectionFactory() throws Exception{
    return connectionFactory().redisConnectionFactory();
  }

  @Bean
  public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory redisConnectionFactory){
    RedisTemplate<String, String> redisTemplate = new RedisTemplate<String, String>();

    redisTemplate.setConnectionFactory(redisConnectionFactory);
    RedisSerializer<String> stringSerializer = new StringRedisSerializer();
    redisTemplate.setDefaultSerializer(stringSerializer);
    return redisTemplate;
  }
}
```

##### 2.3.8 RabbitMQ 연동
RabbitMQ 서비스를 연동 및 설정을 한다..

###### pom.xml 수정
- Maven을 설정하는 pom.xml파일에 dependency 설정을 하여 RabbitMQ Connector를 다운받는다.

```xml
<!-- RabbitMQ -->
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-amqp</artifactId>
    <version>1.4.3.RELEASE</version>
    <exclusions>
        <exclusion>
            <groupId>com.sun.jmx</groupId>
            <artifactId>jmxri</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-rabbit</artifactId>
    <version>1.4.3.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-erlang</artifactId>
    <version>1.4.3.RELEASE</version>
</dependency>
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>1.4.3.RELEASE</version>
</dependency>
```

###### CloudRabbitConfig.java 설정
- Spring Cloud Connector에서 RabbitMQ 정보를 가져와 RabbitTemplate을 설정한다.

```java
@Profile("RabbitMQ Service ID")
@Configuration
public class CloudRabbitConfig extends AbstractCloudConfig{
  @Bean(name = "rabbitmqConnectionFactory")
  @Primary
  public CachingConnectionFactory rabbitConnectionFactory() {
    CloudFactory cloudFactory = new CloudFactory();
    Cloud cloud = cloudFactory.getCloud();

    AmqpServiceInfo serviceInfo = (AmqpServiceInfo)cloud.getServiceInfo("RabbitMQ Service ID");
    ConnectionFactory connectionFactory = new ConnectionFactory();
    connectionFactory.setHost(serviceInfo.getHost());
    connectionFactory.setPort(5671);
    connectionFactory.setUsername(serviceInfo.getUserName());
    connectionFactory.setPassword(serviceInfo.getPassword());
    connectionFactory.setVirtualHost(serviceInfo.getVirtualHost());

    try {
      connectionFactory.useSslProtocol("TLS");
    } catch (KeyManagementException e) {
      e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
      e.printStackTrace();
    }

    return new CachingConnectionFactory(connectionFactory);
  }

  @Bean(name="dynamicExchange")
  public RabbitTemplate devideQueueTemplate(@Qualifier("rabbitmqConnectionFactory") CachingConnectionFactory connectionFactory){
    RabbitTemplate rt = new RabbitTemplate();
    rt.setConnectionFactory(connectionFactory);
    return rt;
  }

  @Bean
  public RabbitAdmin rabbitAdmin(@Qualifier("rabbitmqConnectionFactory") CachingConnectionFactory connectionFactory) {
    RabbitAdmin admin = new RabbitAdmin(connectionFactory);
    return admin;
  }
}
```

##### 2.3.9. GlusterFS 연동
GlusterFS 서비스를 연동 및 설정을 한다..

###### pom.xml 수정
- pom.xml파일에 Swift Connector joss 의존성을 설정한다.

```xml
<!-- Java client for OpenStack Storage Swift(GlusterFS) 설정 -->
<dependency>
    <groupId>org.javaswift</groupId>
    <artifactId>joss</artifactId>
    <version>0.9.8</version>
</dependency>
```

###### GlusterFS 설정
- GlusterFS는 Spring Cloud Connector에서 서비스 바인딩을 제공하지 않는다. 따라서 VCAP_SERVICES 환경변수를 읽어와 직접 연결을 설정한다.

```java
@Profile("GlusterFS Service ID")
@Configuration
public class CloudGlusterfsConfig {
  @Autowired
  Gson gson;

  @Bean
  public AccountConfig accountConfig(){
    String vcap_services = System.getenv("VCAP_SERVICES");

    JsonObject jsonObj = gson.fromJson(vcap_services, JsonElement.class).getAsJsonObject();
    JsonArray userPro = jsonObj.getAsJsonArray("glusterfs");
    jsonObj = userPro.get(0).getAsJsonObject().getAsJsonObject("credentials");

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

  @Bean
  public AccountFactory accountFactory(AccountConfig accountConfig){
    return new AccountFactory(accountConfig);
  }

  @Bean
  public Account account(AccountFactory factory){
    return factory.createAccount();
  }
}
```

##### 2.4. 배포
애플리케이션을 배포하는 방법은 2가지 방법이 있다.

**권장 사항**
> 메모리 제한(Memory Limit)를 1024(MB)이상으로 설정해준다.

**주의 사항**
>서비스팩 바인딩 시에 아래와 같이 서비스팩 인스턴스의 이름과 샘플
애플리케이션의 서비스 ID 이름과 일치 시켜 주어야한다. 서비스의
이름으로 설정을 활성화 시키기 때문에 이름이 일치하지 않으면 설정이
활성화 되지 않거나 오류가 발생한다.
![](images/openpaas-sample-java-connector/image7.png)
![](images/openpaas-sample-java-connector/image8.png)

##### 2.4.1. CLI에서 배포
Openpaas 개발 환경에서 배포하는 과정은 “OpenPaaS 개발 환경 사용 가이드 - 5.1. 애플리케이션 배포”를 참고한다.

**주의 사항**
>배포 시에 서비스팩 바인딩을 꼭 해주어야 한다.

##### 2.4.2. OpenPaas 개발 환경에서 배포
Openpaas 개발 환경에서 배포하는 과정은 “OpenPaaS 개발 환경 사용 가이드 - 5.1. 애플리케이션 배포”를 참고한다.

**주의 사항**
> 배포 시에 서비스팩 바인딩을 꼭 해주어야 한다.

### 2.5. 테스트
테스트는 단위 테스트와 통합 테스트 2가지가 있으며 모두 Junit 으로
작성되어 있다.
단위 테스트는 패키지 별로 작성이 되어 있으며 통합테스트는
openpaas.sample.integration 패키지에 작성되어 있다.

##### 2.5.1 단위 테스트
###### maven-surefire-plugin 설정
- 단위테스트 코드와 통합 테스트 코드가 같은 설정환경에서 실행되는 것을 방지하기 위해 단위테스트 실행시 통합 테스트를 제외한다.
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <argLine>
            -Dfile.encoding=UTF-8
        </argLine>
        <excludes>
            <exclude>**/integration/*.java</exclude>
        </excludes>
    </configuration>
</plugin>
```

###### 단위 테스트 실행
```
mvn test
```

##### 2.5.2. 통합 테스트
###### spring-cloud-localconfig-connector 의존성 추가
- spring-cloud-localconfig-connector는 로컬환경에서 서비스를 설정할 수 있도록 지원한다.
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-localconfig-connector</artifactId>
    <version>1.2.0.RELEASE</version>
    <scope>test</scope>
</dependency>
```

###### 서비스 설정을 위한 properties 파일을 작성한다.
- 설정 파일은 애플리케이션 ID와 서비스 URI 설정을 반드시 포함해야 한다.
- 각 URI의 스킴에 따라 알맞은 서비스 클래스가 활성화 되고 URI가 없거나 알 수 없는 스킴이면 UriBasedServiceInfo 클래스를 기본으로 하여 활성화된다.
- 실제 테스트 수행시 URI는 서비스 환경에 맞게 변경해야 한다.

```
spring.cloud.appId: 애플리케이션 이름
spring.cloud.{서비스아이디} : 서비스 URI

ex)
spring.cloud.appId: openpaas_sample
spring.cloud.openpaas-sample-mysql: mysql://user:passwordi@localhost:3306/dbname?reconnect=true
spring.cloud.openpaas-sample-cubrid:
spring.cloud.openpaas-sample-mongodb: mongodb://user:password@localhost:27017/dbname
spring.cloud.openpaas-sample-glusterfs:
spring.cloud.openpaas-sample-redis: redis://:password@localhost:58689/
spring.cloud.openpaas-sample-rabbitmq: amqps://user:password@localhost/vhost
```

###### maven-failsafe-plugin 설정
- maven-failsafe-plugin은 통합 테스트를 실행하기 위한 플러그인이다.
- 설정파일의 URI를 통해 설정하지 않는 서비스는 VCAP_SERVICES 환경변수를 제공해야 한다. failsafe 플러그인에서 VCAP_SERVICES 환경변수를 설정할 수 있다. 실제 테스트 수행시 서비스 환경에 맞게 VCAP_SERVICES의 값을 변경해야 한다.
- Spring.cloud.propertiesFile 시스템 프로퍼티를 등록하여 설정파일의 위치를 지정해야 한다.
- Includes 설정으로 통합테스트를 실행할 클래스를 지정한다.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-failsafe-plugin</artifactId>
    <configuration>
        <argLine>
            -Dfile.encoding=UTF-8
        </argLine>
        <includes>
            <include>**/integration/*IntegrationTest.java</include>
        </includes>
        <systemPropertyVariables>
            <spring.cloud.propertiesFile>
                C:\\Users…\\...\\spring-cloud.properties
            </spring.cloud.propertiesFile>
        </systemPropertyVariables>
        <environmentVariables>
            <VCAP_SERVICES>
                {"CubridDB": [{"credentials": {………. }
            </VCAP_SERVICES>
        </environmentVariables>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

###### 통합 테스트 실행
```
  mvn verify
```


