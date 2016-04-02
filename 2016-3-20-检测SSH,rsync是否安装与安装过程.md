# 检测SSH,rsync是否安装与安装过程
ssh是一种安全协议,主要用于给远程登录会话数据进行加密,保证数据传输的安全,rsync是一个远程数据同步的工具。

+ 检测SSH,rsync是否安装
检测SSH是否安装(root用户在所有节点执行)
`rpm -qa | grep openssh`
至少出现以下信息,则表示已经安装
![](http://7xqhly.com1.z0.glb.clouddn.com/%E5%9B%BEgweg%E5%83%8F%202.png)
`rpm -qa | grep rsync`
至少出现以下信息,则表示已经安装
![](http://7xqhly.com1.z0.glb.clouddn.com/%E5%9B%BEfwe%E5%83%8F%203.png)

+ SSH,rsync的安装
`yum install ssh`
`yum install rsync`
启动SSH服务
`service sshd restart`






