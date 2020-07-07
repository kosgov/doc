# K8S : Message Queue 배포

Message Queue \( RabbitMQ \)

{% embed url="https://kubeapps.k8s.kpaasta.io" %}

replica count \(default\) : 3 

```text
rabbitmqUsername: admin
rabbitmqPassword: xxxx

managementUsername: management
managementPassword: xxxx

rabbitmqErlangCookie: xxxx

## Number of replicas 
replicaCount: 3

service:
  ...
  type: NodePort
  ...
  epmdNodePort: 
  amqpNodePort: 
  managerNodePort: 
  
resources:
  {
    limits: { cpu: 400m, memory: 1Gi },
    requests: { cpu: 400m, memory: 1Gi }
  }  
  
initContainer:
  enabled: true
  securityContext:
    runAsNonRoot: true
    runAsUser: 100
    fsGroup: 101
  chownFiles: false
  resources:
    {
      limits: { cpu: 100m, memory: 128Mi },
      requests: { cpu: 100m, memory: 128Mi }
    }
    
persistentVolume:
  enabled: true    
  ...
  size: 2Gi

serviceAccount:
  create: false
      
ingress:
  enabled: true
  hostName: msa2-rabbit.k8s.kpaasta.io   

securityContext:
  fsGroup: 101
  runAsNonRoot: true
  runAsUser: 100

prometheus:       
  operator:     
    enabled: false
```

App Security Groups \(ASGs\)

a collection of egress rules \( protocols, ports, and IP address ranges \) 

{% code title="Rabbit-ASG.json" %}
```text
[
  {
    "protocol": "tcp",
    "destination": "192.168.0.0/24",
    "ports": "31672",
    "description": "Allow RabbitMQ traffic to K8S Workers"
  }
]
```
{% endcode %}

확인 \(테스트\)

