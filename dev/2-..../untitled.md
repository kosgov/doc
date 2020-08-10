# K8S : Object Storage 배포

### Object Storage \( Minio \)  

{% embed url="https://kubeapps.k8s.kpaasta.io" %}



```text
...
global:
  minio:
    accessKey: JMWorks
    secretKey: JMWorksSecret
...
serviceAccount:
  create: false
...
clusterDomain: cluster.local
...
## Init containers parameters:
volumePermissions:
  ...
  resources:  
    limits:
      cpu: 200m
      memory: 256Mi
    requests:
      cpu: 100m
      memory: 128Mi
...
## MinIO server mode. Allowed values: standalone or distributed.
mode: distributed
...        
resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi
...
persistence:
  ...
  size: 10Gi    
...  
ingress:
  enabled: true
  ...
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/force-ssl-redirect: true
  ...
  hosts:
    - name: jmworks-minio.k8s.kpaasta.kr
    - name: jmworks-minio.k8s.intl
...
```

확인 \(테스트\)

