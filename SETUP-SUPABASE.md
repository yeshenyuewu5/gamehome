# 🚀 Supabase + 非 Google 全套方案 - 搭建指南

> 方案：Supabase + Plausible + Bing Webmaster + Ezoic

---

## 第 1 步：Supabase 设置（5 分钟）

### 1.1 创建项目
1. 打开 https://supabase.com
2. 点击 **New Project**
3. 填写：
   - Organization: 选择或创建新的
   - Name: `toolhubb`
   - Database Password: 设置强密码
   - Region: **US** (美国节点，全球快)
4. 点击 **Create new project**

### 1.2 获取凭证
创建完成后：
1. 进入 **Project Settings** → **API**
2. 复制以下信息：
   - `Project URL`（格式：`https://xxxxx.supabase.co`）
   - `anon public` key

### 1.3 创建数据表

在 Supabase 后台 **SQL Editor** 中执行：

```sql
-- 创建 games 表
CREATE TABLE games (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  name_zh TEXT,
  description TEXT,
  description_zh TEXT,
  image_url TEXT,
  tags TEXT[],
  tags_zh TEXT[],
  badge TEXT,
  guide_count INTEGER DEFAULT 0,
  display_order INTEGER DEFAULT 0,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 创建 guides 表
CREATE TABLE guides (
  id TEXT PRIMARY KEY,
  game_id TEXT REFERENCES games(id),
  title TEXT NOT NULL,
  title_zh TEXT,
  thumbnail_url TEXT,
  read_time INTEGER DEFAULT 5,
  published_date TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 插入示例数据
INSERT INTO games (id, name, name_zh, description, description_zh, image_url, tags, tags_zh, badge, guide_count, display_order) VALUES
('rust', 'Rust', 'Rust', 'Survival building, base layouts, loot routes & anti-raid strategies.', '生存建造、建家布局、物资路线、防抄家教程。', 'https://images.unsplash.com/photo-1511512578047-dfb367046420?w=600&q=80', ARRAY['Guide', 'Survival'], ARRAY['攻略', '生存'], 'hot', 24, 1),
('palworld', 'Palworld', '幻兽帕鲁', 'Breeding combos, base optimization, fast resource farming & boss tactics.', '配种攻略、基地布局、快速刷资源及BOSS打法。', 'https://images.unsplash.com/photo-1614786269829-d24616faf56d?w=600&q=80', ARRAY['Breeding', 'Base'], ARRAY['配种', '基地'], 'hot', 18, 2),
('valorant', 'Valorant', '无畏契约', 'Agent abilities, map callouts, crosshair settings & ranked climbing strategies.', '角色技能、地图战术、准星设置、段位上分技巧。', 'https://images.unsplash.com/photo-1542751371-adc38448a05e?w=600&q=80', ARRAY['FPS', 'Competitive'], ARRAY['射击', '竞技'], 'new', 32, 3),
('cs2', 'CS2', 'CS2', 'Grenade lineups, movement mechanics, weapon stats & competitive tips.', '道具投掷、身法技巧、枪械数据、各段位攻略。', 'https://images.unsplash.com/photo-1493711662062-fa541adb3fc8?w=600&q=80', ARRAY['Tactical', 'Shooter'], ARRAY['战术', '射击'], '', 28, 4),
('freefire', 'Free Fire', 'Free Fire', 'Headshot sensitivity, drop strategies, best characters & advanced settings.', '灵敏度设置、跳伞落点、最佳角色、枪法进阶设置。', 'https://images.unsplash.com/photo-1538481199705-c710c4e965fc?w=600&q=80', ARRAY['Mobile', 'Battle Royale'], ARRAY['手游', '吃鸡'], 'hot', 15, 5),
('satisfactory', 'Satisfactory', '幸福工厂', 'Factory layouts, production chains, biomass setup & efficient blueprints.', '流水线搭建、量产自动化、资源布局、高效生产蓝图。', 'https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=600&q=80', ARRAY['Automation', 'Factory'], ARRAY['自动化', '工厂'], '', 12, 6);

INSERT INTO guides (id, game_id, title, title_zh, thumbnail_url, read_time, published_date) VALUES
('g1', 'rust', 'Rust Best Base Layouts 2026 — Anti-Grief Edition', 'Rust 2026最佳建家布局 — 防抄家版', 'https://images.unsplash.com/photo-1511512578047-dfb367046420?w=120&q=80', 12, NOW()),
('g2', 'palworld', 'Palworld Breeding Guide: Best Pals Combos', '帕鲁配种攻略：最佳组合', 'https://images.unsplash.com/photo-1614786269829-d24616faf56d?w=120&q=80', 8, NOW()),
('g3', 'valorant', 'Valorant Crosshair Settings: Pro Configs', '瓦罗兰准星设置：职业选手配置', 'https://images.unsplash.com/photo-1542751371-adc38448a05e?w=120&q=80', 5, NOW()),
('g4', 'cs2', 'CS2 Smoke Lineups: 12 Pro Spots', 'CS2烟点火线：12个职业投点', 'https://images.unsplash.com/photo-1493711662062-fa541adb3fc8?w=120&q=80', 15, NOW()),
('g5', 'freefire', 'Free Fire Sensitivity: Headshot Masters', 'Free Fire灵敏度：头牌大师配置', 'https://images.unsplash.com/photo-1538481199705-c710c4e965fc?w=120&q=80', 4, NOW());
```

### 1.4 填入 index.html

打开 `index.html`，找到顶部：

```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

替换为你复制的 URL 和 Key。

---

## 第 2 步：部署到 Vercel（3 分钟）

```powershell
cd C:\Users\1\.qclaw\toolhubb
git add .
git commit -m "v1.0 with Supabase"
git push
```

然后在 Vercel 导入 GitHub 仓库，自动部署。

---

## 第 3 步：Bing Webmaster（2 分钟）

1. 打开 https://www.bing.com/webmasters
2. 用 Microsoft 账号登录
3. 点击 **Add Site**，填入 `toolhubb.top`
4. 验证方式：DNS TXT 记录 或 HTML 文件上传
5. 验证通过后，提交 sitemap：`https://toolhubb.top/sitemap.xml`

---

## 第 4 步：Plausible 统计（可选）

1. 打开 https://plausible.io
2. 注册 → 添加网站 `toolhubb.top`
3. 获取统计代码，添加到 `</head>` 前：

```html
<script defer data-domain="toolhubb.top" src="https://plausible.io/js/script.js"></script>
```

---

## 第 5 步：Ezoic 广告（需要流量后申请）

1. 打开 https://www.ezoic.com
2. 注册账号（用 Email，不需要 Google）
3. 添加网站 `toolhubb.top`
4. 按照指引添加广告代码到网站

**注意：** Ezoic 需要网站有一定流量后才能申请通过。建议先运营 1-2 个月，有一定 UV 后再申请。

---

## ✅ 完成检查清单

- [ ] Supabase 项目创建完成
- [ ] games 和 guides 表创建并有数据
- [ ] index.html 填入 URL 和 Key
- [ ] Vercel 部署成功
- [ ] Bing Webmaster 验证通过
- [ ] sitemap.xml 提交成功
- [ ] Plausible 统计代码添加（可选）
- [ ] Ezoic 广告申请（可选）

---

## 💡 日常更新流程

更新内容时：
1. 登录 Supabase 后台
2. 直接在表格中编辑 games 或 guides
3. 保存后网站自动刷新（1-2秒）

无需再改代码或重新部署！
