## 说明

### 部署

1. 通过脚本安装docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
# 先执行dry-run查看是否可以安装
sudo sh ./get-docker.sh --dry-run
# 上一步没问题就执行
sudo sh get-docker.sh
```

2. 安装docker-compose

```bash
curl -SL https://github.com/docker/compose/releases/download/v2.23.1/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
# 设置可执行权限
sudo chmod +x /usr/local/bin/docker-compose
# 查看是否安装成功
docker-composer --version
# 如果提示命令没找到需要更新环境变量
```

3. 修改项目配置.env文件

`cp .env.exmaple .env` 然后在.env里面编辑需要修改的项, 数据库配置和接口相关配置都在.env里面

4. 修改docker-compose.yml文件

`docker-compose.yml`文件是各服务的配置，如redis和mysql的密码等, 如果这里修改了数据库的密码, 那么.env文件的相关项也需要修改

5. 启动服务

在项目目录下执行`docker-compose up -d`命令来启动服务, 服务启动后修改storage文件夹的权限 `chmod -R 777 storage/`

6. 创建数据库

在.env文件修改完数据库和redis密码后，执行 `docker-compose exec app php artisan migrate`来创建数据库和表

7. 生成项目key

执行 `docker-compose exec app php artisan key:generate` 生成唯一key，用于加密等。

8. 禁用调试模式

将.env文件的`APP_DEBUG=true`改为false，在生产环境禁用debug模式。

将`APP_ENV=local`设置为prod

9. 提升性能(可选)

`docker-compose exec app php artisan config:cache`

`docker-compose exec app php artisan route:cache`

### docker配置说明 

docker中的各种服务主要通过docker-compose.yml和dockerfile来配置。

项目docker文件夹下包含各种服务的配置，php, mysql, nginx的相关配置都在这个文件夹下。如要修改nginx的配置，就在docker/nginx/conf.d/nginx.conf文件中，修改完成后需重启docker服务