安装zlib 1.2.8:

`wget http://zlib.net/zlib-1.2.8.tar.gz`

`tar -zxvf zlib-1.2.8.tar.gz`

`mkdir /usr/local/zlib`

`cd zlib-1.2.8`

`./configure --prefix=/usr/local/zlib`

`make`

`sudo make install`

新建zlib.conf

`sudo vi /etc/ld.so.conf.d/zlib.conf`

加入以下内容

`/usr/local/zlib/lib`

加载刚才编译安装的zlib生成的库文件

`sudo ldconfig`

但由于centos的zlib-devel找不到1.2.8的源码,所以这里我就安装了1.2.7

`rpm -ivh zlib-1.2.7-15.el7.x86_64.rpm`

但提示
>file /usr/lib64/libz.so.1.2.7 from install of zlib-1.2.7-15.el7.x86_64 conflicts with file from package zlib-1.2.7-13.el7.x86_64

于是加上replacefiles参数

`rpm -ivh zlib-1.2.7-15.el7.x86_64.rpm --replacefiles`


安装zlib-devel:

`sudo rpm -ivh zlib-1.2.7-15.el7.x86_64.rpm`

是否已经安装好zlib：

`whereis zlib`
