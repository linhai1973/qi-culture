# Cloudflare Pages 部署 · 傻瓜图文指引

> 目的：把同一个 GitHub 仓库 `qi-culture` 再部署到 **Cloudflare Pages**，作为**国内镜像**。
> GitHub Pages 已上线（`https://linhai1973.github.io/qi-culture/`）。Cloudflare 这一步
> **只需在网页上点几次**，之后每次 `git push` 两个站都会自动同步更新。
>
> 为什么用 Cloudflare 作镜像：GitHub 在国内访问有时不稳，Cloudflare 边缘节点可达性更好，
> 国内打开更快。两套内容一致，`canonical` 指向主域，不会被判重复内容。

---

## 前置条件（你已具备）
- ✅ 已有 GitHub 仓库 `linhai1973/qi-culture`（里面有 `index.html` 等文件）
- ✅ 有一个 Cloudflare 账号（没有就去 [cloudflare.com](https://www.cloudflare.com/) 用邮箱免费注册，**不用绑卡也能用 Pages**）

---

## 第 1 步：进 Cloudflare 控制台
1. 浏览器打开 **https://dash.cloudflare.com/** 并登录。
2. 左侧菜单找到 **「Workers 和 Pages」**（中文界面叫 *Workers 和 Pages*，英文叫 *Workers & Pages*），点进去。

## 第 2 步：创建 Pages 项目
3. 在右上角点 **「创建」**（Create）按钮。
4. 弹出选项里选 **「Pages」**。
5. 再选 **「连接到 Git」**（Connect to Git）。
   > 如果你第一次连 Git，Cloudflare 会让你授权 GitHub —— 点 **「授权 GitHub」**（Authorize GitHub），
   > 在 GitHub 弹窗里点 **绿色「Authorize Cloudflare」** 按钮即可（只授权一次）。

## 第 3 步：选仓库
6. 授权后，列表里会出现你的 GitHub 仓库。找到 **`qi-culture`**，点它（或点右侧「开始设置」Begin setup）。

## 第 4 步：构建设置（关键，照抄）
7. **项目名称（Project name）**：默认会填 `qi-culture`，不动它（或随便起，比如 `qi-culture`）。
8. **生产分支（Production branch）**：选 **`main`**。
9. **框架预设（Framework preset）**：在下拉里选 **「无 / None」**（因为它是纯静态站，不需要构建）。
10. **构建命令（Build command）**：**留空**（什么都不填）。
11. **构建输出目录（Build output directory）**：填 **`.`**（一个英文句点，表示用仓库根目录）。
    > ⚠️ 这步最重要。填 `.` 而不是 `public` 或 `dist`，否则会报“找不到输出”。
12. 其它选项保持默认，点右下角 **「保存并部署」**（Save and Deploy）。

## 第 5 步：等部署完成
13. 页面会跳到部署日志，看到 **「成功 / Success」** 或绿色对勾即完成。
14. Cloudflare 会给你一个临时域名，形如：
    `https://qi-culture.pages.dev`
    点它就能打开（这就是国内镜像，国内一般比 github.io 快）。

---

## 第 6 步（可选但推荐）：绑自己的域名
想用 `qi.yourdomain.com` 这种你自己的域名（更稳、更专业、SEO 主域）：
1. 在 Cloudflare 里点进刚建好的 **qi-culture** 项目。
2. 左侧 **「自定义域」**（Custom domains）→ **「设置自定义域」**（Set up a custom domain）。
3. 输入你的子域，例如 `qi.yourdomain.com`，点继续。
4. 按提示去你的域名服务商（阿里云/腾讯云/Namesilo 等）加一条 **CNAME 记录**：
   - 主机名：`qi`
   - 指向（值）：`qi-culture.pages.dev`（或 Cloudflare 提示的那个地址）
   - TTL：默认/600 即可
5. 等几分钟（最多几小时）变绿，就生效了。

### 绑域后要做一件事（SEO 主域）
把 `index.html` 顶部的这几处改成你的域名（用记事本搜替换即可）：
- `<link rel="canonical" href="https://linhai1973.github.io/qi-culture/" />` → 改成 `https://qi.yourdomain.com/`
- `sitemap.xml` 里的所有 `linhai1973.github.io/qi-culture` → 改成 `qi.yourdomain.com`
- OG 图片 `og:image` / Twitter 图片里的地址同理
改完 `git push` 即生效。这样 Google 会以你的域为权威源，Cloudflare 只是镜像。

---

## 日常更新（以后一直这样）
你在本地改完 `site/` 里的文件，按老三样推上去：
```bash
cd 站点目录
git add -A
git commit -m "更新内容"
git push
```
- GitHub Pages（github.io）和 Cloudflare Pages（pages.dev / 你的域名）**都会自动重新部署**。
- 一般 1–2 分钟生效。Cloudflare 有时要清一下缓存：项目里 **「缓存」→「清除缓存」**（Caching → Purge everything）点一次即可。

---

## 常见坑
| 现象 | 原因 | 解决 |
|---|---|---|
| 部署报 "No output directory" | 输出目录没填对 | 第 4 步第 11 项，填 `.` |
| 页面打开是空白/404 | 分支名不是 main | 第 4 步第 8 项确认选 `main` |
| 国内打不开 pages.dev | 个别网络对 pages.dev 也慢 | 第 6 步绑自己的域名最稳 |
| 改了内容没变 | 缓存 | Cloudflare 项目里清缓存；GitHub Pages 等 1–2 分钟 |

---

## 一句话总结
**控制台 → Workers 和 Pages → 创建 → Pages → 连接 Git → 选 qi-culture → 框架选 None、输出目录填 `.` → 保存并部署。**
就这 6 下，国内镜像就活了。
