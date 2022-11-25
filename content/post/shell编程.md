+++
title = "shell编程"
date = 2022-11-25T23:34:00+08:00
lastmod = 2022-11-25T23:34:08+08:00
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


## 构建基本脚本 {#构建基本脚本}


### 创建shell脚本文件 {#创建shell脚本文件}

在shell脚本文件中用'#'表示注释,但第一行却是个例外,一般shell的第一行为

{{< highlight shell >}}
#!/bin/bash
{{< /highlight >}}

这一行告诉shell用哪个shell来运行脚本,当然这里的bash只是一个示例,你也可以用其他的shell.
除第一行外,其他行若以'#'开头则只起注释作用不做任何解释.


### 变量 {#变量}

在shell中若要使用变量则需要在变量名前加'\\('才可以被解释为一个变量,
否则会被解释为普通字符串,若要输出'\\)'则可以使用'\\$'
在shell中变量命名的规则为字母,数字,下划线且长度不能超20个字符,变量名是区分大小写的.

-   变量创建变量创建只需使用变量名后加'='即可赋值也与变量创建方法相同注意:
    若要将一个变量赋值给其他变量,则必须要有'$',否则会被解释为字符串变量名和'='以及值之间不能有空格
    {{< highlight shell >}}
    value1=fsdfd
    value2=$value1
    {{< /highlight >}}


### 命令替换 {#命令替换}

通常来说,若想获得一条命令的输出,可使用反引号'\`'或者'$()',

{{< highlight shell >}}
val=$(echo  3) 或者 val=`ehco 3`
 echo $val
{{< /highlight >}}

上面示例中'val'的只会等于'ehco 3'的值即为3


### 重定向输入输出 {#重定向输入输出}

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


### 管道 {#管道}

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


### 使用数学表达式 {#使用数学表达式}

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


### 退出脚本 {#退出脚本}

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


## 使用结构化命令 {#使用结构化命令}


### if使用 {#if使用}

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


### test命令 {#test命令}

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
    | -d file         | 检测file是否是一个目录 |
    | -w filed        | 检查file是否存在并可写 |
    | -x file         | 检查file是否存在并可执行 |
    | -O file         | 检查file是否存在并属当前用户所有 |
    | -G file         | 检查file是否存在并且默认组与当前用户相同 |
    | file1 -nt file2 | 检查file1是否比file2新 |
    | file1 -ot file2 | 检查file1是否比file2旧 |
-   test逻辑运算

    | 选项                       | 描述                                              |
    |--------------------------|-------------------------------------------------|
    | expression1 -a expression  | 逻辑与，表达式 expression1 和 expression2 都成立，最终的结果才是成立的。 |
    | expression1 -o expression2 | 逻辑或，表达式 expression1 和 expression2 有一个成立，最终的结果就成立。 |
    | !expression                | 逻辑非，对 expression 进行取反。                  |


### 复合条件测试 {#复合条件测试}

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


### if高级特征 {#if高级特征}

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


### case命令 {#case命令}

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


### for语句 {#for语句}

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


### C语言风格的for {#c语言风格的for}

{{< highlight shell >}}
for (( a = 1; a < 10; a++ ))
do
...
done
{{< /highlight >}}


### whle语句 {#whle语句}

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


### unit语句 {#unit语句}

与while语法相同但作用相反,只有测试语句的退出码返回0才会退出until

{{< highlight shell >}}
until   条件
do
...
done
{{< /highlight >}}


### 循环控制 {#循环控制}

-   break
    与C语言的break功能相似,但shell中的break可以指定参数来表示退出的循环的级别,默认是1即退出当前循环
-   continue
    与C语言的continue功能相似,不过它也能指定参数表示要继续执行的循环的级别


## 处理用户输入 {#处理用户输入}


### 参数 {#参数}

在shell脚本中用$0表运行的程序,$1表第一个参数,$2表第二个参数依次类推


### 特殊参数变量 {#特殊参数变量}

-   $#表示传入shell中的参数个数
-   $\*和$@变量可以用来轻松访问所有的参数两者都会将所有传给shell的变量作为它的值
    $\*会将所有传给shell的参数当作整体处理
    $@则可以单独处理即在使用for时,如果将$\*作为list则不会对其中包含的变量进行拆分而$@则可以


### 移动参数 {#移动参数}

shift命令可以实现移动参数,即shfit可以将所有参数(不包括$0)往左移动也就是使用shift后$1的值会等于$2,$2会等于$3,以此类推,而$1初始值会被丢弃


### 获得用户输入 {#获得用户输入}

基本格式:read  name
将用户输入的内容复制给name


## 呈现数据 {#呈现数据}


### 标准文件描述符 {#标准文件描述符}

linux的文件描述符是一个非负整数，可以唯一标识会话中打开的文件。每个进程一次最多可以有九个文件描述符。出于特殊目的，bash shell保留了前三个文件描述符

| 文件描述符 | 所写   | 描述 |
|-------|------|----|
| 0     | STDIN  | 标准输入 |
| 1     | STDOUT | 标准输出 |
| 2     | STDERR | 标准错误 |

-   STDIN
    STDIN文件描述符对应的键盘获得输入，在用户输入时处理每个字符。在使用输入重定向符号（&lt;）时，Linux会用重定向指定的文件来替换标准输入文件描述符例如cat命令在不加任何参数的情况下就会从STDIN接受输入,用户输入一行显示一行但你也可以通过STDIN重定向符号强制cat命令接受来自另一个非STDIN文件的输入。使用'cat &lt; 文件名'
-   STDOUT
    默认情况下STDOUT标准输出就是显示器,而shell所有的所有的输出(包括脚本)都会重定向到STDOUT中,即会输出在显示器我们可以通过'&gt;'强制将其输出到某个文件中
-   STDERR
    shell产生的错误都会重定向到这里,而STDERR默认情况下与STDOUT指向同一位置也就是说shell产生的错误也会输出到显示器


### 重定向错误 {#重定向错误}

-   只重定向错误
    STDERR文件描述符被设成2。可以选择只重定向错误消息，将该文件描述符值放在重定向符号前。该值必须紧紧地放在重定向符号前，否则不会工作。
    {{< highlight shell >}}
    ls  dir  2>  test
    {{< /highlight >}}
    这样如果dir文件存在那么此命令就与'ls dir'效果相同,
    若不存在则错误不会输出的显示器,而会被输出的test文件
-   重定向错误和数据若只用'2&gt;'进行重定向那么对错误重定向,而正确内容则仍会显示在显示器上

    -   示例
        {{< highlight shell >}}
        ls  dir1  dir2   2>  test
        {{< /highlight >}}
        假设dir1存在,而dir2不存在那么dir1目录的内容会正确显示到显示器上而'ls dir2'的错误会输出到test文件

    要实现数据和错误都重定向可以使用'&amp;&gt;'表所有输出都会被重定向或同时使用'1&gt;'和'2&gt;'表重定向输入和输出
    {{< highlight shell >}}
    #表正常输出重定向到test2,错误输出重定向到test1
    ls   dir1   dir2   2>  test1    1>  test2
    #所有输出重定向到test
    ls   dir1     dir2   &>      test
    {{< /highlight >}}


### 脚本重定向输出 {#脚本重定向输出}


#### 临时重定向 {#临时重定向}

在shell脚本中允许使用临时重定向某条命令

-   示例
    {{< highlight nil >}}
    echo  "error message"  >&2
    echo  "normal message"
    {{< /highlight >}}
    在此脚本中'&gt;&amp;2'中2表示STDERR的文件描述符,
    第一条语句表示将该命令重定向到STDERR中由于STDERR和STDOUT的输出都指向显示器,所以在正常执行是看不出差别但如果将该脚本执行的STDERR重定向到某个文件时,那么第一条语句就会显示在文件上,
    而第二条语句则会正常在显示器中显示


#### 永久重定向 {#永久重定向}

在脚本中使用'exec  文件描述符&gt;文件名'可以重定向某个特定的文件描述符

-   示例
    {{< highlight shell >}}
    #将STDERR重定向到test1
    exec   2>test1
    #将STDOUT重定向到test2
    exec   1>test2
    {{< /highlight >}}


### 脚本重定向输入 {#脚本重定向输入}

与重定向输出格式相同'exec 0&lt;filename'
表示从filename中获取输入而非STDIN


### 创建文件描述符 {#创建文件描述符}

使用exec命令也可以用于创建文件描述符

-   示例创建输出文件描述符:exec 文件描述符&gt;filename
    创建输入文件描述符:exec 文件描述符&lt;filename
    文件描述符间进行操作:
    exec 3&gt;&amp;1
    exec 1&gt;filename

其实也可以这样理解当'exec'用于对文件描述符做操作时,若文件描述符存在则该功能为重定向文件描述符,否则为创建文件描述符]


### 关闭文件描述符 {#关闭文件描述符}

若在shell中自己创建了描述符则在shell结束后会自动关闭,
当然也可以提前关闭

-   示例关闭输出文件描述符3:exec 3&gt;&amp;-


### 列出打开的文件描述符 {#列出打开的文件描述符}

lsof命令会列出整个Linux系统打开的所有文件描述符,
但这数量太庞大了,所以lsof有许多过滤参数,例如'-p'指定进程id(pid)
'-d'指定文件文件描述符的编号,'-a'选项可以是指定参数的结果进行and运算

-   示例
    lsof -a -p $$ -d 0,1,2

'\\('为特殊变量shell会将它的值设为当前pid.
其含义为列出文件pid为'\\)'的文件描述符,并且只列出文件描述符为0,1,2的


### 阻止命令输出 {#阻止命令输出}

在Linux系统上null文件的标准位置是/dev/null。你重定向到该位置的任何数据都会被丢掉

-   示例
    ls fsd 2&gt; _/dev/null_

其含义通俗来说就是丢弃错误


## 控制脚本 {#控制脚本}


### linux常见信号 {#linux常见信号}

不同的Linux信号以及Linux如何用这些信号来停止、启动、终止进程。可以通过对脚本进行编程，使其在收到特定信号时执行某些命令，从而控制shell脚本的操作。以下是linux常见信号

| 信号 | 值      | 描述            |
|----|--------|---------------|
| 1  | SIGHUP  | 挂起进程        |
| 2  | SIGINT  | 终止进程        |
| 3  | SIGQUIT | 停止进程        |
| 9  | SIGKILL | 无条件终止进程  |
| 15 | SIGTERM | 尽可能终止进程  |
| 17 | SIGSTOP | 无条件停止进程，但不是终止进程 |
| 18 | SIGTSTP | 停止或暂停进程，但不终止 |
| 19 | SIGCONT | 继续运行停止的进程 |


### 信号生成 {#信号生成}

-   中断进程
    ctrl-c会产生SIGINT
-   停止进程
    ctrl-z会产生SIGTSTP它会停止进程但不会终止,
    进程仍存在内存中,并且可以重新从上一个停止的位置运行


### 捕获信号 {#捕获信号}

trap命令允许你来指定shell脚本要监看并从shell中拦截的Linux信号。如果脚本收到了trap命令列出的信号,该信号将不再有shell处理,而是交由本地出处理格式:trap 命令 信号值

-   示例
    trap ls SIGINT

表示每次中断是都会执行'ls'命令


### 后台运行 {#后台运行}

-   普通后台运行在命令后加'&amp;'即可
-   在非控制台下运行脚本
    nohup command &amp;
    与普通后台运行不同的是即是终端被关闭了该命令仍会执行到结束并且命令中存在的输出也不会输出到终端,而是会在本文件夹新建一个nohup.out的文件作为输出


### 重启停止的作业 {#重启停止的作业}

我们上面讲到可以用ctrl-z暂停进程,
而继续该进程的命令为'bg'或'fg'
bg和fg分别为后台和前台启动,若是后台启动则操作提示符会立即出现,
即你可以立即在该终端做其他事,而前台的话需要等命令结束才行格式:bg(fg) [作业号]


### linux调度 {#linux调度}

调度优先级是个整数值，从-20（最高优先级）到+19（最低优先级）。默认情况下，bash shell
以优先级0来启动所有进程。


#### nice命令 {#nice命令}

用于命令启动时修改优先级

-   格式
    nice -n 优先级别 命令或者
    nice -优先级别  命令

注意普通用户使用nice命令只能减低优先级,不能提高


#### renice命令 {#renice命令}

用于对已经启动的命令修改其优先级

-   格式
    renice -n 优先级别 命令

与nice命令一样普通用户只能降低优先级


## 函数 {#函数}


### 创建函数 {#创建函数}

{{< highlight shell >}}
  function name
  {
  ...
}
  #或者
  name ()
  {
  ...
}
{{< /highlight >}}


### 函数返回值 {#函数返回值}

默认情况下函数返回值为函数的最后一条语句的退出码也可以使用'return'返回特定的值亦或是将函数的输出作为返回值


### 变量作用域 {#变量作用域}

默认情况下,无论实在外部创建变量,亦或是函数内部创建变量都是全局变量只有在变量前加上'local'关键字才会是变量成为局部变量


### 函数与数组 {#函数与数组}


#### 将数组变量传递给函数 {#将数组变量传递给函数}

在shell中如果将整个数组传递给函数,那么函数只会去数组的第一个值,
正确的做法是将数组拆分成单个值在传递给函数

{{< highlight shell >}}
  function test
  {
  fun1()
{
    local newarry=(`echo "$@"`)
    echo "${newarry[*]}"
}

myarray=(1 2 3 4)
fun1 ${myarray[*]}

}
{{< /highlight >}}


### 从函数中返回数组 {#从函数中返回数组}

从函数里向shell脚本传回数组变量也用类似的方法。函数用echo语句来按正确顺序输出单个数组值，然后脚本再将它们重新放进一个新的数组变量中。


### 库文件 {#库文件}

在shell中允许一个脚本使用其他脚本的函数或变量,只需要在脚本中引入即可假设引入的脚本在与当前脚本在同一目录

{{< highlight shell >}}
source  ./脚本
或者
.   ./脚本
{{< /highlight >}}

注意:如果在脚本A中直接执行需要引入的脚本B这样是不能使用脚本B中的函数或变量的因为但你在脚本A中执行引入的脚本B时,shell会为脚本B会创建一个shell来执行该脚本所以使用这种方法执行的脚本A和脚本B不在同一shell环境中,故不能使用脚本B中定义的函数或变量


## 图形化桌面环境中的脚本编程 {#图形化桌面环境中的脚本编程}


### 创建文本菜单 {#创建文本菜单}

使用函数实现简单的菜单功能

{{< highlight shell >}}
#!/bin/bash

#列出菜单选项
function menu
{
    clear
    echo
    echo -e "\t\t\tSys Admin Menu\n"
    echo -e "\t1. Display disk space"
    echo -e "\t2. Display logged on users"
    echo -e "\t3. Display memory usage"
    echo -e "\t0. Exit menu\n\n"
    echo -en "\t\tEnter option: "
    #获取用户输入
    read -n 1  option
}

function diskspace
{
    clear
    df -k
}

function whoseon
{
    clear
    who
}

function menmusage
{
    clear
    cat /proc/meminfo
}

while [ 1 ]
do
menu

case $option in
    0)
        break;;
    1)
        diskspace;;
    2)
        whoseon;;
    3)
        menmusage;;
    *)
        clear
        echo "error";;
esac
echo -en "\n\npress enter to continue"
read -n 1 line
done
clear
{{< /highlight >}}

也可以使用selece命令,这样我们就不用对选项进行排版了,select命令会自动帮我们进行排版

{{< highlight shell >}}
#!/bin/bash

#使用select命令实现创建菜单

function diskspace {
clear
df -k
}
function whoseon {
clear
who
}

function memusage {
clear
cat /proc/meminfo
}


PS3="Enter option: "

select option in "Display disk space" "Display logged on users" "Display memory usage" "Exit program"
do
case $option in
"Exit program")
break ;;
"Display disk space")
diskspace ;;
"Display logged on users")
whoseon ;;
"Display memory usage")
memusage ;;
*)
clear
echo "Sorry, wrong selection";;
esac
done
clear
{{< /highlight >}}


### dialog包 {#dialog包}

在上面我制作了简单的文本菜单,虽然也实现了我们想要的功能,但是太过简陋了因此用开源爱好者开发了dialog包,该包能够用ANSI转义控制字符在文本环境中创建标准的窗口对话框。你可以轻而易举地将这些对话框融入自己的shell脚本中，借此与用户进行交互。

-   dialog包常用部件

    | 部件         | 描述                      |
    |------------|-------------------------|
    | calendar     | 提供选择日期的日历        |
    | checklist    | 显示多个选项（其中每个选项都能打开或关闭） |
    | form         | 构建一个带有标签以及文本字段（可以填写内容）的表单 |
    | fselect      | 提供一个文件选择窗口来浏览选择文件 |
    | gauge        | 显示完成的百分比进度条    |
    | infobox      | 显示一条消息，但不用等待回应 |
    | inputbox     | 提供一个输入文本用的文本表单 |
    | inputmenu    | 提供一个可编辑的菜单      |
    | menu         | 显示可选择的一系列选项    |
    | msgbox       | 显示一条消息，并要求用户选择OK按钮 |
    | pause        | 显示一个进度条来显示暂定期间的状态 |
    | passwordbox  | 显示一个文本框，但会隐藏输入的文本 |
    | passwordform | 显示一个带标签和隐藏文本字段的表单 |
    | radiolist    | 提供一组菜单选项，但只能选择其中一个 |
    | tailbox      | 用tail命令在滚动窗口中显示文件的内容 |
    | tailboxbg    | 跟tailbox一样，但是在后台模式中运行 |
    | textbox      | 在滚动窗口中显示文件的内容 |
    | timebox      | 提供一个选择小时、分钟和秒数的窗口 |
    | yesno        | 提供一条带有Yes和No按钮的简单消息 |

使用方式:
dialog --widget para
其中widget的值可以是上面表格中的任意一个,para则是widget的参数比如说要显示的宽度,高度之类的
dialog每个部件都提供两种输出新式

1.  使用STDERR
2.  使用退出状态码

dialog退出码通常用来确定用户选择的按钮,比如选择yes会返回退出码0
选择no会返回退出码1,通过$?可获得返回的退出码而dialog的部件如果返回了数据则会被发送到STDERR
dialog不仅提供了基础的部件,同时还提供了一些选项使得部件的功能和外观能够更完善

-   dialog选项

    | 选项                     | 描述                              |
    |------------------------|---------------------------------|
    | --add-widget             | 继续下个对话框，直到按下Esc或Cancel按钮 |
    | --aspect ratio           | 指定窗口宽度和高度的宽高比        |
    | --backtitle title        | 指定显示在屏幕顶部背景上的标题    |
    | --begin x y              | 指定窗口左上角的起始位置          |
    | --cancel-label label     | 指定Cancel按钮的替代标签          |
    | --clear                  | 用默认的对话背景色来清空屏幕内容  |
    | --colors                 | 在对话文本中嵌入ANSI色彩编码      |
    | --cr-wrap                | 在对话文本中允许使用换行符并强制换行 |
    | --create-rc file         | 将示例配置文件的内容复制到指定的file文件中① |
    | --defaultno              | 将yes/no对话框的默认答案设为No    |
    | --default-item string    | 设定复选列表、表单或菜单对话中的默认项 |
    | --exit-label label       | 指定Exit按钮的替代标签            |
    | --extra-button           | 在OK按钮和Cancel按钮之间显示一个额外按钮 |
    | --extra-label label      | 指定额外按钮的替代标签            |
    | --help                   | 显示dialog命令的帮助信息          |
    | --help-button            | 在OK按钮和Cancel按钮后显示一个Help按钮 |
    | --help-label label       | 指定Help按钮的替代标签            |
    | --help-status            | 当选定Help按钮后，在帮助信息后写入多选列表、单选列表或表单信息 |
    | --ignore                 | 忽略dialog不能识别的选项          |
    | --input-fd fd            | 指定STDIN之外的另一个文件描述符   |
    | --insecure               | 在password部件中键入内容时显示星号 |
    | --item-help              | 为多选列表、单选列表或菜单中的每个标号在屏幕的底部添加一个帮助栏 |
    | --keep-window            | 不要清除屏幕上显示过的部件        |
    | --max-input size         | 指定输入的最大字符串长度。默认为2048 |
    | --nocancel               | 隐藏Cancel按钮                    |
    | --no-collapse            | 不要将对话文本中的制表符转换成空格 |
    | --no-kill                | 将tailboxbg对话放到后台，并禁止该进程的SIGHUP信号 |
    | --no-label label         | 为No按钮指定替代标签              |
    | --no-shadow              | 不要显示对话窗口的阴影效果        |
    | --ok-label label         | 指定OK按钮的替代标签              |
    | --output-fd fd           | 指定除STDERR之外的另一个输出文件描述符 |
    | --print-maxsize          | 将对话窗口的最大尺寸打印到输出中  |
    | --print-size             | 将每个对话窗口的大小打印到输出中  |
    | --print-version          | 将dialog的版本号打印到输出中      |
    | --separate-output        | 一次一行地输出checklist部件的结果，不使用引号 |
    | --separator string       | 指定用于分隔部件输出的字符串      |
    | --separate-widget string | 指定用于分隔部件输出的字符串      |
    | --shadow                 | 在每个窗口的右下角绘制阴影        |
    | --single-quoted          | 需要时对多选列表的输出采用单引号  |
    | --sleep sec              | 在处理完对话窗口之后延迟指定的秒数 |
    | --stderr                 | 将输出发送到STDERR（默认行为）    |
    | --stdout                 | 将输出发送到STDOUT                |
    | --tab-correct            | 将制表符转换成空格                |
    | --tab-len n              | 指定一个制表符占用的空格数（默认为8） |
    | --timeout sec            | 指定无用户输入时，sec秒后退出并返回错误代码 |
    | --title title            | 指定对话窗口的标题                |
    | --trim                   | 从对话文本中删除前导空格和换行符  |
    | --visit-items            | 修改对话窗口中制表符的停留位置，使其包括选项列表 |
    | --yes-label label        | 为Yes按钮指定替代标签             |


### 其他图形窗口部件包 {#其他图形窗口部件包}

由于dialog包只提供非常基础的功能,并且界面仍不够美观所以各大桌面环境基本上都是使用自己开发的窗口部件包像kde,gnome就分别使用了kdialog和zenity包其实两者都是在dialog的思路上扩展的,相较于dialog可能kdialog和zenity的功能可能会更为强大,且界面更加美观.但使用方式基本上都是相似的


## sed和gawk {#sed和gawk}


### sed介绍 {#sed介绍}

sed编辑器被称作流编辑器(stream editor),流编辑器则会在编辑器处理数据之前基于预先提供的一组规则来编辑数据流。
sed编辑器可以根据命令来处理数据流中的数据，

-   sed编辑器工作方式
    1.  一次从输入中读取一行数据。
    2.  根据所提供的编辑器命令匹配数据。
    3.  按照命令修改流中的数据。
    4.  将新的数据输出到STDOUT。
-   sed使用的基本格式
    sed option script file
-   sed命令选项

    | 选项      | 描述                          |
    |---------|-----------------------------|
    | -e script | 在处理输入时，将script中指定的命令添加到已有的命令中 |
    | -f file   | 在处理输入时，将file中指定的命令添加到已有的命令中 |
    | -n        | 不产生命令输出，使用print命令来完成输出 |

    -   示例
        echo "this is a test" | sed "s/test/aa/"
        或使用 sed "s/test/aa/" 某个文件其中sed后字符串第一个字符's'表示使用sed的's'命令,s命令会用斜线间指定的第二个文本字符串来替换第一个文本字符串模式.
        其含义为将"this is a test"中的'test'替换为'aa'
        注意's'命令只会替换每行第一个出现的字符串如果上述命令是:echo "this is a test test" | sed "s/test/aa/"
        那么输出将会是"this is a aa test"而不是"this is a aa aa"
        使用多个命令:sed -e "s/test/aa/;s/dog/cat" data.txt
        使用文件内容作为命令:sed -f 文件名 data.txt


### gawk介绍 {#gawk介绍}

gawk程序是Unix中的原始awk程序的GNU版本.它提供了一种编程语言而不只是编辑器命令
gawk和sed一样是属于流编辑器,因此gawk每次也只会读取一行内容

-   使用gawk可以实现
    -   定义变量来保存数据；
    -   使用算术和字符串操作符来处理数据；
    -   使用结构化编程概念（比如if-then语句和循环）来为数据处理增加处理逻辑；
    -   通过提取数据文件中的数据元素，将其重新排列或格式化，生成格式化报告。
-   gawk格式
    gawk option program file
    选项

    | 选项         | 描述                |
    |------------|-------------------|
    | -F fs        | 指定行中划分数据字段的字段分隔符 |
    | -f file      | 从指定的文件中读取程序 |
    | -v var=value | 定义gawk程序中的一个变量及其默认值 |
    | -mf N        | 指定要处理的数据文件中的最大字段数 |
    | -mr N        | 指定数据文件中的最大数据行数 |
    | -W keyword   | 指定gawk的兼容模式或警告等级 |
-   gawk中的数据字段变量
    $0代表整个文本行
    $1代表文本行中第一个数据段
    $2代表文本行中第二个数据段
    $n代表文本行中第n个数据段


### sed基础 {#sed基础}


#### 替换标记 {#替换标记}

在前面演示的's'适用于替换文本,同时它还支持替换标记

-   格式
    s/匹配字符/替换的文本/替换标记

不加替换标记默认是替换每行第一个出现的匹配字符串

-   替换标记

    | 替换标记 | 功能                |
    |------|-------------------|
    | 数字   | 表明新文本将替换第几处模式匹配的地方； |
    | g      | 表明新文本将会替换所有匹配的文本； |
    | p      | 表明原先行的内容要打印出来； |
    | w file | 将替换的结果写到文件中。 |
-   地址在sed的操作中允许指定对某行或某些行进行操作,这种操作被称作'行寻址',通常有两种寻址方式
    1.  以数字形式表示行区间
        -   只修改指定行内容
            sed '2s/dog/cat/' data.txt
            只修改第2行内容
        -   使用区间
            sed '2,5s/dog/cat/' data.txt
            修改第2-5行的内容
        -   从某行到结尾
            sed '2,$/dog/cat/' data.txt
            使用$可表示为到文件结尾位置
    2.  用文本模式过滤出行
        sed '__the/s/cat/dog__' data.txt
        表示只修改含'the'的行,其中'the'这个位置也可以使用正则表达式


#### 删除操作 {#删除操作}

前面讲的's'是替换命令,而'd'则是sed的删除命令
sed 'd' data.txt
不加参数会将所以内容删除所以命令'd'一般会与地址一起使用,并且其使用方式与替换的几乎相同需要注意的是当使用了两个文本匹配时,第一个文本匹配代表开启删除模式,
而第二个则代表关闭删除模式例如

{{< highlight text >}}
This is line number 1.
This is line number 2.
This is line number 3.
This is line number 4.
This is line number 1 again.
This is text you want to keep.
This is the last line in the file.
{{< /highlight >}}

当对上面文件进行下面的操作时,其过程是:
第一行有1与命令中的模式相匹配,故开启删除模式,而此时一直到第三行删除模式都未关闭所以前面3行内容都会被删除,到第4行由于删除模式已关闭并且也没有匹配所以第4行内容保留,
到第5行有匹配了此时由于一直到结尾都没有与模式相匹配的行来关闭删除模式故会一直删除知道结尾所以,一下命令输出是:This is line number 4

{{< highlight shell >}}
sed "/1./3/d" data.txt
{{< /highlight >}}


#### 插入和附加操作 {#插入和附加操作}

插入和附加操作命令分别是'i'和'a',插入操作的内容会出现在流的前面,而附加则会出现在流的后面两者也支持寻址插入和附加

-   将数据插入到第3行前面
    sed '3i/插入的数据' data.txt
-   将数据附加到第3行后面
    sed '3a/追加的数据' data.txt


#### 修改操作 {#修改操作}

修改命令是'c',记得与替换区分开来,修改操作是将一整行的内容都有进行修改其使用方式与替换一样,但效果不同例如:sed '2,5cfff' data.txt
其作用是将2-5行的内容修改为fff,而不是2-5行的每行内容都是修改为fff


#### 转换命令 {#转换命令}

转换命令是'y',它是一个处理字符的命令格式:[address]y/inchar/outchar/
inchar到outchar可理解为是映射y命令会将inchar中的字符装换为outchar对应额度字符即如果某个字符出现在inchar第一个位置,那么它也会被替换为outchar的第一个位置


#### 打印命令 {#打印命令}

<!--list-separator-->

-  打印行命令

    与替换标记一样,打印命令也是'p'
    格式:[address]p

<!--list-separator-->

-  打印行号

    格式:[address]=

<!--list-separator-->

-  列出行

    格式:[address]l


#### sed文件处理 {#sed文件处理}

<!--list-separator-->

-  写入文件

    前面我们所有的操作如果不加写入操作的话,其实都不会对文件本身产生任何影响格式:[address]w filename

<!--list-separator-->

-  读取文件

    读取命令'r'允许你将一个独立文件中的数据插入到数据流中。格式:[address]r filename
    例如:sed '3r insertfile' filename
    此命令会将insertfile中的内容插入到filename的第3行后面


## sed进阶 {#sed进阶}


### next命令 {#next命令}


#### 单行版本next {#单行版本next}

及小写n命令,小写的n命令会告诉sed编辑器移动到数据流中的下一文本行，而不用重新回到命令的最开始例如

{{< highlight text >}}
test

aaa

bbb
ccc

ddd
{{< /highlight >}}

若此时我们想删除test下一行的空行,而保留其他空行我们此时可以使用n命令
sed '/test/{n;d}' data.txt
此命令的执行流程为首先找到test所在的行后n,会先执行n命令将将数据流移动到下一行即我们想删除的空行然后在做d命令将整行删除,而由于之后没有与test相匹配的文本所以该命令只删除了test后的空行


#### 合并文本行 {#合并文本行}

即大写的N命令,多行版本的next命令（用大写N）会将下一文那上面当行版本的例子来说,如果我们执行以下命令
sed '/test/{N;d}' data.txt
那么此时test行也会被删除,因为此命令是会将test及test下一行的内容当作整体处理注意当作整体处理,所以test和其下一行其实还是有换行符'\n'存在的


### 模式空间 {#模式空间}

是一块活跃的缓冲区,在我们使用sed命令读取每行文本时,该文本就是放到模式空间的


### 保持空间 {#保持空间}

sed编辑器的另一个缓冲区,通常用来保存模式空间的一些临时东西

-   对保持空间的操作

    | 命令 | 描述           |
    |----|--------------|
    | h  | 将模式空间复制到保持空间 |
    | H  | 将模式空间附加到保持空间 |
    | g  | 将保持空间复制到模式空间 |
    | G  | 将保持空间附加到模式空间 |
    | x  | 交换模式空间和保持空间的内容 |


### 排除命令 {#排除命令}

排除命令(!),可理解为编程语言中的非操作,在sed中即是执行原来操作相反的操作举例来说
sed -n '/aaa/p' data.txt
其含义为打印行中包括'aaa'的行而若加上排除命令
sed -n '__aaa__!p' data.txt
其含义就变成了打印不包含'aaa'行的行


### 分支 {#分支}

排除命令可支持对命令进行排除,sed也提供了基于地址进行排除的方式我们称作分支(branch)
格式:[address]b [label]
address参数决定了哪些行的数据会触发分支命令。label参数定义了要跳转到的位置,如果没有默认跳转到末尾示例1:sed '{2,5b;s/aaa/bbb/}' data.txt
其作用是将每行出现的第一个'aaa'替换为'bbb'但不包括2-5行示例2:sed '{__first/b jump1;s/aaa/bbb__;:jump1;s/aaa/ccc/}' data.txt
其作用是遇到与'first'匹配的行则跳到:jump1出执行接着执行命令,否则正常顺序执行


### 测试命令 {#测试命令}

与分支命令类似,但测试命令(t)是依据替换结果进行调整的,
如果我们已经做了一次替换而不需要在做另一个替换是就可以使用测试命令例如:sed '{s/aaa/bbb/;t;s/ccc/ddd/}' data.txt
以上含义即为若我们匹配到aaa则将其替换为bbb,继续向下执行t时,t检测到已经进行过一次替换了
t就会是命令跳转,由于此时t没有指定标签,那么就会默认跳转到末尾,而若第一条命令没有发生替换那么t就不会跳转,而继续向下执行


### &amp;符号 {#and-符号}

在sed的替换命令中'&amp;'符号可以代替被替换的文本例如:sed 's/aaa/"&amp;"/ data.txt
其含义为将aaa提换为"aaa",这在一般情况下也许毫无意义因为我们完全可以使用
sed 's/aaa/"aaa"/'替代,但如果要被替换的内容是一个正则表达式,这就显得很有意义了


## gawk进阶 {#gawk进阶}


### 内置变量 {#内置变量}

| 变量        | 描述                          |
|-----------|-----------------------------|
| FIELDWIDTHS | 由空格分隔的一列数字，定义了每个数据字段确切宽度 |
| FS          | 输入字段分隔符                |
| RS          | 输入记录分隔符                |
| OFS         | 输出字段分隔符                |
| ORS         | 输出记录分隔符                |
| ARGC        | 当前命令行参数个数            |
| ARGIND      | 当前文件在ARGV中的位置        |
| ARGV        | 包含命令行参数的数组          |
| CONVFMT     | 数字的转换格式（参见printf语句），默认值为%.6 g |
| ENVIRON     | 当前shell环境变量及其值组成的关联数组 |
| ERRNO       | 当读取或关闭输入文件发生错误时的系统错误号 |
| FILENAME    | 用作gawk输入数据的数据文件的文件名 |
| FNR         | 当前数据文件中的数据行数      |
| IGNORECASE  | 设成非零值时，忽略gawk命令中出现的字符串的字符大小写 |
| NF          | 数据文件中的字段总数          |
| NR          | 已处理的输入记录数            |
| OFMT        | 数字的输出格式，默认值为%.6 g |
| RLENGTH     | 由match函数所匹配的子字符串的长度 |
| RSTART      | 由match函数所匹配的子字符串的起始位置 |


### 自定义变量 {#自定义变量}

gawk中的自定义变量与shell中的赋值语句相同示例:gawk 'BEGIN{var="aaa";print var}'
同时gawk也直接在命令行中定义变量


### 数组 {#数组}


#### 定义数组 {#定义数组}

定义数组的方式与定义变量类似格式:var[index]=element
'var'为数组名,index关联数组的索引值,这个与我们常见编程语言的数组不太一样常见编程语言的index值是数字,而在这里却不仅可以是数字还可以是字符串,'element'为元素

-   例如
    capital["Illinois"] = "Springfield"
    capital["Indiana"] = "Indianapolis"
    capital["Ohio"] = "Columbus"


#### 遍历数组 {#遍历数组}

遍历数组可以使用一种特殊形式的for

{{< highlight text >}}
$ gawk 'BEGIN{
> var["a"] = 1
> var["g"] = 2
> var["m"] = 3
> var["u"] = 4
> for (test in var)
> {
>
print "Index:",test," - Value:",var[test]
> }
> }'
Index: u - Value: 4
Index: m - Value: 3
Index: a - Value: 1
Index: g - Value: 2
$
{{< /highlight >}}


#### 删除数组变量 {#删除数组变量}

删除数组索引即可删除给数组索引的值格式:delete array[index]


### 通配符 {#通配符}

通配符'~'可以指定匹配操作符、数据字段变量以及要匹配的正则表达式。例如:gawk 'BEGIN{FS=","} $2 ~ /^aa/{print $0}' data.txt
此命令中$2表示第二个字段,而使用了通配符后就此命令就表示匹配第二个字段以'aa'开头的文本


### 数学表达式 {#数学表达式}

在gawk中使用数学表达式进行过滤其使用方式与常见编程语言类似,直接指明要匹配的字段和匹配方式即可例如:gawk -F: '$4==0{print $1}' /etc/passwd
表示打印第4个字段等于0的文本的第一个字段

-   常用数学表达式

    | 表达式    | 描述     |
    |--------|--------|
    | x==y      | 值x等于y。 |
    | x &lt;= y | 值x小于等于y。 |
    | x &lt; y  | 值x小于y。 |
    | x &gt;= y | 值x大于等于y。 |
    | x &gt; y  | 值x大于y。 |


### 结构化命令 {#结构化命令}


#### if语句 {#if语句}

格式:if (condition) statement1
if (condition) statement1; else statement2


#### while语句 {#while语句}

格式:
while (condition)
{
statements
}


#### do-while语句 {#do-while语句}

格式:
do
{
statements
} while (condition)


#### for语句 {#for语句}

与c语言的for类似格式:for( variable assignment; condition; iteration process)

{{< highlight text >}}
$ gawk '{
> total = 0
> for (i = 1; i < 4; i++)
> {
>
total += $i
> }
> avg = total / 3
> print "Average:",avg
> }' data5
Average: 128.333
Average: 137.667
Average: 176.667
$
{{< /highlight >}}


### 格式化输出 {#格式化输出}

gawk的格式化输出与C语言类似格式:printf "format string", var1, var2 . . .

-   格式化输出的控制字母

    | 控制字母 | 描述                  |
    |------|---------------------|
    | c    | 将一个数作为ASCII字符显示 |
    | d    | 显示一个整数值        |
    | i    | 显示一个整数值（跟d一样） |
    | e    | 用科学计数法显示一个数 |
    | f    | 显示一个浮点值        |
    | g    | 用科学计数法或浮点数显示（选择较短的格式） |
    | o    | 显示一个八进制值      |
    | s    | 显示一个文本字符串    |
    | x    | 显示一个十六进制值    |
    | X    | 显示一个十六进制值，但用大写字母A~F |

<!--listend-->

{{< highlight text >}}
$ gawk 'BEGIN{
> x = 10 * 100
> printf "The answer is: %e\n", x
> }'
The answer is: 1.000000e+03
$
{{< /highlight >}}

除了控制字母外，还有3种修饰符可以用来进一步控制输出。

-   width
    指定了输出字段最小宽度的数字值。如果输出短于这个值，printf会将文本右对齐，并用空格进行填充。如果输出比指定的宽度还要长，则按照实际的长度输出。
-   prec
    这是一个数字值，指定了浮点数中小数点后面位数，或者文本字符串中显示的最大字符数。
-   -（减号）指明在向格式化空间中放入数据时采用左对齐而不是右对齐。

<!--listend-->

{{< highlight text >}}
$ gawk 'BEGIN{FS="\n"; RS=""} {printf "%16s %s\n",$1,$4}' data.txt
  Riley Mullen (312)555-1234
Frank Williams (317)555-9876
   Haley Snell (313)555-4938
$
%s\n", $1, $4}' data2
{{< /highlight >}}

其中的16就表示第一个输出的字段的宽度为16,从输出中也不难看出,第一行和第三行的输出由于第一个字段的字符不满16个则会用空格补齐而这样不够美观所以我们可以修改它的对齐方式

{{< highlight text >}}
$ gawk 'BEGIN{FS="\n"; RS=""} {printf "%-16s %s\n",$1,$4}' data.txt
Riley Mullen (312)555-1234
Frank Williams (317)555-9876
Haley Snell (313)555-4938
$
%s\n", $1, $4}' data2
{{< /highlight >}}


### 内置函数 {#内置函数}


#### 数学函数 {#数学函数}

| 函数        | 描述              |
|-----------|-----------------|
| atan2(x, y) | x/y的反正切，x和y以弧度为单位 |
| cos(x)      | x的余弦，x以弧度为单位 |
| exp(x)      | x的指数函数       |
| int(x)      | x的整数部分，取靠近零一侧的值 |
| log(x)      | x的自然对数       |
| rand( )     | 比0大比1小的随机浮点值 |
| sin(x)      | x的正弦，x以弧度为单位 |
| sqrt(x)     | x的平方根         |
| srand(x)    | 为计算随机数指定一个种子值 |


#### 按位操作数据的函数 {#按位操作数据的函数}

| 函数               | 描述             |
|------------------|----------------|
| and(v1, v2)        | 执行值v1和v2的按位与运算。 |
| compl(val)         | 执行val的补运算。 |
| lshift(val, count) | 将值val左移count位。 |
| or(v1, v2)         | 执行值v1和v2的按位或运算。 |
| rshift(val, count) | 将值val右移count位。 |
| xor(v1, v2)        | 执行值v1和v2的按位异或运算。 |


#### 字符串函数 {#字符串函数}

| 函数                      | 描述                                                   |
|-------------------------|------------------------------------------------------|
| asort(s [,d])             | 将数组s按数据元素值排序。索引值会被替换成表示新的排序顺序的连续数字。 |
|                           | 另外，如果指定了d，则排序后的数组会存储在数组d中       |
| asorti(s [,d])            | 将数组s按索引值排序。生成的数组会将索引值作为数据元素值， |
|                           | 用连续数字索引来表明排序顺序。另外如果指定了d，排序后的数组会存储在数组d中 |
| gensub(r, s, h [, t])     | 查找变量$0或目标字符串t（如果提供了的话）来匹配正则表达式r。 |
|                           | 如果h是一个以g或G开头的字符串，就用s替换掉匹配的文本。如果h是一个数字，它表示要替换掉第h处r匹配的地方 |
| gsub(r, s [,t])           | 查找变量$0或目标字符串t（如果提供了的话）来匹配正则表达式r。如果找到了，就全部替换成字符串s |
| index(s, t)               | 返回字符串t在字符串s中的索引值，如果没找到的话返回0    |
| length([s])               | 返回字符串s的长度；如果没有指定的话，返回$0的长度      |
| match(s, r [,a])          | 返回字符串s中正则表达式r出现位置的索引。如果指定了数组a，它会存储s中匹配正则表达式的那部分 |
| split(s, a [,r])          | 将s用FS字符或正则表达式r（如果指定了的话）分开放到数组a中。返回字段的总数 |
| sprintf(format,variables) | 用提供的format和variables返回一个类似于printf输出的字符串 |
| sub(r, s [,t])            | 在变量$0或目标字符串t中查找正则表达式r的匹配。如果找到了，就用字符串s替换掉第一处匹配 |
| substr(s, i [,n])         | 返回s中从索引值i开始的n个字符组成的子字符串。如果未提供n，则返回s剩下的部分 |
| tolower(s)                | 将s中的所有字符转换成小写                              |
| toupper(s)                | 将s中的所有字符转换成大写                              |


#### 时间函数 {#时间函数}

| 函数                         | 描述                                                    |
|----------------------------|-------------------------------------------------------|
| mktime(datespec)             | 将一个按YYYY MM DD HH MM SS [DST]格式指定的日期转换成时间戳值① |
| strftime(format[,timestamp]) | 将当前时间的时间戳或timestamp（如果提供了的话）转化格式化日期（采用shell函数date()的格式） |
| systime( )                   | 返回当前时间的时间戳                                    |


### 自定义函数 {#自定义函数}

-   定义函数格式:
    {{< highlight text >}}
      function name([variables])
    {
    statements
    [return value]
    }
    {{< /highlight >}}
-   使用函数使用函数的方式与使用内置函数的方式并无产别,只不过我们在使用自定义函数前需要先进行定义
    {{< highlight text >}}
    $ gawk '
    > function myprint()
    > {
    >   printf "%-16s - %s\n", $1, $4
    > }
    > BEGIN{FS="\n"; RS=""}
    > {
    >
    myprint()
    > }' data2
    Riley Mullen
    - (312)555-1234
    Frank Williams
    - (317)555-9876
    Haley Snell
    - (313)555-4938
    $
    {{< /highlight >}}

-创建函数库与其他编程语言一样函数库说到底就是个文件,所以我们若想写一个函数库并调用它,
  只需要在执行gawk命令时像引进文件一样引进函数库即可例如:gawk -f 函数库 -f gawk命令文件  data.txt
