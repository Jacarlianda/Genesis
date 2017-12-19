## 配置 Docker 加速器

~~~
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://08d15a71.m.daocloud.io
~~~

该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/default/docker 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同。更多详情请[访问文档](http://guide.daocloud.io/dcs/daocloud-9153151.html)。

#### 阿里云的安装脚本

```
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```

#### DaoCloud 的安装脚本

```shell
curl -sSL https://get.daocloud.io/docker | sh
```

#### 添加 yum 源

虽然 CentOS 软件源 `Extras` 中有 Docker，名为 `docker`，但是不建议使用系统源中的这个版本，它的版本相对比较陈旧，而且并非 Docker 官方维护的版本。因此，我们需要使用 Docker 官方提供的 CentOS 软件源。

执行下面的命令添加 `yum` 软件源。

```shell
tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/docker/yum/repo/centos7
enabled=1
gpgcheck=1
gpgkey=https://mirrors.tuna.tsinghua.edu.cn/docker/yum/gpg
EOF
```

#### 安装 Docker

更新 `yum` 软件源缓存，并安装 `docker-engine`。

```shell
yum update
yum install docker-engine
```



### Ubuntu 16.04、Debian 8 Jessie、CentOS 7

对于使用 [systemd](https://www.freedesktop.org/wiki/Software/systemd/) 的系统，用 `systemctl enable docker` 启用服务后，编辑 `/etc/systemd/system/multi-user.target.wants/docker.service` 文件，找到 `ExecStart=` 这一行，在这行最后添加加速器地址 `--registry-mirror=<加速器地址>`，如：

```shell
systemctl enable docker
vi /etc/systemd/system/multi-user.target.wants/docker.service
ExecStart=/usr/bin/dockerd --registry-mirror=http://08d15a71.m.daocloud.io
```

*注：对于 1.12 以前的版本，dockerd 换成 docker daemon。*

重新加载配置并且重新启动。

```shell
systemctl daemon-reload
systemctl restart docker
```


~~~shell
yum install epel-release
yum install libsodium-devel
~~~





从docker1.3.2版本开始默认docker registry使用的是https，当用docker pull 非https的docker regsitry的时候会报错：Error response from daemon: Get https://XXX:8081/v2/: http: server gave HTTP response to HTTPS client

~~~shell
vi /usr/lib/systemd/system/docker.service
ExecStart=/usr/bin/docker --insecure-registry 192.168.50.26:5000
systemctl daemon-reload
systemctl restart docker
~~~



~~~shell
docker login 192.168.50.26:5000
docker tag zookeeper:3.4.10 192.168.50.26:5000/zookeeper:3.4.10
docker push 192.168.50.26:5000/zookeeper:3.4.10
~~~









