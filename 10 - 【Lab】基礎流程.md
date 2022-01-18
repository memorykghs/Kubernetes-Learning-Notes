# 10 - 【Lab】
* 參考 [Kubernetes 基礎教學（二）實作範例：Pod、Service、Deployment、Ingress](https://cwhu.medium.com/kubernetes-implement-ingress-deployment-tutorial-7431c5f96c3e) 部分練習。

* [docker image](https://hub.docker.com/r/hcwxd/kubernetes-demo)

---

首先建立 Pod 的 [yaml](https://github.com/memorykghs/Kubernetes-Learning-Notes/blob/main/yml/little-whale.yaml) 檔：
```yml
apiVersion: v1
  kind: Pod
  metadata:
    name: little-whale
    labels:
      app: little-whale
  spec:
    containers:
      - name: kubernetes-demo-container
        image: hcwxd/kubernetes-demo
        ports:
          - containerPort: 3000
```

然後將做好的 yaml 檔佈上 Node。
```docker
kubectl apply -f C:\Users\user\Desktop\筆記\Kubernetes-Learning-Notes\yml\little-whale.yaml
```

執行後查看 Pod 部署情況與詳細訊息。
```docker
kubectl get pod
kubectl describe pod little-whale 
```

接著建立 Service。
```docker
kubectl expose pod little-whale --type=NodePort --name=little-whale-svc --port=80 --target-port=8080
```

然後查看現在的 Service 資訊。
```docker
kubectl get svc
```
結果如下，可以發現我們剛剛建立的 Service 並沒有對外的 EXTERNAL-IP。<br/>

![](/images/lab-3-1.png)

如果不小心建錯了可以刪掉 Service 重新建立。
```docker
kubectl delete svc <svc-name>
```

這時候去打 `http://localhost:8080/` 應該是什麼都沒有的。因為剛剛在 ymal 檔中設定的 Pod 的 port 是外部無法存取的，且跟本機端的 port 不相通。

所以還要透過 `kubectl port-forward`，把兩端的 port 做 mapping。

```docker
kubectl port-forward little-whale 8080:8080
```