# Kubernetes Overview
Kubernetes 是一個用於自動化部署、擴展與管理容器化應用程式的開源系統，大約在 8 年前 ( 2015 ) 由 Google 工程師開發出來的。其誕生的主要原因就是為了解決 **「手動部署多個容器到多台機器上並監測管理這些容器的狀態很麻煩」** 這件事情。

Kubernetes 除了支援多種不同的容器工具如 Docker，同時也被許多公司用於開發產品，如 Rancher Labs、OpenShift、Tectonic、EcOS、PKS 及 IBM 私有雲產品等，GCP 內部的核心也是 k8s。

## Pod
Pod 是 K8s 中最小的運作單位。每個 Pod 都有一個身分證，也就是屬於這個 Pod 的 yaml 檔。

一個 Pod 包覆著一到多個以上的容器或是應用程式，為了資源的分配，所以大多數時候是一個應用容器。Pod 中的所有 Containers 可以共享一些資源，如 IP 資源與計算資源，並透過內部的 local port number 進行溝通。

好比說有一棟大樓社區，每一間住戶都是一個 Pod，住戶的家庭成員則好比 Conatiners，他們共享了水、店、瓦斯等等 ( 像是 IP 資源 )。當今天這棟大樓住了太多的住戶 ( Pod ) 就會有資源競爭的問題，此時 K8s 可以協調 Pod 中的資源 ( 可以透過 ymal 或 json 設定檔進行設定，不需要手動監控與協調 )。

##### 特性
1. Pod 擁有不確定的生命週期
2. Pod 內有一個讓所有 Container 共用的 Volume，與 Docker 不同
3. Pod 採取 shared IP，內部所有的容器皆使用同一個 Pod IP，也與 Docker 不同
4. Pod 內的眾多容器都會和 Pod 同生共死

## Cluster
一個 Kubernetes 集群由一組工作機器組成，稱為 **Node ( 節點 )**，運行容器化應用程序。每個集群至少有一個工作節點。

Worker Node 託管作為是應用負載組件的 Pod，而 Control-plane 則管理集群中的 Worker Node 和 Pod。

## Container 
應用容器，如 Docker，是內存與 CPU 的資源隔離單位，大部分ㄋ一個 Pod 中止會有一個容器；多個容器存在於 Pod 中，其中一個會是主容器，其他的則是輔助容器。

## 參考
* [Medium - Kubernetes 基礎教學（一）原理介紹](https://cwhu.medium.com/kubernetes-basic-concept-tutorial-e033e3504ec0)
* [Kala Cloud](https://ikala.cloud/kubernetes-gke-introduction/?https://ikala.cloud/kubernetes-gke-introduction/&gclid=CjwKCAiA55mPBhBOEiwANmzoQsVAyRPAjC8giqTXF-n93f2wQVw77AbnXF_fjuSjJqfax6MaK8-DIxoCf4QQAvD_BwE)
* https://kubernetes.io/docs/concepts/overview/components/