# 참고 사항

#### C.F.\(Cloud Foundry\) 로그인 절차   

```
$ cf login -a https://api.kpaasta.io
API endpoint: https://api.kpaasta.io

Email: jmpark93@koscom.co.kr

Password: ******
Authenticating... OK

Select an org: 
1. KOSCOM-DefaultOrg
2. ...

Org (enter to skip): 1
Targeted org KOSCOM-DefaultOrg

Targeted space default

API endpoint: https://api.kpaasta.io (API version: 2.138.0) 
User: jmpark93@koscom.co.kr 
Org: KOSCOM-DefaultOrg 
Space: default 
```

#### C.F CLI 한글 표시 --&gt; 영문으로 변경

```text
$ cf config --locale en-US
```

#### 사용 가능한 빌드팩 확인 

```text
$ cf buildpacks | grep staticfile
staticfile_buildpack            1          true      false    staticfile-buildpack-cflinuxfs3-v1.5.7.zip     cflinuxfs3
staticfile_buildpack-v1-5-7     21         true      false    staticfile-buildpack-cflinuxfs3-v1.5.7.zip     cflinuxfs3
```



