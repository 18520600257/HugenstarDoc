# kafka

## 安装

``` bash
mkdir /usr/local/kafka && cd /usr/local/kafka
wget https://mirrors.bfsu.edu.cn/apache/kafka/2.7.0/kafka_2.13-2.7.0.tgz
tar zxvf kafka_2.13-2.7.0.tgz
cd kafka-2.7.0-src
```



**修改zookeeper log目录 端口配置**

``` bash
vim config/zookeeper.properties
```

**启动 zookeeper**

``` bash
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

**修改kafka默认配置**

```bash
vim config/server.properties
```

```
日志保存时间 (hours|minutes)，默认为7天（168小时）。

超过这个时间会根据policy处理数据。

bytes和minutes无论哪个先达到都会触发。

log.retention.minutes：在删除日志文件之前保留日志文件的分钟数（以分钟为单位），如果未设置，则使用log.retention.hours中的值
log.retention.ms：在删除日志文件之前保留日志文件的毫秒数（以毫秒为单位），如果未设置，则使用log.retention.minutes中的值

log.retention.bytes：日志数据存储的最大字节数。超过这个时间会根据policy处理数据
log.cleanup.policy： 日志清理策略[compact, delete](默认是delete)

log.cleaner.enable：是否开启压缩(默认：true)
log.cleaner.delete.retention.ms ：对于压缩的日志保留的最长时间(默认：86400000ms)

```

**启动kafka**

```bash
bin/kafka-server-start.sh -daemon config/server.properties
```

> 未加 -daemon 可进入console模式  zookeeper 默认端口为2181 kafka默认端口为9092

## kafka插件cmak使用

[插件地址](https://github.com/yahoo/CMAK)

下载解压后**必须**修改conf/conf/application.conf 中zookeeper地址

``` bash
kafka-manager.zkhosts="localhost:2181"
cmak.zkhosts="localhost:2181"
```

**启动**

``` bash
bin/cmak
```

浏览器访问http://host:9000

## supervisor

```bash
wget https://bootstrap.pypa.io/pip/3.5/get-pip.py
python get-pip.py
pip install supervisor
mkdir -p /usr/local/supervisord/conf.d/
echo_supervisord_conf > /etc/supervisord.conf
vim /etc/supervisord.conf
```

修改配置

```
[inet_http_server]         ; inet (TCP) server disabled by default
port=0.0.0.0:9001        ; ip_address:port specifier, *:port for all iface
username=admin              ; default is no username (open server)
password=Hugenstar2021               ; default is no password (open server)

logfile=/data/logs/supervisord.log ; main log file; default $CWD/supervisord.log

[include]
files = /usr/local/supervisord/conf.d/*.conf
```



添加脚本

```bash
/etc/init.d/supervisord
```

复制内容

```bash
#!/bin/sh
#
# /etc/init.d/supervisord
#
# Supervisor is a client/server system that
# allows its users to monitor and control a
# number of processes on UNIX-like operating
# systems.
#
# chkconfig: - 64 36
# description: Supervisor Server
# processname: supervisord

# Source init functions
. /etc/rc.d/init.d/functions
 
prog="supervisord"
  
prefix="/usr/local/bin"
exec_prefix="${prefix}"
prog_bin="${exec_prefix}/supervisord"
PIDFILE="/var/run/$prog.pid"
  
  
start()
{
       echo -n $"Starting $prog: "
       daemon $prog_bin -c /etc/supervisord.conf --pidfile $PIDFILE
       [ -f $PIDFILE ] && success $"$prog startup" || failure $"$prog startup"
       echo
}
  
stop()
{
       echo -n $"Shutting down $prog: "
       [ -f $PIDFILE ] && killproc $prog || success $"$prog shutdown"
       echo
}
  
case "$1" in
  
 start)
   start
 ;;  
  
 stop)
   stop
 ;;  
  
 status)
       status $prog
 ;;  
  
 restart)
   stop
   start
 ;;
 *)
   echo "Usage: $0 {start|stop|restart|status}"
 ;;
esac

```

加入服务

```bash
chmod +x /etc/init.d/supervisord
chkconfig --add supervisord
chkconfig supervisord on
```

> 访问http://host:9001

加入脚本到/usr/local/supervisord/conf.d/

```
[program:kafka-cmak]
command=/usr/local/kafka/cmak-3.0.0.5/bin/cmak -java-home /usr/local/java/jdk-11.0.10
directory=/usr/local/kafka/cmak-3.0.0.5/
numprocs=1
autostart=true
```
启动服务
```
service supervisord start
```



