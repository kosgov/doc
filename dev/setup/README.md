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

* 패키지 리포지토리 설정 

```
$ wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add -
$ echo "deb https://packages.cloudfoundry.org/debian stable main" | sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list

$ sudo apt-get update
```

* C.F. CLI 설치 

```bash
$ sudo apt-get install cf-cli
```

#### &gt; Linux : Enterprise Linux & Fedora/RHEL/CentOS 계열 

* 패키지 리포지토리 설정 

```bash
$ sudo wget -O /etc/yum.repos.d/cloudfoundry-cli.repo https://packages.cloudfoundry.org/fedora/cloudfoundry-cli.repo
```

* C.F. CLI 설치 

```bash
$ sudo yum install cf-cli
```

#### &gt; Mac OS X \( Homebrew 로 설치 \)

```bash
$ brew install cloudfoundry/tap/cf-cli
```

#### &gt; Windows 64 bit

설치 바이너리 파일를 다운로드하여 설치한다. \(다른 OS 들도 설치 바이너리로 설치 가능하다.\)  

{% hint style="info" %}
설치 파일 위치 : 고객센터 &gt; 자료실 \( [실행 바이너리](http://kr.object.gov-ncloudstorage.com/kpaasta-comm/cf-cli_6.51.0_winx64_20200529033325.zip) ,  [설치 파일파](http://kr.object.gov-ncloudstorage.com/kpaasta-comm/cf-cli-installer_6.51.0_winx64_20200529033353.zip)  \) 
{% endhint %}

#### &gt; 설치 확인 & 로그

```bash
$ cf --help 

$ cf login -a https://api.kpaasta.io 
```

{% hint style="info" %}
Windows 를 포함한 C.F. CLI 설치 및 실행 바이너리 위치 

 https://github.com/cloudfoundry/cli\#installers-and-compressed-binaries
{% endhint %}

## Kubectl 설치

## 
