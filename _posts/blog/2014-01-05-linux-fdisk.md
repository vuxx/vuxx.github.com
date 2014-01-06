---
layout: post
title: centos磁盘分区介绍与相关命令使用
description: 分区步骤和命令
category: blog
---

##1.fdisk  磁盘分区命令需在root 下进行

只要我们的硬盘查到电脑上，就在/dev显示

* fdisk -l 查看分区信息 会有一个新硬盘的名字没有分区结构 如/dev/sdb

###fdisk /dev/sbd 

* 按m获得帮助 按n增加一个分区
* 然后出现选择主分区p，扩展分区e，一个硬盘只能分4个主分区，如果你的分区超过4个，那么就需要借助扩展分区了
* 按p 建立一个主分区，`Partition number`给分区分配1个号，只能是1-4，确认没有被占用
* `Fist cylinder`然后磁盘的起始位置，因为新盘默认就行
* `Last cylinder` 接着给这这个分区分多大例如分成4个g`+4G`
* 完成之后按p就可以查看刚才的分区情况,如果不分扩展分区重复就行了

##### 当分区超过4个分区的时候，必须通过扩展分区来体现

* 按 n--->e 增加一个扩展分区
* `Partition number`分区号1-4
* `Fist cylinder`然后磁盘的起始位置
* `Last cylinder`分区分多大，默认全给他
* 事实上扩展分区只是个规范，真正使用的它下的逻辑分区
* n---->l 分逻辑分区，...
*如果想改变分区的类型 按 t 输入分区号 l 查看分区类型，linux 下一般是83
* 分好之后按w写入磁盘

##2.分区完成正好要格式化mkfs -t ext4

* mkfs -t ext4 /dev/sdb1,把新建的分析都格式化（ext4是文件类型)

##3.把格式好的分区挂载到目录下

* `mount`  查看挂载情况 
* `mkdir /www`
* `mount /dev/sdb1 /web` 把dev/sdb1 挂载在/web目录
* `ls -l /web` 挂载成功后会出现lost-found文件

### 给格式好的分区分配卷标

* `e2label /dev/sdb1 web` 给这个分区起个名字叫web 
* `e2label /dev/sdb1/` 查看卷标 
* `mount -L 'web' /web` 使用卷标挂载到/web(!不能重名)   

###取消挂载
* `umount /dev/sdb1 ` 弹出挂载

### `df -h` 查看磁盘使用情况

* 看看是否都挂载成功
* 都挂载成功了，但是重启挂载就没了

###自动挂载

* `vim /etc/fstab` 在最后添加上
* `dev/sdb1  /web  ext4   defaults   0 2`
* `LABEL=web /web  ext4   defaults   0 2` //卷标挂载
*  分区      挂载点 文件类型 defaults  自动备份  是否进行检查
*  `mount -a ` 测试挂载 切结要检查一遍保证没错在重启
*  重启看看 

###挂载失败进不去系统解决办法

* 进入单用户模式
* 重启随便按键盘上的键
* 出现系统框按e-->选择kernel->按e-> 输入single ->在来按b
* 进入系统，单用户默认是readonly 我们想修改要重新挂载
* `mount -n -o remount,rw /`然后再修改`vim etc/fstab`里的错误
* 第二种是在报错的地方输入root密码，过程类似

####root密码忘记可以进单用户类似以上过程

* `passwd root` 修改密码
