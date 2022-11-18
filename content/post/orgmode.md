+++
title = "orgmode"
date = 2022-11-18T22:28:00+08:00
lastmod = 2022-11-18T22:28:59+08:00
draft = false
author = "geezer"
+++

## 文档结构 {#文档结构}


### 列表 {#列表}

-   无序列表
    -   使用"\*"号的列表由于org中\*号可以有两种含义,即标题和列表.所以若要用\*号表示列表的话,\*号不能位于行首否则就成标题了.
        示例:
        -   列表1
        -   列表2
        -   列表3
    -   使用"+"号或"-"号的列表与\*号用法基本一致,唯一不同的是可以位于行首
-   有序列表有序列表使用"1."或者"1)"开头示例:
    1.  第一
        1.  fsdaf
        2.  fdsja
    2.  第二
    3.  第三
-   结束列表可以在列表下空两行,结束该列表,或者将内容的缩进修改成小于或等于该列表也可以结束列表.
-   常用操作与快捷键
    -   &lt;tab&gt;
        对列表进行收缩或展开
    -   M-RET
        在该列表下方新建一个与该列表同级的列表.
        注意:如果光标位于列表名称的中将的话,则会将该列表后的内容作为新列表.
    -   M-S-RET
        在该列表下方新建一个与该列表同级并且带有复选框的列表.其他与M-RET功能相似.
    -   S-UP和S-DOWN
    -   M-UP和M-DOWN
        移动列表,将列表上移或者下移.移动时会连其下的内容也会随之移动.
    -   M-LEFT和M-RIGHT
        提升或者降低列表的级别.不会连同子列表一起移动.
    -   M-S-LEFT和M-S-RIGHT
        提升或者降低列表的级别.会连同子列表一起移动
    -   C-c C-c
        与复选框相关的操作
    -   C-c -
        修改与当前列表同级的表示方式,使它们的表示方式在"-,+,\*,1.,1)"中循环切换.如果有选中区域的话则只对该区域内的列表有效.
    -   C-c \*
        将当前列表修改为标题,使其下与其同级别的列表成为它的子项
    -   C-c C-\*
        将当前列表级别一致的列表修改为缩进一致的标题.
    -   S-LEFT和S-RIGHT
        修改列表与当前列表同级别的表示方式,在"-,+,\*,1.,1)"切换.
        注意:使用此操作是需要将光标移动到表示列表的符号上.
    -   C-c ^
        对列表进行排序,可以按数字,字母,时间或者自定义函数进行排序.


### drawer {#drawer}

-   简介
    drawer里可以保存一些与内容相关的东西,而你又不希望显示他们,就可以使用drawer,这个有点类似于标签的功能.
-   示例

    drawer的内容
-   常用操作与快捷键
    -   插入一个drawer
        C-c C-x d
    -   插入一个带有时间的drawer
        C-c C-z


### block {#block}

-   简介
    block通常用于存放代码块和一些示例,以"#+BEGIN"开头,中间写入内容,以"#+END"结尾.
-   示例
    -   引用块

        > 引用内容
    -   示例块
        {{< highlight text >}}
        示例
        {{< /highlight >}}
    -   代码块
        {{< highlight c >}}
        int main()
          {
            int a=;
            float d=0;
            return 0;
            }
        {{< /highlight >}}


## 表格 {#表格}


### 创建表格 {#创建表格}

-   表格表示如果某行是以'|'开头并且非空的花一般会被视为是一个表格,而以'|-'开头则会被视为是一个表格的水平线.
-   示例:

    |   | 1 | 2 | 3 |
    |---|---|---|---|
    | 1 |   |   |   |
    | 2 |   |   |   |
    | 3 |   |   |   |
-   快捷键
    -   &lt;tab&gt;
        1.  输入"|"时如果是首字母,按tab会自动生成表格
        2.  输入"|-"时如果是首字母,按tab会自动生成表格的水平线
        3.  当光标在表格中输入tab会将往右移至下一个表格,如果表格已经是最后一个则会移动下一行第一个,如果下一行没有内容则会先在下一行新建一列.
    -   S-&lt;tab&gt;
        与&lt;tab&gt;相反,不过只包含移动操作.
    -   M-a
        将光标定位的当前格子的最前面,如果光标所在格子没有内容者与&lt;tab&gt;功能相同
    -   M-a
        将光标定位的当前格子的最后,如果光标所在格子没有内容者与S-&lt;tab&gt;功能相同
    -   RET
        上下移动表格,如果表格已经是最后一行则会在下一行新建一行比表格后载移动下一行第一个
    -   C-c | (org-table-create-or-convert-from-region)
        该命令输入后会提示用户输入表格的列x行,输入后会在该区域创建一个这样的表格.如果当前选中的区域是以逗号或者空格分开的,则该命令会按此自动生成表格.
    -   C-c C-c
        是当前表格对齐
    -   org-table-blank-field
        清空当前光标所处位置的格子中的内容
    -   M-LEFT (org-table-move-column-left)和M-RIGHT (org-table-move-column-right)
        将当前列向指定方向移动
    -   M-S-LEFT (org-table-delete-column)
        将当前列删除
    -   M-S-RIGHT (org-table-insert-column)
        在当前列的前面新建一列
    -   M-UP (org-table-move-row-up)和M-DOWN (org-table-move-row-down)
        将当前行向指定方向移动.
    -   M-S-UP (org-table-kill-row)
        将当前行删除
    -   M-S-DOWN (org-table-insert-row)
        在当前行上面新建一行.
    -   对单个格子移动
        -   S-UP (org-table-move-cell-up)
            将当前格子向上移动
        -   S-DOWN (org-table-move-cell-down)
            将当前格子向下移动
        -   S-LEFT (org-table-move-cell-left)
            将当前格子向左移动
        -   S-RIGHT (org-table-move-cell-right)
            将当前格子向右移动
    -   C-c - (org-table-insert-hline)
        在当前行下面插入表格的水平线
    -   C-c RET (org-table-hline-and-move)
        与C-c -功能一致,不过会移动至新建的水平线下一行表格
    -   C-c ^ (org-table-sort-lines)
        对表格进行排序,排序的选择对象为光标所在的列.
    -   C-c \` (org-table-edit-field)
        新开一个buffer对当前格子的内容进行编辑
    -   M-x org-table-transpose-table-at-point
        将光标所在的表格进行行列互换
    -   C-c TAB (org-table-toggle-column-width)
        收缩或者扩张


## 超链接 {#超链接}


### 创建超链接 {#创建超链接}

示例:

{{< highlight text >}}
[[链接][描述]]
或
[[链接]]
{{< /highlight >}}


### 链接类型 {#链接类型}

-   内部链接
    -   使用id
        跳转到指定id位置
        {{< highlight text >}}
        [#custom_id]
        {{< /highlight >}}
    -   使用章节跳转到指定章节
        {{< highlight text >}}
        [*section]
        或
        [section]
        {{< /highlight >}}
    -   定义标记跳转到自定义的标记,使用'<span class="org-target" id="org-target--target"></span>'自定义一个标记
        {{< highlight text >}}
        <<target>>
        [target]
        {{< /highlight >}}
-   外部链接可以打开网址,文件等内容,一般都有自己的格式开头,比如网址以http或https开头,文件以file开头
    {{< highlight text >}}
    [[https://www.baidu.com]]
    [[file:/home/cl/]]
    {{< /highlight >}}


## todo-item {#todo-item}


### todo的使用 {#todo的使用}

todo跟标题的使用方式几乎一样,在标题前加上TODO(大写)即可是标题成为一个TODO列表

{{< highlight text >}}
*** TODO 开会
{{< /highlight >}}


#### 常用的快捷键和命令 {#常用的快捷键和命令}

| 快捷键         | 命令                    | 作用                                   |
|-------------|-----------------------|--------------------------------------|
| C-c C-t        | org-todo                | 切换todo的状态                         |
| S-right和S-left |                         | 循环切换todo状态                       |
| C-c / t        | org-show-todo-tree      | 将文档中所以的todo项以稀疏树的形式展现出来 |
| C-c a t        | org-adenda t            | 显示全局 TODO 列表。从所有的议程文件中收集 TODO 项到一个缓冲区中 |
| S-M-RET        | org-insert-todo-heading | 插入一个todo列表                       |


### 完成进度 {#完成进度}

可以在标题或者todo列表后加上'<code>[/]</code>'或者'<code>[%]</code>'来其子项todo的完成进度或者复选框的选择比例,按'C-c C-c'可以刷新其状态


#### 示例  <code>[2/4]</code> <code>[50%]</code> {#示例}

<!--list-separator-->

- <span class="org-todo done DONE">DONE</span>  111

<!--list-separator-->

- <span class="org-todo todo TODO">TODO</span>  222

<!--list-separator-->

- <span class="org-todo todo TODO">TODO</span>  333

<!--list-separator-->

- <span class="org-todo done DONE">DONE</span>  444


### 复选框 {#复选框}

复选框是特殊的列表,在列表的名称前加上'[ ]'即是一个复选框例如:


#### <span class="org-todo todo TODO">TODO</span> today arrange <code>[0/3]</code> <code>[0%]</code> {#today-arrange}

-   [-] call someone <code>[1/3]</code>
    -   [ ] peter
    -   [ ] lasy
    -   [X] jonh
-   [-] order food <code>[2/3]</code>
    -   [X] milk
    -   [ ] bread
    -   [X] dumpling
-   [-] listen music <code>[33%]</code>
    -   [ ] jay chou
    -   [X] justin biber
    -   [ ] mike jackson


#### 操作与快捷键 {#操作与快捷键}

-   C-c C-c和C-c C-x C-b(org-toggle-checkbox)
    触发复选框的状态
-   C-c C-x C-r (org-toggle-radio-button)
    是得同级的复选框只能选中一个或不选,执行此命令会是同级复选框都处于非选中状态,如果当前复选框是选中的则取消选中,否则选中.
-   M-S-RET (org-insert-todo-heading)
    插入一个新的复选框


## 标签 {#标签}

标签是对标题的说明,可以在标题后使用并且一个标题可以含有多个标签,标签的前后都必须有一个冒号


### 标签的继承 {#标签的继承}

标签具有继承性,一个标题如果具有一个或多个标签,那么其下的子项也会继承这些标签.

{{< highlight text >}}
* Meeting with the French group      :work:
** Summary by Frank                  :boss:notes:
*** TODO Prepare slides for him      :action:
{{< /highlight >}}

在以上示例中,最后一个todo标题虽然只有一个标签,但其实它会继承其父标签,所以该标题含有work,boss,notes,action四个标签也可以设置一个标签让所有的标签继承,比如

{{< highlight text >}}
#+FILETAGS: :Peter:Boss:Secret:
{{< /highlight >}}

这样设置就好像在第0级的标题设置标签一样,所有标题都会继承该标题的标签


#### 标签操作和快捷键 {#标签操作和快捷键}

-   C-c C-q (org-set-tags-command)
    插入一个标签
-   C-c C-c
    与上面功能相同


## 属性 {#属性}
