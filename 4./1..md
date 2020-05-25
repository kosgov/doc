# 4-5.ingress 사용자 가이드

> 안내 :  Ingress는 클러스터 외부의 요청을 Ingress 리소스에 정의된 규칙에 따라 클러스터 내부의 서비스로 연결해줍니다.  PaaS-TA에서는 호스트에 따른 라우팅 방법만 지원됩니다.

## **1.Ingress 정의 \(deployment & service 진행 이후\)**

```
$ vi hello-app-ingress.yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  name: hello-ingress
  namespace: paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
spec:
  rules:
  - host: hello.k8s.kpaasta.io
    http:
      paths:
      - backend:
          serviceName: hello-service
          servicePort: 80
        path: / 

$ kubectl create -f ingress-nginx/hello-app-ingress.yaml 
ingress.extensions/hello-ingress created

$ kubectl get ingress -n paas-9fe90304-8429-4e4a-abf0-86f83b24edd6-caas
NAME            HOSTS                  ADDRESS   PORTS   AGE
hello-ingress   hello.k8s.kpaasta.io             80      59m
```

{% hint style="info" %}
 호스트 정의 시 필수 확인사항!

1. Public 호스트 규칙 :  \*.k8s.kpaasta.io
2. Private 호스트 규칙 : \*.k8s.kpaasta.intl
{% endhint %}



