---
description: '"Vue.js" 로 만든  간단한 "todo" 앱을 C.F(Cloud Foundry)로 배포하고 웹에서 접근하는 과정을 설명한다.'
---

# 2. 정적 웹페이지 제작

{% hint style="info" %}
데모 사이트 :  [https://cfapp-static.kpaasta.io ](http://cfapp-static.kpaasta.io)
{% endhint %}

### 로컬 환경 빌드

#### &gt; 정적 웹앱\(Web App\)를 을 로컬\(Desktop\)환경에서 개발한다. 

예시를 위해 기존에 만들어진 "TODO" 앱 소스를 Git 에서 받아와서 진행한다.

```
$ git clone https://github.com/jmpark93/cf-static.git
```

{% hint style="info" %}
개발 언어 : Vue.js \(+ Vue CLI, Vuex, Vue Router, Vue Bootstrap, Axios...\)
{% endhint %}

#### &gt; 배포하기 위한 빌드를 수행한다. 

실제 배포할 파일들이 문제없이 ./dist 디렉토리에 생성되는 지 확인한다. 

```text
$ npm install
$ npm run build

$ ls -alF ./dist
...
```

### 배포 수행

#### &gt; C.F. 에 배포할 타켓이 정확하게 설정되어 있는 확인한다. 

```text
$ cf target
api endpoint:   https://api.kpaasta.io
api version:    2.138.0
user:           jmpark93@koscom.co.kr
org:            KOSCOM-DefaultOrg
space:          default
```

#### &gt; C.F. 배포할 때 필요한 설정을 "manifest.yml" 파일로 정의한다. 

staticfile\_buildpack 을 지정하여 사용하면 C.F 가 가지고 있는 staticfile\_buildpack 들 중에 최신 빌드백을 사용하도록 되어 있다.  

{% code title="$ cat ./manifest.yml" %}
```text
applications:
- name: cf-static
  path: ./dist
  buildpacks: 
    - staticfile_buildpack
  memory: 128M
  instances: 1
  routes:
  - route: cfapp-static.kpaasta.io
```
{% endcode %}

#### &gt; C.F 에 배포한다. \( 정상적으로 배포되면 [http or https://cfapp-static.kpaasta.io ](http://cfapp-static.kpaasta.io)으로 확인할 수 있다.\)

```text
$ cf push 
Pushing from manifest to org KOSCOM-DefaultOrg / space default as jmpark93@koscom.co.kr...
Using manifest file /Users/jmpark93/K.PaaS-TA/cf-apps/cf-static/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:         cf-static
  path:         /Users/jmpark93/K.PaaS-TA/cf-apps/cf-static/dist
  buildpacks:
+   staticfile_buildpack
+ instances:    1
+ memory:       128M
  routes:
+   cfapp-static.kpaasta.io

Creating app cf-static...
Mapping routes...
Comparing local files to remote cache...
Packaging files to upload...
Uploading files...
 87.58 KiB / 87.58 KiB [==============================================================================================================================================================] 100.00% 1s

Waiting for API to complete processing files...

Staging app and tracing logs...
   Downloading staticfile_buildpack...
   Downloaded staticfile_buildpack
   Cell 24f5d102-40e0-4319-878b-95d86913dcaf creating container for instance bf84d01b-c59b-4942-931a-e74a77d5cfac
   Cell 24f5d102-40e0-4319-878b-95d86913dcaf successfully created container for instance bf84d01b-c59b-4942-931a-e74a77d5cfac
   Downloading app package...
   Downloaded app package (3.1M)
   -----> Staticfile Buildpack version 1.5.7
   -----> Installing nginx
          Using nginx version 1.17.10
   -----> Installing nginx 1.17.10
          Download [https://buildpacks.cloudfoundry.org/dependencies/nginx-static/nginx-static_1.17.10_linux_x64_cflinuxfs3_f2c99636.tgz]
          See: https://nginx.org/
   -----> Enabling HTTPS redirect
   -----> Root folder /tmp/app
   -----> Copying project files into public
   -----> Configuring nginx
   Uploading droplet...
   Uploading build artifacts cache...
   Uploaded build artifacts cache (2M)
   Uploaded droplet (5.2M)
   Uploading complete
   Cell 24f5d102-40e0-4319-878b-95d86913dcaf stopping instance bf84d01b-c59b-4942-931a-e74a77d5cfac
   Cell 24f5d102-40e0-4319-878b-95d86913dcaf destroying container for instance bf84d01b-c59b-4942-931a-e74a77d5cfac
   Cell 24f5d102-40e0-4319-878b-95d86913dcaf successfully destroyed container for instance bf84d01b-c59b-4942-931a-e74a77d5cfac

Waiting for app to start...

name:              cf-static
requested state:   started
routes:            cfapp-static.kpaasta.io
last uploaded:     Wed 27 May 14:30:16 JST 2020
stack:             cflinuxfs3
buildpacks:        staticfile

type:            web
instances:       1/1
memory usage:    128M
start command:   $HOME/boot.sh
     state     since                  cpu    memory      disk      details
#0   running   2020-05-27T05:30:22Z   0.0%   0 of 128M   0 of 1G   
```

### 실행 환경 설정 

#### &gt; http --&gt; https 로 redirection 하는 설정 : staticfile 정의 및 설

C.F 플랫폼에 앱을 배포하면 http, https 모두 동작하도록 설정되어 있다.   
https 로 강제하고 싶다면 빌드팩에 내장된 nginx 설정을 수정해야하지만 staticfile 작성하여 설정할 수 있다. 

{% code title="$ cat ./dist/Staticfile/" %}
```text
force_https: true
```
{% endcode %}

{% hint style="info" %}
그 외의 다양한 옵션들은 [온라인 가이드](https://docs.cloudfoundry.org/buildpacks/staticfile/index.html)를 참조하기 바란다.
{% endhint %}

