+++
title = "shell"
date = 2022-11-18T22:28:00+08:00
lastmod = 2022-11-18T22:29:11+08:00
draft = false
author = "geezer"
+++

## 基本bash shell命令 {#基本bash-shell命令}

1.  more命令用于浏览文件,实现了基本的翻页功能
2.  less命令可以理解为more的升级版,但实际上与more命令相差不大
3.  tail命令查看文件尾部的几行内容
4.  head命令与tail命令相反


## 更多bash shell命令 {#更多bash-shell命令}


### 进程相关命令 {#进程相关命令}

1.  ps命令
    Linux系统中使用的GNU ps命令支持3种不同类型的命令行参数：
      Unix风格的参数，前面加单破折线(-)
      BSD风格的参数，前面不加破折线
      GNU风格长参数,前面加双破折号(--)
    -   unix风格的参数

        | 参数 | 功能                               |
        |----|----------------------------------|
        | -A | 显示所有进程                       |
        | -N | 显示与指定参数不符的所有进程       |
        | -a | 显示除控制进（session leader）和无终端进程外的所有进程 |
        | -d | 显示除控制进程外的所有进程         |
        | -e | 显示所有进程                       |
        | -C | cmdlist显示包含在cmdlist列表中的进程 |
        | -G | grplist显示组ID在grplist列表中的进程 |
        | -U | userlist显示属主的用户ID在userlist列表中的进程 |
        | -s | sesslist                           |
        | -t | ttylist显示终端ID在ttylist列表中的进程 |
        | -u | userlist显示有效用户ID在userlist列表中的进程 |
        | -F | 显示更多额外输出（相对-f参数而言） |
        | -O | format显示默认的输出列以及format列表指定的特定列 |
        | -M | 显示进程的安全信息                 |
        | -c | 显示进程的额外调度器信息           |
        | -f | 显示完整格式的输出                 |
        | -j | 显示任务信息                       |
        | -l | 显示长列表                         |
        | -o | format仅显示由format指定的列       |
        | -y | 不要显示进程标记（processflag，表明进程状态的标记） |
        | -Z | 显示安全标签（security context）①信息 |
        | -H | 用层级格式来显示进程（树状，用来显示父进程） |
        | -n | namelist定义了WCHAN列显示的值      |
        | -w | 采用宽输出模式，不限宽度显示       |
        | -L | 显示进程中的线程                   |
        | -V | 显示ps命令的版本号                 |
2.  kill命令


### 文件相关命令 {#文件相关命令}


#### grep基本使用 {#grep基本使用}

<!--list-separator-->

-  反向搜索

    查找没有字母"t"的行
    grep -v t filename

<!--list-separator-->

-  显示查找内容的行号

    查找t所在的行并显示行号
    grep -n t filename

<!--list-separator-->

-  统计搜索结果的行数

    统计搜索结果中含有字母"t"的行数
    grep -c t filename

<!--list-separator-->

-  使用多种搜索模式

    查找含"t"或含"f"的行
    grep -e t -e f filename


## linux文件管理系统 {#linux文件管理系统}


### linux基本文件系统 {#linux基本文件系统}


#### ext {#ext}


## shell编程基础 {#shell编程基础}


### 构建基本脚本 {#构建基本脚本}


#### 创建shell脚本文件 {#创建shell脚本文件}

在shell脚本文件中用'#'表示注释,但第一行却是个例外,一般shell的第一行为

{{< highlight shell >}}
#!/bin/bash
{{< /highlight >}}

这一行告诉shell用哪个shell来运行脚本,当然这里的bash只是一个示例,你也可以用其他的shell.
除第一行外,其他行若以'#'开头则只起注释作用不做任何解释.


#### 变量 {#变量}

在shell中若要使用变量则需要在变量名前加'\\('才可以被解释为一个变量,
否则会被解释为普通字符串,若要输出'\\)'则可以使用'\\$'
在shell中变量命名的规则为字母,数字,下划线且长度不能超20个字符,变量名是区分大小写的.

-   变量创建变量创建只需使用变量名后加'='即可赋值也与变量创建方法相同注意:
    若要将一个变量赋值给其他变量,则必须要有'$',否则会被解释为字符串变量名和'='以及值之间不能有空格
    {{< highlight shell >}}
    value1=fsdfd

    value2=$value1
    {{< /highlight >}}


#### 命令替换 {#命令替换}

通常来说,若想获得一条命令的输出,可使用反引号'\`'或者'$()',

{{< highlight shell >}}

{{< /highlight >}}


#### 重定向输入输出 {#重定向输入输出}

-   输出重定向其基本功能是将命令的输出写到指定文件中,

    -   格式:
        ls(仅做示范,使用其他命令均可) &gt; 文件名
        ls &gt;&gt; 文件名

    符号'&gt;'表示覆盖文件符号'&gt;&gt;'表追加
-   输入重定向输入重定向和输出重定向正好相反。输入重定向将文件的内容重定向到命令.

    -   格式:
        wc &lt; 文件名

    wc是一个统计文本的命令,它会计算出文本的行数,词数,及字节数上面示例的含义即为统计文件的行数,词数,字节数

    -   格式2
        wc &lt;&lt; 标记

    这种格式成为内联输入重定向,它的使用需要一个标记,作为文本的结尾标记

    -   例如
        $ wc &lt;&lt; EOF
        &gt; test string 1
        &gt; test string 2
        &gt; test string 3
        &gt; EOF


#### 管道 {#管道}

其含义是将一个命令的输出作为另一个命令的输入,
利用重定向的方式可表示为:

{{< highlight text >}}
ls  -U  >  temp
sort  <  temp
{{< /highlight >}}

示例含义为将ls的不排序的输出结构写到temp文件,在用sort对temp文件排序输出\\
但linux提供了更简便的方式即我们可以利用'|'符号:

{{< highlight text >}}
ls -U  | sort
{{< /highlight >}}

这命令与上面的功能相同,并且还不需要中间文件


#### 使用数学表达式 {#使用数学表达式}

若要在shell中使用数学表达式则需要使用关键字'expr 表达式'或则使用'$[表达式]'

-   示例
    {{< highlight text >}}
    expr  4 / 2
    $[4 / 2]
    {{< /highlight >}}

上面描述的两种数学表达式的使用都不支持浮点数运算解决此问题最常用的是使用bash计算器bc,它能够识别数字,变量,注释,表达式,编程语句,函数而bc中存在内置变量scale它可以控制浮点数输出的位数,若为它赋值则默认为0,即不保留小数位利用其那面学的管道,我们就可以实现在shell脚本中使用bc

{{< highlight text >}}
var1=$(echo  "scale=4;10/3" |   bc)
{{< /highlight >}}

但如果是比较长的计算使用这种方法非常的麻烦,
所以我们可以使用前面所学的内联重定向输入'&lt;&lt;'

{{< highlight shell >}}
var1=10.46
var2=43.67
var3=33.2
var4=71
var5=$(bc << EOF
scale = 4
a1 = ( $var1 * $var2)
b1 = ($var3 * $var4)
a1 + b1
EOF
)
{{< /highlight >}}


#### 退出脚本 {#退出脚本}

-   退出码
    Linux提供了一个专门的变量$?来保存上个已执行命令的退出状态码。

    | 状态码 | 描述              |
    |-----|-----------------|
    | 0     | 命令成功结束      |
    | 1     | 一般性未知错误    |
    | 2     | 不适合的shell命令 |
    | 126   | 命令不可执行(一般为没有执行权限) |
    | 127   | 没找到命令        |
    | 128   | 无效的退出参数    |
    | 128+x | 与Linux信号x相关的严重错误 |
    | 130   | 通过Ctrl+C终止的命令 |
    | 255   | 正常范围之外的退出状态码 |
-   exit命令
    exit命令在脚本中可用于返回自己定义的退出码,一般来说shell脚本会返回最后一个命令执行所返回的退出码,而使用exit可以自己返回一个自定义的退出码
    {{< highlight text >}}
    命令开始
    .
    .
    .
    命令结束
    exit 8
    {{< /highlight >}}
    使用示例中的代码就可以实现返回自定义的退出码,示例中返回的是8


### 使用结构化命令 {#使用结构化命令}


#### if使用 {#if使用}

if后可以接受一个命令,if会根据命令的退出码来判断执行的分支,
退出码为0即命令正确执行,则执行then后的命令集,否则不执行或执行其他分支基本使用方式如下

{{< highlight shell >}}
 #!/bin/bash

 #形式1
 if 命令
 then
 ...
 fi

 #形式2
 if 命令
 then
...
 else
 ...
 fi
 #形式3
 if 命令
 then
 ...
 elif
 then
 ...
 else
 ...
 fi
{{< /highlight >}}


#### test命令 {#test命令}

由于if语句后面只能通过命令的退出码来判定执行分支,使得if使用很不方便
test命令就可以实现类似于其他编程语言的if判断规则,它的工作原理是但test命令中所执行的条件如果为真则返回状态码0,使得then可以执行,否则返回非0状态码,执行其他分支其基本能形式为:

{{< highlight shell >}}
  if test 条件
then
  ...
  fi
{{< /highlight >}}

也可以在if后面加'[条件]'来判断,这与test的功能是一样的

{{< highlight shell >}}
if  [条件]
then
...
fi
{{< /highlight >}}

-   test数值比较

    | 比较        | 描述          |
    |-----------|-------------|
    | n1  -eq  n2 | 检查n1是否与n2相等 |
    | n1  -ge n2  | 检查n1是否大于或等于n2 |
    | n1 -gt n2   | 检查n1是否大于n2 |
    | n1 -le n2   | 检查n1是否小于或等于n2 |
    | n1 -lt  n2  | 检查n1是否小于n2 |
    | n1 -ne  n2  | 检查n1是否不等于n2 |
-   test字符串比较

    | 比较           | 描述            |
    |--------------|---------------|
    | str1 = str2    | 检查str1是否和str2相同 |
    | str1 != str2   | 检查str1是否和str2不同 |
    | str1 &lt; str2 | 检查str1是否比str2小 |
    | str1 &gt; str2 | 检查str1是否比str2大 |
    | -n str1        | 检查str1的长度是否非0 |
    | -z str1        | 检查str1的长度是否为0 |
-   test文件比较

    | 比较            | 描述                   |
    |---------------|----------------------|
    | -d file         | 检查file是否存在并是一个目录 |
    | -e file         | 检查file是否存在       |
    | -f file         | 检查file是否存在并是一个文件 |
    | -r file         | 检查file是否存在并可读 |
    | -s file         | 检查file是否存在并非空 |
    | -w filed        | 检查file是否存在并可写 |
    | -x file         | 检查file是否存在并可执行 |
    | -O file         | 检查file是否存在并属当前用户所有 |
    | -G file         | 检查file是否存在并且默认组与当前用户相同 |
    | file1 -nt file2 | 检查file1是否比file2新 |
    | file1 -ot file2 | 检查file1是否比file2旧 |


#### 复合条件测试 {#复合条件测试}

if语句支持使用布尔值进行复合测试

{{< highlight text >}}
if  [ condition1 ] && [ condition2 ]
then
...
fi


if  [ condition1 ] || [ condition2 ]
then
...
fi
{{< /highlight >}}


#### if高级特征 {#if高级特征}

-   使用双括号双括号可以理解为是test数值检测的升级版,在双括号内不仅可以使用test的数值检测能使用的表达式还能使用一些其他常用的运算符

    | 符号       | 功能 |
    |----------|----|
    | val++      | 后增 |
    | val--      | 后减 |
    | ++val      | 先增 |
    | --val      | 先减 |
    | !          | 逻辑求反 |
    | ~          | 位求反 |
    | \\\*\*     | 幂运算 |
    | &lt;&lt;   | 左位移 |
    | &gt;&gt;   | 右位移 |
    | &amp;      | 位布尔和 |
    | 竖线       | 位布尔或 |
    | &amp;&amp; | 逻辑和 |
    | 双竖线     | 逻辑或 |
-   双方括号双括号只能对进行数学表达是运算,通俗来说就是运算对象只能是数值而双方括号则是test检测字符串的升级它提供了模式匹配功能,并且可以使用正则表达式进行匹配


#### case命令 {#case命令}

其功能类似于switch

{{< highlight shell >}}
case v in
p1 | p2) commands1;;
p3) commands2;;
*) default commands;;
esac
{{< /highlight >}}

其含义为若变量v与p1或p2匹配则执行commands1
若与p3匹配则执行commands2
若都不满足则执行commands


#### for语句 {#for语句}

{{< highlight shell >}}
list="a b c d"
  for var in list
  do
      echo "--$var"
  done
{{< /highlight >}}

上面语句会输出

{{< highlight text >}}
--a
--b
--c
--d
{{< /highlight >}}

这是因为for会自动对list进行分割,在shell中存在一个IFS环境变量定义了
shell用作字段分隔符的一系列字符,默认情况下shell会将空格,制表符,换行符作为分割符同时,通过修改IFS变量可以修改分割规则

{{< highlight shell >}}
IFS="/"
list="a/b/c"
for val in list
do
    echo  "$val"
done
{{< /highlight >}}

上面示例shell会将'_'作为分隔符,及会依次换行输出a,b,c而'/_'作为分隔符不会输出


#### C语言风格的for {#c语言风格的for}

{{< highlight shell >}}
for (( a = 1; a < 10; a++ ))
do
...
done
{{< /highlight >}}


#### whle语句 {#whle语句}

-   从10输出到1
    {{< highlight shell >}}
    var1=10
      while [ $var1 -gt 0 ]
      do
      echo $var1
      var1=$[ $var1 - 1 ]
    {{< /highlight >}}
-   while使用多个测试命令
    {{< highlight shell >}}
    #!/bin/bash
    # testing a multicommand while loop10
    var1=1011
    while echo $var1
    [ $var1 -ge 0 ]
    do
    echo "This is inside the loop"
    {{< /highlight >}}
    示例中的while后有两个测试语句,但while将最后一个语句的退出码做判断而位于它前面的语句只做执行,不用做while的判断


#### unit语句 {#unit语句}

与while语法相同但作用相反,只有测试语句的退出码返回0才会退出until

{{< highlight shell >}}
until   条件
do
...
done
{{< /highlight >}}


#### 循环控制 {#循环控制}

-   break
    与C语言的break功能相似,但shell中的break可以指定参数来表示退出的循环的级别,默认是1即退出当前循环
-   continue
    与C语言的continue功能相似,不过它也能指定参数表示要继续执行的循环的级别


### 处理用户输入 {#处理用户输入}


#### 参数 {#参数}

在shell脚本中用$0表运行的程序,$1表第一个参数,$2表第二个参数依次类推


#### 特殊参数变量 {#特殊参数变量}

-   $#表示传入shell中的参数个数
-   $\*和$@变量可以用来轻松访问所有的参数两者都会将所有传给shell的变量作为它的值
    $\*会将所有传给shell的参数当作整体处理
    $@则可以单独处理即在使用for时,如果将$\*作为list则不会对其中包含的变量进行拆分而$@则可以


#### 移动参数 {#移动参数}

shift命令可以实现移动参数,即shfit可以将所有参数(不包括$0)往左移动也就是使用shift后$1的值会等于$2,$2会等于$3,以此类推,而$1初始值会被丢弃


#### 获得用户输入 {#获得用户输入}

基本格式:read  name
将用户输入的内容复制给name
