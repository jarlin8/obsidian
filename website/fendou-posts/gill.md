---
title: Git it write 支持的 yaml 格式和使用说明
post_status: publish
skip_file: no
taxonomy:
  category:
        - log
  post_tag:
        - code
        - yaml
post_excerpt: 
---
## Git it write 支持的 yaml 字段

* `title`– 帖子标题

* `menu_order`– 菜单顺序

* `post_status`– 帖子的状态。支持的值：publish、draft、pending、future

* `post_excerpt`– 帖子摘录

* `post_date`– 要设置的发布日期。支持的格式：`2022-09-01 20:14:59`

* `comment_status`– 帖子的评论状态。支持的值：`open`,`closed`

* `page_template`– 为帖子设置的页面模板

* `stick_post`– 将帖子标记为置顶。支持的值：`yes`标记为置顶，`no`取消置顶。

* `taxonomy`– 帖子标签、类别等分类法。对于自定义帖子类型，请使用自定义分类法名称。

* `custom_fields`– 帖子的自定义字段。

* `skip_file`– 跳过文件的发布。支持值：`yes`跳过文件。

## 使用例子

```yaml

---

title: Title of the post
menu_order: 1
post_status: publish
post_excerpt: This is a post excerpt
taxonomy:
    category:
        - category-slug-1
        - category-slug-2
custom_fields:
    field1: value 1
    field2: value 2

---

## My post content
```

## 图片使用

* 自定义图床

* 全部放在`_images`目录下

![Image](https://cdn.fendou.la/tuoss/what-is-yaml.png)

> 图片可以正常插入 markdown 文件。所有图像都应存在于_images存储库根目录的文件夹中，而无需组织为文件夹。然后可以将它们用在比较像![alt text](/_images/pic4.jpg "This is pic4"). 文件夹中的所有图像_images都将上传到 WordPress 库，并且图像将在上传时自动链接到帖子。 这些图片只会上传一次，并且只要帖子更新或在另一篇帖子中使用相同的图片，它们就会被重复使用。

## 插入链接

可以按照 [markdown](https://zh.wikipedia.org/wiki/markdown) 语法正常添加链接。最好使用相对链接来引用存储库中的任何帖子，即引用`./faq.md`与链接值相同的目录中的任何帖子。Git it write 会`/faq/`在发布之前将其转换为。通过这样做，降价文件在 github 页面和您的主网站中都正确链接。

**示例：**  将从相对上一级的文件夹`../guide/doc1.md`中链接一个`doc1.md`文件。`guide`同样`.`可用于当前目录并`/`从存储库的根目录引用文件，即`/folder1/folder2/doc2.md`.

## HTML 和 Gutenberg 古腾堡编辑器

目前已经实测支持 Gutenberg 编辑器,如果不能使用,可以使用下面代码禁用 Gutenberg

```php
//Wordpress 5.0+ 禁用 Gutenberg 编辑器
add_filter('use_block_editor_for_post', '__return_false');
remove_action( 'wp_enqueue_scripts', 'wp_common_block_scripts_and_styles' );
// Disables the block editor from managing widgets in the Gutenberg plugin.
add_filter( 'gutenberg_use_widgets_block_editor', '__return_false', 100 );
// Disables the block editor from managing widgets.
add_filter( 'use_widgets_block_editor', '__return_false' );
```

## 参考文档

* [Git it write 官网文档](https://www.aakashweb.com/docs/git-it-write/faq/)

* [YAML 是什么？](https://spacelift.io/blog/yaml)