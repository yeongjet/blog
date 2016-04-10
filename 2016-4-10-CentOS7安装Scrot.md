
安装psychotic源

http://packages.psychotic.ninja/7/base/x86_64/RPMS/

找到 psychotic-release*rpm

`sudo rpm -Uvh http://packages.psychotic.ninja/7/base/x86_64/RPMS/psychotic-release-1.0.0-1.el7.psychotic.noarch.rpm`

`sudo yum --enablerepo=psychotic install giblib-devel`

会顺带安装imlib2


来到scrot目录下

`./configure`

`make`

`make install`

安装完毕

Scrot用法
(整理自网上)

+ 截下整个桌面

`scrot`

+ 指定路径文件名

`scrot ~/pictures/desktop.png`

如果不指定,会根据当前的日期时间、宽高生成文件名。

+ 截取指定矩形区域
 
`scrot -s`

需要用鼠标单击任意窗口或画出一个矩形进行截取

+ 延迟截屏

`scrot -cd 10`

d选项延时抓取,10代表延时10秒,c选项显示倒计时。适合抓取菜单或命令提示

+ 调整截屏质量

`scrot -q 50`

范围在1到100,越大质量越高,默认75


+ 调整截屏尺寸

范围在1到100,数字越大尺寸越大,减小尺寸到原图的10％

`scrot -t 10`

+ 将截取的内容传递给其它命令

`scrot -e 'mv $f ~/images'`

将抓取的图像移动到 ~/images/ 目录

e选项开启操作图像功能，$f代表原图的路径

+ 抓取窗口

`scrot -bs window.png`

b选项在抓取窗口时一同将外边框抓取下来,s选项则让用户选择所要抓取的是何窗口。
　
+ 通过脚本截图

`vim screenshot`

`scrot -cd 3 -t 60 -s ~/pictures`

`sudo ln -s ~/screenshot /usr/bin/scs`
