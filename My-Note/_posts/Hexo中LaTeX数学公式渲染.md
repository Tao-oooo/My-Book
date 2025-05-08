---
title: Hexo中LaTeX数学公式渲染
tags: [LaTeX公式渲染, 数学公式渲染]
categories: [Handbook]
date: 2025-02-04 19:06:58
---


# 概述
在没有做过任何设置的情况下，Hexo 博客文章本身并没有支持 LaTeX 公式渲染，也就是说 LaTeX 公式在博客发布后的网页上并不会显示出来，仍然是以 LaTeX 语法的形式显示。本文的目的是记录博客文章中开启 LaTeX 公式渲染的过程。

# 步骤
## 卸载原有的渲染器
想要渲染 LaTeX 公式，需要替换掉原来的渲染器。 Hexo 默认的渲染器是 `hexo-renderer-marked` ，我们需要使用如下的命令卸载此渲染器。
```bash
npm uninstall hexo-renderer-marked --save
```

## 安装必要的工具包
### 本地安装 pandoc 工具包
此工具包需要先在本地安装，工具包下载地址：[Pandoc](https://pandoc.org/getting-started.html) 。安装包也可以从这里拿到：[Pandoc](../images/Hexo中LaTeX数学公式渲染/pandoc-3.6.2-windows-x86_64.msi) 。下载安装包后，直接安装即可。
安装完成后需要重启电脑，否则后续的过程可能会出现问题。比如在最后使用 `hexo g` 命令后出现 `pandoc exited with code null` 的错误。

### 安装渲染器
在 pandoc 本地安装完成后，再安装新的渲染器 `hexo-renderer-pandoc` 。
```bash
npm install hexo-renderer-pandoc --save
```

### 安装 mathjax 插件
上述安装完成后再安装 `hexo-filter-mathjax` 插件。
```bash
npm install hexo-filter-mathjax
```

## 修改配置文件
将如下内容添加到 `_config.yml` 文件中
```yml
mathjax:
  tags: none # 或 'ams' 或 'all'
  single_dollars: true # 启用单个美元符号作为内联（行内）数学公式定界符
  cjk_width: 0.9 # 相对 CJK 字符宽度
  normal_width: 0.6 # 相对正常（等宽）宽度
  append_css: true # 将 CSS 添加到每个页面
  every_page: true # 如果为 true，那么无论每篇文章的前题中的 `mathjax` 设置如何，每页都将由 mathjax 呈现
```
`every_page` 参数为 `true` 表示默认每篇文章都会自动进行 LaTeX 公式渲染。如果设置为 `false` ，则需要在需要渲染的文章开头添加 `mathjax: true` ，如下所示，表示该文章在发布时需要被渲染。
```
title: 博客文章名词
tags: [TAG]
categories: [CAT]
mathjax: true
```

## 清理 Hexo 缓存
因为修改了 `_config.yml` 文件，因此需要让设置生效的话，需要清理 Hexo 缓存。
```bash
hexo clean
```

## 生成静态文件
最后使用如下命令生成静态文件。
```bash
hexo g
```
到这里，博客文章中的 LaTeX 公式就可以正常渲染了。

# 其他
## 关于公式编号无法显示的问题
默认情况下， `_config.yml` 文件中 `tags` 参数设置为 `none` ，只需要将其设置为 `ams` 即可解决公式编号无法显示的问题。
```yml
mathjax:
  tags: ams
```
设置完成后需要重新进行上面 1.4 和 1.5 小节的操作。
