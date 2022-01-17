# 05 - Trouble Shooting with kubectl
前一章節我們建立了 Cluster 與 Pod，那怎麼知道剛剛的 Pod 到底被佈到哪一個 Node 上了呢?所以接下來要嘗試使用 `kubectl` 指令來獲取相關訊息。常見的操作可以通過以下 `kubectl` 完成：

* `kubectl get` - 列出资源
* `kubectl describe` - 顯示資源的詳細訊息
* `kubectl logs` - 印出 Pod 中的容器日誌
* `kubectl exec` - Pod 中容器內不執行命令

我們可以用這接指令來查看應用程式何時部署、當前的狀態、在哪裡運行及詳細的 config 是什麼。

## 指令
```docker
kubectl get pods
```
可以查看當前正在運作的 Pod。
```
C:\Users\user>kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
ngapp   1/1     Running   1
```

下面的指令則是可以查看 Pod 的詳細資訊，執行後可以看到會拉出長長的一串 ( 礙於太占版面就不放了XD )。
```docker
kubectl describe pods
```

其中的資訊包含 Pod 容器的詳細信息：IP 地址、使用的 port 以及與 Pod 生命週期相關的事件列表。`describe` 命令的輸出內容很豐富，涵蓋了一些我們尚未解釋的概念，暫時先不理他。

p.s. `describe` 命令可用於獲取有關大多數 kubernetes 原生語言的詳細信息：節點、Pod、部署。而我們看到的描述輸出被設計成方便讀取的格式，而不是被腳本化的。



## 參考
* [中文 docs](http://docs.kubernetes.org.cn/115.html)
* [k8s 官網](https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/)
* [k8s 官網互動 docs](https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-interactive/)