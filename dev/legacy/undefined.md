# 이중화 구성

## WEB 서버 이중화 

```text
$ cf scale cf-legacy-web -i 2
Scaling app cf-legacy-web in org KOSCOM-DefaultOrg / space default as jmpark93@koscom.co.kr...
OK
```

앱 상세 화면에서 좀 더 쉽게 조정할 수 있다. \(자원설정상태 &gt; 인스턴스 조정 후 저장\) 

![](../../.gitbook/assets/image%20%28181%29.png)

## WAS 서버 이중화

```text
$ cf scale cf-legacy-api -i 2
Scaling app cf-legacy-api in org KOSCOM-DefaultOrg / space default as jmpark93@koscom.co.kr...
OK
```

![](../../.gitbook/assets/image%20%28180%29.png)

### 기타 

WEB 이중화에 따른 Endpoint 는 처음 정의했던 URL 로 동일하며 Round Robin 형태로 동작하게 된다. WAS 이중화도 마찬가지 방식으로 동작하게 된다. 

Managed DBMS 서비스 \(MySQL\) 은 관리형서비스로 자체적으로 3중화 구성되어 있으므로, WEB &gt; WAS &gt; DB 모두 이중화 되어 있는 상태로 구성되게 된다. 

{% hint style="info" %}
좀 더 우아한 방법 \( Rolling Update, Blue/Green Deploy \) 등은 MSA 웹서비스 제작 과정을 통해서 설몀하도록 한다. 
{% endhint %}

