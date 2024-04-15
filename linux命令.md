#putty的session配置信息在注册表的位置：HKEY_CURRENT_USER\Software\SimonTatham\PuTTY

# Linux命令

## linux常用命令

#### 目录处理命令

ls -l -a -d -h -i /etc
mkdir -p 递归创建
pwd 输出当前工作目录
cd
rmdir
cp -r -p
mv
rm -r -f

#### 文件处理命令

touch 
cat -n
tac
more
less
head -n 7  (head默认10行)
tail -n 3 -f

#### 链接命令

ln
ln -s
		###补充：文件类型###
			-文件
			d目录directory
			l软链接link

#### 权限管理命令

chmod {ugoa}{+-=}{rwx}
	  mode=  	  421
	  -R	递归修改
chown
chgrp
umask -S

#### 文件搜索命令

find -name -iname * ?
	 -size +204800	 	//1数据块=0.5K
	 -user
	 -cmin -amin -mmin -5
	 -a -o (all or) 	-exec/-ok 命令 {}  \;
	 -type f d l	 -inum
locate -i	updatedb
which
whereis
grep -i -v ^#
man ls			1命令的帮助
man	service		5配置文件的帮助
whatis 命令
apropos 配置文件
info
help

#### 压缩解压

.gz			gzip  gunzip(gzip -d)  只能压缩文件
.tar		tar -cf -xf		-v
.tar.gz		tar -zcf -zxf 	-v
.zip		zip -r   uznip
.bz2		bzip2 -k   bunzip2
.tar.bz2	tar -cjf -xjf

#### 网络命令

write
wall
ping -c 4
ifconfig
mail   
last
lastlog -u 502
traceroute 
netstat -t -u -l -r -n  例:-tlun -an -rn
setup //无
mount
umount

#### 关机重启命令

shutdown -c -h -r
halt
poweroff
init 0
reboot
init 6
runlevel
logout

## 文件编辑器vim
命令模式 -> 插入模式 插入命令a o i
命令模式:-> 编辑模式
定位命令
 :set nu		:set nonu
 gg G nG :n $ 0
删除命令
 x nx dd nd dG D  :n1,n2d
复制和剪切命令
 yy nyy dd ndd p P
替换或取消命令
 r R u
搜索和搜索替换命令
 :/String  不区分大小写set ic    n(next)
 :%s/old/new/g
 :n1,n2s/old/new/g	/c询问确认
保存和退出命令
 :w  :w new_filename  :wq  ZZ  :q!  :wq!
导入命令
 :r 文件名    	:!命令
快捷键
 map 快捷键 触发命令  
连续行注释
 :n1,n2s/^/#/g
 :n1,n2s/^#//g
 \转义符
 :n1,n2s/^/\/\//g
替换
 ab mymail alien@not.com
定义的快捷键重启后会消失。保存在.vimrc 中(编辑模式)。root用户/root/.vimrc。普通用户/home/username/.vimrc

## 软件包管理-源码包/rpm包

### rpm包管理

#### rpm包命令管理

 rpm -ivh 包全名(安装)
	 -Uvh 包全名(升级)
	 -e 包名	(卸载)
 rpm -q 包名	(查询)
	 -qa
	 -qi 包名
	 -ql 包名
	 -qf 系统文件名
	 -qR 包名     		-p 未安装包信息
 rpm -V 包名	(校验)
 rpm2cpio 包全名 | cpio -idv .文件绝对路径	(文件提取)
 \一行写不下，换行

#### yum在线管理

 yum list		(查询)
 yum search 关键字
 yum -y install (安装)
 yum -y update	(升级)
 yum -y remove	(卸载)
 yum grouplist  (组管理)
 yum groupinstall
 yum groupremove

### 源码包管理

 需要gcc编译器
 下载并解压源码包
 ./configure --prefix=/usr/local/apache2
 make
 make clean
 make install
 /usr/local/apache2/bin/apachectl start
 脚本安装包
 ./setup.sh

## 用户和用户组管理

who
w
用户信息文件/etc/passwd	影子文件	/etc/shadow
组信息文件 	/etc/group	组密码文件	/etc/gshadow
用户的家目录/home/用户名/和/root/
用户的邮箱	/var/spool/mail/用户名/ 
用户模板目录/etc/skel/
用户默认值文件/etc/default/useradd	/etc/login.defs
useradd -u -d -c -g -G -s
passwd -S -l -u --stdin
usermod -u -c -G -L -U
chage -l -d
userdel -r
id
su - -c
groupadd -g
groupmod -g -n
groupdel 
gpasswd -a -d

## 权限管理

ACL权限 dumpe2fs -h (CentOS7用xfs_info) 
临时开启mount -o remount,acl/
永久开启vi /etc/fstab
getfacl
setfacl -m u:用户名:权限 g:组名:权限  m:权限(最大有效权限)
		-x -b -d -k -R    -m d:u:用户名:权限(d是默认)
文件特殊权限
SetUID权限、SetGID权限、Sticky BIT
chmod 4755  chmod 2755	chmod 1755		7755(可以，但没有意义)
	  u+s		  g+s		  o+t
chattr +-= i a
lsattr -a -d
visudo
sudo -l
sudo 绝对路径

## 文件系统管理

df -h 
du -a -h -s
fsck
dumpe2fs
（挂载：设备文件名和盘符/挂载点连起来）
mount [-l]		(查询
mount -a		(自动挂载
mount [-t][-L][-o] 设备文件名 挂载点
umount
u盘	fdisk -l
	mount -t vfat 设备文件名 挂载点 （fat16识别为fat，fat32识别为vfat）

## Shell

### Shell基础

echo -e
#### 脚本执行方法

1赋予权限后执行		chmod 755 hello.sh	./hello.sh
2通过bash调用脚本	bash hello.sh
dos2unix 格式转换

#### 基本功能

历史命令与补全
history -c -w
!n
!!
!字串
别名与快捷键
alias 别名='原命令'
alias
unalias 别名
vi /root/.bashrc
	#####命令执行顺序#####
	1绝对路径/相对路径
	2别名
	3Bash的内部命令
	4$PATH环境变量
输入和输出重定向
命令 > 文件	（覆盖）
命令 >> 文件（追加）
错误命令 2>文件		错误命令 2>>文件
命令 > 文件 2>&1	命令 >> 文件 2>&1
命令 &>文件			命令 &>>文件
命令 >> 文件1 2>>文件2
wc -c -w -l < 文件
多命令顺序执行和管道符
;	
&&
||
dd if=输入文件 of=输出文件 bs=字节数 count=个数
命令1 | 命令2
grep

#### Bash变量

用户自定义变量（本地变量）
name="alien not"
aa=123
aa="$aa"456
aa=${aa}789
echo $name
set
unset name
环境变量
export 变量名=变量值
env
unset 变量名
位置参数变量
$n
$*
$@
$#
预定义变量
$?
$$
$!
接收键盘输入
read -p "提示信息" -t -n -s

#### 运算符

数值运算与运算符

declare -/+ -i -x -p
expr/let
$((运算式))		$[运算式]
变量测试与内容替换
x=${y-新值}

#### 环境变量配置文件

source 配置文件
. 配置文件
/etc/profile
/etc/profile.d/*.sh
~/.bash_profile
~/.bashrc
/etc/bashrc
其他~/.bash_logout
	~/.bash_history
	/etc/issue
	/etc/issue.net
	/etc/motd

### 正则表达式

*	.	^	$	[]	[^]	\	\{\n}	\{\n,}	\{\m,n}	
字段截取命令
cut -f -d
printf '输出类型输出格式' 输出内容
		%ns %ni %m.nf \a \b \f \n \r \t \v  $(cat student.txt)
awk
sed
字符处理命令
sort
wc
条件判断
test -e 文件
[ -e 文件 ]
-e -d -f
-r -w -x等
单分支if语句
if [ 条件判断式 ];then	或		if[ 条件判断式 ]
	程序							then
fi										程序
								fi
双分支if语句
if []
	then
		执行程序
	else
		执行程序
fi

case $ in
	"值1")
		如果变量的值等于值1，则执行程序1
		;;
	"值2")
		如果变量的值等于值2，则执行程序2
		;;
	"*")
		如果变量的值都不是以上的值，则执行此程序
		;;
esac

for 变量 in 值1 值2 值3...
	do
		程序
	done

for (( 初始值;循环控制条件;变量变化 ))
	do
		程序
	done

while [ 条件判断式 ]
	do
		程序 
	done

until

## 服务管理

分类：rpm包(分为独立的服务和基于xinetd服务)和源码包安装的服务
chkconfig --list 	#查看自启动和rpm包安装的服务 CentOS7用systemctl list-unit-files
/usr/local			#源码包一般在这里
可使用ps aux 或 netstat -tlun查看启动的服务

独立服务的启动
/etc/init.d/ 独立服务名 start|stop|status|restart
service 独立服务名 start|stop|restart||status	(红帽linux)
独立服务的自启动
chkconfig [--level 运行级别][独立服务名][on|off]
修改/etc/rc.d/local文件
ntsysv

## 系统管理

### 进程管理

ps aux
ps -le
top -d		交互模式:?或h P M N q
pstree -p -u
kill -l
常用kill -1/-9/-15 PID		重启/强制结束/正常结束
killall [选项][信号] 进程名
pkill [选项][信号] 进程名

### 工作管理

将进程放入后台	tar -zcf ect.tar.gz /etc &
				ctrl+Z
查看后台进程	jobs -l
恢复到前台		fg %工作号
恢复到后台		bg %工作号
系统资源查看
vmstat [刷新延时 刷新次数]
dmesg
free
cat /proc/cpuinfo
uptime
uname -a -r -s
file /bin/ls
lsb_release -a
lsof
系统定时任务
service crond restart
chkconfig crond on
crontab -e -l -r

## 日志管理

rsyslog
日志轮替
logrotate

## 启动管理

系统运行级别
runlevel
init 运行级别
vi /etc/inittab
启动引导程序
grub
字符界面分辨率调整

## 备份与恢复

​	需要备份的数据：
​	/root/
​	/home/
​	/var/spool/mail/
​	/etc
​	apache 配置 网页 日志
​	mysql 源码包安装：/usr/local/mysql/data/ rpm包安装:/var/lib/mysql
dump
restore
