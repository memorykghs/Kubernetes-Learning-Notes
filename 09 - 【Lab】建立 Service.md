# 09 - 【Lab】建立 Service
建立

## 使用 expose 指令建立 Service
使用 `kubectl` 的 `expose` 指令自動建立一個 Service，讓 Pod 的 port 開放給外部存取。

```docker
kubectl expose pod <pod name> --type=<service type> --name=<service name> \
--port=<port> --target-port=<target port>
```

所以我們要先下下面的指令，查看 Pod 的存活狀態。
```docker
kubectl get pod
```

出來的結果大概會長這樣：
```
C:\Users\user>kubectl get pods --show-labels
NAME    READY   STATUS    RESTARTS   AGE   LABELS
ngapp   1/1     Running   2          43h   app=webserver
```

接下來就要把這個 Pod 的 port 開放給外部使用，指令如下：
```docker
kubectl expose pod ngapp --type=NodePort --name=ngapp-service --port=80 --target-port=8080
```

根據上面獲得的資訊，Pod 的名稱是 `ngapp`，然後我們將這個 Service 命名為 `ngapp-service`。

建立 Service 之後，一樣可以透過 `kubectl` 的 `get` 指令查看我們剛剛建立的 Service。
```docker
kubectl get service
```

獲得資訊如下：
![](/images/9-1.png)
<br/>

當然也可以使用下面指令取得詳細的資訊。
```docker
kubectl describe service
``` 

## 使用 yaml 檔建立 Service
```yml
apiVersion: v1
kind: Service
metadata:
  name: ng-app-service
spec:
  type: NodePort
  selector:
    app: ngApp
  ports:
    -port: 80
    targetPort: 8080
```