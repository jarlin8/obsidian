---
title: 富文本转markdown代码
post_status: publish
skip_file: no
taxonomy:
  category:
        - toutiao
  post_tag:
        - php
post_excerpt: 
---
```javascript
function convertRichTextToMarkdown(richTexts) {
  if (!Array.isArray(richTexts)) {
    return '';
  }
  return richTexts.map(text => {
    if (text.href) {
      // 处理链接
      return `[${text.plain_text}](${text.href})`;
    } else if (text.annotations && text.annotations.code) {
      // 处理内联代码
      return `\`${text.plain_text}\``;
    } else {
      // 支持粗体和颜色
      var markdownText = text.plain_text;
      if (text.annotations && text.annotations.bold) {
        markdownText = `**${markdownText}**`;
      }
      if (text.color && text.color !== 'default') {
        markdownText = `<span style="color:${text.color}">${markdownText}</span>`;
      }
      return markdownText;
    }
  }).join('');
}
```

**山穷水尽疑无路，柳暗花明又一村.**

![Image](https://images.unsplash.com/photo-1674027444485-cec3da58eef4?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb)

![Image](https://images.unsplash.com/photo-1551808525-51a94da548ce?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb)