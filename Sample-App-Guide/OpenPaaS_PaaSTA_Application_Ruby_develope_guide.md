
## Table of Contents
1.	[개요](#1)
     * [1.1.	문서 개요](#2)
          * [1.1.1.	목적](#3)
          * [1.1.2.	범위](#4)
          * [1.1.3.	참고 자료](#5)
2.	[Ruby 애플리케이션 개발가이드](#6)
     * [2.1.	개요](#7)
     * [2.2.	개발환경 구성](#8)
          * [2.2.1.	Ruby & Ruby On Rails설치](#9)
     * [2.3.	개발](#10)
          * [2.3.1.	애플리케이션 생성](#11)
          * [2.3.2.	애플리케이션 환경설정](#12)
          * [2.3.3.	VCAP_SERVICES 환경설정 정보](#13)
          * [2.3.4.	Mysql 연동](#14)
          * [2.3.5.	Cubrid 연동](#15)
          * [2.3.6.	MongoDB 연동](#16)
          * [2.3.7.	Redis 연동](#17)
          * [2.3.8.	RabbitMQ연동](#18)
          * [2.3.9.	GlusterFS 연동](#19)
          * [2.3.10.	개별 서비스 연동 활용](#20)
     * [2.4.	배포](#21)
          * [2.4.1.	개방형 플랫폼 애플리케이션 배포](#22)
     * [2.5.	테스트](#23)

<div id='1'></div>
# 1.	개요

<div id='2'></div>
### 1.1.	문서 개요

<div id='3'></div>
##### 1.1.1.	목적

본 문서(Ruby 애플리케이션 개발 가이드)는 개방형 플랫폼 프로젝트의 서비스팩(Mysql, Cubrid, MongoDB, RabbitMQ, Radis, GlusterFS)을 Ruby 애플리케이션과 연동하여 사용하고 Ruby 애플리케이션을 배포하는 방법에 대해 제시하는 문서이다.

<div id='4'></div>
##### 1.1.2.	범위

본 문서의 범위는 Open PaaS 프로젝트의 Ruby 애플리케이션 개발과 서비스팩 연동, 애플리케이션 배포에 대한 내용으로 한정되어 있다.

<div id='5'></div>
##### 1.1.3.	참고 자료
**<http://rubyinstaller.org/>**  
**<https://docs.pivotal.io/pivotalcf/buildpacks/ruby/index.html/>**  
**<http://rubykr.github.io/rails_guides/getting_started.html/>**  
**<https://github.com/brianmario/mysql2/>**  
**<http://www.cubrid.org/manual/93/ko/api/ruby.html/>**  
**<https://docs.mongodb.org/ecosystem/drivers/ruby/>**  
**<http://rubybunny.info/articles/getting_started.html/>**  
**<https://github.com/redis/redis-rb/>**  
**<https://github.com/fog/fog/>**  


<div id='6'></div>
# 2.	Ruby 애플리케이션 개발가이드


<div id='7'></div>
### 2.1.	개요

개방형 플랫폼에 등록된 다양한 서비스팩을 Ruby언어로 작성된 애플리케이션과 바인딩하고 해당 애플리케이션에 바인딩된 서비스 환경정보(VCAP_SERVICES)를 이용하여 애플리케이션관 연동하고 각 서비스를 사용 할 수 있도록 Windows기반 환경에서 개방형 플랫폼에 배포할 Ruby 애플리케이션을 작성하는 방법을 설명한다.

<div id='8'></div>
### 2.2.	개발환경 구성

Ruby 애플리케이션 개발을 위해 다음과 같은 환경으로 개발환경을 구성 한다.

-	OS : Windows 7 64bit
-	Ruby : 1.9.3-p551
-	Framwork : Ruby On Rails 4.1.8
-	IDE : RubyMine 7.1.1   
※	CubridDB의 Ruby 드라이버 최신 지원 버전이 Ruby 1.9.3 까지 지원하여 해당 버전을 선택하였다. 각 서비스별 지원 되는 드라이버(또는 Gem)에 맞는 Ruby 버전을 사용하길 권장한다.   
※	Ruby IDE는 개별 선택하여 사용한다. 


<div id='9'></div>
##### 2.2.1.	Ruby & Ruby On Rails설치

1)	Ruby & DevKit 다운로드   
**<http://rubyinstaller.org/downloads/>**   
![ruby01]
- 다운로드  
RubyInstallers : Ruby 1.9.3-p551  
DEVELOPMENT KIT : DevKit-tdm-32-4.5.2-20111229-1559-sfx  

2)	Ruby 설치
- Ruby 1.9.3-p551.exe 더블클릭하여 설치를 실행한다.   
![ruby02]    
- “OK” 버튼 클릭  

![ruby03]  
- “I accet the License” 선택 후 “Next” 버튼 클릭   

![ruby04]  
- “Add Ruby executables to your PATH” 선택 후 “Install” 버튼 클릭   
	
![ruby05]  
- “Finish” 버튼을 클릭하여 Ruby 설치를 종료한다.   


3)	DEVELOPMENT KIT 설치
- DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe을 더블클릭하여 설치를 실행한다.   
![ruby06]  
![ruby07]  
- 설치할 폴더를 지정하고 “Extract”버튼을 클릭한다.
- Windows의 CMD 창을 실행하여 DevKit 설치 폴더로 이동한다.  

>ruby dk.rb init
-	“ruby dk.rb init” 명령을 실행하여 “config.yml” 파일을 생성한다.
![ruby08] 

>ruby dk.rb install
-	“ruby dk.rb install” 명령을 실행하여 DevKit을 설치한다.
![ruby09] 
-	“ruby –v” 명령을 실행하여 루비 버전을 확인한다.
![ruby10] 

4)	Ruby On Rails 설치
-	“gem update rdoc” 명령을 실행하여 rdoc gem을 업데이트한다.(미 실행시 rails install시 에러가 발생 할 수 있다.)
![ruby11] 
-	“gem install rails –v 4.1.8” 명령을 실행하여 rails을 설치한다.
![ruby12] 
-	“rails –v” 명령을 사용하여 rails의 버전을 확인한다.
![ruby13] 


<div id='10'></div>
### 2.3.	개발

Ruby 샘플 애플리케이션을 개발하기 위한 애플리케이션의 생성과 환경설정, VCAP_SERVICES 정보의 획득 및 각 서비스의 연동 방법에 대하여 설명한다.

<div id='11'></div>
##### 2.3.1.	애플리케이션 생성

1)	Rails 애플리케이션 생성(bundle install 제외)
>rails new [application name] –B –skip-bundle
![ruby14] 
![ruby15] 

2)	자동 생성 폴더 및 파일 정의

<table>
<tr>
    <td> </td>
    <td> </td>
</tr>
<tr>
    <td> </td>
    <td> </td>
</tr>
<tr>
    <td> </td>
    <td> </td>
</tr>
<tr>
    <td> </td>
    <td> </td>
</tr>
<tr>
    <td> </td>
    <td> </td>
</tr>
<tr>
    <td> </td>
    <td> </td>
</tr>

</table>






<div id='12'></div>
##### 2.3.2.	애플리케이션 환경설정


<div id='13'></div>
##### 2.3.3.	VCAP_SERVICES 환경설정 정보


<div id='14'></div>
##### 2.3.4.	Mysql 연동


<div id='15'></div>
##### 2.3.5.	Cubrid 연동


<div id='16'></div>
##### 2.3.6.	MongoDB 연동


<div id='17'></div>
##### 2.3.7.	Redis 연동


<div id='18'></div>
##### 2.3.8.	RabbitMQ연동


<div id='19'></div>
##### 2.3.9.	GlusterFS 연동


<div id='20'></div>
##### 2.3.10.	개별 서비스 연동 활용


<div id='21'></div>
### 2.4.	배포


<div id='22'></div>
##### 2.4.1.	개방형 플랫폼 애플리케이션 배포


<div id='23'></div>
### 2.5.	테스트



[ruby01]:/Sample-App-Guide/image/ruby/ruby_01.png
[ruby02]:/Sample-App-Guide/image/ruby/ruby_02.png
[ruby03]:/Sample-App-Guide/image/ruby/ruby_03.png
[ruby04]:/Sample-App-Guide/image/ruby/ruby_04.png
[ruby05]:/Sample-App-Guide/image/ruby/ruby_05.png
[ruby06]:/Sample-App-Guide/image/ruby/ruby_06.png
[ruby07]:/Sample-App-Guide/image/ruby/ruby_07.png
[ruby08]:/Sample-App-Guide/image/ruby/ruby_08.png
[ruby09]:/Sample-App-Guide/image/ruby/ruby_09.png
[ruby10]:/Sample-App-Guide/image/ruby/ruby_10.png
[ruby11]:/Sample-App-Guide/image/ruby/ruby_11.png
[ruby12]:/Sample-App-Guide/image/ruby/ruby_12.png
[ruby13]:/Sample-App-Guide/image/ruby/ruby_13.png
[ruby14]:/Sample-App-Guide/image/ruby/ruby_14.png
[ruby15]:/Sample-App-Guide/image/ruby/ruby_15.png
[ruby16]:/Sample-App-Guide/image/ruby/ruby_16.png
