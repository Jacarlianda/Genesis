基础环境配置

~~~
# 系统更新
yum update -y
yum groupinstall development tools -y
yum install wget yum-utils -y

# 安装Docker
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -

# 安装docker-enter工具
wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc

# 配置加速器
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://08d15a71.m.daocloud.io

# 从 docker1.3.2版本开始默认docker registry使用的是https
# 当用 docker pull 非https的docker regsitry的时候会报错
# Error response from daemon: Get https://XXX/v2/: http: server gave HTTP response to HTTPS client
vi /usr/lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd --insecure-registry 192.168.50.178:10000

# 重启docker
systemctl restart docker
systemctl daemon-reload
~~~



docker

参数

`-t` 选项让Docker分配一个伪终端(pseudo-tty)并绑定到容器的标准输入上

`-i` 则让容器的标准输入保持打开

`-d` 需要让 Docker在后台运行而不是直接把执行命令的结果输出在当前宿主机下



更简单的，建议大家下载 [.bashrc_docker](https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker)，并将内容放到 .bashrc 中。

```
$ wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
$ echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc
```

这个文件中定义了很多方便使用 Docker 的命令，例如 `docker-pid` 可以获取某个容器的 PID；而 `docker-enter` 可以进入容器或直接在容器内执行命令。

```
$ echo $(docker-pid <container>)
$ docker-enter <container>
```