# 七、Hadoop组件之HDFS

Hadoop文件系统接口由抽象类org.apache.hadoop.fs.FileSystem定义,该类继承了org.apache.hadoop.conf并实现了java.io.Closeable接口,以下是抽象类FileSystem的几个实现。
##对文件的操作
+ 访问本地文件系统
hadoop dfs -ls file:///(最后一个/表示本地文件系统的根目录)
+ 访问HDFS文件系统
hadoop dfs -ls hdfs:///

##优点
+ 适合存储超大文件,GB至TB级。
+ 低硬件需求,不需要运行在高可靠的昂贵的服务器上
+ 流式数据访问,一次写入,多次读取。数据生成后,会被进行各种分析,每次分析都会涉及该数据集的大部分数据,因此读取整个数据集的时间更重要。

##缺点
+ 实时数据访问弱,HDFS无法做到s或ms级别,HDFS牺牲了数据读取速度来提高吞吐量。有要求的可考虑使用HBase。
+ 大量的小文件,当Hadoop启动时,NameNode会将所有元数据读到内存,构建目录树。一般一个HDFS上的文件,元数据信息大约在150字节,NameNode内存为16GB的话,大概能存480万个文件。
+ 文件只能有一个写入者,写操作在文件末,不支持多个写入者,不支持写入后在文件任意位置修改,如果不将hdfs-site.xml中的dfs.support.append设为true,HDFS也不支持对文件进行追加操作。

##架构
一个典型的HDFS集群中,包含一个NameNode,一个SecondaryNode和至少一个DataNode,而HDFS的客户端无限制。所有的数据放在DataNode进程的节点块里。
### 块
默认块大小64MB,可修改hdfs-site.xml中的dfs.block.size项更改大小。是HDFS存储的最小单元。例如data.txt大小为84M,块大小默认64MB,第一个块64M,第二个20M,小于64M不会占用整个块空间。使用这么大的块是为了减少最小化寻址开销,磁盘速率提升,块大小可以被设为128M或更大。在hdfs-site.xml中,可以在dfs.relication设置每个块的数量,默认为3,2分冗余。
块在DataNode就是一个普通文件,可以到DataNode的目录$(dfs.data.dir)/current,块名称为blk_blkID。
好处
可以保存比存储节点大的文件,突破单一机器的限制
简化存储管理
容错性高

###NameNode与SecondaryNameNode
NameNode:维护整个文件系统的目录树,这些信息存储通过FSImage(FileSystemImage)和EditLog保存。
SecondaryNameNode:定期合并FSImage和EditLog。
FSImage是元数据的检测点,并非每一个写操作都会更新这个文件,而是把改动内容先写入到EditLog,随着时间推移,EditLog会增大,故障后需要很长的时间回滚,因此需要定期合并FSImage和EditLog,但如果由NameNode来合并,则会影响其他业务,因此需要SecondaryNameNode来处理。

NameNode与SecondaryNameNode的交互
1.SecondaryNameNode引导NameNode滚动更新EditLog,并将新内容写入EditLog.new。
2.SecondaryNameNode将NameNode的FSImage和EditLog文件复制到本地的检查点目录。
3.SecondaryNameNode载入FSImage文件,回放EditLog,将其合并到FSImage,将新的FSImage文件压缩后写入磁盘。
4.SecondaryNameNode将新的FSImage文件送回NameNode,NameNode在接受新的FSImage后,直接加载和应用。
5.NameNode将EditLog.new更名为EditLog。
该过程默认每小时发生一次,或者当NameNode的EditLog文件达到默认的64MB。

###DataNode
DataNode会不断向NameNode发送报告,不断更新NameNode,提供本地修改的信息,同时接受NameNode的命令操作本地的数据块。

###HDFS客户端
提供命令行,Java API,Thrift接口,C语言库等客户端。

##容错
+ 心跳机制
NameNode和DataNode之间保持心跳检测,当DataNode发送故障失去心跳时,NameNode就不会将任务分配给它。
这时该DataNode的数据是无效的,当NameNode检测到文件副本数目小于设置值,就会开始复制副本并分发到其他DataNode。
+ 文件块完整性检测
HDFS会记录每个新文件所有文件块的校验和,当以后获取时会检测校验和,如果不一致,会从其他DataNode上获取该块的副本。
+ 集群的负载均衡
当DataNode的空闲空间大于一个临界值时,HDFS会自动从其他DataNode迁移数据过来。
+ SecondaryNameNode对FSImage和EditLog的备份
见NameNode与SecondaryNameNode的交互
+ 文件删除
文件不是马上从NameNode移除命名空间,而是保存在/trash目录以供恢复,超过设定时间才会被移除,可在hdfs-site.xml中的fs.trash.interval设置时间。

副本的存储策略
该数据块的所有副本存储在一个节点中,带宽损失是最小的,但该节点发生故障,数据会丢失
HDFS默认随机在一个节点上放第一个副本,但会避免过满或太忙的节点。第二个副本随机选择另一个机架的节点,第三个与第二个放在相同的机架,切随机选择另一个节点。其他副本(大于3)会放在集群随机选择的节点上,Hadoop会避免在相同的机架放太多副本。
这样提供了良好的数据冗余(放在两个机架),实现负载均衡,节约带宽(中间只需要一个交换机),读取性能(可在两个机架上读取)和数据块的均匀分布。
