
- docker service

- Docker Swarm mode
    - docker swarm init
        - docker swarm join --token SWMTKN-1-1nklb33kt5e8ck0dm8q4qvp1mhx0ep3hikcb403e1omr7581kg-72k24rql3f43s07slywqlpka2 192.168.65.3:2377
        - docker swarm join-token manager
    - docker service create --name demo alpine:3.5 ping 8.8.8.8
    - docker service ps demo
    - docker service logs demo
    - docker service rm demo
    
- stack
    - cd swarm
    - docker stack deploy -c bb-stack.yaml demo
    - docker service ls
    - docker stack rm demo