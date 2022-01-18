# Kubernetes Overview
Kubernetes 是一個可移植、可擴展的開源平台，用於管理容器化工作負載和服務，有助於聲明式配置和自動化。

一個 Kubernetes 集群由一組工作機器組成，稱為 **Node ( 節點 )**，運行容器化應用程序。每個集群至少有一個工作節點。

工作節點託管 豆莢它們是應用程序工作負載的組成部分。這 控制平面管理集群中的工作節點和 Pod。在生產環境中，控制平面通常跨多台計算機運行，集群通常運行多個節點，提供容錯和高可用性。

## 參考
https://kubernetes.io/docs/concepts/overview/components/