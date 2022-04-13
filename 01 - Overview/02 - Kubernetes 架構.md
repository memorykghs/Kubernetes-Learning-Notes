# 02 - Kubernetes Node 架構
分為 Master Node 與 Worker Node。

![](/images/overview-2-1.png)

## Master Node
Master Node 中有 4 個重要的角色：

* apiserver
* [ETCD](https://etcd.io/)
* controller-manager
* scheduler

![](/images/overview-2-2.png)

#### Api Server
接收與傳遞命令的窗口，Kubernetes API 服務器驗證和配置 api 對象的數據，包括 pods、services、replicationcontrollers 等。API Server 提供 REST 服務，並透過 components 之間的交互，為集群  ( cluster ) 的共享狀態提供前端接口，也就是 k8s 集群的接口與通訊組件。與其他組件如 scheduler 交互時就必須通過 apiserver 進行。

同時 apiserver 也是唯一能夠代理使用者請求操作 ETCD 的組件。使用者可以使用 apiserver 的指令從外部進行操作，操作的方式可以分為以下三種：

* `kubectl` - kube control
* REST API 指令
* Web UI

#### ETCD
這裡有一篇 [ETCD](https://brobridge.com/bdsres/2019/10/17/k8s-etcd-%E6%B7%BA%E6%9E%90%E5%88%86%E4%BA%AB/) 的簡介。

總之，整個 cluster 的配置與定義、各項工作的狀態資訊都是由 ETCD 負責統管的，對於 k8s Cluster 來說算是核心大腦組件，會記錄整個 k8s 的狀態的一個儲存庫，包括節點、Pods、Deployments 等資訊。

#### Controller-manager
維持狀態的管理者，各種資源自動化的控制中心，保證集群中的狀態一致的元件。

controller-manager 通過 apiserver 監控各個節點之間的狀態，確保實際狀態與預期狀態一致。如果設定該結點上要有 10 個 Pods 存活，只要有 Pods 掛了，那麼 controller-manager 就會去協調產生新的 Pods 來維持設定的數量。

#### Scheduler
調度決策資源的主要實施者，分配 Pod 到適當的 Node 上。

scheduler 掌握當前集群的使用情況，當有新的應用請求發布到 k8s 集群上，scheduler 決定這些相應的 Pods 應該要被分配到那些空閒的 Nodes 上。

## Worker Node
Worker Node 是 k8s 集群資源的提供者，一個 Worker Node 中會包含 3 個部分：
* [kubelet](https://kubernetes.io/zh/docs/reference/command-line-tools-reference/kubelet/)
* Container Runtime
* [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/proxy/)

![](/images/overview-2-3.png)

#### kubelet
負責監聽 apiserver 的一些事件，執行 Master Node 來的指令，例如透過 CRI 抓取 image 並建立 Pod、或是關閉 port 等，也會將此節點的狀態數據回報給 apiserver。

#### kube-proxy
監聽請求，負責將流量導向至對應的 Pod，是管理 k8s 中服務 ( service ) 網絡的組件。

Pod 的 IP 在 k8s 網路中其實是 **不固定的 ( ephemeral )** 的，為了管理 Pod 的 IP 可能的變化，k8s 引入了 service 的概念，是實現 service 抽象機制的概念。

service 可以屏蔽 Pod 的 IP，並在調用的時候進行負載均衡。而如果要將 service 管理的 port 暴露給外部使用，也需要透過 kube-proxy 進行轉發代理。

如果說 Master Node 是 k8s 集群的大腦，kubelet 就可以稱作是 Worker Node 上的小腦。

#### Container Runtime
Worker Node 上容器資源的管理者，如果用的是 Docker，那麼 Container runtime 就是 Docker engine。kubelet 並不會直接去管理節點上的容器，而是委託給 Conatiner runtime 管理，如啟動或關閉容器。

![](/images/overview-2-3.png)

---

## 小結
master 可以將指令指派給其下管理的 pods 執行 ( 是一種 cluster 之間溝通的語言 )，執行完畢後 pods 會將狀態回報給 master，回報的方式就是透過 apiserver 提供的指令。

![](/images/overview-2-4.png)

1. 當使用者從外部使用 `kubectl`、REST API 或是 Web UI 任一操作指令到 apiserver。
2. apiserver 接收到指令後傳遞給 controller。
3. controller 和 scheduler 會進行協調調度。
4. 協調的過程中 ETCD 會提供一些數據的資源傳達給 controller 跟 scheduler。
5. 最後由 apiserver 形成內部調度指令。
6. apiserver 將指令下發給下面的節點，完成後節點會將狀態回報給 apiserver。

## 參考
* https://zhuanlan.zhihu.com/p/56088355

#### 影片
* [kubernetes 入門 - 快速了解和上手容器編排工具k8s](https://www.youtube.com/watch?v=HsvAVGjlN9k&list=WL&index=32)
* [如何理解 Kubernetes 的架構 (上)](https://www.youtube.com/watch?v=Wc5G80Bd-QI)
* [Kubernetes & Docker的分手肥皂劇 入門](https://www.youtube.com/watch?v=Qw-6k95IBHU)