笔者最初使用的CentOS6安装成功，但是之后安装node环境的时候，踩坑太多，于是重装系统，转为CentOS7（CentOS 7.5 64位）。

[<font color=#00a4ff>CentOS6安装LNMP+wordpress腾讯云官方链接</font>][CentOS6]
[CentOS6]: https://cloud.tencent.com/document/product/213/8044

#### 1. 安装Nginx

```bash
yum install nginx		# 安装nginx
systemctl start nginx		# 启动nginx
systemctl enable nginx.service		# 设置为开机启动
```

设置完成后再浏览器输入：`*.*.*.*`(你的公网ip)，看到nginx页面则表示安装成功

#### 2. 安装MySQL

```bash
rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum repolist enabled | grep “mysql.-community.”
yum -y install mysql-community-server	# 以上安装的社区版

systemctl start mysqld	# 启动mysql
mysql_secure_installation	# 安全设置向导，设置root用户密码，删除匿名账号，取消root用户远程登陆，删除test库和对test库的访问权限，重载授权表使修改生效

mysql -uroot -p		# 用上面设置的密码登陆mysql
mysql>create database wordpress;	# 创建wordpress数据库
mysql>use wordpress;	#切换到wordpress数据库
mysql>exit	# 退出mysql
```

#### 3. 安装PHP

```bash
yum install php-fpm php-mysql	# php-fpm使php与nginx关联，php-mysql为php与mysql关联
systemctl start php-fpm 	# 启动php-fpm
systemctl enable php-fpm	# 设置开机启动
```

#### 4. 修改nginx配置

`vi /etc/nginx/nginx.conf` 打开nginx主配置文件，按i进入编辑模式，修改其中的sever部分为以下内容

```text
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  binnear.com.cn; # 此处修改为你的域名或者公网IP
    root         /usr/share/nginx/html;	# 你的站点的目录

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
        index index.php index.html index.htm;
        try_files $uri $uri/ /index.php?$args;
    }

    rewrite /wp-admin$ $scheme://$host$uri/ permanent;

    location ~* ^.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
                access_log off; log_not_found off; expires max;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

输入完成后，按`ESC`进入命令模式，输入`:wq`，回车保存并退出后，重载nginx

```bash
systemctl reload nginx
```

#### 5. 测试php-fpm是否安装成功

输入`vi /usr/share/nginx/html/index.php`，按i进入编辑模式，输入以下内容：

```php
<?php
	echo "<title>Test Page</title>";
	echo "Hello World!";
?>
```

输入完成后，按`ESC`进入命令模式，输入`:wq`，回车保存并退出；
接着在浏览器中输入`http：//当前服务器公网IP/index.php`；
如果浏览器中出现`Hello World!`则表示配置成功,可继续进行以下步骤，若出现文件下载弹窗，则配置失败，检查以上步骤是否出错。

#### 6. 安装wordpress与配置wordpress

```bash
wget https://cn.wordpress.org/wordpress-4.7.4-zh_CN.tar.gz	# 下载wordpress安装包
tar zxvf wordpress-4.7.4-zh_CN.tar.gz	# 解压缩
cd wordpress/	# 进入到wordpress目录
cp wp-config-sample.php wp-config.php	# 复制wp-config-sample.php并重命名为wp-config.php
vim wp-config.php	# 打开该文件
```

找到mysql设置的配置部分，按i进入编辑模式，将步骤2中配置的mysql信息填入以下内容中

```text
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');	# 数据库名

/** MySQL database username */
define('DB_USER', 'root');	# 数据库用户名

/** MySQL database password */
define('DB_PASSWORD', '123456');	# 数据库密码

/** MySQL hostname */
define('DB_HOST', 'localhost');	# 一般不修改
```

输入完成后，按`ESC`进入命令模式，输入`:wq`，回车保存并退出；

```bash
rm /usr/share/nginx/html/index.html	# 删除nginx中的主页文件
mv * /usr/share/nginx/html/	# 将wordpress文件移动web站点的根目录
```

完成后，在浏览器中输入`http://你的主机IP或者域名/wp-admin/install.php`,进入到wordpress的配置页面，输入网站标题，用户名和密码后，就可以进入wordpress后台管理界面，到此便大功告成。

#### 7. 采坑记录

之前我们下载的wordpress并不是最新版，wordpress后台会提示更新，点击更新后，会进入一个界面，让你输入ftp的主机地址和用户名密码，解决方案：

```bash
usermod -s /sbin/nologin ftpusr		# 创建ftp用户
passwd ftpusr	# 设置ftpusr用户密码
```

`vi /usr/share/nginx/html/wp-config.php` 打开wp-config.php并在该文件后面添加以下内容：

```text
efine("FS_METHOD","direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);
```

接着在wordpress页面输入以下内容：

```text
主机名：你的公网ip:20
用户名：ftpusr
密码：你设置的密码
```

成功后会跳转到更新界面，如果这个时候提示无法创建目录，则可输入以下内容：

```bash
chown -Rf  apache:root /usr/share/nginx/html/
```

重新进入更新页面，则可以顺利更新。