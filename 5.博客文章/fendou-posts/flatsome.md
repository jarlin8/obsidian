---
title: "Flatsome主题nulled代码备忘/Google实时收录"
post_status: publish
post_date: 2023-11-17
taxonomy:
  category:
    - 工作日志
---

## 编辑 function.php

在`require get_template_directory() . '/inc/init.php';`前面添加如下代码:

```
update_option( 'flatsome_wup_purchase_code', 'B5E0B5F8DD8689E6ACA49DD6E6E1A930' );
update_option( 'flatsome_wup_supported_until', '01.01.2050' );
update_option( 'flatsome_wup_buyer', 'Licensed' );
update_option( 'flatsome_wup_sold_at', time() );
delete_option( 'flatsome_wup_errors', '' );
delete_option( 'flatsome_wupdates', '');
```

## 编辑文件 /inc/admin/admin-notice.php

删去提醒通知 注释掉相应代码就可以了.一种简单的方法是注释或移除添加通知的动作挂钩。找到以下两行代码：

```
add_action( 'admin_notices', 'flatsome_status_check_admin_notice' );
add_action( 'admin_notices', 'flatsome_maintenance_admin_notice' );
```

[实时收录链接设置教程](https://rankmath.com/blog/google-indexing-api/#:~:text=4.3-,Delegate%20Service%20Account%20ID%20as%20Owner,-A%20popup%20will)

- https://search.google.com/search-console

- 用户和权限添加 **rankmathgoolgeindex@swift-stack-385704.iam.gserviceaccount.com**
