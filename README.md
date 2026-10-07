# Qi Culture — 极简个人主页（静态站）

为「Chinese Philosophy & Mindfulness for Westerners」项目搭建的极简主页：
视频嵌入位 + 全文 transcript + 配套文章 + FAQ + 全套结构化数据（VideoObject / Article / FAQPage / Person）。

- **SEO**：完整 meta、OG/Twitter、canonical、sitemap.xml、robots.txt、语义化 HTML。
- **GEO（生成式引擎优化）**：AI 引擎（ChatGPT / Perplexity / Gemini / Claude）吃文字不吃视频，
  所以页面内附**全文 transcript + 配套文章 + FAQPage schema**，让 AI 能直接抓取并引用。
- **双通道托管**：GitHub Pages（国际/真源）+ Cloudflare Pages（国内镜像，可达性更好）。
  二者从同一仓库拉取，**一次 `git push` 同步更新**。

## 目录结构
```
index.html              ← 主页（视频 + 文章 + transcript + FAQ + schema）
assets/style.css        ← 样式
assets/poster.png       ← 视频封面（AI 生成）
assets/WhatIsQi_MVP_v2.mp4  ← 视频（自托管，开箱即用）
assets/5MinuteQigong.pdf     ← 免费指南（Etsy/数字产品素材）
robots.txt
sitemap.xml
.github/workflows/deploy.yml  ← GitHub Pages 一键部署
wrangler.toml           ← Cloudflare 直传配置（可选）
```

## 本地预览
```bash
cd site
python -m http.server 8080
# 打开 http://127.0.0.1:8080/
```

## 部署 A：GitHub Pages（一键）
1. 仓库 Settings → Pages → Build and deployment → Source 选 **GitHub Actions**。
2. 把本目录内容推到 `main` 分支，`deploy.yml` 会自动构建并发布。
3. 访问 `https://<用户名>.github.io/qi-culture/`。

> 推送即部署，无需手动操作。

## 部署 B：Cloudflare Pages（国内镜像）
**方式一（推荐，连 GitHub）：**
1. 登录 Cloudflare 控制台 → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**。
2. 选本仓库 → Framework preset 选 **None** → Build command 留空 → Output directory 填 `.`（根目录）。
3. 点 **Save and Deploy**。之后每次 `git push` 自动同步。
4. 可选：在 **Custom domains** 绑一个域名（如 `qi.yourdomain.com`），并把 `index.html` 里的
   `canonical` / sitemap / OG 图片地址改成该域，作为 SEO 主域。

**方式二（wrangler 直传）：**
```bash
npm i -g wrangler
cd site
wrangler pages deploy .
```

## 换平台嵌入视频（以后挂 YouTube / X）
`index.html` 里 `<video>` 上方有注释，列出 YouTube / Vimeo / Bilibili 的 iframe 替换代码。
直接替换 `<video>` 块即可，其余结构不动。

## 注意
- 视频为自托管 mp4（约 7.7MB），GitHub Pages 与 Cloudflare 均支持；如需省流量可改嵌平台。
- `sameAs` 里的 YouTube / X 链接为占位，开通后替换即可增强 Person schema 权威度。
- 国内访问 GitHub Pages 可能不稳，故用 Cloudflare Pages 作镜像；两套内容一致，canonical 指向主域避免重复内容。
