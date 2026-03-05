---
title: Linux常用命令大全
date: 2026-03-05 21:00:27
tags: [Linux, 命令行, 系统管理]
---

# Linux常用命令大全

Linux命令是管理和操作Linux系统的强大工具。本文总结了Linux系统中最常用的命令，按照不同类别进行组织，方便查阅和学习。

## 1. 文件和目录操作

### 1.1 查看目录内容

```bash
# 列出目录内容
ls

# 列出详细信息（包括权限、所有者、大小等）
ls -l

# 显示所有文件（包括隐藏文件，以.开头的文件）
ls -a

# 以人类可读的方式显示文件大小（KB、MB、GB）
ls -lh

# 按文件大小排序（从大到小）
ls -lhS

# 按修改时间排序（最新的在前面）
ls -lht
```

### 1.2 目录操作

```bash
# 创建目录
mkdir dir_name

# 创建多级目录
mkdir -p dir1/dir2/dir3

# 切换目录
cd dir_name

# 回到上一级目录
cd ..

# 回到用户主目录
cd ~

# 回到上次所在的目录
cd -

# 显示当前工作目录的绝对路径
pwd

# 删除空目录
rmdir dir_name

# 删除目录及其内容（递归删除，慎用！）
rm -rf dir_name
```

### 1.3 文件操作

```bash
# 创建空文件或更新文件时间戳
touch file_name

# 复制文件
cp source_file destination_file

# 复制目录及其内容
cp -r source_dir destination_dir

# 移动或重命名文件/目录
mv old_name new_name

# 删除文件
rm file_name

# 强制删除文件（不提示确认）
rm -f file_name

# 查看文件类型
file file_name

# 比较两个文件的差异
diff file1 file2

# 查找文件（在当前目录及其子目录中查找名为file_name的文件）
find . -name "file_name"

# 查找大于100MB的文件
find . -type f -size +100M

# 查找最近7天内修改过的文件
find . -type f -mtime -7
```

## 2. 文件内容查看和编辑

### 2.1 查看文件内容

```bash
# 查看文件全部内容
cat file_name

# 显示行号并查看文件内容
cat -n file_name

# 查看文件开头几行（默认10行）
head file_name

# 查看文件开头20行
head -20 file_name

# 查看文件结尾几行（默认10行）
tail file_name

# 查看文件结尾20行
tail -20 file_name

# 实时查看文件内容（常用于日志文件）
tail -f file_name

# 分页查看文件内容（按空格键翻页，按q退出）
less file_name

# 分页查看文件内容（支持搜索，按/搜索，按q退出）
more file_name

# 查看文件内容并显示行号
nl file_name
```

### 2.2 文本编辑

```bash
# 使用vi编辑器编辑文件
vi file_name

# 使用vim编辑器编辑文件（功能更强大的vi）
vim file_name

# 使用nano编辑器编辑文件（简单易用）
nano file_name

# 使用echo命令将文本写入文件（覆盖原有内容）
echo "Hello, Linux!" > file_name

# 使用echo命令将文本追加到文件末尾
echo "Welcome to Linux!" >> file_name

# 使用cat命令创建文件并写入内容（按Ctrl+D结束）
cat > file_name
```

## 3. 系统信息和管理

### 3.1 系统信息

```bash
# 查看Linux内核版本
uname -a

# 查看系统版本信息
cat /etc/os-release

# 查看主机名
hostname

# 查看当前时间和日期
date

# 查看日历
cal

# 查看系统运行时间
uptime

# 查看系统负载
w

# 查看系统进程信息
top

# 查看系统内存使用情况
free -h

# 查看磁盘使用情况
df -h

# 查看CPU信息
lscpu

# 查看硬件信息
hardinfo

### 3.2 系统管理

```bash
# 重启系统
reboot

# 关闭系统
shutdown -h now

# 延迟30分钟后关闭系统
shutdown -h +30

# 取消定时关闭
shutdown -c

# 查看系统服务状态
systemctl status service_name

# 启动系统服务
systemctl start service_name

# 停止系统服务
systemctl stop service_name

# 重启系统服务
systemctl restart service_name

# 设置系统服务开机自启
systemctl enable service_name

# 禁止系统服务开机自启
systemctl disable service_name
```

## 4. 网络命令

### 4.1 网络连接

```bash
# 查看网络接口信息
ifconfig

# 查看网络接口信息（新命令）
ip addr

# 查看网络路由表
route -n

# 查看网络路由表（新命令）
ip route

# 测试网络连接
ping google.com

# 测试网络连接（发送5个包后停止）
ping -c 5 google.com

# 查看网络连接状态
netstat -tuln

# 查看网络连接状态（新命令）
ss -tuln

# 查看网络连接详细信息
netstat -an

# 查看当前主机的网络连接情况
lsof -i
```

### 4.2 文件传输

```bash
# 使用ssh远程登录
ssh username@remote_host

# 使用scp复制文件到远程服务器
scp local_file username@remote_host:/remote/path

# 使用scp从远程服务器复制文件到本地
scp username@remote_host:/remote/file local/path

# 使用scp复制目录到远程服务器
scp -r local_dir username@remote_host:/remote/path

# 使用wget下载文件
wget https://example.com/file.zip

# 使用curl下载文件
curl -O https://example.com/file.zip

# 使用curl发送HTTP请求
curl https://example.com/api
```

## 5. 压缩和解压缩

### 5.1 ZIP格式

```bash
# 创建zip压缩文件
zip archive.zip file1 file2 dir1

# 创建zip压缩文件并显示压缩进度
zip -r archive.zip dir1

# 解压zip文件
unzip archive.zip

# 查看zip文件内容
unzip -l archive.zip
```

### 5.2 TAR格式

```bash
# 创建tar归档文件
 tar -cf archive.tar file1 file2 dir1

# 创建tar.gz压缩文件（使用gzip压缩）
 tar -czf archive.tar.gz file1 file2 dir1

# 创建tar.bz2压缩文件（使用bzip2压缩）
 tar -cjf archive.tar.bz2 file1 file2 dir1

# 创建tar.xz压缩文件（使用xz压缩）
 tar -cJf archive.tar.xz file1 file2 dir1

# 查看tar文件内容
 tar -tf archive.tar

# 解压tar文件
 tar -xf archive.tar

# 解压tar.gz文件
 tar -xzf archive.tar.gz

# 解压tar.bz2文件
 tar -xjf archive.tar.bz2

# 解压tar.xz文件
 tar -xJf archive.tar.xz

# 解压到指定目录
 tar -xzf archive.tar.gz -C /path/to/directory
```

## 6. 用户和权限管理

### 6.1 用户管理

```bash
# 创建用户
useradd username

# 创建用户并指定主目录
useradd -m -d /home/username username

# 设置用户密码
passwd username

# 修改用户信息
usermod -l new_username old_username

# 删除用户
userdel username

# 删除用户及其主目录
userdel -r username

# 查看当前登录用户
whoami

# 查看所有登录用户
who

# 查看用户登录历史
last

# 切换用户
su username

# 切换到root用户
su

# 以root权限执行命令
sudo command
```

### 6.2 组管理

```bash
# 创建组
groupadd groupname

# 修改组信息
groupmod -n new_groupname old_groupname

# 删除组
groupdel groupname

# 将用户添加到组
usermod -aG groupname username

# 查看用户所属的组
groups username

# 查看当前用户所属的组
groups
```

### 6.3 权限管理

```bash
# 查看文件/目录权限
ls -l

# 修改文件/目录权限
chmod permissions file_name

# 示例：给文件添加执行权限
chmod +x script.sh

# 示例：给所有者添加读写权限，给组和其他人添加读权限
chmod 644 file.txt

# 修改文件所有者
chown username file_name

# 修改文件所有者和组
chown username:groupname file_name

# 修改目录及其内容的所有者和组
chown -R username:groupname dir_name

权限说明：
- r: 读权限 (4)
- w: 写权限 (2)
- x: 执行权限 (1)
- 第一组：所有者权限
- 第二组：所属组权限
- 第三组：其他用户权限
```

## 7. 进程管理

```bash
# 查看所有进程
ps aux

# 查看所有进程（树状结构）pstree

# 实时查看进程动态
top

# 查看进程详细信息
ps -ef | grep process_name

# 根据进程ID查看进程信息
ps -p pid

# 杀死进程
kill pid

# 强制杀死进程
kill -9 pid

# 根据进程名杀死进程
pkill process_name

# 查看进程打开的文件
lsof -p pid

# 查看端口占用情况
lsof -i :port_number

# 查看进程的CPU和内存使用情况
top -p pid
```

## 8. 磁盘和文件系统

```bash
# 查看磁盘分区
fdisk -l

# 查看磁盘使用情况
df -h

# 查看目录占用空间
du -sh dir_name

# 查看目录及其子目录占用空间
du -h dir_name

# 查看文件系统类型
mount | grep -v tmpfs

# 挂载文件系统
mount /dev/sda1 /mnt

# 卸载文件系统
umount /mnt

# 检查文件系统
e2fsck /dev/sda1

# 格式化磁盘
mkfs.ext4 /dev/sda1
```

## 9. 文本处理

```bash
# 在文件中搜索字符串
grep "pattern" file_name

# 在文件中搜索字符串并显示行号
grep -n "pattern" file_name

# 递归搜索目录中的字符串
grep -r "pattern" dir_name

# 忽略大小写搜索
grep -i "pattern" file_name

# 显示不包含匹配的行
grep -v "pattern" file_name

# 统计匹配的行数
grep -c "pattern" file_name

# 替换文件中的字符串（将old替换为new）
sed 's/old/new/g' file_name

# 替换文件中的字符串并保存更改
sed -i 's/old/new/g' file_name

# 排序文件内容
sort file_name

# 去重排序
uniq file_name

# 统计文件行数、单词数、字符数
wc file_name

# 查看文件行数
wc -l file_name

# 查看文件单词数
wc -w file_name

# 查看文件字符数
wc -c file_name
```

## 10. 其他常用命令

### 10.1 系统工具

```bash
# 查看命令历史
history

# 执行历史命令
!n  # 执行第n条历史命令
!!  # 执行上一条命令

# 清除命令历史
history -c

# 显示命令的位置
which command_name

# 显示命令的详细信息
whatis command_name

# 查看命令的帮助文档
man command_name

# 查看命令的简要帮助
command_name --help

# 显示当前环境变量
env

# 设置环境变量
export VAR_NAME=value

# 查看特定环境变量
echo $VAR_NAME

# 查看PATH环境变量
echo $PATH
```

### 10.2 磁盘和文件系统

```bash
# 创建软链接
ln -s source_file link_name

# 创建硬链接
ln source_file link_name

# 查看文件的inode号
ls -i file_name

# 查看文件的详细信息
stat file_name

# 查看当前目录的磁盘使用情况
du -sh .
```

### 10.3 网络和通信

```bash
# 查看DNS配置
cat /etc/resolv.conf

# 查看主机名配置
cat /etc/hostname

# 查看hosts文件
cat /etc/hosts

# 使用nslookup查询域名解析
nslookup example.com

# 使用dig查询域名解析
dig example.com

# 查看当前网络连接的详细信息
ethtool eth0
```

### 10.4 时间和日期

```bash
# 查看当前时间和日期
date

# 设置系统时间
date -s "2023-12-31 23:59:59"

# 查看硬件时钟
hwclock

# 将系统时间同步到硬件时钟
hwclock --systohc

# 将硬件时钟同步到系统时间
hwclock --hctosys
```

## 总结

本文总结了Linux系统中最常用的命令，涵盖了文件操作、系统管理、网络命令、压缩解压、用户和权限管理、进程管理、磁盘和文件系统、文本处理等多个方面。这些命令是Linux系统管理和日常使用的基础，掌握它们可以提高工作效率，更好地管理和使用Linux系统。

当然，Linux命令远不止这些，还有很多更高级和专业的命令等待你去探索。建议你在实际使用中不断学习和积累，以便更好地利用Linux系统的强大功能。

Happy Linuxing! 🐧