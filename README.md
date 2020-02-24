# nehcdahc.github.io

## 博客文章撰写规则

1. 每篇博客文章至少需要设置一个文章分类
1. 博客文章中如果需要提供外部链接，必须以下面示例格式放置在文章尾部

   ```markdown
   ## 参考

   - [...](http://...)
   - <http://...>
   - ...
   ```

1. 博客文章中如果需要修改，必须以下面示例格式将修改记录以时间倒叙放置在文章尾部

   ```markdown
   ## 修改记录

   - yyyy-MM-dd HH:mm 新增了...
   - yyyy-MM-dd HH:mm 修改了...
   - ...
   ```

1. 博客文章的占位必须配上含义的注释说明，且需要以下面示例格式撰写

   ```sql
   -- 表示占位
   -- sample_placeholder，示例占位
   { sample_placeholder, haha }

   -- 表示多个占位示例
   -- sample_placeholder1，示例占位1
   -- sample_placeholder2，示例占位2
   -- sample_placeholder3，示例占位3
   { sample_placeholder1 | sample_placeholder2 | sample_placeholder3 }

   -- 表示可选
   -- sample_placeholder，示例占位
   [{ sample_placeholder }]
   ```

<!--
1. 博客文章手动 POST 到博客园上，新闻手动 POST 到推特、新浪微博、博客园。
-->
