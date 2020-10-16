---
title: hexo icarus - 主题配置
date: 2020-01-22 14:01:17
tags:
- hexo
- icarus
- blog
- 文档
toc: true
# cover: /images/477783156f942426dbff255eb669d31.png
---
摘自官方文章 [icarus用户指南-主题配置](https://blog.zhangruipeng.me/hexo-theme-icarus/Configuration/icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97-%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/) 
主题配置文件在themes文件夹下的_config.yml，文本节选介绍了icarus配置文件的常用参数配置。(侵删)
<!--more-->
## 基本布局配置

### logo
设置你站点的logo,此logo会显示在导航栏和页脚。 logo配置的值既可以是你的logo图片的路径或URL地址：
```yaml
logo: /img/logo.png
```
也可以像下面这样设置成文字：
```yaml
logo: 
    text: My Beautiful Site
```


### favicon (网址图标)
你可以在`head`配置中指定你的网站favicon的路径或URL地址。 
```yaml
head:
    favicon: /img/favicon.png
```

### Web App Manifest
Icarus支持基本的PWAmanifest.json的生成与Meta标签。 要开启web app manifest，请再主题配置中使用如下的配置。 你也可以参考MDN来了解每个配置项的详情
```yaml
manifest:
    # Web应用的名称 (默认为站点标题)
    name: Icaurs - Hexo Theme
    # Web的显示名称
    # 当没有空间显示全名时显示
    short_name: Icarus
    # Web应用的初始URL
    start_url: https://ppoffice.github.io/
    # 应用的默认主题颜色
    theme_color: "#3273dc"
    # 在应用的样式表加载之前显示的应用页默认占位背景颜色
    background_color: "#3273dc"
    # 网站首选的展示模式
    display: standalone
    # 在不同上下文下用作应用图标的图片文件
    icons:
        -
            # 图片文件的路径
            src: icons/touch-icon-iphone.png
            # 空格分割的表示图标尺寸的字符串
            sizes: 144x144
            # 图片的媒体类型提示 (可选)
            type: image/png
        -
            src: icons/touch-icon-ipad.png
            sizes: 152x152
        -
            src: icon/logo.ico
            sizes: 72x72 96x96 128x128 256x256
```

### Open Graph
你可以在`head`配置中设置Open Graph。 你应该在配置文件中将绝大部分配置留空。 仅在需要的时候在文章的front-matter中为这些设置赋值。 请参考[Hexo文档](https://hexo.io/zh-cn/docs/helpers.html#open-graph)来详细了解每个配置项。
```yaml
head:
    open_graph:
        # 页面标题 (og:title) (可选)
        # 大部分情况下请留空
        title: 
        # 页面类型 (og:type) (可选)
        # 大部分情况下请留空
        type: blog
        # 页面URL地址 (og:url) (可选)
        # 大部分情况下请留空
        url: 
        # 页面封面图 (og:image) (可选)
        # 大部分情况下请留空
        image: 
        # 站点名称 (og:site_name) (可选)
        # 大部分情况下请留空
        site_name: 
        # 页面作者 (article:author) (可选)
        # 大部分情况下请留空
        author: 
        # 页面描述 (og:description) (可选)
        # 大部分情况下请留空
        description: 
        # Twitter卡片类型 (twitter:card)
        twitter_card: 
        # Twitter ID (twitter:creator)
        twitter_id: 
        # Twitter站点 (twitter:site)
        twitter_site: 
        # Google+个人主页链接 (已弃用)
        google_plus: 
        # Facebook admin ID
        fb_admins: 
        # Facebook App ID
        fb_app_id: 
```


### 导航栏
`navbar`部分定义了导航栏中的菜单与链接。 你可以通过向menu设置项中添加<链接名>: <链接URL>的方式添加任意导航栏菜单链接。 如要向导航栏右侧添加链接，请向links设置项中添加<链接名>: <链接URL>。
```yaml
navbar:
    # 导航栏菜单项
    menu:
        Home: /
        Archives: /archives
        Categories: /categories
        Tags: /tags
        About: /about
    # 导航栏右侧的链接
    links:
        GitHub: 'https://github.com'
        Download on GitHub:
            icon: fab fa-github
            url: 'https://github.com/ppoffice/hexo-theme-icarus'
```
你也可以使用FontAwesome图标来作为纯文字链接的替换，格式如下：
```yaml
<链接名>:
    icon: <FontAwesome图标的class名>
    url: <链接URL>
```
### 页脚

`footer`部分定义了页脚右侧的链接。
链接的配置格式与`navbar`中`links`的配置格式完全一致。

```yaml
footer:
    links:
        Creative Commons:
            icon: fab fa-creative-commons
            url: 'https://creativecommons.org/'
        Attribution 4.0 International:
            icon: fab fa-creative-commons-by
            url: 'https://creativecommons.org/licenses/by/4.0/'
        Download on GitHub:
            icon: fab fa-github
            url: 'https://github.com/ppoffice/hexo-theme-icarus'
```
 
### 侧边栏

设置`sidebar`中某个侧边栏的`sticky`为`true`来让它的位置固定而不跟随页面滚动。

```yaml
sidebar:
    left:
        sticky: false
    right:
        sticky: true
```


  

## 文章相关配置
### 代码高亮

如果你已在Hexo中启用了代码高亮功能，你可以通过`article`中的`highlight`设置来自定义代码块。
请从[highlight.js/src/styles](https://github.com/highlightjs/highlight.js/tree/9.18.1/src/styles)下列出的所有主题中
选择一个主题。
然后，复制文件名(不带`.css`后缀)到`theme`设置项中。

如要隐藏复制代码按钮，将`clipboard`设置为`false`。
如果你希望折叠或展开所有代码块，将`fold`设置为`"folded"`或`"unfolded"`。
你也可以将`fold`设置为空来禁止代码块折叠。

```yaml
article:
    highlight:
        # 代码高亮主题
        # https://github.com/highlightjs/highlight.js/tree/master/src/styles
        theme: atom-one-light
        # 显示复制代码按钮
        clipboard: true
        # 代码块的默认折叠状态。可以是"", "folded", "unfolded"
        fold: unfolded
```

此外，你可以在Markdown文件中使用下面的语法来折叠单独的代码块：

```yaml
{% codeblock "可选文件名" lang:代码语言 >folded %}
...代码块内容...
{% endcodeblock %}
```

### 封面 & 缩略图

若要为文章添加封面图，请在文章的front-matter中添加`cover`选项：

```yaml
title: Icarus快速上手
cover: /gallery/covers/cover.jpg
---
Post content...
```

类似地，你也可以在文章的front-matter中为文章设置缩略图：

```yaml
title: Icarus快速上手
thumbnail: /gallery/thumbnails/thumbnail.jpg
---
Post content...
```

文章的缩略图会显示在归档页面和最新文章挂件中。

如果你在front-matter中使用的是图片的路径，你需要确保它是绝对或者相对于你的source目录的路径。
例如，为使用`<your blog>/source/gallery/image.jpg`作为缩略图，你需要在front-matter中使用`/gallery/image.jpg`作为图片路径。(建议在source中建一个存放图片文件夹比如`images`)


### 文章阅读时间

你可以将`article`部分的`readtime`设置为`true`来显示文章字数统计以及预计阅读时间。

```yaml
article:
    readtime: true
```

### 文章许可协议

你可以在你的文章/页面的底部展示你的作品的使用许可，许可链接可以是文字或者图标。
这里的配置与导航栏或者页脚的`links`配置一致：

``` yaml
article:
    # 文章许可协议
    licenses:
        Creative Commons:
            icon: fab fa-creative-commons
            url: 'https://creativecommons.org/'
        'CC BY-NC-SA 4.0': 'https://creativecommons.org/licenses/by-nc-sa/4.0/'
```


### 布局配置文件

布局配置文件遵循着与主题配置文件相同的格式和定义。
`_config.post.yml`中的配置对所有文章生效，而`_config.page.yml`中的配置对所有自定义页面生效。
这两个文件将覆盖主题配置文件中的配置。

例如，你可以在`_config.post.yml`中把所有文章变为两栏布局：

```yaml
widgets:
    -
        type: recent_posts
        position: left
    -
        type: categories
        position: left
    -
        type: tags
        position: left
```

同时所有其他页面仍保持三栏布局：

```yaml
widgets:
    -
        type: recent_posts
        position: left
    -
        type: categories
        position: right
    -
        type: tags
        position: right
```


### 文章/页面的Front-matter

如果你只想要在某个文章/页面中覆盖主题配置，你可以在那个文章/页面的front-matter中写下配置。
例如，你可以像下面这样在一篇文章的front-matter中更改某篇文章的代码高亮主题：

```yaml
title: 我的第一篇文章
date: '2015-01-01 00:00:01'
article:
    highlight:
        theme: atom-one-dark
```


### 文章标题
上面的配置会为那篇文章覆盖掉`_config.post.yml`和`_config.icarus.yml`中的`article.highlight`。
这种层次化的配置系统对于页面个性化和不同访客间的差异化优化十分有效。
比如，你可以为根据你页面目标访客的国家和语言来开启更快的CDN或本地化的评论服务。

然而需要注意的是，一些Hexo定义的文章或页面属性不会覆盖掉其他配置源中的同名配置，如
`title`, `date`, `updated`, `comments` (not `comment`), `layout`, `source`, `photos`, 和`excerpt`。





## 其他设置
- [Icarus用户指南](https://blog.zhangruipeng.me/hexo-theme-icarus/Plugins/Other/icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97-%E5%85%B6%E4%BB%96%E6%8F%92%E4%BB%B6/)

### CDN 配置
选择合适的CDN提供商可以大幅度减少网站访客的网页加载时间。
Icarus提供了几种内置的CDN提供商来承载Icaurs所用到的第三方库和资源文件的加载。
国内访问的话谷歌文字/图标被墙导致加载过慢，通过配置cdn可大幅提高页面加载速度。

默认的CDN服务提供商配置为：

```yaml
providers:
    cdn: jsdelivr
    fontcdn: google
    iconcdn: fontawesome
```
你可以通过[bootcdn](https://www.bootcdn.cn/) 查找到你需要的代理链接来替换掉原来的cdn。

`比如原先的谷歌图标国内访问不了，导致网站打开特别慢，找到个国内可访问的cdn替换掉原来的设置就解决啦。`
```yaml 
providers:
    cdn: jsdelivr
    fontcdn: google
    iconcdn: https://cdn.bootcdn.net/ajax/libs/font-awesome/5.1.0/css/all.css
```