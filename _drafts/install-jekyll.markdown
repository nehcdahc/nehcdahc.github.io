# Install Jekyll

Table of Contents

- [Prerequisites](#prerequisites)
- [Instructions](#instructions)
- [Resources](#resources)

## Prerequisites

### Requirements

- [Requirements](https://jekyllrb.com/docs/installation/#requirements)

  - [Ruby](https://www.ruby-lang.org/en/downloads/) version 2.4.0 or above, including all development headers (ruby version can be checked by running ruby -v)
  - [RubyGems](https://rubygems.org/pages/download) (which you can check by running gem -v)
  - [GCC](https://gcc.gnu.org/install/) and [Make](https://www.gnu.org/software/make/) (in case your system doesn’t have them installed, which you can check by running gcc -v,g++ -v and make -v in your system’s command line interface —— or install them by [MinGW](http://www.mingw.org))

## Instructions

1. Install a full Ruby development environment.
1. Install Jekyll and bundler gems.

   ```bash
   gem install jekyll bundler
   ```

1. Create a new Jekyll site at ./myblog.

   ```bash
   jekyll new myblog
   jekyll new myblog --force # overwrite
   ```

1. Change into your new directory.

   ```bash
   cd myblog
   ```

1. Build the site and make it available on a local server.

   ```bash
   bundle exec jekyll serve
   ```

1. Browser the `http://localhost:4000`

## Resources

- <https://idratherbewriting.com/documentation-theme-jekyll/mydoc_install_jekyll_on_windows.html#install-ruby-and-ruby-development-kit>

  Install Jekyll on Windows | Jekyll theme for documentation

- <http://jekyllcn.com/>

  Jekyll • 简单静态博客网站生成器 - 将纯文本转换为静态博客网站

- <https://jekyllrb.com/>

  Jekyll • Simple, blog-aware, static sites | Transform your plain text into static websites and blogs
