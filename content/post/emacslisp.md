+++
title = "emacslisp"
date = 2022-11-18T22:28:00+08:00
lastmod = 2022-11-18T22:28:27+08:00
draft = false
author = "geezer"
+++

## emacs tutorial {#emacs-tutorial}


## emacs init {#emacs-init}


## emacs keys {#emacs-keys}

介绍一些emacs的快捷键,以及如何定义快捷键


### 定义快捷键 {#定义快捷键}

-   定义全局快捷键将crtl-b绑定到whitespace-mode上
    {{< highlight emacs-lisp >}}
    (global-set-key (kbd "C-b") 'whitespace-mode)
    {{< /highlight >}}
-   定义局部快捷键局部快捷键只对当前buffer有效,一旦离开当前buffer则会失效
    {{< highlight emacs-lisp >}}
    (local-set-key (kbd "C-b") 'whitespace-mode)
    {{< /highlight >}}
-   取消某个快捷键
    {{< highlight emacs-lisp >}}
    (global-set-key (kbd "C-b") nil)
    {{< /highlight >}}


### 定义快捷键的语法 {#定义快捷键的语法}

-   单个修饰键和单个特殊键
    {{< highlight emacs-lisp >}}
    (global-set-key (kbd "M-a") 'backward-char) ; Alt+a
    (global-set-key (kbd "C-a") 'backward-char) ; Ctrl+a
    (global-set-key (kbd "<f3>")  'backward-char) ; F3 key
    {{< /highlight >}}
-   定义带前缀的快捷键当定义某个带前缀的快捷键时需要先取消前缀快捷键的绑定.
    {{< highlight emacs-lisp >}}
    (global-set-key (kbd "<f7>") nil) ; good idea to put nil to the starting key
    (global-set-key (kbd "<f7> <f8>") 'calendar)

    (global-set-key (kbd "C-e") nil) ; good idea to put nil to the starting key
    (global-set-key (kbd "C-e a") 'calendar) ; Ctrl+e a
    (global-set-key (kbd "C-e SPC") 'calendar) ; Ctrl+e Space
    {{< /highlight >}}


### 在major-mode中添加快捷键 {#在major-mode中添加快捷键}

-   普通方法要在某个major-mode上添加快捷键使其只作用于该major-mode,
    其主要原理是在该major-mode上添加hook,使得在开启该major-mode
    后调用我们自定义的函数,而自定义的函数中我们可以定义一个局部快捷键这样就可以时的该快捷键只对该major-mode有效
    {{< highlight emacs-lisp >}}
    (when (fboundp 'go-mode)
      (defun my-go-config ()
        "为go-mode添加快捷键"
        (local-set-key (kbd "C-b") 'gocf)
        ;;其他配置)
      (add-hook 'go-mode-hook 'my-go-config))
    {{< /highlight >}}
-   直接修改mode的键位映射如果我们知道某个major-mode的键位映射变量名,则可以直接通过变量名修改
    {{< highlight emacs-lisp >}}
    (progn
    ;; modify dired keys
      (require 'dired )
      (define-key dired-mode-map (kbd "o") 'other-window)
      (define-key dired-mode-map (kbd "2") 'delete-window)
      (define-key dired-mode-map (kbd "3") 'delete-other-windows)
      (define-key dired-mode-map (kbd "4") 'split-window-below)
      (define-key dired-mode-map (kbd "C-o") 'find-file))
    {{< /highlight >}}


### 在minor-mode中添加快捷键 {#在minor-mode中添加快捷键}

minor-mode的键位映射变量名,一般都是'minor-mode名-mode-map',所以要为某个
minor-mode添加快捷键可以

{{< highlight emacs-lisp >}}
(progn
;; change isearch's keys to arrows
(define-key isearch-mode-map (kbd "<up>") 'isearch-ring-retreat )
(define-key isearch-mode-map (kbd "<down>") 'isearch-ring-advance )

(define-key isearch-mode-map (kbd "<left>") 'isearch-repeat-backward)
(define-key isearch-mode-map (kbd "<right>") 'isearch-repeat-forward)
)
{{< /highlight >}}


### minor-mode的快捷键优先级 {#minor-mode的快捷键优先级}

minor-mode的快捷键优先级存放在 **minor-mode-map-alist**
它是一个关联列表,可通过修改该变量来修改minor-mode优先级


### 定义快捷键前缀 {#定义快捷键前缀}

{{< highlight elisp >}}
;;定义快捷键前缀命令
(define-prefix-command 'geezer-prefix)
;;将以上命令绑定到某个快捷键上
(global-set-keys (kbd "C-z") 'geezer-prefix)
;;经过以上两步就可以是的'C-z'成为一个前缀按键
;;之后只需要在该前缀按键添加即可
(define-key my-keymap (kbd "<f6>") 'visual-line-mode)
{{< /highlight >}}


### 同一个命令在不同major-mode的不同作用 {#同一个命令在不同major-mode的不同作用}

要是同一个命令在不同major-mode中的作用不同,最简单的方法就是封装该命令,
再在命令中判断当前major-mode
假设我们在x1-mode时执行该命令会调用x1-cmd
在x2-mode时,为x2-cmd

{{< highlight elisp >}}
;;定义x1-cmd函数和x2-cmd函数
(defun x1-cmd () ())
(defun x2-cmd () ())

(defun geezer-smart-cmd ()
  (interactive)
  (cond
   ((string-equal major-mode "x1-mode") (x1-cmd))
   ((string-equal major-mode "x2-mode") (x2-cmd))
   (t nil)
  ))
{{< /highlight >}}


### 定义可临时重复使用的快捷键 {#定义可临时重复使用的快捷键}

在emacs有这样的操作,调用某个命令后就可以重复按住指定键来重复执行该命令,而且这个操作是可以中断的,中断后之前的按键就会失效.这就好比在浏览器中我们可以按住ctrl键,然后滚动鼠标就能实现放大缩小,而松开ctrl键后就没有这种效果了.
在emacs也有这样的操作,比如 **text-scale-adjust** 调用该命令后,允许按 **+** 键放大,按 **-**
键缩小等操作,而结束该命令后就会失去该效果下面我们来实现下这个操作

{{< highlight elisp >}}
(defun geezer-forward-word ()
  "移动光标"
  (interactive)
  (progn
    (forward-char)
    (set-transient-map
     (let (($kmap (make-sparse-keymap)))
       (define-key $kmap (kbd "r") 'geezer-forward-word)
       (define-key $kmap (kbd "l") 'geezer-backward-word)
       $kmap
       )
     )
    )
  )
(defun geezer-backward-word ()
  "移动光标"
  (interactive)
  (progn
    (forward-char)
    (set-transient-map
     (let (($kmap (make-sparse-keymap)))
       (define-key $kmap (kbd "r") 'geezer-forward-word)
       (define-key $kmap (kbd "l") 'geezer-backward-word)
       $kmap
       )
     )
    )
  )
{{< /highlight >}}

以上代码中 **set-transient-map** 接受一个keymap,并且使用该keymap一次,并且该keymap的优先级会高于其他的minor-mode
而let中的代码,则返回一个 keymap,使用该keymap后按r或者l都会执行一个递归函数以此实现重复调用,这样在调用该函数后,可以重复使用r和l实现对应的操作.按C-g可结束也可以自定义一个键位结束,结束后r和l的功能会还原,


### 在emacs中输入表情,及其他unicode符号 {#在emacs中输入表情-及其他unicode符号}

**key-translation-map** 是emacs自带的一个keymap,在任何buffer都有效所以我们可以向里面添加map实现输入表情的效果

{{< highlight elisp >}}
(define-key key-translation-map (kbd "<f8>") (kbd "•"))
     ;; set keys to insert math symbol
(define-key key-translation-map (kbd "<f9> p") (kbd "φ"))
(define-key key-translation-map (kbd "<f9> x") (kbd "ξ"))
(define-key key-translation-map (kbd "<f9> i") (kbd "∞"))
(define-key key-translation-map (kbd "<f9> <right>") (kbd "→"))

;; set keys to insert emoji
(define-key key-translation-map (kbd "<f9> 1") (kbd "😅"))
(define-key key-translation-map (kbd "<f9> 2") (kbd "❤"))
{{< /highlight >}}

这里不推荐使用 **global-set-key** 的方式,就像下面这种

{{< highlight elisp >}}
(global-set-key (kbd "<f8>") (lambda () (interactive) (insert "→")))
{{< /highlight >}}

这种方法的确可行,但在使用isearch时会失效


### 交换键盘按键 {#交换键盘按键}

交换f11和f12

{{< highlight emacs-lisp >}}
(define-key key-translation-map (kbd "<f11>") (kbd "<f12>"))
(define-key key-translation-map (kbd "<f12>") (kbd "<f11>"))
{{< /highlight >}}


### 鼠标操作 {#鼠标操作}

-   取消鼠标的加速
    {{< highlight elisp >}}
    (setq mouse-wheel-progressive-speed nil)
    {{< /highlight >}}
-   控制鼠标每次移动的行数是鼠标每次能够移动两行
    {{< highlight elisp >}}
    (setq mouse-wheel-scroll-amount '(2))
    {{< /highlight >}}
-   取消鼠标高亮
    {{< highlight elisp >}}
    (setq mouse-highlight nil)
    {{< /highlight >}}


#### 鼠标的语法 {#鼠标的语法}

| 鼠标按键        | elisp表示                  |
|-------------|--------------------------|
| 鼠标左键        | (kbd "&lt;mouse-1&gt;")    |
| 鼠标滚轮键      | (kbd "&lt;mouse-2&gt;")    |
| 鼠标右键        | (kbd "&lt;mouse-3&gt;")    |
| 鼠标滚轮向前(linux) | (kbd "&lt;mouse-4&gt;")    |
| 鼠标滚轮向后(linux) | (kbd "&lt;mouse-5&gt;")    |
| 鼠标滚轮向前(mac,win) | (kbd "&lt;wheel-up&gt;")   |
| 鼠标滚轮向后(mac,win) | (kbd "&lt;wheel-down&gt;") |


## lisp basics {#lisp-basics}


### 基础 {#基础}

-   输出函数print
    格式:(print OBJECT &amp;optional PRINTCHARFUN)
    print接受一个输出对象和一个输出的目标buffer
    object即为输出对象,printcharfun即输出的目标buffer
    {{< highlight elisp >}}
    (progn
      (setq gbuffer (generate-new-buffer "*my output*"))
      (print "fasdfs" gbuffer)
      (switch-to-buffer gbuffer))
    {{< /highlight >}}
-   unicode字符表示
    **\uxxxx** :接受四个16进制数
    **\u00xxxxxx** :接受六个16进制数
-   处理字符串的函数

    | 函数             | 示例                            | 作用            | 结果               |
    |----------------|-------------------------------|---------------|------------------|
    | length           | (length "abc")                  | 获得"abc"长度   | 3                  |
    | substring        | (substring "abc123" 0 3)        | 截取字符串0-3之间的子字符串 | abc                |
    | concat           | (concat "some" "thing")         | 字符串拼接      | something          |
    | split-string     | (split-string "xy_007_cat" "_") | 以'_'截取字符串 | ("xy","007","cat") |
    | string-to-number |                                 | 字符串转数字    |                    |
    | number-to-string |                                 | 数字转字符      |                    |
-   相等判断
    equal:判断数据类型和值是否相同
    eq:判断是否为同一对象
    eql:与eq很像但两个相同的浮点数会返回t


### 数据类型 {#数据类型}

-   mapcar
    (mapcar FUNCTION SEQUENCE)
    会返回处理后的结果
-   mapc
    与mapcar一样但不会返回结果,而返回nil


## practical emacs lisp {#practical-emacs-lisp}


### 概述 {#概述}


#### 光标位置 {#光标位置}

| 函数                    | 作用                       |
|-----------------------|--------------------------|
| point                   | 返回当前光标位置,其值为前面字符个数,包括空格和回车 |
| region-beginning        | 返回选中区域的开头字符位置 |
| region-end              | 返回选中区域的结尾字符位置 |
| line-beginning-position | 返回行首字符位置           |
| line-end-position       | 返回行为字符位置           |
| point-min               | 返回整个文件开头位置       |
| point-max               | 返回整个文件结尾位置       |


#### 光标移动和文本搜索 {#光标移动和文本搜索}

{{< highlight emacs-lisp >}}
;;跳转到39字符的位置
 (goto-char 39)
;;使光标前移和后移四个字符
(forward-char 4)
(backward-char 4)
;;向前搜"some"和向后搜索"some"
(search-forward "some") ; to end of “some”
(search-backward "some") ; to beginning of “some”
;;向前搜索数字和向后搜索数字
(re-search-forward "[0-9]") ; digit
(re-search-backward "[0-9]")
;;将光标前移和将光标后移,知道找到非小写字母
(skip-chars-forward "a-z")
(skip-chars-backward "a-z")
{{< /highlight >}}


#### 修改文本 {#修改文本}

{{< highlight emacs-lisp >}}
;;从光标开始删除9个字符
(delete-char 9)
;;删除3-10位置的字符
(delete-region 3 10)
;;在当前光标处插入字符串
(insert "i ♥ cats")
;;获取当前buffer71-300的字符串,并赋值给x
(setq x (buffer-substring 71 300))

;;将7-300处的字母转为大写
(capitalize-region 71 300)
{{< /highlight >}}


#### buffer操作 {#buffer操作}

{{< highlight emacs-lisp >}}
;;获取buffer名称
(buffer-name)

;;返回当前buffer文件的绝对路径
(buffer-file-name)

;; switch to the buffer named xyz
(switch-to-buffer "xyz")

;;保存当前buffer
(save-buffer)

;; 关闭名为"xyz"的buffer
(kill-buffer "xyz")

;;临时将"xyz"buffer作为当前buffer,函数结束后将返回之前操作的buffer
(with-current-buffer "practical-elisp.el"
  ;;这里可以写一些插入删除等操作
)
{{< /highlight >}}


#### 文件操作 {#文件操作}

-   基本操作
    {{< highlight emacs-lisp >}}
      ;; open a file (in a buffer)
    (find-file "~/")

    ;; same as “Save As”.
    (write-file path)

    ;; insert file into current position
    (insert-file-contents path)

    ;; append a text block to file
    (append-to-file start-pos end-pos path)

    ;; renaming file
    (rename-file file-name new-name)

    ;; copying file
    (copy-file old-name new-name)

    ;; deleting file
    (delete-file file-name)

    ;; get dir path
    (file-name-directory  full-path)

    ;; get filename part
    (file-name-nondirectory full-path)

    ;; get filename's suffix
    (file-name-extension file-name)

    ;; get filename sans suffix
    (file-name-sans-extension file-name)
    {{< /highlight >}}
-   narrow操作
    narrow操作可以使文件只有指定的部分可见,而其余部分不可见,除此以外它的效果与删除效果几乎相同,它会对光标行数等产生影响.可以调用"widen"把刚才隐藏的部分显示出来

    | 函数             | 作用                                |
    |----------------|-----------------------------------|
    | narrow-to-defun  | 仅显示当前光标所在的函数            |
    | narrow-to-page   | 仅显示当前页面中可见的内容          |
    | narrow-to-region | 仅显示指定位置的内容                |
    | widen            | 显示隐藏部分的内容                  |
    | buffer-narrow-p  | 判断当前buffer是否有隐藏内容        |
    | save-excursion   | 保存当前光标位置,执行完body里的内容后返回 |
    | save-restriction | 保存当前缓冲区,执行完body里的内容后返回通常与narrow一起使用 |


#### 案例 {#案例}

-   替换选中区域的文本
    {{< highlight emacs-lisp >}}
    (defun geezer-replace-greek-region ()
     (interactive)
     (let ((start (region-beginning))
           (end (region-end)))
       (save-restriction
         (narrow-to-region start end)
         (goto-char (point-min))
         (while (search-forward "a" nil t)
           (replace-match "A" nil t))
         (goto-char (point-min))
         (while (search-forward "b" nil t)
           (replace-match "B" nil t)))))
    {{< /highlight >}}
-   删除括号中的内容
    {{< highlight emacs-lisp >}}
    (defun geezer-delete-pair ()
     (interactive)
     (save-excursion
       (let (p1 p2)
         (skip-chars-backward "^([{<>")
         (setq p1 (point))
         (skip-chars-forward "^)]}<>")
         (setq p2 (point))
         (delete-region p1 p2))))
    {{< /highlight >}}


#### defun中的interactive的作用 {#defun中的interactive的作用}

定义函数时interactive的作用有两个

1.  是函数可以以命令的形式调用,及通过M-x的方式
2.  在interactive后加上参数可以是调用该函数时实现交互式传参

<!--list-separator-->

-  参数说明

    -   (ineractive)
        可以用命令调用
    -   (ineractive string)
        string第一个字符指定输入的内容,例如为s则表示为输入一个字符串,为n则表示数字等,
        若要从交互界面获取多个参数可以用回车隔开
        {{< highlight emacs-lisp >}}
          (defun ask-name-and-age (x y)
        "Ask name and age"
        (interactive
         "sEnter name
          nEnter age:")
        (message "Name is: %s, Age is: %d" x y))
        {{< /highlight >}}
    -   (ineractive list)
        这种方式是最常用的一种方式,list中可以是任何表达式,只要能够返回对应参数个数的list即可
        {{< highlight emacs-lisp >}}
          (defun ask-name-and-age2 (x y)
        "Ask name and age"
        (interactive
         (list
          (read-string "Enter
        name:")
          (read-number "Enter age:")))
        (message "Name is: %s, Age is: %d" x y))
        {{< /highlight >}}

<!--list-separator-->

-  thing-at-point

    函数调用:(thing-at-point THING &amp;optional NO-PROPERTIES)
    当我们用interactive进行交互时,使用 **thing-at-point** 是非常方便的,
    例如,我们在寻求帮助时,使用的describe-function,describe-variable(对应快捷键C-h f,C-h k)时,
    就会获取当前光标处的函数或变量,使得我们在调用上面两个命令时,如果我们不做输入,它会默认选择光标处的函数或变量
    thing-at-point不仅可以获取当前光标处的函数和变量,同时也可以获取光标处的
    'symbol, 'list, 'sexp, 'defun, 'filename, 'url, 'email, 'uuid, 'word, 'sentence, 'whitespace, 'line, 'number, 'page.
    只需要将 **thing-at-point** 的第二个参数设为上面这些值即可

    {{< highlight emacs-lisp >}}
    (defun test ()
      "获取单词"
      (interactive)
      (message "%s" (thing-at-point 'word)))
    {{< /highlight >}}

<!--list-separator-->

-  bounds-of-thing-at-point

    获取thing-at-point的边界,返回的是一个cons,其第一个元素为边界的开始的位置,第二个元素为边界结束的位置

    {{< highlight emacs-lisp >}}
    (defun test ()
      (interactive)
      (let (bounds pos1 pos2 mything)
        (setq bounds (bounds-of-thing-at-point 'symbol))
        (setq pos1 (car bounds))
        (setq pos2 (cdr bounds))
        (setq mything (buffer-substring-no-properties pos1 pos2))
        (message
         "thing begin at [%s],end at [%s],thing is [%s]"
         pos1 pos2 mything))
    {{< /highlight >}}


#### 查找替换 {#查找替换}

-   search-forward和search-backward.
    (search-forward STRING &amp;optional BOUND NOERROR COUNT)
    将关闭移动到要查找的字符串开始位置,
-   re-search-forward和re-search-backward
    (re-search-forward REGEXP &amp;optional BOUND NOERROR COUNT)
    与上面函数功能一样,不过匹配的是正则表达式,而非字符串
-   search-forward-regexp和search-backward-regexp
    分别是re-search-forward和re-search-backward的别名
-   case-fold-search变量其值默认为t,表示查找是忽略大小写为nil时,表示不忽略大小写


### elisp脚本 {#elisp脚本}


#### 运行elisp脚本 {#运行elisp脚本}

elisp可以运行在终端,比如

{{< highlight shell >}}
emacs --script  /path/to/elisp-file
{{< /highlight >}}


#### buffer相关函数 {#buffer相关函数}

-   buffer-name
    (buffer-name &amp;optional BUFFER)
    获取当前buffer名字
-   buffer-file-name
    (buffer-file-name &amp;optional BUFFER)
    返回当前buffer处文件的绝对路径,如果不存在则返回nil
-   with-current-buffer
    (with-current-buffer BUFFER-OR-NAME &amp;rest BODY)
    将某个缓冲区作为临时的缓冲区,执行完body里的内容后返回
-   set-buffer
    (set-buffer BUFFER-OR-NAME)
    跳转到指定buffer,但该buffer不可见,如果需要可见可使用switch-to-buffer
-   generate-new-buffer
    (generate-new-buffer NAME)
    新建一个buffer
-   get-buffer-create
    (get-buffer-create BUFFER-OR-NAME)
    新建一个buffer,但不将其作为当前buffer
-   kill-buffer
    (kill-buffer &amp;optional BUFFER-OR-NAME)
    关闭指定buffer


#### 文件相关函数 {#文件相关函数}

-   directory-files
    返回一个带有该目录下所有指定文件的list
    不会递归查找,即如果该目录下有子目录不会对子目录进行查找
    (directory-files DIRECTORY &amp;optional FULL MATCH NOSORT)
    参数1:目录路径参数2:返回的文件是否为绝对路径参数3:文件匹配的方式参数4:是否排序
-   directory-files-recursively
    递归查找指定文件,后返回一个带有所有指定文件的list
    会递归查找,即如果该目录下有子目录会对子目录进行查找由于会递归查找所以会返回文件的相对路近,而非只含文件名
    (directory-files-recursively DIR REGEXP &amp;optional INCLUDE-DIRECTORIES)


## elisp example {#elisp-example}


## emacs write major-mode {#emacs-write-major-mode}


### font lock mode {#font-lock-mode}

它是emacs内置的一个minor-mode,它与当前buffer的语法高亮有关,其高亮规则通过两种方式实现

1.  Syntactic fontification
    一些与注释字符串,符号相关的注释,它的注释内容存放在,Syntax Table中,可通过describe-syntax命令查看
2.  Search based fontification
    使用正则表达式查找相关内容进行高亮,通常高亮的是一些关键字,函数名,变量名之类的,它依赖于,font-lock-defaults变量


### font-lock-defaults {#font-lock-defaults}

该变量通常是一个list,为nil的话将不会进行任何高亮,而list里面通常有四个关键变量

1.  font-lock-keywords
    关键字的高亮规则
2.  font-lock-keywords-only
    字符串和注释等的高亮规则
3.  font-lock-keywords-case-fold-search
    正则表达式是否区分大小写
4.  font-lock-syntax-table
    与syntax有关
