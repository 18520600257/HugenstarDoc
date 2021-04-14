### 安装

官网下载源码包
https://www.oracle.com/java/technologies/javase-jdk11-downloads.html

解压缩
``` bash
tar zxvf jdk-11.0.10_linux-x64_bin.tar.gz
```
编辑profile
```bash
vim /etc/profile
```
在最后加入环境变量

```
 export JAVA_HOME=/usr/local/java/jdk-11.0.10
 export CLASSPATH=.:$JAVA_HOME/lib:$CLASSPATH
 export PATH=$JAVA_HOME/bin:$PATH
```

刷新配置

```bash
source /etc/profile
```

测试是否成功

```bash
java -version
```

> 如果不成功尝试重新登陆

