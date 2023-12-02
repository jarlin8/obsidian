---
post_date: <% tp.file.creation_date("YYYY-MM-DD HH:mm:ss") %>
post_tag: <% tp.system.suggester(["牛市直通车","牛哥早盘","复利说"],["牛市直通车","牛哥早盘","复利说"] )%>
---
<% await tp.file.move("5.博客文章/wechat-posts/" + tp.file.title) %> 