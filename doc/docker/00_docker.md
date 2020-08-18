
- docker 搜索
    - docker search httpd
        - tag name默认为latest，即httpd:latest
    - docker pull httpd

- docker 镜像更新
    - 在现有镜像基础上，生成一容器，容器内进行修改操作后，退出，运行：
        - docker commit 容器ID 

- 当前采用的registry？
    - docker info | grep Registry

- 每个模块都有配置：http://config:8888，这里的"config"是如何被其它容器识别的？

- Registry、Repository、Image（Tag）、Container
    - 一个 Docker Registry 中可以包含多个仓库（Repository），如：[Docker Hub](https://hub.docker.com)
    - Docker 仓库用来保存镜像，每个仓库可以包含多个标签（Tag）；每个标签对应一个镜像。
    - 一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。
    - 我们可以通过 <仓库名>:<标签> 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签。
    - 容器是镜像运行时的实体。
    
- 中文学习网站
    - https://wiki.jikexueyuan.com/project/docker-technology-and-combat/
    
- 环境搭建
    - 查看服务器系统信息
        - cat /proc/version
    - 更新yum
        - yum update
        - 更新docker版本
            - https://www.cnblogs.com/mybxy/p/10576399.html
    - 安装docker
        - 基于本地rpm仓库
            - yum install docker
        - 手动下载安装方式
            - wget http://10.12.76.66/mirror/docker-ce/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
                - https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
            - wget http://10.12.76.66/mirror/docker-ce/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-19.03.5-3.el7.x86_64.rpm
            - wget http://10.12.76.66/mirror/docker-ce/linux/centos/7/x86_64/stable/Packages/docker-ce-19.03.5-3.el7.x86_64.rpm
            - yum install -y containerd.io-1.2.6-3.3.el7.x86_64.rpm docker-ce-cli-19.03.5-3.el7.x86_64.rpm docker-ce-19.03.5-3.el7.x86_64.rpm
    - 卸载docker
        - yum list installed | grep docker
            -  yum -y remove docker.x86_64 docker-client.x86_64 docker-common.x86_64
        - yum clean all
        - yum makecache
        - yum repolist
    - 压缩/解压
        - tar -zcvf tarname.tar file/dir
        - tar -zxvf tarname.tar
    - 镜像导入/导出
        - docker image save
        - docker image load
        - docker container export 1e560fca3906 > ubuntu.tar
            - 导出容器快照
        - docker image import   
            
    - systemctl disable firewalld
    
- 三步骤
    - 手工构建、保存、加载镜像
        - 本地执行
            - docker build 
            - docker save -o java.tar java:8-jre
            - scp -P 22 java.tar hbhdc@10.12.5.67:/opt/hbhdc/images
        - 测试服务器上执行（加载镜像）
            - docker image load -i /opt/hbhdc/images/java.tar
            
        
        
    - k8s容器管理