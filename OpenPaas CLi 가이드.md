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

 기본 Syntax

 | cf [global options] command <arguments...> [command options] |
 |-------------------------------------------------------------------------------------------|

cf 명령어에 대괄호로 묶인 인자인 <arguments>과 [command options]는 명령어에 따라 선택적으로 사용되고, command 인자는 필수 인자이다. CF 운영 및 관리하기 위한 도구인 CF CLI 아래와 같이 많은 명령어들을 제공하고 있다.

## GETTING STARTED

#### login
- **기본 Syntax**


| $ cf login [-a API_URL] [-u USERNAME] [-p PASSWORD] [-o ORG] [-s SPACE] |
|-------------------------------------------------------------------------------------------|


|설명| OpenPaas에게 로그인 하기 위한 명령어|
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


