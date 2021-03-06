- 镜像使用
    - https://www.runoob.com/docker/docker-image-usage.html
    - 拉取
        - docker pull ubuntu:13.10
    - 删除
        - docker rmi hello-world
    - 更新
        - 在现有镜像基础上，生成一容器，容器内进行修改操作后，退出，运行：
            - docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
                - e218edb10161为容器ID
    - 构建/创建
        - 使用 Dockerfile 指令来创建一个新的镜像
        - docker build -t repo:tag .
            - 其中.为当前目录，同时表示当前目录下会有Dockerfile
    - 设置标签
        - docker tag ubuntu:18.04 username/ubuntu:18.04
    - 将自己的镜像推送到 Docker Hub
        - docker push username/ubuntu:18.04
    - 搜索
        - docker search username/ubuntu
        
- docker image
    - https://docs.docker.com/engine/reference/commandline/image/
    - 清理所有没有tag的镜像
        - docker image prune
    - 清理所有没有生成容器的镜像，也即未被使用的镜像
        - docker image prune -a