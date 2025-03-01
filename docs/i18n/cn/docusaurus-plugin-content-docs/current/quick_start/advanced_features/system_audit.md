---
sidebar_position: 7
sidebar_label: "系统审计日志"
---

# 审计日志

为了记录 HwameiStor 集群系统的使用和操作历史信息，HwameiStor 提供了系统审计日志。该审计日志具有 HwameiStor 系统语义，易于用户查阅、解析。
审计日志针对 HwameiStor 系统中的每类资源，记录其使用操作信息。该资源包括：Cluster、Node、StoragePool、Volume，等等。

## 使用方式

审计日志通过 CRD 的方式存入系统中，为每一个资源创建一个 CR 来记录其操作历史。该 CRD 如下：

```yaml
apiVersion: hwameistor.io/v1alpha1
kind: Event
  name: 
spec:
  resourceType: <Cluster | Node | StoragePool | Volume>
  resourceName:
  records:
  - action:
    actionContent: # in JSON format
    time:
    state:
    stateContent: # in JSON format

```

例如，我们可以看看数据卷的审计信息：

```yaml
apiVersion: hwameistor.io/v1alpha1
kind: Event
metadata:
  creationTimestamp: "2023-08-08T15:52:55Z"
  generation: 5
  name: volume-pvc-34e3b086-2d95-4980-beb6-e175fd79a847
  resourceVersion: "10221888"
  uid: d3ebaffb-eddb-4c84-93be-efff350688af
spec:
  resourceType: Volume
  resourceName: pvc-34e3b086-2d95-4980-beb6-e175fd79a847
  records:
  - action: Create
    actionContent: '{"requiredCapacityBytes":5368709120,"volumeQoS":{},"poolName":"LocalStorage_PoolHDD","replicaNumber":2,"convertible":true,"accessibility":{"nodes":["k8s-node1","k8s-master"],"zones":["default"],"regions":["default"]},"pvcNamespace":"default","pvcName":"mysql-data-volume","volumegroup":"db890e34-a092-49ac-872b-f2a422439c81"}'
    time: "2023-08-08T15:52:55Z"
  - action: Mount
    actionContent: '{"allocatedCapacityBytes":5368709120,"replicas":["pvc-34e3b086-2d95-4980-beb6-e175fd79a847-krp927","pvc-34e3b086-2d95-4980-beb6-e175fd79a847-wm7p56"],"state":"Ready","publishedNode":"k8s-node1","fsType":"xfs","rawblock":false}'
    time: "2023-08-08T15:53:07Z"
  - action: Unmount
    actionContent: '{"allocatedCapacityBytes":5368709120,"usedCapacityBytes":33783808,"totalInode":2621120,"usedInode":3,"replicas":["pvc-34e3b086-2d95-4980-beb6-e175fd79a847-krp927","pvc-34e3b086-2d95-4980-beb6-e175fd79a847-wm7p56"],"state":"Ready","publishedNode":"k8s-node1","fsType":"xfs","rawblock":false}'
    time: "2023-08-08T16:03:03Z"
  - action: Delete
    actionContent: '{"requiredCapacityBytes":5368709120,"volumeQoS":{},"poolName":"LocalStorage_PoolHDD","replicaNumber":2,"convertible":true,"accessibility":{"nodes":["k8s-node1","k8s-master"],"zones":["default"],"regions":["default"]},"pvcNamespace":"default","pvcName":"mysql-data-volume","volumegroup":"db890e34-a092-49ac-872b-f2a422439c81","config":{"version":1,"volumeName":"pvc-34e3b086-2d95-4980-beb6-e175fd79a847","requiredCapacityBytes":5368709120,"convertible":true,"resourceID":2,"readyToInitialize":true,"initialized":true,"replicas":[{"id":1,"hostname":"k8s-node1","ip":"10.6.113.101","primary":true},{"id":2,"hostname":"k8s-master","ip":"10.6.113.100","primary":false}]},"delete":true}'
    time: "2023-08-08T16:03:38Z"
```
