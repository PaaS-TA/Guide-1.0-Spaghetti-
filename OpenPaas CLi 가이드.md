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


 | cf [global options] command <arguments...> [command options] |
 |--------------------------------------------------------------|


cf 명령어에 대괄호로 묶인 인자인 <arguments>과 [command options]는 명령어에 따라 선택적으로 사용되고, command 인자는 필수 인자이다. CF 운영 및 관리하기 위한 도구인 CF CLI 아래와 같이 많은 명령어들을 제공하고 있다.


## GETTING STARTED

#### login
- **기본 Syntax**


| $ cf login [-a API_URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE] |
|-------------------------------------------------------------------------------------------|


|설명 class="css3"| OpenPaas에게 로그인 하기 위한 명령어|
|---|-----------------------------------|


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


| $ cf logout|
|-------------------------------------------------------------------------------------------|


|설명| cf에서 logout한다.|
|---|-----------------------------------|


- **파라미터**

  -없음


- **사용예시**

```
$ cf logout

```

#### passwd
- **기본 Syntax**


| $ cf passwd|
|-------------------------------------------------------------------------------------------|


|설명| OpenPaas 사용자계정의 패스워드를 변경한다.|
|---|-----------------------------------|


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


| $ cf target [-o ORG] [-s SPACE]|
|-------------------------------------------------------------------------------------------|


|설명| 로그인한 사용자가 사용할 Target 조직 및 스페이스 설정|
|---|-----------------------------------|


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

| $ cf api <URL>|
|-------------------------------------------------------------------------------------------|


|설명 | Target api를 조회하거나 target api URL을 설정한다.|
|---|-----------------------------------|


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

| $ cf auth <USERNAME> <PASSWORD>|
|-------------------------------------------------------------------------------------------|


|설명 |OpenPaas login시 로그인만 되며 스페이스, 타겟은 지정되지 않는다..|
|---|-----------------------------------|


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


| $cf apps |
|-------------------------------------------------------------------------------------------|


|설명| 타겟 스페이스에 App 목록을 조회한다.|
|---|-----------------------------------|


- **파라미터**

  -없음

  - **사용예시**

  ```
  $ cf apps
  ```

#### app

  - **기본 Syntax**


  | $cf app <APP_NAME> |
  |-------------------------------------------------------------------------------------------|


  |설명| App의 상태를 조회한다. |
  |---|-----------------------------------|


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


  | $ cf push <APP_NAME> [-b BUILDPACK_NAME] [-c COMMAND] [-d DOMAIN] [-f MANIFEST_PATH] [-i NUM_INSTANCES] [-k DISK] [-m MEMORY] [-n HOST] [-p PATH] [-s STACK] [-t TIMEOUT] [--no-hostname] [--no-manifest] [--no-route] [--no-start]  |
  |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


  |설명| App을 OpenPaas에 배포 하고 app을 Start한다. |
  |---|-----------------------------------|


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


    | $ cf scale <APP_NAME> [-i INSTANCES] [-k DISK] [-m MEMORY] [-f]  |
    |---------------------------------------------------------------------------------------------------------------|


    |설명| App의 메모리,디스크 크기 및 인스턴스 갯수를 조정한다.  |
    |---|-----------------------------------|


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


    | $ cf delete <APP_NAME> [--f] [--r]  |
    |------------------------------------------------------------------|


    |설명| App을 삭제한다.  |
    |---|-----------------------------------|


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
