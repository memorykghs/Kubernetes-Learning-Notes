# 01 - Kubernetes 簡介
分為 master 端與 worker 端。master 端有 4 個重要的角色：

* [kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
* [ETCD](https://etcd.io/)
* [kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)
* [kube-scheduler](https://kubernetes.io/zh/docs/concepts/scheduling-eviction/kube-scheduler/)

![](/images/1-1.png)


## Master 端
#### kube-apiserver
Kubernetes API 服務器驗證和配置 api 對象的數據，包括 pods、services、replicationcontrollers 等。API Server 提供 REST 服務，並透過 components 之間的交互，為集群  ( cluster ) 的共享狀態提供前端接口。

master 可以將指令指派給其下管理的 pods 執行 ( 是一種 cluster 之間溝通的語言 )，執行完畢後 pods 會將狀態回報給 master，回報的方式就是透過 kube-apiserver 提供的指令。使用者也可以使用 kube-apiserver 的指令從外部進行操作，操作的方式可以分為以下三種：

* `kubectl` - kube control
* REST API 指令
* Web UI

#### ETCD
這裡有一篇 [ETCD](https://brobridge.com/bdsres/2019/10/17/k8s-etcd-%E6%B7%BA%E6%9E%90%E5%88%86%E4%BA%AB/) 的簡介。

總之，整個 cluster 的配置與定義、各項工作的狀態資訊都是由 ETCD 負責統管的，對於 k8s Cluster 來說算是核心大腦組件。

#### kube-controller-manager
各種資源自動化的控制中心

#### kube-scheduler
調度資源的主要實施者

---
![](/images/1-2.png)

1. 當使用者從外部使用 `kubectl`、REST API 或是 Web UI 任一操作指令到 apiserver。
2. apiserver 接收到指令後傳遞給 kube-controller。
3. kube-controller 和 kube-scheduler 會進行協調調度。
4. 協調的過程中 ETCD 會提供一些數據的資源傳達給 kube-controller 跟 kube-scheduler。
5. 最後由 apiserver 形成內部調度指令。
6. apiserver 將指令下發給下面的節點，完成後節點會將狀態回報給 apiserver。

## Worker 端
首先介紹一個 Node 中會包含 3 個部分：
* [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) 
* [kubelet](https://kubernetes.io/zh/docs/reference/command-line-tools-reference/kubelet/)
* docker 容器

也就是說，每個節點上必須有 docker 容器包裝。

![](/images/1-3.png)

## Kubernetes 基礎概念
#### Pod
調度的最小單位。
```
[pod] = [docker] + [pause]
```

一個 Pod 可以有多個 docker 容器，但大多數時候是一個應用容器加一個 pause。Pod 中的所有 containers 可以共享一些資源，如 IP 資源與計算資源。

好比說有一棟大樓社區，每一間住戶都是一個 Pod，住戶的家庭成員則好比 conatiners，他們共享了水、店、瓦斯等等 ( 像是 IP 資源 )。當今天這棟大樓住了太多的住戶 ( Pod ) 就會有資源競爭的問題，此時 K8s 可以協調 Pod 中的資源 ( 可以透過 ymal 或 json 設定檔進行設定，不需要手動監控與協調 )。

#### Deployment
維持 Pods 數量。

```
kubectl run d1 --image httpd:alpine --pirt 80
```
其中 d1 是容器名稱。

#### Service
多個 Pod 抽象為一個服務。

```
kubectl expose deployment d1 --target-port 80 --type NodePort
```

## 參考
* https://zhuanlan.zhihu.com/p/56088355
* [kubernetes 入門 - 快速了解和上手容器編排工具k8s](https://www.youtube.com/watch?v=HsvAVGjlN9k&list=WL&index=32)
* [Kubernetes & Docker的分手肥皂劇 入門](https://www.youtube.com/watch?v=Qw-6k95IBHU)