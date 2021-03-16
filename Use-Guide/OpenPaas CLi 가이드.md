## Table of Contents
1. [Outline](#Outline)
     * [Document Objectivce](#Document-Objectivce)
     * [Range](#Range)
     * [References Materials](#References-Materials)

1. [OpenPaaS CLI Basic anual](#ID-OpenPaaS-CLI-USAGE)

1. [GETTING STARTED](#ID-GETTING-STARTED)
     * [login](#login)
     * [logout](#logout)
     * [passwd](#passwd)
     * [target](#target)
     * [api](#api)
     * [auth](#auth)

1. [APPS](#ID-APPS)
     * [apps](#apps)
     * [app](#app)
     * [push, p](#push-p)
     * [scale](#scale)
     * [delete](#delete)
     * [rename](#rename)
     * [start, st](#start-st)
     * [stop, sp](#stop-sp)
     * [restart, rs](#restart-rs)
     * [restage, rg](#restage-rg)
     * [restart-app-instance](#restart-app-instance)
     * [events](#events)
     * [files](#files)
     * [logs](#logs)
     * [env, e](#env-e)
     * [set-env, se](#set-env-se)
     * [unset-env](#unset-env)
     * [stacks](#stacks)
     * [stack](#stack)
     * [copy-source](#copy-source)
     * [create-app-manifest](#create-app-manifest)

1. [SERVICES](#ID-SERVICES)
    * [marketplace, m](#marketplace-m)
    * [services, s](#services-s)
    * [service](#service)
    * [create-service](#create-service)
    * [update-service](#update-service)
    * [delete-service](#delete-service)
    * [rename-service](#rename-service)
    * [create-service-key, csk](#create-service-key-csk)
    * [service-keys, sk](#service-keys-sk)
    * [service-key](#service-key)
    * [delete-service-key, dsk](#delete-service-key-dsk)
    * [bind-service, bs](#bind-service-bs)
    * [unbind-service,us](#unbind-service-us)
    * [create-user-provided-service, cups](#create-user-provided-service-cups)
    * [update-user-provided-service, cups](#update-user-provided-service-uups)

1. [ORGS](#ID-ORGS)
    * [orgs, o](#orgs-o)
    * [org](#org)
    * [create-org](#create-org-co)
    * [delete-org](#delete-org)
    * [rename-org](#rename-org)

1. [SPACES](#ID-SPACES)
    * [spaces](#spaces)
    * [space](#space)
    * [create-space](#create-space)
    * [delete-space](#delete-space)
    * [rename-space](#rename-space)

1. [DOMAINS](#ID-DOMAINS)
    * [domains](#domains)
    * [create-domain](#create-domain)
    * [delete-domain](#delete-domain)
    * [create-shared-dommain](#create-shared-domain)
    * [delete-shared-dommain](#delete-shared-domain)

1. [ROUTES](#ID-ROUTES)
    * [routes, r](#routes-r)
    * [create-route](#create-route)
    * [update-route](#update-route)
    * [check-route](#check-route)
    * [map-route](#map-route)

1. [BUILDPACKS](#ID-BUILDPACKS)
    * [buildpacks](#buildpacks)
    * [create-buildpack](#create-buildpack)
    * [update-buildpack](#update-buildpack)
    * [rename-buildpack](#rename-buildpack)
    * [delete-buildpack](#delete-buildpack)

1. [USER ADMIN](#ID-USER-ADMIN)
    * [create-user](#create-user)
    * [delete-user](#delete-user)
    * [org-users](#org-users)
    * [set-org-role](#set-org-role)
    * [unset-org-role](#unset-org-role)
    * [space-user](#space-user)
    * [set-space-role](#set-space-role)
    * [unset-space-role](#unset-space-role)

1. [ORG ADMIN](#ID-ORG-ADMIN)
    * [quotas](#quotas)
    * [quota](#quota)
    * [set-quota](#set-quota)
    * [create-quota](#create-quota)
    * [delete-quota](#delete-quota)
    * [update-quota](#update-quota)
    * [shared-private-domain](#shared-private-domain)
    * [unshared-private-domain](#unshared-private-domain)

1. [SPACE ADMIN](#ID-SPACE-ADMIN)
    * [space-quotas](#space-quotas)
    * [space-quota](#space-quota)
    * [create-space-quota](#create-space-quota)
    * [update-space-quota](#update-space-quota)
    * [delete-space-quota](#delete-space-quota)
    * [set-space-quota](#set-space-quota)
    * [unset-space-quota](#unset-space-quota)

1. [SERVICE ADMIN](#ID-SERVICE-ADMIN)
    * [service-auth-tokens](#service-auth-tokens)
    * [create-service-auth-token](#create-service-auth-token)
    * [update-service-auth-token](#update-service-auth-token)
    * [delete-service-auth-token](#delete-service-auth-token)
    * [service-brokers](#service-brokers)
    * [create-service-broker](#create-service-broker)
    * [create-service-broker](#create-service-broker)
    * [update-service-broker](#update-service-broker)
    * [delete-service-broker](#delete-service-broker)
    * [rename-service-broker](#rename-service-broker)
    * [migrate-service-broker](#migrate-service-broker)
    * [purge-service-offering](#purge-service-offering)
    * [service-access](#service-access)
    * [enable-service-access](#enable-service-access)
    * [disable-service-access](#disable-service-access)

1. [SECURITY GROUP](#ID-SECURITY-GROUP)
    * [security-group](#security-group)
    * [security-groups](#security-groups)
    * [create-security-group](#create-security-group)
    * [update-security-group](#update-security-group)
    * [delete-security-group](#delete-security-group)
    * [bind-security-group](#bind-security-group)
    * [unbind-security-group](#unbind-security-group)
    * [bind-staging-security-group](#bind-staging-security-group)
    * [staging-security-groups](#staging-security-groups)
    * [unbind-staging-security-group](#unbind-staging-security-group)
    * [running-security-group](#running-security-group)

1. [ENVIRONMENT VARIABLE GROUPS](#ID-ENVIRONMENT-VARIABLE-GROUPS)
    * [running-environment-variable-group, revg](#running-environment-variable-group-revg)
    * [staging-environment-variable-group, sevg](#staging-environment-variable-group-sevg)
    * [set-staging-environment-variable-group, ssevg](#set-staging-environment-variable-group-ssevg)
    * [set-running-environment-variable-group, ssevg](#set-running-environment-variable-group-ssevg)

1. [FEATURE FLAGS](#ID-FEATURE-FLAGS)
    * [feature-flags](#feature-flags)
    * [feature-flag](#feature-flag)
    * [enable-feature-flag](#enable-feature-flag)
    * [disable-feature-flag](#disable-feature-flag)

1. [ADVANCE](#ID-ADVANCE)
    * [curl](#curl)
    * [config](#config)
    * [oauth-token](#oauth-token)

1. [ADD/REMOVE PLUGIN REPOSITORY](#ID-ADD-REMOVE-PLUGIN-REPOSITORY)
    * [add-plugin-repo](#add-plugin-repo)
    * [remove-plugin-repo](#remove-plugin-repo)
    * [list-plugin-repos](#list-plugin-repos)
    * [repo-plugins](#repo-plugins)

1. [ADD/REMOVE PLUGIN](#ID-ADD-REMOVE-PLUGIN)
    * [plugins](#plugins)
    * [install-plugin](#install-plugin)

## Outline
---

#### Document Objective

This document aims to understand OpenPaaS through basic usage and usage examples of OpenPaaS CLI, a tool for installation and operation management of OpenPaaS.

#### Range

This document has been written about OpenPaaS CLI classification and basic usage.

#### References Materials

 This document was written with reference to CF Document of Cloud Foundry.

 [***https://docs.cloudfoundry.org/devguide/installcf/***](https://docs.cloudfoundry.org/devguide/installcf/)

<div id='ID-OpenPaaS-CLI-USAGE'/>
## OpenPaaS CLI Basic Manual

OpenPaaS CLI : CLI tool for managing OpenPaaS..

CLI is a command line utility that helps to manage OpenPaaS deployment and release. Its usage is as follows.



 - **basic Syntax**


 ```
cf [global options] command <arguments...> [command options]
 ```

Abbreviations are provided according to the OpenPaaS command. For example, the app start CLI command is start, but st can also be used.

- **Examples of abbreviations**

```
$ cf start

$ cf st
```

[Command options], an argument enclosed in brackets for the OpenPaaS command, is optionally used depending on the command, and the command `<arguments>` argument is a required argument. OpenPaaS CLI, a tool for operating and managing OpenPaaS, provides the following commands.

<div id='ID-GETTING-STARTED'/>
## GETTING STARTED


#### login


- **Basic Syntax**


```
$ cf login [-a API_URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE]
```


- **Description**


```
Command to log in to OpenPaaS
```


- **Parameter **


| Parameter Name   |           Description                 | Required(O/X) |
|-------------|-----------------------------|-----------|
|-a API_URL    |CLI가 접속 하려는 OpenPaaS  URL<br>Ex) https://api.10.244.0.34.xip.io    |X        |
|-u USERNAMEL  |User ID to access OpenPaaS             |X        |
|-p PASSWORD   |User password to access OpenPaaS         |X        |
|-o ORG        |The name of the organization of the user who accesses OpenPaaS     |X        |
|-s SPACE      |Name of the space in the organization of the user who accesses OpenPaaS      |X        |


- **Example**


```
# In case of the  parameter is specified
$ cf login --skip-ssl-validation -a https://api.10.244.0.34.xip.io -u admin -p admin -o crossent -s development

# In case of the  parameter is not specified
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
- **Basic Syntax**


```
$ cf logout
```


- **Description**


```
Logout to cf.
```


- **parameter**

  -none


- **Example**

```
$ cf logout

```

#### passwd
- **Basic Syntax**


```
$ cf passwd
```



- **Description**


```
Change the password of the OpenPaaS user account.
```


- **parameter**

  -none


- **Example**


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
- **Basic Syntax**


```
$ cf target [-o ORG] [-s SPACE]
```

- **Description**


```
Set the target organization and space to be used by the logged-in user.
```


- **parameter**

| Parameter Name   |           Description                 | Required(O/X) |
|-------------|-----------------------------|-----------|
|-o ORG      |Target organization                    |X        |
|-s SPACE    |Target space                |X        |



- **Example**

```
# In case of the  parameter is specified
$ cf target -o cf -s development
API endpoint:   https://api.10.244.0.34.xip.io (API version: 2.29.0)   
User:           admin   
Org:            cf   
Space:          development
# In case of the  parameter is not specified(Currently targeted information is outputted)
$ cf target
API endpoint:   https://api.10.244.0.34.xip.io (API version: 2.29.0)   
User:           admin   
Org:            cf
Space:          development  
```


#### api
- **Basic Syntax**

```
$ cf api <URL>
```


- **Description**


```
Lookup the target api or set up the target api URL.
```



- **Parameter **

| Parameter Name   |           Description                 | Required(O/X) |
|-------------|-----------------------------|-----------|
|URL         |Api Target URL                   |O        |


- **Example**

```
$ cf api --skip-ssl-validation api.10.244.0.34.xip.io
```

#### auth
- **Basic Syntax**


```
$ cf auth <USERNAME> <PASSWORD>
```


- **Description**


```
When logging in with OpenPaaS, only login and no space or target are specified.
```


- **Parameter **

| Parameter Name   |           Description                 | Required(O/X) |
|-------------|--------------------------------|-----------|
|USERNAME     |Login user ID                 |O        |
|PASSWORD    |Login user PASSWORD            |O        |



- **Example**

```
$ cf api --skip-ssl-validation api.10.244.0.34.xip.io
```
<div id='ID-APPS'/>
## APPS


#### apps
- **Basic Syntax**


```
$cf apps
```


- **Description**


```
Look up the list of apps in the target space.
```


- **Parameter **

  -none

  - **Example**

  ```
  $ cf apps
  ```

#### app

  - **Basic Syntax**


  ```
  $cf app <APP_NAME>
  ```


  - **Description**


  ```
  Inquires the status of the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP Name                          |O        |


    - **Example**

    ```
    $ cf app spring-music
    ```

<div id='push-p'/>
#### push,p

  - **Basic Syntax**


  ```
  $ cf push <APP_NAME> [-b BUILDPACK_NAME] [-c COMMAND] [-d DOMAIN] [-f MANIFEST_PATH] [-i NUM_INSTANCES] [-k DISK] [-m MEMORY] [-n HOST] [-p PATH] [-s STACK] [-t TIMEOUT] [--no-hostname] [--no-manifest] [--no-route] [--no-start]
  ```


  - **Description**


  ```
  Deploy the app to OpenPaaS and start the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App name to push (directory name if not specified)                         |O        |
  |-b BUILDPACK |custom buildpack URL <br> ex) https://github.com/OpenPaaSRnD/egov-java-buildpack   |X        |
  |-c COMMAND   |App start command              |X        |
  |-d DOMAIN    |App domain                      |X        |
  |-f MANIFEST_PATH    |Manifest file path       |X        |
  |-i NUM_INSTANCES     |the number of App instance       |X        |
  |-m MEMORY     |instance memory capacity             |X        |
  |-k DISK     |disk use capacity                  |X        |
  |-n HOST     |host name <br> ex) my-subdomain)  |X        |
  |-p PATH     |directory path of App or App file(zip,war etc.)path   |X        |
  |-s STACK    |The operating system the app is running on (default: cflinuxfs2)       |X        |
  |-t TIMEOUT  |The timeout time that the CLI waits while the app is running                     |X        |
  |--no-hostname     |Mapping root domain to app                          |X        |
  |--no-manifest     |Ignore manifest file                        |X        |
  |--no-route     |Delete route information in pushed app and do not map route information to the app|X        |
  |--no-start     |Push the app and don't start                       |X        |
  |--random-route    |Randomly generate route information to the app        |X        |

  - **Example**

  ```
  $ cf push spring-music
  ```

#### scale

  - **Basic Syntax**


  ```
  $ cf scale <APP_NAME> [-i INSTANCES] [-k DISK] [-m MEMORY] [-f]
  ```


  - **Description**


  ```
  Adjust the memory, disk size and number of instances of the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |-i INSTANCES |Number of instances                 |X        |
  |-k DISK      |Disk capacity                      |X        |
  |-m MEMORY    |Memory capacity                     |X        |
  |-f           |App restart forcibly         |X        |


  - **Example**

  ```
  $ cf scale spring-music -i 2 -m 512m
  ```


#### delete

  - **Basic Syntax**


  ```
  $ cf delete <APP_NAME> [--f] [--r]
  ```


  - **Description**


  ```
Delete the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP Name                           |O        |
  |--f          |Delete the app without confirmation              |X        |
  |--r          |Delete route information mapped to App  |X        |


  - **Example**

  ```
  $  cf delete spring-music
  ```


#### rename

  - **Basic Syntax**


  ```
  $ cf rename <APP_NAME> <NEW_APP_NAME>
  ```


  - **Description**


  ```
Change the app name.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP Name                           |O        |
  |NEW_APP_NAME |App Name to  be change                 |O        |


  - **Example**

  ```
  $  cf rename spring-music new-spring-music
  ```

<div id='start-st'/>
#### start,st

  - **Basic Syntax**


  ```
  $ cf start <APP_NAME>
  ```


  - **Description**


  ```
  Running the App
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP Name                           |O        |


  - **Example**

  ```
  $  cf start spring-music
  ```
<div id='stop-sp'/>
#### stop,sp

  - **Basic Syntax**


  ```
  $ cf stop <APP_NAME>
  ```


  - **Description**


  ```
  Stop the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |


  - **Example**

  ```
  $  cf stop spring-music
  ```

<div id='restart-rs'/>
#### restart, rs

  - **Basic Syntax**


  ```
  $ cf restart <APP_NAME>
  ```


  - **Description**


  ```
  Restart the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |


  - **Example**

  ```
  $cf restart spring-music
  ```
<div id='restage-rg'/>
#### restage, rg

  - **Basic Syntax**


  ```
  $ cf restage <APP_NAME>
  ```


  - **Description**


  ```
  Restage the app.(Used when setting environment variables or binding services)
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |


  - **Example**

  ```
  $cf restage spring-music
  ```

#### restart-app-instance

  - **Basic Syntax**


  ```
  $ cf restart-app-instance <APP_NAME> <INDEX>
  ```


  - **Description**


  ```
  Restart a specific instance of App.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |INDEX        |Instance index                   |O        |

  - **Example**

  ```
  $cf restart-app-instance spring-music 1
  ```

#### events

  - **Basic Syntax**


  ```
      $ cf events <APP_NAME>
  ```


  - **Description**


  ```
    Search the latest event information that occurred in the app. (History of start/stop/scale, etc.)
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |

  - **Example**

  ```
  $ cf events spring-music
  ```


#### files

  - **Basic Syntax**


  ```
  $ cf files <APP_NAME> [PATH] [-i INSTANCE]
  ```


  - **Description**


  ```
  Search the list of files and directories of the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |PATH         |Directory of APP                    |X        |
  |-i INSTANCE  |App Instance Index                 |X        |

  - **Example**

  ```
  $  cf files spring-music
  ```

#### logs

  - **Basic Syntax**


  ```
  $ cf logs <APP_NAME> [--recent]
  ```


  - **Description**


  ```
  Search the logs generated by the app
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |PATH         |Directory of APP                  |X        |
  |-i INSTANCE  |App Instance Index                 |X        |

  - **Example**

  ```
  $  cf logs spring-music
  ```
<div id='env-e'/>
#### env,e

  - **Basic Syntax**


  ```
  $ cf env  <APP_NAME>
  ```


  - **Description**


  ```
  Inquiries the environment variable on app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |

  - **Example**

  ```
  $ cf env spring-music
  ```

<div id='set-env-se'/>
#### set-env,se

  - **Basic Syntax**


  ```
  $ cf set-env <APP_NAME> <ENV_VAR_NAME> <ENV_VAR_VALUE>
  ```


  - **Description**


  ```
  Set environment variables of the app. (Requires restage when applied)
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |ENV_VAR_NAME |App의 environment variables Key               |O        |
  |ENV_VAR_VALUE|App의 environment variables Value               |O        |


  - **Example**

  ```
  $ cf se spring-music author Jim
  ```



#### unset-env

  - **Basic Syntax**


  ```
  $ cf unset-env <APP_NAME> <ENV_VAR_NAME>
  ```


  - **Description**


  ```
  App에 설정된 environment variables를 삭제합니다.(적용시 restage필요)
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |ENV_VAR_NAME |environment variables Key of APP               |O        |


  - **Example**

  ```
  $ cf unset-env spring-music author
  ```

#### stacks

  - **Basic Syntax**


  ```
  $ cf stacks
  ```


  - **Description**


  ```
  OpenPaaS의 stack목록(운영체제 파일시스템) 목록을 조회합니다.
  ```


  - **Parameter **


    - none


  - **Example**

  ```
  $  cf stacks
  ```

#### stack

  - **Basic Syntax**


  ```
  $ cf stack <STACK_NAME> [--guid]
  ```


  - **Description**


  ```
  Inquire OpenPaaS stack list (OS file system) 
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                           |O        |
  |--guid       |Inquire Stack guid             |X        |


  - **Example**

  ```
  $  cf stack cflinuxfs2
  ```

#### copy-source

  - **Basic Syntax**


  ```
  $ cf copy-source <SOURCE-APP> <TARGET-APP> [-o TARGET-ORG] [-s TARGET-SPACE] [--no-restart]
  ```


  - **Description**


  ```
  App의 소스를 다른 App에 복사합니다. 파일이 덥어 쓰이지 않으면 자동 restart합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |원본 APP  name                        |O        |
  |TARGET-APP   |소스가 복사될 대상 App  name            |X        |
  |-o TARGET-ORG |타겟 조직                         |O        |
  |-s TARGET-SPACE|타겟 스페이스                    |X        |
  |--no-restart   |소스 복사 후 restart하지 않음  |X        |

  - **Example**

  ```
  $ cf copy-source spring-music another-music
  ```

#### create-app-manifest

  - **Basic Syntax**


  ```
  $ cf create-app-manifest <APP_NAME> [-p /path/<app-name>-manifest.yml]
  ```


  - **Description**


  ```
  App의 manifest파일을 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |원본 APP  name                        |O        |
  |-p /path/<app-name>.yml   |파일이 생성될 위치와 file name(-p 를 사용하지 않으면 자동생성된다)            |X        |

  - **Example**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```

<div id='ID-SERVICES'/>
## SERVICES

<div id='marketplace-m'/>
#### marketplace,m

  - **Basic Syntax**


  ```
  $ cf marketplace [-s SERVICE_NAME]
  ```


  - **Description**


  ```
  cf 마켓플레이스에서 제공하는 서비스 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |서비스의 plan이 조회된다.    |X        |


  - **Example**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```
<div id='services-s'/>
#### services,s

  - **Basic Syntax**


  ```
  $ cf services
  ```


  - **Description**


  ```
  타겟 스페이스에 Service Instance목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |서비스의 plan이 조회된다.    |X        |


  - **Example**

  ```
  $  cf create-app-manifest spring-music -p ./spring-music-manifest.yml
  ```

#### service

  - **Basic Syntax**


  ```
  $ cf service <SERVICE_INSTANCE> [--guid]
  ```


  - **Description**


  ```
  서비스 인스턴스의 정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE   |Service Instance name           |O        |
  |--guid             |서비스 인스턴스의 Guid를 조회합니다.   |X        |

  - **Example**

  ```
  $ cf service spring-music-db
  ```


#### create-service

  - **Basic Syntax**


  ```
  $ cf create-service <SERVICE> <PLAN> <SERVICE_INSTANCE> [-c PARAMETERS_AS_JSON] [-t TAGS]
  ```


  - **Description**


  ```
  마켓플레이스에서 제공하는 서비스로 서비스 인스턴스를 만든다.
  ```

  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |마켓플레이스에 있는 서비스  name                                              |O        |
  |PLAN         |서비스 플랜  name                                                           |O        |
  |SERVICE_INSTANCE   |만들 Service Instance name                                             |O        |
  |-c PARAMETERS_AS_JSON |서비스 설정정보를 json 형태로 입력 <br> Ex) -c '{"ram_gb":4}'    |X        |
  |-t TAGS      |Service Instance테그                                                     |X        |

  - **Example**

  ```
  $ cf create-service spring-music-db silver p-mysql
  ```


#### update-service

  - **Basic Syntax**


  ```
  $ cf update-service <SERVICE_INSTANCE> [-p NEW_PLAN] [-c PARAMETERS_AS_JSON] [-t TAGS]
  ```


  - **Description**


  ```
  서비스 인스턴스를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE        |Service Instance name                                               |O        |
  |-p NEW_PLAN             |서비스 플랜  name                                                  |O        |
  |-c PARAMETERS_AS_JSON   |서비스 설정정보를 json 형태로 입력 <br> Ex) -c '{"ram_gb":4}'    |O        |
  |-t TAGS                 |Service Instance테그                                            |X        |

  - **Example**

  ```
  $ cf update-service spring-music-db -p gold_plan
  ```


#### delete-service

  - **Basic Syntax**


  ```
  $ cf delete-service SERVICE_INSTANCE [-f]
  ```


  - **Description**


  ```
  서비스 인스턴스를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                                        |O        |
  |-f                |삭제 확인 메시지 없이 Service Instance삭제합니다.             |X        |

  - **Example**

  ```
  $ cf delete-service spring-music-db
  ```

#### rename-service

  - **Basic Syntax**


  ```
  $ cf rename-service <SERVICE_INSTANCE> <NEW_SERVICE_INSTANCE>
  ```


  - **Description**


  ```
  Service Instance name을 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |Service Instance name                       |O        |
  |NEW_SERVICE_INSTANCE   |변경하려는 Service Instance name             |O        |

  - **Example**

  ```
  $ cf rename-service spring-music-db new_spring-music-db
  ```

<div id='create-service-key-csk'/>
#### create-service-key,csk

  - **Basic Syntax**


  ```
  $ cf create-service-key <SERVICE_INSTANCE> <SERVICE_KEY> [-c PARAMETERS_AS_JSON]
  ```


  - **Description**


  ```
  서비스 인스턴스의 key를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |Service Instance name                       |O        |
  |SERVICE_KEY            |Service Instancekey  name                   |O        |
  |-c PARAMETERS_AS_JSON  |Service Instance설정(JSON Parameter)    |X        |


  - **Example**

  ```
  $ cf create-service-key spring-music-db mykey -c '{"permissions":"read-only"}'
  ```

<div id='service-keys-sk'/>
#### service-keys,sk

  - **Basic Syntax**


  ```
  $ cf service-keys <SERVICE_INSTANCE>
  ```


  - **Description**


  ```
  서비스 인스턴스의 key 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |Service Instance name                       |O        |


  - **Example**

  ```
  $ cf service-keys spring-music-db
  ```

#### service-key

  - **Basic Syntax**


  ```
  $ cf service-key <SERVICE_INSTANCE> <SERVICE_KEY> [--guid]
  ```


  - **Description**


  ```
  서비스 인스턴스의 key의 상세정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                       |O        |
  |SERVICE_KEY       |Service Instancekey  name                   |O        |
  |--guid            |Service Instanceguid를 조회합니다.         |X        |



  - **Example**

  ```
  $ cf service-key spring-music-db mykey
  ```

<div id='delete-service-key-dsk'/>
#### delete-service-key,dsk

  - **Basic Syntax**


  ```
  $ cf delete-service-key <SERVICE_INSTANCE> <SERVICE_KEY> [-f]
  ```


  - **Description**


  ```
  서비스 key를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                       |O        |
  |SERVICE_KEY       |Service Instancekey  name                   |O        |
  |--guid            |Service Instanceguid를 조회합니다.         |X        |



  - **Example**

  ```
  $ cf delete-service-key spring-music-db mykey
  ```

<div id='bind-service-bs'/>
#### bind-service,bs

  - **Basic Syntax**


  ```
  $ cf bind-service <APP_NAME> <SERVICE_INSTANCE> [-c PARAMETERS_AS_JSON]
  ```


  - **Description**


  ```
  App과 서비스 인스턴스를 바인딩합니다.<br> - 서비스 인스턴스와 APP을 바인딩해야 App에서 서비스 사용가능
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                                            |O        |
  |SERVICE_INSTANCE  |Service Instance  name                           |O        |
  |-c PARAMETERS_AS_JSON   |바인딩 설정 Parameter  (json형태)         |X        |



  - **Example**

  ```
  $ cf bind-service spring-music spring-music-db -c '{"permissions":"read-only"}'

  $ cf bind-service spring-music spring-music-db -c ~/workspace/tmp/instance_config.json
  ```

<div id='unbind-service-us'/>
#### unbind-service,us

  - **Basic Syntax**


  ```
  $ cf unbind-service <APP_NAME> <SERVICE_INSTANCE>
  ```


  - **Description**


  ```
  App과 서비스 인스턴스를 언바인딩합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME          |Service Instance name                            |O        |
  |SERVICE_INSTANCE  |Service Instance  name                           |O        |



  - **Example**

  ```
  $ cf unbind-service spring-music spring-music-db
  ```

<div id='create-user-provided-service-cups'/>
#### create-user-provided-service,cups

  - **Basic Syntax**


  ```
  $ cf create-user-provided-service <SERVICE_INSTANCE> [-p CREDENTIALS] [-l SYSLOG-DRAIN-URL]
  ```


  - **Description**


  ```
  Market place에서 제공하는 서비스를 사용하지 않고 사용자가 별도의 서비스를 구성하여 APP과 바인딩합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE          |Service Instance name                            |O        |
  |-p CREDENTIALS            |Service Instance  name                           |X        |
  |-l SYSLOG-DRAIN-URL       |Service Instance  name                           |X        |


  - **Example**

  ```
  $ cf create-user-provided-service spring-music-db -p '{"username":"admin","password":"pa55woRD"}'
  ```

<div id='update-user-provided-service-uups'/>
#### update-user-provided-service,uups

  - **Basic Syntax**


  ```
  $ cf update-user-provided-service <SERVICE_INSTANCE> [-p CREDENTIALS] [-l SYSLOG-DRAIN-URL]
  ```


  - **Description**


  ```
  user-provided service instance 정보를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE          |Service Instance name                            |O        |
  |-p CREDENTIALS            |Service Instance  name                           |X        |
  |-l SYSLOG-DRAIN-URL       |Service Instance  name                           |X        |


  - **Example**

  ```
  $  cf update-user-provided-service spring-music-db -p '{"username":"admin","password":"pa55woRD"}'
  ```

<div id='ID-ORGS'/>
## ORGS

<div id='orgs-o'/>
#### orgs,o

  - **Basic Syntax**


  ```
  $ cf orgs
  ```


  - **Description**


  ```
  조직정보 목록을 조회합니다...
  ```


  - **Parameter **

   - none


  - **Example**

  ```
  $ cf orgs
  ```



#### org

  - **Basic Syntax**


  ```
  $ cf org <ORG_NAME>
  ```


  - **Description**


  ```
  조직 상세 정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                         |O        |
  |--guid       |조직의 guid를 조회합니다.           |X        |


  - **Example**

  ```
  $ cf org cf
  ```


<div id='create-org-co'/>
#### create-org,co

  - **Basic Syntax**


  ```
  $ cf create-org <ORG_NAME> [-q QUOTA_NAME]
  ```


  - **Description**


  ```
  조직정보를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                         |O        |
  |-q QUOTA_NAME |조직에게 할당할 quota           |X        |


  - **Example**

  ```
  $cf create-org test -q default
  ```



#### delete-org

  - **Basic Syntax**


  ```
  $ cf delete-org <ORG_NAME> [-f]
  ```


  - **Description**


  ```
  조직정보 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                          |O        |
  |-f           |확인메시지 없이 조직정보 삭제합니다.  |X        |


  - **Example**

  ```
  $ cf delete-org cf -f
  ```


#### rename-org

  - **Basic Syntax**


  ```
    $ cf rename-org <ORG_NAME> <NEW_ORG_NAME>
  ```


  - **Description**


  ```
  Organization Name을 변경합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME       |Organization Name                          |O        |
  |NEW_ORG_NAME   |변경할 Organization Name                    |O        |


  - **Example**

  ```
  $ cf rename cf new-cf
  ```

<div id='ID-SPACES'/>
## SPACES

#### spaces

  - **Basic Syntax**


  ```
  $ cf spaces
  ```


  - **Description**


  ```
  스페이스 목록을 가져온다.
  ```


  - **Parameter **

   - none


  - **Example**

  ```
  $ cf spaces
  ```


#### space

  - **Basic Syntax**


  ```
  $ cf space <SPACE_NAME>
  ```


  - **Description**


  ```
  스페이스 상세정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O          |


  - **Example**

  ```
  $ cf space development
  ```

#### create-space

  - **Basic Syntax**


  ```
  $ cf create-space <SPACE_NAME> [-o ORG_NAME] [-q SPACE-QUOTA-NAME]
  ```


  - **Description**


  ```
  스페이스 정보를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |-o ORG_NAME  |스페이스에 매핑될 Organization Name               |X         |
  |-q SPACE-QUOTA-NAME    |스페이스에 할당될 QUOTA  name    |X         |

  - **Example**

  ```
  $ cf create-space -o cf -q cf-space-quota
  ```

#### delete-space

  - **Basic Syntax**


  ```
  $ cf delete-space <SPACE_NAME> [-f]
  ```


  - **Description**


  ```
  스페이스정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |-f           |삭제 확인메시지 없이 스페이스 삭제합니다. |X         |

  - **Example**

  ```
  $ cf delete-space development
  ```

#### rename-space

  - **Basic Syntax**


  ```
  $ cf rename-space <SPACE_NAME> <NEW_SPACE_NAME>
  ```


  - **Description**


  ```
  스페이스   name을 변경합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME     |Space Name                           |O         |
  |NEW_SPACE_NAME |삭제 확인메시지 없이 스페이스 삭제합니다. |O         |

  - **Example**

  ```
  $ cf rename-space development new_development
  ```

<div id='ID-DOMAINS'/>
## DOMAINS

#### domains

  - **Basic Syntax**


  ```
  $ cf domains
  ```


  - **Description**


  ```
  도메인 정보 목록을 조회합니다.
  ```


  - **Parameter **


    - none


  - **Example**

  ```
  $ cf domains
  ```


#### create-domain

  - **Basic Syntax**


  ```
  $ cf create-domain <ORG_NAME> <DOMAIN>
  ```


  - **Description**


  ```
  도메인 정보 목록을 생성합니다. 생성된 도메인은 설정된 조직에서 사용가능하다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME     |Organization Name                           |O         |
  |DOMAIN       |도메인  name                          |O         |


  - **Example**

  ```
  $ cf create-domain cf-org cf.or.kr
  ```


#### delete-domain

  - **Basic Syntax**


  ```
  $ cf delete-domain <DOMAIN> [-f]
  ```


  - **Description**


  ```
  도메인 정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |도메인  name                           |O         |
  |-f           |삭제 확인메시지 없이 도메인을 삭제합니다. |X         |


  - **Example**

  ```
  $ cf delete-domain cf.or.kr
  ```


#### create-shared-domain

  - **Basic Syntax**


  ```
  $ cf create-shared-domain <DOMAIN>
  ```


  - **Description**


  ```
  공유 도메인정보를 생성한다
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |Organization Name                           |O         |


  - **Example**

  ```
  $ cf create-shared-domain cf.or.kr
  ```

#### delete-shared-domain

  - **Basic Syntax**


  ```
  $ cf delete-shared-domain <DOMAIN> [-f]
  ```


  - **Description**


  ```
  공유 도메인정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |Organization Name                           |O         |
  |-f           |삭제 확인메시지 없이 도메인을 삭제합니다.    |X         |

  - **Example**

  ```
  $ cf delete-shared-domain cf.or.kr
  ```

<div id='ID-ROUTES'/>
## REOUTES

<div id='routes-r'/>
#### routes, r

  - **Basic Syntax**


  ```
  $ cf routes
  ```


  - **Description**


  ```
  현재 조직/스페이스에 존재하는 라우트 정보목롤을 조회합니다.
  ```


  - **Parameter **

    - none

  - **Example**

  ```
  $ cf routes
  ```


#### create-route

  - **Basic Syntax**


  ```
  $ cf create-route <SPACE_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **Description**


  ```
  공유 도메인정보를 삭제합니다...
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제합니다. <br>   - 도메인 정보가 입력되어있어야 합니다.   |O         |
  |-n HOSTNAME  |호스트   name                          |X         |


  - **Example**

  ```
  $ cf create-route development cf.or.kr
  ```


#### update-route

  - **Basic Syntax**


  ```
  $ cf update-route <SPACE_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **Description**


  ```
  공유 도메인정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제합니다. <br>   - 도메인 정보가 입력되어있어야 합니다.   |O         |
  |-n HOSTNAME  |호스트   name                          |X         |


  - **Example**

  ```
  $ cf update-route development cf.or.kr
  ```



#### check-route

  - **Basic Syntax**


  ```
  $ cf check-route <HOST> <DOMAIN>
  ```


  - **Description**


  ```
  라우트 정보가 존재하는지 체크합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |HOST         |호스트   name                                    |O         |
  |DOMAIN       |삭제 확인메시지 없이 공유 도메인을 삭제합니다.    |O         |


  - **Example**

  ```
  $ cf check-route spring-music cf.or.kr
  ```


#### map-route

  - **Basic Syntax**


  ```
  $ cf map-route <APP_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **Description**


  ```
  App에게 URL route정보를 할당합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App  name                           |O         |
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |


  - **Example**

  ```
  $ cf map-route spring-music cf.or.kr -n test
  ```


#### unmap-route

  - **Basic Syntax**


  ```
  $ cf unmap-route <APP_NAME> <DOMAIN> [-n HOSTNAME]
  ```


  - **Description**


  ```
  App에게 URL route정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App  name                           |O         |
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |


  - **Example**

  ```
  $ cf unmap-route spring-music cf.or.kr -n spring-music
  ```

#### delete-route

  - **Basic Syntax**


  ```
  $ cf delete-route <DOMAIN> [-n HOSTNAME] [-f]
  ```


  - **Description**


  ```
  App에게 URL route정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |App에게 할당할 도메인             |O         |
  |-n HOSTNAME  |App에게 할당할 Host              |X         |
  |-f           |삭제 확인메시지 없이 라우트 정보를 삭제합니다.              |X         |


  - **Example**

  ```
  $ cf delete-route spring-music cf.or.kr -n spring-music
  ```

#### delete-orphaned-routes

  - **Basic Syntax**


  ```
  $ cf delete-orphaned-routes [-f]
  ```


  - **Description**


  ```
  App에 매핑되지 않은 라우트 정보를 모두 삭제한다
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-f           |삭제 확인메시지 없이 라우트 정보를 삭제합니다.           |X         |


  - **Example**

  ```
  $ cf delete-orphaned-routes
  ```

<div id='ID-BUILDPACKS'/>
## BUILDPACKS


#### buildpacks

  - **Basic Syntax**


  ```
  $ cf buildpacks
  ```


  - **Description**


  ```
  빌드팩 목록을 조회합니다.
  ```


  - **Parameter **


    - none


  - **Example**

  ```
  $ cf buildpacks
  ```


#### create-buildpack

  - **Basic Syntax**


  ```
  $ cf create-buildpack <BUILDPACK> <-p PATH> <-i POSITION> [--enable|--disable]
  ```


  - **Description**


  ```
  빌드팩을 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩  name                           |O         |
  |-p PATH       |빌드팩 path                      |O         |
  |-i POSITIONE  |빌드팩 auto-detection동안 빌드팩 체크 순서  <br> ex)1.2.3              |O         |
  |--enable      |스테이징시 사용                  |X         |
  |--disable     |스테이징시 미사용                |X         |



  - **Example**

  ```
  $ cf create-buildpack egov-buildpack ~/workspace/buildpack/egov -i 1
  ```


#### update-buildpack

  - **Basic Syntax**


  ```
  $ cf update-buildpack <BUILDPACK> [-p PATH] [-i POSITION] [--enable|--disable] [--lock|--unlock]
  ```


  - **Description**


  ```
  빌드팩 정보를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩  name                           |O         |
  |-p PATH       |빌드팩 path                      |O         |
  |-i POSITIONE  |빌드팩 auto-detection동안 빌드팩 체크 순서  <br> ex)1.2.3              |O         |
  |--enable      |스테이징시 사용                  |X         |
  |--disable     |스테이징시 미사용                |X         |



  - **Example**

  ```
  $ cf create-buildpack egov-buildpack ~/workspace/buildpack/egov -i 1
  ```

#### delete-buildpack

  - **Basic Syntax**


  ```
  $ cf delete-buildpack <BUILDPACK_NAME> [-f]
  ```


  - **Description**


  ```
  빌드팩을 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |빌드팩  name                           |O         |
  |-f           |삭제 확인메시지 없이 빌드팩 정보를 삭제       |X         |


  - **Example**

  ```
  $ cf delete-buildpack egov-buildpack
  ```

<div id='ID-USER-ADMIN'/>
## USER ADMIN


#### create-user

  - **Basic Syntax**


  ```
  $ cf create-user <USERNAME> <PASSWORD>
  ```


  - **Description**


  ```
  새로운 사용자 계정을 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자 ID                        |O         |
  |PASSWORD     |패스워드                         |O         |


  - **Example**

  ```
  $ cf create-user cfuser userpassword
  ```


#### delete-user

  - **Basic Syntax**


  ```
  $ cf delete-user <USERNAME> [-f]
  ```


  - **Description**


  ```
    새로운 사용자 계정을 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |사용자 ID                       |O         |
  |-f           |삭제 확인메시지 없이 사용자 정보를 삭제                         |X         |


  - **Example**

  ```
  $ cf delete-user cfuser
  ```


#### org-users

  - **Basic Syntax**


  ```
  $ cf org-users <ORG_NAME>
  ```


  - **Description**


  ```
  조직에 소속된 사용자를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME     |Organization Name                          |O         |


  - **Example**

  ```
  $ cf org-users cforg
  ```

#### set-org-role

  - **Basic Syntax**


  ```
  $ cf set-org-role <USERNAME> <ORG> <ROLE>
  ```


  - **Description**


  ```
  사용자에게 특정조직의 role을 설정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                          |O         |
  |ORG          |Organization Name                          |O         |
  |ROLE        |역할  name <br>  - OrgManager : 사용자 관리 및 plan설정/변경 권한 <br> - BillingManager : 빌링계정 및 과금정보 생성 및 관리 <br>  - OrgAuditor : 조직 quota사용률 및 사용자 role을 조회             |O         |



  - **Example**

  ```
  $ cf set-org-role cfuser cforg OrgManager
  ```


#### unset-org-role

  - **Basic Syntax**


  ```
  $ cf unset-org-role <USERNAME> <ORG> <ROLE>
  ```


  - **Description**


  ```
  사용자에게 특정조직의 role을 설정을 해제합니다..
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                          |O         |
  |ORG          |Organization Name                          |O         |
  |ROLE        |역할  name <br>  - OrgManager : 사용자 관리 및 plan설정/변경 권한 <br> - BillingManager : 빌링계정 및 과금정보 생성 및 관리 <br>  - OrgAuditor : 조직 quota사용률 및 사용자 role을 조회             |O         |



  - **Example**

  ```
  $ cf unset-org-role cfuser cforg OrgManager
  ```


#### space-users

  - **Basic Syntax**


  ```
  $ cf space-users <ORG> <SPACE>
  ```


  - **Description**


  ```
  조직의 스페이스에 할당된 사용자 목록정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG          |Organization Name                          |O         |
  |SPACE        |Space Name                       |O         |


  - **Example**

  ```
  $ cf space-users development
  ```


#### set-space-role

  - **Basic Syntax**


  ```
  $ cf set-space-role <USERNAME> <ORG> <SPACE> <ROLE>
  ```


  - **Description**


  ```
  사용자에게 조직의 스페이스에 role을 할당합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                         |O         |
  |ORG          |Organization Name                           |O         |
  |SPACE        |Space Name                       |O         |
  |ROLE         |역할  name  <br>  - SpaceManager: 스페이스의 관리자로 스페이스 내의 사용자 계정 관리 및 인스턴스 수, 서비스 바인딩 상태 및 스페이스 내의 리소스 상태를 조회 및 변경 <br> - SpaceDeveloper: 서비스 관리로 App 배포 <br> - SpaceAuditor: 서비스 관리로 App을 배포   |O         |

  - **Example**

  ```
  $ cf set-space-role cfuser cforg development OrgManager
  ```


#### unset-space-role

  - **Basic Syntax**


  ```
  $ cf unset-space-role <USERNAME> <ORG> <SPACE> <ROLE>
  ```


  - **Description**


  ```
  사용자에게 조직의 스페이스에 role을 회수합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                         |O         |
  |ORG          |Organization Name                           |O         |
  |SPACE        |Space Name                       |O         |
  |ROLE         |역할  name  <br>  - SpaceManager: 스페이스의 관리자로 스페이스 내의 사용자 계정 관리 및 인스턴스 수, 서비스 바인딩 상태 및 스페이스 내의 리소스 상태를 조회. <br> - SpaceDeveloper: 서비스 관리로 App 배포 <br> - SpaceAuditor: 스페이스 내의 서비스 바인딩, 인스턴스 수, app사용률등을 조회   |O         |

  - **Example**

  ```
  $ cf unset-space-role cfuser cforg development OrgManager
  ```

<div id='ID-ORG-ADMIN'/>
## ORG ADMIN


#### quotas

  - **Basic Syntax**


  ```
  $ cf quotas
  ```


  - **Description**


  ```
  Quota 목록을 조회합니다.
  ```


  - **Parameter **

    - none

  - **Example**

  ```
  $ cf quotas
  ```


#### quota

  - **Basic Syntax**


  ```
  $ cf quota <QUOTA>
  ```


  - **Description**


  ```
  Quota의 상세정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |QUOTA  name                         |O         |



  - **Example**

  ```
  $ cf quota cf-quota
  ```

#### set-quota

  - **Basic Syntax**


  ```
  $ cf set-quota <ORG> <QUOTA>
  ```


  - **Description**


  ```
  조직에게 QUOTA를 할당합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG          |직  name                            |O         |
  |QUOTA        |QUOTA  name                         |O         |


  - **Example**

  ```
  $ cf set-quota cf-quota
  ```


#### create-quota

  - **Basic Syntax**


  ```
  $ cf create-quota <QUOTA> [-m TOTAL_MEMORY] [-i INSTANCE_MEMORY] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans]
  ```


  - **Description**


  ```
  Quota정보를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 Service Instance수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |


  - **Example**

  ```
  $ cf create-quota cf-quota -m 500m -i 256m -r 2000 -s 500
  ```


#### delete-quota

  - **Basic Syntax**


  ```
  $ cf delete-quota <QUOTA> [-f]
  ```


  - **Description**


  ```
  Quota정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA        |QUOTA  name                                                  |O         |
  |-f           |삭제 확인메시지 없이 QUOTA 정보를 삭제                      |X         |


  - **Example**

  ```
  $ cf delete-quota cf-quota
  ```


#### update-quota

  - **Basic Syntax**


  ```
  $ cf update-quota <QUOTA> [-m TOTAL_MEMORY] [-i INSTANCE_MEMORY][-n NEW_NAME] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans | --disallow-paid-service-plans]
  ```


  - **Description**


  ```
  Quota정보를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-n NEW_NAME                 |QUOTA  name 변경시 변경할 이름                                      |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 Service Instance수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |
  |--disallow-paid-service-plans  |과금 서비스 plan 사용 불가                                       |X        |

  - **Example**

  ```
  $ cf update-quota cf-quota -m 500m -i 256m -r 2000 -s 500
  ```


#### shared-private-domain

  - **Basic Syntax**


  ```
  $ cf shared-private-domain <ORG> <DOMAIN>
  ```


  - **Description**


  ```
  private도메인을 다른 조직과 공유합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                 |QUOTA  name                        |O         |
  |DOMAIN                |도메인  name                        |O         |


  - **Example**

  ```
  $ cf shared-private-domain cf-org sharedomain.or.kr
  ```


#### unshared-private-domain

  - **Basic Syntax**


  ```
  $ cf unshared-private-domain <ORG> <DOMAIN>
  ```


  - **Description**


  ```
  다른 조직과 share한 도메인 정보를 unshare합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG                   |도메인  name                        |O         |
  |DOMAIN                |도메인  name                        |O         |


  - **Example**

  ```
  $ cf unshared-private-domain cf-org sharedomain.or.kr
  ```

<div id='ID-SPACE-ADMIN'/>
## SPACE ADMIN


#### space-quotas

  - **Basic Syntax**


  ```
  $ cf space-quotas
  ```


  - **Description**


  ```
  Space-quota정보 목록을 조회합니다.
  ```


  - **Parameter **


    - none


  - **Example**

  ```
  $ cf space-quotas
  ```



#### space-quota

  - **Basic Syntax**


  ```
  $ cf space-quota <SPACE_QUOTA_NAME>
  ```


  - **Description**


  ```
  Space quota 상세정보를 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_QUOTA_NAME       |스페이스 QUOTA  name       |O         |


  - **Example**

  ```
  $ cf space-quota cf-space-quota
  ```


#### create-space-quota

  - **Basic Syntax**


  ```
  $ cf create-space-quota <QUOTA> [-i INSTANCE_MEMORY] [-m MEMORY] [-r ROUTES] [-s SERVICE_INSTANCES] [--allow-paid-service-plans]
  ```


  - **Description**


  ```
  스페이스 Quota정보를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |메모리 할당량  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |최대 라우트 수                                                 |X         |
  |-s SERVICE_INSTANCES        |최대 Service Instance수                                         |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |


  - **Example**

  ```
  $ cf create-space-quota cf-space-quota -i 2G -m 10G -r 3000 -s 200
  ```


#### update-space-quota

  - **Basic Syntax**


  ```
      $ cf update-space-quota <SPACE-QUOTA-NAME> [-i MAX-INSTANCE-MEMORY] [-m MEMORY] [-n NEW_NAME] [-r ROUTES] [-s SERVICES] [--allow-paid-service-plans | --disallow-paid-service-plans]
  ```


  - **Description**


  ```
  스페이스 Quota정보를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME            |스페이스 QUOTA  name                                               |O         |
  |-i MAX-INSTANCE-MEMORY      |App instance가 가질수 있는 최대할당량 (-1은 무한대) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-m MEMORY                   |스페이스가 가질수 있는 최대 메모리                               |X         |
  |-n NEW_NAME                 |변경하려는 SPACE-QUOTA  name                                       |X         |
  |-r ROUTES                   |스페이스가 가지는 최대 route 갯수                               |X         |
  |-s SERVICES                 |스페이스가 가지는 최대 Service Instance갯수                       |X         |
  |--allow-paid-service-plans  |과금 서비스 plan 사용가능                                       |X        |
  |--disallow-paid-service-plans  |과금 서비스 plan 사용 불가                                   |X        |


  - **Example**

  ```
  $ cf update-space-quota cf-space-quota -i 2G -m 10G -r 3000 -s 200
  ```


#### delete-space-quota

  - **Basic Syntax**


  ```
  $ cf delete-space-quota <SPACE-QUOTA-NAME>
  ```


  - **Description**


  ```
  스페이스 Quota정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME     |스페이스 QUOTA  name                                     |O         |
  |-f           |삭제 확인메시지 없이 SPACE-QUOTA 정보를 삭제               |X         |



  - **Example**

  ```
  $ cf delete-space-quota cf-space-quota
  ```


#### set-space-quota

  - **Basic Syntax**


  ```
  $ cf set-space-quota <SPACE-NAME> <SPACE-QUOTA-NAME>
  ```


  - **Description**


  ```
  스페이스에 quota를 할당합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-NAME            |Space Name                    |O         |
  |SPACE-QUOTA-NAME      |스페이스 Quota  name              |O         |



  - **Example**

  ```
  $ cf set-space-quota development cf-space-quota
  ```


#### unset-space-quota

  - **Basic Syntax**


  ```
  $ cf unset-space-quota SPACE QUOTA
  ```


  - **Description**


  ```
  스페이스에 할당된 quota를 회수합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE        |Space Name                   |O         |
  |QUOTA        |스페이스 Quota  name             |O         |



  - **Example**

  ```
  $ cf unset-space-quota development cf-space-quota
  ```

<div id='ID-SERVICE-ADMIN'/>
## SERVICE ADMIN


#### service-auth-tokens

  - **Basic Syntax**


  ```
  $ cf service-auth-tokens
  ```


  - **Description**


  ```
  서비스 인증 토큰 목록을 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $ cf service-auth-token
  ```


#### create-service-auth-token

  - **Basic Syntax**


  ```
  $ cf create-service-auth-token <LABEL> <PROVIDER> <TOKEN>
  ```


  - **Description**


  ```
  스페이스에 할당된 quota를 회수합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |Service Provider                    |O         |
  |TOKEN        |토큰  name                          |O         |


  - **Example**

  ```
  $ cf create-service-auth-token token-label mysql token
  ```


#### update-service-auth-token

  - **Basic Syntax**


  ```
  $ cf update-service-auth-token <LABEL> <PROVIDER> <TOKEN>
  ```


  - **Description**


  ```
  Service auth token 정보를 수정합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |Service Provider                    |O         |
  |TOKEN        |토큰  name                          |O         |


  - **Example**

  ```
  $ cf update-service-auth-token token-label mysql token
  ```


#### delete-service-auth-token

  - **Basic Syntax**


  ```
  $ cf delete-service-auth-token <LABEL> <PROVIDER> [-f]
  ```


  - **Description**


  ```
  Service auth token 정보를 삭제합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |서비스 토큰 라벨                 |O         |
  |PROVIDER     |Service Provider                    |O         |
  |-f           |삭제 확인메시지 없이 SERVICE TOKEN 정보를 삭제      |X         |


  - **Example**

  ```
  $ cf delete-service-auth-token token-label mysql
  ```


#### service-brokers

  - **Basic Syntax**


  ```
  $ cf delete-service-auth-token <LABEL> <PROVIDER> [-f]
  ```


  - **Description**


  ```
  Service Broker정보 목록을 조회합니다.
  ```



  - **Parameter **

    - none


  - **Example**

  ```
  $ cf service-brokers
  ```


#### create-service-broker

  - **Basic Syntax**


  ```
  $ cf create-service-broker <SERVICE_BROKER> <USERNAME> <PASSWORD> <URL>
  ```


  - **Description**


  ```
  Service Broker정보를 등록합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |Service Broker Name            |O         |
  |USERNAME                   |UserName                   |O         |
  |PASSWORD                   |패스워드                   |O         |
  |URL                        |서비스 브로커 URL           |O         |



  - **Example**

  ```
  $ cf create-service-broker mysql-service-broker admin password http://p-mysql.10.244.0.34.xip.io
  ```


#### update-service-broker

  - **Basic Syntax**


  ```
  $ cf update-service-broker <SERVICE_BROKER> <USERNAME> <PASSWORD> <URL>
  ```


  - **Description**


  ```
  Service Broker정보를 등록합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |Service Broker Name            |O         |
  |USERNAME                   |UserName                   |O         |
  |PASSWORD                   |패스워드                   |O         |
  |URL                        |서비스 브로커 URL           |O         |



  - **Example**

  ```
  $ cf update-service-broker mysql-service-broker admin password http://p-mysql.10.244.0.34.xip.io
  ```


#### delete-service-broker

  - **Basic Syntax**


  ```
  $ cf delete-service-broker <SERVICE_BROKER> [-f]
  ```


  - **Description**


  ```
  Service Broker정보를 삭제합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER    |Service Broker Name                                         |O         |
  |-f                |삭제 확인메시지 없이 SERVICE BROKER 정보를 삭제       |X         |



  - **Example**

  ```
  $ cf delete-service-broker mysql-service-broker
  ```


#### rename-service-broker

  - **Basic Syntax**


  ```
  $ cf rename-service-broker <SERVICE_BROKER> <NEW_SERVICE_BROKER>
  ```


  - **Description**


  ```
  Service Broker  name을 수정합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER     |Service Broker Name            |O         |
  |NEW_SERVICE_BROKER |변경할 Service Broker Name      |O         |



  - **Example**

  ```
  $ cf rename-service-broker mysql-service-broker new_mysql-service-broker
  ```


#### migrate-service-broker

  - **Basic Syntax**


  ```
  $ cf migrate-service-instances <v1_SERVICE> <v1_PROVIDER> <v1_PLAN> <v2_SERVICE> <v2_PLAN>
  ```


  - **Description**


  ```
  서비스 인스턴스에서 사용하는 서비스 및 플랜을 다른 플랜으로 변경합니다. <br> - App이 사용하는 서비스를 다른 서비스로 변경하려 할때 사용합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |v1_SERVICE     |기존 Service Name                         |O         |
  |v1_PROVIDER    |기존 서비스를 제공하는 제공자            |O         |
  |v1_PLAN        |기존 서비스 인스턴스에서 사용하는 플랜    |O         |
  |v2_SERVICE     |신규 Service Name                         |O         |
  |v2_PLAN        |신규 서비스에서 사용하는 플랜            |O         |  


  - **Example**

  ```
  $ cf migrate-service-instances p-mysql mysql-provider silver  postgres silver
  ```


#### purge-service-offering

  - **Basic Syntax**


  ```
  $ cf purge-service-offering <SERVICE> [-p PROVIDER]
  ```


  - **Description**


  ```
  cf와 서비스 브로커간의 정보 불일치를 해결할때 사용합니다. <br>   (migrate-service-instances   name령 이후 사용)
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |Service Name                                  |O         |
  |-p PROVIDER  |Service Provider                               |O         |
  |-f           |Delete service information without confirmation message  |O         |



  - **Example**

  ```
  $ cf purge-service-offering mysql
  ```


#### service-access

  - **Basic Syntax**


  ```
  $ cf service-access
  ```


  - **Description**


  ```
  서비스 access 될 서비스 목록 조회합니다..
  ```



  - **Parameter **


     - none


  - **Example**

  ```
  $ cf service-access
  ```


#### enable-service-access

  - **Basic Syntax**


  ```
  $ cf enable-service-access <SERVICE> [-p PLAN] [-o ORG]
  ```


  - **Description**


  ```
  조직 또는 서비스 plan을 서비스에 접근 가능하도록 설정합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |Service Name                        |O          |
  |-p PLAN      |PLAN  name                          |O          |
  |-o ORG       |Organization Name                           |O          |



  - **Example**

  ```
  $ cf enable-service-access mysql -p silver -o cf-org
  ```


#### disable-service-access

  - **Basic Syntax**


  ```
  $ cf disable-service-access <SERVICE> [-p PLAN] [-o ORG]
  ```


  - **Description**


  ```
  조직 또는 서비스 plan을 서비스에 접근 불가 하도록 설정합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |Service Name                        |O          |
  |-p PLAN      |PLAN  name                          |O          |
  |-o ORG       |Organization Name                           |O          |



  - **Example**

  ```
  $ cf disable-service-access mysql -p silver -o cf-org
  ```

<div id='ID-SECURITY-GROUP'/>
## SECURITY GROUP


#### security-group

  - **Basic Syntax**


  ```
  $ cf security-group <SECURITY_GROUP>
  ```


  - **Description**


  ```
  시큐리티 그룹 상세정보를 조회합니다.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP      |Security Group Name                        |O          |



  - **Example**

  ```
  $ cf security-group cf-security-group
  ```


#### security-groups

  - **Basic Syntax**


  ```
  $ cf security-groups
  ```


  - **Description**


  ```
  시큐리티 그룹 목록을 조회합니다.
  ```


  - **Parameter **


    - none



  - **Example**

  ```
  $ cf security-groups
  ```


#### create-security-group

  - **Basic Syntax**


  ```
  $ cf create-security-group <SECURITY_GROUP> <PATH_TO_JSON_RULES_FILE>
  ```


  - **Description**


  ```
  시큐리티 그룹정보를 생성합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP      |Security Group Name                                           |O          |
  |PATH_TO_JSON_RULES_FILE      |시큐리티 룰을   name세한 JSON 파일의 path 및 file name<br> ex) rule file작성 예제 <br> [ <br> &nbsp;&nbsp;{   <br> &nbsp;&nbsp;&nbsp;&nbsp;"protocol": "tcp",     <br> &nbsp;&nbsp;&nbsp;&nbsp;"destination": "10.244.1.18", <br>     &nbsp;&nbsp;&nbsp;&nbsp;"ports": "3306" <br>&nbsp;&nbsp;} <br> ]     |O          |


  - **Example**

  ```
  $ cf create-security-group cf-security-group ./rule.json
  ```


#### update-security-group

  - **Basic Syntax**


  ```
  $ cf update-security-group <SECURITY_GROUP> <PATH_TO_JSON_RULES_FILE>
  ```


  - **Description**


  ```
  시큐리티 그룹정보를 수정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP      |Security Group Name                                           |O          |
  |PATH_TO_JSON_RULES_FILE      |시큐리티 룰을   name세한 JSON 파일의 Spath 및 file name<br> ex) rule file작성 예제 <br> [ <br> &nbsp;&nbsp;{   <br> &nbsp;&nbsp;&nbsp;&nbsp;"protocol": "tcp",     <br> &nbsp;&nbsp;&nbsp;&nbsp;"destination": "10.244.1.18", <br>     &nbsp;&nbsp;&nbsp;&nbsp;"ports": "3306" <br>&nbsp;&nbsp;} <br> ]     |O          |

  - **Example**

  ```
  $ cf update-security-group cf-security-group ./rule.json
  ```


#### delete-security-group

  - **Basic Syntax**


  ```
  $ cf delete-security-group <SECURITY_GROUP> [-f]
  ```


  - **Description**


  ```
  시큐리티 그룹정보를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                                    |O          |
  |-f              |삭제 확인메시지 없이 시큐리지 그룹 정보를 삭제합니다.    |X          |


  - **Example**

  ```
  $ cf update-security-group cf-security-group ./rule.json
  ```


#### bind-security-group

  - **Basic Syntax**


  ```
  $ cf bind-security-group <SECURITY_GROUP> <ORG> <SPACE>
  ```


  - **Description**


  ```
  시큐리티 그룹 정보와 스페이스를 바인드 합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                |O          |
  |ORG             |Organization Name                        |O          |
  |SPACE           |Space Name                    |O         |


  - **Example**

  ```
  $ cf update-security-group cf-security-group ./rule.json
  ```


#### unbind-security-group

  - **Basic Syntax**


  ```
  $ cf unbind-security-group <SECURITY_GROUP> <ORG> <SPACE>
  ```


  - **Description**


  ```
  시큐리티 그룹 정보와 스페이스를 언바인드 합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                |O          |
  |ORG             |Organization Name                        |O          |
  |SPACE           |Space Name                     |O          |


  - **Example**

  ```
  $ cf unbind-security-group cf-security-group cf-group development
  ```


#### bind-staging-security-group

  - **Basic Syntax**


  ```
  $ cf bind-staging-security-group <SECURITY_GROUP>
  ```


  - **Description**


  ```
  App staging처리를 하기 위해 시큐리티 그룹을 설정합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                |O          |


  - **Example**

  ```
  $ cf bind-staging-security-group cf-security-group
  ```


#### staging-security-groups

  - **Basic Syntax**


  ```
  $ cf staging-security-groups
  ```


  - **Description**


  ```
  Staging security group 정보 목록을 조회합니다.
  ```


  - **Parameter **

    - none


  - **Example**

  ```
  $ cf staging-security-groups
  ```


#### unbind-staging-security-group

  - **Basic Syntax**


  ```
  $ cf unbind-staging-security-group <SECURITY_GROUP>
  ```


  - **Description**


  ```
  App staging처리를 하기 위한 시큐리티 그룹을 설정을 해제 합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                |O          |


  - **Example**

  ```
  $ cf unbind-staging-security-group cf-security-group
  ```


#### running-security-groups

  - **Basic Syntax**


  ```
  $ cf unbind-staging-security-group <SECURITY_GROUP>
  ```


  - **Description**


  ```
  실행중인 시큐리트 그룹 목록을 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $ cf unbind-staging-security-group cf-security-group
  ```


<div id='ID-ENVIRONMENT-VARIABLE-GROUPS'/>
## ENVIRONMENT VARIABLE GROUPS

<div id='running-environment-variable-group-revg'/>
#### running-environment-variable-group, revg

  - **Basic Syntax**


  ```
  $ cf running-environment-variable-group
  ```


  - **Description**


  ```
  실environment variables 내용을 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $ cf running-environment-variable-group
  ```

<div id='staging-environment-variable-group-sevg'/>
#### staging-environment-variable-group, sevg

  - **Basic Syntax**


  ```
  $ cf staging-environment-variable-group
  ```


  - **Description**


  ```
  스테이징시 사용되는 environment variables 내용을 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $ cf staging-environment-variable-group
  ```



<div id='set-staging-environment-variable-group-ssevg'/>
#### set-staging-environment-variable-group, ssevg

  - **Basic Syntax**


  ```
  $ cf set-staging-environment-variable-group <ENV_VARIABLE>
  ```


  - **Description**


  ```
  스테이징시 사용되는 environment variables 내용을 설정한다
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ENV_VARIABLE  |environment variables 내용으로 KEY/VALUE로 구성              |O          |



  - **Example**

  ```
  $ cf set-staging-environment-variable-group '{"name":"value","name":"value"}'
  ```

<div id='set-running-environment-variable-group-ssevg'/>
#### set-running-environment-variable-group, ssevg

  - **Basic Syntax**


  ```
  $ cf set-running-environment-variable-group <ENV_VARIABLE>
  ```


  - **Description**


  ```
  environment variables 내용을 설정 합니다.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ENV_VARIABLE  |environment variables 내용으로 KEY/VALUE로 구성된다.                |O          |



  - **Example**

  ```
  $ cf set-running-environment-variable-group '{"name":"value","name":"value"}'
  ```

<div id='ID-FEATURE-FLAGS'/>
## FEATURE FLAGS

#### feature-flags

  - **Basic Syntax**


  ```
  $ cf feature-flags
  ```


  - **Description**


  ```
  feature flags 목록을 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $ cf feature-flags
  ```


#### feature-flag

  - **Basic Syntax**


  ```
  $ cf feature-flag <FEATURE_NAME>
  ```


  - **Description**


  ```
  특정 Feature flag의 상태를 조회합니다.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag   name. <br> - feature flag에는 6가지가 있다. <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



  - **Example**

  ```
  $ cf feature-flag app_bits_upload
  ```


#### enable-feature-flag

  - **Basic Syntax**


  ```
  $ cf enable-feature-flag <FEATURE_NAME>
  ```


  - **Description**


  ```
  특정 Feature flag의 상태를 enable로 변경합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag   name. <br> - feature flag에는 6가지가 있다. <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



  - **Example**

  ```
  $ cf enable-feature-flag app_bits_upload
  ```


#### disable-feature-flag

  - **Basic Syntax**


  ```
  $ cf disable-feature-flag <FEATURE_NAME>
  ```


  - **Description**


  ```
  특정 Feature flag의 상태를 disable로 변경합니다.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag   name. <br> - feature flag에는 6가지가 있다. <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



  - **Example**

  ```
  $ cf disable-feature-flag app_bits_upload
  ```

<div id='ID-ADVANCE'/>
## ADVANCE


#### curl

  - **Basic Syntax**


  ```
  $ cf curl <PATH> [-i] [-v] [-X METHOD] [-H HEADER] [-d DATA] [--output FILE]
  ```


  - **Description**


  ```
  OpenPaaS CLI  name령어가 아닌 OpenPaaS API를 호출합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |PATH         |Cf api path <br>  Ex) /v2/spaces/2d94e7ee-9805-408d-a1eb-ceac319e603b/summary             |O          |
  |-i           |Response header포함한 결과                                                                  |X          |
  |-v           |Request/response에 CF_TRACE enable된 내용 포함                                              |X          |
  |-X METHOD    |HTTP method((GET,POST,PUT,DELETE,etc)                                                      |X          |
  |-H HEADER    |Request에 Custom Header를 포함합니다.                                                         |X          |
  |-d DATA      |Request에 Http data를 포함합니다.                                                             |X          |
  |--output FILE |Response결과를 stdout대신 FILE로 결과 저장                                                  |X          |


  - **Example**

  ```
  $ cf curl /v2/spaces/2d94e7ee-9805-408d-a1eb-ceac319e603b/summar
  ```


#### config

  - **Basic Syntax**


  ```
  $ cf config [--async-timeout TIMEOUT_IN_MINUTES] [--trace true | false | path/to/file] [--color true | false] [--locale (LOCALE | CLEAR)]
  ```


  - **Description**


  ```
  CF CLI에 대한 설정.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |--async-timeout TIMEOUT_IN_MINUTES        |CLI   name령 전송시 async timeout 설정                     |X          |
  |--trace (true / false / path/to/file   )    |CLI   name령 수행시 실행되는 cf api의 내용 출력 설정         |X          |
  |--color true / false                      |CLI   name령 수행시 실행되는 cf api의 내용 color 설정        |X          |
  |--locale (LOCALE / CLEAR)                 |CLI   name령 수행시 실행되는 cf api의 내용 locale 설정       |X          |


  - **Example**

  ```
  $ cf curl /v2/spaces/2d94e7ee-9805-408d-a1eb-ceac319e603b/summar
  ```

#### oauth-token

  - **Basic Syntax**


  ```
  $ cf oauth-token
  ```


  - **Description**


  ```
  사용자가 cf login후 CF에서 받은 token 값 조회합니다.
  ```


  - **Parameter **

    - none



  - **Example**

  ```
  $cf oauth-token
  ```

<div id='ID-ADD-REMOVE-PLUGIN-REPOSITORY'/>
## ADD/REMOVE PLUGIN REPOSITORY


#### add-plugin-repo

  - **Basic Syntax**


  ```
  $ cf add-plugin-repo <REPO_NAME> <URL>
  ```


  - **Description**


  ```
  OpenPaaS CLI plugin repository(저장소)를 추가합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |REPO_NAME    |Repository   name                   |X          |
  |URL          |Repository URL                 |X          |



  - **Example**

  ```
  cf add-plugin-repo Diego-SSH http://plugins.cloudfoundry.org
  ```


#### remove-plugin-repo

  - **Basic Syntax**


  ```
  $ cf remove-plugin-repo <REPO_NAME> <URL>
  ```


  - **Description**


  ```
  CLI plugin repository(저장소)를 삭제합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |REPO_NAME    |Repository   name                   |O          |
  |URL          |Repository URL                 |O         |



  - **Example**

  ```
  cf remove-plugin-repo Diego-SSH http://plugins.cloudfoundry.org
  ```


#### list-plugin-repos

  - **Basic Syntax**


  ```
  $ cf list-plugin-repos
  ```


  - **Description**


  ```
  CLI에 추가된 plugin repository(저장소)목록을 조회합니다.
  ```


  - **Parameter **

    - none


  - **Example**

  ```
  $cf list-plugin-repos
  ```


#### repo-plugins

  - **Basic Syntax**


  ```
  $ cf repo-plugins [-r REPO_NAME]
  ```


  - **Description**


  ```
  Repository에 있는 플러그인 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |REPO_NAME    |Repository   name                   |X          |



  - **Example**

  ```
  $ cf repo-plugins
  ```

<div id='ID-ADD-REMOVE-PLUGIN'/>
## ADD/REMOVE PLUGIN


#### plugins

  - **Basic Syntax**


  ```
  $ cf plugins
  ```


  - **Description**


  ```
  추가된 plugin의 사용가능한   name령어 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |REPO_NAME    |Repository   name                   |X          |



  - **Example**

  ```
  $ cf repo-plugins
  ```



#### install-plugin

  - **Basic Syntax**


  ```
  $ cf install-plugin < URL or LOCAL-PATH/TO/PLUGIN> [-r REPO_NAME]
  ```


  - **Description**


  ```
  추가된 plugin의 사용가능한   name령어 목록을 조회합니다.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |URL or LOCAL-PATH/TO/PLUGIN   |Plugin URL 또는 로컬path 또는 repository에 있는 플러그인  name                |X          |
  |-r REPO_NAME                  |Plugin repository  name                                                   |X          |


  - **Example**

  ```
  $cf install-plugin 'Usage Report' -r CF-Community
  ```


----------
