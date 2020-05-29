# 1. 개발환경 구성

## C.F. Cli 설치 

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

#### &gt; Windows 

설치 바이너리 파일를 다운로드하여 설치한다. \(다른 OS 들도 설치 바이너리로 설치 가능하다.\)  

#### 설치 확인 & 로그



## Kubectl 설치

## 

