# 4-6.pvc 사용자 가이드

 필요한 `PersistentVolume`\(PV\) 리소스는 `PersistentVolumeClaim`\(PVC\)을 통해 요청할 수 있습니다. 

#### 1.PersistentVolumeClaim 생성

```text
$ vi persistent-volume-claim.yml

---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-test # 변
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-storage # 고정
  
  
$ kubectl apply -f persistent-volume-claim.yml
  
  
// 결과 확인 
$ kubectl describe pvc pvc-test
Name:          pvc-test
Namespace:     paas-4b5a3805-2ea4-4b41-97af-6b49dcea4120-caas
StorageClass:  nfs-storage
Status:        Bound
Volume:        pvc-a9677a38-9bf3-11ea-9fbe-f220cdd74412
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               volume.beta.kubernetes.io/storage-provisioner: cluster.local/nfs-client-nfs-client-provisioner
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Mounted By:    <none>
Events:
  Type    Reason                 Age   From                                                                                                                                     Message
  ----    ------                 ----  ----                                                                                                                                     -------
   Normal  ProvisioningSucceeded  20m   cluster.local/nfs-client-nfs-client-provisioner_nfs-client-nfs-client-provisioner-58c4b879d6-gzb5n_bb36f4c7-95ac-11ea-ab08-ca0b7655f2bd  Successfully provisioned volume pvc-a9677a38-9bf3-11ea-9fbe-f220cdd74412
```

{% hint style="info" %}
* AccessModes 
  * ReadWriteOnce : 단일 노드가 볼륨을 읽기/쓰기 모드
  * ReadOnlyMany : 읽기전용 모드
  * ReadWriteMany : 여러 노드에 의한 읽기/쓰기 모드
{% endhint %}

\*\*\*\*

PVC생성 완료 이후 KubernetesDashboard &gt; Storages에서 생성한 pvc 볼륨을 확인할 수 있습니다.

![](../../.gitbook/assets/image%20%28153%29.png)

#### 2. 기존 생성된 Pod에 단일 신규 볼륨 할당

```text
$ vi persistent-pod.yml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-pod-test
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-storage
---
kind: Pod
apiVersion: v1
metadata:
  name: hello-app
spec:
  containers:
    - name: my-frontend
      image: busybox
      volumeMounts:
      - mountPath: "/data"
        name: my-volume
      command: [ "sleep", "100000" ]
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: pvc-pod-test

$  kubectl apply -f persistent-pod.yml
persistentvolumeclaim/pvc-pod-test created
pod/hello-app created

$ kubectl describe pod hello-app
Name:         hello-app
Namespace:    paas-4b5a3805-2ea4-4b41-97af-6b49dcea4120-caas
Priority:     0
Node:         192.168.0.36/192.168.0.36
Start Time:   Fri, 22 May 2020 15:32:26 +0900
Labels:       <none>
Annotations:  kubernetes.io/limit-ranger:
                LimitRanger plugin set: cpu, memory request for container my-frontend; cpu, memory limit for container my-frontend
Status:       Running
IP:           172.20.44.150
IPs:          <none>
Containers:
  my-frontend:
    State:          Running
      Started:      Fri, 22 May 2020 15:32:31 +0900
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  500Mi
    Requests:
      cpu:        100m
      memory:     500Mi
    Environment:  <none>
    Mounts:
      /data from my-volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-f9w9q (ro)

```

생성 완료 이후 KubernetesDashboard &gt; Storages에서 생성 pvc 볼륨을 확인할 수 있습니다. 

![](../../.gitbook/assets/image%20%28148%29.png)

