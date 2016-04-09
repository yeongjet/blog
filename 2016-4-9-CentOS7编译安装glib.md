编译前先要有python2.7.11 (参考CentOS7编译安装Python2)




编译安装

`mkdir /usr/local/glib`

`./configure --prefix=/usr/local/glib --with-pcre=system`

提示
>configure: error: *** Working zlib library and headers not found ***

需要先安装zlib和zlib-devel,参考《2016-4-9-CentOS7安装zlib-devel和zlib》

再次执行,提示
>No package 'libffi' found

那么先安装
libffi-3.2.1

(http://www.linuxfromscratch.org/blfs/view/svn/general/libffi.html)

很简单,三部曲就搞定了

如果问题依旧,可能要设置环境变量

`export LIBFFI_CFLAGS=-I/usr/local/glib/lib/libffi-3.0.11/include`

`export LIBFFI_LIBS=-L/usr/local/glib/lib -lffi`

再次执行,提示
>No package 'libpcre' found

查看pcre是否安装

`rpm -qa pcre`

没有安装的话先安装pcre再安装pcre-devel

`rpm -ivh pcre-8.32-15.el7.x86_64.rpm`
`rpm -ivh pcre-devel-8.32-15.el7.x86_64.rpm`

还出现刚才的提示,设置PKG_CONFIG_PATH

`export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig`

再次执行,已成功

`make`

`sudo make install`

检查是否安装成功:

`rpm -qa | grep  glib`

