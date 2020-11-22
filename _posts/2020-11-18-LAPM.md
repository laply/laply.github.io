# Linux / Apache2 / PHP / Mysql

## 1. linux 설치하기 

2020.11.18 기준 ubuntu 20.04.1 version 사용 

linux 서버를 돌리기 위해 Oracle VM VirtualBox를 먼저 설치한다. 

https://www.virtualbox.org/

![virtualbox_site](https://user-images.githubusercontent.com/65690925/99492733-7099a600-29b1-11eb-9d83-f6c7c7e3c633.JPG)

virtualBox는 많이 사용하기도 했었고 VMWare와는 달리 무료로 이용할 수 있다. 
가상 머신을 돌리기 쉽게 되어있어 사용하기 편하다.

그 외의 방법은 aws, azure를 사용해서 서버를 구성해도 되고,
최근에 장고를 연습할때 사용했던 goormide를 사용해 컨테이너를 만들어 사용할 수도 있다. 

설치 완료 후 ubuntu server 버전을 다운받는다. 

https://ubuntu.com/download/server

![ubuntu_site](https://user-images.githubusercontent.com/65690925/99492776-81e2b280-29b1-11eb-8e32-4790b995dcf0.JPG)

총 3가지 옵션이 존재한다. 

1. Multipass 
    자세히 공부가 필요하다. 설치해서 사용 해봤을 때, 
    윈도우 cmd, power shell으로 가상 하드의 분리 없이 사용할 수 있도록 되어 있는 것 같다. 

2. Maas


3. Manual install 
    직접적으로 사용할 파일이다. 
    20.04.1 LTS 버전을 다운 받아서 사용한다. 
    iso 파일로 가상 하드에 설치할 os 파일이다.


VirtualBox를 실행하여 새로 만들기를 통해 
가상 머신을 나타내는 이름과 저장할 폴더, 설치 운영체제 및 
할당할 메모리와, 가상하드의 용량을 선택하여 생성한다. 

생성된 가상머신을 실행시, 광학 드라이브를 다운 받은 ubuntu server의 iso 파일을 선택하여 실행 후 
설정을 완료하면 설치가 마무리 된다. 

![virtualbox](https://user-images.githubusercontent.com/65690925/99487467-c5392300-29a9-11eb-8c8a-08dbc34f3034.JPG)

### 추가

내부 IP를 할당 받기 위한 네트워크 설정 

실행된 가상 머신을 종료한 후

virtual box에 새로 만들기 옆으로 설정을 클릭한다. 

새로 뜬 창에서 왼쪽 메뉴에 네트워크를 클릭해,
 어뎁터 1 을 어뎁터에 브리지 변경한다.
 어뎁터 2 를 NAT으로 변경한다.

변경 후 내부 IP가 할당된다. 

## 2. Apache INSTALL
1에서 설치한 가상 리눅스를 실행한다.
설치시 설정한 아이디와 비밀 번호를 사용하여 접속한다.

![linux server_enter](https://user-images.githubusercontent.com/65690925/99487693-47c1e280-29aa-11eb-940c-35677d0827a1.JPG)

설치에 앞서 업데이트를 수행한다. 
sudo apt-get update && sudo apt-get upgrade

아파치2를 설치한다. 
sudo apt-get install apache2 

설치 후 버전확인으로 설치가 잘되었는지 확인하고,
서비스를 시작한다. 

apache2 -v 
sudo service apache2 start

apache 명령어 
start 시작
stop 중지
restart 재 실행

실행 후에 아무런 변화가 없는 것처럼 보여서 실망할 수 있지만, 
서버가 돌아가는지 절차를 통해서 확인하자.

1. 서비스가 내부적으로 돌아가는 지 확인 
    
    sudo apt-get install net-tools
    netstat -ntlp

    local Address에 :::80 이 보인다면 서비스가 돌고있는 것을 알 수 있다. 

2. 돌아가고 있는 웹 서버로 접속하여 확인

    1에서 네트워크 설정을 완료 했다면, 
    ifconfig를 통해 내부 아이피 값을 확인할 수 있다.
    
    웹사이트에 아이피 값을 친다면 아파치 파일 사이트를 확인할 수 있다. 

![linux netstat](https://user-images.githubusercontent.com/65690925/99487703-4b556980-29aa-11eb-8a25-99bdf5076feb.JPG)
![apache 실행](https://user-images.githubusercontent.com/65690925/99488074-f82fe680-29aa-11eb-8479-4888e607fb5e.JPG)


html, css등 파일을 올리는 위치 

cd /var/www/html

위치에 파일을 올리면 확인 가능하다. 
apache 기본페이지도 index.html에 존재한다.

### 추가 

1. 방화벽 설정
    ufw를 사용한다. 

    #### what is ufw ? 

    sudo ufw app info "Apache Full"
    sudo ufw allow in "Apache Full"

    sudo ufw status numbered

2. 포트 변경 

    ports.conf 수정 

    sudo nano /etc/apache2/ports.conf 
    Listen 80 > 사용할 포트 번호로 수정 

    000-default.conf 수정 

    sudo nano /etc/apache2/sites-available/000-default.conf 
    <VirtualHost *:80> 의 80 > 사용할 포트번호

    이후 방화벽을 설정했다면 사용할 포트번호를 재등록 해야한다.
    sudo ufw allow 사용할 포트번호

3. 한글 깨짐 수정 
    
    만약 html과 같은 파일을 한글이 깨진다면 
    charset.conf 를 수정해야 한다.
    sudo nano /etc/apache2/conf-enabled/charset.conf

    #AddDefaultCharset UTF-8

    주석을 제거한다.  

    AddDefaultCharset UTF-8


각 설정 후에 아파치를 재 실행하여 서버에 설정된 파일을 변경한다. 

sudo service apache2 restart

## 3. mysql 설치 

터미널에서 mysql-server를 설치한다.

sudo apt-get install mysql-server

설치 완료 후 mysql을 세팅한다.
sudo mysql_secure_installation 

질문에 대한 자세한 설명은 하지않고 (필자는 다 Y 값으로)
root 비밀번호 생성

mysql 실행
sudo mysql -u root -p 

![mysql enter](https://user-images.githubusercontent.com/65690925/99492743-75f6f080-29b1-11eb-8007-98c3c6bb6870.JPG)

외부에서 접근할때 사용할 수 있는 아이디를 생성하고 모든 권한을 부여한다. 
이후 phpmyadmin에서 사용한다 // 추가내용 '' 도 같이 적는다.

create user 'id'@''localhost' identified by 'password'
grant all privileges on *.* to 'id'@'localhost' with grant option;

mysql 에서 쿼리를 사용해 CRUD를 할 수 있고, 
sql 파일을 사용해서 추가도 가능하다. 

sudo mysql -u root -p < aa.sql 

## 4. php 설치 

터미널에서 php를 설치한다.

sudo apt-get install php php-mysql 

설치가 완료되면 window 환경과 다르게 설정하지 않고 사용할 수 있다. 
설치를 확인하기 파일을 생성해서 확인한다. 

info.php 파일을  /var/www/html/ 경로에 생성한다.

sudo nano /var/www/html/info.php 

파일 내부에 작성할 코드 

<?php phpinfo(); ?>

그리고 apache 실행시 접속했던 ip로 접속 하여 확인한다.
port가 80이면 생략가능 

ip:port/info.php

![pipInfo page](https://user-images.githubusercontent.com/65690925/99493237-341a7a00-29b2-11eb-93c9-fd01dcfb053b.JPG)

실행 시 설치가 확인된다. 

## 5. myphpadmin 설치 및 설정 

window 개발환경과 다르게 ubuntu server는 CLI로 설정된다.
사용을 많이 했다면 편리하게 사용할 수 있지만,
GUI 많이 접했다면, 특히 Database 사용에 어려움이 있을 수 있다.

mysql을 web page에서 설정하고 관리할 수 있게 만들어 진 
오픈 소스 도구를 이용해 관리할 수 있는 환경을 세팅 한다. 

먼저 터미널에서 phpmyadmin을 설치한다. 

sudo apt-get install phpmyadmin 

이후 apache에 include 한다.

sudo nano /etc/apache2/apache.conf 

파일 마지막 라인에 추가한다.
Include /etc/phpmyadmin/apache.conf 
sudo service apache2 restart

http://ip:port/phpmyadmin 

![phpmyadmin](https://user-images.githubusercontent.com/65690925/99492748-77281d80-29b1-11eb-8344-3b87b7cbbe11.JPG)

이전 mysql 사용할때 생성했던 아이디와 패스워드를 사용하여 접속한다.
