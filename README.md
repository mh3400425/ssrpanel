# ssrpanel
由于ssrpanel闭源，在这里保存ssrpanel最后一个开源版本作为备份，版本号V4.8.0。
此版本取自胖虎github，未作任何更改或优化。

宝塔安装好，新建网站，域名要真实，不是乱填
进入网站根目录

# 教程开始
注意替换你自己的网站路径
cd www/wwwroot/网站路径

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

REDIRECT_HTTPS: 是否启用 HTTPS。

运行目录

宝塔面板 -> 网站 -> 设置 -> 运行目录 -> 选择/public。

伪静态

宝塔面板 -> 网站 -> 设置 -> 伪静态 -> 选择laravel5-> 保存。
