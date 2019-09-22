---
layout: post
title:  "awk速查手册"
date:   2019-08-25 19:01:42 +0800
categories: tech
typora-root-url: ..
---

## 基础

### 数据

```shell
cat score.txt
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62

cat netstat.txt
Proto Recv-Q Send-Q Local-Address          Foreign-Address             State
tcp        0      0 0.0.0.0:3306           0.0.0.0:*                   LISTEN
tcp        0      0 0.0.0.0:80             0.0.0.0:*                   LISTEN
tcp        0      0 127.0.0.1:9000         0.0.0.0:*                   LISTEN
tcp        0      0 coolshell.cn:80        124.205.5.146:18245         TIME_WAIT
tcp        0      0 coolshell.cn:80        61.140.101.185:37538        FIN_WAIT2
tcp        0      0 coolshell.cn:80        110.194.134.189:1032        ESTABLISHED
tcp        0      0 coolshell.cn:80        123.169.124.111:49809       ESTABLISHED
tcp        0      0 coolshell.cn:80        116.234.127.77:11502        FIN_WAIT2
tcp        0      0 coolshell.cn:80        123.169.124.111:49829       ESTABLISHED
tcp        0      0 coolshell.cn:80        183.60.215.36:36970         TIME_WAIT
tcp        0   4166 coolshell.cn:80        61.148.242.38:30901         ESTABLISHED
tcp        0      1 coolshell.cn:80        124.152.181.209:26825       FIN_WAIT1
tcp        0      0 coolshell.cn:80        110.194.134.189:4796        ESTABLISHED
tcp        0      0 coolshell.cn:80        183.60.212.163:51082        TIME_WAIT
tcp        0      1 coolshell.cn:80        208.115.113.92:50601        LAST_ACK
tcp        0      0 coolshell.cn:80        123.169.124.111:49840       ESTABLISHED
tcp        0      0 coolshell.cn:80        117.136.20.85:50025         FIN_WAIT2
tcp        0      0 :::22                  :::*                        LISTEN
```

### 字段

```shell
awk '{print $0}' demo.txt
# $0代表当前行，$1、$2、$3分别代表第1、2、3个字段
```

### 分隔符

```java
1)默认分隔符为空格或制表符

2)指定分隔符为冒号
awk -F: '{print $1}' /etc/passwd
awk -F':' '{print $1}' /etc/passwd

3)多个字符作为分隔符
awk 'BEGIN{FS="\",\""}{print NF}' a.txt

4)多个分隔符
awk -F '[:; ]+' '{print $1}' a.txt
# +表示多个连在一起的分隔符算一个

参考资料：
关于其中的一些知识点可以参看gawk的手册：http://www.gnu.org/software/gawk/manual/gawk.html
内建变量，参看：http://www.gnu.org/software/gawk/manual/gawk.html#Built_002din-Variables
流控方面，参看：http://www.gnu.org/software/gawk/manual/gawk.html#Statements
内建函数，参看：http://www.gnu.org/software/gawk/manual/gawk.html#Built_002din
正则表达式，参看：http://www.gnu.org/software/gawk/manual/gawk.html#Regexp
```

### 变量

```shell
# 输出行号
head -4 netstat.txt | awk '{print NR, $3, $6}'
```

```shell
$0：当前记录（这个变量中存放着整个行的内容）
$1~$n：当前记录的第n个字段，字段间由FS分隔
FS：字段分隔符，默认是空格和制表符。
NF：当前记录中的字段个数，就是有多少列
NR：已经读出的记录数，就是行号，从1开始，如果有多个文件话，这个值也是不断累加中。
FNR：当前记录数，与NR不同的是，这个值会是各个文件自己的行号
RS：行分隔符，用于分割每一行，默认是换行符。
OFS：输出字段的分隔符，用于打印时分隔字段，默认为空格。
ORS：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
OFMT：数字输出的格式，默认为％.6g。
FILENAME：当前文件名
```

### 函数

```shell
awk -F ':' '{ print toupper($1) }' demo.txt
toupper()用于将字符转为大写
tolower()：字符转为小写。
length()：返回字符串长度。
substr()：返回子字符串。
sin()：正弦。
cos()：余弦。
sqrt()：平方根。
rand()：随机数。
```

### 过滤，条件

```shell
# 格式
awk '条件 动作' 文件名
# 输出包含usr的行
awk -F ':' '/usr/ {print $1}' demo.txt
# 输出奇数行
awk -F ':' 'NR % 2 == 1 {print $1}' demo.txt
# 输出第三行以后的行
awk -F ':' 'NR >3 {print $1}' demo.txt
# 输出第一个字段等于指定值的行
awk -F ':' '$1 == "root" {print $1}' demo.txt
awk -F ':' '$1 == "root" || $1 == "bin" {print $1}' demo.txt
# 第三个字段大于0
awk ' $3>0 {print $0}' netstat.txt
# 第三个字段等于0并且第6个字段为LISTEN并且输出表头
awk '$3==0 && $6=="LISTEN" || NR==1 ' demo.txt
# 第6个字段匹配模式，~表示模式开始，//中是模式(其实就是一个正则表达式的匹配)
awk '$6 ~ /FIN/ || NR==1 {print NR,$4,$5,$6}' demo.txt
# 模式取反
awk '$6 !~ /WAIT/ || NR==1 {print NR,$4,$5,$6}' demo.txt
```

### 过滤，if语句

```shell
# 输出第一个字段的第一个字符大于m的行
awk -F ':' '{if ($1 > "m") print $1}' demo.txt
awk -F ':' '{if ($1 > "m") print $1; else print "---"}' demo.txt
```

### 拆分文件

```shell
awk 'NR!=1{print > $6}' netstat.txt
awk 'NR!=1{print $0 > $6}' netstat.txt
```

### 统计

```shell
# 计算所有的C文件，CPP文件和H文件的文件大小总和
ls -l  *.cpp *.c *.h | awk '{sum+=$5} END {print sum}'
# 统计各个connection状态的用法
awk 'NR!=1{a[$6]++;} END {for (i in a) print i ", " a[i];}' netstat.txt
# 统计每个用户的进程的占了多少内存
ps aux | awk 'NR!=1{a[$1]+=$6;} END { for(i in a) print i ", " a[i]"KB";}'
```

### 脚本

```shell
$ cat cal.awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0
 
    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}

# 执行脚本
awk -f cal.awk score.txt
```

- [AWK 简明教程 - 酷壳](https://coolshell.cn/articles/9070.html)
- [awk 入门教程 - 阮一峰](http://www.ruanyifeng.com/blog/2018/11/awk.html)
- [The GNU Awk User’s Guide](http://www.gnu.org/software/gawk/manual/gawk.html)