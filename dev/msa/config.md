# 설정 파일 외부화

![](../../.gitbook/assets/image%20%28182%29.png)

1. 먼저, 위와 같이 관리되도록 'Config Server'를 구성한다. 
2. 로컬 환경으로 구축된 API 서버들을 Cloud Foundry 에 배포한다.  

이 때, 실행시 필요한 설정 정보들을 "Config Server"를 통해서 외부\(Github Repository\)를 통해서 관리되도록 한다.

### Config Server 구성

BootAdmin 과 동일한 방식으로 배포하면 된다. 

앱 위치 : **마켓플레이스 &gt; 앱 &gt;  Config Server** 선택 후에 "배포하기" 를 실행한다.



| 변수 이름 | 변수 값 | 필수 여부 |
| :--- | :--- | :--- |
| JBP\_CONFIG\_OPEN\_JDK\_JRE | { jre: { version: 11.+}} | 필수 |
| GIT\_URL | [https://github.com/jmpark93/cf-msa-config	](https://github.com/jmpark93/cf-msa-config	) | 필수 |
| USE\_BOOT\_ADMIN | 필수 |  |
| BOOT\_ADMIN\_URL | [http://msa-bootadmin.cf.intl	](http://msa-bootadmin.cf.intl	) | 옵션 |
| BOOT\_ADMIN\_ID |  | 옵션 |
| BOOT\_ADMIN\_PWD |  | 옵 |



