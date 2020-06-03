# WEB 개발 및 배포

## 로컬 환경 구성 및 테스트 

정적 웹페이지 과정과 동일하며 API 통신을 위한 부분을 변경하여 개발하였 

```
$ git clone https://github.com/jmpark93/cf-legacy-web.git
```

## C.F. 배포 

#### 최종 배포 대상 디렉토리 구

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



