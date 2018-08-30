# docker-training

下載 image
```
> docker run hello-world
```

檢示目前 container
```
1.> docker container ls -a
2.> docker ps -a
```
```
> docker run busybox echo hi there
```

可進入 container
```
> docker run -it busybox
```
離開
```
> exit
```

秀出目前正在運行的 container
```
1.> docker container ls
2.> docker ps
```

docker run = docker crteate + docker start

```
> docker create hello-world
> start -a 81f0311b05e694ed8b61247ddaacb1bbf98d98ae415e6796d0d4fb718afa722f
```

移除所有垃圾
```
> docker rm $(docker ps -a -q)
> docker system prune
```

停掉正在運行的 docker
```
> docker stop b7e7a94ca788
```
查看 container logs
```
> docker ps
> docker log {your container ID}
```
測試 stop 與 kill 的不同
```
> docker run busybox ping google.com
``````
會繼續跑10秒後停止
```
> docker stop {your container ID}
```
立刻停止
```
> docker kill {your container ID}
```

exec 可執行 container 裡的程式
```
> docker exec -it 7dc3182fa315 redis-cli
> set vicky 5
5
> get vicky
"5"
```

可以看到 container 裡的資料夾，但不能進去
```
> docker exec -i {your container ID} ls
bin
dev
etc
home
proc
root
sys
tmp
usr
var
```

```
> docker exec -it {your container ID} ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
```

sh 的退出無法使用 control c，需改成 control d
```
> docker run -it busybox sh
```

不同的 container 互不相影響，執行二次
```
> docker run -it busybox sh
> docker run -it busybox sh
> ls
> touch test
```

Alpine 簡介 https://yeasy.gitbooks.io/docker_practice/cases/os/alpine.html

建立第一個 docker

mkdir redis-v2-image

cd redis-v2-image

可用文字編輯器 create a file，名為 Docker，沒有副檔名，context 如下
```
# Use and existing docker image as a base
FROM alpine

# Download and nstall a dependency
RUN apk add --update redis

# Tell the image what to do when it starts
# as a container
CMD ["redis-server"]
```

```
> build image
> docker build -t v2-redis .
```

執行如下
```
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM alpine
 ---> 11cd0b38bc3c
Step 2/3 : RUN apk add --update redis
 ---> Using cache 
 ---> aa072262966d 
Step 3/3 : CMD ["redis-server"]
 ---> Using cache 
 ---> 7af6ecb9642b 
Successfully built 7af6ecb9642b
Successfully tagged v2-redis:latest
```

完成後，運行該 image
```
> docker run v2-redis
> docker ps -a
> docker exec -it {your container ID} redis-cli
> set abc 1
> get abc
```

node.js 文檔 http://nodejs.cn/api/

建立第一個 node.js 專案結合 docker
```
> mkdir simpleweb
> cd simpleweb
```

建立 package.json，內容如下
```
{
    "dependencies": {
     "express": "*"   
    },
    "scripts": {
        "start": "node index.js"
    }
}
```

建立 index.js，內容如下
```
const express = require('express')

const app = express();

app.get('/', (req, res) => {
    res.send('Hi there');
});

app.listen(8080, () => {
    console.log('Listening on port 8080');
});
```

建立 Dockerfile，內容如下
```
# Specify a base image
FROM alpine

# Install some depenendencies
RUN
RUN npm install

# Defaule command
CMD ["npm", "start"]
```

打包 image
```
> docker build .
```

結果如下
```
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM alpine
 ---> 11cd0b38bc3c
Step 2/3 : RUN npm install
 ---> Running in 1b61fba0a560
/bin/sh: npm: not found
The command '/bin/sh -c npm install' returned a non-zero code: 127
```
會出錯，因為 alpine 裡並沒有安裝 npm，我們指定版本，node:alpine
```
# Specify a base image
FROM node:alpine

# Install some depenendencies
RUN npm install

# Defaule command
CMD ["npm", "start"]
```

8/30


常用指令

https://blog.gtwang.org/linux/docker-commands-and-container-management-tutorial/

如何 copy docker image
```
# Specify a base image
FROM node:alpine

# Install some depenendencies
COPY ./ ./
RUN npm install

# Defaule command
CMD ["npm", "start"]
```
```
docker build -t ppanan/simpleweb .
docker run ppanan/simpleweb
```

如果要讓 Docker 容器內部的服務可以接收來自於外部的網路連線，可以使用 -p 參數將 Docker 容器內部的連接埠對應到實體機器的連接埠
```
docker run -p 8080:8080 ppanan/simpleweb
```

打開 browser 可看到結果

來測試看看加了 WORKDIR 的效果
```
# Specify a base image
FROM node:alpine

WORKDIR /user/app

# Install some depenendencies
COPY ./ ./
RUN npm install

# Defaule command
CMD ["npm", "start"]
```

查看運行中的 docker
```
docker ps
docker exec -it {your image ID} sh
```
我們會預設進去到 /user/app 


測試看看加了 WORKDIR 的效果
```
docker build -t ppanan/visist .
docker run ppanan/visist


docker-compose up
docker network ls
docker ps

背景執行
```
docker run -d redis
```


