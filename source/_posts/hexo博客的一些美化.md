---
title: hexo博客的一些美化
categories: hexo
description: next主题之各种优化
abbrlink: 5a20745a
date: 2020-05-19 23:10:32
tags:
comments:
---
<!--more-->

本文所用到的主题版本：`*v7.7.2版本*`

## 更改背景图

在Hexo主目录下的 `source/_data/styles.styl`中添加下方的代码

```css
body {
  background-image:url(https://gitee.com/maping666/image/raw/master/background.png);
  background-repeat: no-repeat;
  background-attachment:fixed;
  background-size:100% 100%;
}
```

## 添加动态背景

1. 基于`canvas-nest.js`  的动态背景

	 项目地址： [点我开始传送](https://github.com/hustcc/canvas-nest.js)

	中文文档： [点我开始传送](https://github.com/hustcc/canvas-nest.js/blob/master/README-zh.md)

主目录的`source/_data/footer.swig` 文件中添加下方的代码

| 参数       | 说明                                                         |
| :--------- | ------------------------------------------------------------ |
| color      | 线条颜色, 默认: '0,0,0'   三个数字分别为(R,G,B) , 注意用半角" , " 分割 |
| opacity    | 线条透明度（0~1）, 默认: 0.5                                 |
| count      | 线条的总数量, 默认: 100                                      |
| zIndex     | 背景的 z-index 属性用于控制所在层的位置, 默认: -1            |
| pointColor | 交点颜色, 默认: '0,0,0' ；三个数字分别为(R,G,B)，注意用半角" , "分割 |

``` javascript
<script color="0,0,0" opacity="0.5" zIndex="-1" count="100" src="https://cdn.jsdelivr.net/npm/canvas-nest.js@1/dist/canvas-nest.js"></script>
```

 

2. 基于`particles.js`  有丰富配置选项, 可以对漂浮线的数量, 密度,  走势, 吸引范围, 速度, 等选项进行配置, 功能强大.

	开源仓库地址： [点我开始传送](https://github.com/VincentGarreau/particles.js/)

## 文章样式美化

### 文章添加分块

在主题目录下的`source/css/_common/components/post/post.styl` 文件中找到`.use-motion ` 的`.post-block`函数  按照下面的代码进行修改

```css
.use-motion {
  if (hexo-config('motion.transition.post_block')) {
    .post-block {
   margin-top: auto; //自适应的上边距
   margin-bottom: 18px; //下边距
   padding: 25px; //填充内边距的宽度
   -webkit-box-shadow: 0 0 5px rgba(0, 0, 0, .5); //适配chrome
   -moz-box-shadow: 0 0 5px rgba(0, 0, 0, .5); //适配火狐
}
 .pagination, .comments {
      opacity: 0;
    }
  }

```

### 给分块加上自适应高度

编辑主题目录下的 `source/css/_common/components/post/post-eof.styl`  修改`margin` 的值为`auto` 即可自适应高度.   `height` 的值是`略读全文` 按钮到文章分块的距离 

```css
.post-eof {
  background: #fff0;
  height: 10px;
  margin: auto;
  text-align: center;
  width: auto;
.post-block:last-of-type & {
    display: none;
  }
}
```

### 文章底部留白高度自适应

在主题目录下  编辑`source/css/_common/components/post/post-header.styl` 文件  找到下方的代码块修改 `margin`的值为`auto`

```css
.posts-expand .post-meta {
  color: $grey-dark;
  font-family: $font-family-posts;
  font-size: $font-size-smallest;
  margin: auto; //上下，左右皆自适应
  text-align: center;

```





## 添加统计系统

### 卜算子统计

将下方的代码覆盖到主题目录下的  `layout/_third-party/statistics/busuanzi-counter.swig`  文件中

```css
{%- if theme.busuanzi_count.enable %}
<div class="busuanzi-count">
  <script{{ pjax }} async src="https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>

  {%- if theme.busuanzi_count.total_visitors %}
    <span class="post-meta-item" id="busuanzi_container_site_uv" style="display: inline;">来过本站的小可爱有
      <span class="post-meta-item-icon">
        <i class="fa fa-{{ theme.busuanzi_count.total_visitors_icon }}"></i>
      </span>
      <span class="site-uv" title="{{ __('footer.total_visitors') }}">
        <span id="busuanzi_value_site_uv"></span>个呐</span><br/>
    </span>
  {%- endif %}

  {%- if theme.busuanzi_count.total_visitors and theme.busuanzi_count.total_views %}
    <span class="post-meta-divider"></span>
  {%- endif %}

  {%- if theme.busuanzi_count.total_views %}
    <span class="post-meta-item" id="busuanzi_container_site_pv" style="display: inline;">本站一共被访问了
      <span class="post-meta-item-icon">
        <i class="fa fa-{{ theme.busuanzi_count.total_views_icon }}"></i>
      </span>
      <span class="site-pv" title="{{ __('footer.total_views') }}">
        <span id="busuanzi_value_site_pv"></span>遍</span>
    </span>
  {%- endif %}
</div>
{%- endif %}

```



在主题的配置文件中找到`busuanzi_count`  改成下面的样子即可启用卜算子统计

```
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user-circle-o
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```



### 添加cnzz 统计

将下方的代码覆盖主题目录下的 `layout/_third-party/statistics/cnzz-analytics.swig`文件,  删掉注释

```c
{%- if theme.cnzz_siteid %}
//在下面粘贴从cnzz官网复制下来的js代码
<div>
<script type="text/javascript" src="https://v1.cnzz.com/z_stat.php?id=1278918852&web_id=1278918852"></script>
</div>
{%- endif %}

```

**然后在主题目录下的配置文件`_config.yml`中的 ` cnzz_siteid:` 后面填上你获取的webid即可**

## 添加自定义友情链接

### 新增友链专用页面

在命令行输入`hexo new page links` 

### 修改主题配置文件

在主题配置文件`_config.yml`中`menu` 下添加：

```html
  links: /links/ || link
```

`/themes/next/languages/zh-CN.yml`文件中menu下增加中文描述

```
  links: 友链
```

### 新增`links.swig` 页

在主目录 输入命令行新增一个swig文件

```bash
touch /themes/next/layout/links.swig 
```

然后在往该文件写入下面的代码

```html

{% block content %}
  {######################}
  {### LINKS BLOCK ###}
  {######################}

    <div id="links">
        <style>

            #links{
               margin-top: 5rem;
            }

            .links-content{
                margin-top:1rem;
            }

            .link-navigation::after {
                content: " ";
                display: block;
                clear: both;
            }

            .card {
                width: 300px;
                font-size: 1rem;
                padding: 10px 20px;
                border-radius: 4px;
                transition-duration: 0.15s;
                margin-bottom: 1rem;
                display:flex;
            }
            .card:nth-child(odd) {
                float: left;
            }
            .card:nth-child(even) {
                float: right;
            }
            .card:hover {
                transform: scale(1.1);
                box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
            }
            .card a {
                border:none;
            }
            .card .ava {
                width: 3rem!important;
                height: 3rem!important;
                margin:0!important;
                margin-right: 1em!important;
                border-radius:4px;

            }
            .card .card-header {
                font-style: italic;
                overflow: hidden;
                width: 236px;
            }
            .card .card-header a {
                font-style: normal;
                color: #2bbc8a;
                font-weight: bold;
                text-decoration: none;
            }
            .card .card-header a:hover {
                color: #d480aa;
                text-decoration: none;
            }
            .card .card-header .info {
                font-style:normal;
                color:#a3a3a3;
                font-size:14px;
                min-width: 0;
                text-overflow: ellipsis;
                overflow: hidden;
                white-space: nowrap;
            }
        </style>
        <div class="links-content">
            <div class="link-navigation">

                {% for link in theme.mylinks %}

                    <div class="card">
                        <img class="ava" src="{{ link.avatar }}"/>
                        <div class="card-header">
                        <div><a href="{{ link.site }}" target="_blank">@ {{ link.nickname }}</a></div>
                        <div class="info">{{ link.info }}</div>
                        </div>
                    </div>

                {% endfor %}

            </div>
            {{ page.content }}
            </div>
        </div>

  {##########################}
  {### END LINKS BLOCK ###}
  {##########################}
{% endblock %}

```

### 修改page.swig

修改 `/themes/next/layout/page.swig` 文件，在

```
#}{% elif page.type === "tags" and not page.title %}{#
    #}{{ __('title.tag') + page_title_suffix }}{#
```

下面添加下面的代码

```
  <!-- 友情链接-->
  #}{% elif page.type === 'links' and not page.title %}{#
    #}{{ __('title.links') + page_title_suffix }}{#
```

继续，在最后一行添加

```
 <!-- 友情链接-->
 {% elif page.type === 'links' %}
     {% include 'links.swig' %}
```

调用刚刚添加的代码

### 最后，现在在主题的配置文件末尾处新增友链的配置

```
  mylinks:
  
  - nickname: 		#友链名称
    avatar: 		#友链头像
    site: 		#友链地址
    info:  		#友链说明
    
  - nickname: 		#友链名称
    avatar: 		#友链头像
    site: 		#友链地址
    info:  		#友链说明
  ```
  
## 添加评论系统

这里我们以`gitalk` 为例

### 在github上注册oauth应用

 [点我开始传送](https://github.com/settings/applications/new)

| 参数                       | 说明                       |
| -------------------------- | -------------------------- |
| Application name           | 应用名字，内容随意         |
| Homepage URL               | 博客域名                   |
| Application description    | 应用的简介，内容随意       |
| Authorization callback URL | 回调地址，填上博客域名就行 |
|                            |                            |

### 修改主题目录下的 `_config.yml`

```
gitalk:
  enable: true
  github_id: 3207754367 # github的用户名，只要名字
  repo: 3207754367.github.io # 指定一个用于储存评论的仓库
  client_id: 719aee584a7fec60d13d # 填上获取的 Client ID
  client_secret: 8c79f63e83dfe84de930b63258cea108a24c4985  # 填上获取的Client Secret
  admin_user: 3207754367 # 添加管理员用户名，只有管理员有权初始化评论区，你必须用这个帐号登录一次，用于初始化
  distraction_free_mode: false # Facebook-like distraction free mode
  # Gitalk's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Available values: en | es-ES | fr | ru | zh-CN | zh-TW
  language: zh-CN

```

