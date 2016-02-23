---
layout: post
wordpress_id: golang_note_2016_02_23
title: "Golang Note 20160223"
description: ""
category: golang
tags: [golang]
---
{% include JB/setup %}

# golang。。。做点小笔记

1.一个文件夹不能存在两个package，在go install会编译不过
2.同个package中不能存在连个命名相同的全局变量
3.go install只能编译包含main的package，并生成exe文件（windwos下）
4.main包外的其他包中main函数不会生效
5.同个包的全局变量可以共享
6.不能导入包含package为main的包
7.同一命名空间不能存在一样的函数
8.struct中的嵌套struct，在初始化时需要注明类型
9.只要是可导的（首字母大写）全局变量，即使是在不同的命名空间中，都可以用*命名空间.全局变量*的方式来获取
10.变量名与结构体名不能重复，否则会报redeclared in this block错误
11.结构体与该结构体的函数得定义在同一命名空间
12.结构体中的属性类型可以为Func
13.nil只能复制给指针，channel，func，interface，map或者slice类型的变量