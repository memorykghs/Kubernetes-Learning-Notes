# 05 - Dockerfile
當使用 Image 建立 Container 後，有時候需要進入 Docker Container 裡面下一些指令安裝程式或是修改設定檔，起多個 Container 就需要重複執行動作不少次。

而 Dockerfile 內可以包含一些啟動 Container 之後要執行的指令，之後就使用 `docker build` 指令來建立 Image 即可。

Dockerfile 是由一行一行的命令語句組成，並支援以 `#` 開頭的註解行。一般而言，Dockerfile 分為四部分：
1. 基底映像檔資訊
2. 維護者資訊
3. 映像檔操作指令
4. 容器啟動值型指令

Dockerfile 中每一條指令都會建立一層映像檔，例如：
```docker
# 基本映像檔，必須是第一個指令
FROM ubuntu

# 維護者： docker_user <docker_user at email.com> (@docker_user)
MAINTAINER docker_user docker_user@email.com

# 更新映像檔的指令
RUN echo "deb http://archive.ubuntu.com/ubuntu/ raring main univ
erse" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf

# 建立新容器時要執行的指令
CMD /usr/sbin/nginx
```

* 一開始必須指明作為基底的映像檔名稱，接下來說明維護者資訊。
* 接著是映像檔操作指令，例如 `RUN` 指令， `RUN` 指令將對映像檔執行相對應的命令。
* 每運行一條 `RUN` 指令，映像檔就會新增一層。
* 最後是 `CMD` 指令，指定執行容器時的操作命令。

## 指令
指令的一般格式為 `INSTRUCTION arguments`，指令包括
`FROM`、`MAINTAINER`、`RUN` 等。

#### FROM
Dockerfile 中第一條指令必須為 `FROM` 指令，如果要在同一個 Dockerfile 中建立多個映像檔時，可以使用多個 `FROM` 指令 ( 每個映像檔一次 )。
* 格式：`FROM <image>` 或 `FROM <image>:<tag>`。
* 告訴 Docker 使用哪個映像檔作為基底，上面的例子後面接著是維護者的信息。

#### MAINTAINTER
格式：`MAINTAINER <name>`，指定維護者訊息。

#### RUN
每條 RUN 指令將在當前映像檔基底上執行指定命令，並產生新的映像檔。
* 格式：`RUN <command>` 或 `RUN ["executable", "param1", "param2"]`。

  * `RUN <command>`：在 shell 終端中運行命令，即 `/bin/sh -c`
  * `RUN ["executable", "param1", "param2"]`：使用 exec 執行。
  
  指定使用其它終端可以透過第二種方式實作，例如 `RUN ["/bin/bash", "-c", "echo hello"]`。

當命令較長時可以使用 `\` 來換行。

#### CMD
指定啟動容器時執行的命令，每個 Dockerfile 只能有一條 CMD 命令。如果指定了多條命令，只有最後一條會被執行。而使用者啟動容器時候指定了運行的命令，則會覆蓋掉 CMD 指定的命令。

支援三種格式：
1. `CMD ["executable","param1","param2"]`：使用 `exec` 執行，推薦使用
2. `CMD command param1 param2`：在 `/bin/sh` 中執行，使用在給需要互動的指令
3. `CMD ["param1","param2"]`：提供給 `ENTRYPOINT` 的預設參數

完成 Dockerfile 後可以使用 `docker build` 建立映像檔。