
- docker-compose 
    - https://docs.docker.com/compose/reference/down/
    - 批量管理（启动/停止）container
        
- 安装docker-compose
    - sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose
    - 测试环境下网络不可用的话，采用：
        - 下载至本地，然后上传测试服务器
        - cp /opt/hbhdc/docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
        - chmod +x /usr/local/bin/docker-compose
    - 参考指南：https://docs.docker.com/compose/install/#install-compose
    

- 指定工程名
    - docker-compose --project-name xxx
    - 默认为工作目录名，(default: directory name)
    - 如：piggymetrics
    
- 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
    - 创建的网络
        - 默认名称为工程名_default
        - 如：piggymetrics_default
    - 同一宿主机上容器间通讯
        - 意思是在registry容器上可ping通同一个网络下其它容器
            - 如：ping config
        - 基于bridge网络实现
            - https://www.cnblogs.com/shenh/p/9714547.html
         
- docker-compose.yml
    - depends_on
        - 设置依赖关系。
    
- dep
    - 指定与服务的部署和运行有关的配置。只在 swarm 模式下才会有用。
    
    
- docker-compose命令
    - build
        - 构建生成镜像
    - create
        - 构建、生成容器
    - rm
        - 移除容器
    - up
        - 构建、生成、启动容器
    - down
        - 停止、删除容器
- 批量删除镜像
    - docker rmi `docker images | grep 10.12.5.67 | awk '{print $3}'`