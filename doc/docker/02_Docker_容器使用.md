
- docker container prune
    - 清理所有停止的容器
        - Remove all stopped containers
        
- docker container run -it IMAGE /bin/bash
    - Run a command in a new container
    - 通过镜像创建并运行容器
    - 容器启动后又自动停止的问题
        - 参考：https://www.jianshu.com/p/ca63b6c8fdf1
        - docker run -d java:8-jre
            - 后台执行，但执行完就退出了
        - docker run -dt java:8-jre
            - 后台执行完，还有个`伪终端`运行着，故不会退出
        - docker run -dt java:8-jre pwd
            - 指定执行命令，执行完会退出
    - -P，将容器内部使用的网络端口随机映射到我们使用的主机上。
    - --name，指定容器名称
    - network，指定容器连接的网络
    - --restart，重启策略，always/no
    - -h HOSTNAME 或者 --hostname=HOSTNAME，设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts
    - --dns=IP_ADDRESS：添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。
    
- docker container exec -it CONTAINER free -h
    - Run a command in a running container
    - 在运行中的容器，执行命令
    - 进入容器的方式有两种：
        - docker attach
        - docker exec：推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。
            
- docker logs CONTAINER
    - 实现原理
        - 当我们输入docker logs的时候会转化为Docker Client向Docker Daemon发起请求
        - Docker Daemon 在运行容器时会去创建一个协程(goroutine)，绑定了整个容器内所有进程的标准输出文件描述符。
        - 因此容器内应用的所有只要是标准输出日志，都会被 goroutine 接收
    - 存放位置
        - /var/lib/docker/containers
    
- docker top CONTAINER
    - 查看容器内部运行的进程
    
- docker port CONTAINER
    - 查看指定容器的端口映射
    
- docker stats CONTAINER
    - 查看容器内资源使用统计
