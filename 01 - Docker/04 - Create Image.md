# 04 - Create Image
建立映像檔有很多方法，可以從 Docker Hub 取得已有映像檔並更新，或是在本機建立一個。首先從修改已存在的映像檔開始。

## 修改已有映像檔
先使用下載的映像檔啟動容器。

```docker
docker run -t -i redis /bin/bash
```

* `-i` - 以交互模式運行容器，通常和 `-t` 一起使用。
  > flag allows you to make an interactive connection by grabbing the standard in (STDIN) of the container.


* `-t` - 為容器重新分配一個偽輸入中端，簡而言之就是進入容器的終端，通常與 `-i` 一起使用。
  > flag assigns a pseudo-tty or terminal inside the new container.

* `-d` - 背景執行

* `/bin/bash` - 在 Container 中啟動一個 bash shell
  > launches a Bash shell inside our container

![](/images/docker/4-1.png)

按 `Ctrl + C` 可以退出。

## 利用 Dockerfile 建立 Image
使用 `docker build` 來建立一個新的映像檔，所以在此之前要先建立一個 Dockerfile。

首先新增一個目錄跟一個 Dockerfile。
```docker
mkdir sinatra
cd sinatra
touch Dockerfile
```

先 `vi` 進去 Dockerfile 內。
```docker
vi Dockerfile
```

Dockerfile 中每一條指令都會建立一層映像檔，例如：
```docker
# This is a comment
FROM ubuntu:14.04
MAINTAINER Docker Newbee <newbee@docker.com>
RUN apt-get -qq update
RUN apt-get -qqy install ruby ruby-dev
RUN gem install sinatra
```

* `#` - 註解
* `FROM` - 告訴 Docker 使用哪個映像檔作為基底，上面的例子後面接著是維護者的信息
* `RUN` - 開頭的指令會在建立中執行，比如安裝一個套件，在這裏使用 `apt-get` 來安裝了一些套件

下面的圖片以以下資訊為範例：
```docker
FROM redis:latest
MAINTAINER memorykghs 
```


完成 Dockerfile 後可以使用 `docker build` 建立映像檔。
```docker
docker build -t redis . --no-cache
```

* `-t` - 表示建立的 image 名稱。
* `-f` - 指定讀取的 Dockerfile 檔，若沒指定預設讀取 Dockerfile。
* `--no-cache` - 避免在 Build Docker image 時被 cache 住，而造成沒有 build 到修改過的 Dockerfile

![](/images/docker/4-3.png)
![](/images/docker/4-4.png)

## 上傳映像檔
使用者可以通過 docker push 命令，把自己建立的映像檔上傳到倉庫中來共享。例如，使用者在 Docker Hub 上完成註冊後，可以推送自己的映像檔到倉庫中。

先登入
```docker
docker login -u memorykghs
```

還有因為我們剛剛建立的 Image 沒有 Tag，無法上傳，所以要先將 Image 打上 Tag。
```docker
# docker tag ${Image Name} DockerHub帳號/Image Name
docker tag redis memorykghs/docker-playground
```

```docker
docker push memorykghs/docker-playground
```
p.s. Docker Hub 上的 Repository 都必須是小寫，不能出現大寫，但可以包含一些特殊符號

## 刪除 Image
```docker
# docker rmi ${image_name}
docker rmi memorykghs/docker-playground
```
* `rmi` - 代表要 `rm` 的是 Image

## 參考
* [菜鳥教程 - Docker run 命令](https://www.runoob.com/docker/docker-run-command.html)
* https://blog.csdn.net/dongdong9223/article/details/52998375
* https://ithelp.ithome.com.tw/articles/10191016?sc=hot