```
<% await tp.file.rename(tp.file.path(true).split('/')[tp.file.path(true).split('/').length-2] + " " + ((tp.file.title.includes("未命名") || tp.file.title.toLowercase().includes("untitled")) ? (await tp.system.prompt("请输入要创建的文件名")) :tp.file.title)) %>
```