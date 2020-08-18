- FROM nginx
    - 定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。
    
- RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
    - 用于执行后面跟着的命令行命令。有以下俩种格式：
        - RUN <命令行命令>
            - <命令行命令> 等同于，在终端操作的 shell 命令。
        - RUN ["可执行文件", "参数1", "参数2"]
            - 例如：RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
    - Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。
        - 以 && 符号连接命令，这样执行后，只会创建 1 层镜像。
        
              RUN yum install wget \
                  && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
                  && tar -xvf redis.tar.gz  
                  
- 开始构建镜像
    - 在 Dockerfile 文件的存放目录下，执行构建动作。
    - docker build -t nginx:test .
        - 最后的 . 代表本次执行的上下文路径
        
- 上下文路径
    - 在构建镜像时，有时候想要使用到本机的文件，docker build 命令得知这个路径后，会将路径下的所有内容打包。
    - 解析：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。
        实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。
        这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。
    - 注意：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。

- COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
    - 复制指令，从上下文目录中复制文件或者目录到容器里指定路径。
    - [--chown=<user>:<group>]：可选参数，用户改变复制到容器内文件的拥有者和属组。
    - <源路径>：源文件或者源目录，这里可以是通配符表达式
    - <目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。
    
- ADD
    - ADD 指令和 COPY 的使用格式一致（同样需求下，官方推荐使用 COPY）。功能也类似，不同之处如下：
        - ADD 的优点：在执行 <源文件> 为 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，会自动复制并解压到 <目标路径>。
        - ADD 的缺点：在不解压的前提下，无法复制 tar 压缩文件。会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢。具体是否使用，可以根据是否需要自动解压来决定。
        
- CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
    - 类似于 RUN 指令，用于运行程序，但二者运行的时间点不同:
        - CMD 在docker run 时运行。
        - RUN 是在 docker build。
    - 为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。
    - CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。
    - 注意：如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。
    
- ENTRYPOINT
    - 可以搭配 CMD 命令使用：一般是变参才会使用 CMD ，这里的 CMD 等于是在给 ENTRYPOINT 传参
            
          ENTRYPOINT ["nginx", "-c"] # 定参
          CMD ["/etc/nginx/nginx.conf"] # 变参 
    - 运行 docker run 时使用了 --entrypoint 选项，此选项的参数可当作要运行的程序覆盖 ENTRYPOINT 指令指定的程序。

- ENV
    - 设置环境变量
          
          ENV <key> <value>
          ENV <key1>=<value1> <key2>=<value2>...

- ARG <参数名>[=<默认值>]
    - 构建参数，与 ENV 作用一致。不过作用域不一样。
    - ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效，构建好的镜像内不存在此环境变量。
    - 构建命令 docker build 中可以用 --build-arg <参数名>=<值> 来覆盖。
    
- VOLUME
    - 定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。
    - 作用：
        - 避免重要的数据，因容器重启而丢失，这是非常致命的。
        - 避免容器不断变大。
    - 格式：
    
          VOLUME ["<路径1>", "<路径2>"...]
          VOLUME <路径>
    - 在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点。
    
- EXPOSE <端口1> [<端口2>...]
    - 仅仅只是声明端口。
    - 作用：
        - 帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射。
        - 在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口。
        
- WORKDIR <工作目录路径>
    - 指定工作目录。用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在。（WORKDIR 指定的工作目录，必须是提前创建好的）。
    - docker build 构建镜像过程中的，每一个 RUN 命令都是新建的一层。只有通过 WORKDIR 创建的目录才会一直存在。
    
- USER <用户名>[:<用户组>]
    - 用于指定执行后续命令的用户和用户组，这边只是切换后续命令执行的用户（用户和用户组必须提前已经存在）。
    
- HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
    - 用于指定某个程序或者指令来监控 docker 容器服务的运行状态。
    - 例如config模块下的：
        - HEALTHCHECK --interval=30s --timeout=30s CMD curl -f http://localhost:8888/actuator/health || exit 1
        
- ONBUILD <其它指令>
    - 用于延迟构建命令的执行。
    - 简单的说，就是 Dockerfile 里用 ONBUILD 指定的命令，在本次构建镜像的过程中不会执行（假设镜像为 test-build）。
        当有新的 Dockerfile 使用了之前构建的镜像 FROM test-build ，当执行新镜像的 Dockerfile 构建时候，
        会执行 test-build 的 Dockerfile 里的 ONBUILD 指定的命令。
