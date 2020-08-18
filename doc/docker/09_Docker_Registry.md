
- Docker Registry storage driver
    - oss: A driver storing objects in Aliyun OSS.
    
- docker pull [OPTIONS] NAME[:TAG|@DIGEST]
    - docker pull ubuntu
        - a shortcut for the longer `docker pull docker.io/library/ubuntu` command
    - pull-from-a-different-registry
        - docker pull myregistry.local:5000/testing/test-image
            - pulls the `testing/test-image` image from a local registry listening on port 5000 (myregistry.local:5000):
        - https://docs.docker.com/engine/reference/commandline/pull/#pull-from-a-different-registry
    - -a or --all-tags，pull all images from a repository

- use-cases
    - https://docs.docker.com/registry/introduction/#use-cases
    
            Running your own Registry is a great solution to integrate with and complement your CI/CD system. 
            In a typical workflow, a commit to your source revision control system would trigger a build on your CI system, 
            which would then push a new image to your Registry if the build is successful. 
            A notification from the Registry would then trigger a deployment on a staging environment, 
            or notify other systems that a new image is available.
            
            It’s also an essential component if you want to quickly deploy a new image over a large cluster of machines.
            
            Finally, it’s the best way to distribute images inside an isolated network.     
            
    - 补足 CI/CD 流程
        - 提交到代码版本控制系统，将在CI系统上触发一次构建，当构建成功会推送一新的镜像到Registry。
            从Registry发出的通知，会触发一次在阶段环境的部署，或通知其他系统有新的可用镜像。
        - 如果你想要在集群上快速部署新的镜像，这也是一个基础组件
        - 最后，最好是将镜像分配在一个隔离的网络
    
- Run an externally-accessible registry
    - 域名
    - https证书
        
- Deploy your registry using a Compose file
    
        registry:
          restart: always
          image: registry:2
          ports:
            - 5000:5000
          environment:
            REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
            REGISTRY_HTTP_TLS_KEY: /certs/domain.key
            REGISTRY_AUTH: htpasswd
            REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
            REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
          volumes:
            - /path/data:/var/lib/registry
            - /path/certs:/certs
            - /path/auth:/auth
    - Replace /path with the directory which contains the certs/ and auth/ directories.
    - docker-compose up -d
    
- Configuring a registry
    - 覆盖部分配置，替换规则为：REGISTRY_XXX_XXX=xxx
        
            storage:
              filesystem:
                rootdirectory: /var/lib/registry
                
        - 在docker run 命令，-e REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/somewhere
        - 或在Dockerfile.yml，配置ENV
    - 覆盖整体配置文件
        - -v `pwd`/config.yml:/etc/docker/registry/config.yml
    - 常见的配置
        - hooks
        - storage
        - http
            - addr
            - tls
                - letsencrypt
        - notifications
            - endpoints

- 搭建私有docker registry
    - 测试服务器无法访问公服
    - 本地执行
        - docker pull registry
        - docker save -o registry.tar registry:latest
        - scp -P 22 registry.tar hbhdc@10.12.5.67:/opt/hbhdc/images
    - 测试服务器上执行
        - docker image load -i /opt/hbhdc/images/registry.tar
        - docker tag 2d4f4b5309b1 registry:2
        - docker run -d -p 5000:5001 -e REGISTRY_HTTP_ADDR=0.0.0.0:5001 --restart=always --name registry -v /mnt/registry:/var/lib/registry registry:2
            - -e REGISTRY_HTTP_ADDR=0.0.0.0:5001
                - 修改Registry监听端口的环境变量
            - -v /mnt/registry:/var/lib/registry
                - 挂载磁盘，/mnt/registry为宿主机目录
                - 删除容器时，数据就不会被一并删除了
            - -P:将容器内部使用的网络端口随机映射到我们使用的主机上。
            - Customize the storage back-end
            
- 使用私服实践
    - 远程测试服务器上：
        - 创建并运行Registry容器
            - docker run -d -p 3030:5050 -e REGISTRY_HTTP_ADDR=0.0.0.0:5050 --restart=always --name registry -v "$(pwd)"/registry:/var/lib/registry registry:2
        - 停止&删除容器
            - docker container stop registry && docker container rm -v registry
    - 本地MacOS机器上：
        - 从公服拉取镜像
            - docker pull ubuntu:16.04
        - 新增标签
            - docker tag ubuntu:16.04 10.12.5.67:3030/ubuntu:16.04
        - 默认https方式，需修改daemon.json，允许采用http访问私服
            - https://docs.docker.com/registry/insecure/#deploy-a-plain-http-registry
            - 添加配置
                - "insecure-registries":["10.12.5.67:3030"]
            - 检测配置是否生效
                - docker info 查看 Insecure Registries
        - 推送镜像到私服
            - docker push 10.12.5.67:3030/ubuntu:16.04
        - 删除本地镜像
            - docker image remove ubuntu:16.04
            - docker image remove 10.12.5.67:3030/ubuntu:16.04
        - 从私服拉取
            - docker pull 10.12.5.67:3030/ubuntu:16.04
        - 查看私服目录
            - curl -XGET http://10.12.5.67:3030/v2/_catalog
        - 查看私服镜像标签列表
            - curl -XGET http://10.12.5.67:3030/v2/ubuntu/tags/list
            
- harbor
    - https://goharbor.io/
    - https://blog.csdn.net/qq_24095941/article/details/86063684
    - tar -zxvf harbor-offline-installer-v1.7.7-rc1.tgz
    - cd harbor
    - vim harbor.cfg
        - hostname = 10.12.5.67
        - customize_crt = off
        - harbor_admin_password = Harbor12345
    - ./prepare
    - ./install.sh
    - 本地访问http://10.12.5.67
        - admin
        - Harbor12345
    - 后期维护
        - docker-compose down
        - ./prepare
        - docker-compose up
        
- 将piggymetrics发布镜像到harbor私服
    - 新建rabbitmq目录及Dockerfile文件
    - 新建docker-compose.test.yml
    - docker-compose -f docker-compose.test.deploy.yml -f docker-compose.test.deliver.yml build
    - docker-compose -f docker-compose.yml -f docker-compose.test.yml push
    
- 在测试服务器上拉取、运行容器
    - 将 .env docker-compose.test.deploy.yml 上传至测试服务器
        - scp -P 22 .env docker-compose.test.deploy.yml hbhdc@10.12.5.67:/opt/hbhdc/
    - 在测试服务器上启动项目
        - 配置允许docker-client采用http访问harbor私服
            - 创建文件/etc/docker/daemon.json
            - 添加文本 "insecure-registries":["10.12.5.67:80"]
        - docker login http://10.12.5.67:80
            - admin
            - Harbor12345
        - docker-compose -f docker-compose.test.deploy.yml up
    - 重要端点
        - http://10.12.5.67:4000/
            - 网关
        - http://10.12.5.67:8761/
            - eureka，服务注册发现中心
        - http://10.12.5.67:9000/hystrix
            - http://turbine-stream-service:8080/turbine/turbine.stream
            - turbine stream 负责收集应用发来的信息，hystrix dashboard 负责展示
        - http://10.12.5.67:15672/#/
            - guest/guest
            - 消息处理