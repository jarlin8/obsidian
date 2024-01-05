---
title: 多站点 WPMU 注意事项/屏蔽 IP 方法
post_status: publish
skip_file: no
post_date: 2023-12-29T04:10:00.000Z
taxonomy:
  category:
        - smartbook
  post_tag:
        - 投资
post_excerpt: 
---
WordPress 多站点是指安装一个程序，就可以用来建立管理多个站点，属于自带功能。

```php
// 开启前 缓存插件 比如wp-rocket、redis对象存储等最好先停用
// 开启功能 配置文件wp-config.php里面加入一行代码
// /* That’s all, stop editing! Happy blogging. */上面
define('WP_ALLOW_MULTISITE', true);
```

进入后台 工具>>站点网络配置，按照页面提示进行下一步操作。

主题里面要装一个默认的主题比如 twentytwentytwo，以免出错。

## 多站点网站迁移注意

直接在 phpmyadmin 管理后台找到相应的数据库 搜索 `blogs` `options` 然后双击进入 相应修改。这种方法适用于网站后台打不开的情况。

* wp-config.php 里面的 URL 要更新

* 数据库里面的 wp_options `siteurl`

* 数据库里面的 wp_blogs `domain`

## 如何使用宝塔面板拒绝拦截 IP 地址

宝塔面板提供屏蔽 IP 工具允许您阻止单个 IP 地址以及 IP 地址范围，而无需编辑 Web 服务器配置文件。

> 重要：屏蔽 IP 工具是一项强大的功能，如果使用不当，可能会阻止合法服务或个人。

要在宝塔面板中屏蔽拦截 IP 地址，请导航至**安全**，选择屏蔽 IP，填入屏蔽 IP 或者 IP 段，点击屏蔽按钮即可添加指定 IP/IP 段至屏蔽列表。

在“将 IP 地址添加到屏蔽”模式中，您可以将 IPV4 地址、IPV6 地址和 CIDR（无类域间路由）IP 地址范围添加到阻止列表。CIDR 范围可用于屏蔽 IP 地址的连续范围（例如 127.0.0.1 到 127.0.0.255）。要生成有效的 CIDR 范围，我们建议使用这样的[工具](https://www.ipaddressguide.com/cidr)。

以下是一些您可以屏蔽的 IP 地址示例：

* IPV4 地址 – 103.5.140.141

* IPV6 地址 – 2001:0db8:0a0b:12f0:0000:0000:0000:0001

* CIDR 范围 – 128.0.0.1/32

## 如何在 Cloudflare 中拒绝拦截 IP 地址

如果您是 Cloudflare 用户，您可以使用 Cloudflare 仪表盘中的“IP Access Rules”工具来屏蔽 IP 地址和 IP 范围。

在 Cloudflare 仪表盘中，导航到**Firewall > Tools**。

要创建新的 IP 访问规则，请添加 IP 地址，选择“Block”操作，选择“This Website”（如果您希望规则适用于所有 Cloudflare 域，则选择“All Websites in Account”），然后单击“Add”。

添加访问规则后，它将出现在“IP Access Rules”列表中。在这里，您可以对访问规则进行更改，例如更改操作、添加注释和删除规则。

除了“Block”动作，Cloudflare 还支持“Challenge”、“Allow”和“JavaScript Challenge”。根据您要实现的目标，您可能希望使用这些其他操作之一，而不是“Block”。

### 在 Cloudflare 中拦截 IP 范围、国家和 ASN

除了单个 IP 地址，Cloudflare 的 IP 访问规则还支持 IP 范围、国家名称和 ASN（自治系统编号）。

* 要屏蔽 IP 范围，请为 IP 访问规则值指定 CIDR 范围。

* 要屏蔽某个国家/地区，请指定其为[Alpha-2 国家/地区代码](https://www.iban.com/country-codes)。

* 要屏蔽 ASN（由单个网络运营商控制的 IP 列表），请指定以“AS”开头的有效 ASN。

## 如何在 Nginx 中拒绝拦截 IP 地址

如果您的站点是使用 Nginx Web 服务器自托管的，您可以直接在 Web 服务器配置中屏蔽 IP 地址。

要在 Nginx 中屏蔽 IP 地址，请通过 SSH 连接到您的服务器并使用`nano`文本编辑器打开 Nginx 配置文件，如下所示：

`nano /etc/nginx/nginx.conf`

### 如何使用 Nginx 屏蔽单个 IP 地址

要在 Nginx 中阻止单个 IP（IPV4 或 IPV6）地址，请使用如下`deny`指令：

```php
deny 190.60.78.31;
deny 4b73:8cd3:6f7b:8ddc:d2f9:31ca:b6b1:834e;
```

### 如何使用 Nginx 屏蔽 CIDR IP 范围

要在 Nginx 中阻止 CIDR IP 范围，请使用以下指令：

`deny 192.168.0.0/24;`

### 高级 Nginx IP 拦截技术

如果要阻止对特定目录的访问（ed domain.com/secret-directory/），可以使用下面的 Nginx 指令：

```php
location /secret-directory/ {
<strong>deny</strong> 192.168.0.0/24;</p>
}
```

`deny`指令接受`all`一个值。这对于您想要阻止所有 IP 地址访问您的站点的情况很有用。`deny all;`指令通常与`allow` – 一起使用- 这使您可以允许特定的 IP 地址同时阻止其他所有内容。

```php
location /secret-directory/ {
allow 192.168.0.0/16;
deny all;
}
```

### 保存 Nginx 配置并重新加载 Nginx

使用 nano 完成配置编辑后，请务必按 Ctrl+O 保存更改。保存文件后，按 Ctrl+X 退出 nano。

要激活新的 IP 屏蔽规则，您还需要使用以下命令重新加载 Nginx 配置：

`sudo systemctl reload nginx`

## 如何在 Apache 中屏蔽 IP 地址

如果您的站点是使用 Apache Web 服务器自托管的，您可以直接在 Web 服务器配置中屏蔽 IP 地址。要在 Apache 中屏蔽 IP 地址，您需要使用.htaccess 文件，它允许您将唯一规则应用于特定目录。要在整个站点上应用规则，应将.htaccess 文件放在站点的根目录中。

首先，通过 SSH 连接到您的服务器，导航到您站点的根目录，然后使用以下命令创建.htaccess 文件：

`touch .htaccess`

接下来，使用`nano`文本编辑器打开.htaccess 文件，如下所示：

`nano .htaccess`

屏蔽 IP 的确切规则取决于您使用的是 Apache 2.2 还是 2.4，因此我们将包含两个版本的规则。编辑.htaccess 文件时，请使用 Apache 版本对应的规则。