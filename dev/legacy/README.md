---
description: 현재 많이 사용하는 WEB > WAS > DB 구조의 웹 어플리케이션 제작 방법에 대해서 설명한다.
---

# 3. 전통적인 웹앱 제작

## 전체 구성도  

앞 장에서 작성한 "정적 웹페이지 \(TODO 앱\)"를 좀 더 확장하여 만들어 보겠습니다

![](../../.gitbook/assets/image%20%28179%29.png)

{% hint style="info" %}
백엔드 저장소로 사용되는 데이터베이스는 Managed Service DB\(MySQL\) 며,  
모든 데이터는 WAS\(API\) 서버를 통해서 관리되고, API / MySQL 서버는 사설 네트워크로만  접근되도록 한다.  
{% endhint %}

### 플랫폼\(PaaS\) 주요 제공 내역 

* APP 단일 엔드포인트 : 이중화
* APP 간의 내부 통신 : WEB --&gt; WAS\(API\) 통신 
* Managed 서비스 사용 : MySQL  



