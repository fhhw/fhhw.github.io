---
title: 博客搭建教程
date: 2022-10-22 14:08:15
tags: 博客搭建
cover: /img/hexo/hexo_cover.png
description: 本教程基于 Hexo 框架 + Butterfly 主题 + GitHub Pages 搭建博客。
---
# 博客搭建教程 

本教程基于 Hexo 框架 + Butterfly 主题 + GitHub Pages 搭建博客

## 环境准备

- 安装 [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)

## 搭建步骤

### 安装 Hexo

[Hexo](https://hexo.io/zh-cn/docs/) 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

```bash
npm install -g hexo-cli
```

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
hexo init <folder>    # 初始化Hexo项目文件夹
cd <folder>
npm install           # 安装项目依赖
```

新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml      # 网站的配置信息
├── package.json     # 应用程序的信息
├── scaffolds        # 模版文件夹,当新建文章时，Hexo会根据scaffold来建立文件
├── source           # 存放用户资源的地方
|   ├── _drafts      # 存放drafts文档
|   └── _posts       # 存放posts文档
└── themes           # 主题文件夹
```

### 主题

[hexo-theme-butterfly](https://butterfly.js.org/)是基于 hexo-theme-melody 的基础上进行开发的

```bash
npm i hexo-theme-butterfly
npm update hexo-theme-butterfly  # 升级命令
```

修改 Hexo 根目录下的 _config.yml，把主题改为butterfly

```yaml
theme: butterfly
```

 下载安装pug 以及 stylus 的渲染器

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

在 hexo 的根目录创建一个文件 _config.butterfly.yml，并把主题目录的 [\_config.yml](https://github.com/jerryc127/hexo-theme-butterfly) 内容复製到 _config.butterfly.yml 去，在 _config.butterfly.yml 更改主题配置。

### 部署

在本教程中，我们将会使用 [Travis CI](https://travis-ci.com/) 将 Hexo 博客部署到 GitHub Pages 上。Travis CI 对于开源 repository 是免费的，但是这意味着你的站点文件将会是公开的。

1. 新建一个 repository。如果你希望你的站点能通过域名 `<你的 GitHub 用户名>.github.io` 访问，你的 repository 应该直接命名为 `<你的 GitHub 用户名>.github.io`。

2. 将你的 Hexo 站点文件夹推送到 repository 中。默认情况下 `public` 目录将不会（也不应该）被推送到 repository 中，你应该检查 `.gitignore` 文件中是否包含 `public` 一行，如果没有请加上。

3. 将 [Travis CI](https://github.com/marketplace/travis-ci) 添加到你的 GitHub 账户中。

4. 前往 GitHub 的 [Applications settings](https://github.com/settings/installations)，配置 Travis CI 权限，使其能够访问你的 repository。

5. 你应该会被重定向到 Travis CI 的页面。如果没有，请 [手动前往](https://travis-ci.com/)。

6. 在浏览器内新建一个标签页，前往 GitHub [新建 Personal Access Token](https://github.com/settings/tokens)，只勾选 `repo` 的权限并生成一个新的 Token。Token 生成后请复制并保存好。

7. 回到 Travis CI，前往你的 repository 的设置页面，在 **Environment Variables** 下新建一个环境变量，**Name** 为 `GH_TOKEN`，**Value** 为刚才你在 GitHub 生成的 Token。确保 **DISPLAY VALUE IN BUILD LOG** 保持 **不被勾选** 避免你的 Token 泄漏。点击 **Add** 保存。

8. 在你的 Hexo 站点文件夹中新建一个 `.travis.yml` 文件：

   ```yml
   sudo: false
   language: node_js
   node_js:
     - 10 # use nodejs v10 LTS
   cache: npm
   branches:
     only:
       - master # build master branch only
   script:
     - hexo generate # generate static files
   deploy:
     provider: pages
     skip-cleanup: true
     github-token: $GH_TOKEN
     keep-history: true
     on:
       branch: master
     local-dir: public
   ```

9. 将 `.travis.yml` 推送到 repository 中。Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 `gh-pages` 分支下。

10. 在 GitHub 中前往你的 repository 的设置页面，修改 `GitHub Pages` 的部署分支为 `gh-pages`。

11. 前往 `https://<你的 GitHub 用户名>.github.io` 查看你的站点是否可以访问。这可能需要一些时间。

**注意**：

Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。这个步骤不是必须的。

#### 一键部署

1. 安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。

```bash
npm install hexo-deployer-git --save
```

1. 修改配置。

```yml
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
  token: [token]
```

token是部署中第6步产生的token，用于访问仓库，否则需要配置账号与密码。

由于 Hexo 的部署默认使用分支 `master`，所以如果你同时正在使用 Git 管理你的站点目录，你应当注意你的部署分支应当不同于写作分支。一个好的实践是将站点目录和 Pages 分别存放在两个不同的 Git 仓库中，可以有效避免相互覆盖。Hexo 在部署你的站点生成的文件时并不会更新你的站点目录。因此你应该手动提交并推送你的写作分支。修改上述branch为站点目录分支，同时需要修改仓库的部署分支。

3. Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上。

   ```bash
   hexo deploy
   ```


## 目录结构

**source** 目录用户资源文件，是项目的根文件 / ，所有文章、页面、图片、音视频等资源都放在该目录下，应用该目录下的文件，文件路径从根目录 / 开始。

**scaffolds** 目录存放模版文件，当新建文章时，Hexo会根据scaffold来建立文件。

Hexo 有三种默认布局（模版）：`post`、`page` 和 `draft`。在创建这三种不同类型的文件时，它们将会被保存到不同的路径；自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

| 布局    | 路径             |
| :------ | :--------------- |
| `post`  | `source/_posts`  |
| `page`  | `source`         |
| `draft` | `source/_drafts` |

**public** 目录是Hexo框架根据源文件产生的静态网站文件，部署时，需要将 **public** 目录中的所有内容上传到网站。

## 博客写作

执行下列命令来创建一篇新文章或者新的页面：

```bash
hexo new [layout] <title>
```

可以在命令中指定文章的布局（layout），默认为 `post`，可以通过修改 `_config.yml` 中的 `default_layout` 参数来指定默认布局。

通常 post 布局就是我们的博文模板，而 page 布局用来定义tags、categories、links、about等页面。

在 _config.butterfly.yml文件中定义菜单栏：

```yaml
menu:
  首页: / || fas fa-home
  目录||fas fa-list:
    时间轴: /archives/ || fas fa-archive
    标签: /tags/ || fas fa-tags
    分类: /categories/ || fas fa-folder-open
  链接: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

需要定义相应的 page 页面：

```
hexo new page tags
hexo new page categories
```

接下来修改生成的 index.md 文件中的类型（type）：

```markdown
---
title: 标签
date: 2022-10-22 18:46:11
type: tags
---
```

## 部署

运行下述命令生成网站静态文件：

```bash
hexo clean    # 清理之前生成的文件
hexo g        # 生成网站静态文件
hexo d        # 配置了部署信息则运行该命令，否则手动部署
hexo s        # 本地测试生成的网站文件
```

## 插件

### 搜索功能

1. 安装依赖。
    前往博客根目录，打开cmd命令窗口执行`npm install hexo-generator-search --save`。

   ```bash
   npm install hexo-generator-search --save
   ```

2. 注入配置。
    修改站点配置文件`_config.yml`，添加如下代码：

   ```yaml
   search:
     path: search.xml
     field: post
     content: true
     template: ./search.xml
   ```

3. 主题中开启搜索。
    在主题配置文件`_config.butterfly.yml`中修改以下内容：

   ```yaml
   local_search:
     enable: true
   ```

### 使用 LaTex 公式

Hexo 博客使用 LaTeX 公式有两种方法： MathJax 和 KaTeX 。其中 MathJax 功能多，但渲染时间长，且效果不如 KaTeX 。两种方法只能用一种，推荐用 KaTeX 。

Butterfly 中使用 KaTeX 步骤如下：

1. 更换插件

   ```bash
   npm un hexo-renderer-marked --save # 卸载 marked 插件
   npm un hexo-renderer-kramed --save # 卸载 kramed 插件
   npm i hexo-renderer-markdown-it --save # 安装渲染插件
   npm install @neilsustc/markdown-it-katex --save # 安装katex插件
   ```

2. 修改主题配置文件 `_config.butterfly.yml`

   ```yaml
   # KaTeX
   katex:
     enable: true
     per_page: false
     hide_scrollbar: true
   ```

3. 在博客配置文件 `_config.yml` 中追加代码：

   ```yaml
   markdown:
    plugins:
      - plugin:
        name: '@neilsustc/markdown-it-katex'
        options:
          strict: false
   ```

配置成功后，需要渲染的文章开头，添加参数 `katex: true` 即可。

> 方法缺陷：插件 `hexo-renderer-markdown-it` 在渲染的文章，一级目录无法跳转。
