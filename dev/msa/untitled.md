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

nginx/vue.js 앱에서 이제 나머지 3개의 API 서버들로 분기가 되도록 설정이 필요하다. 

다음과 같이 nginx 설정에서 해당 서버의 url 을 보고 분기처리 되도록 설정한다. 

{% code title="./conf/nginx.conf" %}
```text
    ...
    location ^~ /authapi/ {
        rewrite ^/authapi/(.*)$ /api/auth/$1 break;
        proxy_pass {{env "BACKEND_AUTHAPI"}};
    }

    location ^~ /userapi/ {
        rewrite ^/userapi/(.*)$ /api/user/$1 break;
        proxy_pass {{env "BACKEND_USERAPI"}};
    }

    location ^~ /todoapi/ {
        rewrite ^/todoapi/(.*)$ /api/todos/$1 break;
        proxy_pass {{env "BACKEND_TODOAPI"}};
    }

    location ^~ /bookapi/ {
        rewrite ^/bookapi/(.*)$ /api/book/$1 break;
        proxy_pass {{env "BACKEND_BOOKAPI"}};
    }
    ...
```
{% endcode %}

위에 정의된 환경변수들을 처리할 수 있도록 buildpack.yml 에도 정의한다. 

{% code title="./conf/buildpack.yml" %}
```text
---
nginx:
  version: stable
  plaintext_env_vars:
    - "BACKEND_AUTHAPI"
    - "BACKEND_USERAPI"
    - "BACKEND_TODOAPI"
    - "BACKEND_BOOKAPI"
```
{% endcode %}

마지막으로 배포하기 위한 manifest.yml 파일을 수정한고, 미리 만들어 놓은 배포 스크립트\(deploy.sh\)를 실행한다. 

```text
$ cat ./manifest.yml

applications:
- name: cf-msa-web
  path: ./dist
  buildpacks: 
    - nginx_buildpack
  memory: 256MB
  disk: 512MB
  instances: 1
  env:
    BACKEND_AUTHAPI: 'http://msa-auth.cf.intl'
    BACKEND_USERAPI: 'http://msa-auth.cf.intl'
    BACKEND_TODOAPI: 'http://msa-todo.cf.intl'
    BACKEND_BOOKAPI: 'http://msa-contents.cf.intl'
  routes:
  - route: msa-web.kpaasta.io
  
$ chmod +x ./deploy.sh

$ ./deploy.sh
...   
```

브라우저에서 해당 URL \(https://msa-web.kpaasta.io\) 로 접근해서 테스트 해본다.   
각자 자신의 계정을 만들어서 사용하면 된다. 

### 정리 

WEB 서버 \(nginx/vue.js\) 서버를 제외한 나머지 API 서버들은 사설 도메인으로 Private 하게 통신하면 되므로, 확인 테스트를 위해 등록해 놓은 Public 도메인을 모두 제거한다. 

K.PaaS-TA 포털에서 삭제해도 되고 다음과 같이 C.F. CLI 에서 작업하여도 된다. 

```text
$ cf unmap-route cf-msa-auth kpaasta.io --hostname msa-auth

$ cf unmap-route cf-msa-todo kpaasta.io --hostname msa-todo

$ cf unmap-route cf-msa-contents kpaasta.io --hostname msa-todo msa-contents
```



