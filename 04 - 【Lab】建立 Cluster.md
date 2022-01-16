# 04 - 【Lab】建立 Cluster
使用 kind 簡單 run 依次過程，首先建立 cluster。

## Create a Cluster
可以直接建立預設的 cluster，或是依照指定的 yaml 檔建立 cluster。

#### 使用預設建立 cluster
```docker
kind create --name test-cluster
```
![](/images/4-1.png)

#### 依照 yaml 建立 cluster
先創建一個 yaml 檔 [`kind.yaml`]()，內容如下：
```yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```
檔案設定中的 `nodes` 屬性帶下有 3 個 `role`，代表我們會建立 3 個 node，其中 1 個是 master node，另外 2 個是 worker node。


建立好的檔案可以隨便放在任何一個資料夾，不過就需要指定檔案路徑與名稱。
```docker
--name test-cluster --config <path/file-name.yaml>

# ex
--name test-cluster --config C:\Users\user\Desktop\筆記\Kubernetes-Learning-Notes\yml\kind.yaml
```

執行上述指令後，可以打開 docker desktop，可以看到有 3 個 node 正在運行。

![](/images/4-5.png)
<br/>

或是下 `kubectl` 指令也可以看到 node 的數量與狀態。
```docker
kubectl get node
```

![](/images/4-8.png)

## 查看所有已建立的 Cluster
```docker
kind get clusters
```
![](/images/4-2.png)
<br/>

## 建立 Pod
接著一樣使用 [`pod.yaml`]() 建立 Pod，檔案內容如下：
```yml
apiVersion: v1
kind: Pod
metadata:
  name: ngapp
  labels:
    app: webserver
spec:
  containers:
  - name: ngapp
    image: bonexd/docker-demo:ngapp
    ports:
    - containerPort: 8080
```

接著執行以下指令：
```docker
kubectl apply -f C:\Users\user\Desktop\筆記\Kubernetes-Learning-Notes\yml\pod.yaml
```
![](/images/4-6.png)
<br/>

## 查看 Pods

```docker
kubectl get pods
```
![](/images/4-7.png)
<br/>

## 刪除 Cluster
```docker
kind delete cluster
```

* 指定刪除哪一個 cluster，`<cluster-name>` 帶入要刪除的 cluster 名稱：
  ```docker
  kind delete cluster --name <cluster-name>
  ```
  錯誤示範XD：
  ![](/images/4-3.png)
  
  正確指令：
  ![](/images/4-4.png)
<br/>

https://blog.tienyulin.com/kubernetes-pod/

## 參考
* https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/
* [kubebuilder](https://book.kubebuilder.io/reference/kind.html)