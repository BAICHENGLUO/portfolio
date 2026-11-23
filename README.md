# 童乐 · AI美术/视频生成 作品集 — 使用与部署指南

纯静态单页网站，无需安装任何构建工具。双击 `index.html` 即可在浏览器中预览；
所有图片、视频位于 `assets/` 目录，页面通过相对路径引用。

---

## 一、目录结构

```
portfolio/
├── index.html                  # 页面入口（文案、结构、样式、脚本都在这一个文件里）
├── README.md                   # 本指南
└── assets/
    ├── portrait.jpg            # 个人证件照
    ├── characters/             # 角色设定（4 张：全身 / 头部三视图）
    ├── covers/                 # 公司项目封面（4 张）
    ├── props/                  # 道具设定（4 张）
    ├── scenes/                 # 场景概念（15 张）
    └── video/
        └── guoxu-shang.mp4     # 《故国墟上》成片
```

页面板块：个人简介 → 项目经历与原创作品 → 项目获奖与展映 → 角色设定 → 道具设定 → 场景概念。
页面已做响应式适配，手机与电脑浏览器均可正常浏览。

---

## 二、如何修改文字

用记事本 / VS Code / 任意文本编辑器打开 `index.html`，按下面位置查找修改。
**搜索中文标题即可快速定位**，改完保存，刷新浏览器即可看到效果。

### 1. 首页（求职意向、简介、标签、联系方式）
在文件前部找到 `<header class="hero">`：
- 姓名、求职意向：改 `<h1>` 内文字；
- 一句话简介：改 `<p class="hero-sub">`；
- 技能标签：改 `<div class="tags">` 里的 `<span class="tag">…</span>`；
- 电话 / 邮箱：改 `<div class="hero-contact">` 内两个 `<a class="contact-chip">`，
  其中 `tel:` / `mailto:` 后为实际号码与邮箱，同行小字为标签、大字为显示号码 / 邮箱。
- 年龄、政治面貌、坐标、到岗状态：改 `<div class="hero-meta">` 内各 `<span>`。

### 2. 关于我、经历时间线
- 「关于我」三段文字：找到 `<div class="about-text">` 直接修改；
- 工作 / 教育 / 服役经历：在底部 `<script>` 中找到 `TIMELINE` 数组，每条格式：

```js
{ time:"2026.02 – 2026.11", title:"单位 · 岗位", desc:"这段经历做了什么……" }
```

### 3. 项目经历与原创作品
脚本中两个数组：
- `FILMS_COMPANY`：公司项目经历（展示在前）；
- `FILMS_ORIGINAL`：个人原创全流程作品（展示在后）。

每条格式：

```js
{
  title:"《片名》", year:"题材 · 类型 · 公司项目 / 个人原创",
  role:"你在片中负责的工作",
  desc:"你在该项目中承担的具体职责……",
  meta:["画幅","摄影机","影调","模型"],
  chips:["播放成绩","入围 / 奖项标签"],
  type:"poster",                    // video = 内嵌播放；poster = 海报占位
  poster:"assets/scenes/xxx.jpg",   // 封面图；无图时填 null
  link:"#"                          // 公司项目：平台作品跳转链接占位
}
```

**跳转链接**：`link` 字段填写该作品在 B 站 / 微信视频号等平台的真实网址，卡片上会显示「查看平台作品 ↗」按钮；
更换平台时直接替换该网址即可。原创成片条目使用 `type:"video"` 并填 `src` 成片路径，不需要 `link`。

### 4. 项目获奖与展映
- `AWARDS_FILM`：影展、行业荣誉；
- `AWARDS_SCHOOL`：在校竞赛与服役表彰。

每条格式：

```js
{ title:"奖项名称 · 等级", small:"奖项类别 / 备注", work:"对应作品或方向" }
```

### 5. 角色 / 道具 / 场景文案
对应数组：`CHARACTERS_HUOYUAN`、`CHARACTERS_WENZHE`、`PROPS`、
`SCENES_SHUKU`（书库）、`SCENES_WAR`（《一把军号》河谷战场）、`SCENES_CITY`（《家人无法验证》近未来都市）。

每条格式：

```js
{ src:"assets/characters/xxx.jpg", title:"图片标题", desc:"一句话说明", tags:["标签1","标签2"] }
```

> 小提示：文字里的引号请用中文引号「」或 ""，不要删掉代码本身的英文引号；每行结尾保留逗号。

---

## 三、如何替换图片

### 方式 A：同名替换（最简单，不用改代码）
把新图片按**相同文件名**覆盖到 `assets/` 对应目录即可，页面路径不变。

### 方式 B：换文件名 / 新增图片
1. 把图片放进对应目录（角色 → `characters/`，场景 → `scenes/`，道具 → `props/`）；
2. 在对应数组里修改 `src` 路径，或复制一整段 `{ … }` 新增一条；
3. 保持路径写法：`assets/characters/文件名.jpg`。

### 图片规格建议
| 用途 | 长边尺寸 | 格式 / 质量 | 单张体积 |
| --- | --- | --- | --- |
| 角色三视图 | ≤ 1800 px | JPG，质量 80–85 | < 1 MB |
| 场景概念图 | ≤ 1600 px | JPG，质量 80–85 | < 1 MB |
| 证件照 | 800 px 宽即可 | JPG | < 300 KB |

- 文件名建议只用英文、数字、短横线，不要带中文或空格；
- 图片比例不限，页面会自动裁切为 16:10 缩略图，点击弹窗查看原图比例；
- 原图过大（几十 MB）会拖慢打开速度，建议先压缩再放入。

### 新增一整个分组
在场景部分，复制一段 `<div class="group-title">…</div>` + 一个 `<div class="gallery" id="grid-xxx">`，
并在 JS 中新增对应数组与一行 `renderGallery("grid-xxx", 数组名, "分组名")`。

### 弹窗查看与缩放
点击画廊图片打开大图弹窗，支持：
- **滚轮**：以中心为基准放大 / 缩小（100%–350%）；
- **按钮**：底部「＋ / −」缩放，「复位」回到原始大小；
- **双击图片**：在 100% 与 250% 之间切换；
- **拖拽平移**：放大后按住图片（或画布空白处）拖动查看细节，拖到边缘自动限位；
- **键盘**：`← / →` 同组切换，`+ / −` 缩放，`0` 复位，`Esc` 关闭。

---

## 四、如何替换 / 新增视频

### 1. 替换现有成片
把新的 MP4 覆盖 `assets/video/guoxu-shang.mp4`，或在 `FILMS_ORIGINAL` 中修改该项的 `src` 路径。

### 2. 把「海报占位」改成可内嵌播放
找到对应影片这一项：
1. `type:"poster"` 改为 `type:"video"`；
2. 补上成片路径：`src:"assets/video/你的成片.mp4"`；
3. 把 `poster` 换成封面图路径。

### 视频规格建议
- 编码 H.264 MP4（浏览器兼容性最好），竖屏 / 横屏均可；
- 单段建议 < 50 MB；过长的片子可先压缩或截取精彩片段；
- 封面图比例 16:9，放在 `assets/scenes/` 或单独 `assets/video/` 均可。

### 3. 本地没有成片时
保持 `type:"poster"`，页面仅显示封面图；拿到成片后再按第 2 步改为内嵌播放。
若无对应封面图，可把 `poster` 填 `null`，页面显示片名占位块。

---

## 五、部署到 GitHub Pages

### 1. 准备仓库
1. 登录 GitHub，点击右上角 **+ → New repository**；
2. 仓库名可填 `用户名.github.io`（地址最短），或任意名字如 `portfolio`；
3. 设为 **Public**，点击 **Create repository**。

### 2. 上传文件
确保仓库根目录结构如下（`assets` 文件夹要整个一起上传）：

```
index.html
assets/...
```

- 网页端：**Add file → Upload files**，把 `index.html` 和 `assets` 目录拖入，提交 commit；
- 或本地 Git 推送：

```bash
git init
git add .
git commit -m "AIGC portfolio"
git branch -M main
git remote add origin https://github.com/你的用户名/仓库名.git
git push -u origin main
```

### 3. 开启 Pages
1. 进入仓库 **Settings → Pages**；
2. **Source** 选择 `Deploy from a branch`；
3. **Branch** 选 `main`，目录选 `/ (root)`，点击 **Save**；
4. 等待 1–3 分钟即可访问：
   - 仓库名为 `用户名.github.io`：`https://用户名.github.io/`
   - 其他仓库名：`https://用户名.github.io/仓库名/`

### 4. 日常更新
重新上传 / 推送改动到 `main` 分支后，Pages 会自动重新部署，约 1 分钟内生效；
若页面未刷新，清除浏览器缓存后再试。

---

## 六、常见问题

| 问题 | 原因与处理 |
| --- | --- |
| 打开是 404 | 入口文件必须叫 `index.html` 且在仓库根目录；确认 Pages 分支与目录选择正确。 |
| 图片不显示 | 检查 `src` 路径拼写与大小写；确认图片已上传到 `assets/` 对应子目录；使用相对路径。 |
| 视频无法播放 | 使用 H.264 编码的 MP4；确认 `src` 路径正确；文件不要过大。 |
| 「查看平台作品」点击无反应 | `link` 仍是占位 `#`，替换为真实平台网址即可。 |
| 图片变形 | 缩略图统一按 16:10 裁切，弹窗内按原图比例显示；重要主体尽量放在画面中部。 |
| 地址打不开 | 免费账号仓库必须为 Public；刚提交需等待 1–3 分钟部署。 |
| 想换整体配色 | 在 `index.html` 顶部 `:root` 变量区修改 `--accent`（点缀色）、`--bg`（背景）等颜色值。 |
| 绑定自定义域名 | 在 Pages 设置中填写域名，并按提示在 DNS 服务商处添加 CNAME 记录。 |

## 七、本地预览

直接双击 `index.html` 即可；也可在目录下启动本地服务器：

```bash
python -m http.server 8080
# 然后访问 http://localhost:8080
```
