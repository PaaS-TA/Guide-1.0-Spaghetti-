## Table of Contents
1. [개요](#개요)
     * [목적](#목적)
     * [범위](#범위)
     * [참고자료](#참고자료)
2. [BOSH Lite 설치](#bosh-lite-설치)
     * [설치전 준비사항](#설치전-준비사항)
     * [OpenPaaS 설치패키지 다운로드](#openpaas-설치패키지-다운로드)
     * [RUBY 설치](#ruby-설치)
     * [BOSH CLI 설치](#bosh-cli-설치)
     * [Virtualbox 및 Vagrant 설치](#virtualbox-및-vagrant-설치)
     * [BOSH Lite 설치](#bosh-lite-설치)
     * [로컬 가상머신의 IP 변경](#로컬-가상머신의-ip-변경)
     * [BOSH Lite 설치 Troubleshooting](#bosh-lite-설치-troubleshooting)
3. [OpenPaaS Container 설치와 배포 및 설정](#openpaas-container-설치와-배포-및-설정)
     * [OpenPaaS Release 업로드](#openpaas-release-업로드)
     * [OpenPaaS Container Release 업로드](#openpaas-container-release-업로드)
     * [BOSH Stemcell 업로드](#bosh-stemcell-업로드)
     * [OpenPaaS Release 배포](#openpaas-release-배포)
     * [OpenPaaS Container Release 배포](#openpaas-container-release-배포)
     * [OpenPaaS 로그인](#openpaas-로그인)
     * [배포한 OpenPaaS Container 설정](#배포한-openpaas-container-설정)
     * [OpenPaaS Container 설정 Troubleshooting](#openpaas-container-설정-troubleshooting)


# 개요


###	목적 
        

클라우드 환경에 서비스 시스템을 배포할 수 있는 BOSH는 릴리즈 엔지니어링, 개발, 소프트웨어 라이프사이클 관리를 통합한 오픈소스 프로젝트이다. 특히,
BOSH Lite는 사용자가 BOSH를 실제 환경에 도입하기 전에 로컬 환경에서 BOSH의 주요 기능을 검증하기 위한 프로젝트이다. 본 문서는 로컬 환경에서
BOSH Lite를 설치하고, BOSH 기능을 테스트 할 수 있는 환경을 구축하는 것이 목표이다.


### 범위 
    
가이드 범위는 BOSH Lite 설치하고, 설치한 BOSH Lite에 Warden OpenPaaS를 릴리즈하는 것까지 기술하였다.


### 참고자료 
    

본 문서는 Cloud Foundry의 BOSH Document를 참고하여 작성하였다.

Bosh Lite 설치:
[**https://github.com/cloudfoundry/bosh-lite**](https://github.com/cloudfoundry/bosh-lite)

OpenPaaS Container 배포:
[**https://github.com/cloudfoundry-incubator/diego-release**](https://github.com/cloudfoundry-incubator/diego-release)


# BOSH Lite 설치 


### 설치전 준비사항 
        

본 설치 가이드는 Linux(Ubuntu 14.04 64bit) 환경에서 설치하는 것을 기준으로 하였다. BOSH Lite는 Ruby언어로 되었기 때문에 BOSH Lite를
실행하기 Ruby가 설치되어 있어야 한다. 또한 BOSH Lite가 설치한 가상 머신을 구동할 가상 환경과 가상 환경을 설치하기 위한 도구로써
Virtualbox와 Vagrant를 설치해야 한다.



-   환경구성에 필요한 OpenPaaS 설치패키지

    -   OpenPaaS-Env

    -   OpenPaaS-Dev-Tools

-   메모리: 6GB의 메모리를 가진 가상 머신을 구동할 수 있는 여유 메모리

-   디스크: 최소 80GB 이상의 여유 디스크

-   위의 조건을 충족하는 Linux 또는 OSX 환경 (BOSH는 Window 환경을
    지원하지 않는다. 참조:[https://github.com/cloudfoundry-community/bosh-lite-demo](https://github.com/cloudfoundry-community/bosh-lite-demo))

### OpenPaaS 설치패키지 다운로드 

OpenPaaS 설치패키지를 다운받아 다운로드한 디렉토리에 압축해제 한다.

1.  **OpenPaaS 설치패키지를 다음 사이트에서 다운로드 한다.**

  
		http://openpaas.org
  


2.  **패키지 압축해제**

  
  		# 패키지의 확장자가 tar.gz인 경우
  		$ tar -xvzf <다운로드 디렉토리>/<사이트에서 내려받은 OpenPaaS 설치패키지>.tar.gz

  		# 패키지의 확장자가 zip 인 경우
  		$ cd <다운로드 디렉토리>
  		$ unzip <다운로드 디렉토리>/<사이트에서 내려받은 OpenPaaS 설치패키지>.zip


### RUBY 설치
    
Ruby 설치 절차는 다음과 같다.

1.  **Ruby 설치에 필요한 Dependency들을 설치한다.**


	- **온라인인 경우 ( 권장 )**


			$ sudo apt-get update

			$ sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev  
			sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev


	- 오프라인인 경우
	
			#다음의 패키지를 수동으로 설치한다.
	  		$ sudo dpkg -i <다운로드 디렉토리>/OpenPaaS-Env/bosh-lite-ruby-dependency/<패키지 파일명>
	
	  	    ---- ---------------------------- -----------------------------------------------------
		    No   패키지명                      패키지 파일명 (Ubuntu trusty amd64)
			---- ---------------------------- -----------------------------------------------------
		    1    build-essential              build-essential_11.6ubuntu6_amd64.deb
		    2    cluster-glue-dev             comerr-dev_2.1-1.42.9-3ubuntu1.2_amd64.deb
		    3    dpkg-dev                     dpkg-dev_1.17.5ubuntu5.4_all.deb
		    4    fakeroot                     fakeroot_1.20-3ubuntu2_amd64.deb
		    5    g++                          g++_4%3a4.8.2-1ubuntu6_amd64.deb
		    6    g++-4.8                      g++-4.8_4.8.4-2ubuntu1~14.04_amd64.deb
		    7    git                          git_1%3a1.9.1-1ubuntu0.1_amd64.deb
		    8    git-core                     git-core_1%3a1.9.1-1ubuntu0.1_all.deb
		    9    git-man                      git-man_1%3a1.9.1-1ubuntu0.1_all.deb
		    10   libdevel                     krb5-multidev_1.12+dfsg-2ubuntu5.1_amd64.deb
		    11   libalgorithm-diff-perl       libalgorithm-diff-perl_1.19.02-3_all.deb
		    12   libalgorithm-diff-xs-perl    libalgorithm-diff-xs-perl_0.04-2build4_amd64.deb
		    13   libalgorithm-merge-perl      libalgorithm-merge-perl_0.08-2_all.deb
		    14   libcurl4-openssl-dev         libcurl4-openssl-dev_7.35.0-1ubuntu2.5_amd64.deb
		    15   liberror-perl                liberror-perl_0.17-1.1_all.deb
		    16   libfakeroot                  libfakeroot_1.20-3ubuntu2_amd64.deb
		    17   libffi-dev                   libffi-dev_3.1~rc1+r3.0.13-12_amd64.deb
		    18   libgcrypt11-dev              libgcrypt11-dev_1.5.3-2ubuntu4.2_amd64.deb
		    19   libgnutls-dev                libgnutls-dev_2.12.23-12ubuntu2.2_amd64.deb
		    20   libgnutlsxx27                libgnutlsxx27_2.12.23-12ubuntu2.2_amd64.deb
		    21   libgpg-error-dev             libgpg-error-dev_1.12-0.2ubuntu1_amd64.deb
		    22   libgssrpc4                   libgssrpc4_1.12+dfsg-2ubuntu5.1_amd64.deb
		    23   libidn11-dev                 libidn11-dev_1.28-1ubuntu2_amd64.deb
		    24   libkadm5clnt-mit9            libkadm5clnt-mit9_1.12+dfsg-2ubuntu5.1_amd64.deb
		    25   libkadm5srv-mit9             libkadm5srv-mit9_1.12+dfsg-2ubuntu5.1_amd64.deb
		    26   libkdb5-7                    libkdb5-7_1.12+dfsg-2ubuntu5.1_amd64.deb
		    27   libkrb5-dev                  libkrb5-dev_1.12+dfsg-2ubuntu5.1_amd64.deb
		    28   libldap2-dev                 libldap2-dev_2.4.31-1+nmu2ubuntu8.1_amd64.deb
		    29   libp11-kit-dev               libp11-kit-dev_0.20.2-2ubuntu2_amd64.deb
		    30   libreadline6-dev             libreadline6-dev_6.3-4ubuntu2_amd64.deb
		    31   libreadline-dev              libreadline-dev_6.3-4ubuntu2_amd64.deb
		    32   librtmp-dev                  librtmp-dev_2.4+20121230.gitdf6c518-1_amd64.deb
	    	33   libsqlite3-dev               libsqlite3-dev_3.8.2-1ubuntu2.1_amd64.deb
		    34   libssl-dev                   libssl-dev_1.0.1f-1ubuntu2.15_amd64.deb
		    35   libssl-doc                   libssl-doc_1.0.1f-1ubuntu2.15_all.deb
		    36   libstdc++-4.8-dev            libstdc++-4.8-dev_4.8.4-2ubuntu1~14.04_amd64.deb
		    37   libtasn1-6-dev               libtasn1-6-dev_3.4-3ubuntu0.3_amd64.deb
		    38   libtinfo-dev                 libtinfo-dev_5.9+20140118-1ubuntu1_amd64.deb
		    39   libxml2-dev                  libxml2-dev_2.9.1+dfsg1-3ubuntu4.4_amd64.deb
		    40   libxslt1-dev                 libxslt1-dev_1.1.28-2build1_amd64.deb
		    41   libyaml-0-2                  libyaml-0-2_0.1.4-3ubuntu3.1_amd64.deb
		    42   libyaml-dev                  libyaml-dev_0.1.4-3ubuntu3.1_amd64.deb
		    43   python-pycurl                python-pycurl_7.19.3-0ubuntu3_amd64.deb
		    44   python-software-properties   python-software-properties_0.92.37.3_all.deb
		    45   sqlite3                      sqlite3_3.8.2-1ubuntu2.1_amd64.deb
		    46   zlib1g-dev                   zlib1g-dev_1%3a1.2.8.dfsg-1ubuntu1_amd64.deb
		    ---- ---------------------------- -----------------------------------------------------

  		- ***패키지 설치시 의존관계를 유의하여 설치한다.***

  		- ***표의 패키지 파일명은 ubuntu trusty 64bit 버전을 기준으로 쓰여졌다.***

	  	- ubuntu의 다른 버전 또는 CentOS나 OSX경우 인터넷이 되는 단말기에서 아래의 패키지에 해당하는 파일을 내려받아 수동으로 설치한다.

	  	- 오프라인인 경우, ruby dependency의 샘플 설치 스크립트를 환경에 맞게 수정하여 설치할 수도 있다.

		  		$ cd <다운로드 디렉토리>/OpenPaaS-Env/bosh-lite-ruby-dependency
				$ sudo ./ruby-dependency-packages.sh ./



2.  **패키지에서 설치**

		$ tar -xvzf <다운로드 디렉토리>/OpenPaaS-Env/ruby/ruby-2.2.2.tar.gz
		$ cd ruby-2.2.2
		$ ./configure
		$ make
		$ sudo make install

	※ 온라인의 경우, rvm 또는 rbenv등을 이용하여 ruby를 설치할 수 있다. 
    단, ruby 1.9.3버전 이상을 설치하도록 한다.

3.  **설치 확인**
  
		$ ruby -v

### BOSH CLI 설치 
  
BOSH CLI의 설치 절차는 다음과 같다.

1.  **설치할 환경 생성**

		$ mkdir -p ~/workspace/bosh
		$ cd ~/workspace/bosh

2.  **Bosh_cli 설치**

	- 온라인의 경우

			#bosh 압축해제
			$ unzip <다운로드 디렉토리>/OpenPaaS-Dev-Tools/bosh-cli.zip
	
	  		#bosh_cli 의존 패키지 설치
			$ gem install bosh_cli

	- 오프라인의 경우
  
			#bosh 압축해제
			$ unzip <다운로드 디렉토리>/OpenPaaS-Dev-Tools/bosh-cli.zip
	
			#bosh_cli 의존 패키지 압축해제
			$ sudo tar -xvf <다운로드 디렉토리>/OpenPaaS-Env/gem-dependency/bosh_cli_gem_install_package.tar -C /usr/local/lib/ruby/gems/2.2.0
	
			# 압축해제한 gem 설치
			$ sudo gem install --force --local /usr/local/lib/ruby/gems/2.2.0/cache/*.gem
  

3.  **Bosh 설치 확인**

		$ bosh


### Virtualbox 및 Vagrant 설치
   

BOSH Lite는 가상 환경 구축을 위해 Vagrant를 사용한다. Vagrant는 완벽한 가상 개발 환경을 구축해 주는 오픈 소스이다. 또한 BOSH Lite는 가상 환경으로 Virtualbox를 사용한다.


1.  **VirtualBox Dependency를 설치한다.**

		$ sudo dpkg -i <다운로드 디렉토리>/OpenPaaS-Env/virtualbox-dependency/libsdl1.2debian_1.2.15-8ubuntu1_amd64.deb

	※ **패키지명 : libsdl1.2debian**


2.  **Virtualbox 설치**


		#VirtualBox 설치
		$ sudo dpkg -i <다운로드 디렉토리>/OpenPaaS-Env/virtualbox-5.0/virtualbox-5.0_5.0.0-101573-Ubuntu-trusty_amd64.deb
  
		#VirtualBox 확장팩 설치
		$ sudo VBoxManage extpack install <다운로드 디렉토리>/OpenPaaS-Env/virtualbox-5.0/Oracle_VM_VirtualBox_Extension_Pack-5.0.0-101573.vbox-extpack


	- CentOS 또는 RHEL의 경우는 *.rpm을 설치한다.

	- OSX의 경우는 VirtualBox-5.0.0-101573-OSX.dmg를 설치한다.

	- VirtualBox 확장팩은 모든 OS에 공통으로 설치할 수 있다.

	- 온라인의 경우 다른 버전의 virtualbox를 다운로드 받아 설치할 수 있다.
    단, virtualbox 4.3.18 버전 이상을 설치해야 한다.

	- 다른 버전의 virtualbox를 설치한 경우, 설치한 virtualbox와 동일한 버전의 확장팩을 다운받아 설치한다.

3.  **VirtualBox 설치 확인**

		$ VBoxManage --version

4.  **Vagrant 설치**

		$ sudo dpkg -i <다운로드 디렉토리>/OpenPaaS-Env/vagrant-1.7.4/vagrant_1.7.4_x86_64.deb

	- CentOS 또는 RHEL의 경우는 *.rpm을 설치한다.

	- OSX의 경우는 vagrant_1.7.4.dmg를 설치한다.

5.  **Vagrant 설치 확인**

		$ vagrant --version

### BOSH Lite 설치

BOSH Lite를 설치하는 절차는 다음과 같다.

1.  **BOSH Lite 압축해제**

		$ cd ~/workspace
		$ unzip <다운로드 디렉토리>/OpenPaaS-Dev-Tools/bosh-lite.zip

2.  **Vagrantfile 수정**

	- ***온라인의 경우, Vagrantfile의 수정이 필요없다.***

	- 오프라인의 경우
  
			$ cd bosh-lite
			$ vi Vagrantfile
	
	
			Vagrant.configure('2') do |config|
	
				config.vm.box = 'cloudfoundry/bosh-lite'
					config.vm.provider :virtualbox do |v, override|
					override.vm.box_version = '9000.39.0' # ci:replace
					# To use a different IP address for the bosh-lite director, uncomment this line:
					# override.vm.network :private_network, ip: '192.168.59.4', id: :local
		  			override.vm.network :private_network, ip: '192.168.50.4', id: :local				## 추가한다.
					override.vm.network :public_network													## 추가한다.
				end
		
				config.vm.provider :aws do |v, override|
					override.vm.box_version = '9000.39.0' \# ci:replace
					# To turn off public IP echoing, uncomment this line:
					# override.vm.provision :shell, id: "public_ip", run: "always", inline: "/bin/true"
		
					# To turn off CF port forwarding, uncomment this line:
					# override.vm.provision :shell, id: "port_forwarding", run: "always", inline: "/bin/true"
		
					# Following minimal config is for Vagrant 1.7 since it loads this file before downloading the box.
					# (Must not fail due to missing ENV variables because this file is loaded for all providers)
					v.access_key_id = ENV['BOSH_AWS_ACCESS_KEY_ID'] || ''
					v.secret_access_key = ENV['BOSH_AWS_SECRET_ACCESS_KEY'] || ''
					v.ami = ''
				end
	
			end


3.  **Vagrant에 BOSH Lite box 추가**

	- **온라인의 경우, Vagrant up 을 실행할 때 자동으로 BOSH Lite box 를 추가한다.**


	- 오프라인의 경우
      
			$ vagrant box add cloudfoundry/bosh-lite <다운로드 디렉토리>/OpenPaaS-Env/vagrantbox/bosh-lite.box
      

		-   추가하는 box명은 cloudfoundry/bosh-lite 이다.

		-   Vagrantfile의 config.vm.box에서 선언한 이름과 동일해야 한다.

		-   설치패키지의 override.vm.box_version은 ‘9000.39.0’이다.


4.  **BOSH Lite 설치**

	-   온라인의 경우
  
  			$ vagrant up

	-   오프라인의 경우
      
			$ vagrant up
			

			
			cloud4u@XPS-15-9530:~/workspace/bosh-lite\$ vagrant up
			Bringing machine 'default' up with 'virtualbox' provider...
			==\> default: Importing base box 'cloudfoundry/bosh-lite'...
			==\> default: Matching MAC address for NAT networking...
			==\> default: Setting the name of the VM: bosh-lite_default_1438745131006_83536
			==\> default: Clearing any previously set network interfaces...
			==\> default: Available bridged network interfaces:

			1) eth1						## 서버의 물리 NIC
			​2) wlan0										
			​3) virbr0					## 가상 bridge NIC
			
			==\> default: When choosing an interface, it is usually the one that is
			==\> default: being used to connect to the internet.
			default: Which interface should the network bridge to? 1			## 연결할 물리 NIC을 선택한다.			
			# 설치한 환경에 따라 network interface의 보기는 다르다.
			
			==\> default: Preparing network interfaces based on configuration...
			default: Adapter 1: nat
			default: Adapter 2: hostonly
			default: Adapter 3: bridged
			==\> default: Forwarding ports...
			default: 22 =\> 2222 (adapter 1)
			==\> default: Booting VM...
			==\> default: Waiting for machine to boot. This may take a few minutes...
			default: SSH address: 127.0.0.1:2222
			default: SSH username: vagrant
			default: SSH auth method: private key
			default: Warning: Connection timeout. Retrying...
			==\> default: Machine booted and ready!
			==\> default: Checking for guest additions in VM...
			default: The guest additions on this VM do not match the installed version of
			default: VirtualBox! In most cases this is fine, but in rare cases it can
			default: prevent things such as shared folders from working properly. If you see
			default: shared folder errors, please make sure the guest additions within the
			default: virtual machine match the version of VirtualBox you have installed on
			default: your host and reload your VM.
			
			default:
			default: Guest Additions Version: 4.3.18
			default: VirtualBox Version: 5.0
			==\> default: Configuring and enabling network interfaces...
			==\> default: Mounting shared folders...
			default: /vagrant =\> ~/workspace/bosh-lite
   
		-   Vagrantfile이 있는 곳에서 vagrant up을 실행해야 한다.



5.  **BOSH Lite 로그인**

	- login


			$ bosh target 192.168.50.4 lite
		
			Target set to \`Bosh Lite Director'
			Your username: admin
			Enter password: *****
			Logged in as `admin'

		-   Bosh lite의 기본 IP는 192.168.50.4 이다.

		-   login 기본 사용자와 패스워드는 admin/admin



6.  **Route 추가**

		$ sudo bin/add-route


7.  **생성한 Bosh lite 가상 머신 확인**

		$ cd ~/workspace/bosh-lite
		$ vagrant ssh


8.  **가상 머신의 vagrant 권한 확인** ***※가상 머신에서 실행***

	- 가상 머신 login

			$ vagrant ssh

	- vagrant 권한 확인

			$ cd /
			$ ll

			drwxr-xr-x 10 vcap vcap 4096 Aug 5 07:28 vagrant/

		-   Vagrant의 소유자가 vcap이 아닌 경우 (예: vagrant) 또는 vcap에게 쓰기권한이 없는 경우 bosh deploy를 할 때 오류가 발생한다.



9.  **가상 머신의 vagrant의 소유자가 vcap이 아닌 경우만 실행** ***※가상머신에서 실행***

  
		#vagrant 백업 디렉토리 생성
		$ sudo mkdir temp**

		#vagrant 모든파일 백업
		$ sudo cp -aR vagrant/* /temp

		#vagrant 소유자 변경
		$ sudo chown -hR vcap:vcap /vagrant

		# 소유권 변경 확인
		$ ll

		# 가상 머신 재기동
		$ sudo reboot

		# 가상 머신이 재기동 한 후 , 가상 머신에 로그인
		$ vagrant ssh

		#vagrant 파일 및 소유권 확인
		$ cd /vagrant
		$ ll

		# 파일이 없거나 소유권이 변경되지 않은 경우만 실행
		$ sudo cp -aR /temp/* /vagrant
		$ sudo chown -hR vcap:vcap /vagrant


### 로컬 가상머신의 IP 변경 

설치한 가상머신는 IP가 192.168.50.4로 고정되어 있다. 만일 해당 IP를 변경하고 싶다면 vagrant up을 실행하기 전에 Vagrantfile을 수정해야 한다.
수정할 곳은 붉은 표시가 된 곳의 주석을 삭제하고 바꾸고자 하는 IP주소로 변경하면 된다. 또한 route를 추가 할 때 필요한 add-route도 수정해야 한다.

1.	**Vagrantfile 수정**


		$ vi ~/workspace/bosh-lite/Vagrantfile


		Vagrant.configure('2') do |config|
			config.vm.box = 'cloudfoundry/bosh-lite'

			config.vm.provider :virtualbox do |v, override|
				override.vm.box_version = '9000.38.0' # ci:replace
				# To use a different IP address for the bosh-lite director, uncomment this line:
				override.vm.network :private_network, ip: '192.168.59.4', id: :local				## 주석표시를 삭제하고 사용할 ip로 변경한다.
			end

			config.vm.provider :aws do |v, override|
				override.vm.box_version = '9000.38.0' # ci:replace
				# To turn off public IP echoing, uncomment this line:
				# override.vm.provision :shell, id: "public_ip", run: "always", inline: "/bin/true"

				# To turn off CF port forwarding, uncomment this line:
				# override.vm.provision :shell, id: "port_forwarding", run: "always", inline: "/bin/true"

  				# Needed for Vagrant 1.7 since it loads Vagrantfile before downloading the box
				env = ENV.to_hash
				v.access_key_id = env.fetch('BOSH_AWS_ACCESS_KEY_ID', '')
				v.secret_access_key = env.fetch('BOSH_AWS_SECRET_ACCESS_KEY', '')
				v.ami = ''
			end
		end
  

2.	**add-route 수정**
  
		$ vi ~/workspace/bosh-lite/bin/add-route

		#!/bin/bash

		echo "Adding the following route entry to your local route table to enable direct warden container access. Your sudo password may be required."
		echo " - net 10.244.0.0/19 via 192.168.50.4"

		if [ `uname` = "Darwin" ]; then
			sudo route delete -net 10.244.0.0/19 192.168.50.4 > /dev/null 2>&1				## 192.168.50.4를 위에서 변경한 ip로 바꾼다. 
			sudo route add -net 10.244.0.0/19 192.168.50.4									## 192.168.50.4를 위에서 변경한 ip로 바꾼다.
		elif [ `uname` = "Linux" ]; then
			sudo route add -net 10.244.0.0/19 gw 192.168.50.4								## 192.168.50.4를 위에서 변경한 ip로 바꾼다.
		fi
  

### BOSH Lite 설치 Troubleshooting 

1.  **Bosh login 또는 stemcell의 업로드가 잘 되지 않는 경우, 아래의 명령어로 bosh를 재기동 한다.**

		$ vagrant ssh -c "sudo sv restart director"


2.  **가상 머신을 삭제할 경우, 아래의 명령어를 사용한다.**
  
		# 가상 머신 삭제
		$ vagrant destroy

	※ 또는 Virtualbox에서 해당 가상 머신을 삭제한다.

		# 가상 머신 삭제 확인
		$ vagrant global-status --prune


3.  **Bosh의 Blobstore를 clean할 경우, 아래의 명령어를 사용한다.**

		$ bosh cleanup


4.  **생성한 BOSH Lite 가상머신에 로그인 할 경우, 아래의 명령어를 사용한다.**

		$ vagrant ssh


5.  **생성한 Bosh lite는 vagrant halt 또는 virtualbox에서 power off를 실행해서 종료하면 가상 머신에 오류가 발생 할 수 있다. Virtualbox
    등에서 가상 머신을 <Save state\>모드로 종료하도록 한다.**


6.  **가상 머신에 오류가 발생 한 경우, 다음의 명령어로 복구한다.**
   
		$ bosh cck

6.  **Vagrant up 실행시 VirtualBox에 가상 머신이 등록되지 않는 경우**

  
		# 가끔 virtualbox 확장팩 버전과 bosh lite box 의 확장팩 버전이 일치 하지 않는 경우 발생한다.
		# 설치한 가상 머신을 삭제하고 서버를 재기동 한 후, 다시 가상 머신을 설치하면 해결 된다.

		# 가상 머신 삭제
		$ vagrant destroy

		# 서버 재시작
		$ sudo reboot
  
		# 가상 머신 설치
		$ cd ~/workspace/bosh-lite
		$ vagrant up

# OpenPaaS Container 설치와 배포 및 설정


본 절에서는 BOSH Lite에 OpenPaaS Container를 릴리즈하는 절차를 기술한다. Container는 OpenPaaS의 새로운 런타임 아키텍처로 이전 버전의 OpenPaaS의
DEAs와 Health Manager를 대체 한다. 자세한 내용은 다음 사이트를 참조한다.

참조:[https://github.com/cloudfoundry-incubator/diego-release](https://github.com/cloudfoundry-incubator/diego-release)


-   환경구성에 필요한 OpenPaaS 설치패키지

    -   OpenPaaS-Stemcells

    -   OpenPaaS-Controller

    -   OpenPaaS-Container

    -   OpenPaaS-Env


### OpenPaaS Release 업로드 


1.  **release를 bosh에 업로드**

		$ cd ~/workspace/bosh-lite
		$ bosh upload release <다운로드 디렉토리>/OpenPaaS-Controller/openpaas-beta-1.0.tgz


2.  **release 업로드 확인**

		$ bosh releases


### OpenPaaS Container Release 업로드 

1.  **OpenPaaS Container Release 업로드**

		$ cd ~/workspace/bosh-lite
		$ bosh upload release <다운로드 디렉토리>/OpenPaaS-Container/openpaas-container.tgz


2.  **OpenPaaS Container Release 확인**

		$ bosh releases


### BOSH Stemcell 업로드 


1.  **BOSH에 Warden Stemcell 업로드**

  
		$ bosh upload stemcell <다운로드 디렉토리>/OpenPaaS-Stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent


2.  **Stemcell 업로드 확인**


		$ bosh stemcells


### OpenPaaS Release 배포 

OpenPaaS Release를 배포한다.


1.  **Bosh 상태 확인**


		$ bosh status


		~/workspace/bosh-lite$ bosh status

		Config
		/home/user/.bosh_config

		Director
			Name 						Bosh Lite Director
			URL							https://192.168.50.4:25555
			Version 					1.3062.0 (00000000)
			User 						admin
			UUID 						cf5f29da-51c9-433f-8c1f-9ae0536d4778			## 나중에 yml파일 수정시 필요하다.
			CPI 						cpi
			dns 						disabled
			compiled_package_cache 		enabled (provider: local)
		snapshots disabled
		
		
		Deployment
		not set



2.  **Yml 파일을 OpenPaaS환경에 맞게 수정**
  
		$ cp <다운로드 디렉토리>/OpenPaaS-Deployment/openpaas-beta-lite.yml ~/workspace
		$ vi ~/workspace/openpaas-beta-lite.yml


		compilation:
		cloud_properties:
		name: random
		network: cf1
		reuse_compilation_vms: true
		workers: 6
		director_uuid: cf5f29da-51c9-433f-8c1f-9ae0536d4778		## 조회한 bosh director uuid로 변경
		
		jobs:
		  - instances: 1
		  name: consul_z1
		...



3.  **Yml 파일을 BOSH Lite에 deployment**

		$ bosh deployment ~/workspace/openpaas-beta-lite.yml


4.  **업로드한 release를 배포**


		$ bosh deploy


5.  **배포한 release 확인**

  
		$ bosh vms


### OpenPaaS Container Release 배포

OpenPaaS Container Release를 배포한다.


1.  **Yml 파일을 수정**


		$ cp <다운로드 디렉토리>/OpenPaaS-Deployment/openpaas-container-beta-lite.yml ~/workspace
		$ vi ~/workspace/openpaas-container-beta-lite.yml


		compilation:
		cloud_properties: {}
		network: diego1
		reuse_compilation_vms: true
		workers: 6
		director_uuid: cf5f29da-51c9-433f-8c1f-9ae0536d4778 		## 조회한 bosh director uuid 로 변경
		
		jobs:
		  - instances: 1
		  name: etcd_z1

		...



2.  **Yml 파일을 BOSH Lite에 deployment**

  
		$ bosh deployment ~/workspace/openpaas-container-beta-lite.yml


3.  **OpenPaaS Container Release 배포**

  
		$ bosh deploy


4.  **OpenPaaS Container Release 배포 확인**


		$ bosh vms


### OpenPaaS 로그인 

1.  **openpaas CLI 설치**

  
		$ sudo tar -xzf <다운로드 디렉토리>/OpenPaaS-Dev-Tools/openpaas-CLI/openpaas-linux-amd64.tgz -C /usr/local/bin

	-   OSX의 경우 openpaas-darwin-amd64.tgz를 설치한다.


2.  **배포한 OpenPaaS Container Release에 로그인**


		$ cf login -a api.10.244.0.34.xip.io -u admin -p admin --skip-ssl-validation


### 배포한 OpenPaaS Container 설정 


1.  **조직 작성**


		$ cf create-org openpaas-org

	-   openpaas-org는 다른 조직명으로 변경 가능하다.


2.  **조직 지정**


		$ cf target -o openpaas-org


3.  **Space 생성**

  
		$ cf create-space openpaas-space

	-   openpaas-space는 다른 스페이스명으로 변경 가능하다.


4.  **Space 지정**


		$ cf target -s openpaas-space


### OpenPaaS Container 설정 Troubleshooting 

1.  **OpenPaaS Release 타겟 지정에서 오류가 발생하는 경우**
  
		# 오프라인 상태에서 OpenPaaS Container를 타겟으로 지정 할 경우, 실패한다.
	
	- 네트워크에 연결하여 온라인 상태로 OpenPaaS Container를 타겟 지정하거나 별도 DNS서버를 구축하여 DNS서버와 연결한 후, OpenPaaS Container를 타겟 지정한다.
  	
  
