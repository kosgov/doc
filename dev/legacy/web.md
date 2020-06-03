# WEB 개발 및 배포

## 로컬 환경 구성 및 테스트 

[정적 웹페이지 과정](../static/)과 동일하며 API 통신을 위한 부분을 변경하여 개발하였다. 

```
$ git clone https://github.com/jmpark93/cf-legacy-web.git
```

## C.F. 배포 

#### 최종 배포 대상 디렉토리 구조는 다음과 같다. 

```bash
$ tree -L 2 ./dist 
./dist
├── buildpack.yml
├── mime.types
├── nginx.conf
└── public
    ├── css
    ├── favicon.ico
    ├── img
    ├── index.html
    └── js
...
manifest.yml    
deploy.sh
```

{% hint style="info" %}
"public" 디렉토리가 실제 "npm run build" 로 생성되는 디렉토리이다.   
\(기존 ./dist 디렉토리\)
{% endhint %}

#### 먼저, 전체 배포 과정을 단순화 하기 위해 다음과 같은 스크립트를 만들어 사용한다. 

{% code title="./deploy.sh" %}
```bash
#!/bin/bash

npm run build

mkdir -p dist/public
mv dist/* dist/public 
cp conf/* dist/

cf push
```
{% endcode %}

#### 배포 설정 파일 \(빌드 팩 : nginx\_buildpack\)

원하는 형태의 구성\( API 접근시 WEB을 통해 접근 등\)을 하려면 nginx\_buildpack 을 사용하는 것을 권장한다.  

{% code title="./manifest.yml" %}
```bash
applications:
- name: cf-legacy-web
  path: ./dist
  buildpacks: 
    - nginx_buildpack
  memory: 128M
  instances: 1
  env:
    BACKEND_API: 'http://legtodo-api.cf.intl'
  routes:
  - route: legtodo-web.kpaasta.io
```
{% endcode %}

환경 변수로 정의된 "BACKEND\_API"는 nginx.conf 파일에 들어가 있는 환경 변수 있이며, API 서버의 내부 URL 이다. 

#### Nginx 설정 - 버전 지

빌드 팩 내에 두가지 버전이 존재하며 다음과 같다.

* mainline \(default\) : 1.17.10
* stable : 1.18.0

{% code title="./conf/buildpack.yml" %}
```bash
---
nginx:
  version: stable
  plaintext_env_vars:
    - "BACKEND_API"
```
{% endcode %}

"BACKEND\_API" 를 여기서 정의해 주어야 nginx.conf 파일에서 사용할 수 있고, manifest.yml 안에 실제 값을 줄 수 있다. 

#### Nginx 설정 파일 - nginx.conf

기존 nginx 에서 사용하는 모든 설정 들을 그대로 사용한다. 

{% code title="" %}
```bash
worker_processes 1;
daemon off;

error_log stderr;
events { worker_connections 1024; }

http {
  charset utf-8;
  log_format cloudfoundry 'NginxLog "$request" $status $body_bytes_sent';
  access_log /dev/stdout cloudfoundry;
  default_type application/octet-stream;
  include mime.types;
  sendfile on;

  tcp_nopush on;
  keepalive_timeout 30;
  port_in_redirect off; # Ensure that redirects don't include the internal container PORT - 8080

  server {
    listen {{port}};

    root public;
    index index.html index.htm Default.htm;

    if ($http_x_forwarded_proto != "https") {
      return 301 https://$host$request_uri;
    }

    location / {      
      try_files $uri $uri/ /index.html;
    }
    
    location ^~ /todoapi/ {
        rewrite ^/todoapi/(.*)$ /$1 break;
        proxy_pass {{env "BACKEND_API"}};
    }
  }
}
```
{% endcode %}

#### 정리작업

정상적으로 잘 처리가 되었다면 API 서버는 더 이상 인터넷으로 접근할 필요가 없다. manifest.yml 에서도 삭제하고 실행환경에서도 삭제한다. 

```bash
$ cf unmap-route cf-legacy-api kpaasta.io --hostname legtodo-api 
```



