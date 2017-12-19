

1、环境配置

```
cd /etc/yum.repos.d/
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
curl -O http://mirrors.163.com/.help/CentOS7-Base-163.repo
yum clean all
yum makecache
yum update -y
yum install net-tools -y
yum groupinstall development tools -y
systemctl stop firewalld.service
systemctl disable firewalld.service
firewall-cmd --state
```

2、解压

```
unzip fastdfs-nginx-module-master.zip
unzip fastdfs-master.zip
unzip libfastcommon-master.zip
tar -xzvf nginx-1.12.1.tar.gz
```

3、安装

```
cd /root/libfastcommon-master
./make.sh
./make.sh install
cd /root/fastdfs-master
./make.sh
./make.sh install
```

4、配置

```
vi /etc/fdfs/tracker.conf
# 是否禁用该配置文件
# false：开启
# true：禁用
disabled=false
# 绑定地址
# 空字符表示绑定所有地址
bind_addr=
# 端口设置
port=22122
# 连接超时设置
connect_timeout=30
# 网络超时设置
network_timeout=30
# 存储数据和日志的目录
base_path=/mnt/fastdfs
# 最大并发连接数
max_connections=256
# 接受请求的线程数
accept_threads=1
# 执行请求的线程数 <= max_connections
work_threads=4
# 如何选择上传文件的存储group
# 0: 轮询
# 1: 制定group名称
# 2: 负载均衡, 选择空闲空间最大的存储group
store_lookup=2
# 选择哪一个存储group，当store_lookup=1时，须指定存储group的名称
store_group=group2
# 如何选择上传文件的存储server
# 0: 轮询
# 1: 以ip地址排序的第一个地址
# 2: 以优先级排序
store_server=0
# 选择存储server的哪一个路径(磁盘或挂载点)上传文件
# 0: 轮询
# 2: 负载均衡, 选择空闲空间最大的路径来存储文件
store_path=0
# 选择哪一个存储server下载文件
# 0: 轮询
# 1: 选择该文件上传时的那个存储server
download_server=0
# 预留的存储空间 G(GB) M(MB) K(KB) 默认byte(B)，% 表示比例，如下，预留的存储空间为10%，即只占用90%
reserved_storage_space = 10%
# log级别：alert error notice info debug
log_level=info
# 运行用户组，默认当前用户组
run_by_group=
# 运行用户，默认当前用户
run_by_user=
# 允许的host，例子如下
# "*" 表示所有
# 10.0.1.[1-15,20]
# host[01-08,20-25].domain.com:
# allow_hosts=10.0.1.[1-15,20]
# allow_hosts=host[01-08,20-25].domain.com
allow_hosts=*
# 同步内存日志到磁盘的间隔时间，默认10s
sync_log_buff_interval = 10
# 检查存储server存活的间隔时间，默认120s
check_active_interval = 120
# 线程栈大小 >= 64KB
thread_stack_size = 64KB
# 当存储server变化时，是否自动调整存储server的ip
storage_ip_changed_auto_adjust = true
# 存储server同步文件的最大延时，默认86400s(1天)
storage_sync_file_max_delay = 86400
# 存储server同步一个文件的最大时间
storage_sync_file_max_time = 300
# 是否用一个主文件存储多个小文件
use_trunk_file = false
# 最小的slot大小(多个小文件之间的空隙大小)，小于4k
slot_min_size = 256
# 最大的slot大小(多个小文件之间的空隙大小) > slot_min_size
# 当上传的文件大小 < slot_max_size时，那么该文件将合并到一个主文件
slot_max_size = 16MB
# 主文件大小 >= 4MB
trunk_file_size = 64MB
# 是否提前创建trunk文件
trunk_create_file_advance = false
# 创建trunk文件的基准时间
trunk_create_file_time_base = 02:00
# 创建trunk文件的间隔时间，默认86400s(1天)
trunk_create_file_interval = 86400
# 创建trunk文件的阈值空间
trunk_create_file_space_threshold = 20G
# 当加载trunk空间空间时，是否检查trunk空间占用情况，设置为true，启动会变慢 
trunk_init_check_occupying = false
# 是否忽略storage_trunk.dat
trunk_init_reload_from_binlog = false
# 压缩trunk binlog文件的间隔时间
# 0表示不压缩
trunk_compress_binlog_min_interval = 0
# 是否使用storage ID，而不是ip地址
use_storage_id = false
# 定义storage ids文件，相对或绝对路径
storage_ids_filename = storage_ids.conf
# 存储id类型，当use_storage_id=true时有效
## ip: 存储server的IP
## id: 存储server的ID
id_type_in_filename = ip
# 是否使用链接文件存储slave文件
store_slave_file_use_link = false
# 是否滚动错误日志文件
rotate_error_log = false
# 滚动错误日志文件的时间点
error_log_rotate_time=00:00
# 当错误日志文件超出该值时，滚动错误日志文件
rotate_error_log_size = 0
# 保存日志文件的天数
# 0表示不删除日志文件
log_file_keep_days = 0
# 是否使用连接池
use_connection_pool = false
# 连接最大空闲时间
connection_pool_max_idle_time = 3600
# tracker的http端口
http.server_port=8080
# 检查http server存活的时间间隔
http.check_alive_interval=30
# 检查http sever存活的类型
#   tcp : 仅连接http端口，不发请求
#   http: 发http请求，并需返回200
# default value is tcp
http.check_alive_type=tcp
# 检查http server的uri
http.check_alive_uri=/status.html
```

~~~
vi /etc/fdfs/storage.conf
## 是否禁用该配置文件
# false：开启
# true：禁用
disabled=false
# 组名称
group_name=group1
# 绑定地址
# 空字符表示绑定所有地址
bind_addr=
# 当连接其他存储server时，是否绑定地址 
client_bind=true
# 存储server端口
port=23000
# 连接超时
connect_timeout=30
# 网络超时
network_timeout=60
# 心跳
heart_beat_interval=30
# 磁盘使用报道的时间间隔
stat_report_interval=60
# 数据和日志目录
base_path=/mnt/fastdfs
# 最大并发连接数
max_connections=256
# 接收和发送数据的缓冲区大小
buff_size = 256KB
# 接受请求的线程数
accept_threads=1
# 执行请求的线程数
work_threads=4
# 磁盘读写是否分开
disk_rw_separated = true
# 每个存储基路径的读线程数
disk_reader_threads = 1
# 每个存储基路径的写线程数
disk_writer_threads = 1
# 当没有进行同步时, 多少毫秒后读取binlog
sync_wait_msec=50
# 当同步完一个文件后, 暂停多少毫秒
# 0 表示不调用usleep
sync_interval=0
# 每天存储同步的开始时间
sync_start_time=00:00
# 每天存储同步的结束时间
sync_end_time=23:59
# 同步多少个文件后，写入mark文件
write_mark_file_freq=500
# 路径(磁盘或挂载点)数
store_path_count=1
# 存储路径
store_path0=/mnt/fastdfs
# 子目录数
subdir_count_per_path=256
# tracker地址，可以配置多个tracker_server
tracker_server=192.168.50.106:22122
# 日志级别：alert error notice info debug
log_level=debug
# 运行进程的用户组，默认当前用户组
run_by_group=
# 运行进程的用户，默认当前用户
run_by_user=
# 允许的host，例子如下# "*" 表示所有
# 10.0.1.[1-15,20]
# host[01-08,20-25].domain.com:
# allow_hosts=10.0.1.[1-15,20]
# allow_hosts=host[01-08,20-25].domain.com
allow_hosts=*
# 文件分布模式
# 0: 轮询
# 1: 随机
file_distribute_path_mode=0
# 写多少个文件后，轮到下一个路径 
file_distribute_rotate_count=100
# 当写入多大文件时，调用fsync
# 0: 不调用
# other: 文件大小(byte)
fsync_after_written_bytes=0
# 同步内存日志到磁盘的间隔时间，默认10s
sync_log_buff_interval=10
# 同步binlog到磁盘的间隔时间，默认10s
sync_binlog_buff_interval=10
# 同步存储状态信息到磁盘的间隔时间，默认300s
sync_stat_file_interval=300
# 线程栈大小 >= 512KB
thread_stack_size=512KB
# 上传文件的优先级，tracker中使用
upload_priority=10
# the NIC alias prefix, such as eth in Linux, you can see it by ifconfig -a
# multi aliases split by comma. empty value means auto set by OS type
# default values is empty
if_alias_prefix=
# 是否检查文件重复
check_file_duplicate=0
# 文件签名方法
## hash: hash
## md5: MD5
file_signature_method=hash
# 存储文件索引的命名空间，check_file_duplicate=true时
key_namespace=FastDFS
# 设置与FastDHT保持长连接的数目，0表示使用短连接
keep_alive=0
# FastDHT server列表，需要安装FastDHT
##include /home/yuqing/fastdht/conf/fdht_servers.conf
# 是否记录访问日志
use_access_log = true
# 是否每天滚动访问日志文件
rotate_access_log = true
# 访问日志滚动时间点
access_log_rotate_time=00:00
# 是否每天滚动错误日志文件
rotate_error_log = true
# 错误日志滚动时间点
error_log_rotate_time=00:00
# 访问日志滚动大小
rotate_access_log_size = 0
# 错误日志滚动大小
rotate_error_log_size = 0
# 日志保留天数
log_file_keep_days = 7
# 同步文件时是否跳过无效的文件
file_sync_skip_invalid_record=false
# 是否使用连接池
use_connection_pool = false
# 连接最大空间时间
connection_pool_max_idle_time = 3600
# 域名
http.domain_name=
# http端口
http.server_port=8888
~~~

~~~
vi /etc/fdfs/client.conf
connect_timeout=30
network_timeout=60
base_path=/mnt/fastdfs_client
tracker_server=192.168.50.106:22122
log_level=debug
use_connection_pool = false
connection_pool_max_idle_time = 3600
load_fdfs_parameters_from_tracker=false
use_storage_id = false
storage_ids_filename = storage_ids.conf
~~~

4、配置自启动&&启动

~~~
chmod +x /etc/rc.d/rc.local
mkdir /mnt/fastdfs
echo '/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart' >> /etc/rc.d/rc.local
echo '/usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart' >> /etc/rc.d/rc.local
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart
~~~

5、测试

```
mkdir /mnt/fastdfs_client
/usr/bin/fdfs_monitor /etc/fdfs/client.conf
/usr/bin/fdfs_test /etc/fdfs/client.conf upload /var/log/yum.log
```

6、安装nginx

~~~
vi /etc/fdfs/mod_fastdfs.conf
connect_timeout=5
network_timeout=30
base_path=/mnt/fastdfs/logs
load_fdfs_parameters_from_tracker=true
storage_sync_file_max_delay = 86400
use_storage_id = false
storage_ids_filename = storage_ids.conf
tracker_server=192.168.50.106:22122
storage_server_port=23000
group_name=group1 			# 与该Storage的配置一致
url_have_group_name = true
store_path_count=1 			# 与该Storage的配置一致
store_path0=/mnt/fastdfs 	# 与该Storage的配置一致
log_level=debug
log_filename=/usr/local/nginx/logs/mod_fastdfs.log
response_mode=proxy
if_alias_prefix=
#include http.conf
flv_support = true
flv_extension = flv
group_count = 0
~~~

~~~
vi /etc/fdfs/http.conf
http.default_content_type = application/octet-stream
http.mime_types_filename=mime.types
http.anti_steal.check_token=false
http.anti_steal.token_ttl=900
http.anti_steal.secret_key=FastDFS1234567890
http.anti_steal.token_check_fail=/path/to/check_failed.jpg
~~~

~~~
cp /root/fastdfs-master/conf/mime.types /etc/fdfs/mime.types
~~~

~~~
vi /root/fastdfs-nginx-module-master/src/config
ngx_addon_name=ngx_http_fastdfs_module
HTTP_MODULES="$HTTP_MODULES ngx_http_fastdfs_module"
NGX_ADDON_SRCS="$NGX_ADDON_SRCS $ngx_addon_dir/ngx_http_fastdfs_module.c"
CORE_INCS="$CORE_INCS /usr/include/fastdfs /usr/include/fastcommon/"
CORE_LIBS="$CORE_LIBS -L/usr/local/lib -lfastcommon -lfdfsclient"
CFLAGS="$CFLAGS -D_FILE_OFFSET_BITS=64 -DFDFS_OUTPUT_CHUNK_SIZE='256*1024' -DFDFS_MOD_CONF_FILENAME='\"/etc/fdfs/mod_fastdfs.conf\"'"
~~~

~~~
yum -y install gcc gcc-c++ make zlib-devel pcre-devel openssl-devel
groupadd nginx
useradd -g nginx nginx --shell=/sbin/nologin
cd /root/nginx-1.12.1
./configure --prefix=/usr/local/nginx --add-module=/root/fastdfs-nginx-module-master/src --group=nginx --user=nginx
make && make install
chown -R nginx:nginx /usr/local/nginx
~~~

~~~
vi /usr/local/nginx/conf/nginx.conf
user nginx;
worker_processes 1;
events {
    worker_connections  1024;
}
 
http {
    server {
        listen 80;
        server_name localhost;
        location /group1/M00 { 
            root /mnt/fastdfs/data;
            if ($arg_oname != ''){
                add_header Content-Disposition "attachment;filename=$arg_oname";
            }
            ngx_fastdfs_module;
        }
    }
}
~~~





~~~

~~~

