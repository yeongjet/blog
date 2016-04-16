在NASM中,声明一个段:
section a align=b vstart=c
a 表示段名
b 表示对齐的字节数
c 表示段内汇编地址的开始点

align和vstart都是可选的,当然也可以用segment声明

+ section默认情况下是4字节对齐的,align用于修改默认值
![](http://7xqhly.com1.z0.glb.clouddn.com/align4.PNG)
如图,因为data1前面没有内容,所以加不加align都是一样的,由于data2没有align,默认是4字节对齐,所以右边编译后的bin文件中可以看到data1末尾的55所在的地址是0x11,之前的地址总数为18,不是4的倍数,为了对齐4字节,所以在后面加了全为0的两个字节,然后data2才从这里开始。
而,data3是16字节对齐,像上面一样,由于data2的末尾地址是0x18,之前的地址总数为25,所以需要在后面加7个为0的字节,使得data3从地址0x20开始
下图 data2用align指定对齐字节数
![](http://7xqhly.com1.z0.glb.clouddn.com/align1.PNG)

+ vstart指明段内汇编地址起始位置 该段内的汇编的地址都是从vstart开始算的
+ section.段名.start可以获取该段的汇编地址
![](http://7xqhly.com1.z0.glb.clouddn.com/align6.PNG)
图中,因为align=16,所以data2,data3的汇编地址分别是0x10,0x20,由于data2指定了vstart=0,所以该段内的地址从0开始算,所以标号lbc为0x02,而data3没有指定vstart,所以该段内的地址从整个程序的开头算起,由于data3的汇编地址是0x20,而lbd又是它的第一个标号,所以lbd为0x20

最后来看\$和\$\$

+ \$代表当前行行首的标号 \$\$代表当前段的起始汇编地址
\$-\$\$ 代表当前位置在段内的偏移
![](http://7xqhly.com1.z0.glb.clouddn.com/fe.PNG)
来看上图,data1的起始汇编地址为0,所以\$\$=0x00,段内\$当前行的标号是0x01,所以\$=0x01;
data2没有声明vstart,所以该段的汇编地址从0x10开始,故\$\$=0x10,段内\$当前行的标号是0x12,所以\$=0x12;
data3的vstart指明该段段内汇编地址起始位置为0x04,所以段内第一个\$的行标号为0x04,\$\$=0x04,注意0xfff0占用了两个地址,所以第二个\$的行标号为0x07,由于db \$\$,\$占用了两个地址,所以末尾的\$行标号为0x09
data4就不解释了



