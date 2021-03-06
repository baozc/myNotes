### 撷取命令： cut, grep
> 什么是撷取命令啊？说穿了，就是将一段数据经过分析后，取出我们所想要的。 或者是，经由分析关键词，取得我们所想要的那一行！ 不过，要注意的是，一般来说，撷取讯息通常是针对『一行一行』来分析的，并不是整篇讯息分析的喔～底下我们介绍两个很常用的讯息撷取命令：

- cut

cut 不就是『切』吗？没错啦！这个指令可以将一段讯息的某一段给他『切』出来～ 处理的讯息是以『行』为单位喔！底下我们就来谈一谈：

```
[root@linux ~]# cut -d'分隔字符' -f fields
[root@linux ~]# cut -c 字符区间
参数：
-d ：后面接分隔字符。与 -f 一起使用；
-f ：依据 -d 的分隔字符将一段讯息分割成为数段，用 -f 取出第几段的意思；
-c ：以字符 (characters) 的单位取出固定字符区间；

范例：
范例一：将 PATH 变量取出，我要找出第三个路径。
[root@linux ~]# echo $PATH
/bin:/usr/bin:/sbin:/usr/sbin:/usr/local/bin:/usr/X11R6/bin:/usr/games:
[root@linux ~]# echo $PATH | cut -d ':' -f 5
# 嘿嘿！如此一来，就会出现 /usr/local/bin 这个目录名称！
# 因为我们是以 : 作为分隔符，第五个就是 /usr/local/bin 啊！
# 那么如果想要列出第 3 与第 5 呢？，就是这样：
[root@linux ~]# echo $PATH | cut -d ':' -f 3,5

范例二：将 export 输出的讯息，取得第 12 字符以后的所有字符串
[root@linux ~]# export
declare -x HISTSIZE="1000"
declare -x INPUTRC="/etc/inputrc"
declare -x KDEDIR="/usr"
declare -x LANG="zh_TW.big5"
......其它省略......
[root@linux ~]# export | cut -c 12-
HISTSIZE="1000"
INPUTRC="/etc/inputrc"
KDEDIR="/usr"
LANG="zh_TW.big5"
......其它省略......
# 知道怎么回事了吧？用 -c 可以处理比较具有格式的输出数据！
# 我们还可以指定某个范围的值，例如第 12-20 的字符，就是 cut -c 12-20 等等！

范例三：用 last 将这个月登入者的信息中，仅留下使用者大名
[root@linux ~]# last
vbird tty1 192.168.1.28 Mon Aug 15 11:55 - 17:48 (05:53)
vbird tty1 192.168.1.28 Mon Aug 15 10:17 - 11:54 (01:37)
[root@linux ~]# last | cut -d ' ' -f 1
# 用 last 可以取得最近一个月登入主机的使用者信息，
# 而我们可以利用空格符的间隔，取出第一个信息，就是使用者账号啰！
# 但是因为 vbird tty1 之间空格有好几个，并非仅有一个，所以，如果要找出
# tty1 其实不能以 cut -d ' ' -f 1,2 喔！输出的结果会不是我们想要的。
```

这个 cut 实在很好用！不过，说真的，除非你常常在分析 log 档案，否则使用到 cut 的机会并不多！好了！ cut 主要的用途在于将『同一行里面的数据进行分解！』， 最常使用在分析一些数据或文字数据的时候！这是因为有时候我们会以某些字符当作分割的参数， 然后来将数据加以切割，以取得我们所需要的数据。我也很常使用这个功能呢！尤其是在分析 log 档案的时候！不过， cut 在处理多空格相连的数据时，可能会比较吃力一点～

- grep

刚刚的 cut 是将一行讯息当中，取出某部分我们想要的，而 grep 则是分析一行讯息， 若当中有我们所需要的信息，就将该行拿出来～简单的语法是这样的：  
```
[root@linux ~]# grep [-acinv] '搜寻字符串' filename
参数：
-a ：将 binary 档案以 text 档案的方式搜寻数据
-c ：计算找到 '搜寻字符串' 的次数
-i ：忽略大小写的不同，所以大小写视为相同
-n ：顺便输出行号
-v ：反向选择，亦即显示出没有 '搜寻字符串' 内容的那一行！

范例：
范例一：将 last 当中，有出现 root 的那一行就取出来；
[root@linux ~]# last | grep 'root'

范例二：与范例一相反，只要没有 root 的就取出！
[root@linux ~]# last | grep -v 'root'

范例三：在 last 的输出讯息中，只要有 root 就取出，并且仅取第一栏
[root@linux ~]# last | grep 'root' |cut -d ' ' -f1
# 在取出 root 之后，利用上个指令 cut 的处理，就能够仅取得第一栏啰！
```

grep 是个很棒的指令喔！他支持的语法实在是太多了～用在正规表示法里头， 能够处理的数据实在是多的很～不过，我们这里先不谈正规表示法～下一章再来说明～ 您先了解一下， grep 可以解析一行文字，取得关键词，若该行有存在关键词， 就会整行列出来！
