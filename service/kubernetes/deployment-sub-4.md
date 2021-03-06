# 4-4.deployment & service 사용자 가이드

> 안내 : 본 문서는 사용자가 kubectl 통해 application을 container에 배포하고 서비스를 정의하는 과정을 설명하였습니다.



## **1. 준비작업**

* kubectl설치 & 환경구성

![](../../.gitbook/assets/kubectl.png)

* 사용자 namespace 사전 확인

![](../../.gitbook/assets/namespace.png)

## **2. App Deployment**

```
# sample app deployment test

$ vi hello-app.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-app
  namespace: paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
spec:
  selector:
    matchLabels:
      app: hello
  replicas: 1
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello
        image: "gcr.io/google-samples/hello-app:2.0"

$ kubectl create -f hello-app.yaml
deployment.apps/hello-app created  

$ kubectl get deployments -n paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
hello-app   1/1     1            1           52s    

$ kubectl get pods -n paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
NAME                         READY   STATUS    RESTARTS   AGE
hello-app-5bbc94b9f9-8hss5   1/1     Running   0          62s                    
```

{% hint style="info" %}
사용자 namespace변경 ****
{% endhint %}

## **3. 서비스 정의**

```bash
$ vi hello-app-svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-service
  namespace: paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
  labels:
    app: hello
spec:
  type: ClusterIP
  selector:
    app: hello
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
    
$ kubectl create -f hello-app-svc.yaml
service/hello-service created   

$ kubectl get svc -n paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
hello-service   ClusterIP   10.100.200.230   <none>        80/TCP    2s 
```

{% hint style="info" %}
서비스 port 정의 시 80, 443만 허용
{% endhint %}



