## 代码段如下

```
<% await tp.file.move("/目标文件夹/" + ((tp.file.title.includes("未命名") || tp.file.title.toLowercase().includes("untitled"))  ? (await tp.system.prompt("请输入要创建的文件名")) : tp.file.title)) %>
```