---
title: golang数据类型
date: 2023-12-08T16:53:03+08:00
tags:
  - golang
categories:
  - Program
---
|   |   |   |
|---|---|---|
|数据类型|描述|包含类型|
|基础类型(basic type)|包含数字（number）、字符串（string）和布尔型（boolean）|整数类型：int、int8、int16、int32、int64、uint、uint8、uint16、uint32、uint64；浮点类型：float32、float64；布尔类型：bool；字符串类型：string；字节类型：byte（_uint8_ 的别名），rune（_int32_ 的别名）；复数类型:complex64、complex128|
|聚合类型(aggregate type)|通过组合各种简单类型得到的更复杂的数据类型|array；struct；|
|引用类型(referenct type)|指向程序变量或者状态，操作所引用数据的效果会遍及该数据的全部引用|pointer，slice，map，function，channel|
|接口类型(interface type)|接口类型是对其他类型行为的概括与抽象|interface|