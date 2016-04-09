1.下载

`wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz`


2.创建编译输出目录

`mkdir /usr/local/python27`


3.编译环境

`sudo ./configure --prefix=/usr/local/python27`


4.编译

`sudo make`

5.安装

`sudo make install`

6.`sudo mv /usr/bin/python /usr/bin/python-2.7.5`

7.`sudo ln -s /usr/local/python27/bin/python /usr/bin/python`

8.`sudo vi /usr/bin/yum`

将第一行中的`#!/usr/bin/python`修改为`#!/usr/bin/python-2.7.5`

9.`sudo ldconfig`

10.执行`python -V`和`python-2.7.5 -V`
分别显示的是`Python 2.7.5`和`Python 2.7.11`

编译过程中会有提示,但我忽略了。
>INFO: Can't locate Tcl/Tk libs and/or headers
Python build finished, but the necessary bits to build these modules were not found:
_bsddb             _curses            _curses_panel   
_sqlite3           _ssl               _tkinter        
bsddb185           bz2                dbm             
dl                 gdbm               imageop         
readline           sunaudiodev        zlib            
To find the necessary bits, look in setup.py in detect_modules() for the module's name.
