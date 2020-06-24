# 실시간 모니터링 및 관리

SpringBoot 으로 개발된 앱들의 실시간 모니터링 및 관리를 위해 우선 BootAdmin을 구성해보자 

앱 위치 : **마켓플레이스 &gt; 앱 &gt;  Boot Admin** 선택 후에 "배포하기" 를 실행한다.

![](../../.gitbook/assets/image%20%28206%29.png)

사용할 수 있는 환경변수는 다음과 같다. 

| 변수이름 | 변수  |  |
| :--- | :--- | :--- |
| JBP\_CONFIG\_OPEN\_JDK\_JRE | { jre: { version: 11.+}} | 필수 |
| ADMIN\_ID |  | 필수 |
| ADMIN\_PWD |  | 필수 |
| SLACK\_WEBHOOK |  | 옵션 |
| SLACK\_CHANNEL |  | 옵션 |
| SLACK\_USERNAME |  | 옵션 |

> JAVA 빌드팩의 디폴트 JRE 버전이 8으로 되어 있으나, 향후 11+ 변경 예정이다.

ADMIN\_ID / ADMIN\_PWD 는 BootAdmin 포털에 접속하기 위한 계정 정보이며,  
SLACK 이외에도 다른 노티\(Notification\)을 설정할 수 있지만, 환경변수는 SLACK 만 외부로 빼놓았다. 

로그인 이후에 접속해보면 다음과 같은 모니터링 및 관리 가능한 화면들을 볼 수 있다. 

![](../../.gitbook/assets/image%20%28204%29.png)

![](../../.gitbook/assets/image%20%28203%29.png)

참고로, BootAdmin 앱을 직접 수정하고 싶은 분들은 다음 Github 리포지토리에서 다운로드하여 사용하면 된다. 

```text
$ git clone https://github.com/jmpark93/cf-bootadmin.git
```



