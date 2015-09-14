## Table Contents


#### 문서 목적
본 문서는 MicroBOSH/BOSH에 대한 설치 및 운영 관리를 위한 도구인 BOSH CLI에 대해 기본 사용법 및 사용 예시를 통해 BOSH를 이해하는데 목적이 있다. 

#### 범위

본 문서에서는 BOSH CLI 사용법에 대해서 작성하였습니다.

#### 참고 자료

본 문서는 Cloud Foundry의 BOSH Document([http://bosh.io](http://bosh.io))를 참고로 작성하였습니다.


## BOSH CLI 기본 사용법

CLI는 BOSH 배포와 Release를 관리하기 위해 도움을 주는 커맨드 라인 유틸리티로 아래와 같이 2가지 형태로 구분된다.

- bosh : BOSH (Multi-VM BOSH)를 관리하기 위한 CLI 도구
- bosh micro : MicroBOSH (Single-VM BOSH)를 관리하기 위한 CLI 도구


- **기본 Syntax**

		$ bosh [<options>] <command> [<args>]
		$ bosh micro [<options>] <command> [<args>]


	bosh 또는 bosh micro 명령어에 대괄호로 묶인 options 과 args는 명령어에 따라 선택적으로 사용되고, command는 필수인자이다.


- **Options**

	|**번호**   |**옵션**                  |**설명**|
	|----------|-------------------------|--------------------------------|
	|1          |-c, --config              |BOSH configuration file 지정|
	|2          |--parallel MAX            |병렬 다운로드 최대 개수 설정|
	|3          |--[no-]color              |Toggle colorized output|
	|4          |-v, --verbose             |Show additional output|
	|5          |-q, --quiet               |suppress all output|
	|6          |-n, --non-interactive     |Don’t ask for user input|
	|7          |-N, --no-track            |Return task ID and don’t track|
	|8          |-P, --poll INTERVAL       |Director task polling interval|
	|9          |-t, --target URL          |타겟 디렉터 지정|
	|10         |-u, --user USER           |BOSH 사용자 아이디|
	|11         |-p, --password PASSWORD   |BOSH 사용자 비밀번호|
	|12         |-d, --deployment FILE     |BOSH 배포파일 지정|
	|13         |-h, --help                |Help 메시지 보기|
