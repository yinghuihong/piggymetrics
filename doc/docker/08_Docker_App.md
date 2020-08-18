- https://docs.docker.com/app/working-with-app/

- enable experimental
    - To enable experimental features in the Docker CLI, edit the config.json file and set experimental to enabled.
    - To enable experimental features from the Docker Desktop menu, 
        click Settings (Preferences on macOS) > Command Line and then turn on the Enable experimental features toggle. 
        Click Apply & Restart.

- docker app init --single-file hello-world
    - Created "hello-world.dockerapp"
    - 如果没有指定--single-file，将生成目录以及目录下三个文件：
        - docker-compose.yml
        - metadata.yml
        - parameters.yml
    
- docker app inspect hello-world.dockerapp

- docker app validate hello-world.dockerapp
    - 检查配置是否有语法错误

- Deploy the app
    - There are several options for deploying a Docker App project.
        - Deploy as a native Docker App application
        - Deploy as a Compose app application
        - Deploy as a Docker Stack application