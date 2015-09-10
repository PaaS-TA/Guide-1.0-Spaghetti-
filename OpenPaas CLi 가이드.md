## Table of Contents
1. [개요](#개요)
     * [문서 목적](#문서 목적)
     * [범위](#범위)
     * [참고 자료](#참고 자료)

## 개요
---

#### 문서 목적

본 문서는 OpenPaas에 대한 설치 및 운영 관리를 위한 도구인 OpenPaas CLI에 대해 기본 사용법 및 실 사용 예시를 통해 OpenPaas를 이해하는데 목적이 있습니다. OpenPaas CLI명령어 사용법에 대해서 기술하였다습니다.

#### 범위

본 문서에서는 OpenPaaS CLI 분류 및 기본 사용법에 대해서 작성하였습니다.

#### 참고 자료

 본 문서는 Cloud Foundry의 CF Document를 참고로 작성하였습니다.

 [***https://docs.cloudfoundry.org/devguide/installcf/***](https://docs.cloudfoundry.org/devguide/installcf/)

## OpenPaaS CLI기본 사용법

CLI는 OpenPaas배포와 Release를 관리하기 위해 도움을 주는 커맨드 라인 유틸리티로 아래와 같이 2가지 형태로 구분된다.

 OpenPaaS CLI : OpenPaas를 관리하기 위한 CLI 도구

 - **기본 Syntax**


 ```
    cf [global options] command <arguments...> [command options]
 ```


cf 명령어에 대괄호로 묶인 인자인 <arguments>과 [command options]는 명령어에 따라 선택적으로 사용되고, command 인자는 필수 인자이다. CF 운영 및 관리하기 위한 도구인 CF CLI 아래와 같이 많은 명령어들을 제공하고 있다.


## GETTING STARTED

#### login


- **기본 Syntax**


```
    $ cf login [-a API_URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE]
```


- **설명**


```
  OpenPaas에게 로그인 하기 위한 명령어
```


- **파라미터**

| 파라미터명   |           설명                 | 필수(O/X) |
|-------------|-----------------------------|-----------|
|-a API_URL    |CLI가 접속 하려는 OpenPaas  URL<br>Ex) https://api.10.244.0.34.xip.io    |X        |
|-u USERNAMEL  |OpenPaas에 접속하는 사용자 id               |X        |
|-p PASSWORD   |OpenPaas에 접속하는 사용자 password         |X        |
|-o ORG        |OpenPaas에 접속하는 사용자의 소속조직 명      |X        |
|-s SPACE      |OpenPaas에 접속하는 사용자의 소속조직 스페이스직 명      |X        |


- **사용예시**


```
# 파라미터 지정한 경우
$ cf login --skip-ssl-validation -a https://api.10.244.0.34.xip.io -u admin -p admin -o crossent -s development

# 파라미터 지정하지 않을 경우
$ cf login
API endpoint: https://api.10.244.0.34.xip.io

Email> admin

Password>
Authenticating...
OK

Targeted org crossent

Select a space (or press enter to skip):
1. development
2. staged
3. oper

Space> 1
Targeted space development

API endpoint:   https://api.10.244.0.34.xip.io (API version: 2.29.0)   
User:           admin   
Org:            crossent   
Space:          development

```

#### logout
- **기본 Syntax**


```
    $ cf logout
```


- **설명**


```
  cf에서 logout한다.
```


- **파라미터**

  -없음


- **사용예시**

```
$ cf logout

```

#### passwd
- **기본 Syntax**


```
    $ cf passwd
```



- **설명**


```
  OpenPaas 사용자계정의 패스워드를 변경한다.
```


- **파라미터**

  -없음


- **사용예시**


```
$ cf passwd
Current Password>

New Password>

Verify Password>
Changing password...
OK
Please log in again  
```

#### target
- **기본 Syntax**


```
    $ cf target [-o ORG] [-s SPACE]
```

- **설명**


```
  로그인한 사용자가 사용할 Target 조직 및 스페이스 설정
```


- **파라미터**

| 파라미터명   |           설명                 | 필수(O/X) |
|-------------|-----------------------------|-----------|
|-o ORG      |Target 조직                    |X        |
|-s SPACE    |Target 스페이스                |X        |



- **사용예시**

```
# 파라미터 지정한 경우
$ cf target -o cf -s development
API endpoint:   https://api.10.244.0.34.xip.io (API version: 2.29.0)   
User:           admin   
Org:            cf   
Space:          development
# 파라미터 지정하지 않은 경우(현재 Target된 정보가 출력된다.
$ cf target
API endpoint:   https://api.10.244.0.34.xip.io (API version: 2.29.0)   
User:           admin   
Org:            cf
Space:          development  
```


#### api
- **기본 Syntax**

```
    $ cf api <URL>
```


- **설명**


```
  Target api를 조회하거나 target api URL을 설정한다.
```



- **파라미터**

| 파라미터명   |           설명                 | 필수(O/X) |
|-------------|-----------------------------|-----------|
|URL         |Api Target URL                   |O        |


- **사용예시**

```
$ cf api --skip-ssl-validation api.10.244.0.34.xip.io
```

#### auth
- **기본 Syntax**


```
    $ cf auth <USERNAME> <PASSWORD>
```


- **설명**


```
  OpenPaas login시 로그인만 되며 스페이스, 타겟은 지정되지 않는다.
```


- **파라미터**

| 파라미터명   |           설명                 | 필수(O/X) |
|-------------|--------------------------------|-----------|
|USERNAME     |로그인 사용자 ID                 |O        |
|PASSWORD    |로그인 사용자 PASSWORD            |O        |



- **사용예시**

```
$ cf api --skip-ssl-validation api.10.244.0.34.xip.io
```

## APPS


#### apps
- **기본 Syntax**


```
    $cf apps
```


- **설명**


```
  타겟 스페이스에 App 목록을 조회한다.
```


- **파라미터**

  -없음

  - **사용예시**

  ```
  $ cf apps
  ```

#### app

  - **기본 Syntax**


  ```
      $cf app <APP_NAME>
  ```


  - **설명**


  ```
    App의 상태를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                          |O        |


    - **사용예시**

    ```
    $ cf app spring-music
    ```

#### push,p

  - **기본 Syntax**


  ```
      $ cf push <APP_NAME> [-b BUILDPACK_NAME] [-c COMMAND] [-d DOMAIN] [-f MANIFEST_PATH] [-i NUM_INSTANCES] [-k DISK] [-m MEMORY] [-n HOST] [-p PATH] [-s STACK] [-t TIMEOUT] [--no-hostname] [--no-manifest] [--no-route] [--no-start]
  ```


  - **설명**


  ```
    App을 OpenPaas에 배포 하고 app을 Start한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |push하는 App명(지정하지 않으면 디렉토리명)                         |O        |
  |-b BUILDPACK |custom빌드팩 URL <br> ex) https://github.com/heroku/heroku-buildpack-play    |X        |
  |-c COMMAND   |App start command              |X        |
  |-d DOMAIN    |App 도메인                      |X        |
  |-f MANIFEST_PATH    |Manifest 파일 경로       |X        |
  |-i NUM_INSTANCES     |App 인스턴스 갯수        |X        |
  |-m MEMORY     |인스턴스 메모리 용량             |X        |
  |-k DISK     |디스크 사용 용량                  |X        |
  |-n HOST     |호스트명 <br> ex) my-subdomain)  |X        |
  |-p PATH     |App의 디렉토리 경로 또는 App file(zip,war등)경로   |X        |
  |-s STACK    |App이 실행되는 운영체제 파일시스템(default: cflinuxfs2)       |X        |
  |-t TIMEOUT  |App이 실행되는동안 CLI가 대기하는 timeout시간                          |X        |
  |--no-hostname     |App에 root 도메인을 매핑                          |X        |
  |--no-manifest     |Manifest 파일을 무시한다.                         |X        |
  |--no-route     |Push된 앱에 라우트 정보를 삭제하고 App에 라우트 정보를 매핑하지 않는다    |X        |
  |--no-start     |App을 push하고 Start하지 않는다.                        |X        |
  |--random-route    |App에게 라우트 정보를 랜덤하게 생성한다                |X        |

  - **사용예시**

  ```
  $ cf push spring-music
  ```

#### scale

  - **기본 Syntax**


  ```
      $ cf scale <APP_NAME> [-i INSTANCES] [-k DISK] [-m MEMORY] [-f]
  ```


  - **설명**


  ```
    App의 메모리,디스크 크기 및 인스턴스 갯수를 조정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |-i INSTANCES |인스턴스 갯수                    |X        |
  |-k DISK      |디스크 용량                      |X        |
  |-m MEMORY    |메모리 용량                      |X        |
  |-f           |App 강제 restart                |X        |


  - **사용예시**

  ```
  $ cf scale spring-music -i 2 -m 512m
  ```


#### delete

  - **기본 Syntax**


  ```
      $ cf delete <APP_NAME> [--f] [--r]
  ```


  - **설명**


  ```
    App을 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |--f          |확인 없이 App 삭제               |X        |
  |--r          |App에 매핑된 라우트 정보 삭제     |X        |


  - **사용예시**

  ```
  $  cf delete spring-music
  ```


#### rename

  - **기본 Syntax**


  ```
      $ cf rename <APP_NAME> <NEW_APP_NAME>
  ```


  - **설명**


  ```
    App명을 변경한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |NEW_APP_NAME |변경하려는 App명                 |O        |


  - **사용예시**

  ```
  $  cf rename spring-music new-spring-music
  ```


#### start,st

  - **기본 Syntax**


  ```
      $ cf start <APP_NAME>
  ```


  - **설명**


  ```
    App을 Start한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |


  - **사용예시**

  ```
  $  cf start spring-music
  ```

#### stop,sp

  - **기본 Syntax**


  ```
      $ cf stop <APP_NAME>
  ```


  - **설명**


  ```
    App을 stop한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |


  - **사용예시**

  ```
  $  cf stop spring-music
  ```


#### restart

  - **기본 Syntax**


  ```
      $ cf restart <APP_NAME>
  ```


  - **설명**


  ```
    App을 restart 한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |


  - **사용예시**

  ```
  $cf restart spring-music
  ```

#### restage

  - **기본 Syntax**


  ```
      $ cf restage <APP_NAME>
  ```


  - **설명**


  ```
    App을 restage한다.(환경변수 설정 또는 서비스 바인딩시 사용)
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |


  - **사용예시**

  ```
  $cf restage spring-music
  ```

#### restart-app-instance

  - **기본 Syntax**


  ```
      $ cf restart-app-instance <APP_NAME> <INDEX>
  ```


  - **설명**


  ```
    App의 인스턴스중 특정 인스턴스를 restart한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |INDEX        |인스턴스 인덱스                   |O        |

  - **사용예시**

  ```
  $cf restart-app-instance spring-music 1
  ```

#### events

  - **기본 Syntax**


  ```
      $ cf events <APP_NAME>
  ```


  - **설명**


  ```
    App에서 발생한 최근 Event정보를 조회한다. (start/stop/scale등의 이력)
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |

  - **사용예시**

  ```
  $ cf events spring-music
  ```


#### files

  - **기본 Syntax**


  ```
      $ cf files <APP_NAME> [PATH] [-i INSTANCE]
  ```


  - **설명**


  ```
    App의 file및 디렉토리 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |PATH         |APP의 디렉토리                   |X        |
  |-i INSTANCE  |App인스턴스 인덱스               |X        |

  - **사용예시**

  ```
  $  cf files spring-music
  ```

#### logs

  - **기본 Syntax**


  ```
      $ cf logs <APP_NAME> [--recent]
  ```


  - **설명**


  ```
    App에서 발생한 로그를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |PATH         |APP의 디렉토리                   |X        |
  |-i INSTANCE  |App인스턴스 인덱스               |X        |

  - **사용예시**

  ```
  $  cf logs spring-music
  ```

#### env,e

  - **기본 Syntax**


  ```
      $ cf env  <APP_NAME>
  ```


  - **설명**


  ```
    App의 환경변수를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |

  - **사용예시**

  ```
  $ cf env spring-music
  ```

#### unset-env

  - **기본 Syntax**


  ```
      $ cf unset-env <APP_NAME> <ENV_VAR_NAME>
  ```


  - **설명**


  ```
    App에 설정된 환경변수를 삭제한다.(적용시 restage필요)
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |ENV_VAR_NAME |App의 환경변수 Key               |O        |


  - **사용예시**

  ```
  $ cf unset-env spring-music author
  ```

#### stacks

  - **기본 Syntax**


  ```
      $ cf stacks
  ```


  - **설명**


  ```
    OpenPaaS의 stack목록(운영체제 파일시스템) 목록을 조회한다.
  ```


  - **파라미터**


    - 없음


  - **사용예시**

  ```
  $  cf stacks
  ```

#### stack

  - **기본 Syntax**


  ```
      $ cf stack <STACK_NAME> [--guid]
  ```


  - **설명**


  ```
    OpenPaaS의 stack목록(운영체제 파일시스템) 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                           |O        |
  |--guid       |Stack guid를 조회한다            |X        |


  - **사용예시**

  ```
  $  cf stack cflinuxfs2
  ```

#### copy-source

  - **기본 Syntax**


  ```
      $ cf copy-source <SOURCE-APP> <TARGET-APP> [-o TARGET-ORG] [-s TARGET-SPACE] [--no-restart]
  ```


  - **설명**


  ```
    App의 소스를 다른 App에 복사한다. 파일이 덥어 쓰이지 않으면 자동 restart한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |원본 APP명                        |O        |
  |TARGET-APP   |소스가 복사될 대상 App명            |X        |
  |-o TARGET-ORG |타겟 조직                         |O        |
  |-s TARGET-SPACE|타겟 스페이스                    |X        |
  |--no-restart   |소스 복사 후 restart하지 않는다.  |X        |

  - **사용예시**

  ```
  $ cf copy-source spring-music another-music
  ```

#### create-app-manifest

  - **기본 Syntax**


  ```
      $ cf create-app-manifest <APP_NAME> [-p /path/<app-name>-manifest.yml]
  ```


  - **설명**


  ```
    App의 manifest파일을 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |원본 APP명                        |O        |
  |-p /path/<app-name>.yml   |파일이 생성될 위치와 파일명(-p 를 사용하지 않으면 자동생성된다)            |X        |

  - **사용예시**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```


## SERVICES


#### marketplace,mt

  - **기본 Syntax**


  ```
      $ cf marketplace [-s SERVICE_NAME]
  ```


  - **설명**


  ```
    cf 마켓플레이스에서 제공하는 서비스 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |서비스의 plan이 조회된다.    |X        |


  - **사용예시**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```

#### services,s

  - **기본 Syntax**


  ```
      $ cf services
  ```


  - **설명**


  ```
    타겟 스페이스에 서비스 인스턴스 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |서비스의 plan이 조회된다.    |X        |


  - **사용예시**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```

#### service

  - **기본 Syntax**


  ```
      $ cf service <SERVICE_INSTANCE> [--guid]
  ```


  - **설명**


  ```
    서비스 인스턴스의 정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE   |서비스 인스턴스명           |O        |
  |--guid             |서비스 인스턴스의 Guid를 조회한다.   |X        |

  - **사용예시**

  ```
  $ cf service spring-music-db
  ```


#### create-service

  - **기본 Syntax**


  ```
      $ cf create-service <SERVICE> <PLAN> <SERVICE_INSTANCE> [-c PARAMETERS_AS_JSON] [-t TAGS]
  ```


  - **설명**


  ```
    마켓플레이스에서 제공하는 서비스로 서비스 인스턴스를 만든다.
  ```

  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |마켓플레이스에 있는 서비스명                                              |O        |
  |PLAN         |서비스 플랜명                                                           |O        |
  |SERVICE_INSTANCE   |만들 서비스 인스턴스명                                             |O        |
  |-c PARAMETERS_AS_JSON |서비스 설정정보를 json 형태로 입력 <br> Ex) -c '{"ram_gb":4}'    |X        |
  |-t TAGS      |서비스 인스턴스 테그                                                     |X        |

  - **사용예시**

  ```
  $ cf create-service spring-music-db silver p-mysql
  ```


#### update-service

  - **기본 Syntax**


  ```
      $ cf update-service <SERVICE_INSTANCE> [-p NEW_PLAN] [-c PARAMETERS_AS_JSON] [-t TAGS]
  ```


  - **설명**


  ```
    서비스 인스턴스를 수정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE        |서비스 인스턴스명                                               |O        |
  |-p NEW_PLAN             |서비스 플랜명                                                  |O        |
  |-c PARAMETERS_AS_JSON   |서비스 설정정보를 json 형태로 입력 <br> Ex) -c '{"ram_gb":4}'    |O        |
  |-t TAGS                 |서비스 인스턴스 테그                                            |X        |

  - **사용예시**

  ```
  $ cf update-service spring-music-db -p gold_plan
  ```


#### delete-service

  - **기본 Syntax**


  ```
      $ cf delete-service SERVICE_INSTANCE [-f]
  ```


  - **설명**


  ```
    서비스 인스턴스를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |서비스 인스턴스명                                        |O        |
  |-f                |삭제 확인 메시지 없이 서비스 인스턴스 삭제한다.             |X        |

  - **사용예시**

  ```
  $ cf delete-service spring-music-db
  ```

#### rename-service

  - **기본 Syntax**


  ```
      $ cf rename-service <SERVICE_INSTANCE> <NEW_SERVICE_INSTANCE>
  ```


  - **설명**


  ```
    서비스 인스턴스명을 수정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |서비스 인스턴스명                       |O        |
  |NEW_SERVICE_INSTANCE   |변경하려는 서비스 인스턴스명             |O        |

  - **사용예시**

  ```
  $ cf rename-service spring-music-db new_spring-music-db
  ```


#### create-service-key,csk

  - **기본 Syntax**


  ```
      $ cf create-service-key <SERVICE_INSTANCE> <SERVICE_KEY> [-c PARAMETERS_AS_JSON]
  ```


  - **설명**


  ```
    서비스 인스턴스의 key를 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |서비스 인스턴스명                       |O        |
  |SERVICE_KEY            |서비스 인스턴스 key명                   |O        |
  |-c PARAMETERS_AS_JSON  |서비스 인스턴스 설정(JSON Parameter)    |X        |


  - **사용예시**

  ```
  $ cf create-service-key spring-music-db mykey -c '{"permissions":"read-only"}'
  ```

#### service-keys,sk

  - **기본 Syntax**


  ```
      $ cf service-keys <SERVICE_INSTANCE>
  ```


  - **설명**


  ```
    서비스 인스턴스의 key 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |서비스 인스턴스명                       |O        |


  - **사용예시**

  ```
  $ cf service-keys spring-music-db
  ```

#### service-key

  - **기본 Syntax**


  ```
      $ cf service-key <SERVICE_INSTANCE> <SERVICE_KEY> [--guid]
  ```


  - **설명**


  ```
    서비스 인스턴스의 key의 상세정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |서비스 인스턴스명                       |O        |
  |SERVICE_KEY       |서비스 인스턴스 key명                   |O        |
  |--guid            |서비스 인스턴스 guid를 조회한다.         |X        |



  - **사용예시**

  ```
  $ cf service-key spring-music-db mykey
  ```


#### delete-service-key,dsk

  - **기본 Syntax**


  ```
      $ cf delete-service-key <SERVICE_INSTANCE> <SERVICE_KEY> [-f]
  ```


  - **설명**


  ```
    서비스 key를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |서비스 인스턴스명                       |O        |
  |SERVICE_KEY       |서비스 인스턴스 key명                   |O        |
  |--guid            |서비스 인스턴스 guid를 조회한다.         |X        |



  - **사용예시**

  ```
  $ cf delete-service-key spring-music-db mykey
  ```


#### bind-service,bs

  - **기본 Syntax**


  ```
      $ cf bind-service <APP_NAME> <SERVICE_INSTANCE> [-c PARAMETERS_AS_JSON]
  ```


  - **설명**


  ```
    App과 서비스 인스턴스를 바인딩한다.<br> - 서비스 인스턴스와 APP을 바인딩해야 App에서 서비스 사용가능
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP명                                            |O        |
  |SERVICE_INSTANCE  |서비스 인스턴스 명                           |O        |
  |-c PARAMETERS_AS_JSON   |바인딩 설정 파라미터 (json형태)         |X        |



  - **사용예시**

  ```
  $ cf bind-service spring-music spring-music-db -c '{"permissions":"read-only"}'

  $ cf bind-service spring-music spring-music-db -c ~/workspace/tmp/instance_config.json
  ```


#### unbind-service,us

  - **기본 Syntax**


  ```
      $ cf unbind-service <APP_NAME> <SERVICE_INSTANCE>
  ```


  - **설명**


  ```
    App과 서비스 인스턴스를 언바인딩한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME          |서비스 인스턴스명                            |O        |
  |SERVICE_INSTANCE  |서비스 인스턴스 명                           |O        |



  - **사용예시**

  ```
  $ cf unbind-service spring-music spring-music-db
  ```


#### create-user-provided-service, cups

  - **기본 Syntax**


  ```
      $ cf create-user-provided-service <SERVICE_INSTANCE> [-p CREDENTIALS] [-l SYSLOG-DRAIN-URL]
  ```


  - **설명**


  ```
    Market place에서 제공하는 서비스를 사용하지 않고 사용자가 별도의 서비스를 구성하여 APP과 바인딩한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE          |서비스 인스턴스명                            |O        |
  |-p CREDENTIALS            |서비스 인스턴스 명                           |X        |
  |-l SYSLOG-DRAIN-URL       |서비스 인스턴스 명                           |X        |


  - **사용예시**

  ```
  $ cf create-user-provided-service spring-music-db -p '{"username":"admin","password":"pa55woRD"}'
  ```



#### create-org,co

  - **기본 Syntax**


  ```
      $ cf create-org <ORG_NAME> [-q QUOTA_NAME]
  ```


  - **설명**


  ```
    조직정보를 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |조직명                         |O        |
  |-q QUOTA_NAME |조직에게 할당할 quota           |X        |


  - **사용예시**

  ```
  $cf create-org test -q default
  ```



#### delete-org

  - **기본 Syntax**


  ```
      $ cf delete-org <ORG_NAME> [-f]
  ```


  - **설명**


  ```
    조직정보 목록을 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |조직명                          |O        |
  |-f           |확인메시지 없이 조직정보 삭제한다.  |X        |


  - **사용예시**

  ```
  $ cf delete-org cf -f
  ```


#### rename-org

  - **기본 Syntax**


  ```
      $ cf rename-org <ORG_NAME> <NEW_ORG_NAME>
  ```


  - **설명**


  ```
    조직명을 변경한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME       |조직명                          |O        |
  |NEW_ORG_NAME   |변경할 조직명                    |O        |


  - **사용예시**

  ```
  $ cf rename cf new-cf
  ```


## SPACES

#### spaces

  - **기본 Syntax**


  ```
      $ cf spaces
  ```


  - **설명**


  ```
    스페이스 목록을 가져온다.
  ```


  - **파라미터**

   - 없음


  - **사용예시**

  ```
  $ cf spaces
  ```


#### space

  - **기본 Syntax**


  ```
      $ cf space <SPACE_NAME>
  ```


  - **설명**


  ```
    스페이스 상세정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |스페이스명                           |O          |


  - **사용예시**

  ```
  $ cf space development
  ```

#### create-space

  - **기본 Syntax**


  ```
      $ cf create-space <SPACE_NAME> [-o ORG_NAME] [-q SPACE-QUOTA-NAME]
  ```


  - **설명**


  ```
    스페이스 정보를 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |스페이스명                           |O         |
  |-o ORG_NAME  |스페이스에 매핑될 조직명               |X         |
  |-q SPACE-QUOTA-NAME    |스페이스에 할당될 QUOTA명    |X         |

  - **사용예시**

  ```
  $ cf create-space -o cf -q cf-space-quota
  ```

#### delete-space

  - **기본 Syntax**


  ```
      $ cf delete-space <SPACE_NAME> [-f]
  ```


  - **설명**


  ```
    스페이스정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |스페이스명                           |O         |
  |-f           |삭제 확인메시지 없이 스페이스 삭제한다. |X         |

  - **사용예시**

  ```
  $ cf delete-space development
  ```

#### rename-space

  - **기본 Syntax**


  ```
      $ cf rename-space <SPACE_NAME> <NEW_SPACE_NAME>
  ```


  - **설명**


  ```
    스페이스 명을 변경한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME     |스페이스명                           |O         |
  |NEW_SPACE_NAME |삭제 확인메시지 없이 스페이스 삭제한다. |O         |

  - **사용예시**

  ```
  $ cf rename-space development new_development
  ```

## DOMAINS

#### domains

  - **기본 Syntax**


  ```
      $ cf domains
  ```


  - **설명**


  ```
    도메인 정보 목록을 조회한다.
  ```


  - **파라미터**


    - 없음


  - **사용예시**

  ```
  $ cf domains
  ```


#### create-domain

  - **기본 Syntax**


  ```
      $ cf create-domain <ORG_NAME> <DOMAIN>
  ```


  - **설명**


  ```
    도메인 정보 목록을 생성한다. 생성된 도메인은 설정된 조직에서 사용가능하다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME     |조직명                           |O         |
  |DOMAIN       |도메인명                          |O         |


  - **사용예시**

  ```
  $ cf create-domain cf-org cf.or.kr
  ```


#### delete-domains

  - **기본 Syntax**


  ```
      $ cf delete-domain <DOMAIN> [-f]
  ```


  - **설명**


  ```
    도메인 정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |도메인명                           |O         |
  |-f           |삭제 확인메시지 없이 도메인을 삭제한다. |X         |


  - **사용예시**

  ```
  $ cf delete-domain cf.or.kr
  ```


#### create-shared-domains

  - **기본 Syntax**


  ```
      $ cf create-shared-domain <DOMAIN>
  ```


  - **설명**


  ```
    공유 도메인정보를 생성한다
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |조직명                           |O         |


  - **사용예시**

  ```
  $ cf create-shared-domain cf.or.kr
  ```

#### delete-shared-domains

  - **기본 Syntax**


  ```
      $ cf delete-domain <DOMAIN> [-f]
  ```


  - **설명**


  ```
    공유 도메인정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |조직명                           |O         |
  |-f           |삭제 확인메시지 없이 도메인을 삭제한다.    |X         |

  - **사용예시**

  ```
  $ cf delete-shared-domain cf.or.kr
  ```


## SPACES


#### routes,r

  - **기본 Syntax**


  ```
      $ cf routes
  ```


  - **설명**


  ```
    현재 조직/스페이스에 존재하는 라우트 정보목롤을 조회한다.
  ```


  - **파라미터**

    - 없음

  - **사용예시**

  ```
  $ cf delete-shared-domain cf.or.kr
  ```


#### create-route

  - **기본 Syntax**


  ```
      $ cf create-route <SPACE_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **설명**


  ```
    공유 도메인정보를 삭제한다...
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |스페이스명                           |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제한다. <br>   - 도메인 정보가 입력되어있어야 한다.   |O         |
  |-n HOSTNAME  |호스트 명                          |X         |


  - **사용예시**

  ```
  $ cf delete-shared-domain cf.or.kr
  ```


#### update-route

  - **기본 Syntax**


  ```
      $ cf update-route <SPACE_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **설명**


  ```
    공유 도메인정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |스페이스명                           |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제한다. <br>   - 도메인 정보가 입력되어있어야 한다.   |O         |
  |-n HOSTNAME  |호스트 명                          |X         |


  - **사용예시**

  ```
  $ cf update-route development cf.or.kr
  ```



#### check-route

  - **기본 Syntax**


  ```
      $ cf check-route <HOST> <DOMAIN>
  ```


  - **설명**


  ```
    라우트 정보가 존재하는지 체크한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |HOST         |호스트 명                                    |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제한다.    |O         |


  - **사용예시**

  ```
  $ cf check-route spring-music cf.or.kr
  ```


#### map-route

  - **기본 Syntax**


  ```
      $ cf map-route <APP_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **설명**


  ```
    App에게 URL route정보를 할당한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App명                           |O         |
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |


  - **사용예시**

  ```
  $ cf map-route spring-music cf.or.kr -n test
  ```


#### unmap-route

  - **기본 Syntax**


  ```
      $ cf map-route <APP_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **설명**


  ```
    App에게 URL route정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App명                           |O         |
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |


  - **사용예시**

  ```
  $ cf unmap-route spring-music cf.or.kr -n spring-music
  ```

#### delete-route

  - **기본 Syntax**


  ```
      $ cf delete-route <DOMAIN> [-n HOSTNAME] [-f]
  ```


  - **설명**


  ```
    App에게 URL route정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App명                           |O         |
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |


  - **사용예시**

  ```
  $ cf delete-route spring-music cf.or.kr -n spring-music
  ```

#### delete-orphaned-routes

  - **기본 Syntax**


  ```
      $ cf delete-orphaned-routes [-f]
  ```


  - **설명**


  ```
    App에 매핑되지 않은 라우트 정보를 모두 삭제한다
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |-f           |삭제 확인메시지 없이 라우트 정보를 삭제한다.           |X         |


  - **사용예시**

  ```
  $ cf delete-orphaned-routes
  ```


## BUILDPACKS


#### buildpacks

  - **기본 Syntax**


  ```
      $ cf buildpacks
  ```


  - **설명**


  ```
    빌드팩 목록을 조회한다.
  ```


  - **파라미터**


    - 없음


  - **사용예시**

  ```
  $ cf buildpacks
  ```


#### create-buildpack

  - **기본 Syntax**


  ```
      $ cf create-buildpack <BUILDPACK> <-p PATH> <-i POSITION> [--enable|--disable]
  ```


  - **설명**


  ```
    빌드팩을 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩명                           |O         |
  |-p PATH       |빌드팩 경로                      |O         |
  |-i POSITIONE  |빌드팩 auto-detection동안 빌드팩 체크 순서  <br> ex)1.2.3              |O         |
  |--enable      |스테이징시 사용                  |X         |
  |--disable     |스테이징시 미사용                |X         |



  - **사용예시**

  ```
  $ cf create-buildpack egov-buildpack ~/workspace/buildpack/egov -i 1
  ```


#### update-buildpack

  - **기본 Syntax**


  ```
      $ cf update-buildpack <BUILDPACK> [-p PATH] [-i POSITION] [--enable|--disable] [--lock|--unlock]
  ```


  - **설명**


  ```
    빌드팩 정보를 수정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩명                           |O         |
  |-p PATH       |빌드팩 경로                      |O         |
  |-i POSITIONE  |빌드팩 auto-detection동안 빌드팩 체크 순서  <br> ex)1.2.3              |O         |
  |--enable      |스테이징시 사용                  |X         |
  |--disable     |스테이징시 미사용                |X         |



  - **사용예시**

  ```
  $ cf create-buildpack egov-buildpack ~/workspace/buildpack/egov -i 1
  ```

#### delete-buildpack

  - **기본 Syntax**


  ```
      $ cf delete-buildpack <BUILDPACK_NAME> [-f]
  ```


  - **설명**


  ```
    빌드팩을 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩명                           |O         |
  |-f           |삭제 확인메시지 없이 빌드팩 정보를 삭제한다.       |X         |


  - **사용예시**

  ```
  $ cf delete-buildpack egov-buildpack
  ```

## USER ADMIN


#### create-user

  - **기본 Syntax**


  ```
      $ cf create-user <USERNAME> <PASSWORD>
  ```


  - **설명**


  ```
    새로운 사용자 계정을 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자 ID                        |O         |
  |PASSWORD     |패스워드                         |O         |


  - **사용예시**

  ```
  $ cf create-user cfuser userpassword
  ```


#### delete-user

  - **기본 Syntax**


  ```
      $ cf delete-user <USERNAME> [-f]
  ```


  - **설명**


  ```
    새로운 사용자 계정을 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자 ID                       |O         |
  |-f           |삭제 확인메시지 없이 사용자 정보를 삭제한다.                         |X         |


  - **사용예시**

  ```
  $ cf delete-user cfuser
  ```


#### org-users

  - **기본 Syntax**


  ```
      $ cf org-users cforg
  ```


  - **설명**


  ```
    조직에 소속된 사용자를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME     |조직명                          |O         |


  - **사용예시**

  ```
  $ cf org-users cforg
  ```

#### set-org-role

  - **기본 Syntax**


  ```
      $ cf set-org-role <USERNAME> <ORG> <ROLE>
  ```


  - **설명**


  ```
    사용자에게 특정조직의 role을 설정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자명                          |O         |
  |ORG          |조직명                          |O         |
  |ROLE        |역할명 <br>  - OrgManager : 사용자 관리 및 plan설정/변경 권한 <br> - BillingManager : 빌링계정 및 과금정보 생성 및 관리 <br>  - OrgAuditor : 조직 quota사용률 및 사용자 role을 조회한다.             |O         |



  - **사용예시**

  ```
  $ cf set-org-role cfuser cforg OrgManager
  ```


#### unset-org-role

  - **기본 Syntax**


  ```
      $ cf unset-org-role <USERNAME> <ORG> <ROLE>
  ```


  - **설명**


  ```
    사용자에게 특정조직의 role을 설정을 해제한다..
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자명                          |O         |
  |ORG          |조직명                          |O         |
  |ROLE        |역할명 <br>  - OrgManager : 사용자 관리 및 plan설정/변경 권한 <br> - BillingManager : 빌링계정 및 과금정보 생성 및 관리 <br>  - OrgAuditor : 조직 quota사용률 및 사용자 role을 조회한다.             |O         |



  - **사용예시**

  ```
  $ cf unset-org-role cfuser cforg OrgManager
  ```


#### space-users

  - **기본 Syntax**


  ```
      $ cf space-users <ORG> <SPACE>
  ```


  - **설명**


  ```
    조직의 스페이스에 할당된 사용자 목록정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG          |조직명                          |O         |
  |SPACE        |스페이스명                       |O         |


  - **사용예시**

  ```
  $ cf space-users development
  ```


#### set-space-role

  - **기본 Syntax**


  ```
      $ cf set-space-role <USERNAME> <ORG> <SPACE> <ROLE>
  ```


  - **설명**


  ```
    사용자에게 조직의 스페이스에 role을 할당한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자명                         |O         |
  |ORG          |조직명                           |O         |
  |SPACE        |스페이스명                       |O         |
  |ROLE         |역할명  <br>  - SpaceManager: 스페이스의 관리자로 스페이스 내의 사용자 계정 관리 및 인스턴스 수, 서비스 바인딩 상태 및 스페이스 내의 리소스 상태를 조회 할 수 있다. <br> - SpaceDeveloper: 스페이스 내의 App 및 서비스 관리  <br> - SpaceAuditor: 스페이스 내의 서비스 바인딩, 인스턴스 수, app사용률등을 조회   |O         |

  - **사용예시**

  ```
  $ cf set-space-role cfuser cforg development OrgManager
  ```


#### unset-space-role

  - **기본 Syntax**


  ```
      $ cf unset-space-role <USERNAME> <ORG> <SPACE> <ROLE>
  ```


  - **설명**


  ```
    사용자에게 조직의 스페이스에 role을 회수한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자명                         |O         |
  |ORG          |조직명                           |O         |
  |SPACE        |스페이스명                       |O         |
  |ROLE         |역할명  <br>  - SpaceManager: 스페이스의 관리자로 스페이스 내의 사용자 계정 관리 및 인스턴스 수, 서비스 바인딩 상태 및 스페이스 내의 리소스 상태를 조회 할 수 있다. <br> - SpaceDeveloper: 스페이스 내의 App 및 서비스 관리  <br> - SpaceAuditor: 스페이스 내의 서비스 바인딩, 인스턴스 수, app사용률등을 조회   |O         |

  - **사용예시**

  ```
  $ cf unset-space-role cfuser cforg development OrgManager
  ```


## ORG ADMIN


#### quotas

  - **기본 Syntax**


  ```
      $ cf quotas
  ```


  - **설명**


  ```
    Quota 목록을 조회한다.
  ```


  - **파라미터**

    - 없음

  - **사용예시**

  ```
  $ cf quotas
  ```


#### quota

  - **기본 Syntax**


  ```
      $ cf quota <QUOTA>
  ```


  - **설명**


  ```
    Quota의 상세정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |QUOTA명                         |O         |



  - **사용예시**

  ```
  $ cf quota cf-quota
  ```

#### set-quota

  - **기본 Syntax**


  ```
      $ cf set-quota <ORG> <QUOTA>
  ```


  - **설명**


  ```
    조직에게 QUOTA를 할당한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG          |직명                            |O         |
  |QUOTA        |QUOTA명                         |O         |


  - **사용예시**

  ```
  $ cf set-quota cf-quota
  ```


#### create-quota

  - **기본 Syntax**


  ```
      $ cf create-quota <QUOTA> [-m TOTAL_MEMORY] [-i INSTANCE_MEMORY] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans]
  ```


  - **설명**


  ```
    Quota정보를 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA명                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 서비스 인스턴스 수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |


  - **사용예시**

  ```
  $ cf create-quota cf-quota -m 500m -i 256m -r 2000 -s 500
  ```


#### delete-quota

  - **기본 Syntax**


  ```
      $ cf delete-quota <QUOTA> [-f]
  ```


  - **설명**


  ```
    Quota정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA        |QUOTA명                                                       |O         |
  |-f           |삭제 확인메시지 없이 QUOTA 정보를 삭제한다.                       |X         |


  - **사용예시**

  ```
  $ cf delete-quota cf-quota
  ```


#### update-quota

  - **기본 Syntax**


  ```
      $ cf update-quota <QUOTA> [-m TOTAL_MEMORY] [-i INSTANCE_MEMORY][-n NEW_NAME] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans | --disallow-paid-service-plans]
  ```


  - **설명**


  ```
    Quota정보를 수정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA명                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-n NEW_NAME                 |QUOTA명 변경시 변경할 이름                                      |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 서비스 인스턴스 수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |
  |--disallow-paid-service-plans  |과금 서비스 plan 사용 불가                                       |X        |

  - **사용예시**

  ```
  $ cf update-quota cf-quota -m 500m -i 256m -r 2000 -s 500
  ```


#### shared-private-domain

  - **기본 Syntax**


  ```
      $ cf share-private-domain <ORG> <DOMAIN>
  ```


  - **설명**


  ```
    private도메인을 다른 조직과 공유한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                 |QUOTA명                        |O         |
  |DOMAIN                |도메인명                        |O         |


  - **사용예시**

  ```
  $ cf share-private-domain cf-org sharedomain.or.kr
  ```


#### unshared-private-domain

  - **기본 Syntax**


  ```
      $ cf unshare-private-domain <ORG> <DOMAIN>
  ```


  - **설명**


  ```
    다른 조직과 share한 도메인 정보를 unshare한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG                   |도메인명                        |O         |
  |DOMAIN                |도메인명                        |O         |


  - **사용예시**

  ```
  $ cf unshare-private-domain cf-org sharedomain.or.kr
  ```


## SPACE ADMIN


#### space-quotas

  - **기본 Syntax**


  ```
      $ cf space-quotas
  ```


  - **설명**


  ```
    Space-quota정보 목록을 조회한다.
  ```


  - **파라미터**


    - 없음


  - **사용예시**

  ```
  $ cf space-quotas
  ```



#### space-quota

  - **기본 Syntax**


  ```
      $ cf create-space-quota <QUOTA> [-i INSTANCE_MEMORY] [-m MEMORY] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans]
  ```


  - **설명**


  ```
    Space quota 상세정보를 조회한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_QUOTA_NAME       |스페이스 QUOTA명       |O         |


  - **사용예시**

  ```
  $ cf space-quota cf-space-quota
  ```


#### create-space-quota

  - **기본 Syntax**


  ```
      $ cf space-quota <SPACE_QUOTA_NAME>
  ```


  - **설명**


  ```
    스페이스 Quota정보를 생성한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA명                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 서비스 인스턴스 수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |


  - **사용예시**

  ```
  $ cf create-space-quota cf-space-quota -i 2G -m 10G -r 3000 -s 200
  ```


#### update-space-quota

  - **기본 Syntax**


  ```
      $ cf update-space-quota <SPACE-QUOTA-NAME> [-i MAX-INSTANCE-MEMORY] [-m MEMORY] [-n NEW_NAME] [-r ROUTES] [-s SERVICES] [--allow-paid-service-plans | --disallow-paid-service-plans]
  ```


  - **설명**


  ```
    스페이스 Quota정보를 수정한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME            |스페이스 QUOTA명                                               |O         |
  |-i MAX-INSTANCE-MEMORY      |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-m MEMORY                   |스페이스가 가질수 있는 최대 메모리                               |X         |
  |-n NEW_NAME                 |변경하려는 SPACE-QUOTA명                                       |X         |
  |-r ROUTES                   |스페이스가 가지는 최대 route 갯수                               |X         |
  |-s SERVICES                 |스페이스가 가지는 최대 서비스 인스턴스 갯수                       |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |
  |--disallow-paid-service-plans  |과금 서비스 plan 사용 불가                                   |X        |


  - **사용예시**

  ```
  $ cf create-space-quota cf-space-quota -i 2G -m 10G -r 3000 -s 200
  ```


#### delete-space-quota

  - **기본 Syntax**


  ```
      $ cf delete-space-quota <SPACE-QUOTA-NAME>
  ```


  - **설명**


  ```
    스페이스 Quota정보를 삭제한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME     |스페이스 QUOTA명                                     |O         |
  |-f           |삭제 확인메시지 없이 SPACE-QUOTA 정보를 삭제한다.               |X         |



  - **사용예시**

  ```
  $ cf delete-space-quota cf-space-quota
  ```


#### set-space-quota

  - **기본 Syntax**


  ```
      $ cf set-space-quota <SPACE-NAME> <SPACE-QUOTA-NAME>
  ```


  - **설명**


  ```
    스페이스에 quota를 할당한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-NAME            |스페이스명                    |O         |
  |SPACE-QUOTA-NAME      |스페이스 Quota명              |O         |



  - **사용예시**

  ```
  $ cf set-space-quota development cf-space-quota
  ```


#### unset-space-quota

  - **기본 Syntax**


  ```
      $ cf unset-space-quota SPACE QUOTA
  ```


  - **설명**


  ```
    스페이스에 할당된 quota를 회수한다.
  ```


  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE        |스페이스명                   |O         |
  |QUOTA        |스페이스 Quota명             |O         |



  - **사용예시**

  ```
  $ cf set-space-quota development cf-space-quota
  ```


## service admin


#### service-auth-tokens

  - **기본 Syntax**


  ```
      $ cf service-auth-tokens
  ```


  - **설명**


  ```
    서비스 인증 토큰 목록을 조회한다.
  ```


  - **파라미터**

    - 없음



  - **사용예시**

  ```
  $ cf service-auth-token
  ```


#### create-service-auth-token

  - **기본 Syntax**


  ```
      $ cf create-service-auth-token <LABEL> <PROVIDER> <TOKEN>
  ```


  - **설명**


  ```
    스페이스에 할당된 quota를 회수한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |서비스 제공자                    |O         |
  |TOKEN        |토큰명                          |O         |


  - **사용예시**

  ```
  $ cf create-service-auth-token token-label mysql token
  ```


#### update-service-auth-token

  - **기본 Syntax**


  ```
      $ cf update-service-auth-token <LABEL> <PROVIDER> <TOKEN>
  ```


  - **설명**


  ```
    Service auth token 정보를 수정한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |서비스 제공자                    |O         |
  |TOKEN        |토큰명                          |O         |


  - **사용예시**

  ```
  $ cf update-service-auth-token token-label mysql token
  ```


#### delete-service-auth-token

  - **기본 Syntax**


  ```
      $ cf delete-service-auth-token <LABEL> <PROVIDER> [-f]
  ```


  - **설명**


  ```
    Service auth token 정보를 삭제한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |서비스 제공자                    |O         |
  |-f           |삭제 확인메시지 없이 SERVICE TOKEN 정보를 삭제한다.      |X         |


  - **사용예시**

  ```
  $ cf delete-service-auth-token token-label mysql
  ```


#### service-brokers

  - **기본 Syntax**


  ```
      $ cf delete-service-auth-token <LABEL> <PROVIDER> [-f]
  ```


  - **설명**


  ```
    Service Broker정보 목록을 조회한다.
  ```



  - **파라미터**

    - 없음


  - **사용예시**

  ```
  $ cf service-brokers
  ```


#### create-service-broker

  - **기본 Syntax**


  ```
      $ cf create-service-broker <SERVICE_BROKER> <USERNAME> <PASSWORD> <URL>
  ```


  - **설명**


  ```
    Service Broker정보를 등록한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |서비스 브로커명             |O         |
  |USERNAME                   |사용자명                   |O         |
  |PASSWORD                   |패스워드                   |O         |
  |URL                        |서비스 브로커 URL           |O         |



  - **사용예시**

  ```
  $ cf create-service-broker mysql-service-broker admin password http://p-mysql.10.244.0.34.xip.io
  ```


#### update-service-broker

  - **기본 Syntax**


  ```
      $ cf update-service-broker <SERVICE_BROKER> <USERNAME> <PASSWORD> <URL>
  ```


  - **설명**


  ```
    Service Broker정보를 등록한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |서비스 브로커명             |O         |
  |USERNAME                   |사용자명                   |O         |
  |PASSWORD                   |패스워드                   |O         |
  |URL                        |서비스 브로커 URL           |O         |



  - **사용예시**

  ```
  $ cf update-service-broker mysql-service-broker admin password http://p-mysql.10.244.0.34.xip.io
  ```


#### delete-service-broker

  - **기본 Syntax**


  ```
      $ cf delete-service-broker <SERVICE_BROKER> [-f]
  ```


  - **설명**


  ```
    Service Broker정보를 삭제한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER    |서비스 브로커명                                          |O         |
  |-f                |삭제 확인메시지 없이 SERVICE BROKER 정보를 삭제한다.       |X         |



  - **사용예시**

  ```
  $ cf delete-service-broker mysql-service-broker
  ```


#### rename-service-broker

  - **기본 Syntax**


  ```
      $ cf rename-service-broker <SERVICE_BROKER> <NEW_SERVICE_BROKER>
  ```


  - **설명**


  ```
    Service Broker명을 수정한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER     |서비스 브로커명             |O         |
  |NEW_SERVICE_BROKER |변경할 서비스 브로커명       |O         |



  - **사용예시**

  ```
  $ cf rename-service-broker mysql-service-broker new_mysql-service-broker
  ```


#### migrate-service-broker

  - **기본 Syntax**


  ```
      $ cf migrate-service-instances <v1_SERVICE> <v1_PROVIDER> <v1_PLAN> <v2_SERVICE> <v2_PLAN>
  ```


  - **설명**


  ```
    서비스 인스턴스에서 사용하는 서비스 및 플랜을 다른 플랜으로 변경한다. <br> - App이 사용하는 서비스를 다른 서비스로 변경하려 할때 사용한다.
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |v1_SERVICE     |기존 서비스 명                         |O         |
  |v1_PROVIDER    |기존 서비스를 제공하는 제공자            |O         |
  |v1_PLAN        |기존 서비스 인스턴스에서 사용하는 플랜    |O         |
  |v2_SERVICE     |신규 서비스 명                         |O         |
  |v2_PLAN        |신규 서비스에서 사용하는 플랜            |O         |  


  - **사용예시**

  ```
  $ cf migrate-service-instances p-mysql mysql-provider silver  postgres silver
  ```

#### purge-service-offering

  - **기본 Syntax**


  ```
      $ cf purge-service-offering <SERVICE> [-p PROVIDER]
  ```


  - **설명**


  ```
    cf와 서비스 브로커간의 정보 불일치를 해결할때 사용 <br>   (migrate-service-instances 명령 이후 사용)
  ```



  - **파라미터**


  | 파라미터명   |           설명                 | 필수(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |서비스 명                                  |O         |
  |-p PROVIDER  |서비스 제공자                               |O         |
  |-f           |삭제 확인메시지 없이 서비스 정보를 삭제한다    |O         |



  - **사용예시**

  ```
  $ cf purge-service-offering
  ```
