# Kubernetes-Learning-Notes

## Docker
* [Docker--從入門到實踐](https://www.aurotek.com.tw/uploads/files/hello.pdf)
* [Docker 指令小抄](https://mileslin.github.io/2019/04/Docker-%E6%8C%87%E4%BB%A4%E5%B0%8F%E6%8A%84/)

## Kubernetes 官網
* https://kubernetes.io/docs/concepts/

## 參考系列文章
* [Kubernetes 指南](https://feisky.gitbooks.io/kubernetes/content/introduction/)
* [Kubernetes 30天學習筆記](https://ithelp.ithome.com.tw/users/20103753/ironman/1590)

* [Kubernetes Handbook——Kubernetes 中文指南](https://jimmysong.io/kubernetes-handbook/)

#### 有的沒的
* [邊車模式 Sidecar Pattern](https://tachingchen.com/tw/blog/desigining-distributed-systems-the-sidecar-pattern-concept/)

#### 其他補充
* [Kubernetes Handbook - 雲原生應用架構實踐手冊](https://jimmysong.io/kubernetes-handbook/)


---

## 安裝
#### Kubernetes
* [kubernetes 官網 docs](https://kubernetes.io/docs/tasks/tools/)
* https://blog.kennycoder.io/2020/03/15/Kubernetes-minikube-%E8%BC%95%E9%AC%86%E5%BB%BA%E7%AB%8B%E6%9C%AC%E5%9C%B0%E7%AB%AF%E7%9A%84K8S%E9%9B%86%E7%BE%A4%E5%B7%A5%E5%85%B7-%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8/

#### Chocolatey
[Chocolatey](https://chocolatey.org/)
[Chocolatey Package 商店](https://community.chocolatey.org/packages)

#### Kind
[kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
[kind 安裝](https://www.gushiciku.cn/pl/pG79/zh-tw)

#### Cmder
[Cmder](https://cmder.net/)
[安裝Cmder](https://ithelp.ithome.com.tw/m/articles/10262144)

---

## 問題
1. 佈 pod 上去怎麼知道佈在哪個 node 上?
```
kubectl describe <pod_name>
```

2. Service Type v.s. 外部溝通的三個方法(nodePort、Cluster IP、Load Balance)???
3. Cluster 怎麼停止@@? 用 Kind 指令停
