# 앱 배포 및 테스트

### Auth/User 서버 배포 및 테스트

로컬 환경 구축 및 테스트에서 진행한 위치에서 다음과 같이 확인 후 C.F 로 배포한다.

* 요구사항 : **MySQL 서비스 생성 \( 이 : msa-auth\)**

```text
$ pwd
.../msa/cf-msa-auth

$ cat manifest.yml
---
applications:
  - name: cf-msa-auth
    memory: 1G
    instances: 1
    buildpacks:
      - java_buildpack
    path: ./build/libs/auth-0.0.1-SNAPSHOT.jar
    env:
      JBP_CONFIG_OPEN_JDK_JRE: '{ jre: { version: 11.+}}'
      SPRING_PROFILES_ACTIVE: dev
    services:
      - msa-auth
    routes:
      - route: msa-auth.kpaasta.io
      - route: msa-auth.cf.intl
      
$ cf push 
Pushing from manifest to org KOSCOM-DefaultOrg / space default as jmpark93@koscom.co.kr...
Using manifest file /Users/jmpark93/temp/msa/cf-msa-auth/manifest.yml
Getting app info...
Updating app with these attributes...
  name:                cf-msa-auth
  path:                ... (생략)... build/libs/auth-0.0.1-SNAPSHOT.jar
  buildpacks:
    java_buildpack
  command:             ... (생략) ... 
  health check type:   port
  instances:           1
  memory:              1G
  stack:               cflinuxfs3
  services:
    msa-auth
  env:
    JBP_CONFIG_OPEN_JDK_JRE
    SPRING_PROFILES_ACTIVE
  routes:
    msa-auth.cf.intl
    msa-auth.kpaasta.io
```

참고로, Cloud Server 연동 관련된 내역은 "SPRING\_PROFILE\_ACTIVE: dev" 이며,   
해당 내역은 다음 파일에서 확인할 수 있다. 

{% code title=".../src/main/resources/bootstrap.yml" %}
```text
server:
...

spring:
  application:
    name: cf-msa-auth
  profiles:
    active: local

---
spring:
  profiles: dev

  cloud:
    config:
      uri: http://msa-config.cf.intl

---
spring:
  profiles: local

  cloud:
    config:
      enabled: false
...
```
{% endcode %}

로컬 환경에서와 마찬가지로 인증서버가 정상적으로 동작하는지 확인한다. 

![](../../.gitbook/assets/image%20%28206%29.png)

### Todo 서버 배포 및 테스트

* 요구사항 : **MySQL 서비스 생성 \( 이 : msa-todo\)**

```text
$ pwd
.../msa/cf-msa-auth

$ cat manifest.yml
---
applications:
  - name: cf-msa-todo
    memory: 1G
    instances: 1
    buildpacks:
      - java_buildpack
    path: ./build/libs/todoapi-0.0.1-SNAPSHOT.jar
    env:
      JBP_CONFIG_OPEN_JDK_JRE: '{ jre: { version: 11.+}}'
      SPRING_PROFILES_ACTIVE: dev
    services:
      - msa-auth  
    routes:
      - route: msa-todo.kpaasta.io
      - route: msa-todo.cf.intl
      
$ cf push 
...
```

Auth/User 서버에서 생성된 "access\_token"으로 Todo 서버도 정상적으로 동작하는지 확인한다. 

![](../../.gitbook/assets/image%20%28211%29.png)

### Contents 서버 배포 및 테스트

마지막 API 서버도 마찬가지로 배포하고 테스트 한다. 

```text
$ pwd
.../msa/cf-msa-contents

$ cat manifest.yml
---
applications:
  - name: cf-msa-contents
    memory: 1G
    instances: 1
    buildpacks:
      - java_buildpack
    path: ./build/libs/contents-0.0.1-SNAPSHOT.war
    env:
      JBP_CONFIG_OPEN_JDK_JRE: '{ jre: { version: 11.+}}'
      SPRING_PROFILES_ACTIVE: dev
    routes:
      - route: msa-contents.kpaasta.io
      - route: msa-contents.cf.intl
      
$ cf push 
...
```

Auth/User 서버에서 생성된 "access\_token"으로 정상적으로 동작하는 다음과 같이 확인해 볼 수 있다. 

![](../../.gitbook/assets/image%20%28217%29.png)

### WEB  서버 배포 및 테스트

```text

```



