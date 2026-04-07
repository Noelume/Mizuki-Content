# 快速参考

> 常用操作的速查卡片

## 📝 发布新文章

```bash
# 1. 创建文件
cd posts/
touch my-article.md

# 2. 编写内容 (添加 Frontmatter)
# 参考: docs/WRITING_GUIDE.md

# 3. 提交推送
git add .
git commit -m "post: 添加新文章"
git push
```

## 🏷️ Frontmatter 最小模板

```yaml
---
title: 文章标题
published: 2024-01-01
description: 文章描述
---
```

## 🖼️ 添加图片

```bash
# 1. 上传图片到 images/posts/
cp my-image.jpg images/posts/

# 2. 在文章中引用
![描述](/images/posts/my-image.jpg)

# 3. 提交
git add images/posts/my-image.jpg
git commit -m "assets: 添加图片"
git push
```

## 🔄 更新数据

```bash
# 编辑对应文件
data/anime.ts      # 追番列表
data/projects.ts   # 项目展示
data/skills.ts     # 技能树
data/timeline.ts   # 时间线

# 提交
git commit -m "update: 更新XX数据"
git push
```

## 🚀 常用 Git 命令

```bash
# 查看状态
git status

# 添加所有更改
git add .

# 提交
git commit -m "描述信息"

# 推送
git push

# 拉取最新
git pull

# 查看历史
git log --oneline
```

## 📋 提交信息规范

| 类型 | 说明 | 示例 |
|------|------|------|
| `post:` | 新文章 | `post: 添加《TypeScript 教程》` |
| `update:` | 更新内容 | `update: 更新关于页面` |
| `fix:` | 修复错误 | `fix: 修正文章中的拼写错误` |
| `assets:` | 添加资源 | `assets: 添加文章配图` |
| `delete:` | 删除内容 | `delete: 删除过期文章` |

## 🔗 快速链接

- [文章编写指南](docs/WRITING_GUIDE.md)
- [自动构建配置](.github/workflows/README.md)
- [主仓库文档](https://github.com/matsuzaka-yuki/Mizuki)

## 💡 常见问题

### 推送后站点没更新？

配置自动构建触发器 → [查看说明](.github/workflows/README.md)

### 图片不显示？

检查路径是否以 `/` 开头: `/images/posts/xxx.jpg` ✅

### 文章不显示？

检查 `draft: false` 且 `published` 日期不在未来

---

**需要详细说明？查看完整文档！** 📚

## 🎬 番剧数据路径（content 版本）

```bash
# 数据文件
content/data/anime.ts

# 封面图片
content/images/anime/

# anime.ts 中封面字段写法
cover: "/images/anime/your-cover.jpg"
```

更多说明：`docs/ANIME_GUIDE.md`
