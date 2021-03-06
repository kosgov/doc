# 4-2. Kubernetes서비스 사용하기

앱 & 서비스 &gt; 서비스  페이지에서 사용자가 생성한 Kubernetes 서비스 우측 대시보드 연결 버튼을 클릭시 별도로 구성된 Kubernetes Dashboard 페이지로 이동합니다.

![](../../.gitbook/assets/image%20%28142%29.png)



Kubernetes Dashboard페이지로 이동시 추가적인 로그인 절차가 수행됩니다. 

![](../../.gitbook/assets/image%20%28152%29.png)

{% hint style="info" %}
**k.PaaS-TA접속 로그인 정보**를 동일하게 입력합니다.
{% endhint %}



Kubernetes Dashboard 최초 로그인 사용자에 대한 인증 절차를 수행합니다. **AUTHORIZE를 선택하여 인증을 수행합니다.**

![](../../.gitbook/assets/image%20%28162%29.png)



\[Intro &gt; Overview\] Kubernetes Dashboard 메인 접속 화면입니다. 사용자 **name,  plan, Resource Quotas**정보를 확인합니다.

![](../../.gitbook/assets/image%20%28151%29.png)

\[Intro &gt; Access\] 사용자 정보 및 Kubectl 사용을 위한 가이드 페이지입니다. Kubectl 설치 가이드 링크에 접근하여 사용자 환경에 맞는 설치를 수행합니다.

![](../../.gitbook/assets/image%20%28146%29.png)

 kubectl 설치한 서버에서 Access 페이지에 나와있는 가이드 절차에 따라서 환경을 구성합니다.

```text
# 2. 환경 변수 설정
root@VM1588746377319:~/git# export CAAS_SERVICE_CLUSTER_NAME="kubernetes"
root@VM1588746377319:~/git# export CAAS_SERVICE_CLUSTER_SERVER="https://api.k8s.kpaasta.io:8443"
root@VM1588746377319:~/git# export CAAS_SERVICE_USER_NAME="4b5a3805-2ea4-4b41-97af-6b49dcea4120-19"
root@VM1588746377319:~/git# export CAAS_SERVICE_CONTEXT_NAME="4b5a3805-2ea4-4b41-97af-6b49dcea4120-19-context"
root@VM1588746377319:~/git# export CAAS_SERVICE_NAMESPACE_NAME="paas-4b5a3805-2ea4-4b41-97af-6b49dcea4120-caas"
root@VM1588746377319:~/git# export CAAS_SERVICE_CREDENTIALS_TOKEN="eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJwYWFzLTRiNWEzODA1LTJlYTQtNGI0MS05N2FmLTZiNDlkY2VhNDEyMC1jYWFzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImNhMzVkMDRlLWU2MDUtNDAyMi1iNGZiLWI3ZDUyN2RkOGI3ZS1raW1tYXJ0ODgtdXNlci10b2tlbi03N3E3ZiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJjYTM1ZDA0ZS1lNjA1LTQwMjItYjRmYi1iN2Q1MjdkZDhiN2Uta2ltbWFydDg4LXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiI4ZWM1ZWRjMy05YTc3LTExZWEtOWZiZS1mMjIwY2RkNzQ0MTIiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6cGFhcy00YjVhMzgwNS0yZWE0LTRiNDEtOTdhZi02YjQ5ZGNlYTQxMjAtY2FhczpjYTM1ZDA0ZS1lNjA1LTQwMjItYjRmYi1iN2Q1MjdkZDhiN2Uta2ltbWFydDg4LXVzZXIifQ.e_oy_SxlzSuGhRzOM1_5PdqHWvqxfuxCrGGv3jtC2X96QOzBz3_CDWdV-TrjKRiGbse34ZWiLJeVJRb74RIAY3y3kB9i5FiKw27GF5em7hJ27z6f2nNoElvsqxnqNJqRj_xMJhAvzJIrn9Ks-QM8Kz98vYka8xpVbT87qn2NXz8gBx8gMw6bH8cDrx_ebCR5B9NYHu1gh1w7EpOXfNzmtkbDe-yEVTVHSwMkz0VAVWb7nsnzeXFH6htIJAzBzDRGpF82BRC7b6HL0CKbXpAaxGWgozEMixHhLSK7AWS13oI6nWMATbih6NUpyse6vcSOGknHX14mWDRNP7FFH-liNQ"

# 3. Cluster 등록
root@VM1588746377319:~/git# kubectl config set-cluster ${CAAS_SERVICE_CLUSTER_NAME} --embed-certs=true --server=${CAAS_SERVICE_CLUSTER_SERVER} --certificate-authority=./cluster-certificate.crt
Cluster "kubernetes" set.

# 4. Credential 등록
root@VM1588746377319:~/git# kubectl config set-credentials ${CAAS_SERVICE_USER_NAME} --token=${CAAS_SERVICE_CREDENTIALS_TOKEN}
User "4b5a3805-2ea4-4b41-97af-6b49dcea4120-19" set.

# 5. Context 설정
root@VM1588746377319:~/git# kubectl config set-context ${CAAS_SERVICE_CONTEXT_NAME} --user=${CAAS_SERVICE_USER_NAME} --namespace=${CAAS_SERVICE_NAMESPACE_NAME} --cluster=${CAAS_SERVICE_CLUSTER_NAME}
Context "4b5a3805-2ea4-4b41-97af-6b49dcea4120-19-context" created.

# 6. Context 사용 설정
root@VM1588746377319:~/git# kubectl config use-context ${CAAS_SERVICE_CONTEXT_NAME}
Switched to context "4b5a3805-2ea4-4b41-97af-6b49dcea4120-19-context".

# 7. Config 구성 확인
root@VM1588746377319:~/git# kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://api.k8s.kpaasta.io:8443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    namespace: paas-4b5a3805-2ea4-4b41-97af-6b49dcea4120-caas
    user: 4b5a3805-2ea4-4b41-97af-6b49dcea4120-19
  name: 4b5a3805-2ea4-4b41-97af-6b49dcea4120-19-context
current-context: 4b5a3805-2ea4-4b41-97af-6b49dcea4120-19-context
kind: Config
preferences: {}
users:
- name: 4b5a3805-2ea4-4b41-97af-6b49dcea4120-19
  user:
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJwYWFzLTRiNWEzODA1LTJlYTQtNGI0MS05N2FmLTZiNDlkY2VhNDEyMC1jYWFzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImNhMzVkMDRlLWU2MDUtNDAyMi1iNGZiLWI3ZDUyN2RkOGI3ZS1raW1tYXJ0ODgtdXNlci10b2tlbi03N3E3ZiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJjYTM1ZDA0ZS1lNjA1LTQwMjItYjRmYi1iN2Q1MjdkZDhiN2Uta2ltbWFydDg4LXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiI4ZWM1ZWRjMy05YTc3LTExZWEtOWZiZS1mMjIwY2RkNzQ0MTIiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6cGFhcy00YjVhMzgwNS0yZWE0LTRiNDEtOTdhZi02YjQ5ZGNlYTQxMjAtY2FhczpjYTM1ZDA0ZS1lNjA1LTQwMjItYjRmYi1iN2Q1MjdkZDhiN2Uta2ltbWFydDg4LXVzZXIifQ.e_oy_SxlzSuGhRzOM1_5PdqHWvqxfuxCrGGv3jtC2X96QOzBz3_CDWdV-TrjKRiGbse34ZWiLJeVJRb74RIAY3y3kB9i5FiKw27GF5em7hJ27z6f2nNoElvsqxnqNJqRj_xMJhAvzJIrn9Ks-QM8Kz98vYka8xpVbT87qn2NXz8gBx8gMw6bH8cDrx_ebCR5B9NYHu1gh1w7EpOXfNzmtkbDe-yEVTVHSwMkz0VAVWb7nsnzeXFH6htIJAzBzDRGpF82BRC7b6HL0CKbXpAaxGWgozEMixHhLSK7AWS13oI6nWMATbih6NUpyse6vcSOGknHX14mWDRNP7FFH-liNQ

```

\[Intro &gt; User Guide\] Kubernetes 유용하게 사용할 수 있는 가이드 페이지입니다. 기능별 대략적인 설명이 기술되어 있고 상세한 가이드 내용은 설명 오른쪽 링크를 통해서 확인 가능합니다.

![](../../.gitbook/assets/image%20%28145%29.png)

{% hint style="info" %}
UserGuide수행 이전에  Role별 권한 정보를 확인합니다. Role에 대한 변경 권한은 서비스 관리자에게 있으며 Init User 사용자는 Role 변경 이후 진행 가능합니다.

* Administrator :
  * Get
  * List
  * Create
  * Delete
  * Update
  * Patch
  * Watch
  * User Management
* Regular User   :
  * Get
  * List
  * Create
  * Delete
  * Update
  * Patch
  * Watch
* Init User           :
  * Get
  * List
  * Watch
{% endhint %}

Kubernetes Dashboard상단 오른쪽 마크를 클릭 User탭에서 사용자별 Role을 수정 할 수 있습니다. 

수정 권한은 관리자만 가능합니다.

![](../../.gitbook/assets/image%20%28138%29.png)



