###  安装kinbana1 1231

```bash
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.12.0-linux-x86_64.tar.gz
tar zxvf kibana-7.12.0-linux-x86_64.tar.gz
cd kibana-7.12.0-linux-x86_64
cp config/logstash-sample.conf config/logstash.conf
vim config/logstash.conf
```

> kafka 转入到kafak为例

```
# Sample Logstash configuration for creating a simple
# Beats -> Logstash -> Elasticsearch pipeline.

input {
  kafka {
    bootstrap_servers => "kafka1.connect.host:9092,kafka2.connect.host:9092,kafka3.connect.host:9092" #这里可以是kafka集群，如"192.168.149.101:9092,192.168.149.102:9092,192.168.149.103:9092"
    group_id => "host_log"
    client_id => "client_1" #注意，多台logstash实例消费同一个topics时，client_id需要指定不同的名字
    auto_offset_reset => "latest"
    topics => ["login","logout"]
    add_field => {"logs_type" => "10006"}
   # codec => json { charset => "UTF-8" }
    }   

}

output {
   kafka {
            acks => "1" 
            bootstrap_servers => "127.0.0.1:9092"
            topic_id => "login"
            batch_size => 10
            #codec => "json" #写入的时候使用json编码，因为logstash收集后会转换成json格式
         }   
    

}
```

* 启动logstatsh并查看信息

```
  bin/logstash -f config/logstash.conf
  bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic login
```

  

* 进入到项目根目录

``` bash
composer create-project hyperf/hyperf-skeleton .
composer require hyperf/model-cache
```

### 热重载启动

``` bash
conposer require hyperf/watcher
php bin/hyperf.php server:watch
```

### 启动方式

```
php bin/hyperf.php start
```

### 框架引入kafka和初始化消费者

```  bash
composer require hyperf/kafka
php bin/hyperf.php vendor:publish hyperf/kafka 
php bin/hyperf.php gen:kafka-consumer KafkaConsumer
```

