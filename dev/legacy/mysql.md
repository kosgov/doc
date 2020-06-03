# MySQL 서비스 생성 및 연결

{% hint style="info" %}
C.F. CLI 를 이용한 서비스 생성 및 바인딩은 해당 [메뉴얼](../../service/mysql/mysql-sub-2.md)에서 확인하기 바란다.
{% endhint %}

### MySQL 서비스 생성 및 바인딩 \(UI\)

#### 위치 : 마켓플레이스 &gt; 서비스 &gt;  MySQL Database

![](../../.gitbook/assets/image%20%28173%29.png)

#### 서비스 추가 및 바인딩 앱 선택

 앱의 상세 설정 페이지에서도 서비스 바인딩 할 수 있다. 

![](../../.gitbook/assets/image%20%28174%29.png)

#### "앱 & 서비스 &gt; 앱 &gt; 앱 상세" 에서 바인딩 되었는지 확인 

![](../../.gitbook/assets/image%20%28172%29.png)

### WAS\(API\) 서비스 동작 확인

#### SpringBoot 에서 MySQL 서비스 연결 방법

#### 1\) 자동 설정 방식 : 바인딩 된 상태 그대로 재시작 

{% code title="resources/main/application.yml" %}
```text
spring:
#  devtools:
#    livereload:
#      enabled: false
  jpa:
    hibernate:
      ddl-auto: update
  h2:
    console:
      enabled: true
      settings:
        web-allow-others: true
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:todo
    username: todo
    password: koscom
```
{% endcode %}

별도 MySQL 인스턴스를 Profile 에 지정하지 않았지만 "legtodo-default" 인스턴스를 JPA Repository로 등록한 것을 확인할 수 있다.  재시작 한 이후에 뭔가 깔끔하지는 않지만 정상적으로 동작하는 것을 확인 할 수 있다. 

```text
$ cf restart cf-legacy-api
...

$ cf logs cf-legacy-api [ --recent ]
...
~ : ERR Loading class `com.mysql.jdbc.Driver'. This is deprecated. 
    The new driver class is `com.mysql.cj.jdbc.Driver'. 
    The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
...
~ : 'dataSource' bean of type with 'javax.sql.DataSource' 
        reconfigured with 'legtodo-default' bean
...        
~ : Registered driver with driverClassName=com.mysql.jdbc.Driver was not found, 
        trying direct instantiation.
```

#### 2\) 매뉴얼 설정 방식 : 자세한 정보는 [공식 사이트](https://docs.cloudfoundry.org/buildpacks/java/configuring-service-connections/spring-service-bindings.html)에서 확인 

### 테스트 및 확인

인터넷에 오픈되어 있는 URL 을 사용해서 테스트 진행 

```text
--> 생성 
$ curl -X POST "http://legtodo-api.kpaasta.io" -H "accept: */*" \
       -H "Content-Type: application/json" \
       -d "{ \"todoItem\": \"Vue.js 와 API 서버 연결작업 ... \"}"
       
--> 조       
$ curl -X GET "http://legtodo-api.kpaasta.io" \
       -H "accept: application/json"
```

데이터가 위에서 바인딩한 DBMS 에 잘 들어갔는지  확인 

{% hint style="info" %}
DBMS 관리 대쉬보드 : [https://mysqladmin.kpaasta.io/](https://mysqladmin.kpaasta.io/)
{% endhint %}

### 기타

배포 시점에 바로 서비스 바인딩 하기 위해서 manifest.yml 설정 

```text
---
applications:
  - name: cf-legacy-api
    memory: 1G
    instances: 1
    buildpacks:
      - java_buildpack
    path: build/libs/todoapi-0.0.1-SNAPSHOT.jar
    env:
      JBP_CONFIG_OPEN_JDK_JRE: '{ jre: { version: 11.+}}'
    routes:
      - route: legtodo-api.kpaasta.io
      - route: legtodo-api.cf.intl
    services:
      - legtodo-default
```

{% hint style="info" %}
서비스 바인딩 해제 \(서비스 삭제 또는 명시적인 해제\) 시에 데이터는 남아있지만,   
APP 에 할당되는 계정 정보는 초기화 됩니다. 
{% endhint %}

