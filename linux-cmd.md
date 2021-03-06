# <首先是vim的常用技巧>

* 命令模式 、编辑模式 、尾行模式

### 命令模式 
文档拷贝当前行(yy | p 粘贴到下面)  (nyy 复制n行)
删除当前行(dd )  （ndd  删除n行）
(0-行首/$-行尾) (gg-文档首行/G-文档末行/3gg-文档第三行)
u (撤销)  gg(首航) G(尾行)  n shift+g(到n行)

### 插入模式
i, o, a, s  四个按键不同插入

### 尾行模式
保存(:w)、不保存退出(:q)、强制退出(:q!)
:ste nu    :set nonu  (行号)
替换:  
(范围: 1,3  3  %) (s) (分隔符)(旧的内容)(分隔符)(新的内容)(分隔符)(gi-全局,不区分大小写)
(% s/^/#/g  - 添加注释)
查找:
/内容  （n/N 上下查找）

# <Ubuntu系统常用命令>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~shell
# 《以下针对Ubuntu18.04》

创建目录：           mkdir   ( -p 添加多级目录)
删除空目录：       rmdir
删除目录：           rm -rf
删除文件：           rm  ( -f不提示)
创建空文件：       touch


拷贝：                   cp file newfile   (cp -r )
覆盖拷贝：          cp -r  (强制覆盖)

移动：                   mv  /home/test.txt   /root
重命名：               mv oderfilename new..

浏览文件：          cat   (cat -n ...  | more)
                    			more
大文件查看:         less
查看文件前5行：      head -5 file
查看文件后5行:         tail -5  file
实时监控文件更新:   tail -5f file  （ctrl+c退出）

覆盖写：             ll > 文件 (文件列表列表写入)
追加写：             ll >> 文件
覆盖写：             cat 文件1 > 文件2
追加内容:            echo content >> file

输出内容到控制台:     echo ($PATH)
日历信息:                cal

软连接:                    ln -s [源目录][软链接]

历史指令：           history

日期：                    date "+%Y-%m-%d %H:%M:%S"
设置日期：           date -s "1999-12-27 12:00:00"
                 
查找:               
                            find [目录] [选项]
                            find . (-user group)
                            find / -name *.txt
                            find . -perm 776
                            find / size （- +20M)
                            find . -type (b块设备文件 d目录 f普通文件 c字符设备文件 p管道文件)
                            find . atime (+n -n)   在文件更改时间距现在n天以内  以外
                            find . -user zhangsan (-a和 o或 not非) -type f
		    
过滤文本内容：       
				        cat file | grep (-i (^# 以#开头) #$ n) (检索内容) 
					    cat /etc/passwd | grep -n -i "Bash$"
					 
统计文件内容：       
                        wc /etc/passwd
                        wc (-l行数 w单词数 c字节数) file

压缩文件(不保留压缩: gzip   (gunzip 解压)
压缩：               zip (-r) myzip.zip  /home/
解压：               unzip (-d [指定目录])[解压文件]

压缩：               tar -zcvf [..tar.gz] [进行打包的文件 / 目录]
解压:                  tar -zxvf [解压的文件] [-C 指定目录]

快速定位文件路径:     updatedb
                     locate hello.txt
                     
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~shell
#《组 用户 权限的管理》

/etc/passwd (用户信息)
/etc/shadow (密码权限)
/etc/group  (组信息)
/etc/gshadow (组密码)

/var/spool/mail

查询用户信息:        id 

添加用户:            
                                useradd  用户名     (useradd -d 目录)
                                useradd -g 用户组 用户名  （同时添加到附加组，此时有两个组）
制定密码 ：          passwd  用户名

删除用户:            
                                userdel  用户名 (保留家目录)
                                userdel -r 用户名 (不保留家目录)
					
添加组：             groupadd  (groupdel)
转组：                  gpasswd -a 用户 组

查看文件所有者：      ls -al
改变文件所有者:       chown [user][file]  (chown 张三:李四 file )  (-R 递归修改子目录)
改变文件所在组:       chgrp [group][file] (不常用，用上面那个即可)
修改文件/目录权限:    chmod u+x,g+x,o+x [file]    (操作符“+-=”, x:可执行权限) (r=4 w=2 x=1)
                      (u:所有者, g:所在组, o:其他组, a前面所有)
					  
切换用户:	          su - 用户名
					  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

```shell
# 定时任务编辑:         crontab -e
(*/10 7,8  0-23 1-12 1-7 [指令]) (每隔10分钟，7点和8点，0-23号，1-12月，星期1-7 执行后面的指令)
终止任务调度：        crontab -r
列出任务调度：        crontab -l
重启任务调度:         service crond restart
```

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~shell
# <增加硬盘>(添加硬盘、分区、格式化、挂载、配置)

查看linux磁盘分区：   lsblk (lsblk -f)
磁盘分区:             fdisk [磁盘]
格式化:                mkfs -t ext4 [磁盘]
挂载:                   mount [磁盘] [目录]
永久挂载:             
查看分区UUID号:  ls -l /dev/disk/by-uuid
写入}文件:        sudo gedit /etc/fstab


系统整体磁盘信息查询:  df -lh
指定目录磁盘占用情况:  du -lh (-a含文件 --max-depth 子目录深度 -c增加汇总值)   (du -ach --max-depth=1 /home)
统计目录下文件数：     ls -l /home | grep"^-" | wc -l
统计指定及其子目录下目录数: (ls -lR ......"^d")

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

```shell
# 进程管理
查看进程:              ps -aux  (ps -ef 父进程) (ps -aux | grep sshd)
过滤:                      grep
终止进程：           kill
```

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~shell
# 安装包管理
查看安装了那些软件     dpkg -l
查找是否安装了软件     dpkg -l "*google*"
彻底卸载某个软件       dpkg apt-get--purge remove <programname>
如果想删除apt-get下载的某个软件安装包呢？
Ubuntu 中apt-get下载的安装包放在/var/cache/apt/archives里。所以可以在这个路径下删除
		      apt-get autoclean
将已安装的软件包的安装包也删除掉
		      apt-get clean
		      
# 清理Linux软件包垃圾
	* df -a -h	显示磁盘使用情况
	* sudo apt-get autoclean
	* sudo apt-get autoremove
	* suod apt-get clean --清理所有安装包，不破坏系统

# 卸载ros
	sudo apt-get purge ros-*
	sudo rm -rf /etc/ros
	gedit ~/.bashrc         //删除环境变量
	source ~/.bashrc
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```shell
# 忘记root密码
sudo -i
passwd

查看运行级别：      runlevel
运行级别配置文件;   (暂缺)
```

```shell
# 显示显卡类型：　　　ubuntu-drivers devices
内存数据同步到磁盘：sync
退出远程登录：              logout
```

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~shell
#科学上网
sudo apt-get insatll shadowsocks

sslocal  //查看命令是否存在判断安装成功
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```shell
# 安装 SSH服务
* sudo apt-get install openssh-client
* sudo apt-get install openssh-server

# 远程登录
* ssh name@ip
* ssh -X name@ip (调用图形界面程序)
* ssh -p 23 name@ip (通过23号端口登录)

#查看SSH服务是否启动
* ps -e | grep ssh

# SSH服务管理
* sudo /etc/init.d/ssh (restart/stop/start)
```

```shell
# 查看网络IP/网关       
* ifconfig

# 查看无线网卡
* iwconfig

# 启动wifi 
* rfkill unblock wifi


```























 






