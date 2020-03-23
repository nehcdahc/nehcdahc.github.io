---
layout: post
title: "Hello Scrapy"
date: 2020-03-19 17:10:00 +0800
tags: [Python, Scrapy]
categories: ["编程框架", "编程语言", "工具"]
---

## 步骤

1. 安装 Python，版本选择 Python 3，原因看这里：<https://wiki.python.org/moin/Python2orPython3>

1. 创建 virtual environment(venv)

   ```powershell
   # 在当前目录创建虚拟环境
   python -m venv .

   # 激活虚拟环境
   .\Scripts\Activate.ps1
   ```

1. 安装 pip

   ```powershell
   # 升级 pip 版本
   # -i 用来指定 pipy 源
   python -m pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```

1. 安装 Scrapy

   ```powershell
   pip install Scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple
   ```

1. 写脚本 quotes_spider.py

   ```python
   import scrapy


   class QuotesSpider(scrapy.Spider):
      name = 'quotes'
      start_urls = [
         'http://quotes.toscrape.com/tag/humor/',
      ]

      def parse(self, response):
         for quote in response.css('div.quote'):
               yield {
                  'author': quote.xpath('span/small/text()').get(),
                  'text': quote.css('span.text::text').get(),
               }

         next_page = response.css('li.next a::attr("href")').get()
         if next_page is not None:
               yield response.follow(next_page, self.parse)
   ```

1. 执行脚本

   ```python
   scrapy runspider quotes_spider.py -o quotes.json
   ```

## 参考

- <https://pip-cn.readthedocs.io/en/latest/>
- <https://mirror.tuna.tsinghua.edu.cn/help/pypi/>
- <https://docs.python.org/zh-cn/3/library/venv.html#module-venv>
