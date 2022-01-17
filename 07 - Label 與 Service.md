# 07 - Lable 與 Service

## Lable 是什麼?
Pod 本身會帶著許多不同標籤 ( Label ) 來辨別其實際用途，使用者可以透過賦予一組唯一的標籤組合

## Service 是什麼?
Service 可以想像成是**通往 pod 的通道**，在 k8s 中，由於 pods 數量是不固定的 ( 動態的 )，每當有新的 Pod 被新建或是刪除，IP 都會跟著改變，想要使用在上面的服務，每次都需要去動態的改 IP。

Service 的存在可以解決這個問題，我們只要把 request 送至 Service，它就會自動忙做負載平衡 ( load-balance ) 並將 request 送到 Pod 當中。

![](/images/5-1.png)

Service 有 4 種類型：
1. Cluster IP
2. NodePort
3. LoadBalancer
4. ExternalName

接下來我們將陸續提到 Service 相關的名詞與概念。

#### port
是暴露在 Cluster IP 上的埠，提供 cluster 內部客戶端訪問 Service 的入口。
```
clusterIP:port
```

根據下面的 yaml，假設今天設定的 port 是 3306，cluster 內部的其他容器可以透過 33306 port 存取 mysql 服務。但是外部就不能存取 mysql 服務了，因為 mysql 服務沒有設定 NodePort。

```yml
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  ports:
  - port: 33306
    targetPort: 3306
  selector:
    name: mysql-pod
```

#### NodePort
顧名思義，就是在 node 節點上，開了一個 port，可以想像成是**通往 pod 的通道**。`NodePort` 是**節點外部流量**的關鍵入口，可以由外部存取到某個 Service，是最基礎的 Service。
```
nodeIP:nodePort
```

#### targetPort
是 Pod 上的埠，從 port 或 nodePort 來的資料，經過 kube-proxy 流入到後端 Pod 的 targetPor，最後進入容器。

```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort            // 配置NodePort，外部流量可訪問k8s中的服務
  ports:
  - port: 30080             // 服務訪問埠，叢集內部訪問的埠
    targetPort: 80          // pod控制器中定義的埠（應用訪問的埠）
    nodePort: 30001         // NodePort，外部客戶端訪問的埠
  selector:
    name: nginx-pod
```

#### Endpoint
建立 Service 的同時，會自動建立跟 Service 同名的 Endpoints。

Endpoint 是 k8s 叢集中一個資源物件，儲存在 etcd 裡面，用來記錄一個 Service 對應的所有 Pod 的訪問地址。Service 通過 selector 和 Pod 建立關聯。

```
Endpoint = Pod IP + Container Port
```

Service 配置 selector endpoint controller 才會自動建立對應的 Endpoint 物件，否則是不會生產 Endpoint 物件。

#### External IP
供外部使用者存取的 IP，對應到 Service 上的 NodePort。
```
<EXTERNAL-IP>:<nodePort>
```

#### Cluster IP
是 cluster 內部的網路位址，供內部 ( Pod 與 Pod 之間 ) 使用，是 k8s 預設的 service。cluster 內的其他應用都可以存取該 service，但是在 cluster 外部的就無法訪問。

Cluster IP 為虛擬靜態 IP，當 cluster 物件被產生時，會一併產生 Cluster IP，並且在物件刪除前不會改變。

#### Pod IP
每個 Pod 都有的獨立的網路位址，只有 cluster 內可以存取。對應到 Service 上的 TargetPort。

## 建立 Service
Service 也可以透過 yaml 檔或是 json 格式的檔案定義並建立，例如下面定義了一個 nginx 服務，將服務的 80 port 轉發到 default namespace 中帶有 run=nginx 標籤的 Pod 上，該 Pod 的 port 也是 80。

```yml
apiVersion: v1
kind: Service
metadata:
  labels:
    run: nginx
  name: nginx
  namespace: default
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    run: nginx
  sessionAffinity: None
  type: ClusterIP
```

## Kubernetes Service NodePort yaml 範例
* 參考網址：[Service Types in Kubernetes?](https://medium.com/avmconsulting-blog/service-types-in-kubernetes-24a1587677d6)

![](/images/5-2.png)

## 參考

#### Label
* https://tachingchen.com/tw/blog/kubernetes-service-in-detail-2/

#### Service
* https://ithelp.ithome.com.tw/articles/10249718
* [Kubernetes 的三種外部訪問方式](http://dockone.io/article/4884)
* https://www.gushiciku.cn/pl/gzXl/zh-tw
* https://iter01.com/523068.html
* https://iter01.com/558744.html



