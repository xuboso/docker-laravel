# docker-compose相关

### 安装

curl -SL https://github.com/docker/compose/releases/download/v2.23.1/docker-compose-linux-x86_64 -o docker-compose

### docker-compose重启删除volume

docker-compose down -v

### 项目部署步骤

1. 代码下载完后修改storage的权限 `chmod -R 777 storage/`
2. 更新.env文件
3. 执行 `docker-compose up -d`

### 配置MySQL

[https://gist.github.com/ksundong/7f695db1486d1ab28f854d3f0d1dcf66](https://gist.github.com/ksundong/7f695db1486d1ab28f854d3f0d1dcf66)

[https://stackoverflow.com/questions/3513773/change-mysql-default-character-set-to-utf-8-in-my-cnf](https://stackoverflow.com/questions/3513773/change-mysql-default-character-set-to-utf-8-in-my-cnf)

### 权限问题

为什么chown不生效，说的很清楚
https://github.com/docker/compose/issues/8706

https://stackoverflow.com/questions/26145351/why-doesnt-chown-work-in-dockerfile

docker exec -u 0 app chmod -R 777 /var/www/storage/

app是container_name

https://stackoverflow.com/questions/26145351/why-doesnt-chown-work-in-dockerfile

### 配置定时任务

* * * * * docker exec -i [CONTAINER_NAME] php /var/www/[PROJECT_FOLDER]/artisan schedule:run >> /dev/null 2>&1

https://laracasts.com/discuss/channels/servers/run-the-scheduler-in-a-docker-image

### 配置php-fpm

在容器中执行`php-fpm -tt`查看php-fpm的配置

https://github.com/docker-library/php/issues/1242

https://stackoverflow.com/questions/65693404/how-can-i-override-ddevs-php-fpm-conf-or-pool-d-www-conf

https://ibrahimgunduz34.medium.com/doesnt-php-fpm-docker-image-care-about-your-configuration-changes-97dfd0ca8169

php-fpm.conf文件必须要全部覆盖掉zz-docker.conf，不能只添加部分命令

### 安装composer依赖

https://medium.com/@lukaspereyra8/docker-compose-php-composer-the-missing-vendors-folder-issue-66faa5475c59


### docker contaier不能执行ps命令

https://stackoverflow.com/questions/26982274/ps-command-doesnt-work-in-docker-container

要么在构建dockerFile的时候安装procps

`RUN apt-get update && apt-get install -y procps && rm -rf /var/lib/apt/lists/*`

要么使用`docker top <container ID>`
