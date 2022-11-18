
---
title: on lisp
tags:
---
# on lisp

## 第二章 函数

### mapcar 函数

​	带有两个以上参数 一个函数加上一个以上的列表(每个列表都分别是函数的参数)，然后它可以将参数里的函数依次作用在每个列表的元素上。

```commonlisp
;;列表个数与mapcar接收的函数的参数个数有关
;;例如一下函数调用会报错，因为 “#'(lambda (x) (+ 10 x))” 函数只能接收一个参数，而其后有两个
  >(mapcar #'(lambda (x) (+ 10 x)) '(1 2 3) '(4 5 6))
;;正确调用
  > (mapcar #'(lambda (x) (+ x 10))
  '(1 2 3))
  (11 12 13)
  > (mapcar #'+
  '(1 2 3)
  '(10 100 1000))
  (11 102 1003)
```

### remove-if函数

​	它接受一个函数和列表，并且返回这个列表中所有调用那个函数返回假的元素.

```commonlisp
;;将1 2 ... 9依次调用remove-if后的函数如果调用结果返回T则不操作,否则将其添加到新的列表中
;;以下函数中if语句中的 "9" 其实不是必为9,只要是一个非nil的元素(即为真)的变量均可
>(remove-if
 #'(lambda (x)
     (if (= (mod x 2) 0) 9))
 '(1 2 3 4 5 6 7 8 9))
output:(1 3 5 7 9)
```

### 局部函数定义

​	lambda函数的最大缺陷在于它不能递归调用,所以引出了其他i定义局部函数的方法 labels函数,labels可以看作let的函数版本,但两者是有区别的,在let 表达式里，变量的值不能依赖于同一个let 里生成的另一个变量,而在labels中是允许的.这同时也使得由labels生成的函数能够进行递归调用.

```commonlisp
;;报错,x是let生成的变量所以y不能依赖于x
(let ((x 10) (y x)) (+ x y))

(defun count-instances (obj lsts)
    (labels ((instances-in (lst)
        (if (consp lst)
            (+ (if (eq (car lst) obj) 1 0)
               (instances-in (cdr lst)))
               0)))
     (mapcar #'instances-in lsts)))
```

