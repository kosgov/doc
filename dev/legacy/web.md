# WEB 개발 및 배포

## 로컬 환경 구성 및 테스트 

정적 웹페이지 과정과 동일하며 API 통신을 위한 부분을 변경하여 개발하였 

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

#### 전체 배포 과정을 단순화 하기 위해 다음과 같은 스크립트를 만들어 사용한다. 

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



