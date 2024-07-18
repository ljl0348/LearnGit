- 1、用户和组
	- 1.1、含义
	- 1.2、用户和用户组管理命令
- 2、Linux文件系统
	- 2.1、文件和目录操作
	- 2.2、文件内容操作
	- 2.3、其他常用命令
### 1、用户和组

### 1.1、含义  
（1） **用户**：每个在Linux系统上登录的实体都代表一个用户。用户可以是一个人、一台机器或者是软件进程。每个用户都有唯一的用户名，并且关联着一组属性，如密码、主目录（Home Directory）等。用户还可以拥有特定的权限级别。  
- root用户：root在linux里面拥有所有的系统权限，可以畅行无阻地修改所有系统文件和其他用户的文件，挂载文件系统等等的一系列操作，因为linux内核执行进程的过程中，首先检查进程所属，如果属于root则一切放行;  
```bash
root@L0348:~#
```

	- "root"代表root用户，
	- L0348代表当前主机名，
	- ~表示当前用户，
	- 在#下输入命令。

- 普通用户：普通用户则有很大的限制，例如不能修改系统关键配置文件，想查看其他用户的文件则需要相应的权限，不能安装软件，甚至关机关机指令都需要以root身份执行。

（2） **组**：一组具有相似权限需求的用户组成一个组。例如，管理员组（root）、普通用户组、开发者组等。Linux将用户分配到一个或多个组中，共享相同的文件和目录权限。组的主要作用是简化权限管理和避免重复设置权限。
    
（3） **用户和组的关系**：用户可以属于一个或多个组，通过这种方式，用户可以获得组内的权限。当一个文件设置了所有者和组所有者，通常只有文件所有者和该组成员有读写权限，其他人则受限。

### 1.2、用户和用户组管理命令

- 用户账号的添加、删除与修改。
- 用户口令的管理。
- 用户组的管理

(1) 用户组
- 创建新用户组
`groupadd  选项 用户组`
```shell
groupadd group1
```
- 删除用户组
```bash
groupdel group1
```
- 修改用户组属性
`groupmod 选项  用户组`
```shell
groupmod -g 102 group2
```
 _将组group2的组标识号修改为102_

- 切换用户组
```bash
newgrp group1
```
_如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换用户可以在登录后，使用命令newgrp切换到其他用户组，这个命令的参数就是目的用户组_

- 查看组及组内用户
```shell
cat /etc/group
```

(2) 用户管理
- 进入超级用户
通过`su root`命令并输入密码进入超级用户内
```shell
su root
```
***如果已经配置普通用户免密码（下面会有），则可直接使用`sudo su` 切换至root用户***
```shell
sudo su
```
- 创建新用户并加入sudo组
```shell
adduser ljl  # 创建新用户ljl
usermod -aG sudo ljl   # 将用户ljl添加到sudo组
```

- 配置sudo免密码权限
```bash
visudo # 打开sudoers文件
```

- 打开的文件中寻找下图所示代码
```
root    ALL=(ALL:ALL) ALL
```
- 该行下面添加命令：
```
ljl ALL=(ALL) NOPASSWD:ALL
```
- 然后同时按`ctrl+X`，输入 `Y` 保存更改，最后按 `Enter` 退出。
- 切换用户
```bash
su ljl
```

- 测试sudo免密码配置
```bash
sudo apt update
```
不需要输入密码则配置成功
- 登出
```bash
exit
```
或者使用快捷键`Ctrl + d` 退回之前用户。

- 设置默认登录用户
	- Windows中打开powershell，输入指令`wsl --shutdown`关闭子系统
	- 设置默认用户`<wsl> config --default-user <username>`
		- `<wsl>`更改为对应的wsl版本，`<username>` 更改为欲设置的默认用户名。
```bash
wsl --shutdown

ubuntu2204.exe config --default-user ljl
```

### 2、Linux文件系统

### 2.1 文件和目录操作

- `ls` ：列出目录内容
	- `ls /lj`  简要列出目录内容  /lj表示目录，根目录为`/`，子目录则输入详细路径，如`/home/ljl`
示例：
```shell
ljl@L0348:~$ ls /
```
```
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
```

```bash
ljl@L0348:~$ ls /home
lijialong  ljl
```

- `ls /path -l` 详细列表显示
示例：
```shell
ljl@L0348:~$ ls /home -l
```
```
total 8
drwxr-x--- 2 lijialong lijialong 4096 Jul  2  2024 lijialong
drwxr-x--- 4 ljl       sudo      4096 Jul 15  2024 ljl
```
- `ls /lj -a`显示所有文件（包括隐藏文件）
示例：
```bash
ljl@L0348:~$ ls /home/ljl -a
```
```
.   .bash_history  .bashrc   .sudo_as_admin_successful  file1.txt  src
..  .bash_logout   .profile  archive.tar                file2.txt  test
```
- `pwd` ：显示当前目录路径
示例：
```bash
ljl@L0348:~$ pwd
/home/ljl
```
- `cd`：切换工作目录
	- `cd ~` 切换到当前用户主目录
示例：
```shell
ljl@L0348:/root$ cd ~
ljl@L0348:~$ pwd
/home/ljl
```
	- `cd /`切换到根目录
示例：
```bash
ljl@L0348:~$ cd /
ljl@L0348:/$ pwd
/
```
- `cd /lj` ：切换到到对应路径
示例：
```bash
ljl@L0348:~$ cd src
ljl@L0348:~/src$
```
- `cd ..` ：切换到上级目录
示例：
```bash
ljl@L0348:~/src$ cd ..
ljl@L0348:~$ pwd
/home/ljl
```
- `cd -` ：切换到上一次目录
示例：
```bash
ljl@L0348:~$ cd -
/home/ljl/src
ljl@L0348:~/src$
```

- `mkdir new_dir` ：创建新目录new_dir
示例：
```bash
ljl@L0348:~$ mkdir sss
ljl@L0348:~$ ls -l
```
```
total 24
-rw-r--r-- 1 root root 10240 Jul 15  2024 archive.tar
-rw-r--r-- 1 root root     0 Jul 15  2024 file1.txt
-rw-r--r-- 1 root root     0 Jul 15  2024 file2.txt
drwxr-xr-x 2 root root  4096 Jul 15  2024 src
drwxrwxr-x 2 ljl  sudo  4096 May 23 02:26 sss
drwxr-xr-x 2 root root  4096 Jul 15  2024 test
```
- `rmdir dir` ：删除空目录
- `rm -r dir` ：删除目录dir
示例：
```shell
ljl@L0348:~$ rmdir sss
ljl@L0348:~$ ls -l
```
```
total 20
-rw-r--r-- 1 root root 10240 Jul 15  2024 archive.tar
-rw-r--r-- 1 root root     0 Jul 15  2024 file1.txt
-rw-r--r-- 1 root root     0 Jul 15  2024 file2.txt
drwxr-xr-x 2 root root  4096 Jul 15  2024 src
drwxr-xr-x 2 root root  4096 Jul 15  2024 test1
```
```shell
ljl@L0348:~$ rm -r test
rm: remove write-protected directory 'test'? Y
```
- `cat > file.txt` --->  `ctrl + D` 创建新文件
- `touch file.txt` 创建新文件
示例：
```bash
ljl@L0348:~$ cat > file3.txt
ljl@L0348:~$ touch file4.txt
ljl@L0348:~$ ls -l
```
```
total 20
-rw-r--r-- 1 root root 10240 Jul 15  2024 archive.tar
-rw-r--r-- 1 root root     0 Jul 15  2024 file1.txt
-rw-r--r-- 1 root root     0 Jul 15  2024 file2.txt
-rw-rw-r-- 1 ljl  sudo     0 May 23 02:27 file3.txt
-rw-rw-r-- 1 ljl  sudo     0 May 23 02:27 file4.txt
drwxr-xr-x 2 root root  4096 Jul 15  2024 src
drwxr-xr-x 2 root root  4096 Jul 15  2024 test
```
- `rm name.hz` ：删除文件或目录
示例：
```bash
ljl@L0348:~$ rm file3.txt file4.txt
ljl@L0348:~$ ls -l
```
```
total 20
-rw-r--r-- 1 root root 10240 Jul 15  2024 archive.tar
-rw-r--r-- 1 root root     0 Jul 15  2024 file1.txt
-rw-r--r-- 1 root root     0 Jul 15  2024 file2.txt
drwxr-xr-x 2 root root  4096 Jul 15  2024 src
drwxr-xr-x 2 root root  4096 Jul 15  2024 test
```
- `cp file.txt /path` ：将file文件复制到/path中
- `cp -r dir/ newdir`：将当前目录dir的所有文件复制至newdir中
- `mv file.txt /path`移动文件或目录 
- `mv old_name.txt new_name.txt`重命名文件

### 2.2 文件内容操作

- `cat file.txt` :显示文件file的内容
- `tail -f file.txt` : 将 file 文件里的最尾部的内容显示在屏幕上，并且不断刷新，只要 file 更新就可以看到最新的文件内容。
- `grep hello file.txt` ：在文件 file.txt 中查找字符串 "hello"，并打印匹配的行。
- `grep -r update /etc/acpi` :查找指定目录/etc/acpi 及其子目录（如果存在子目录的话）下所有文件中包含字符串"update"的文件，并打印出该字符串所在行的内容。
### 2.3 其他常用命令

##### 2.3.1 压缩与解压缩
- `tar -cvf archive.tar file1 file2 directory`  ：将文件 file1、file2 和 directory 打包到一个名为 archive.tar 的归档文件中。
	- `-c`: 创建新的归档文件
	- `-v`: 显示详细输出，列出被添加到归档中的文件
	- `-f`: 指定归档文件的名称
示例：
```shell
ljl@L0348:~$ tar -cvf tar_test_tar1.tar tar_test1.txt tar_test2.txt test1
```
```
tar_test1.txt
tar_test2.txt
test1/
```
- `tar -xvf archive.tar` ：解压名为 archive.tar 的归档文件，还原其中包含的文件和目录。
	- `-x`: 解压归档文件
	- `-v`: 显示详细输出，列出被解压的文件
	- `-f`: 指定要解压的归档文件的名称
示例：
```bash
ljl@L0348:~$ tar -xvf tar_test_tar1.tar
```
```
tar_test1.txt
tar_test2.txt
test1/
```
- `tar -tvf archive.tar` ：列出名为 archive.tar 的归档文件中包含的所有文件和目录。
	- `-t`: 列出归档文件中的内容
	- `-v`: 显示详细输出，列出归档文件中的所有文件和目录
	- `-f`: 指定要列出内容的归档文件的名称
示例：
```bash
ljl@L0348:~$ tar -tvf tar_test_tar1.tar
```
```
-rw-rw-r-- ljl/sudo      10240 2024-07-15 01:07 tar_test1.txt
-rw-rw-r-- ljl/sudo          0 2024-07-15 01:06 tar_test2.txt
drwxr-xr-x root/root         0 2024-07-15 15:47 test1/
```

##### 2.3.2 文件及目录复制
- **本地文件**及目录 ：
	- `cp file.txt /path` ：将file文件复制到/path中；
	- `cp -r dir/ newdir`：将当前目录dir的所有文件复制至newdir中。
- **远程文件** ：
	- `scp <本地存放路径>/<文件名> <username>@<远端ip地址>:<远端文件存放路径>` 将本地的local_file文件复制给远端；
	- `scp <username>@<远端ip地址>:<远端文件存放路径>/<文件名> <本地存放路径>` 从远端复制文件至本地。
示例：
```bash
scp libat501-jammercode.so root@192.168.3.152:/opt/lib
```
将本地编译后的.so文件复制到用户名为root、IP地址为192.168.3.152、目录为/opt/lib中。
##### 2.3.3 磁盘使用情况
- `df -h ` ：显示文件系统的磁盘空间使用情况。
- `du -h dir` ：显示目录或文件占用的磁盘空间
示例：
```bash
ljl@L0348:~$ df -h /home
```
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb        251G  9.7G  229G   5% /
```
```shell
ljl@L0348:~$ du -h /home
```
```
du: cannot read directory '/home/lijialong': Permission denied
4.0K    /home/lijialong
4.0K    /home/ljl/src
4.0K    /home/ljl/test1
40K     /home/ljl
48K     /home
```
##### 2.3.4 进程信息显示
- `top -n 2` ：显示进程信息，更新两次后停止
- `top -d 3` ：进程信息更新周期为3s
- `top -p 139` ：显示指定进程的信息

##### 2.3.5 显示时间信息
- `date` ：显示当前时间
```bash
ljl@L0348:~$ date
```
```
Wed May 23 02:45:23 CST 2012
```
- `date +"%Y-%m-%d"` ：格式化输出时间  年-月-日
```bash
ljl@L0348:~$ date +"%Y-%m-%d"
```
```
2012-05-23
```
- `date -s`  ：设置当前时间（root用户）
示例：
```shell
ljl@L0348:~$ sudo date -s "20240715 01:01:01"
```
```
Mon Jul 15 01:01:01 CST 2024
```
##### 2.3.6 进程状态
- `killall name`  ：直接对进程名字进行操作，杀死指定进程。
- `Ctrl + c` : 结束当前进程。

##### 2.3.7 查看历史命令
- `history` ： 在终端中查看历史输入的命令（每条命令有对应编号）
示例 ：
```bash
ljl@L0348:~$ history
```
```bash
    1  exit
    2  sudo apt update
    3  sudo su ljl
    4  exit
    5  sudo apt update
    6  exit
    7  sudo apt update
    8  exit
    9  cd ~
   10  pwd
   11  exit
   12  pwd
   13  cd ~
   14  pwd
   15  exit
   16  cd ~
   17  exit
   18  cd ~
   19  exit # zhushi
   20  cd ~
   21  sudo apt update
   22  exit
   23  sudo apt update
   24  cd ~
   25  pwd
   ...............
```

- `history | grep "ls"` ：在终端中查看包含特定内容的命令
示例：
```bash
ljl@L0348:~$ history | grep "date"
```
```bash
    2  sudo apt update
    5  sudo apt update
    7  sudo apt update
   21  sudo apt update
   23  sudo apt update
   74  date
   75  date +"%Y-%m-%d"
   76  sudo date  "20240715 18:06:55"
   77  sudo date -s "20240715 01:01:01"
   79  history | grep "date"
```
- `!n` ：重复执行编号为n的命令
示例：
```bash
ljl@L0348:~$ !75
```
```
date +"%Y-%m-%d"
2024-07-15
```
- `!!` ：重复执行上一条命令
示例：
```bash
ljl@L0348:~$ !!
```
```
date +"%Y-%m-%d"
2024-07-15
```

##### 2.3.8 显示内存状态
- `free` ：显示内存使用信息
- `free -s 10` ：每10s显示一次
- `free -b`  ：以byte为单位显示内存使用信息
		- -k：以kb为单位
		- -m：以mb为单位
		- -h：自动计算合适单位
```bash
ljl@L0348:~$ free
```
```
               total        used        free      shared  buff/cache   available
Mem:         8012720     1527932     3947740          76     2537048     6220388
Swap:        2097152           0     2097152
ljl@L0348:~$ free -b
               total        used        free      shared  buff/cache   available
Mem:      8205025280  1565368320  4041711616       77824  2597945344  6368911360
Swap:     2147483648           0  2147483648
ljl@L0348:~$ free -k
               total        used        free      shared  buff/cache   available
Mem:         8012720     1528388     3947268          76     2537064     6219932
Swap:        2097152           0     2097152
ljl@L0348:~$ free -m
               total        used        free      shared  buff/cache   available
Mem:            7824        1493        3854           0        2477        6073
Swap:           2048           0        2048
ljl@L0348:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.6Gi       1.5Gi       3.8Gi       0.0Ki       2.4Gi       5.9Gi
Swap:          2.0Gi          0B       2.0Gi
```
##### 2.3.9 远程登陆
- `ssh username@userip` ：远程连接用户名为username、ip地址为userip的服务器。
示例：
```bash
ssh root@192.111.1.1
```
##### 2.3.10 快捷键
- `Ctrl + c` ：强制中断程序
- `Ctrl + z` ：暂停任务
- `Ctrl + l` ：清屏
- `Ctrl + s` ：暂停屏幕输出
- `Ctrl + q` ：恢复屏幕输出
- `Ctrl + a` ：切换到命令行开始
- `Ctrl + e` ：切换到命令行结尾
- `Ctrl + u` ：剪切光标前到行首的所有字符
- `Ctrl + w` ：剪切光标前的一个词
- `Ctrl + k` ：剪切光标处到行尾的所有字符
- `Ctrl + y` ：在光标处粘贴内容
- `Ctrl + r` ：查找历史命令
- `Ctrl + x + u` ：撤销处理
- `Ctrl + d` ：退出当前shell命令行，如果是切换过来的用户，则执行这个命令回退到原用户
- `Tap` ：自动补全命令

#### 2.3.11 Ubuntu包管理工具apt
- apt 主要用于远程安装软件包（从远程仓库搜索包的名字，下载并安装）
- apt 安装软件包时会自动安装依赖包，配置好环境
- 常用apt命令集：`<package_name>` 、`<package_n>` 、`<keyword>` 等替换为对应的包或命令名字
```shell
sudo apt update # 列出所有可更新的软件清单命令

sudo apt upgrade # 升级软件包

apt list --upgradable # 列出可更新的软件包及版本信息

sudo apt full-upgrade # 升级软件包，升级前先删除需要更新软件包

sudo apt install <package_name> #安装指定的软件命令

sudo apt install <package_1> <package_2> <package_3> # 安装多个软件包

sudo apt update <package_name> # 更新指定的软件命令
    
sudo apt show <package_name> # 显示软件包具体信息,例如：版本号，安装大小，依赖关系等等
    
sudo apt remove <package_name> # 删除软件包命令
    
sudo apt autoremove # 清理不再使用的依赖和库文件
    
sudo apt purge <package_name> # 移除软件包及配置文件
    
sudo apt search <keyword> # 查找软件包命令
    
apt list --installed # 列出所有已安装的包
    
apt list --all-versions # 列出所有已安装的包的版本信息
```
