编译环境:
python2.7.11 (参考CentOS7编译安装Python2)
libffi-3.2.1 (http://www.linuxfromscratch.org/blfs/view/svn/general/libffi.html)


编译安装

`mkdir /usr/local/glib`

`./configure --prefix=/usr/local/glib --with-pcre=system`

提示
>configure: error: *** Working zlib library and headers not found ***

需要先安装zlib和zlib-devel,参考《2016-4-9-CentOS7安装zlib-devel和zlib》

再次执行,提示
>No package 'libffi' found

需要设置环境变量
`export LIBFFI_CFLAGS=-I/usr/local/glib/lib/libffi-3.0.11/include`
`export LIBFFI_LIBS=-L/usr/local/glib/lib -lffi`

再次执行,提示
>No package 'libpcre' found

查看pcre是否安装

`rpm -qa pcre`

没有安装的话先安装pcre再安装pcre-devel

`rpm -ivh pcre-8.32-15.el7.x86_64.rpm`
`rpm -ivh pcre-devel-8.32-15.el7.x86_64.rpm`

如果还出现刚才的提示,设置PKG_CONFIG_PATH
`export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig`

再次执行,已成功

`make`

`sudo make install`

检查是否安装成功:

`rpm -qa | grep  glib`

出现
>spice-glib-0.20-8.el7.x86_64
dbus-glib-0.100-7.el7.x86_64
glibmm24-2.36.2-4.el7.x86_64
glibc-headers-2.17-106.el7_2.4.x86_64
libvirt-glib-0.1.7-3.el7.x86_64
glibc-2.17-106.el7_2.4.x86_64
json-glib-0.16.0-3.el7.x86_64
taglib-1.8-7.20130218git.el7.x86_64
glibc-common-2.17-106.el7_2.4.x86_64
glib2-2.36.3-5.el7.x86_64
telepathy-glib-0.20.4-5.el7.x86_64
avahi-glib-0.6.31-13.el7.x86_64
ModemManager-glib-1.1.0-6.git20130913.el7.x86_64
pulseaudio-libs-glib2-3.0-22.el7.x86_64
glibc-devel-2.17-106.el7_2.4.x86_64
NetworkManager-glib-0.9.9.1-13.git20140326.4dba720.el7.x86_64
glib-networking-2.36.2-3.el7.x86_64
poppler-glib-0.22.5-6.el7.x86_64
PackageKit-glib-0.8.9-11.el7.centos.x86_64

说明安装成功
