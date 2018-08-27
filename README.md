# docker-training

下載 image
> docker run hello-world

檢示目前 container
- 1.docker container ls -a
# 2.docker ps -a

docker run busybox echo hi there

可進入 container
docker run -it busybox
離開
exit

秀出目前正在運行的 container
1.docker container ls
2.docker ps 

docker run = docker crteate + docker start


docker create hello-world
start -a 81f0311b05e694ed8b61247ddaacb1bbf98d98ae415e6796d0d4fb718afa722f

移除所有垃圾
docker rm $(docker ps -a -q)

停掉正在運行的 docker	
docker stop b7e7a94ca788
