---
description: TODO 앱의 데이터를 관리를 위한 REST API 제공을 위한 WAS 서비스를 개발하고 배포하는 과정을 설명한다.
---

# WAS\(API\) 개발 및 배포

## 로컬\(Desktop\) 개발 & 테스트

{% hint style="info" %}
빌드 및 실행 환경 : Java SE 11+  필요
{% endhint %}

#### &gt; 소스 다운로 

```
$ git clone https://github.com/jmpark93/cf-legacy-was.git
```

#### &gt; 빌드 및 실

```text
$ ./gradlew build

$ java -jar build/libs/todoapi-0.0.1-SNAPSHOT.jar
```

#### &gt; 테스트 \( http://localhost:8080/swagger-ui.html \)

* 전체 API 목록 확인 
* 데이터 추가

## C.F. 배포





