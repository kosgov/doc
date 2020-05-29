---
description: TODO 앱의 데이터를 관리를 위한 REST API 제공을 위한 WAS 서비스를 개발하고 배포하는 과정을 설명한다.
---

# WAS\(API\) 개발 및 배포

## 로컬\(Desktop\) 개발 & 테스트

{% hint style="info" %}
빌드 및 실행 환경 : Java SE 11+  필요
{% endhint %}

#### &gt; 소스 다운로드  

```
$ git clone https://github.com/jmpark93/cf-legacy-was.git
```

#### &gt; 빌드 및 실행  

```text
$ ./gradlew build

$ java -jar build/libs/todoapi-0.0.1-SNAPSHOT.jar
```

#### &gt; 테스트 \( http://localhost:8080/swagger-ui.html \)

* 전체 API 목록 확인 

![](../../.gitbook/assets/image%20%28170%29.png)

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
별도의 데이터베이스를 제공하지 않아서 기본적으로 제공되는 H2 데이터베이스에 저장된다.  
H2 콘솔 UI : http://localhost:8080/h2-console  
{% endhint %}

## C.F. 배포





