# 06 - Docker Container
Docker 利用容器來執行應用。容器是從映像檔建立的執行實例。它可以被啟動、開始、停止、刪除。每個容器都是相互隔離的、保證安全的平台。

可以把容器看做是一個簡易版的 Linux 環境 ( 包括root使用者權限、程式空間、使用者空間和網路空間等 ) 和在其中執行的應用程式。

* 映像檔是唯讀的，容器在啟動的時候建立一層可寫層作為最上層。

## 列出映像檔
這個只會列出正在運行中的 Container。
```docker
docker container ls
```
![](/images/docker/6-1.png)

這個則是列出當前所有的 Container，不論狀態。
```docker
docker container ls -a
```
![](/images/docker/6-2.png)

## 啟動容器
啟動容器有兩種方式：
1. 一種是將映像檔新建一個容器並啟動
2. 另一種是將終止狀態 ( stopped ) 的容器重新啟動

#### 新建並啟動容器
```docker
docker run java-app
```
![](/images/docker/6-3.png)

這時候下 `docker container ls` 會發現是空的，要下 `docker contaienr ls -a` 才會看到，而且該 Container 已經退出 ( exited )，因為內容已經執行完畢了。

![](/images/docker/6-4.png)

而下面這個指令，可以在 Container 與 Docker Terminal 中交互下指令。
```docker
docker run -t -i ubuntu:14.04 /bin/bash root@af8bae53bdd3:/#
```
其中， `-t` 選項讓 Docker 分配一個虛擬終端 ( pseudo-tty ) 並綁定到容器的標準輸入上， `-i` 則讓容器的標準輸入保持打開。

在互動模式下，使用者可以透過所建立的終端來輸入命令，例如：

```docker
root@af8bae53bdd3:/# pwd
/
root@af8bae53bdd3:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin
srv sys tmp usr var
```

當利用 `docker run` 來建立容器時，Docker 在後臺執行的標準操作包括：
* 檢查本地是否存在指定的映像檔，不存在就從公有倉庫下載
* 利用映像檔建立並啟動一個容器
* 分配一個檔案系統，並在唯讀的映像檔層外面掛載一層可讀寫層
* 從宿主主機設定的網路橋界面中橋接一個虛擬埠到容器中去
* 從位址池中設定一個 ip 位址給容器
* 執行使用者指定的應用程式
* 執行完畢後容器被終止

要退出容器的話，輸入 `exit` 即可。

#### 啟動已終止容器
使用 `docker start` 來啟動。容器的核心為所執行的應用程式，所需要的資源都是應用程式執行所必需的。可以在虛擬終端中利用 `ps` 或 `top` 來查看程式訊息。

如果容器原本就是啟動狀態的，`docker start` 會先終止容器在重新啟動它。

![](/images/docker/6-5.png)

可以發現容器中僅執行了指定的 bash 應用。這種特點使得 Docker 對資源的使用率極高，是貨真價實的輕量級虛擬化。

#### 守護態執行
更多的時候，需要讓 Docker 容器在後臺以守護 ( Daemonized ) 形式執行。此時，可以透過新增 `-d` 參數來實作。
```docker
docker run -d redis
3e5da2a074ee8a0190720fda67bb9b0258ab710ea722300351ffd884175f5217
```

![](/images/docker/6-6.png)

容器啟動後會返回一個唯一的 id，也可以透過 `docker ps` 命令來查看容器訊息。

![](/images/docker/6-7.png)


## 刪除容器
可以使用 `docker rm` 來刪除一個處於終止狀態的容器。
```docker
# docker rm ${container_id}
docker rm 3e5da2a074ee
```

如果要刪除執行中的容器，可以使用 `-f` 參數，Docker 會發送 `SIGKILL` 信號給容器。

## 其他
輸入 `docker` 指令可以看到 Docker 的相關指令說明，其中可以分成兩大部分：
1. Management Commands
2. Commands

![](/images/docker/6-8.png)

而簡寫的部分，例如列出 Container，下面兩個指令是等價的。
```docker
docker container ls
docker ps

docker container ls -a
docker ps -a
```

刪除 Container 的部分也是一樣，可以直接用 `rm` 直接執行。
```docker
docker image rm ${image_id}
docker rmi ${image_id}

docker container rm ${container_id}
docker rm ${container_id}
```

在刪除的部分，想要一次刪除所有 Container 可以這樣做：
1. 先找出所有 Container ID
  ```docker 
  docker container ls -aq
  ```
  ![](/images/docker/6-9.png)

2. 刪除所有 Container
```docker
docker rm $(docker container ls -aq)
```

若是只需要刪除停止的 Container，則可以先下一個 filter 再刪除：
```docker 
docker container ls -f "status=exited"
```

## 小結
* 啟動新建容器：`docker run ${image_name}`
* 啟動已終止容器：`docker start ${container_id}`
* 列出所有終止狀態的 Container：`docker ps -a`
* 刪除容器：`docker rm ${container_id}`
* 刪除執行中容器：`docker rm -f ${container_id}`

## 參考
* https://hackmd.io/llU_a500RHO3dHPH1rhwQw