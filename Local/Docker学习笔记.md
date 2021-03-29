## Docker环境搭建

[DockerHub](https://registry.hub.docker.com/)

### [安装Docker](https://docs.docker.com/engine/install/centos/)

1. 卸载系统之前的docker

   ```
   sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
               
   ```

2. 安装依赖的包

   ```
   sudo yum install -y yum-utils
   ```

3. 配置镜像

   ```
   sudo yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   ```

4. 安装Docker

   ```
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

5. 启动Docker服务

   ```
   sudo systemctl start docker
   ```

6. 检测Docker是否安装成功

   ```
   docker -v
   ```

7. 查看下载镜像

   ```
   sudo docker images
   ```

8. 设置开机自启动

   ```
   sudo systemctl enable docker
   ```

### 配置Docker[镜像加速](https://cr.console.aliyun.com/cn-qingdao/instances/mirrors)

针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

1. 创建目录

    ```
    sudo mkdir -p /etc/docker
    ```

2. 配置镜像加速器地址
    ```
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://578xeysa.mirror.aliyuncs.com"]
    }
    EOF
    ```

3. 重启Docker后台线程
    ```
    sudo systemctl daemon-reload
    ```

4. 重启Docker服务
    ```
    sudo systemctl restart docker
    ```

### 安装MySQL环境

1. 拉去MySQL 5.7 的镜像

    ```
    sudo docker pull mysql:5.7
    ```
    
2. 检查下载的镜像

    ```
    sudo docker images
    ```

3. 创建实例并启动

    ```
    sudo docker run -p 3306:3306 --name mysql \
    -v /mydata/mysql/log:/var/log/mysql \
    -v /mydata/mysql/data:/var/lib/mysql \
    -v /mydata/mysql/conf:/etc/mysql \
    -e MYSQL_ROOT_PASSWORD=root \
    -d mysql:5.7
    ```

    > `-p 3306:3306` 将容器的3306端口映射到主机的3306端口
    >
    > --name指定容器名字
    >
    > `-v`目录挂载
    >
    > `-e`设置mysql参数，初始化root用户的密码
    >
    > `-d`后台运行

4. 查看docker正在运行的容器

   ```
   docker ps
   ```

5. 进入容器内部

   ```
   docker exec -it mysql bin/bash
   # exit
   ```

6. 查看MySQL位置

   ```
   whereis mysql
   ```

7. 修改MySQL配置文件

   ```
   vi /mydata/mysql/conf/my.cnf
   ```

   ```
   [client]
   default-character-set=utf8
   [mysql]
   default-character-set=utf8
   [mysqld]
   init_connect='SET collation_connection = utf8_unicode_ci'
   init_connect='SET NAMES utf8'
   character-set-server=utf8
   collation-server=utf8_unicode_ci
   skip-character-set-client-handshake
   skip-name-resolve
   ```

   ```
   docker restart mysql
   ```

   

### 安装Redis环境

1. 拉取镜像

   ```
   docker pull redis
   ```

2. 先配置目录，注意`redis.conf`是文件

   ```
   mkdir -p /mydata/redis/conf
   touch /mydata/redis/conf/redis.conf
   ```

3. 安装，挂载目录

   ```
   docker run -p 6379:6379 --name redis \
   -v /mydata/redis/data:/data \
   -v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
   -d redis redis-server /etc/redis/redis.conf
   ```

4. 直接进去redis客户端

   ```
   docker exec -it redis redis-cli
   ```

5. 配置redis持久化

   ```
   vi /mydata/redis/conf/redis.conf
   ```

   插入以下内容

   ```
   appendonly yes
   ```

   ```
   docker restart redis
   ```

6. 设置redis容器在docker启动的时候启动

   ```
   docker update redis --restart=always
   ```

   

### 安装Nacos环境

1. 拉取镜像

   ```
   docker pull nacos/nacos-server
   ```

2. 安装

   ```
   docker run -d -p 8848:8848 --env MODE=standalone --name nacos nacos/nacos-server
   ```

   

