## 	robots.txt

### 概述

robots.txt是网站管理者写给爬虫的一封信，里面描述了网站管理者不希望爬虫做的事，比如

* 不要访问某个文件、文件夹
* 禁止某些爬虫的访问
* 限制爬虫访问网站的频率

如果爬虫遵守规定，那么爬虫在抓取页面之前，会先阅读 robots.txt 协议，然后了定制的爬虫策略

### 规则

User-agent 爬取者名称

```
User-agent: * // 允许全部
User-agent: Googlebot // 谷歌的爬虫
User-agent: Baiduspider // 百度的爬虫
User-agent: YoudaoBot // 有道的爬虫
```

Disallow 不允许爬取的路径

```
Disallow: /baidu
Disallow: /s?
Disallow: /shifen/
Disallow: /homepage/
Disallow: /cpro
Disallow: /ulink?
Disallow: /link?
```

Allow 允许爬取的路径

```
Allow: /api/maps/
Allow: /shop$
Allow: /shop?
Allow: /shop/
Allow: /stores$
Allow: /stores/ 
```

完整示例：截取部分 cn.bing.com/robots.txt

```
User-agent: msnbot-media
Disallow: /
Allow: /th?

User-agent: Twitterbot
Disallow:

User-agent: *
Disallow: /account/
Disallow: /amp/
Allow: /api/maps/
Disallow: /api/
Disallow: /bfp/search
Disallow: /bing-site-safety
Disallow: /blogs/search/
```

### 文件放到哪里

放到根路径下，比如

```
http://example.com/robots.txt
```



## 参考

百度的robots.txt：https://baidu.com/robots.txt

必应的robots.txt：https://cn.bing.com/robots.txt

BiliBili的robots.txt：https://bilibili.com/robots.txt

