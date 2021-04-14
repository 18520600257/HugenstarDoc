<<<<<<< HEAD
﻿## node.js安装 123123
=======
﻿## node.js安装1234
>>>>>>> 6e8f9bcc7bb393cbb25c87f5f01a3d1311ffdb4c
地址：https://nodejs.org/en/
> 下载最新版本 注意CentOs6.5可能只支持到10
``` bash
mkdir /usr/local/node && cd $_
wget https://nodejs.org/dist/v14.16.1/node-v14.16.1-linux-x64.tar.xz
tar xvf node-v14.16.1-linux-x64.tar.xz
ln -s /usr/local/node/node-v14.16.1-linux-x64/bin/node /usr/bin/node
ln -s /usr/local/node/node-v14.16.1-linux-x64/bin/npm /usr/bin/npm
#版本检查
npm -v 
``` 
   npm设置淘宝源

```	bash
npm config set registry https://registry.npm.taobao.org
```
## docsify安装

​地址：
https://docsify.js.org/

安装
``` bash
npm i docsify-cli -g
```
初始化
```
docsify init ./docs
docsify serve docs
```
访问 http://localhost:3000
> **提示：**根目录下的README.md 默认为index <br/>
> _sidebar.md为侧边栏 <br/>
> README.md为主页

