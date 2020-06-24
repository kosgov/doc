# 1차 - 앱 배포 및 테스트

### Auth/User 서버 배포 및 테스트

로컬 환경 구축 및 테스트에서 진행한 위치에서 다음과 같이 확인 후 C.F 로 배포한다.

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

### 

### Todo 서버 배포 및 테스트

```text

```

### Contents 서버 배포 및 테스트

```text

```

### WEB  서버 배포 및 테스트

```text

```



