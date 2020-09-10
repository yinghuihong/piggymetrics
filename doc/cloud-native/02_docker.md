
- 营造独立的环境，使进程不会相互影响
    - 独立文件系统
        - chroot（change root），可将子路径设为root路径，从而实现独立的文件系统
    - 资源视图隔离
        - namespace，如能看见部分进程、独立主机名等
    - 资源使用限制
        - cgroup
        
- 镜像
    - 运行容器所需要的所有文件集合，或叫文件系统
    - 采用changeset设计
        - 类似 disk snapshot
        - 分层、复用，提高分发效率、节省磁盘空间
        
- 数据卷 volume
    - 是独立于容器的生命周期
    
- 容器引擎架构    
    - moby daemon
    - containerd
    - shim
    - 容器运行时
        - runC 
        - kata
        - gVisor
        
        