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

#### 1\) 바인딩 된 상태 그대로 재시작 

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

#### 2\) Java BuildPack 의 "Auto Reconfiguration" 을 사용하지 않고 Runtime 환경 변수를 직접 사용

```text
$ cf env cf-legacy-api
...
System-Provided:
{
 "VCAP_SERVICES": {
  "MySQL Database": [
   {
    "binding_name": null,
    "credentials": {
     "hostname": "proxy-intl.service.kosgov",
     "name": "op_f2702132_560d_4461_b510_efba367905bd",
     "password": "*******",
     "port": "3306",
     "uri": "mysql://*******:*******@proxy-intl.service.kosgov:3306/op_f2702132_560d_4461_b510_efba367905bd",
     "username": "*******"
    },
    "instance_name": "legtodo-default",
    "label": "MySQL Database",
    "name": "legtodo-default",
    "plan": "Free",
    "provider": null,
    "syslog_drain_url": null,
    "tags": [
     "mysql",
     "document"
    ],
    "volume_mounts": []
   }
  ]
 }
}

```

#### Active Profile 로 사용할 application-\[profile\].yml 파일 추가 

{% code title="application-cloud.yml" %}
```text
spring:
  jpa:
    hibernate:
      ddl-auto: update
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: ${vcap.services.mydb.credentials.jdbcUrl}
    username: ${vcap.services.mydb.credentials.username}
    password: ${vcap.services.mydb.credentials.password}
```
{% endcode %}

#### 관리도구 \(phpMyAdmin\) 으로 접속하여 확인할 수 있음 ... 

