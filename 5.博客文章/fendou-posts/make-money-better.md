---
title: Content Egg Pro 自助配置文档
post_status: publish
post_date: 2023-12-04 22:45:24
skip_file: no
custom_fields: 
  post-format: 
taxonomy:
  category:
        - everyday
  post_tag:

post_excerpt: 
---
## Feed 模块

* feed 是保存在自己服务器上

* feed 每天重新加载用来更新价格

保存 feed 模块的设置后，csv/xml 文件会在回来导入到本地服务器，更新了 csv 文件可重新保存下相应 feed 模块的设置（点一下保存就好）。

### Feed 中 csv 文件映射

![Image](https://cdn.fendou.la/fendou/2022/04/feed-maping.png)

* `id` - 每个产品对应的独立 ID.

* `affiliate link` – 你的联盟链接（可追踪收益）.

* `is in stock` – 支持的值: “1”, “true”, “on” and “yes”, “0”, “false”, “off”, “no”.

* `availability` - 支持的值: “in stock”, “out of stock”.

* `direct link` – 原始产品页面上的直接 URL，没有重定向和附属参数.

* `gtin` - EAN 13 位数字，如：3001234567892.

### 大规模数据导入

如果在前面的步骤中映射了 EAN 代码，则 EAN 搜索将起作用。

URL 搜索适用于直接链接映射字段。如果您没有在上一步映射它，插件将尝试从您的会员链接建立直接链接。

![Image](https://cdn.fendou.la/fendou/2022/08/EAN-import.png)

## 利用 feed 功能导入京东联盟

* 利用京东排行榜获取产品链接、标题、图片、价格信息

* 利用产品链接获取永久固定推广链接

```plain text
<a href=[jd]https://item.jd.com/100032883020.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/10035709567913.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/10056338425042.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100039483134.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100013037954.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100014697861.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100029545999.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/3977065.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/29174400914.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100018360731.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/10049953660716.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/10047953506123.html[/jd]>京东联盟链接</a>
<a href=[jd]https://item.jd.com/100017043075.html[/jd]>京东联盟链接</a>
```

## EAN 编码自动生成校验

使用 Excel 对批量生成的产品码进行 EAN 校验

`=A799&RIGHT(SUM(LEFT($A799,{0,1}+{1;3;5;7;9;11})*{9,7}))`

## ContentEGG RPO

### 1.解除限制 A

**content-egg/application/admin/LicConfig.php** 92:107

```plain text
public function licFormat($value)
{
return true;
}
public function activatingLicense($value)
{
return true;
}
```

### 2.解除限制 B

**content-egg/application/components/LManager.php** 379

```plain text
public static function isNulled(){
return false;​
}
```

### 3. 比价模板添加商家 logo ✔

**content-egg/templates/block_price_comparison_card.php: 44**

**使用自定义模板解决**：block_ComparePrice

```plain text
//原代码
<?php echo esc_html(TemplateHelper::getMerhantName($item)); ?>
//替换成
<img src="https://testingcf.jsdelivr.net/gh/jarlin8/OSS@main/icons/favicon/<?php echo esc_attr( $item['domain']); ?>.svg" height="18" width="18">
 <?php echo esc_attr( $item['domain']); ?>
```

### 4.top-list 列表添加商家 logo ✔

**content-egg/templates/block_top_listing.php:55 行**

**使用自定义模板解决**： block_TopListing

```plain text
//原代码
 <div class="cegg-no-top-margin cegg-list-logo-title">
 <a<?php TemplateHelper::printRel(); ?> target="_blank" href="<?php echo esc_url_raw($item['url']); ?>"><?php echo esc_html(TemplateHelper::truncate($item['title'], 100)); ?></a>
</div>
// 替换成
<div class="cegg-no-top-margin cegg-list-logo-title">
 <img src="https://testingcf.jsdelivr.net/gh/jarlin8/OSS@main/icons/favicon/<?php echo esc_attr( $item['domain']); ?>.svg" height="18" width="18">
 <a<?php TemplateHelper::printRel(); ?> target="_blank" href="<?php echo esc_url_raw($item['url']); ?>"><?php echo esc_html(TemplateHelper::truncate($item['title'], 100)); ?></a>
 </div>
```

### 5.单个产品 list 展示 显示商家 logo ✖ 未解决

**content-egg/application/templates/blocks/list_row.php**：20&7 后面添加图标代码就好

```plain text
<div class="cegg-no-top-margin cegg-list-logo-title">
<img src="https://testingcf.jsdelivr.net/gh/jarlin8/OSS@main/icons/favicon/<?php echo esc_attr( $item['domain']); ?>.svg" height="18" width="18">
----
<img src="https://laowei8.com/favicon/get.php?url=<?php echo esc_attr( $item['domain']); ?>" height="18" width="18">

fccm: 使用https://icon.horse/icon/
```

### 6.替换全部 favicon 的来源

```plain text
https://www.google.com/s2/favicons?domain=  >>  https://icon.horse/icon/
<img src="https://fastly.jsdelivr.net/gh/jarlin8/OSS@main/icons/favicon/<?php echo esc_attr( $item['domain']); ?>.svg" height="18" width="18">
```

## AffiliateEGG PRO

### 1.解除限制 A

**affiliate-egg/application/admin/LicConfig.php**:76

```plain text
public function licFormat($value)    {
if (preg_match('/[^0-9a-zA-Z_~-]/', $value))
return false;
if (strlen($value) !== 32 && !preg_match('/^w{8}-w{4}-w{4}-w{4}-w{12}$/', $value))
return false;
return true;  }
public function activatingLicense($value)    {
return true;
$response = AffiliateEgg::apiRequest(array('method' => 'POST', 'timeout' => 15, 'httpversion' => '1.0', 'blocking' => true, 'headers' => array(), 'body' => array('cmd' => 'activate', 'key' => $value, 'd' => parse_url(site_url(), PHP_URL_HOST), 'p' => AffiliateEgg::product_id, 'v' => AffiliateEgg::version()), 'cookies' => array()));
if (!$response)
return false;
$result = json_decode(wp_remote_retrieve_body($response), true);
if ($result && !empty($result['status']) && $result['status'] === 'valid')
return true;
else
return false;
}
```

替换成

```plain text
public function licFormat($value)    {
return true;
}public function activatingLicense($value)    {
return true;
}
```

### 2.解除限制 B

**affiliate-egg/application/admin/LManager.php**:302

```plain text
public static function isNulled()​    {​
$l = LicConfig::getInstance()->option('license_key');​​
if (!$l && Plugin::isEnvato())​
return false;​​
if (!LManager::isValidLicFormat($l))​
return true;
if (in_array(md5($l), LManager::getNulledLics()))​
return true;​​
return false;​}
```

替换成

```plain text
public static function isNulled(){
return false;​ }
```

## Linux 批量删除.DS_Store

```plain text
find . -name ".DS_Store" -print -delete
find . -name "*.log" -print -delete
```

## 可获取网站 favicon 的链接（国内）

```plain text
https://f5.allesedv.com/16/google.com （部分网址无法获取）
https://api.faviconkit.com/amazon.cn  (速度较慢，几乎全部可获取到)
https://icon.horse/icon/alibaba.com (全 免费 较快 推荐)
```

## 阿里 OSS 静态存储-删除 js/css 外所有格式文件

win+R -> cmd -> `cd C:UsershankOneDrive - teleworm桌面img`

```plain text
del /a /f /s /q  "*.DS_Store"
del /a /f /s /q  "*.editorconfig"
del /a /f /s /q  "*.gitattributes"
del /a /f /s /q  "*.gitignore"
del /a /f /s /q  "*.gitkeep"
del /a /f /s /q  "*.gitmodules"
del /a /f /s /q  "*.htaccess"
del /a /f /s /q  "*.jsdtscope"
del /a /f /s /q  "*.project"
del /a /f /s /q  "*.yml"
del /a /f /s /q  "*.xlsx"
del /a /f /s /q  "*.xls"
del /a /f /s /q  "*.html"
del /a /f /s /q  "*.phtml"
del /a /f /s /q  "*.po"
del /a /f /s /q  "*.mo"
del /a /f /s /q  "*.pot"
del /a /f /s /q  "*.serialized"
del /a /f /s /q  "*.ico"
del /a /f /s /q  "*.ts"
del /a /f /s /q  "*.sql"
del /a /f /s /q  "*.cd"
del /a /f /s /q  "*.csproj"
del /a /f /s /q  "*.zip"
del /a /f /s /q  "*.gz"
del /a /f /s /q  "*.sh"
del /a /f /s /q  "*.pem"
del /a /f /s /q  "*.crt"
del /a /f /s /q  "*.user"
del /a /f /s /q  "*.sln"
del /a /f /s /q  "*.lock"
del /a /f /s /q  "*.m4"
del /a /f /s /q  "config"
del /a /f /s /q  "*.rb"
del /a /f /s /q  "*.w32"
del /a /f /s /q  "*.ini"
del /a /f /s /q  "*Dockerfile"
del /a /f /s /q  "*.mp3"
del /a /f /s /q  "*.html_gzip"
del /a /f /s /q  "*LICENCE"
del /a /f /s /q  "*.xml"
del /a /f /s /q  "*.woff"
del /a /f /s /q  "*.ttf"
del /a /f /s /q  "*.eot"
del /a /f /s /q  "*.php"
del /a /f /s /q  "*.png"
del /a /f /s /q  "*.jpg"
del /a /f /s /q  "*.jpeg"
del /a /f /s /q  "*.svg"
del /a /f /s /q  "*.webp"
del /a /f /s /q  "*.gif"
del /a /f /s /q  "*.swf"
del /a /f /s /q  "*.babelrc"
del /a /f /s /q  "*.buildpath"
del /a /f /s /q  "*cleanup"
del /a /f /s /q  "*COPYING"
del /a /f /s /q  "*create-pear"
del /a /f /s /q  "*create-phar"
del /a /f /s /q  "*.po~"
del /a /f /s /q  "*.csv"
del /a /f /s /q  "*.psd"
del /a /f /s /q  "*functions"
del /a /f /s /q  "*.woff2"
del /a /f /s /q  "*LICENSE"
del /a /f /s /q  "*.conf"
del /a /f /s /q  "*.iml"
```

## thrive 插件的下载更新

`# thrive leadshttp://download.thrivethemes.com/thrive-leads-3.6.zip​# thrive-architecthttp://download.thrivethemes.com/thrive-architect-3.8.zip​Thrive Plugins Package Nulled - April 6, 2022​Thrive Automator v0.9Thrive Optimize v2.6Thrive Comments v2.6Thrive Clever Widgets v2.9.1Thrive Headline Optimizer v2.3.1Thrive Ovation v3.6Thrive Leads v3.6Thrive Ultimatum v3.6Thrive Quiz Builder v3.6Thrive Apprentice v4.2Thrive Architect v3.8`

## DeepLink 链接汇总

### impact 加入的商家

* bluehost `https://bluehost.sjv.io/c/2469506/795082/11352?u={{url_encoded}}`

* bluehost(旧) [https://www.bluehost.com/track/jarlin8/](https://www.bluehost.com/track/jarlin8/) 【[后台](https://www.bluehost.com/hosting/partner)】

* Envato(卖 wordpress 主题插件 etc) `https://1.envato.market/c/2469506/275988/4415?u={{url_encoded}}`

* Hostinger `https://hostinger.sjv.io/c/2469506/888231/12282?u={{url_encoded}}`

* Hostgator(恐龙) `https://partners.hostgator.com/c/2469506/177309/3094?u={{url_encoded}}`

* InMotion `https://partners.inmotionhosting.com/c/2469506/260033/4222?u={{url_encoded}}`

* LiquidWeb `https://liquidweb.i3f2.net/c/2469506/278394/4464?u={{url_encoded}}`

* NameCheap `https://namecheap.pxf.io/c/2469506/386170/5618?u={{url_encoded}}`

* SiteGround `https://www.siteground.com/index.htm?afcode=7dca1a3d149d92812da634af29bdad6b` (`{{url}}?afcode=7dca1a3d149d92812da634af29bdad6b`)

* Justhost `https://www.justhost.com/track/jialinwei/`

* interServer [https://www.interserver.net/r/667363](https://www.interserver.net/r/667363) [[https://www.interserver.net/r/667363?url={{url_encoded](https://www.interserver.net/r/667363?url=%7B%7Burl_encoded)}}]

* HostPapa [https://tracking.opienetwork.com/aff_c?offer_id=437&aff_id=16860&file_id=1313](https://tracking.opienetwork.com/aff_c?offer_id=437&aff_id=16860&file_id=1313)

* A2hosting [http://www.a2hosting.com?aid=jarlinwei&cid=edae5de3](http://www.a2hosting.com/?aid=jarlinwei&cid=edae5de3)

* Hostwinds `https://www.hostwinds.com/10193.html`[ [https://affiliates.hostwinds.com/hostwinds.php?id=10193](https://affiliates.hostwinds.com/hostwinds.php?id=10193) ]

* WISE: `https://wise.prf.hn/click/camref:1101lqnyb/destination:{{url_encoded}}`

* tradingview 后台【`[https://tradingview.hasoffers.com/](https://tradingview.hasoffers.com/)】 [https://www.tradingview.com/?offer_id=10&aff_id=22792](https://www.tradingview.com/?offer_id=10&aff_id=22792)`

* shareasale: [https://www.shareasale.com/r.cfm?b=40&u=2789158&m=47](https://www.shareasale.com/r.cfm?b=40&u=2789158&m=47)

### 亚马逊

Amazon `tag=jarlin-20`

### FIVERR

* [fiverr.com](https://affiliates.fiverr.com/) (混合模式)`https://go.fiverr.com/visit/?bta=498363&brand=fiverrhybrid&landingPage={{url_encoded}}`

* sub-affiliate `https://go.fiverr.com/visit/?bta=498363&brand=fiverraffiliates&landingPage={{url_encoded}}`

### VULTR

vultr.com `ref=9197180-8H`

## flatsome 主题添加最后更新时间

```plain text
#: inc/structure/structure-posts.php:235
msgctxt "post date"
msgid "Posted on %s"
msgstr "最后更新于：%s"
//修改方法
将236行中的  . $time_string .  替换为 . get_the_modified_time('Y-n-d G:i') .
```