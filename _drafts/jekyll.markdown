---
layout: post
title: Jekyll 摘要
date: 2020-01-15 00:00:00 +8000
categories:
tags: [Jekyll]
---

## 在 Windows 上安装

Requirements [Permalink](https://jekyllrb.com/docs/installation/#requirements)

- [Ruby](https://www.ruby-lang.org/en/downloads/) version 2.4.0 or above, including all development headers (ruby version can be checked by running ruby -v)
- [RubyGems](https://rubygems.org/pages/download) (which you can check by running gem -v)
- [GCC](https://gcc.gnu.org/install/) and [Make](https://www.gnu.org/software/make/) (in case your system doesn’t have them installed, which you can check by running gcc -v,g++ -v and make -v in your system’s command line interface)

### GCC 的安装

- <http://www.mingw.org>
- <https://www.jianshu.com/p/ff24a81f3637>

## 关于 Jekyll

Jekyll 是使用 Ruby 实现的。

Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是：你用你最喜欢的标记语言来写文章，可以是 Markdown, 也可以是 Textile, 或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置 URL 路径，你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

### Gems

一个 gem 就是可以包含在 Ruby 项目中的代码。gem 可以实现类似以下功能：

1. 转换一个 Ruby 对象为 JSON
1. 分页功能
1. 与 GitHub 的 APIs 通信

Jekyll 自身和 Jekyll 的插件包括 jekyll-feed，jekyll-seo-tag 和 jekyll-archives 都是 gem。

### Gemfile

一个 Gemfile 包含了一个站点需要的 gem 列表。示例：

```ruby

source "https://rubygems.org"

gem "jekyll"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
end

```

### Bundler

Bundler 用来安装包含在 Gemfile 中的 gems。

```bash
# 安装 Bundler
gem install bundler

# 初始化 Gemfile
bundle init

# 安装 gems
bundle install

# 编译站点
bundle exec jekyll serve
```

## 关于 Liquid

Liquid 是一个模板语言，主要包含三个部分：objects、tags 和 filters。

### Objects

Objects 告诉模板在什么地方输出内容。使用 \{\{ \}\} 表示。
示例：

```html
{{ page.title }}
```

### Tags

Tags 用来创建和控制模板的流程。使用 \{\% \%\} 表示。
示例：

```html
{% if page.show_sidebar %}
<div class="sidebar">
  sidebar content
</div>
{% endif %}
```

### Filters

Filters 用来修改输出格式。
示例：

```html
{{ "hi" | capitalize }}
```

## 关于 Front Matter

Front Matter 是 YAML 的片段，在文件顶部使用两个 --- 行包裹。通常用来设置页面的变量。
示例：

```yaml
---
my_number: 5
---

```

Front Matter 变量在 Linquid 作为 page 的属性

```html
{{ page.my_number }}
```

## 其他 Jekyll 脚本

```bash
# 创建 Blog
jekyll new myblog
```
