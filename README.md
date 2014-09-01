swoole
======

swoole扩展研究



环境搭建步骤


sudo -s
LANG=C
yum -y install gcc gcc-c++ autoconf


1、安装Apache

   mkdir /usr/local/webserver
   
   tar zxvf httpd.tar.gz
   
   ./configure --prefix=/usr/local/webserver/httpd
   
   make 
   
   make install
   
   
   启动apache
   
   /usr/local/webserver/httpd/bin/apachectl start




2、安装php
   
   （1）先安装libxml
    tar zxvf libxml2-2.6.30.tar.gz
    cd libxml2-2.6.30
    ./configure --prefix=/usr/local/webserver/libxml2
    make&&make install

    （2）安装php
  tar zxvf php-5.5.15.tar.gz 
  
   cd php-5.5.15
   
     ./configure --prefix=/usr/local/webserver/php --with-config-file-path=/usr/local/webserver/php/etc --with-apxs2=/usr/local/webserver/httpd/bin/apxs  --with-libxml-dir=/usr/local/webserver/libxml2 --enable-soap --enable-socket    

     make && make  install
     
     cp php.ini-dist /usr/local/webserver/php/etc/php.ini  

     #cp /usr/local/webserver/httpd/bin/apachectl     /etc/rc.d/init.d/httpd
     #vi /etc/rc.d/init.d/httpd
     添加以下注释信息：
	# chkconfig: 345 85 15
	#  description: Activates/Deactivates Apache Web Server
	第一行3个数字参数意义分别为：哪些Linux级别需要启动httpd(3,4,5)；启动序号(85)；关闭序号(15)。
     chkconfig --add httpd
     chkconfig httpd on
     service httpd restart


    //创建配置文件 
    # vi /usr/local/webserver/httpd/conf/httpd.conf
    //使用vi编辑apache配置文件
    Addtype application/x-httpd-php .php .phtml  
    AddType application/x-httpd-php-source  .phps
    

3、安装swoole扩展
 cd swoole
phpize
./configure --with-php-config=/usr/local/webserver/php/bin/php-config
make &&  make install

extension_dir=/usr/local/webserver/php/lib/php/extensions/no-debug-non-zts-20121212/

DirectoryIndex index.php index.html

修改php.ini加入extension=swoole.so




service httpd restart


修改PHP环境变量
vi /etc/profile

在文件末尾加上如下两行代码 （shift+g)


PATH=/usr/local/webserver/php/bin:$PATH

export PATH
马上生效：
#source /etc/profile


搭建完毕

