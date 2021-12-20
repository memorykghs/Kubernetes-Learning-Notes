# 00 - Docker

[Play with Docker](https://www.docker.com/play-with-docker) 或下載 [Cmder](https://cmder.net/) 與 Docker。<br/>

![](/images/0-1.png)

## 基本操作
#### Step 1
```docker
docker pull nginx

# 效果同上
docker pull nginx: latest

# 查看現在有的 image
docker images
```
![](/images/0-2.png)<br/>
![](/images/0-3.png)

#### Step 2
```docker
# 啟動 container
# -d：在背景直行，不要阻擋下指令的視窗
# -p：指定內外 port 的映射
docker run -d -p 80:80 nginx
docker run -d -p 81:80 nginx

# 查看現有的 container 運行狀態
docker ps
```
![](/images/0-4.png)

到目前為止已經使用 `pull` 及 `run` 指令將 image 拉下來、建立容器並啟動。啟動後，可以在網址列上輸入 `localhost:80` ( 或是替換成自己的 port 號 )，就可以看到預設的畫面。<br/>

![](/images/0-6.png)

##### Step 3
```docker
# 對 nginx 這個 container 進行修改
# docker exec -it <container_id> bash
docker exec -it 34 bash
```

其中 `container id` 只要可以辨別現有正在 run 的容器即可，不需要打太長。輸入指令後可以發現角色已經切換到 container 中 ( 就是最下面那行 `root@34e023e61ff1:/#` )。<br/>

![](/images/0-5.png)

#### Step 4
接著修改 html 文件。

```docker
cd /usr/share/nginx/html/

cat index.html

# 修改 index.html 文件
echo hello > index.html
```

![](/images/0-7.png)<br/>

![](/images/0-8.png)

此時重新整理網頁，會發現內容已經被我們改掉了!<br/>

![](/images/0-9.png)

#### Step 5
要退出 container 的輸入區的話，打 `exit` 就可以回到 docker 的指令區。

```docker
# 刪除容器
docker rm -f
```

![](/images/0-10.png)

#### Step 6
```docker
# 指定某個映像黨的名稱
docker commit 34 my-image
```

![](/images/0-11.png)


## 參考
* [Docker 入門](https://www.youtube.com/watch?v=AASsRoUfisk)