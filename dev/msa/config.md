# 설정 파일 외부화

![](../../.gitbook/assets/image%20%28182%29.png)

1. 먼저, 위와 같이 관리되도록 'Config Server'를 구성한다. 
2. 로컬 환경으로 구축된 API 서버들을 Cloud Foundry 에 배포한다.  

이 때, 실행시 필요한 설정 정보들을 "Config Server"를 통해서 외부\(Github Repository\)를 통해서 관리되도록 한다.

### Config Server 구성

BootAdmin 과 동일한 방식으로 배포하면 된다. 

앱 위치 : **마켓플레이스 &gt; 앱 &gt;  Config Server** 선택 후에 "배포하기" 를 실행한다.

![](../../.gitbook/assets/image%20%28204%29.png)

사용할 수 있는 환경변수는 다음과 같다. 

| 변수 이름 | 변수 값 | 필수 여부 |
| :--- | :--- | :---: |
| JBP\_CONFIG\_OPEN\_JDK\_JRE | { jre: { version: 11.+}} | O |
| GIT\_URL | https://github.com/jmpark93/cf-msa-config	 | O |
| USE\_BOOT\_ADMIN | true | X |
| BOOT\_ADMIN\_URL | http://msa-bootadmin.cf.intl | X |
| BOOT\_ADMIN\_ID | admin | X |
| BOOT\_ADMIN\_PWD | \*\*\*\*\*\* | X |

Boot Admin 를 통해서 연동하지 않으면 "USE\_BOOT\_ADMIN : false"로 설정하면 된다.   
위에서는 연동할 때 앞에서 정의한 사설 도메인 \(\*.cf.intl\) 으로 연동하였다.  

위에서 정의한 Git URL은 MSA 샘플 앱들의 환경정보를 위해 만든 리포지토리이며,  앱 이름과 환경\(profile\)은 다음과 같다. 

| 서버 | 앱 이름 | 환경 \(Profile\) |
| :--- | :--- | :--- |
| Auth/User | cf-msa-auth | dev |
| Todo | cf-msa-todo | dev  |
| Contents | cf-msa-contents | dev |

 그럼 정상적으로 동작하는지 테스트를 해보자   
Config 서버 URL 뒤에 **"앱 이름"-"환경\(Profile\)"**를 붙여서 호출해 본다. 

```text
$ curl http://msa-config.kpaasta.io/cf-msa-auth-dev

spring:
  cloud:
    discovery:
      client:
        composite-indicator:
          enabled: false
  jpa:
    hibernate:
      ddl-auto: update
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/todo
    username: todo
    password: koscom
    
...
```

참고로, Config Server 앱을 직접 수정하고 싶은 분들은 다음 Github 리포지토리에서 다운로드하여 사용하면 된다. 

```text
$ git clone https://github.com/jmpark93/cf-configserver.git
```



