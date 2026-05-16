# 🟢 ToolHubb 发布清单 — 一步步操作指南

> 按顺序执行，每完成一步打 ✅

---

## 第一阶段：Firebase 配置（约 10 分钟）

### 1.1 创建 Firebase 项目
- 打开 https://console.firebase.google.com
- 点击 **Add project** → 项目名：`toolhubb-cms` → 继续
- 关闭 Google Analytics（可选，跳过）→ 创建完成

### 1.2 开启 Firestore Database
- 左侧菜单 → **Build** → **Firestore Database**
- 点击 **Create database** → **Start in test mode** → Next
- 选择服务器：**us-central1**（美国节点，海外访问快）→ Enable

### 1.3 创建 games 集合
- Firestore 首页 → **Start collection** → ID 填：`games`
- 点击 **Next**，创建第一个文档：

| 字段 | 值 |
|------|-----|
| id | `rust` |
| name | `Rust` |
| nameZh | `Rust` |
| desc | `Survival building, base layouts, loot routes & anti-raid strategies.` |
| descZh | `生存建造、建家布局、物资路线、防抄家教程。` |
| img | `https://images.unsplash.com/photo-1511512578047-dfb367046420?w=600&q=80` |
| tags | `["Guide","Survival"]` |
| tagsZh | `["攻略","生存"]` |
| badge | `hot` |
| count | `24` |
| order | `1` |

- 点击 **Save**
- 继续添加剩余 5 款游戏（参考 `SEO-Content-Plan.md` 中的游戏数据）

### 1.4 创建 guides 集合
- 同样操作，集合 ID：`guides`
- 添加 5 个示例攻略文档（参考 index.html 中的 FALLBACK_GUIDES 数据）

### 1.5 获取 Firebase 配置
- ⚙️ **Project Settings**（右上角）
- 向下滚动 → **Your apps** → 点击 **</> Web** 图标
- App 名称：`toolhubb-site` → Register app
- 复制 `firebaseConfig` 对象

### 1.6 填入 index.html
打开 `toolhubb/index.html`，找到顶部区域：

```javascript
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  ...
};
```

替换为你在 1.5 复制的真实配置（6 个字段全部替换）。

---

## 第二阶段：上传 GitHub（约 5 分钟）

### 2.1 创建 GitHub 仓库
- 打开 https://github.com → 登录 → **New repository**
- Repository name: `toolhubb`
- Public / Private: **Public**（Vercel 免费部署需要 Public）
- 不勾选任何初始化选项 → Create repository

### 2.2 本地上传（PowerShell）
```powershell
cd C:\Users\1\.qclaw\toolhubb

git init
git add .
git commit -m "ToolHubb v1.0 launch"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/toolhubb.git
git push -u origin main
```

> ⚠️ 把 `YOUR_USERNAME` 替换为你的 GitHub 用户名

---

## 第三阶段：Vercel 部署（约 5 分钟）

### 3.1 连接 Vercel
- 打开 https://vercel.com → Login（用 GitHub 账号）
- 点击 **Import Project** → 选择 `toolhubb` 仓库
- **Framework Preset:** Other
- **Root Directory:** `.`（保持默认）
- **Build Command:** （留空）
- **Output Directory:** `/`
- 点击 **Deploy**

### 3.2 访问临时域名
部署完成后，Vercel 会给你一个临时域名，例如：
`toolhubb.vercel.app`

先用这个域名在浏览器打开，确认网站正常运行。

---

## 第四阶段：绑定域名（约 5 分钟）

### 4.1 添加域名
- Vercel Dashboard → 点击 `toolhubb` 项目 → **Settings** → **Domains**
- 填入：`toolhubb.top` → Add

### 4.2 DNS 配置
在你的域名服务商（阿里云/腾讯云/Namecheap）添加 DNS 记录：

| 类型 | 名称 | 值 |
|------|------|-----|
| CNAME | `toolhubb.top` | `cname.vercel-dns.com` |

> 具体操作：域名控制台 → DNS 解析 → 添加记录 → CNAME

### 4.3 等待生效
DNS 传播通常需要 **10 分钟 - 24 小时**。期间可以用 Vercel 临时域名测试。

---

## 第五阶段：Google 收录（约 5 分钟）

### 5.1 Google Search Console
- 打开 https://search.google.com/search-console
- 点击 **Add property** → 输入 `toolhubb.top`
- 选择验证方法（推荐：DNS TXT 记录 或 HTML 文件上传）
- 验证通过后，提交 sitemap：`https://toolhubb.top/sitemap.xml`

### 5.2 Google Analytics（可选）
- 打开 https://analytics.google.com
- 创建账号 → 获取 Measurement ID（如 `G-XXXXXXXXXX`）
- 在 `index.html` 的 `</head>` 前插入：

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

---

## 第六阶段：Google AdSense 申请（约 10 分钟）

### 6.1 申请条件
AdSense 审核需要：
- ✅ 网站已上线（URL 可访问）
- ✅ 网站有实质性内容（你有 8 篇攻略）
- ✅ 域名注册超过 6 个月（新域名可能拒批）
- ✅ 网站无违规内容

### 6.2 申请步骤
- 打开 https://www.google.com/adsense
- 登录 Google 账号 → 申请网站
- 填入 `toolhubb.top`
- 选择广告类型 → 提交
- Google 会在 **1-3 天** 内审核

> 💡 如果 AdSense 暂时申请不下来，先申请 **Google AdMob**（移动端广告）或 **Ezoic**（对新人更友好）作为过渡

---

## ✅ 发布后检查清单

- [ ] 网站在浏览器打开正常（https://toolhubb.top）
- [ ] 双语切换正常（EN / 中文）
- [ ] Firebase 连接成功（右上角绿灯亮起）
- [ ] 搜索功能正常
- [ ] Google Search Console 收录成功
- [ ] sitemap.xml 可访问（https://toolhubb.top/sitemap.xml）
- [ ] Google AdSense 申请已提交

---

## ⚠️ 常见问题

**Q: Firebase 连不上怎么办？**
A: 检查 FIREBASE_CONFIG 的 6 个字段是否全部填对，有没有多余空格。

**Q: Vercel 部署失败？**
A: 检查是否有 `.vercelignore` 文件或奇怪的文件夹名。常见错误：文件夹名含中文。

**Q: 域名在国内访问慢？**
A: 正常，Vercel 在海外。如果主要用户在国内，考虑用 Cloudflare Pages 或阿里云国际版。

**Q: AdSense 被拒了？**
A: 常见原因：内容不够、域名新、页面内容少。补充更多攻略内容后再申请。
