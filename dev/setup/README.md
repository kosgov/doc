---
description: 개발 / 구축 / 운영 단계에서 가장 많이 사용되는 CLI(Command Line Interface) 설치과정입니다.
---

# 1. 개발환경 구성

## C.F. Cli 설치 

{% hint style="info" %}
최신 버전\(6.50 이상 ~\) 사용하여 배포한 앱 로그 확인 시\("cf logs ..."\) 과도한 접속 요청이 발생합니다. 6.49 ~ 이하 버전을  사용하시기를 권장합니다. 

v6.49.0 : [https://github.com/cloudfoundry/cli/releases/tag/v6.49.0](https://github.com/cloudfoundry/cli/releases/tag/v6.49.0)
{% endhint %}

![](../../.gitbook/assets/image%20%28221%29.png)

#### &gt; Linux : Debian & Ubuntu 계열 

```
$ wget -O cf-cli-installer_6.49.0_x86-64.deb \
    https://packages.cloudfoundry.org/stable?release=debian64&version=6.49.0&source=github-rel

$ sudo dpkg -i cf-cli-installer_6.49.0_x86-64.deb 
```

#### &gt; Linux : Enterprise Linux & Fedora/RHEL/CentOS 계열 

```bash
$ wget -O cf-cli-installer_6.49.0_x86-64.rpm \
    https://packages.cloudfoundry.org/stable?release=redhat64&version=6.49.0&source=github-rel

$ sudo rpm -ivh cf-cli-installer_6.49.0_x86-64.rpm
```

#### &gt; Mac OS X  [64 bit](https://packages.cloudfoundry.org/stable?release=macosx64-binary&version=6.49.0&source=github-rel) \(pkg\)

설치 바이너리 파일를 다운로드하여 설치한다.  

```bash
$ wget -O cf-cli-installer_6.49.0_x86-64.pkg \
    https://packages.cloudfoundry.org/stable?release=macosx64&version=6.49.0&source=github-rel
```

#### &gt; Windows [64 bit ](https://packages.cloudfoundry.org/stable?release=windows64&version=6.49.0&source=github-rel)/ [32 bit](https://packages.cloudfoundry.org/stable?release=windows32&version=6.49.0&source=github-rel)

설치 바이너리 파일를 다운로드하여 설치한다.  

#### &gt; 설치 확인 & 로그

```bash
$ cf version
cf version 6.49.0+d0dfa93bb.2020-01-07

$ cf --help 

$ cf login -a https://api.kpaasta.kr 
```

## Kubectl 설치

## 

