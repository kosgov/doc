# 설정 파일 외부화

![](../../.gitbook/assets/image%20%28182%29.png)

1. 먼저, 위와 같이 관리되도록 'Config Server'를 구성한다. 
2. 로컬 환경으로 구축된 API 서버들을 Cloud Foundry 에 배포한다.  

이 때, 실행시 필요한 설정 정보들을 "Config Server"를 통해서 외부\(Github Repository\)를 통해서 관리되도록 한다.

### Config Server 구성

BootAdmin 과 동일한 방식으로 배포하면 된다. 

앱 위치 : **마켓플레이스 &gt; 앱 &gt;  Boot Admin** 선택 후에 "배포하기" 를 실행한다.

![](../../.gitbook/assets/image%20%28206%29.png)

