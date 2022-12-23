# Docker安装Gitlab



## 服务器介绍

服务器：HECS 华为云耀云服务器

2C4G

```
# os-release
$ cat /etc/os-release
NAME="Ubuntu"
VERSION="20.04.4 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.4 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal

# cpuinfo
$ cat /proc/cpuinfo | grep processor
processor       : 0
processor       : 1

# meminfo
$ cat /proc/meminfo
MemTotal:        4025664 kB
MemFree:          142748 kB
MemAvailable:     391176 kB

```



## 环境配置

1. 设置GITLAB_HOME ,  vi ~/.bashrc， 添加如下

   ```
   # gitlab env
   export GITLAB_HOME=/src/gitlab
   ```

   ```
   source ~/.bashrc
   ```

2. docker安装 gitlab-ce

   ```
   sudo docker run --detach \
     --hostname gitlab.example.com \
     --publish 443:443 --publish 80:80 --publish 2222:22 \
     --name gitlab \
     --restart always \
     --volume $GITLAB_HOME/config:/etc/gitlab \
     --volume $GITLAB_HOME/logs:/var/log/gitlab \
     --volume $GITLAB_HOME/data:/var/opt/gitlab \
     --shm-size 256m \
     gitlab/gitlab-ce:latest
   ```

   说明：--publish 做端口映射 ，这里我把容器内的端口映射到外部服务器的2222端口，应为我的机器的本地的22端口已经被占用。

   ​			--hostname 暂时设置成gitlab.example.com

3.  等待安装成功，就可以通过IP访问自己的gitlab服务器了，第一次登录需要输入密码，默认用户为root,通过以下命令获取初始密码

   ```
   sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
   ```

4. 登录界面如下![gitlab](https://s1.ax1x.com/2022/12/18/zqk3Md.png)

   

5.  接下来为了更好的使用，进到容器内部，做些相应的配置，vi /etc/gitlab/gitlab.rb

   ```
   gitlab_rails['gitlab_shell_ssh_port'] = 2222
   
   # email config
   gitlab_rails['smtp_enable'] = true
   gitlab_rails['smtp_address'] = "smtp.163.com"
   gitlab_rails['smtp_port'] = 465
   gitlab_rails['smtp_user_name'] = "xxx@163.com"
   gitlab_rails['smtp_password'] = "QSIYPMJUGKSYINGX"
   gitlab_rails['smtp_authentication'] = "login"
   gitlab_rails['smtp_enable_starttls_auto'] = true
   gitlab_rails['smtp_tls'] = true
   gitlab_rails['gitlab_email_from'] = 'xxx@163.com'
   gitlab_rails['smtp_domain'] = "163.com"
   
   external_url 'http://123.249.46.24'
   ```

   说明：默认所有配置都是注释掉的，需要设置下相应的配置项

    1. 开启ssh端口，设置为映射的本地的端口号

    2. 设置邮箱相关配置项，这样可以修改默认的admin@example.com的admin邮箱账号

    3. external_url设置为公网的IP

    4. 设置完需要重启gitlab服务

       ```
       gitlab-ctl reconfigure
       gitlab-ctl restart
       ```

## 踩坑

gitlab初步配置好后，就急着创建新的仓库，并且尝试clone下来，这时发现怎么clone都不行，ssh keys也设置了，定位了好几个多小时，才发现，一是发现ssh端口没有开，于是设置了上述第5步的2222端口，还是不行，想了下，由于是公网服务器，默认2222端口，是不对外开放的，于是修改安全组，开放2222端口，接下来就可以顺利clone仓库了。



## References

1. https://docs.gitlab.com/ee/install/docker.html