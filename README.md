# Mizuki 博客内容仓库

这是 Mizuki 博客的内容仓库，采用**内容代码分离**架构，独立管理所有文章、数据和图片。

## 📁 目录结构

```
Mizuki-Content/
├── posts/              # 博客文章 (Markdown)
│   ├── *.md            # 单文件形式文章
│   └── guide/          # 文件夹形式文章
├── spec/               # 特殊页面
│   ├── about.md        # 关于页面
│   └── friends.md      # 友链页面
├── data/               # 结构化数据
│   ├── anime.ts        # 番剧数据
│   ├── devices.ts      # 设备数据
│   ├── diary.ts        # 日记数据
│   ├── friends.ts      # 日记数据
│   ├── projects.ts     # 项目展示
│   ├── skills.ts       # 技能数据
│   └── timeline.ts     # 时间线
├── images/             # 图片资源
│   ├── posts/          # 文章配图
│   ├── albums/         # 相册图片
│   └── diary/          # 日记图片
└── .github/
    └── workflows/      # 自动构建触发器
```

## 🚀 快速开始

### 推荐阅读

1. **[快速参考](docs/QUICK_REFERENCE.md)** - 5 分钟上手 ⚡
2. **[文章编写指南](docs/WRITING_GUIDE.md)** - 详细写作说明 📝
3. 参考 `posts/` 目录下的示例文章

### 发布新文章

1. 在 `posts/` 目录创建新的 Markdown 文件
2. 添加 Frontmatter (文章元数据)
3. 编写文章内容
4. 提交并推送

```bash
# 创建新文章
cd posts/
# 编辑文章...

# 提交
git add .
git commit -m "post: 添加新文章"
git push
```

### 更新数据

编辑 `data/` 目录下的对应文件：
- `anime.ts` - 追番列表
- `projects.ts` - 项目展示
- `skills.ts` - 技能树
- `timeline.ts` - 个人时间线

### 添加图片

将图片放到对应目录：
- 文章配图 → `images/posts/`
- 相册图片 → `images/albums/`
- 日记图片 → `images/diary/`

## 🔄 自动部署

### 问题

推送内容后，站点不会自动更新？

### 解决

配置自动构建触发器，让内容更新时自动重新部署站点。

**5 步快速配置** → 查看 [.github/workflows/README.md](.github/workflows/README.md)

配置完成后，每次推送都会自动触发代码仓库的重新部署！✨

## 📝 文章编写指南

### Frontmatter 格式

每篇文章需要在开头添加 Frontmatter：

```markdown
---
title: 文章标题
published: 2024-01-01
description: 文章描述
image: /images/posts/cover.jpg
tags: [标签1, 标签2]
category: 分类
draft: false
---

文章正文内容...
```

### 必填字段

- `title` - 文章标题
- `published` - 发布日期 (YYYY-MM-DD)
- `description` - 文章描述 (用于 SEO)

### 可选字段

- `image` - 封面图片
- `tags` - 标签数组
- `category` - 分类
- `draft` - 是否草稿 (true/false)
- `lang` - 语言 (zh-cn/en/ja)

### 示例

参考 `posts/` 目录下的示例文章。

## 🔗 关联关系

此内容仓库与代码仓库的关联方式：

### Submodule 模式 (推荐)

代码仓库通过 Git Submodule 引用此仓库：
```bash
# 在代码仓库中
git submodule add <this-repo-url> content
```

### 独立模式

代码仓库在构建时自动克隆此仓库的内容。

## 📚 更多文档

### 本仓库文档

- **[快速参考](docs/QUICK_REFERENCE.md)** - 常用操作速查卡片 ⚡
- **[文章编写指南](docs/WRITING_GUIDE.md)** - 详细的写作指南 📝
- **[自动构建触发配置](.github/workflows/README.md)** - 自动部署配置 🔄

### 代码仓库文档

- [Mizuki 主文档](https://github.com/matsuzaka-yuki/Mizuki) - 项目总览
- [部署指南](https://github.com/matsuzaka-yuki/Mizuki/blob/main/docs/DEPLOYMENT.md) - 部署配置
- [内容分离指南](https://github.com/matsuzaka-yuki/Mizuki/blob/main/docs/CONTENT_SEPARATION.md) - 完整说明

## 💡 提示

### 多人协作

1. 添加协作者: Settings → Collaborators
2. 设置分支保护: Settings → Branches
3. 使用 Pull Request 进行内容审核

### 私有内容

如果设为私有仓库：
- 代码仓库需要配置 SSH 密钥或 Token
- 详见 [内容分离指南 - 私有仓库配置](https://github.com/matsuzaka-yuki/Mizuki/blob/main/docs/CONTENT_SEPARATION.md#-私有仓库配置)

## 🤝 需要帮助？

- 查看 [代码仓库文档](https://github.com/matsuzaka-yuki/Mizuki)
- 提交 [Issue](https://github.com/matsuzaka-yuki/Mizuki/issues)
- 阅读 [自动构建配置](.github/workflows/README.md)

---

**Happy Writing!** ✨

## 番剧数据（已迁移到 content）

- 追番数据：`content/data/anime.ts`
- 封面目录：`content/images/anime/`
- `cover` 写法：`/images/anime/<文件名>`
- 详细说明：`docs/ANIME_GUIDE.md`
