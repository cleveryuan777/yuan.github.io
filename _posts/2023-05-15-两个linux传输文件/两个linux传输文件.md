在日常服务器租用中，有时需要将文件从一台服务器传到另一台服务器，下面给大家介绍四种linux服务器之间传输文件方式。

## **1. scp**

【优点】简单方便，安全可靠；支持限速参数

【缺点】不支持排除目录

【用法】

scp就是secure copy，是用来进行远程文件拷贝的。数据传输使用 ssh，并且和ssh 使用相同的认证方式，提供相同的安全保证 。

命令格式：

scp [参数]  < 源地址（用户名@IP地址或主机名）>:<文件路径> <目的地址（用户名 @IP 地址或主机名）>:<文件路径>

举例：

scp /home/work/source.txt work@192.168.0.10:/home/work/ #把本地的source.txt文件拷贝到192.168.0.10机器上的/home/work目录下

scp work@192.168.0.10:/home/work/source.txt /home/work/ #把192.168.0.10机器上的source.txt文件拷贝到本地的/home/work目录下

scp work@192.168.0.10:/home/work/source.txt work@192.168.0.11:/home/work/ #把192.168.0.10机器上的source.txt文件拷贝到192.168.0.11机器的/home/work目录下

scp -r /home/work/sourcedir work@192.168.0.10:/home/work/ #拷贝文件夹，加-r参数

scp -r /home/work/sourcedir work@www.myhost.com:/home/work/ #使用主机名

scp -r -v /home/work/sourcedir work@www.myhost.com:/home/work/ #显示详情，加-v参数

## **2. rcp**

【概述】

目标主机需要事先打开rcp功能，并设置好rcp的权限：把源主机加入到可信任主机列表中，否则无法在源主机上使用rcp远程复制文件到目标主机。

## **3. wget**

【优点】简单方便，支持排除目录，支持限速参数 【缺点】只能从远程机器将文件或文件夹下载到本地，并且远程机器需要支持ftp服务（例如启动proftpd）；参数较多，使用上比scp复杂

【用法】

wget是一个从网络上自动下载文件的自由工具，支持通过HTTP、HTTPS、FTP三个最常见的TCP/IP协议下载，并可以使用HTTP代理。

命令格式：

wget [参数] ftp://<目标机器ip或主机名>/<文件的绝对路径> #proftpd格式

举例：

wget ftp://192.168.0.10//home/work/source.txt #从192.168.0.10上拷贝文件夹source.txt

wget ftp://www.myhost.com//home/work/source.txt #使用主机名

wget -nH -P /home/work/ ftp://www.myhost.com//home/work/source.txt #指定本地保存路径，使用参数“-P 路径”或者“--directory-prefix=路径”；-nH, --no-host-directories 不创建主机目录

wget -r -l 0 -nH -P /home/work/ ftp://www.myhost.com//home/work/sourcedir #递归下载sourcedir目录，使用参数-r；参数-l, --level=NUMBER 最大递归深度 (inf 或 0 代表无穷).

wget --cut-dirs=3 -r -l 0 -nH -P /home/work/ ftp://www.myhost.com//home/work/sourcedir #-参数-cut-dirs=NUMBER 忽略 NUMBER层远程目录，本例中将myhost上的sourcedir目录保存到本地的work目录下。

wget --limit-rate=200k --cut-dirs=3 -r -l 0 -nH -P /home/work/ ftp://www.myhost.com//home/work/sourcedir #-参数--limit-rate=RATE 限定下载输率

wget --limit-rate=200k --cut-dirs=3 -r -l 0 -nH -P /home/work/ -X /home/work/sourcedir/notincludedir ftp://www.myhost.com//home/work/sourcedir #排除路径使用-X参数

wget -q --limit-rate=200k --cut-dirs=3 -r -l 0 -nH -P /home/work/ -X /home/work/sourcedir/notincludedir ftp://www.myhost.com//home/work/sourcedir #参数-q表示安静模式，无输出；默认是-v，冗余模式

## **4. rsync**

【优点】功能强大，操作类似scp，支持排除目录，支持限速参数；还支持本地复制。

【缺点】暂无

【用法】

rsync是类unix系统下的数据镜像备份工具，从软件的命名上就可以看出来了——remote sync。它的操作方式和scp和相似，但是比scp强大很多。使用双冒号分割主机名和文件路径时，是使用rsync服务器，这里不做介绍。

命令格式：

rsync [参数] < 源地址（用户名@IP地址或主机名）>:<文件路径> <目的地址（用户名 @IP 地址或主机名）>:<文件路径>

举例：

rsync /home/work/source.txt work@192.168.0.10:/home/work/ #把本地的source.txt文件拷贝到192.168.0.10机器上的/home/work目录下

rsync work@192.168.0.10:/home/work/source.txt /home/work/ #把192.168.0.10机器上的source.txt文件拷贝到本地的/home/work目录下

rsync work@192.168.0.10:/home/work/source.txt work@192.168.0.11:/home/work/ #把192.168.0.10机器上的source.txt文件拷贝到192.168.0.11机器的/home/work目录下

rsync -r /home/work/sourcedir work@192.168.0.10:/home/work/ #拷贝文件夹，加-r参数

rsync -r /home/work/sourcedir work@www.myhost.com:/home/work/ #使用主机名

rsync -r -v /home/work/sourcedir work@www.myhost.com:/home/work/ #显示详情，加-v参数

rsync -r -v --exclude sourcedir/notinclude /home/work/sourcedir work@www.myhost.com:/home/work/ #排除子目录，注意：--exclude后面的路径不能为绝对路径，必须为相对路径才可以，否则匹配不上，就不会被排除掉。