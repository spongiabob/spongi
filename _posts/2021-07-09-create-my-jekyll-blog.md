---
title: 创建一个自己的博客
author: 
  name: nancyxia
  link: https://github.com/Spongiabob
date: 2021-01-03 18:32:00 +8
categories: [Blogging, Tool]
tags: [创建博客]
---

手把手教程，教你使用工具github+jeklly在mac上创建一个自己的[**博客**][blog]


## 环境准备

### jekyll简介

Jekyll 是一个静态站点生成器，内置对 GitHub Pages 的支持和简化的构建过程。Jekyll 采用 Markdown 和 HTML 文件，并根据您选择的布局创建一个完整的静态网站。Jekyll 支持 Markdown 和 Liquid，这是一种在您的网站上加载动态内容的模板语言。有关更多信息，请参阅J[**jekyll**][jekyll]。

Windows 不正式支持 Jekyll。有关详细信息，请参阅 Jekyll 文档中的“ Windows上的 Jekyll”。



### 安装ruby&jekyll环境

1.mac系统自带ruby环境,但是由于mac的ruby环境安装目录安装jekyll无权限，所以建议自己重新安装ruby环境，安装教程如下：
<https://www.jianshu.com/p/bbe6d6860881>(不相信可以自己踩坑试一下哦)

2.安装ruby成功(自带gem),使用如下命令进行安装jekyll
gem install  jekyll
# 图片教程
![海绵宝宝]({{"/assets/img/blog/1.jpeg"|absolute_url}}){: width="100" height="100"}

# 代码段书写教程
Now, click on the new data stream and grab the **Measurement ID**. It should look something like `G-V6XXXXXXXX`. Copy this to your `_config.yml`{: .filepath} file:

```yaml
google_analytics:
  id: 'G-V6XXXXXXX'   # fill in your Google Analytics ID
  # Google Analytics pageviews report settings
  pv:
    proxy_endpoint:   # fill in the Google Analytics superProxy endpoint of Google App Engine
    cache_path:       # the local PV cache data, friendly to visitors from GFW region
```
{: file="_config.yml"}



## Reference

[^ga-filters]: [Google Analytics Core Reporting API: Filters](https://developers.google.com/analytics/devguides/reporting/core/v3/reference#filters)

[blog]: https://spongiabob.github.io/spongi/
[jekyll]: https://jekyllrb.com/
