
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

-	http://rubyinstaller.org/
-	https://docs.pivotal.io/pivotalcf/buildpacks/ruby/index.html
-	http://rubykr.github.io/rails_guides/getting_started.html
-	https://github.com/brianmario/mysql2
-	http://www.cubrid.org/manual/93/ko/api/ruby.html
-	https://docs.mongodb.org/ecosystem/drivers/ruby/
-	http://rubybunny.info/articles/getting_started.html
-	https://github.com/redis/redis-rb
-	https://github.com/fog/fog


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


<div id='10'></div>
### 2.3.	개발


<div id='11'></div>
##### 2.3.1.	애플리케이션 생성


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


