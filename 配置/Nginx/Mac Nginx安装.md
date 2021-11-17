### 一. brew 安装

打开mac终端，输入以下命令：

```ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```
安装完成，路径信息：

```apl
安装路径：/usr/local/Cellar/nginx/1.17.0
配置文件路径：/usr/local/etc/nginx/nginx.conf
服务器默认路径:/usr/local/var/www
```

brew 升级命令：

```sudo brew update```

### 二. nginx安装、启动

**1、打开mac终端，输入以下命令执行nginx安装：**

```sudo brew install nginx```
ps: 如果之前安装过，可以先卸载：

```sudo brew uninstall nginx```

**2、启动nginx服务**

```sudo brew services start nginx```

**3、查看nginx是否启动成功**
Nginx 默认8080端口，访问localhost:8080，若出现欢迎界面，说明成功安装和启动
或者在终端输入： ps -ef|grep nginx 查看

**4、查看nginx版本**

```nginx -v```

**5、重新启动nginx服务**

```sudo brew services restart nginx```

**6、关闭nginx服务**

```sudo brew services stop nginx```

在终端中输入

​     

如果执行的结果是

```json
501 15800 1 0 12:17上午 ?? 0:00.00 nginx: master process /usr/local/Cellar/nginx/1.8.0/bin/nginx -c /usr/local/etc/nginx/nginx.conf

501 15801 15800 0 12:17上午 ?? 0:00.00 nginx: worker process

501 15848 15716 0 12:21上午 ttys000 0:00.00 grep nginx
```

表示已启动成功，如果不是上图结果，在终端中执行

```/usr/local/Cellar/nginx/1.17.0/bin/nginx -c /usr/local/etc/nginx/nginx.conf```

命令即可启动nginx。
这时候如果成功访问localhost:8080，说明成功安装和启动好了。

### 三. 停止nginx

终端输入ps -ef|grep nginx获取到nginx的进程号, 注意：是找到“nginx:master”的那个进程号。

```
kill -QUIT 15800 (正常停止，即不会立刻停止)
Kill -TERM 15800 （快速停止）
Kill -INT 15800 （快速停止）
```

### 四. 其他命令

1、修改 nginx.conf 后，重载配置文件

```sudo nginx -s reload```
2、停止 nginx 服务器

```sudo nginx -s stop```

