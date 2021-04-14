###  框架安装
> 框架文档地址 https://hyperf.wiki/2.1/#/
* 首先需要安装好composer
* 进入到项目根目录

``` bash
composer create-project hyperf/hyperf-skeleton .
composer require hyperf/model-cache
composer require hyperf/crontab
composer require hyperf/async-queue 
```

### 热重载启动
> hyperf框架热重载启动为重启服务 非reload
``` bash
conposer require hyperf/watcher
php bin/hyperf.php server:watch
```

### 普通方式启动

```
php bin/hyperf.php start
```

### 框架引入kafka和初始化消费者

```  bash
composer require hyperf/kafka
php bin/hyperf.php vendor:publish hyperf/kafka 
php bin/hyperf.php gen:kafka-consumer KafkaConsumer
```

