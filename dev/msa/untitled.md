# 앱 배포 및 테스트

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
```

참고로, 

### Todo 서버 배포 및 테스트

```text

```

```text

```

### Contents 서버 배포 및 테스트

### WEB  서버 배포 및 테스트



