---
title: "7个有用的wordpress图片云储存方案"
post_status: publish
post_date: 2022-05-20
taxonomy:
  category:
    - 杂谈系列
---

储存图片一直是我们这些小站长的难题，本地占用空间大，速度还提不起来。

这里搜集整理了三个可用的解决方案和插件分享：

- 阿里云 OSS
- 腾讯云 COS
- 远程上传到 Github 使用 Jsdelivr CDN 加速

阿里云是我自己在使用的，一起开了 CDN,后来没有啥访问量，很鸡肋，就关掉了。

## 阿里云 OSS

### Aliyun **OSS** 

作者： [Ivan Chou](https://yii.im/)在 wordpress 后台插件库可以直接搜索到，目前使用正常，好像快两年没有更新了。推荐使用 wordpress 插件页面安装的 3.2.7 版本，新版 3.30 在 wordpress5.7 里面出现故障，提示致命错误。

github 仓库地址：[https://github.com/yiichou/aliyun-oss-support](https://github.com/yiichou/aliyun-oss-support)

#### 插件特点

1. 支持 Aliyun OSS 的图片服务（根据参数获得不同尺寸的图片）
2. 自定义文件在 Bucket 上的存储位置
3. 支持 Https 站点
4. 支持阿里云内网和 VPC 网络
5. 全格式附件支持，不仅仅是图片
6. 支持 WordPress 4.4+ 新功能 srcset，在不同分辨率设备上加载不同大小图片
7. 支持 WordPress 5.0+ 默认古藤堡编辑器 Gutenburg
8. 支持阿里云 url 鉴权，实现高级别防盗链功能，支持阿里云 CDN 的 A、B、C 所有方式鉴权，推荐 A 方式
9. 支持在 WordPress 后台编辑图片
10. 支持预设图片样式，图片保护，自定义分割符
11. 中英文双语支持，方便使用英文为默认语言的同学
12. 支持在其他插件/主题中通过系统钩子调用插件功能
13. 代码遵循 PSR-4 规则编写

### Aliyun-oss-wordpress

该插件将 WordPress 站点图片等多媒体文件直接上传到阿里云对象存储 OSS 中，一键将静态资源托管到阿里云

github 仓库地址：[https://github.com/sy-records/aliyun-oss-wordpress](https://github.com/sy-records/aliyun-oss-wordpress)

#### 插件特点

1. 可配置是否上传缩略图和是否保留本地备份
2. 本地删除可同步删除阿里云对象存储 OSS 中的文件
3. 支持阿里云对象存储 OSS 绑定的用户域名
4. 支持替换数据库中旧的资源链接地址
5. 支持阿里云对象存储 OSS 完整地域使用
6. 支持同步历史附件到阿里云对象存储 OSS
7. 支持使用阿里云内网传输

## 腾讯云 COS 云存储插件

该插件将 WordPress 站点图片等多媒体文件直接上传到腾讯云对象存储 COS 中，该插件依赖腾讯云对象存储 COS。

**Github 下载节点：**[https://github.com/sy-records/wordpress-qcloud-cos/releases/latest](https://github.com/sy-records/wordpress-qcloud-cos/releases/latest)

**Github 项目地址：**[https://github.com/sy-records/wordpress-qcloud-cos](https://github.com/sy-records/wordpress-qcloud-cos)

#### 插件特点

1. 可配置是否上传缩略图和是否保留本地备份
2. 本地删除可同步删除腾讯云对象存储 COS 中的文件
3. 支持腾讯云 COS 存储桶绑定自定义域名
4. 支持替换数据库中旧的资源链接地址
5. 支持北京、上海、广州、香港、法兰克福等完整地域使用
6. 支持同步历史附件到 COS
7. 支持验证桶名是否填写正确
8. 更多功能正在路上…

## Github 作为免费图床

Github 项目地址: [https://github.com/niqingyang/wp-github-gos](https://github.com/niqingyang/wp-github-gos "https://github.com/niqingyang/wp-github-gos")

#### 插件特色

- 使用 GitHub 仓库存储 WordPress 站点图片等多媒体文件
- 可配置是否上传缩略图和是否保留本地备份
- 本地删除可同步删除腾讯云上面的文件
- 支持替换数据库中旧的资源链接地址
- 支持在图片链接地址后面自定义拼接图片`宽度`、`高度`、`大小`三个参数
- 建议使用 jsdelivr 免费加速 github 下的图片，速度还是很可以的

#### 插件缺点

- 使用 Github API 同步图片等附件的时候速度相较于国内的免费图床比较慢
- 未来不知道会不会被屏蔽

感谢以上作者的辛勤付出，有需求的小伙伴自取。

## 华为云 OBS

[GitHub](https://github.com/sy-records/huaweicloud-obs-wordpress)，[WordPress Plugins](https://wordpress.org/plugins/obs-huaweicloud)

## 七牛云 KODO

[GitHub](https://github.com/sy-records/qiniu-kodo-wordpress)，[WordPress Plugins](https://wordpress.org/plugins/kodo-qiniu)

## 又拍云 USS

[GitHub](https://github.com/sy-records/upyun-uss-wordpress)，[WordPress Plugins](https://wordpress.org/plugins/uss-upyun)
