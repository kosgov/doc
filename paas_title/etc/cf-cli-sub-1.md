# 5.1 CF-CLI로 PaaS 사용하기 1

### 서비스 로그인과 영역 생성하기

> 안내 : CF-CLI 툴을 이용하여 손쉽게 앱 배포, 서비스 생성, 바인딩하는 방법을 알아보겠습니다.

CF-CLI 는 클라우드 파운더리에서 제공하는 CLI \(common language infrastructure\) 툴 입니다. CF CLI 를 통해 PaaS의 앱 배포 부터 서비스 생성, 앱과 서비스의 바인딩 작업까지 실행할 수 있습니다. CF-CLI는 아래 다운로드 링크 사이트에 접속해서 설치 가능합니다.

CF-CLI 다운로드 하기 : [https://docs.cloudfoundry.org/cf-cli/install-go-cli.html](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

본 가이드는 CF-CLI를 통해 PaaS 기능의 전반적인 사용 방법에 대해 안내하며, 서비스 생성부터 앱 배포까지에 대한 가이드를 하고 있습니다.

### **CF 로그인 하기**

> 안내 : 본 가이드는 cf-cli 와 코스콤 클라우드 서비스의 PaaS 이용자가 사용 가능한 기능힙니다. 먼저 코스콤 클라우스 서비스에 가입 후 cf-cli를 설치 하시고 가이드에 맞춰 사용하시면 됩니다.

CF-CLI에서 PaaS 서비스를 사용하기 위해서 CF-CLI에 로그인 할 수 있어야 합니다. CF를 통하여 로그인 하는 방법에 대해 알아보도록 하겠습니다.

![](../../.gitbook/assets/image%20%2852%29.png)

" cf login -a 링크 주소 " 명령어를 통해 코스콤 클라우드 PaaS 서비스로 접속을 합니다. 링크주소는 [https://api.kpaasta.cloud](https://api.kpaasta.cloud/) 로 작성하시면 됩니다.

![](../../.gitbook/assets/image%20%2860%29.png)

명령어를 입력하면 사용자의 아이디와 비밀번호를 입력할 수 있는 로그인 화면이 생성되며 아이디와 비밀번호를 입력 후 엔터를 통해 로그인을 합니다.

CF-CLI를 통해 로그인에 성공하였으면 사용자의 조직과 영역을 선택 할 수 있도록 나옵니다. \( 단일인 경우 자동으로 선택 됩니다. \)

다음으로 영역 생성에 대해 알아보도록 하겠습니다.

### **CF 영역 생성**

먼저 포탈에서 사용하는 PaaS 서비스 또한 서비스 생성 및 앱 배포를 위해서 영역을 생성 하듯이 먼저 생성을 생성합니다. \(영역이 이미 있는 경우 상관 없습니다.\)영역 생성은 'cf create-space 영역명' 을 통해 생성 할 수 있습니다.

![](../../.gitbook/assets/image%20%2858%29.png)

영역 생성이 완료 되면 해당 영역으로 이동합니다.

![](../../.gitbook/assets/image%20%2842%29.png)

영역만 이동시 "cf target -s 영역명" 명령어를 통해 이동할 수 있으며, 조직도 이동할 경우 "cf target -o 조직명 -s 영역명" 명령어를 통해 이동 할 수 있습니다.

추후 해당 영역의 사용량 조회 시 "cf space 영역명" 을 통해 조회 할 수 있습니다.

![](../../.gitbook/assets/image%20%2839%29.png)

