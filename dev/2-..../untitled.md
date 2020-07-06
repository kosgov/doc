# K8S : Object Storage 배포

### Object Storage \( Minio \)  

{% embed url="http://kubeapps.k8s.kpaasta.io/" %}

```text
...
## Set default accesskey, secretkey, Minio config file path, volume mount path and
## number of nodes (only used for Minio distributed mode)
## AccessKey and secretKey is generated when not set

accessKey: JMWorks
secretKey: JMWorksSecret
...

# Number of MinIO containers running

replicas: 1

persistence:
  ...
  size: 2Gi
  
ingress:
  enabled: true
  ...
  annotations:
  {
    kubernetes.io/ingress.class: nginx,
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
  }
  ...
  hosts:
  - msa2-minio.k8s.kpaasta.io
  - msa2-minio.k8s.intl
  
resources:
  requests:
    memory: 0.5Gi
  limits:
    memory: 0.5Gi  
    
buckets:
  [
    { name: bucket-public, policy: public, purge: false },
    { name: bucket-download, policy: download, purge: false },
    { name: bucket-upload, policy: upload, purge: false }
  ]
  
## Specify the service account to use for the Minio pods. If 'create' is set to 'false'
## and 'name' is left unspecified, the account 'default' will be used.
serviceAccount:
  create: false
```

