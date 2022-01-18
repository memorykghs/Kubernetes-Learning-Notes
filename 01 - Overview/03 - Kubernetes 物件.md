# 03 - Kubernetes 物件

## Replica Set

## Deployment
維持 Pods 數量。

Deployments 顧名思義掌控了部署 Kubernetes 服務的一切。它主要掌管了 Replica Set 的個數，而 Replica Set 的組成就是一個以上的 Pod。

1. Deployments 的設定檔 ( 底下以 yaml 格式為例 )，可以指定 replica，並保證在該 replica 的數量運作
2. Deployments 會檢查 pod 的狀態
3. Deployments 下可執行滾動更新或者回滾

```
kubectl run d1 --image httpd:alpine --pirt 80
```
其中 d1 是容器名稱。

## Service
多個 Pod 抽象為一個服務。

##### 特性
1. 每個 Service 包含著一個以上的 Pod
2. 每個 Service 有個獨立且固定的 IP 地址 ( Cluster IP )
3. 客戶端訪問 Service 時，會經由上述提過的 proxy 來達到負載平衡、與各 Pod 連結的結果
4. 利用標籤選擇器 ( Label Selector )，有效率的選擇那些已貼上標籤的 Pod

```
kubectl expose deployment d1 --target-port 80 --type NodePort
```

## ConfigMap
在 k8s 中與應用程式相關的設定則可放在 ConfigMap 內。ConfigMap 內容中的格式就像是 Map 一樣是由 key-value pair 組成的。

```yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
```

使用 ConfigMap 儲存設定的方式是希望讓與應用程式與設定解耦 (decoupled)。意思是，ConfigMap 與 Pod 可以單獨存在於 k8s 叢集中，當 Pod 需要使用 ConfigMap 時才需要將 ConfigMap 掛載到 Pod 內使用。