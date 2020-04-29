# SpaceSniffer 二进制快照文件格式解析

官方网站：[http://www.uderzo.it/main_products/space_sniffer/](http://www.uderzo.it/main_products/space_sniffer/)

后缀名：`.sns`

## 文件格式描述

```
数字编码：little-endian
日期格式：MS File Dates (LDAP Timestamp, 可用此网站转换：https://www.epochconverter.com/ldap)
文件顺序：字典序（暂不确定如果不依照字典序是否会有影响）
中文字符集：GBK / GB2312（其他语言应该是按照系统字符集）

层级：最后要跳出进入的第一层级
如果当前层是最底层，那么是文件，否则是文件夹
就像是一个栈的感觉

每个文件项：
[类型标志：目录03, 文件02, 2byte]
[编码后文件名长度, 4byte]
[base64编码的文件名]
[文件大小 byte, 16byte]
[磁盘上实际占用 byte, 8byte]
[未知参数 8byte] <- 修改后无界面明显变化
[date1, 16byte] <- 应该是a/c/mtime，但是还没看明确对应关系
[date2, 16byte]
[date3, 16byte]
[当前项结束标志 0000]
[返回上层 0100]
```



## 示例文件夹架构

示例文件：example.sns

```
jerrylu@JERRYLU-DELL:/mnt/c/sns_example$ tree --du
.
├── [          4]  a.txt
├── [          8]  abc.txt
├── [        512]  folder1
│   └── [          0]  a.txt
├── [       1137]  folder2
│   ├── [         26]  b.txt
│   └── [        599]  folder3
│       └── [         87]  d.txt
└── [         13]  z.txt

        2186 bytes used in 3 directories, 6 files
```



## TODO

- [ ] 英文文档
- [ ] 上传示例文件 + 图片说明
- [ ] decoder
- [ ] encoder