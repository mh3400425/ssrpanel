# ssrpanel
由于ssrpanel闭源，在这里保存ssrpanel最后一个开源版本作为备份，版本号V4.8.0。
此版本取自胖虎github，未作任何更改或优化。

宝塔安装好，新建网站，域名要真实，不是乱填
进入网站根目录

# 使用宝塔面板的好处
可视化管理；一键安装网站环境；自动更改时区以及校时（SSRPanel 要求多节点之间时区相同，时间一样）；端口管理方便。


安装宝塔面板


首先要安装宝塔面板啦，复制下面这条命令在控制台中执行就行了。

yum install -y wget && wget -O install.sh https://github.com/mh3400425/baota.git && bash install.sh

安装完成之后你会得到一个访问网址，以及用户名和密码，在浏览器中打开给定网址就能访问宝塔面板的控制台了。


安装 LNMP


第一次打开宝塔控制台时会提示你安装网站运行环境。

PHP 版本选择 7.1。

SSRPanel 推荐使用MySQL 5.6+，低内存服务器就老老实实选择MySQL 5.5吧。


添加网站


在宝塔面板 -> 网站 -> 添加站点中来添加一个新的站点。配置的信息都很重要，要记下来。

域名：ssrpanel.com；然后添加服务器ip：端口 转接用

根目录：你的网站文件在服务器上的位置，要记住自己网站的根目录；www/wwwroot/ssrpanel.com

FTP：是否创建 FTP 用户，可根据需求选择；

数据库：类型选择MySQL，编码选择utf8mb4；

数据库设置：数据库的用户名：ssrpanel 和 密码：ssrpanel；

PHP 版本：PHP-71。


导入数据库


在宝塔面板 -> 数据库中，找到你刚刚创建的数据库，点击导入 -> 从本地上传。

数据库文件位于sql/db.sql，你可以从 Github 上下载到。


SWAP 配置


SSRPanel 依赖 phpfileinfo 扩展，phpfileinfo 的安装对内存容量有一定的要求，如果内存太小的话会安装失败，所以小内存机器可以通过添加 Swap 的方式增大可用内存容量。

在宝塔面板 -> 首页 ->Linux 工具箱 ->Swap / 虚拟内存中，添加 Swap 为2048 MB。

安装 phpfileinfo 扩展

在宝塔面板 -> 软件管理 ->PHP-7.1-> 安装扩展中，找到名为fileinfo的扩展并安装。


删除禁用函数


在宝塔面板 -> 软件管理 ->PHP-7.1-> 禁用函数中，删除 proc_开头的所有函数。



# 教程开始

注意替换你自己的网站路径

cd www/wwwroot/ssrpanel.com


克隆文件到目录

方法1
git git clone https://github.com/mh3400425/XiuGouDisco-master.git

方法1
wget -N --no-check-certificate https://github.com/hubooy/ssrpanel-deploy.git

更改权限

chown -R www:www storage/

chmod -R 755 storage/

安装依赖-报错取php7.1禁用报错函数

php composer.phar install

生成秘钥

php artisan key:generate

# 编写配置文件

www/wwwroot/config/database.php 修改对应的MySQL信息

'mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'ssrpanel'),
            'username' => env('DB_USERNAME', 'ssrpanel'),
            'password' => env('DB_PASSWORD', 'ssrpanel'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => env('DB_STRICT', true),
            'engine' => null,
        ],

因为网站需要用到 MySQL 数据库，这一步我们配置数据库连接信息。

把.env.example复制一份命名为.env，按需更改配置。

可以在宝塔面板 -> 文件里操作，也可以在服务器控制台操作。

cp .env.example .env

vi .envcp .env.example .env

vi .env
 
 
DB_HOST: 数据库地址，如果在本机就是 127.0.0.1；

DB_PORT: 数据库端口，默认 3306；

DB_DATABASE: 数据库名；

DB_USERNAME: 数据库用户名；

DB_PASSWORD: 数据库密码；

REDIRECT_HTTPS: 是否启用 HTTPS。重要！重要！重要！

运行目录

宝塔面板 -> 网站 -> 设置 -> 运行目录 -> 选择/public。防跨站攻击关闭

伪静态

宝塔面板 -> 网站 -> 设置 -> 伪静态 -> 选择laravel5-> 保存。


定时任务

SSRPanel 需要定时任务来完成自动维护。

必须给 www 用户创建一个目录，否则没法用其身份去运行crontab

mkdir /home/www && chown -R www:www /home/www/

注意运行权限，必须跟SSRPanel项目权限一致，否则出现各种莫名其妙的错误

宝塔默认生成的用户是www

crontab -e -u www

 crontab加入下面的命令，自行替换路径

* * * * * php /www/wwwroot/网站路径/artisan schedule:run >> /dev/null 2>&1

# 打开网站

在打开网站之前，要先重启一下服务器，使网站环境重载配置。

如果以上步骤都没出错，那么现在在浏览器输入你的域名就能访问网站了。

默认登录账号是admin，密码是123456。

修改管理员密码

点击右上角头像 -> 个人设置 -> 修改密码。

添加节点

SSRPanel-> 管理面板 -> 节点管理 -> 添加节点。

基础信息

节点名称：起个名字；

绑定域名：如果用域名解析可以填，不过没有可以忽略；

SSH 端口：填服务器的 SSH 端口，用于 TCP 阻断检测；

IPV4 地址：填 SSR 服务器的 IP 地址；

标签：起一个标签名字，用户是通过标签与服务器关联起来的
