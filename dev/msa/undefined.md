# 로컬 환경 구축 및 테스트

 MSA 제작 테스트 진행을 위해 필요한 앱 목록은 다음과 같다. 

| 서버 | Github URL | 용도 |
| :--- | :--- | :--- |
| WEB | [jmpark93/cf-msa-web](https://github.com/jmpark93/cf-msa-web.git) | nginx, vue.js |
| Auth/User | [jmpark93/cf-msa-auth](https://github.com/jmpark93/cf-msa-auth.git) | 인증 및 사용자 관리 API |
| Todo | [jmpark93/cf-msa-todo](https://github.com/jmpark93/cf-msa-todo.git) | TODO 앱 API  |
| Contents | [jmpark93/cf-msa-contents](https://github.com/jmpark93/cf-msa-contents.git) | Dummy API |

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

```text
$ 
```

