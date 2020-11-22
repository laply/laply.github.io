---
category : back-end
author_profile : true 
toc : true
toc_sticky: true
comments: true
---

# linux / Tomcat / jsp / mysql

리눅스 서버로 톰캣 설치와 jsp - mysql을 연동하는 절차를 정리한다.
리눅스 서버는 ubuntu 20.04를 사용하였고, 설치하는 방법을 제외한다. 
설치하는 방법은 2020/TODO/back-end/LAPM 참조

# Tomcat 설치하기 

리눅스 서버 20.04을 실행한 후 접속 
톰켓9 을 설치한다.

sudo apt-get install tomcat9

// 설치되는 명령어 및 확인 창

잘 설치 되었는지 버전 채크로 확인 한다.
tomcat9 -version

sudo service tomcat9 start

톰켓 실행 후 
ifconfig를 사용해 아이피 값을 확인한다.

실행 및 설치가 잘 되었는지 확인하기 위해 
http://ip:8080 으로 접속해 확인한다.

// 실행 확인 화면

만약 설치도 잘 되었고 동작 수행도 
시켰는데 뜨지 않는다면 방화벽을 확인해, 포트를 열어준다. 

sudo ufw allow 8080
파트를 열고 다시 확인해 보면 될 수있다. 

# 설치된 tomcat의 기본 파일의 위치 

톰캣이 어디에 설치되어있고 웹 페이지를 실행하기 위해선 
기본 위치를 알아야 한다. 

    /var/lib/tomcat9/webapps/ROOT 
    
에 위치한다.

sudo cd /var/lib/tomcat9/webapps/ROOT  
를 사용해 들어가자.

들어간다면 index.html 파일이 존재한다.

이곳에 사용할 jsp 파일과, html, css 등의 파일을 두어 실행시킨다. 

# mysql 설치 

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


# tomcat-jsp와 mysql 연결하기 

jsp에서 mysql 과 연결 및 CRUD 하기 위해서 필요한 절차이다.

JDBC mysql connector를 설치해서 
정학한 위치에 넣어 주면 실행된다.  

하지만 필자가 많이 해맸던 부분이다. 

먼저 서버환경 ( GUI가 없는 환경 ) 에서 수행해서 
파일을 어떻게 설치해야 하는지도 고민했고, 

분명 드라이버를 설치 했는데도 되지 않아서 난감 하고,

위치를 정확하게 넣은 것 같은데 jsp 파일은 에러가 났다.

어떻게 해결 할 수 있었는지 이야기 하겠습니다. 
혹시 다른 편한 방법이 있었다면 알려주길 바랍니다~!

먼저 

sudo apt-get install libmysql-java를 실행 하였는데 수행되지 않았다.

다른 방법을 통해 jdbc를 설치 하였다. 

가상환경에서 벗어나 현재 실행하고 있는 os를 사용하여 os와 버전에 맞는 JDBC를 다운받았다.
https://dev.mysql.com/downloads/connector/j/

// url 페이지 화면 

mysql-connector-java_8.0.22-1ubuntu20.04_all.deb

그 후 파일을 github을 통해 서버로 보냈다. 

파일을 리눅스 서버에 올긴 후

파일이 존재하는 위치에서 dpkg를 사용해 압축을 풀었다. 
새로운 폴더를 생성하는 방법으로 풀었다. 간혹 바로 실행되고, 파일은 확인 할 수 없었다. 

sudo dpkg-deb -x 파일명.deb folder

cd folder && ls 

mysql-connector-java-8.0.22.jar

jar 파일이 적용될 위치로 넣어주어야한다.  
여기서 중요한점은 파일의 위치이다. 다른 위치에 넣어주었을때 실행이 되지 않았다. 

아까 전 실행파일을 넣어야하는 위치에 폴더 
WEB-INF/lib 로 이동시켜준다. 

sudo mv mysql-connector-java-8.0.22.jar  /var/lib/tomcat9/webapps/ROOT/WEB-INF/lib

( WEB-INF 파일 폴더가 없어서 많이 해맸다. 새로 폴더를 만들어서 사용하였다.  )

옮겼다면, 적용이 잘되었는지 테스트 코드를 사용해서 실행해 보자.

테스트 코드는 이곳에서 참고 하였다. (감사합니다~) https://titis.tistory.com/88

...
    <%
        String DB_URL = "jdbc:mysql://아이피:3306/데이터베이스";
        String DB_USER = "이름";
        String DB_PASSWORD= "암호";
...

전체코드는 /code/dbtest.jsp 파일을 확인바란다. 


간단한 jsp db 연동 확인 코드 이다. 이전에 설정 해 두었던 mysql 아이디와 비밀번호를 넣고 db를 새로 만든 뒤 그 정보를 넣어 
ROOT위치에 dbtest.jsp로 저장한다. 

이전에 실행하였던 자신의 아이피와 8080 포트를 통해 잘동작 하는지 확인한다.

// 동작 화면  

http://ip:8080/dbtest.jsp

잘 실행 된다면 connect ok 
잘 실행 되지 않는다면 error가 나타날것이다. 
자신에 에러를 검색해서 문제를 해결하면된다. 

+ 추가 
    자신이 만든 java class를 jsp이외의 다른 파일에서 실행하고 싶다면.

DB 연결시 많이 사용되는 클래스가 있다. 

db데이터를 직접적으로 갖는 data 클래스와 
연결할때 사용하는 DTO 클래스가 바로 그것이다. 

대부분 이클립스에서의 위치 밖에 나오지 않아 찾는데 어려움이 있었는데, 
필자가 사용한 방법을 정리한다. 

이번에 만든 파일 폴더인 WEB-INF/ 폴더에 classes 파일을 만든다. 
그 후 사용할 java 파일을 복사 이동시킨다. 

sudo cp -.java /var/lib/Tomcat9/ROOT/WEB-INF/classes

이동시킨 파일을 javac 를 사용하여 class 파일로 컴파일 한다. 

sudo apt-get install javac 
sudo javac -.java 

그 후 ls로 class 파일이 생성되었는지 확인 후 rm를 사용해 java 파일을 삭제한다. 

이렇게 생성된 class 파일은 jsp 파일에서 import 해 사용할 수 있디.

여기까지 jsp를 사용하기 위해 tomcat과 mysql을 설치하고 연결했던 작업을 수행하였다. 
이상한 부분이나 바르지 않는 부분이 있다면 알려주시면 감사합니다. 
