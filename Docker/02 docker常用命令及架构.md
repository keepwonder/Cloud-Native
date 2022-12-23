## Docker使用



- docker ps

  ```
  jone@k8s:~$ docker ps
  CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
  ```

- docker pull

  ```
  docker pull busybox      #拉取busybox镜像
  ```

- docker run

  ```
  jone@k8s:~$ docker run busybox echo hello world
  hello world
  ```

- docker ps  -a

  ```
  jone@k8s:~$ docker ps -a
  CONTAINER ID   IMAGE                                                                 COMMAND                  CREATED              STATUS                          PORTS                                                                                                                                  NAMES
  907bb55ff3fb   busybox                                                               "echo hello world"       About a minute ago   Exited (0) About a minute ago                                                                                                                                          naughty_goodall
  ```

  



### Docker架构图

![Docker架构](https://s1.ax1x.com/2022/12/24/zjqCOf.png)



Docker 官方还提供一个“hello-world”示例，展示了 Docker client 到 Docker daemon 再到 Registry 的详细工作流程，执行命令：`docker run hello-world`

```
jone@k8s:~$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

