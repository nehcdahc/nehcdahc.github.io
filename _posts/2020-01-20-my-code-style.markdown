---
layout: post
title:  "我的代码规则"
date:   2020-01-20 19:29:00 +0800
categories: ["软件工程"]
---

## 规则归类

1. 【强制】
1. 【推荐】

## 命名规则

1. 【强制】专有名词保留自身的大小写
1. 【强制】包含特殊字符的优先选择顺序：英文空格 》 连字符 》 下划线 》 英文点号
1. 【推荐】英文单词能使用单数就尽量使用单数
1. 【推荐】目录能使用英文的尽量使用英文

## 文案

1. 【强制】中文文案排版，请参考《[中文文案排版指北](https://github.com/mzlogin/chinese-copywriting-guidelines)》进行中文排版。
1. 【强制】提示给用户的说明文字结尾一般不需要标点符号。如果有断句，则可以以句号结尾。不应该使用感叹号结尾。
1. 【推荐】在不支持语言包的系统中，使用英文表述。

## CHANGELOG

1. 【强制】更新日志撰写，请参考[如何维护更新日志](https://keepachangelog.com/)

## Git

1. 【强制】Git 的 commit message 必须使用英文进行撰写，即使不地道
1. 【推荐】Git 的 commit message 撰写应该要描述出所处项目的上下文环境
1. 【推荐】Git 的 commit message 应该遵循 [commitlint](https://commitlint.js.org/#/)

## 接口

1. 【强制】调用第三方接口时，先处理自己的业务流程，在最后的步骤进行事务提交。
1. 【推荐】接口的输出参数应该尽量使用强类型而不是匿名类型。

## 异常处理

1. 【强制】代码中需要避免在构造函数和析构函数中抛出异常。

## CSS

1. 【强制】CSS 书写顺序：
   1. 位置属性（position，top，right，z-index，display，float 等）
   1. 大小（width，height，padding，margin）
   1. 文字系列（font，line-height，letter-spacing，color，text-align 等）
   1. 背景（background，border 等）
   1. 其他（animation，transition 等）

## CSHARP

1. 【强制】类的私有属性使用下划线开始的 lowerCamelCase 命名。示例：

   ```c#
   /// <summary>
   /// 订单标识
   /// </summary>
   private string _orderId;
   ```

1. 【强制】方法内部变量使用 lowerCamelCase 命名。
1. 【推荐】命名空间最多保留一级在代码行中，其余的应该通过 using 的方式引入。
1. 【推荐】能给阅读带来很大的帮助时才使用命名参数【命名参数】。
   > 命名参数(Named Arguments)就是说在调用函数时可以通过指定参数名称的方式来调用参数。它最大的好处就是方便调用参数时按调用者的需要来排列顺序，而不必死守函数声明时的顺序，同时结合默认参数值的特性，可以选择使用默认参数还是不使用默认参数。坏处：不能修改被调用函数的参数名称，否则编译会出错。

## SQL

1. 【强制】SQL 语句中别名必须加 AS 关键字，以保证数据库兼容性。
1. 【强制】SQL 语句关键字大写。
1. 【强制】SQL 语句的执行必须要参数化，以防止 SQL 注入。
1. 【强制】使用 -- 进行行注释时，-- 后必须保留有且只有一个半角空格。
1. 【强制】使用 COUNT(\*)，不要使用 COUNT(1)，参考 <https://blog.jooq.org/2019/09/19/whats-faster-count-or-count1/>
1. 【推荐】SQL 语句中尽量不要做时间之类的格式化，以保证查询性能。
1. 【推荐】不要使用多重视图，原因有两个：视图上创建视图有可能会降低 SQL 的性能；DROP VIEW 时可能会报错，需要 CASECADE，维护比较麻烦。
1. 【推荐】为了聚合使用 GROUP BY；为了去重使用 DISTINCT。二者效率上应该是差不多的。

## MARKDOWN

1. 【强制】.markdownlint.yaml

   ```yml
   MD002: false # First heading should be a top level heading
   MD013: false # Line length
   MD024: { siblings_only: true } # Multiple headings with the same content
   MD040: false # Fenced code blocks should have a language specified
   MD041: false # First line in file should be a top level heading
   ```

## 修改记录

- 2020-09-04 新增了关于“异常处理”的撰写规范
- 2020-09-04 新增了关于“接口”的撰写规范
- 2020-09-04 新增了关于 commitlint 的 Git commit message 撰写要求
- 2020-04-08 新增了 CHANGELOG 撰写规范
