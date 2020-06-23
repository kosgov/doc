# 로컬 환경 구축 및 테스트

 MSA 제작 테스트 진행을 위해 필요한 앱 목록은 다음과 같다.   
\* Port 는 로컬 테스트를 위해 서로 겹치치 않게 정의하였다. 

| 서버 | Github URL | Port | 용도 |
| :--- | :--- | :--- | :--- |
| WEB | [jmpark93/cf-msa-web](https://github.com/jmpark93/cf-msa-web.git) | 8080 | nginx, vue.js |
| Auth/User | [jmpark93/cf-msa-auth](https://github.com/jmpark93/cf-msa-auth.git) | 8081 | 인증 및 사용자 관리 API |
| Todo | [jmpark93/cf-msa-todo](https://github.com/jmpark93/cf-msa-todo.git) | 8082 | TODO 앱 API  |
| Contents | [jmpark93/cf-msa-contents](https://github.com/jmpark93/cf-msa-contents.git) | 8083 | Dummy API |

### Auth/User 앱 

로컬 테스트를 위한 Docker 로 별도 DBMS \(MySQL\)를 구성하였다.   
\*  resources/bootstrap.yml 에 정의된 대로 생성하거나 자신의 환경에 맞게 수정하여 사용하면 된다.

Docker 가 설치된 환경이면 간단하게 다음과 같이 수행하면 된다.

```text
$ git clone https://github.com/jmpark93/cf-msa-auth

$ cd cf-msa-auth

$ docker-compose up -d 

// 맥북에서는 ... 
$ mysql -h 127.0.0.1 --user=auth --password auth
...
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| auth               |
+--------------------+
2 rows in set (0.05 sec)
```

실제 Auth/User API 서버를 띄워보자.  
\* 별도의 IDE 도구들을 활용하셔서 실행해도 된다. \( 참고로 IntelliJ 로 개발되었다.\)

로컬에서 테스트 하므로 Active Profile : local 로 실행시킨다.

```text
$ ./gradlew build

$ ls -alF build/libs
total 117368
drwxr-xr-x  3 jmpark93  staff        96  6 23 16:38 ./
drwxr-xr-x  9 jmpark93  staff       288  6 23 16:38 ../
-rw-r--r--  1 jmpark93  staff  59731995  6 23 16:38 auth-0.0.1-SNAPSHOT.jar

$ java -jar -Dspring.profiles.active=local build/libs/auth-0.0.1-SNAPSHOT.jar
...
... The following profiles are active: local
```

OAuth2 \(GrantType : Password\) 로 JWT 토큰을 정상적으로 가져오는지 확인해 보자.  
\(여기서는 Postman 을 사용하여 테스트 하였다.\)

![](../../.gitbook/assets/image%20%28196%29.png)

Basic Auth : Username --&gt;  client id   
                      Password --&gt; client secret 값을 넣는다   
\( 참고  : resources/bootstrap.yml  \)

![](../../.gitbook/assets/image%20%28194%29.png)

grant\_type : password 로 지정하고 계정은 앱이 실행되면서 자동으로 만들어진 테스트 계정이다.   
나중에, 직접 등록한 계정으로 테스트할 경우에 수정해서 실행하면 된다. 

다음과 같이 정상적인 값을 가져오면 성공이다. 

![](../../.gitbook/assets/image%20%28198%29.png)

