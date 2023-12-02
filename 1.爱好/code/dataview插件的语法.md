#dataview
## Dataview的基础语法
| 设置字段 | 释义                                                         | 必要与否 |
| -------- | ------------------------------------------------------------ | -------- |
| list     | 展现形式。创建列表，还有 table、task 可以选择                | 必要     |
| from     | 检索范围。从哪个文件夹（写在双引号里面），或者标签（写在#后面） | 非必要   |
| where    | 聚合条件。contains(file.name,"Dataview") 就是匹配文件名为 “Dataview” 的文件 | 非必要   |
| sort     | 排序，根据什么做排序。 `sort file.ctime` 就是根据文件的创建时间正序 | 非必要   |

## Dataview基础代码
```
```dataview 
list 
from "" 
where contains(file.name,"习惯")  ```
```

## Dataview支持的元数据
Dataview自动为每个页面添加大量元数据
`file.name:`文件标题 (字符串)
`file.folder :` 该文件所属文件夹的路径
`file.path:`完整的文件路径(字符串)
`file.link:`文件的链接(链接)
`file.size:` (一个数字)文件的大小(以字节为单位)
`file.ctime :`文件创建日期 (日期+时间)
`file.tags`: 笔记中所有标签的数组。子标签按每个级别细分，因此 `#Tag/1/A`将存储在数组中，作为`#Tag，#Tag/1，#Tag/1/A`
`file.etags` : 注释中所有显性标签的数组; 不像`file.tags` , 不包括子标签.
`file.cday :`文件创建的日期 (只是一个日期)
`file.mtime :`文件上次修改的日期(日期+时间)
`file.mday:`文件上次修改的日期 (只是一个日期)
`file.inlinks`: 指向此文件的所有传入链接的数组
`file.outlinks` : 此文件所有出站链接的数组。
`file.aliases`:注释的所有别名数组

## dataview如何去掉table视图File列
```
``` dataview
table without ID file.link AS "文章",file.cday AS "创建时间",file.etags AS "标签"
from #历史 
where file.name !="1.AllHistory"
sort file.name asc, file.cday desc
```
## dataview用法示例
``` dataview
table without ID file.link AS "文章",file.cday AS "创建时间",file.etags AS "标签"
from #历史 
where file.name !="1.AllHistory"
sort file.name asc, file.cday desc
```
## dataview实操视频
https://www.bilibili.com/video/BV1hB4y1y7BX/