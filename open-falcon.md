## 官方文档

~~~
https://www.open-falcon.org/zh_0_2/
~~~

## 系统初始化

~~~shell
# 关闭防火墙
systemctl stop firewalld.service
systemctl disable firewalld.service
# 修改hostname
## 临时修改
hostname 主机名称
## 永久修改(重启生效)
hostnamectl set-hostname 主机名称

# 系统更新
rm /etc/yum.repos.d/*

vi /etc/yum.repos.d/CentOS-Base.repo
[base]
name=CentOS-$releasever - Base
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
[updates]
name=CentOS-$releasever - Updates
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
[extras]
name=CentOS-$releasever - Extras
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
**


yum makecache
yum update -y
yum groupinstall development tools -y
~~~

## 环境配置

~~~shell
export GOROOT=/opt/go
export GOPATH=/opt/go/bin
export PATH=$PATH:$GOROOT/bin
export PATH=$PATH:/opt/go/bin

# go下载地址
https://golang.org/dl/

wget https://redirector.gvt1.com/edgedl/go/go1.9.2.linux-amd64.tar.gz
tar -xzvf go1.9.2.linux-amd64.tar.gz -C /opt/
mkdir -p $GOPATH/src/github.com/open-falcon/
cd $GOPATH/src/github.com/open-falcon/falcon-plus/
git clone https://github.com/open-falcon/falcon-plus.git
~~~

## 导入数据

1、安装配置Mysql数据库

2、导入数据

~~~ shell
cd /tmp/ && git clone https://github.com/open-falcon/falcon-plus.git 
cd /tmp/falcon-plus/scripts/mysql/db_schema/
mysql -h 127.0.0.1 -u root -p < 1_uic-db-schema.sql
mysql -h 127.0.0.1 -u root -p < 2_portal-db-schema.sql
mysql -h 127.0.0.1 -u root -p < 3_dashboard-db-schema.sql
mysql -h 127.0.0.1 -u root -p < 4_graph-db-schema.sql
mysql -h 127.0.0.1 -u root -p < 5_alarms-db-schema.sql
rm -rf /tmp/falcon-plus/
~~~

 ## dashboard

~~~
yum install -y python-virtualenv
yum install -y python-devel
yum install -y openldap-devel
yum install -y mysql-devel
yum groupinstall "Development tools"
cd $HOME/open-falcon/dashboard/
virtualenv ./env
./env/bin/pip install -r pip_requirements.txt -i http://mirrors.aliyun.com/pypi/simple/
~~~

## 查看日志

~~~
http://blog.csdn.net/vbaspdelphi/article/details/52817604
https://github.com/sdvdxl/falcon-logdog
~~~

### 项目启动

1、服务端

~~~
cd /home/work/open-falcon/
# transfer是数据转发服务
# 它接收agent上报的数据
# 然后按照哈希规则进行数据分片、并将分片后的数据分别push给graph&judge等组件
# transfer 8433
./open-falcon start transfer

# agent用于采集机器负载监控指标，比如cpu.idle、load.1min、disk.io.util等等
# 每隔60秒push给Transfer。agent与Transfer建立了长连接，数据发送速度比较快
# agent提供了一个http接口/v1/push用于接收用户手工push的一些数据
# 然后通过长连接迅速转发给Transfer
# agent 1988
./open-falcon start agent

# graph是存储绘图数据的组件
# graph组件 接收transfer组件推送上来的监控数据
# 同时处理api组件的查询请求、返回绘图数据
# graph 6070
./open-falcon start graph

# api组件，提供统一的restAPI操作接口
# api组件接收查询请求
# 根据一致性哈希算法去相应的graph实例查询不同metric的数据
# 汇总拿到的数据，最后统一返回给用户
# api 8080
./open-falcon start api

# heartbeat rpc:6030 http:6031
./open-falcon start hbs

# Judge用于告警判断
# agent将数据push给Transfer
# Transfer不但会转发给Graph组件来绘图
# 还会转发给Judge用于判断是否触发告警。
# judge 6080
./open-falcon start judge

# dashboard 8081
cd /home/work/open-falcon/dashboard
# 以开发者模式启动
./env/bin/python wsgi.py
# 在生产环境启动
bash control start
~~~

2、监控端

~~~
./open-falcon start agent
~~~

3、redis监控

~~~
https://github.com/iambocai/falcon-monit-scripts/tree/master/redis
https://github.com/ZhuoRoger/redismon
~~~

4、mysql监控

~~~
https://github.com/open-falcon/mymon
~~~

5、查看falcon-agent

~~~
http://192.168.50.84:1988/

~~~























































































































































































































































































































































