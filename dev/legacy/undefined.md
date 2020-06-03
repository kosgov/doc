# 이중화 구성

## WEB 서버 이중화 

```text
$ cf scale cf-legacy-web -i 2
Scaling app cf-legacy-web in org KOSCOM-DefaultOrg / space default as jmpark93@koscom.co.kr...
OK
```

앱 상세 화면에서 좀 더 쉽게 조정할 수 있다. \(자원설정상태 &gt; 인스턴스 조정 후 저장\) 

![](../../.gitbook/assets/image%20%28180%29.png)

## WAS 서버 이중화



좀 더 우아한 방법 \( Rolling Update, Blue/Green Deploy \) 등은 MSA 웹서비스 제작 과정을 통해서 설몀하도록 한다. 



