
<p align="center">
  <span>文档：</span>
  <a href="https://hexo.fluid-dev.com/docs/guide/">主题配置</a> |
  <a href="https://hexo.io/zh-cn/docs/front-matter">文章配置</a>
</p>

<p align="center">
  <span>预览：</span>
  <a href="https://hexo.fluid-dev.com/">Fluid's blog</a> |
  <a href="https://zkqiang.cn">zkqiang's blog</a>
</p>

## 快速开始

#### 1. 搭建 Hexo 博客

```yaml
  node_modules: 依赖包
  public：存放生成的页面
  scaffolds：生成文章的一些模板
  source：用来存放你的文章
  themes：主题
  _config.yml: 博客的配置文件
```

如下修改 Hexo 博客目录中的 `_config.yml`：

```yaml
theme: fluid  # 指定主题

language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```

#### 2. 创建「关于页」

首次使用主题的「关于页」需要手动创建：

```bash
hexo new page about
```

创建成功后，编辑博客目录下 `/source/about/index.md`，添加 `layout` 属性。

修改后的文件示例如下：

```yaml
---
title: about
layout: about
---

这里写关于页的正文，支持 Markdown, HTML
```

#### 3. 创建文章

首次使用主题的「关于页」需要手动创建：

```bash
---
title: 使用Hexo搭建博客（基础版）
index_img: https://gitee.com/youlan_lan/md_image/raw/master/20210607012255.png
categories:
- 博客搭建
tags:
- 博客搭建
- node
- git
- github
---


```

#### 4. 部署到Github上

首次使用主题的「关于页」需要手动创建：

```bash

deploy:
  type: git
  # YourgithubName需要改成你的GitHub账号名
  repository: https://github.com/mickyzyf/mickyzyf.github.io.git
  # 这里是你的代码推送到的分支名
  branch: main

```

### 写在最后

1、在_posts文件夹下创建md

2、执行 hexo d 直接部署

重要的命令

```bash

    hexo clean
    hexo g --d

```
