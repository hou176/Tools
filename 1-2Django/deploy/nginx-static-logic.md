# Nginx部署静态数据

> 提示：

> 当Django运行在**`生产环境`**时，将**`不再提供静态文件的支持`**，需要将**`静态文件交给静态文件服务器`**。

> 我们需要收集项目中静态文件，并放到静态文件服务器中。

> 我们使用Nginx服务器作为静态文件服务器。

### 1. 收集项目静态文件

> **1.配置收集静态文件存放的目录**

```python
# 注释掉 STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

> **2.执行收集静态文件命令**

```bash
$ python manage.py collectstatic
```

<img src="/deploy/images/02收集静态文件.png" style="zoom:40%">
### 2.安装Nginx

```
#解压：
tar -zxvf nginx-1.8.1.tar.gz
#进入解压目录：
cd nginx-1.8.1
#配置：
./configure --prefix=/usr/local/nginx 
#编译：
make
#安装：
sudo make install
```

### 3. 部署Nginx服务器提供静态数据

> 提示：

* 美多商城的域名：`www.meiduo.site`
* 美多商城的端口：`80`

> **1.打开Nginx服务器配置文件**

```bash
$ sudo vim /usr/local/nginx/conf/nginx.conf
```

> **2.修改Nginx服务器配置文件**

```bash
http {
	......
	server { #静态服务
        listen       80;
        server_name  www.meiduo.site;
		# 1.精确查找
        location =/{
            root /home/python/Desktop/meiduo_mall/static;
            index index.html;
        }
		# 1.精确查找
        location =/index.html{
            root /home/python/Desktop/meiduo_mall/static;
        }
		# 2.范围查找
        location /static{
            root /home/python/Desktop/meiduo_mall;
        }
        location /detail{
            root /home/python/Desktop/meiduo_mall/static;
        }
	}
}
```

> **3.启动Nginx服务器**

```bash
# 检查配置文件
$ sudo /usr/local/nginx/sbin/nginx -t
# 首次启动
$ sudo /usr/local/nginx/sbin/nginx
# 重启
sudo /usr/local/nginx/sbin/nginx -s reload
# 停止
$ sudo /usr/local/nginx/sbin/nginx -s stop
```

> **4.测试静态文件访问**

```
# 静态主页
http://www.meiduo.site/
# 静态详情页
http://www.meiduo.site/detail/1.html
```
