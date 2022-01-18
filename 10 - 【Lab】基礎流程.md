# 10 - 【Lab】
參考 [Kubernetes 基礎教學（二）實作範例：Pod、Service、Deployment、Ingress](https://cwhu.medium.com/kubernetes-implement-ingress-deployment-tutorial-7431c5f96c3e) 部分練習。

首先建立 Pod 的 yaml 檔：
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
```
kubectl apply -f C:\Users\user\Desktop\筆記\Kubernetes-Learning-Notes\yml\little-whale.yaml
```

