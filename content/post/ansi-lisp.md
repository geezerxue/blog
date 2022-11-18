
---
title: ansi commonlisp 
tags:
---
# 第五章  数据和控制流

**apply** 函数和 **funcall** 函数的区别：

```commonlisp
;;funcall 的第一个参数为执行函数（将第一个参数称为“执行函数”），剩余参数作为该函数的参数；
;;apply 与 funcall 的作用一样，只是其最后一个参数必须为list；

(funcall #'+ 3 4)
;;7
 (apply #'+ 3 4 '(3 4))
;;14
```

**fboundp** 函数接受一个参数name如果name被某个函数绑定则返回true否则为false

***fmakunbound*** 函数接收一个参数name,  如果name被某个函数绑定的话，在全局环境中移除这个名字 为name 的函数或宏定义，注意name不能为common lisp库中的函数，

```commonlisp
(defun myfun () (+ 1 2))
>(myfun)  ;;正常输出 3
(fmakunbound 'myfun)
>(myfun)  ;;报错
```

  ***flet, labels, 和 macrolet*** 定义局部函数和宏, 并且使用这些局部定义执行表达式形式这些表达式形式以出现的顺序被执行.

 
