# 使用 Notion 作为 CMS 的 Hugo 站点模板

迷上了 Notion，想把 Notion 作为 CMS 来用，于是就有了这个项目。

示例站点：https://rxrw.me

## 为什么

总结式发言，该项目有如下特点：

1. 使用 Notion 作为 CMS，使用 Github Action 自动发布，无需关心发布流程。
2. 无需处理图片，图片直接上传到 Notion，自动发布到 Github。
3. 默认使用 Hugo Meme 主题，可以自行修改。（但这个真的好看啊）

我不会把别人主题里的特点写在自己这里，感觉好像因为有这个项目才有这些功能一样。

这个项目的根本目的是：避免手动通过 Git 和手动处理很多事情来发布博客。

### 为什么要使用 Notion 作为 CMS

原始使用 Hugo/Hexo 发布博客的步骤如下：
1. 在本地写好 Markdown 文件
2. 手动填写好 Front Matter
3. 将图片上传到图床
4. 上传到 Github

我曾因为想写 Markdown 而又不想每次为了编辑文章的 Metadata 而苦恼万分。

但有了 Notion 之后，流程变成了：

1. 在 Notion 写好文章

之后等着 Github Action 自动发布就好了。

### 为什么要使用 Hugo

我不喜欢 Nodejs 的生态，所以我选择了 Hugo。

### 为什么要使用这个主题

这个模板大概是 Hugo 功能最全的了吧。

而且我挺喜欢这个主题的作者。（主要是模板好看）

## 如何配置

### 在 Notion 上的操作

此处因为没有将 database 设置为公开，所以需要使用 Intergration Token。作为 CMS，内容我并不认为应该公开。

1. 在 [Notion Developer](https://www.notion.so/my-integrations) 中创建一个属于自己的 Integration，记住你的 `Internal Integration Token`。

2. Duplicate [示例模板](https://rxrw.notion.site/fb04c7eebcac4e7c9cfc7a9ca23b8c3d?v=9e0f9e6e598d4dc3a031fd4248dd7750)，在右上角 ... 选择 Add connections，搜索/选择你刚刚创建的 Intergration。

3. 点击 Share，右下角的 Copy link 按钮。复制出来的链接格式应该是 https://xxx.notion.site/这里是32位的字符串?v=xxx。其中 `这里是32位的字符串` 就是你的 `Notion Database ID`。

### 在 Github 上的操作

1. Fork And Star(Star 算是对我的支持吧..) 这个项目。
2. 编辑 notionblog.config.json，将 `databaseID` 改为你的 Notion Database ID。
3. 编辑 `config.toml`，详情可以参考 [Hugo Meme 主题文档](https://github.com/reuixiy/hugo-theme-meme/blob/master/README.md)。
4. 在 Github 的 Settings 中的 Secrets 中添加 `NOTION_TOKEN`，值为你刚刚记下来的 `Internal Integration Token`。

这样一来，一切就配置好了。接下来没有了，你只需要在 Notion 中写文章就好了。

> 如果你觉得说明很复杂的话，其实——有些看起来简单的说明只是没有把所有的细节都写出来，那种看起来似乎不容易理解。所以我觉得这个说明还是挺清晰的。

### notionblog.config.json 说明

```json
{
  "databaseID": "你 Notion 数据库的 ID",
  "imagesLink": "/images", // 图片链接地址。图片是从 Notion 下载到代码库中的，所以这里需要填写图片的链接地址。
  "imagesFolder": "static/images/", // 图片存放地址。
  "contentFolder": "content/zh", // Hugo 的 content 文件夹。生成的 Markdown 文件会按照分类放在这里。
  "archetypeFile": "archetypes/notion.md", // 生成文件输出时的模板。
  "propertyDescription": "Description", // Notion 中的 Description 字段。
  "propertyTags": "Tags", // Notion 中的 Tags 字段。
  "propertyCategories": "Category", // Notion 中的 Category 字段。
  "categoryMap": { // 分类映射。这里的 key 是 Notion 中的分类，value 是 Hugo 中的分类，也就是文件夹的名称。记得要和 config.toml 中的分类保持一致。
    "技术": "tech",
    "随笔": "essay",
    "关于": "about"
  },
  "filterProp": "Status", // 过滤字段。
  "filterValue": [ // 过滤值。只有文章的过滤字段值在其中的时候才会被处理。
    "Finished ✅"
  ],
  "publishedValue": "Published 🖨", // 发布后自动更新文章状态。
  "useShortcodes": true // 生成链接时是否使用 Hugo 的 Shortcodes。
}
```
以上，你可以根据自己的需要进行修改。

## 一些问题

这些问题出自 Notion API 的限制，我也无能为力。

1. 导入历史数据（之前的文章的数据）时，文章的创建时间和更新时间不会被导入。每篇文章的创建时间都是导入的时间，更新时间是最后一次更新的时间。
2. 当文章中有 table BLOCK 时，似乎有数量错误的问题。通过 Postman 调用 Notion 的接口也是如此。可能需要等待官方修复。

## 感谢

Hugo: [gohugoio/hugo](https://github.com/gohugoio/hugo)

Hugo Meme 主题：[reuixiy/hugo-theme-meme](https://github.com/reuixiy/hugo-theme-meme)

notion-blog 原作者: [xzebra/notion-blog](https://github.com/xzebra/notion-blog)

本项目所使用的 Actions 是基于上述项目进行[二次开发重构](https://github.com/rxrw/notion-blog)的。
