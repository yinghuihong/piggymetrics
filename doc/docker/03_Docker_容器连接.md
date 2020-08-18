- 端口映射
    -P : 是容器内部端口随机映射到主机的高端口。
    -p : 是容器内部端口绑定到指定的主机端口。

- 创建一个新的 Docker 网络
    - docker network create -d bridge test-net
    - -d：参数指定 Docker 网络类型，有 bridge、overlay。
        - 其中 overlay 网络类型用于 Swarm mode
        
- 加入同一个network的容器，容器间可互联
    - 进入容器，ping 另一容器名称
    
- 配置 DNS
    - 在宿主机的 /etc/docker/daemon.json 文件中增加以下内容来设置全部容器的 DNS：
      
          {
            "dns" : [
              "114.114.114.114",
              "8.8.8.8"
            ]
          }
          
        - docker run -it --rm ubuntu  cat /etc/resolv.conf
            - 查看容器的 DNS 是否生效可以使用该命令，它会输出容器的 DNS 信息
    - 手动指定容器的配置
        - docker run -it --rm -h host_ubuntu --dns=114.114.114.114 --dns-search=test.com ubuntu