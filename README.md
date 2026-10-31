# 92ch41的部落阁

Hugo + [Stack](https://github.com/CaiJimmy/hugo-theme-stack) 重建版。旧站是 Hexo + Icarus，源码已丢失（`hexo-deployer-git` 每次 force push 覆盖了历史），这里是从零重建的工程。

站点：https://kaiteedreamchai.github.io/

## 目录结构

```
config/_default/       # 站点配置（拆成多个文件，改哪个看哪个）
  config.toml          #   baseurl / 标题 / 语言 / 输出格式
  params.toml          #   主题参数：KaTeX 开关、侧栏组件、评论
  menu.toml            #   顶部菜单 + 侧栏社交链接
  markup.toml          #   Markdown 渲染：公式分隔符、代码高亮、TOC
  permalinks.toml      #   链接风格（保持旧的 /YYYY/MM/DD/slug/）
content/
  _index.md            # 首页
  post/<slug>/index.md # 文章（page bundle，图片放同目录）
  page/about/          # 关于页
  page/archives/       # 归档页（layout: archives）
  page/search/         # 搜索页（layout: search）
data/external.toml     # 覆盖主题的 CDN 依赖（KaTeX 已改为本地自托管）
static/vendor/katex/   # 自托管 KaTeX（css/js/fonts）
themes/hugo-theme-stack/  # 主题，已随仓库提交（不用 submodule）
.github/workflows/     # Pages 自动部署
```

## 写作

```bash
hugo new content/post/my-new-post/index.md   # 也可以直接手建目录
hugo server --port 13131                     # 本地预览 http://127.0.0.1:13131
hugo --minify --gc                           # 构建到 public/
```

文章 front matter 参考：

```yaml
---
title: 标题
description: 一句话摘要（列表卡片会显示）
date: 2026-10-31
lastmod: 2026-10-31
slug: my-new-post
categories: [物理]
tags: [电场]
draft: false
---
```

- 公式：行内 `$...$`，独立成块 `$$...$$`。构建时按需加载 KaTeX（只有含公式的页面才请求）。
- 图片：放在文章同目录下，`![](1.jpg)`，主题会生成缩略图并支持点击放大。
- 草稿：`draft: true`，本地 `hugo server -D` 可预览，不会发布。

## 配置里改什么

| 想改的东西 | 去哪 |
|---|---|
| 站点标题、域名 | `config/_default/config.toml` |
| 头像、页脚起始年份、阅读时长 | `config/_default/params.toml` |
| 顶部菜单、社交链接 | `config/_default/menu.toml` |
| 评论（giscus） | `params.toml` 里 `[comments]`，填 repo 信息后把 `enabled` 改 `true` |
| 代码高亮行号 | `markup.toml` 的 `[highlight]` |

## 换主题

Stack 之外想换别的主题，改动只有三处：

1. 把新主题放进 `themes/`，改 `config.toml` 的 `theme = "新主题名"`；
2. 按新主题的要求调整 `config/_default/*.toml`（各主题的变量名不同，通常看它的 exampleSite）；
3. `data/external.toml` 里针对 CDN 的覆盖按需删改。

因为内容是纯 Markdown + 标准 front matter，**换主题不需要动文章**。

## 部署

已配置 GitHub Actions（`.github/workflows/deploy.yml`）：推送到 `master` → 构建 → 发布到 Pages。

⚠️ **首次切站需要按顺序操作**（否则会短暂 404）：

1. GitHub 仓库 → Settings → Pages → Source 改成 **GitHub Actions**（此时线上仍是旧站）；
2. 本地提交并推送源码到 `master`；
3. 在 Actions 里确认 workflow 跑绿，再打开站点核对。

之后一切改动都只是 `git push`，历史可回滚，不再 force push。

## 本地依赖

- Hugo extended ≥ 0.157（当前 0.169.0）：`brew install hugo`
- 无需 Node / npm，无第三方依赖树

## 致谢

主题 [Stack](https://github.com/CaiJimmy/hugo-theme-stack) by Jimmy Cai（GPL-3.0）。
