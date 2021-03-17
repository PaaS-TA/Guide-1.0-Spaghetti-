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
|-a API_URL    |OpenPaaS that the CLI attempts to access. URL<br>Ex) https://api.10.244.0.34.xip.io    |X        |
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
  Inquires the state of the app.
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
  |ENV_VAR_NAME |environment variables Key of App              |O        |
  |ENV_VAR_VALUE|environment variables Value of App              |O        |


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
  Delete the environment variables set in the App. (restage required when applied)
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
  Check the stack list (operating system file system) of OpenPaaS.
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
  Copy the source of the app to another app. If the file is not overwritten, it will restart automatically.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |Orginall APP  name                        |O        |
  |TARGET-APP   |The app name to which the source will be copied      |X        |
  |-o TARGET-ORG |Target Organization              |O        |
  |-s TARGET-SPACE|Target Space     |X        |
  |--no-restart   |Do not restart after source copy |X        |

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
  Create a manifest file of the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SOURCE-APP   |Orginall APP  name                        |O        |
  |-p /path/<app-name>.yml   |The location where the file will be created and the file name (-p will be auto-generated if not used)     |X        |

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
  Check the list of services provided by the cf marketplace.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |The service plan is checked  |X        |


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
  Check the list of Service Instances in the target space.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-s SERVICE_NAME   |The service plan is checked   |X        |


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
  Check the information of the service instance
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE   |Service Instance name           |O        |
  |--guid             |Check the Service Instance Guide |X        |

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
  Create a service instance with a service provided by Marketplace.
  ```

  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE      |Service name in Marketplace                                       |O        |
  |PLAN         |Service plan  name                                                           |O        |
  |SERVICE_INSTANCE   |Service Instance name to be created                                      |O        |
  |-c PARAMETERS_AS_JSON |Input service settings information in json type <br> Ex) -c '{"ram_gb":4}'    |X        |
  |-t TAGS      |Service Instance tag                                                   |X        |

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
  Modify the service instance.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE        |Service Instance name                                               |O        |
  |-p NEW_PLAN             |Service Plan  Name                                                  |O        |
  |-c PARAMETERS_AS_JSON   |Input service settings information in json type <br> Ex) -c '{"ram_gb":4}'    |O        |
  |-t TAGS                 |Service Instance Tag                                            |X        |

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
  Delete the service instance.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                                        |O        |
  |-f                |Delete the Service Instance without confirmation message.  |X        |

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
  Modify the Service Instance name.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |Service Instance name                       |O        |
  |NEW_SERVICE_INSTANCE   |Service Instance name you want to change     |O        |

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
  Create a key of the service instance.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE       |Service Instance name                       |O        |
  |SERVICE_KEY            |Service Instancekey  name                   |O        |
  |-c PARAMETERS_AS_JSON  |Service Instance setting(JSON Parameter)    |X        |


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
  Check the list of keys of the service instance.
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
  Check detailed information about the key of the service instance.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                       |O        |
  |SERVICE_KEY       |Service Instancekey  name                   |O        |
  |--guid            |Check the Service Instanceguid       |X        |



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
  Delete Service key.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_INSTANCE  |Service Instance name                       |O        |
  |SERVICE_KEY       |Service Instancekey  name                   |O        |
  |--guid            |Check Service Instanceguid         |X        |



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
 Bind the app and service instance.<br>-The service can be used in the app only when the service instance and the APP are bound.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |APP  name                                            |O        |
  |SERVICE_INSTANCE  |Service Instance  name                           |O        |
  |-c PARAMETERS_AS_JSON   |Binding Settings Parameter(json type)         |X        |



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
  Unbind the app and service instances.
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
  Instead of using the service provided by the market place, the user configures a separate service and binds it with the APP.
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
  Modify user-provided service instance information.
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
  Check the organization information list...
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
  Check the organization deatails
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                         |O        |
  |--guid       |Check the guid of organization        |X        |


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
  Create organization information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                         |O        |
  |-q QUOTA_NAME |Quota to allocate to the organization  |X        |


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
  Check the organization information list.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME      |Organization Name                          |O        |
  |-f           |Organization information is deleted without confirmation message.|X        |


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
  Change the Organization Name.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME       |Organization Name                          |O        |
  |NEW_ORG_NAME   |Organization Name to be created                    |O        |


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
  Bring a list of spaces.
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
  Check space details.
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
  Create space information
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |-o ORG_NAME  |Organization Name to be mapped to the space  |X         |
  |-q SPACE-QUOTA-NAME    |QUOTA name to be allocated to the space|X         |

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
  Delete the Space informaiton.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |-f           |Delete the space without a confirmation message.|X         |

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
  Change the space name.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME     |Space Name                           |O         |
  |NEW_SPACE_NAME |Delete the space without a confirmation message.|O         |

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
  Check the domain information list.
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
  Create a list of domain information. The created domain can be used in the established organization.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG_NAME     |Organization Name                           |O         |
  |DOMAIN       |Domain  name                          |O         |


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
  Delete domain information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |Domain  name                           |O         |
  |-f           |Deletes the domain without a confirmation message.|X         |


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
  Create shared domain information
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
  Delete shared domain information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |Organization Name                           |O         |
  |-f           |Delete the domain without a confirmation message.  |X         |

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
  Check the route information list that exists in the current organization/space.
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
  Delete shared domain information...
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |DOMAIN       |Delete the shared domain without a confirmation message. <br>   - Domain information must be inputted.   |O         |
  |-n HOSTNAME  |Host Name                          |X         |


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
  Delete shared domain information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_NAME   |Space Name                           |O         |
  |DOMAIN       |Delete the shared domain without a confirmation message. <br>   - Domain information must be inputted. |O         |
  |-n HOSTNAME  |Host name                          |X         |


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
  Check if route information exists.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |HOST         |Host name                                    |O         |
  |DOMAIN       |Delete the shared domain without a confirmation message.|O         |


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
  Allocate URL route information to the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App  name                           |O         |
  |DOMAIN       |Domain to allocate to App             |O         |
  |-n HOSTNAME  |Host to allocate to App             |X         |


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
  Delete the URL route information to the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |APP_NAME     |App  name                           |O         |
  |DOMAIN       |Domain to allocate to App              |O         |
  |-n HOSTNAME  |Host to allocate to App              |X         |


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
  Delete URL route information to APP.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |DOMAIN       |Domain to allocate to App              |O         |
  |-n HOSTNAME  |Host to allocate to App              |X         |
  |-f           |Delete route information without confirmation message. |X         |


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
  Delete all route information that is not mapped to the app.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |-f           |Delete route information without confirmation message.|X         |


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
  Check the list of build packs.
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
  Create a build pack.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |buildpack name                           |O         |
  |-p PATH       |buildpack path                      |O         |
  |-i POSITIONE  |Buildpack check order during buildpack auto-detection<br> ex)1.2.3              |O         |
  |--enable      |Used when staging                 |X         |
  |--disable     |Not used when staging            |X         |



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
  Modify buildpack imformation
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |buildpack  name                           |O         |
  |-p PATH       |buildpack path                      |O         |
  |-i POSITIONE  |Buildpack check order during buildpack auto-detection <br> ex)1.2.3              |O         |
  |--enable      |Used when staging                    |X         |
  |--disable     |Not used when staging                   |X         |



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
  Delete buildpack
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |BUILDPACK     |buildpack  name                           |O         |
  |-f           |Delete buildpack information without confirmation message|X         |


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
  Create a new user account.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |User ID                        |O         |
  |PASSWORD     |Password                         |O         |


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
  Delete a new user account.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |User ID                       |O         |
  |-f           |Delete user information without confirmation message |X         |


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
  Check the users belonging to the organization.
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
  Set the role of a specific organization to the user.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                          |O         |
  |ORG          |Organization Name                          |O         |
  |ROLE        |Role  name <br>  - OrgManager : User management and plan setting/change authority<br> - BillingManager : Create and manage billing account and billing information <br>  - OrgAuditor : Check organization quota capacity and user role        |O         |



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
  Unset the role of a specific organization to the user..
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                          |O         |
  |ORG          |Organization Name                          |O         |
  |ROLE        |Role  name <br>  - OrgManager :User management and plan setting/change authority<br> - BillingManager : Create and manage billing account and billing information <br>  - OrgAuditor : Check organization quota capacity and user role  |O         |



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
  Check User list allocated to space of Organization. 
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
  Allocate roles to users for organizational space.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                         |O         |
  |ORG          |Organization Name                           |O         |
  |SPACE        |Space Name                       |O         |
  |ROLE         |Role  name  <br>  - SpaceManager: As an administrator of the space, you can manage user accounts in the space. also check and change the number of instances, service binding state, and resource state in the space.<br> - SpaceDeveloper: Deploy the app as service management. <br> - SpaceAuditor: Deploy the app as service management.   |O         |

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
  Recalls the role from the user in the organizational  space.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |USERNAME     |UserName                         |O         |
  |ORG          |Organization Name                           |O         |
  |SPACE        |Space Name                       |O         |
  |ROLE         |Role name  <br>  - SpaceManager: As an administrator of the space, you can manage user accounts in the space. also check and change the number of instances, service binding state, and resource state in the space. <br> - SpaceDeveloper: Deploy the app as service management. <br> - SpaceAuditor: Check the service binding, number of instances, app use rate, etc in space. |O         |

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
  Check the Quota list.
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
  Check detailed information of Quota.
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
  Allocate QUOTA to the organization.
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
  Create Quota information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |Memory quota<br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |Maximum quota that an app instance can have (-1 is infinite)<br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |Maximum number of routes                                      |X         |
  |-s SERVICE_INSTANCES        |Maximum number of Service Instances                |X         |
  |--allow-paid-service-plans  |Enable billing service plan              |X        |


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
  Delete Quota Information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA        |QUOTA  name                                                  |O         |
  |-f           |Delete QUOTA information without confirmation message    |X         |


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
  Modify the Quota information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |Memory quota<br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |Maximum quota that an app instance can have (-1 is infinite)<br>  Ex) 1024M, 1G, 10G                        |X         |
  |-n NEW_NAME                 |The name to be changed when changing the QUOTA name              |X         |
  |-r ROUTES                   |Maximum number of routes                      |X         |
  |-s SERVICE_INSTANCES        |Maximum number of Service Instances                                  |X         |
  |--allow-paid-service-plans  |Enable billing service plan                                 |X        |
  |--disallow-paid-service-plans  |Disable billing service plan                             |X        |

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
  Share your privateDomain with other organizations.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                 |QUOTA  name                        |O         |
  |DOMAIN                |Domain  name                        |O         |


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
  Unshare domain information shared with other organizations.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ORG                   |Domain name                        |O         |
  |DOMAIN                |Domain name                        |O         |


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
  Check the list of space-quota information.
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
  Check space quota details.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE_QUOTA_NAME       |Space QUOTA  name       |O         |


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
  Create space quota information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |QUOTA                       |QUOTA  name                                                       |O         |
  |-m TOTAL_MEMORY             |Memory quota  <br> Ex) 1024M, 1G, 10G                         |X         |
  |-i INSTANCE_MEMORY          |Maximum quota that an app instance can have (-1 is infinite)<br>  Ex) 1024M, 1G, 10G                        |X         |
  |-r ROUTES                   |Maximum number of routes                  |X         |
  |-s SERVICE_INSTANCES        |Maximum number of Service Instances                                 |X         |
  |--allow-paid-service-plans  |Enable billing service plan                       |X        |


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
  Modify the space quota information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME            |Space QUOTA  name                                               |O         |
  |-i MAX-INSTANCE-MEMORY      |Maximum quota that an app instance can have (-1 is infinite) <br>  Ex) 1024M, 1G, 10G                        |X         |
  |-m MEMORY                   |Maximum memory a space can have                  |X         |
  |-n NEW_NAME                 |SPACE-QUOTA name you want to change                     |X         |
  |-r ROUTES                   |Maximum number of routes a space has                           |X         |
  |-s SERVICES                 |The maximum number of Service Instances a space has            |X         |
  |--allow-paid-service-plans  |Enable billing service plan                                     |X        |
  |--disallow-paid-service-plans  |Disable billing service plan                     |X        |


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
  Delete Space Quota Information
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-QUOTA-NAME     |Space QUOTA  name                                     |O         |
  |-f           |Delete SPACE-QUOTA information without confirmation message  |X         |



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
  Allocate quota to space.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE-NAME            |Space Name                    |O         |
  |SPACE-QUOTA-NAME      |Space Quota  name              |O         |



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
  Recall the quota allocated to the space.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SPACE        |Space Name                   |O         |
  |QUOTA        |Space Quota  name             |O         |



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
  Check a list of service authentication tokens.
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
  Recall the quota allocated to the space.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |Service token label       |O         |
  |PROVIDER     |Service Provider                    |O         |
  |TOKEN        |token name                          |O         |


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
  Modify the service auth token information.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |Service Token Label                 |O         |
  |PROVIDER     |Service Provider                    |O         |
  |TOKEN        |Token  name                          |O         |


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
  Delete Service auth token information
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |LABEL        |Service Token Label                 |O         |
  |PROVIDER     |Service Provider                    |O         |
  |-f           |Delete SERVICE TOKEN information without confirmation message |X         |


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
  Check the Service Broker information list.
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
  Register for Service Broker information.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |Service Broker Name            |O         |
  |USERNAME                   |UserName                   |O         |
  |PASSWORD                   |Password                  |O         |
  |URL                        |Service Broker URL           |O         |



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
  Register for Service Broker information.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKERABEL        |Service Broker Name            |O         |
  |USERNAME                   |UserName                   |O         |
  |PASSWORD                   |Password                  |O         |
  |URL                        |Service Broker URL           |O         |



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
  Delete Service Broker information
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER    |Service Broker Name                                         |O         |
  |-f                |Delete SERVICE BROKER information without confirmation message  |X         |



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
  Modify Service Broker Name
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SERVICE_BROKER     |Service Broker Name            |O         |
  |NEW_SERVICE_BROKER |Service Broker Name to change     |O         |



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
  Change the service and plan used by the Service instance to another plan. <br>-It is used to change the service used by the app to another service.
  ```



  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |v1_SERVICE     |Existing Service Name                         |O         |
  |v1_PROVIDER    |Provider that provides Existing Service  |O         |
  |v1_PLAN        |Plan used by Existing Service instance   |O         |
  |v2_SERVICE     |New Service Name                         |O         |
  |v2_PLAN        |Plan used by New Service           |O         |  


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
  It is used to resolve information inconsistency between cf and Service Broker.<br>   (Used after the migrate-service-instances command.)
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
  Check the list of services to be accessed to service..
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
  Make your organization or service plan accessible to the Service.
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
  Make the organization or service plan inaccessible to the service.
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
  Check security group detailed information.
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
  Check the list of security groups.
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
  Create the Security Group Information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP      |Security Group Name                                           |O          |
  |PATH_TO_JSON_RULES_FILE      |The path and file name of the JSON file specifying the security rules<br> ex) rule file example <br> [ <br> &nbsp;&nbsp;{   <br> &nbsp;&nbsp;&nbsp;&nbsp;"protocol": "tcp",     <br> &nbsp;&nbsp;&nbsp;&nbsp;"destination": "10.244.1.18", <br>     &nbsp;&nbsp;&nbsp;&nbsp;"ports": "3306" <br>&nbsp;&nbsp;} <br> ]     |O          |


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
  Modify the security group information.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP      |Security Group Name                                           |O          |
  |PATH_TO_JSON_RULES_FILE      |The path and file name of the JSON file specifying the security rules<br> ex) rule file example<br> [ <br> &nbsp;&nbsp;{   <br> &nbsp;&nbsp;&nbsp;&nbsp;"protocol": "tcp",     <br> &nbsp;&nbsp;&nbsp;&nbsp;"destination": "10.244.1.18", <br>     &nbsp;&nbsp;&nbsp;&nbsp;"ports": "3306" <br>&nbsp;&nbsp;} <br> ]     |O          |

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
  Delete the security group information
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |SECURITY_GROUP  |Security Group Name                                    |O          |
  |-f              |Deletes security group information without delete confirmation message.  |X          |


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
  Bind security group information and space.
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
  Unbind security group information and space.
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
  Set up a security group for app staging processing.
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
  Check the list of staging security group information.
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
  Unset the security group for app staging processing.
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
  Check the list of running security groups.
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
  Check the contents of environment variables.
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
  Check the contents of environment variables that are used when staging.
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
  Set the contents of environment variables that are used when staging.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ENV_VARIABLE  |Consists of KEY/VALUE as contents of environment variables              |O          |



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
  Set the contents of environment variables.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |ENV_VARIABLE  |It is consist of KEY/VALUE as contents of environment variables.            |O          |



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
  Check a list of feature flags.
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
  Check the state of a specific feature flag.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag  name. <br> - There are six ways in feature flags. <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



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
  Change the state of a specific feature flag to enable.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag   name. <br> - There are six ways in feature flags. <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



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
  Change the state of a specific feature flag to disable.
  ```


  - **Parameter **



  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |FEATURE_NAME  |Feature flag   name. <br> - There are six ways in feature flags.  <br> 1)user_org_creation <br> 2) private_domain_creation <br> 3) app_bits_upload <br> 4) app_scaling <br>  5) route_creation <br> 6) service_instance_creation               |O          |



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
  Call the OpenPaaS API, not the OpenPaaS CLI command.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |PATH         |Cf api path <br>  Ex) /v2/spaces/2d94e7ee-9805-408d-a1eb-ceac319e603b/summary             |O          |
  |-i           |Response header included result.                                                                |X          |
  |-v           |Including the contents of CF_TRACE enabled in the request/response.                                     |X          |
  |-X METHOD    |HTTP method((GET,POST,PUT,DELETE,etc)                                                      |X          |
  |-H HEADER    |Include Custom Header in Request.                                                   |X          |
  |-d DATA      |Include Http data in Request.                                                       |X          |
  |--output FILE |Save response result as FILE instead of stdout                                         |X          |


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
  Set up the CF CLI.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |--async-timeout TIMEOUT_IN_MINUTES        |Set async timeout when sending CLI command                |X          |
  |--trace (true / false / path/to/file   )    |Setting to output the contents of the cf api executed when executing the CLI command |X          |
  |--color true / false                      |Setting the color of the contents of the cf api executed when executing the CLI command    |X          |
  |--locale (LOCALE / CLEAR)                 |Set contents locale localeof the cf api executed when executing the CLI command.    |X          |


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
  User checks the token value received from CF after cf login.
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
  Add the OpenPaaS CLI plugin repository.
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
  Delete CLI plugin repository
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
  Check the plugin repository list added to CLI
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
  Check the list of plugins in the repository.
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
  Check the list of available commands added plugin.
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
  Check the list of available commands of the added plugin.
  ```


  - **Parameter **


  | Parameter Name   |           Description                 | Required(O/X) |
  |-------------|--------------------------------|-----------|
  |URL or LOCAL-PATH/TO/PLUGIN   |Plugin URL or plugin name in local path or repository  |X          |
  |-r REPO_NAME                  |Plugin repository  name                                                   |X          |


  - **Example**

  ```
  $cf install-plugin 'Usage Report' -r CF-Community
  ```


----------
