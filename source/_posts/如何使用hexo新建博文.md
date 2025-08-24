---
title: 如何使用hexo新建博文
date: 2025-08-24 18:40:10
tags:
---
使用hexo写一篇新博客只需 4 步：
1. 进入 Hexo 源码目录
```
cd ~/my-blog
```

2. 创建新文章

```
npx hexo new "文章标题"
# 等价于：创建 source/_posts/文章标题.md
npx hexo new draft "文章标题"   # 草稿，默认不发布
```

3. 用 Markdown 编辑器写文章或用任意 Markdown 工具打开。
```
code source/_posts/文章标题.md
```

Markdown 文件头（Front-matter）示例：
```
---
title: 文章标题
date: 2025-08-12 12:00:00
tags: [Hexo, GitHub]
categories: 技术
cover: /images/cover.jpg   # 文章封面图（可选）
---
正文写在这里……
```

4. 本地预览 → 发布
```
npx hexo clean && npx hexo generate && npx hexo server
```
浏览器访问 http://localhost:4000 确认无误。
正式发布：
```
npx hexo deploy
```
等 1 分钟，访问响应网址即可即可看到新文章。
