## EC2 톰캣 서버 셋팅

EC2 생성은 [프레시안님 글 참조](http://pyrasis.com/aws.html)

EC2 Amazon Linux- java 1.8, tomcat8, mysql 기준



#### **Putty 접속**

puttyGen 이용 private Key 생성

푸티접속 법 : [http://cafe.naver.com/jpvsme/6](http://cafe.naver.com/jpvsme/6)




#### **AWS 웹상에서 Security Group 설정**

80, 8080, 3306 포트 열기




#### **자바 업데이트**

> sudo yum remove java-1.7.0-openjdk
>
> sudo yum install java-1.8.0




#### **톰캣, 아파치, mysql 설치**

> sudo yum update -y
>
> sudo yum install -y httpd24 mysql55-server tomcat8-webapps tomcat8-docs-webapp tomcat8-admin-webapps




#### **명령어 정리**

서버실행시  자동 시작

> sudo chkconfig tomcat8 on

서비스 재시작

> sudo service mysqld restart

경로 찾기

> sudo find / -name '*.pl' (전체 디렉)
>
> sudo find / -name 'et*' -type d (전체 디렉 폴더 찾기)




#### **톰캣 설정**

다음 명령어로 톰캣 설정 경로 찾기

> sudo find / -name 'tomcat-users.xml'

해당경로에서 다음 주석 해제 및 정보설정

- **tomcat-users.xml**

```xml
<tomcat-users ...>
  <role rolename="admin"/>
  <role rolename="admin-gui"/>
  <!-- <role rolename="admin-script"/> -->
  <role rolename="manager"/>
  <role rolename="manager-gui"/>
  <!-- <role rolename="manager-script"/> -->
  <!-- <role rolename="manager-jmx"/> -->
  <!-- <role rolename="manager-status"/> -->
  <user name="admin" password="설정" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" />
</tomcat-users>
```

저장후 서버 재실행

> sudo service tomcat8 restart




#### **톰캣 관리자로 접속 후 WAR 파일 Deploy**

> http://주소:8080/manager/html

war 파일이름을 ROOT.war로 지으면,

context path가 /로 자동 설정된다.
