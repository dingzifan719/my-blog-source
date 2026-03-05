---
title: Shell脚本学习基础
date: 2026-03-05 20:43:55
tags: [Shell, Linux, 脚本编程]
---

# Shell脚本学习基础

## 1. Shell脚本简介

Shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序。Shell脚本是一种为Shell编写的脚本程序，类似于Windows下的批处理文件。

Shell脚本的优点：
- 自动化日常任务
- 批处理文件操作
- 快速原型开发
- 系统管理和维护

常见的Shell类型：
- Bash (Bourne Again SHell) - 最常用的Shell
- Sh (Bourne Shell) - 早期的Shell
- Csh (C Shell) - 类似C语言的语法
- Ksh (Korn Shell) - 结合了Bash和Csh的特点

## 2. 第一个Shell脚本

### 创建脚本文件

使用文本编辑器创建一个脚本文件，扩展名为 `.sh`：

```bash
touch hello.sh

### 编写脚本内容

```bash
#!/bin/bash
# 这是一个简单的Shell脚本示例
echo "Hello, World!"
echo "当前日期和时间：$(date)"
echo "当前工作目录：$(pwd)"
echo "当前登录用户：$(whoami)"

### 执行脚本

1. 给脚本添加执行权限：
   ```bash
   chmod +x hello.sh
   ```

2. 执行脚本：
   ```bash
   ./hello.sh
   ```
   或者
   ```bash
   bash hello.sh
   ```

## 3. Shell变量

### 变量定义

```bash
# 定义变量，等号两边不能有空格
name="Shell脚本"
age=20

# 使用变量
echo "变量name的值：$name"
echo "变量age的值：${age}岁"

# 只读变量
readonly PI=3.14159

# 删除变量（不能删除只读变量）
unset age
```

### 环境变量

```bash
# 查看所有环境变量
env

# 查看特定环境变量
echo $PATH
echo $HOME
echo $USER

# 设置环境变量
export MY_VAR="my value"
```

### 位置参数变量

```bash
#!/bin/bash
# 脚本名称：params.sh
echo "脚本名称：$0"
echo "第一个参数：$1"
echo "第二个参数：$2"
echo "参数个数：$#"
echo "所有参数：$@"
echo "所有参数（作为一个字符串）：$*"

执行：bash
./params.sh arg1 arg2
```

## 4. Shell基本运算符

### 算术运算符

```bash
#!/bin/bash

a=10
b=20

# 加法
echo "a + b = $((a + b))"
echo "a + b = $(expr $a + $b)"

# 减法
echo "a - b = $((a - b))"

# 乘法（注意expr需要转义*）
echo "a * b = $((a * b))"
echo "a * b = $(expr $a \* $b)"

# 除法
echo "b / a = $((b / a))"

# 取余
echo "b % a = $((b % a))"

# 赋值
echo "a = $a"

# 相等
echo "a == b ? $((a == b))"

# 不相等
echo "a != b ? $((a != b))"
```

### 关系运算符

```bash
#!/bin/bash

a=10
b=20

if [ $a -eq $b ]
then
    echo "a 等于 b"
else
    echo "a 不等于 b"
fi

if [ $a -ne $b ]
then
    echo "a 不等于 b"
fi

if [ $a -gt $b ]
then
    echo "a 大于 b"
else
    echo "a 不大于 b"
fi

if [ $a -lt $b ]
then
    echo "a 小于 b"
fi

if [ $a -ge $b ]
then
    echo "a 大于等于 b"
fi

if [ $a -le $b ]
then
    echo "a 小于等于 b"
fi
```

### 字符串运算符

```bash
#!/bin/bash

a="hello"
b="world"

if [ $a = $b ]
then
    echo "a 等于 b"
else
    echo "a 不等于 b"
fi

if [ $a != $b ]
then
    echo "a 不等于 b"
fi

if [ -z $a ]
then
    echo "a 是空字符串"
else
    echo "a 不是空字符串"
fi

if [ -n $a ]
then
    echo "a 不是空字符串"
fi

if [ $a ]
then
    echo "a 不是空"
fi
```

### 文件测试运算符

```bash
#!/bin/bash

file="test.txt"

if [ -e $file ]
then
    echo "文件存在"
else
    echo "文件不存在"
fi

if [ -f $file ]
then
    echo "是普通文件"
fi

if [ -d $file ]
then
    echo "是目录"
fi

if [ -r $file ]
then
    echo "文件可读"
fi

if [ -w $file ]
then
    echo "文件可写"
fi

if [ -x $file ]
then
    echo "文件可执行"
fi
```

## 5. Shell条件语句

### if语句

```bash
#!/bin/bash

read -p "请输入一个数字：" num

if [ $num -gt 0 ]
then
    echo "这是一个正数"
elif [ $num -lt 0 ]
then
    echo "这是一个负数"
else
    echo "这是零"
fi
```

### case语句

```bash
#!/bin/bash

read -p "请选择一个选项(1-3)：" choice

case $choice in
    1)
        echo "你选择了选项1"
        ;;
    2)
        echo "你选择了选项2"
        ;;
    3)
        echo "你选择了选项3"
        ;;
    *)
        echo "无效选项"
        ;;
esac
```

## 6. Shell循环语句

### for循环

```bash
#!/bin/bash

# 遍历数字
echo "遍历数字："
for i in {1..5}
do
    echo "数字：$i"
done

# 遍历列表
echo "\n遍历列表："
fruits=("apple" "banana" "orange" "grape")
for fruit in "${fruits[@]}"
do
    echo "水果：$fruit"
done

# C风格for循环
echo "\nC风格for循环："
for ((i=0; i<5; i++))
do
    echo "i = $i"
done
```

### while循环

```bash
#!/bin/bash

count=1
while [ $count -le 5 ]
do
    echo "计数：$count"
    count=$((count + 1))
done

# 无限循环（按Ctrl+C退出）
# while true
# do
#     echo "这是一个无限循环"
#     sleep 1
# done
```

### until循环

```bash
#!/bin/bash

count=1
until [ $count -gt 5 ]
do
    echo "计数：$count"
    count=$((count + 1))
done
```

## 7. Shell函数

### 函数定义和调用

```bash
#!/bin/bash

# 定义函数
greeting() {
    echo "Hello, $1!"
    echo "当前时间：$(date)"
}

# 调用函数
greeting "Shell"
```

### 带返回值的函数

```bash
#!/bin/bash

# 定义带返回值的函数
add() {
    local sum=$(( $1 + $2 ))
    echo $sum
}

# 调用函数并获取返回值
result=$(add 10 20)
echo "10 + 20 = $result"
```

## 8. 常见Shell命令

### 文件和目录操作

```bash
# 列出文件和目录
ls -la

# 创建目录
mkdir dir1 dir2

# 创建空文件
touch file.txt

# 复制文件或目录
cp file.txt file_copy.txt
cp -r dir1 dir1_copy

# 移动或重命名文件/目录
mv file.txt new_file.txt
mv dir1 new_dir

# 删除文件
rm file.txt

# 删除目录
rm -rf dir1

# 查看文件内容
cat file.txt
head -5 file.txt
 tail -5 file.txt

# 搜索文件内容
grep "pattern" file.txt
```

### 系统信息

```bash
# 查看系统版本
uname -a
cat /etc/os-release

# 查看内存使用情况
free -h

# 查看磁盘使用情况
df -h

# 查看进程
ps aux
top

# 查看网络连接
netstat -tuln
ss -tuln
```

## 9. Shell脚本调试

### 调试选项

```bash
# 执行时显示命令
bash -x script.sh

# 检查语法错误
bash -n script.sh

# 进入调试模式
bash -v script.sh
```

### 在脚本中添加调试信息

```bash
#!/bin/bash

# 设置调试模式
set -x

echo "调试模式开启"
name="Shell"
echo "变量name：$name"

# 关闭调试模式
set +x

echo "调试模式关闭"
```

## 10. 脚本示例：自动备份

```bash
#!/bin/bash

# 定义备份目录和源目录
BACKUP_DIR="/backup"
SOURCE_DIR="/home/user/documents"

# 创建备份目录（如果不存在）
mkdir -p $BACKUP_DIR

# 生成备份文件名（包含日期）
BACKUP_FILE="$BACKUP_DIR/documents_$(date +%Y%m%d_%H%M%S).tar.gz"

# 执行备份
echo "开始备份..."
tar -czf $BACKUP_FILE $SOURCE_DIR

# 检查备份是否成功
if [ $? -eq 0 ]
then
    echo "备份成功！备份文件：$BACKUP_FILE"
else
    echo "备份失败！"
    exit 1
fi

# 清理7天前的备份文件
echo "清理旧备份..."
find $BACKUP_DIR -name "documents_*.tar.gz" -type f -mtime +7 -delete

echo "备份任务完成！"
```

## 总结

本教程介绍了Shell脚本的基础知识，包括：
- Shell脚本的基本概念和创建方法
- 变量定义和使用
- 基本运算符（算术、关系、字符串、文件测试）
- 条件语句（if-elif-else、case）
- 循环语句（for、while、until）
- 函数定义和调用
- 常见Shell命令
- 脚本调试技巧
- 一个自动备份的脚本示例

通过学习这些基础知识，你可以开始编写简单的Shell脚本来自动化日常任务。随着实践的深入，你可以学习更高级的Shell脚本编程技巧，如正则表达式、进程控制、信号处理等。

Happy scripting! 🚀