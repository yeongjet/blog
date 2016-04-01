# 六、Hadoop安装与启动(CDH5.6)
注意事项:[]为替换部分,基于cloudera的hadoop-2.6.0-cdh5.6.0
## Hadoop运行模式
+ 单机:Hadoop的默认模式
+ 伪分布:所有守护进程都运行在一个节点上。
+ 完全分布模式:守护进程运行在多个节点上,真正的集群。
以下是完全分布式安装步骤,所有节点均用root用户执行。

##准备工作
1.在每个节点新建hadoop用户,相同的密码。
`useradd hadoop
passwd hadoop`
2.修改好主机名
    选择一台作为master主机名修改为master,其余作为slave,主机名改为slave1,slave2...
    见《云主机主机名修改》
3.配置每台主机的静态ip地址
    修改ifcfg-eth0 vim /etc/sysconfig/network-scripts/ifcfg-eth0
改为
![](http://7xqhly.com1.z0.glb.clouddn.com/qwe.png)
4.配置主机间的SSH无密码连接
Hadoop并非通过SSH协议传输的,只是在启动和停止的时候需要主节点通过SSH协议将节点的进程启动或停止。
确定安装上SSH,见《检测SSH,rsync是否安装与安装过程》
然后配置无密码连接,确保实现了master登录各台slave和各台slave登录master,见《SSH公钥生成以及各主机间的无密码登录》
5.在master和slaves配置好java环境()
见《centos上java环境配置》

##安装Hadoop
注意:以下操作均在master以hadoop身份进行
1.到`http://archive.cloudera.com/cdh5/cdh/5/`
选择版本下载到/opt目录下
2.从root用户取得/opt文件夹权限
    `chown -R hadoop /opt`
3.解压文件`tar -zxvf hadoop-2.6.0-cdh5.6.0.tar.gz`
4.修改etc/hadoop/hadoop-env.sh,在文件末尾追加
`export JAVA_HOME=/usr/java/jdk1.8.0_73
export HADOOP_HOME=/opt/hadoop-2.6.0-cdh5.6.0`

5.修改配置文件,配置文件位于etc/hadoop目录下
(1)修改core-site.xml
先创建tmp文件夹
`mkdir tmp`
内容为:
`<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://master:9000</value>
        </property>
        <property>
                <name>io.file.buffer.size</name>
                <value>131072</value>
        </property>
<property>
               <name>hadoop.tmp.dir</name>
               <value>file:/opt/hadoop-2.6.0-cdh5.6.0/tmp</value>
       </property>
        <property>
               <name>hadoop.proxyuser.hadoop.hosts</name>
               <value>*</value>
       </property>
       <property>
               <name>hadoop.proxyuser.hadoop.groups</name>
               <value>*</value>
       </property>
</configuration>`
fs.defaultFS是设置提供HDFS服务的主机名和端口号,就是是说HDFS通过master的9000端口提供服务,也指明了master所在节点。
(2)修改hdfs-site.xml
先创建文件夹
`mkdir dfs`
`mkdir dfs/name`
`mkdir dfs/data`
内容为:
`<configuration>
       <property>
                <name>dfs.namenode.secondary.http-address</name>
               <value>master:9001</value>
       </property>
     <property>
             <name>dfs.namenode.name.dir</name>
             <value>file:/opt/hadoop-2.6.0-cdh5.6.0/dfs/name</value>
       </property>
      <property>
              <name>dfs.datanode.data.dir</name>
              <value>file:/opt/hadoop-2.6.0-cdh5.6.0/dfs/data</value>
       </property>
       <property>
               <name>dfs.replication</name>
               <value>3</value>
        </property>
        <property>
                 <name>dfs.webhdfs.enabled</name>
                  <value>true</value>
         </property>
</configuration>`
 dfs.replication配置项设置HDFS的副本数为3,表示有两份冗余,dfs.namenode.name.dir设置NameNode元数据存放的本地文件系统路径,dfs.datanode.data.dir设置DataNode存放数据的本地文件系统路径。
 
(3)修改yarn-site.xml,内容为:
`<configuration>
        <property>
               <name>yarn.nodemanager.aux-services</name>
               <value>mapreduce_shuffle</value>
        </property>
        <property>
<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
               <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
        <property>
               <name>yarn.resourcemanager.address</name>
               <value>master:8032</value>
       </property>
       <property>
               <name>yarn.resourcemanager.scheduler.address</name>
               <value>master:8030</value>
       </property>
       <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
             <value>master:8031</value>
      </property>
      <property>
              <name>yarn.resourcemanager.admin.address</name>
               <value>master:8033</value>
       </property>
       <property>
               <name>yarn.resourcemanager.webapp.address</name>
               <value>master:8088</value>
       </property>
</configuration>`

(4)先把mapred-site.xml.template复制一份,改名为mapred-site.xml,内容为:
`<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
 <property>
                  <name>mapreduce.jobhistory.address</name>
                  <value>master:10020</value>
          </property>
          <property>
                <name>mapreduce.jobhistory.webapp.address</name>
                <value>master:19888</value>
       </property>
</configuration>`
    
8.把slaves添加到在etc/hadoop/slaves中
![](http://7xqhly.com1.z0.glb.clouddn.com/cvge.png)
这样,运行DataNode和TaskTracker的节点就变为slave1,slave2...
    
9.下载编译好的本地库
`http://dl.bintray.com/sequenceiq/sequenceiq-bin/`
选择对应的版本下载后解压
`tar -xvf hadoop-native-64-2.6.0.tar`
把解压后的文件放到lib/native目录下。
![](http://7xqhly.com1.z0.glb.clouddn.com/bgb.png)

10.将安装文件夹分发到slave的相同路径下(Hadoop用户)
    `nohup scp -r /opt/hadoop-2.6.0-cdh5.6.0 hadoop@slave1:/opt &`
    
11.配置环境变量,在/etc/profile的尾部添加
    `export HADOOP_HOME=/opt/hadoop-0.20.2-cdh3u6 
    export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin`
    ![](http://7xqhly.com1.z0.glb.clouddn.com/hth7.png)
    立即生效
    `source /etc/profile`
    
12.格式化HDFS
    `hdfs namenode -format`
    ![](http://7xqhly.com1.z0.glb.clouddn.com/gw.png)
    此时HDFS的根目录没有任何内容
    `hadoop fs -ls /`
    ![](http://7xqhly.com1.z0.glb.clouddn.com/hyk.png)
    
13.启动Hadoop验证安装是否成功
    有三种方式启动
    a.
     `hadoop-daemon.sh start namenode
     hadoop-daemon.sh start datanode
     hadoop-daemon.sh start secondarynamenode`
    b.
    ` start-dfs.sh
     start-yarn.sh `
    c.
    ` start-all.sh `
     出现以下信息说明启动成功:
    ![](http://7xqhly.com1.z0.glb.clouddn.com/hyhy.png)

14.执行jps命令查看进程是否启动
    master应出现:
![](http://7xqhly.com1.z0.glb.clouddn.com/gerg5.png)
    slave应出现:
   ![](http://7xqhly.com1.z0.glb.clouddn.com/ukkager.png)

15.执行MapReduce作业验证,进行单词计数。
    新建文件
    `vim words`
    内容为:
    "data mining on data warehouse"
    在HDFS创建input目录和output目录:
    `hdfs dfs -mkdir /input`
    `hdfs dfs -mkdir /output`
    将该文件上传至HDFS目录:
    `hdfs dfs -put words /input`
    执行MapReduce任务:
    `hadoop jar /opt/hadoop-2.6.0-cdh5.6.0/share/hadoop/mapreduce2/hadoop-mapreduce-examples-2.6.0-cdh5.6.0.jar wordcount /input /output`
    查看结果
    `hdfs dfs -cat /output/*`
    出现
    ![](http://7xqhly.com1.z0.glb.clouddn.com/966.png)
    至此安装成功
    相关文章:
    《Hadoop HDFS命令》
    《Hadoop可能遇到的问题》
    部分内容参考自:
 《hadoop（2.x）以hadoop2.2为例完全分布式最新高可靠安装文档》
    http://www.aboutyun.com/thread-7684-1-1.html






