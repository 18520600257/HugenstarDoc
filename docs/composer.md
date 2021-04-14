### 安装
``` bash
php1 -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```




### 使用阿里云镜像

``` bash
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

