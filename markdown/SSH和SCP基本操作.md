### 1、SSH远程登陆
### 1.1 初次登陆工作站
-  远程登陆命令：`ssh <username>@<ip>`
示例：
```shell
ssh se@192.168.3.25
```
输入密码后，连接到远程。
- 创建用户，并加入用户组sudo、配置sudo，使得采用sudo的命令不需要输入密码。
示例：
```bash
sudo adduser ljl
sudo -aG sudo ljl
sudo visudo
```
- 打开vim编辑器后，进入的是命令模式，按键盘`i` 进入编辑模式，输入`ljl ALL=(ALL) NOPASSWD:ALL` ，输入完成后，按`Esc` 切换至命令模式。
- 在命令模式中，按`:`进入底行模式，输入`wq`保存文件并退出vim编辑器。
### 1.2 以自己创建的用户登陆工作站
- 远程登陆命令：`ssh <username>@<ip>`
示例：
```bash
ljl@L0348:~$ ssh ljl@192.168.3.25
ljl@192.168.3.25's password:
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-41-generic x86_64)
```

### 2、SCP远程文件传输
### 2.1、准备阶段
-  在目录/home/ljl/src/test下创建文件hello.py
示例：
```bash
ljl@workstation:~$ pwd
/home/ljl
ljl@workstation:~$ mkdir src
ljl@workstation:~$ cd src
ljl@workstation:~/src$ mkdir test
ljl@workstation:~/src$ cd test
ljl@workstation:~/src/test$ touch hello.py
ljl@workstation:~/src/test$ ls -l
```
```
total 0
-rw-rw-r-- 1 ljl ljl 0  7月 16 10:57 hello.py
```
或者：
```
ljl@workstation:~$ mkdir src src/test
ljl@workstation:~$ touch src/test/hello.py
```

- 查询自身主机ip地址
	- 使用`ifconfig` 命令
	- 使用`ip addr show eth0`  命令
	- 使用`hostname -I` 命令
示例：
```bash
ljl@L0348:~$ ifconfig
```
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.22.128.251  netmask 255.255.240.0  broadcast 172.22.143.255
        inet6 fe80::215:5dff:fe94:a6cc  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:94:a6:cc  txqueuelen 1000  (Ethernet)
        RX packets 13905  bytes 19886131 (19.8 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 9954  bytes 759680 (759.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 12  bytes 600 (600.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 12  bytes 600 (600.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
上述输出中，`inet 172.22.128.251`行显示了 eth0 接口的 IP 地址。

```bash
ljl@L0348:~$ ip addr show eth0
```

```
6: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:94:a6:cc brd ff:ff:ff:ff:ff:ff
    inet 172.22.128.251/20 brd 172.22.143.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::215:5dff:fe94:a6cc/64 scope link
       valid_lft forever preferred_lft forever
```
上述输出中，`inet 172.22.128.251`行显示了 eth0 接口的 IP 地址。

```bash
ljl@L0348:~$ hostname -I
```
```
172.22.128.251
```

### 2.2、本地与远程服务器间文件传输

- 本地上传至服务器：
`scp /<本地文件路径>/<文件名> <username>@<远端ip地址>:/<文件存放路径>`
示例：
```bash
scp  hello.py ljl@192.168.3.25:~/src/test/ #文件在当前目录路径下

scp  ~/src/test/hello.py ljl@192.168.3.25:~/src/test/ #文件不在当前目录路径下
```
***注：`~` 代表当前用户目录/home/ljl***

- 从服务器下载文件至本地：
`scp <username>@<远端ip地址>:<远端文件存放路径>/<文件名> <本地存放路径>`
示例：
```bash
scp ljl@192.168.3.25:~/src/test/hello.py ~/src/test
ljl@192.168.3.25's password:
hello.py                                                                              100%    0     0.0KB/s   00:00

ls test -l
total 0
-rw-r--r-- 1 ljl1 ljl1 0 Jul 17 09:17 hello.py
```

- 如果传输的是整个文件夹，则在scp后面加个 -r  即可。
示例：
```bash
scp -r ~/src/test ljl@192.168.3.25:~/src/

scp -r ljl@192.168.3.25:~/src/test ~/src/
```

***注：以上命令都需在本地终端上执行！！！！***

