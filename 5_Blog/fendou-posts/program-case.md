---
title: "PHP&Linux服务器配置基本笔记"
post_status: publish
post_date: 2022-08-24
taxonomy:
  category:
    - 靠谱项目
---

## PHP 安装 Redis 扩展

https://www.gwern.net/index开始在 PHP 中使用 Redis 前，我们需要确保已经安装了 redis 服务，且你的机器上能正常使用 PHP。 接下来让我们安装 PHP redis 驱动，下载地址为:[https://github.com/phpredis/phpredis/](https://github.com/phpredis/phpredis/)

## oneinstack 中的 igbinary 编译问题

```
 C:UsershankOneDrive - teleworm桌面oneinstackincluderedis.sh （匹配2次）
    行 50:   if [ -e "${php_install_dir}/bin/phpize" ]; then
    行 60:     ${php_install_dir}/bin/phpize

./configure --enable-redis-igbinary  --with-php-config=${php_install_dir}/bin/php-config
```

### 下载并安装

```
git clone https://github.com/phpredis/phpredis.git
cd phpredis
phpize
./configure  --enable-redis-igbinary --enable-redis-zstd --with-php-config=/www/server/php/74/bin/php-config
make && make install

// configure: error: Please reinstall the libzstd distribution报错
sudo apt update && sudo apt install libzstd-dev

// 设置的淘汰策略：
 通过redis.conf 配置文件设置 重启redis
maxmemory-policy allkeys-lru

// object-redis-pro插件配置文件wp-config
define('WP_REDIS_CONFIG', [
    'token' => 'zupbuTFQtMatJv-RDp@+A#kIabgcMjdN0-iOr5w9AurifIenINk*v*TM7MKo',
    'maxttl' => 3600 * 24, // 24 hours
    'host' => '127.0.0.1',
    'port' => 6379,
    'database' => 0, // change for each site
    'timeout' => 0.5,
    'read_timeout' => 0.5,
    'retry_interval' => 10,
    'retries' => 3,
    'backoff' => 'smart',
    'compression' => 'zstd',
    'serializer' => 'igbinary',
    'async_flush' => true,
    'split_alloptions' => true,
    'prefetch' => true,
    'debug' => false,
    'save_commands' => false,
]);

define('WP_REDIS_DISABLED', getenv('WP_REDIS_DISABLED') ?: false);
```

### redis 配置文件

在网上查询了许多的资料都是直接在`php.ini`文件中添加`extension=redis.so`.当我添加之后会出现错误:

```
PHP Warning: PHP Startup: Unable to load dynamic library 'redis.so'
(tried: /usr/lib64/php/modules/redis.so (/usr/lib64/php/modules/redis.so: ....
```

不要在 php.ini 里加入`extension=redis.so`这行，可在 php.d(`whereis php.d`查看在哪)文件夹下创建新文件 redis.ini，在 redis.ini 里加入`extension=redis.so`这行.  
重启 php

### 查看 PHP 启动了哪些扩展和服务

`php -m` 发现 redis 扩展加载上了

## argon 主题底部版权申明

argon: Theme Footer (footer.php):5

```
<div>Theme <a href="https://github.com/solstice23/argon-theme" target="_blank"><strong>Argon</strong></a><?php if (get_option('argon_hide_footer_author') != 'true') {echo " By solstice23"; }?></div>
```

argon: Theme Functions (functions.php):2007

```
//检测页面底部版权是否被修改
function alert_footer_copyright_changed(){ ?>
    <div class='notice notice-warning is-dismissible'>
        <p><?php _e("警告：你可能修改了 Argon 主题页脚的版权声明，Argon 主题要求你至少保留主题的 Github 链接或主题的发布文章链接。", 'argon');?></p>
    </div>
<?php }
function check_footer_copyright(){
    $footer = file_get_contents(get_theme_root() . "/" . wp_get_theme() -> template . "/footer.php");
    if ((strpos($footer, "github.com/solstice23/argon-theme") === false) && (strpos($footer, "solstice23.top") === false)){
        add_action('admin_notices', 'alert_footer_copyright_changed');
    }
}
check_footer_copyright();
```

argontheme.js:2611

```
/*Console*/
!function(){...}();
```

## 主题顶部 ajax 搜索添加

```
// argon: Theme Header (header.php):421
<div id="banner_container" class="banner-container container text-center">
<?php echo do_shortcode('[wpdreams_ajaxsearchpro id=1]'); ?>
</div>
```

## 禁用 wp-emoji-release.min.js

```
// Disable the emoji's
function disable_emojis() {
 remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
 remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
 remove_action( 'wp_print_styles', 'print_emoji_styles' );
 remove_action( 'admin_print_styles', 'print_emoji_styles' );
 remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
 remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
 remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
 add_filter( 'tiny_mce_plugins', 'disable_emojis_tinymce' );
 add_filter( 'wp_resource_hints', 'disable_emojis_remove_dns_prefetch', 10, 2 );
}
add_action( 'init', 'disable_emojis' );

/**
 * Filter function used to remove the tinymce emoji plugin.
 * @param array $plugins
 * @return array Difference betwen the two arrays
 */
function disable_emojis_tinymce( $plugins ) {
 if ( is_array( $plugins ) ) {
 return array_diff( $plugins, array( 'wpemoji' ) );
 } else {
 return array();
 }
}

/**
 * Remove emoji CDN hostname from DNS prefetching hints.
 * @param array $urls URLs to print for resource hints.
 * @param string $relation_type The relation type the URLs are printed for.
 * @return array Difference betwen the two arrays.
 */
function disable_emojis_remove_dns_prefetch( $urls, $relation_type ) {
 if ( 'dns-prefetch' == $relation_type ) {
 /** This filter is documented in wp-includes/formatting.php */
 $emoji_svg_url = apply_filters( 'emoji_svg_url', 'https://s.w.org/images/core/emoji/2/svg/' );

$urls = array_diff( $urls, array( $emoji_svg_url ) );
 }

return $urls;
}
```

## sharelist 安装避雷

### 忘记后台密码？

```
// 一般在路径./cache/config.jason里面 token="#$%******"
```

### nginx 配置反向代理

```
端口：33001
// 自行安装 docker和pm2
// cd到根目录 bash install.sh 报错 npm comand not found
使用文本编辑器打开install.sh文件
不难发现PATH指定了NodeJs的路径，本人配置了全局NodeJs环境，所以注释掉该行（前面加个#号），保存即可。
```

## jsDelivr 域名遭 DNS 污染解决方案

### 官方子域

- CloudFlare：test1.jsdelivr.net

- CloudFlare：testingcf.jsdelivr.net

- Fastly：fastly.jsdelivr.net

- GCORE：gcore.jsdelivr.net

- originfastly.jsdelivr.net

### 针对 GH 的反向代理

```
#针对/gh目录的反代
location /gh
{
proxy_pass https://104.16.86.20;
proxy_set_header Host cdn.jsdelivr.net;
proxy_ssl_server_name on;
proxy_ssl_name cdn.jsdelivr.net;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header REMOTE-HOST $remote_addr;
}
```

### 360 静态库：`cdn.baomitu.com`

完全同步的 cdnjs 内容，又要提供谷歌字体的加速，通过自己的 CDN 加速，前段时间启用了 AWS CloudFront 的海外节点，是目前国内公共 CDN 做的比较好的。

## begin 主题将标题下的日期改为更新日期

原代码以及替换后的代码

```
		echo '<span class="meta-date">';
		echo '<time datetime="';
		echo get_the_date('Y-m-d');
		echo ' ' . get_the_time('H:i:s');
		echo '">';
		time_ago( $time_type ='posts' );
		echo '</time></span>';
	-----------
       echo '<span class="meta-date">';
        echo '<time title="' . __('发布于') . ' ' . get_the_time('Y-n-d G:i:s') . ' | ' . __('编辑于',) . ' ' . get_the_modified_time('Y-n-d G:i:s') . '">' . get_the_modified_time('Y-n-d G:i') . '</time>';
		echo '</span>';
```
