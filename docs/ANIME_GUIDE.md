# Anime 数据与图片位置说明

本项目已将本地追番数据迁移到 `content` 目录统一管理。

## 1. 追番数据文件

- 文件：`content/data/anime.ts`
- 作用：维护追番条目（标题、状态、评分、封面、简介、进度等）

示例：

```ts
{
  title: "BanG Dream! (Season 1)",
  status: "completed",
  cover: "/images/anime/186515_ZJhwb.jpg",
  progress: 13,
  totalEpisodes: 13,
}
```

## 2. 番剧封面图片

- 目录：`content/images/anime/`
- 访问路径：`/images/anime/<文件名>`

例如文件在：

`content/images/anime/186515_ZJhwb.jpg`

则 `anime.ts` 中应写：

`cover: "/images/anime/186515_ZJhwb.jpg"`

## 3. 与 public 目录的关系

启用内容隔离后，`content/images` 会映射到 `public/images`。

所以：
- 实际维护位置：`content/images/anime/`
- 站点访问路径：`/images/anime/...`

## 4. 旧路径说明

`/assets/anime/...` 属于旧路径，不建议继续使用。

如果后续启用 `bangumi` 自动更新脚本，请将脚本中的默认封面兜底路径也统一为 `/images/anime/default.webp`。
