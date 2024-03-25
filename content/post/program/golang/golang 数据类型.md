---
title: golang 数据类型
date: 2023-12-08T16:53:03+08:00
tags:
  - golang
categories:
  - Program
---

| 数据类型                      | 包含类型                                                                                                                                                                                                                                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 基础类型(**Basic types**)     | **布尔类型**（bool）：取值为true或false。<br>**整型**：<br>    - 有符号整型：int8、int16、int32、int64<br>    - 无符号整型：uint8、uint16、uint32、uint64<br>    - 特殊整型：uint、int、uintptr、byte(uint8)、rune(int32)<br>**浮点型**：<br>    - float32<br>    - float64<br>**复数类型**：complex64、complex128<br>**字符类型**（string）：由字节序列组成，支持Unicode字符集 |
| 复合类型(**Composite types**) | array, struct, pointer, function, interface, slice, map, channel                                                                                                                                                                                                                                        |
## 参考资料
 [Types](https://go.dev/ref/spec#Types)
 [Go语言数据类型]( https://zhuanlan.zhihu.com/p/612665303)

