---
title: Hexo 评论系统
date: 2023-04-09 19:35:58
tags: 博客搭建
description: Hexo 博客系统是静态博客，本身无法支持评论等动态的功能，但是可以通过第三方的评论系统让 Hexo 博客支持评论功能。常见的评论系统包括：Valine、Disqus、Gitment、Giscus 等，本文主要介绍 Giscus 的使用。
---

# Hexo 评论系统

上一篇文章[博客搭建教程](https://fhhw.github.io/2022/10/22/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/index.html)搭建了一个博客的基本框架，但是没有解决博客评论问题，本文填充一下这部分欠缺。

Hexo 博客系统是静态博客，本身无法支持评论等动态的功能，但是可以通过第三方的评论系统让 Hexo 博客支持评论功能。常见的评论系统包括：Valine、Disqus、Gitment、Giscus 等，本文主要介绍 Giscus 的使用。

## giscus 简介 

giscus是由 [GitHub Discussions](https://docs.github.com/en/discussions) 驱动的评论系统。让访客借助 GitHub 在你的网站上留下评论和反应，该项目受 [utterances](https://github.com/utterance/utterances) 启发。包括以下特性：

- 开源；
- 无跟踪，无广告，永久免费；
- 无需数据库，全部数据均储存在 GitHub Discussions 中；
- 支持自定义主题；
- 支持多种语言；
- 高度可配置；
- 自动从 GitHub 拉取新评论与编辑；
- 可自建服务；

### 运作原理

giscus 加载时，会使用 [GitHub Discussions 搜索 API](https://docs.github.com/en/graphql/guides/using-the-graphql-api-for-discussions#search) 根据选定的映射方式（如 URL、`pathname`、`<title>` 等）来查找与当前页面关联的 discussion。如果找不到匹配的 discussion，giscus bot 就会在第一次有人留下评论或回应时自动创建一个 discussion。

要评论，访客必须按 GitHub OAuth 流程授权 [giscus app](https://github.com/apps/giscus) [代表他发帖](https://docs.github.com/en/developers/apps/identifying-and-authorizing-users-for-github-apps)。或者访客也可以直接在 GitHub Discussion 里评论。你可以在 GitHub 上管理评论。

## 在 Hexo 中配置 giscus 

**Step 1：**新建 Github 仓库，确保：

1. 此仓库是[公开的](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/setting-repository-visibility#making-a-repository-public)，否则访客将无法查看 discussion。
2. [giscus](https://github.com/apps/giscus) app 已安装否则访客将无法评论和回应。
3. Discussions功能已[在你的仓库中启用](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository)。

**Step 2：**Hexo 配置

在你的 Hexo 博客目录中执行以下命令，安装 hexo-next-giscus 插件。

```bash
npm install hexo-next-giscus --save
```

然后在 Hexo 的 _config.yml 配置文件添加如下内容：

```yaml
giscus:
  enable: true
  repo: # Github repository name
  repo_id: # Github repository id
  category: # Github discussion category
  category_id: # Github discussion category id
  # Available values: pathname | url | title | og:title
  mapping: pathname
  # Available values: 0 | 1 
  reactions_enabled: 1
   # Available values: 0 | 1 
  emit_metadata: 1
  # Available values: light | dark | dark_high_contrast | transparent_dark | preferred-color-scheme
  theme: light
  # Available values: en | zh-C
  lang: en
  # Available value: anonymous
  crossorigin: anonymous
```

或者在主题的配置文件 _config.themename.yml 中对应项添加即可，例如 butterfly 主题：

```yml
giscus:
 repo:    # uername/repo_name
 repo_id: 
 category_id: 
 theme:
  light: light
  dark: dark
 option:
```

`repo_id`是托管博客的代码仓库的一个标识值，`category`是该仓库Issues里面对应的分类（或者说是主题）。一个仓库默认具有下面几个分类：Announcements、General、Ideas、Q&A、Show and tell。这里我选择General作为评论的分类。最后的`category_id`类似`repo_id`也是对该分类的一个标识值。

如何快速的获取这些数据呢，可以通过GitHub官方的[GraphQL API Explorer](https://docs.github.com/en/graphql/overview/explorer)查询到。这里把查询所用的语句进行记录。

```GraphQL
query {
  repository (name: "repo_name", owner: "owner_name")  {
    id
    discussionCategories (first: 5) {
      nodes {
        name
        id
      }
    }
  }
}
```

然后将查询数据填入对应项即可。

推荐使用**公告（announcements）**类型的分类，以确保新 discussion 只能由仓库维护者和 giscus 创建。

## 参考资料

[1] [Giscus](https://giscus.app/zh-CN)

[2] [Giscus的基础设置](https://www.michaeltan.org/posts/giscus/)

[3] [How does one find out one's own Repo ID?](https://stackoverflow.com/questions/13902593/how-does-one-find-out-ones-own-repo-id)

