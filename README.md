相关软件版本：
- PHP 7.2
- MySQL 5.7
- Nginx 1.12
- Redis 3.2


用到的 PHP 拓展(2018.2.9更新)：
- redis 3.1.4
#- Phalcon 3.3.1
- swoole 4.2.9

## 使用
### 1.安装 Docker，Docker-compose  
- Docker，详见官方文档：https://docs.docker.com/engine/installation/linux/docker-ce/centos/
- docker-compose，文档：https://docs.docker.com/compose/install/
```
sudo pip install -U docker-compose
```
进入 docker-compose.yml 所在目录：
执行命令：
```
docker-compose up
```  

如果没问题，下次启动时可以以守护模式启用，所有容器将后台运行：  
```
docker-compose up -d
``` 

使用 docker-compose 基本上就这么简单，Docker 就跑起来了，用 stop，start 关闭开启容器服务。  
更多的是在于编写 dockerfile 和 docker-compose.yml 文件。 

可以这样关闭容器并删除服务：
```
docker-compose down
```
