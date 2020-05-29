---
description: TODO 앱의 데이터를 관리를 위한 REST API 제공을 위한 WAS 서비스를 개발하고 배포하는 과정을 설명한다.
---

# WAS\(API\) 개발 및 배포

### 로컬 환경 개발 

{% hint style="info" %}
빌드 및 실행 환경 : Java SE 11+  필요
{% endhint %}

#### &gt; 소스 다운로드  

```
$ git clone https://github.com/jmpark93/cf-legacy-was.git
```

#### &gt; 빌드 및 실행  

내장 DBMS 인 H2 를 사용하며 접근 콘솔과 데이터베이스명을 기억하자. 

```text
$ ./gradlew build

$ java -jar build/libs/todoapi-0.0.1-SNAPSHOT.jar
...
~~ : H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:todo'
...
```

### 로컬 환경 테스트 

* 전체 API 목록 확인 \( http://localhost:8080/swagger-ui.html \)

![](../../.gitbook/assets/image%20%28172%29.png)

* 데이터 추가 및 확인
  * swagger-ui 를 통해서 입력/조회하여도 되지만 여기서는 CLI 로 처리해본다. 

```text
$ curl -X POST "http://localhost:8080/api/todos" -H "accept: */*" \
       -H "Content-Type: application/json" \
       -d "{ \"todoItem\": \"Vue.js 와 API 서버 연결작업 ... \"}"
       
$ curl -X GET "http://localhost:8080/api/todos" -H "accept: application/json"
{
  "_embedded" : {
    "todos" : [ {
      "id" : 1,
      "todoItem" : "Vue.js 와 API 서버 연결작업 ... ",
      "createdTimeAt" : "2020-05-29T15:22:16.452713",
      "updateTimeAt" : "2020-05-29T15:22:16.45276",
      "done" : false,
      "_links" : {
        "self" : {
          "href" : "http://localhost:8080/api/todos/1"
        },
        "todo" : {
          "href" : "http://localhost:8080/api/todos/1"
        }
      }
    },
  "_links" : {
    "self" : {
      "href" : "http://localhost:8080/api/todos"
    },
    "profile" : {
      "href" : "http://localhost:8080/api/profile/todos"
    }
  },
  "page" : {
    "size" : 20,
    "totalElements" : 2,
    "totalPages" : 1,
    "number" : 0
  }
}       
```

* 데이터베이스에도 정상적으로 입력되어 있는 지 확인한다. 

{% hint style="info" %}
별도의 데이터베이스를 사용하지 않고 테스트 목적으로  H2 데이터베이스를 사용하였다.

H2 콘솔 UI : http://localhost:8080/h2-consol  
DB URL       :  jdbc:h2:mem:todo   
계정            :  todo / koscom
{% endhint %}

![](../../.gitbook/assets/image%20%28169%29.png)

{% hint style="info" %}
C.F. 배포 시에 MySQL 서비스를 사용하면 실행시에 자동으로 MySQL DB 인스턴스에 데이터가 저장된다.
{% endhint %}

### C.F. 배포

#### &gt; 배포 설정 파일 확인

{% code title="$ cat manifest.yml" %}
```text
---
applications:
  - name: cf-legacy-api
    memory: 1G
    instances: 1
    buildpacks:
      - java_buildpack
    path: build/libs/todoapi-0.0.1-SNAPSHOT.jar
    env:
      JBP_CONFIG_OPEN_JDK_JRE: '{ jre: { version: 11.+}}'
    routes:
      - route: legtodo.kpaasta.io
      - route: legtodo.cf.intl
```
{% endcode %}

{% hint style="info" %}
routes, 즉, 접속 URL 을 두개를 지정하였다.   
- 인터넷 URL \( legtodo.kpaasta.io \)은 정상적으로 처리되었는지 확인 용도이다.   
- 내부 URL \( legtodo.cf.intl \)은 내부 서비스들 통신만 가능하다.   
  
테스트 완료 후에 인터넷 URL \( legtodo.kpaasta.io \) 은 삭제한다. 
{% endhint %}

{% hint style="info" %}
JBP\_CONFIG\_OPEN\_JDK\_JRE: '{ jre: { version: 11.+} }' 

지정하지 않으면 "java\_buildpack"에서 지정한 디폴트 런타임\(jre 8\)이 사용되며 실행 시킬 서비스가 지원되지 않는다는 다음과 같은 오류가 발생한다.
{% endhint %}

* JRE 버전 지정하지 않을 경우 오류 메시지 내용 

```text
$ cf logs cf-legacy-api
...
2020-05-29T16:06:39.24+0900 [APP/PROC/WEB/0] ERR Exception in thread "main" java.lang.UnsupportedClassVersionError: com/jmworks/todoapi/TodoapiApplication has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of t
he Java Runtime only recognizes class file versions up to 52.0
...
```

#### &gt; 배포 및 확인 

로컬 데스크톱에서 테스트 했던 것과 동일한 방법으로 정상적으로 처리되는지 확인한다. 

```text
$ cf push 

$ cf logs cf-legacy-api
...
~ : 'cloud' property source added
~ : Reconfiguration enabled
~ : The following profiles are active: cloud
~ : Finished Spring Data repository scanning in 83ms. Found 1 JPA repository interfaces.
~ : Tomcat initialized with port(s): 8080 (http)
...
```

{% hint style="info" %}
로컬과는 다른게 C.F. 에서 조정해 주는 부분들은 다음과 같다. 



자세한 설명은  
{% endhint %}



