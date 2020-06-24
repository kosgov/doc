# 실시간 모니터링 및 관리

SpringBoot 으로 개발된 앱들의 실시간 모니터링 및 관리를 위해 우선 BootAdmin을 구성해보자 

앱 위치 : 마켓플레이스 &gt; 앱 &gt;  Boot Admin 선택 후에 "배포하기" 실

![](../../.gitbook/assets/image%20%28203%29.png)

사

| 변수이름 | 변수  |  |
| :--- | :--- | :--- |
| JBP\_CONFIG\_OPEN\_JDK\_JRE | { jre: { version: 11.+}} | 필수 |
| ADMIN\_ID |  | 필수 |
| ADMIN\_PWD |  | 필수 |
| SLACK\_WEBHOOK |  | 옵션 |
| SLACK\_CHANNEL |  | 옵션 |
| SLACK\_USERNAME |  | 옵 |

> JAVA 빌드팩의 디폴트 JRE 버전이 8으로 되어 있으나, 향후 11+ 변경 예정이다.

참고로, BootAdmin 앱을 직접 수정하고 싶은 분들은 다음 Github 리포지토리에서 다운로드하여 사용하면 된다.

```text
$ git clone https://github.com/jmpark93/cf-bootadmin.git
```



