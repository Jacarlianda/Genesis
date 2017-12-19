### 1、升级系统，配置JDK(java8以上)

~~~shell
nohup yum update -y && yum install net-tools -y && nohup yum
# groupinstall development tools -y &
# vi /etc/profile
JAVA_HOME=/opt/jdk1.8.0_131
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib/dt.jar
export JAVA_HOME JRE_HOME PATH CLASSPATH
# tar -xzvf jdk-8u131-linux-x64.tar.gz -C /opt/
# 关闭防火墙
systemctl stop firewalld.service
systemctl disable firewalld.service
groupadd elk
useradd elk -g elk

# vi /etc/security/limits.conf 
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
~~~



### 2、配置YUM源

~~~  shell
# rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
~~~

```shell
# vi /etc/yum.repos.d/elk.repo
[logstash-5.x]
name=Elastic repository for 5.x packages
baseurl=https://artifacts.elastic.co/packages/5.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

~~~ shell
# yum install logstash
~~~



### 3、安装ELK

~~~shell
# ln -s /opt/jdk1.8.0_131/bin/java /usr/bin/
# yum install elasticsearch
# yum install logstash
# yum install kibana
~~~



### 4、编辑配置文件

~~~shell
# vi /etc/elasticsearch/elasticsearch.yml
~~~



### 5、启动服务

~~~shell
# systemctl daemon-reload 
# systemctl start elasticsearch 
# systemctl status elasticsearch 
# systemctl enable elasticsearch 

systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
systemctl status elasticsearch
 
~~~



### 6、查看侦听端口

~~~ shell
# netstat -nltp
~~~



### 7、部署中可能出现的问题

~~~shell
Q: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
A: # vi /etc/sysctl.conf
添加
vm.max_map_count=262144
# sysctl -p

Q: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
vi /etc/security/limits.conf
添加
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
~~~











