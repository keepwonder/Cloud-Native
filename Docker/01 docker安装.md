## 基于Ubuntu22.04安装docker

###  安装Docker

```
sudo apt install -y docker.io #安装Docker Engine
```



### 开启服务

```
sudo service docker start         #启动docker服务

```



### 加入用户组

```
sudo usermod -aG docker ${USER}   #当前用户加入docker组
```



第一个 service docker start 是启动 Docker 的后台服务，第二个 usermod -aG 是把当前的用户加入 Docker 的用户组。这是因为操作 Docker 必须要有 root 权限，而直接使用 root 用户不够安全，加入 Docker 用户组是一个比较好的选择，这也是 Docker 官方推荐的做法。当然，如果只是为了图省事，你也可以直接切换到 root 用户来操作 Docker。

重启系统，使usermod修改用户组命令生效

### 查看docker信息

```
docker info 或 docker version 
```

`docker version `输出客户端和服务端信息

```
jone@k8s:~$ docker version
Client:
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.17.3
 Git commit:        20.10.12-0ubuntu4
 Built:             Mon Mar  7 17:10:06 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.3
  Git commit:       20.10.12-0ubuntu4
  Built:            Mon Mar  7 15:57:50 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.9-0ubuntu3
  GitCommit:
 runc:
  Version:          1.1.0-0ubuntu1
  GitCommit:
 docker-init:
  Version:          0.19.0
  GitCommit:

```

`docker info` 会显示当前 Docker 系统相关的信息，例如 CPU、内存、容器数量、镜像数量、容器运行时、存储文件系统等等，

```
jone@k8s:~$ docker info
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 9
  Running: 0
  Paused: 0
  Stopped: 9
 Images: 13
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version:
 runc version:
 init version:
 Security Options:
  apparmor
  seccomp
   Profile: default
  cgroupns
 Kernel Version: 5.15.0-56-generic
 Operating System: Ubuntu 22.04.1 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 3.832GiB
 Name: k8s
 ID: SYZF:PKLZ:UZDR:OKO7:JUAR:NFQI:DHTR:377B:GPYS:P6YD:FHJZ:XDK2
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  http://hub-mirror.c.163.com/
  https://mirror.baidubce.com/
 Live Restore Enabled: false

```



