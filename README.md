# WebNav Hub

WebNav Hub 是一个响应式、高性能的个人及团队常用网站导航中心。本项目已使用 **Vue 3 (v3.5+)** 及 VoidZero 推出的下一代前端统一开发工具链 **Vite+** (`vite-plus`) 进行全面重构。

---

## ⚡ 特性亮点

- **Vue 3 & 响应式渲染**：使用 Vue 3 最新稳定版（Composition API 与 `<script setup>`）重构，数据与视图彻底解耦，组件更加轻量。
- **Vite+ 极速构建**：底层采用 Rust 编写的下一代打包器 **Rolldown** 替代传统 Rollup，生产环境构建耗时缩短至 **`240ms`** 左右，冷启动与热重载（HMR）快如闪电。
- **实时快捷搜索**：
  - 支持对**网站名称**或**网址 URL** 进行不区分大小写的实时模糊搜索。
  - **动态分类折叠**：若某一分类下无符合关键字的链接，该分类标题及网格会自动隐藏，保持导航界面清爽。
  - 提供友好的“未匹配结果”空白状态提示。
- **高级交互与定位**：
  - 点击分类标签实现平滑滚动定位（Smooth Scroll），并保持浏览器地址栏哈希 (`#hash`) 自动同步。
  - 监听浏览器哈希变化（如前进/后退），自动滚动至对应锚点并点亮对应导航项。
- **无障碍友好 (A11y)**：原生绑定 `:aria-label` 属性，增强屏幕阅读器的无障碍解析。
- **全新黑橙科技感 favicon**：精心设计了带有中心 Hub 拓扑网格和字母 `W` 的黑橙高对比度矢量 SVG 浏览器标签图标。

---

## 📁 目录结构

```text
net/
├── .github/
│   └── workflows/
│       └── deploy.yml    # 原生 GitHub Actions CI/CD 自动部署工作流
├── public/
│   ├── CNAME             # GitHub Pages 自定义域名配置
│   └── favicon.svg       # 重新设计的黑橙渐变 SVG favicon
├── src/
│   ├── data/
│   │   └── links.js      # 结构化的导航链接数据模块 (ESM)
│   ├── App.vue           # 导航站主 App 组件 (搜索、渲染及滚动监听)
│   ├── main.js           # Vue 应用初始化入口
│   └── style.css         # 页面全局及组件响应式样式 (Vanilla CSS)
├── .gitignore            # Git 忽略配置
├── .npmrc                # 依赖镜像源与 CI 拉取策略
├── index.html            # Vite+ 挂载模板页
├── package.json          # 依赖配置与 Vite+ 重写指令
├── vite.config.js        # Vite+ 配置文件
└── README.md             # 本说明文档
```

---

## 🛠️ 本地开发与指令

### 1. 安装依赖
本项目集成了 Vite+ 的依赖重定向。执行以下命令即可自动使用符合 Vite+ 规范的包依赖：
```bash
npm install
```

### 2. 启动开发服务器
启动本地开发服务并启用 Vite+ 极速热重载：
```bash
npm run dev
# 或直接使用全局 vp 指令
vp dev
```
启动后在浏览器中访问 `http://localhost:3000/` 即可。

### 3. 生产环境编译
编译并生成打包产物，输出至 `dist/` 目录：
```bash
npm run build
# 或直接使用全局 vp 指令
vp build
```

---

## ✍️ 如何管理链接与分类 (增减指南)

在重构后的项目中，所有的导航数据已经与 UI 视图彻底解耦，统一存放在 [links.js](./src/data/links.js) 数据模块中。您无需再像以前那样修改繁杂的 HTML 标签，只需编辑该数据文件即可完成内容管理。

### 1. 增加/修改/删除 链接 (Link)
打开 [links.js](./src/data/links.js)，定位到对应分类下的 `links` 数组：
* **新增链接**：在数组中追加一个新对象，例如：
  ```javascript
  {
    "name": "自定义网站",
    "url": "https://example.com/",
    "icon": "fa-solid fa-globe"  // FontAwesome 图标类名
  }
  ```
* **修改链接**：修改对应对象的 `name`、`url` 或 `icon` 字段。
* **删除链接**：直接将该链接的对象从数组中移除。

### 2. 增加/修改/删除 分类 (Category)
分类信息存放在最外层的主数组中，单个分类对象的结构如下：
```javascript
{
  "id": "tools",            // 英文唯一标识，用作 HTML 锚点 ID 及 URL 哈希 (例如 #tools)
  "title": "实用工具",       // 展示在导航栏及页面分类标题中的文字
  "links": [ ... ]          // 该分类下的链接列表
}
```
* **新增分类**：在主数组中追加或插入一个新的分类对象。*注意：`id` 必须为唯一的英文标识符，不要包含中文字符、特殊符号或空格。*
* **修改分类**：修改 `title` 属性以更改显示文字，修改 `id` 属性将改变锚点位置和网址哈希值。
* **删除分类**：从数组中彻底删除该分类对象。

> [!TIP]
> 修改并保存数据后，处于运行状态的本地开发服务 (`npm run dev`) 会自动进行热更新（HMR），您直接在浏览器中即可看到增减后的内容，非常高效。

---

## 🚀 部署至各大托管平台教程

由于重构后的项目为纯静态单页应用（SPA），因此可以极为方便地部署至所有主流的静态网站托管平台。以下是各平台的详细部署步骤。

---

### 1. 部署至 GitHub Pages (推荐 - 自动流水线)

我们已经在项目中内置了**原生 GitHub Actions** 自动化构建与发布流程。

#### 步骤 A：配置自定义域名 DNS
如果您使用自定义域名（例如 `pvue.ndjp.net`）：
- 请前往您的域名服务商后台，为子域名（如 `pvue`）添加一条 **CNAME** 解析记录。
- **记录类型**：`CNAME`
- **主机记录**：`pvue`
- **记录值**：`bbylw.github.io` (替换为您的 GitHub Pages 默认域名)
- 已经在 [public/CNAME](./public/CNAME) 中内置了您的自定义域名声明。

#### 步骤 B：启用 GitHub 托管
1. 将本项目推送至您的 GitHub 仓库（例如 `https://github.com/bbylw/pvue`）。
2. 进入仓库的 **Settings** -> 左侧导航 **Pages**。
3. 在 **Build and deployment** -> **Source** 下拉菜单中，选择 **`GitHub Actions`**。
4. 随后，每次推送到 `main` 分支时，项目便会自动进行极速打包并上线部署。

---

### 2. 部署至 Vercel

Vercel 提供了优秀的零配置 Vue 3 部署体验。

> [!WARNING]
> 由于 Vercel 构建容器默认没有全局安装 `vp` 命令行工具，**请勿将构建命令设为 `vp build`**。请务必使用 `npm run build` 或 `npx vp build` 来调用项目中安装的 Vite+ 工具链。

#### 步骤：
1. 注册并登录 [Vercel 官网](https://vercel.com/)。
2. 点击 **Add New** -> **Project**，导入您的 GitHub 仓库 `bbylw/pvue`。
3. 在 **Configure Project** 页面配置构建参数：
   - **Framework Preset**：选择 `Vite`。
   - **Build Command**：开启覆盖并设置为 `npm run build`（或 `npx vp build`）。
   - **Output Directory**：`dist`。
4. 点击 **Deploy** 开始部署。
5. **绑定自定义域名**：
   - 部署完成后，在项目的 **Settings** -> **Domains** 中，输入 `pvue.ndjp.net` 进行添加。
   - 根据页面提示，在域名解析后台为 `pvue` 添加 CNAME 记录指向 `cname.vercel-dns.com`。

---

### 3. 部署至 Netlify

Netlify 同样是一款高效的静态托管平台。

> [!WARNING]
> 与 Vercel 类似，Netlify 构建容器也无全局 `vp` 变量。**请确保将构建命令设为 `npm run build` 或 `npx vp build`**。

#### 步骤：
1. 登录 [Netlify 官网](https://www.netlify.com/)。
2. 点击 **Add new site** -> **Import an existing project**，选择 GitHub 并导入 `bbylw/pvue` 仓库。
3. 确认以下构建参数：
   - **Build command**：`npm run build`（或 `npx vp build`）。
   - **Publish directory**：`dist`。
4. 点击 **Deploy site** 启动构建。
5. **绑定自定义域名**：
   - 前往 **Site configuration** -> **Domain management** -> **Custom domains**，添加 `pvue.ndjp.net`。
   - 根据页面指引，在您的域名 DNS 控制台将 CNAME 指向 Netlify 分配给您的二级域名（如 `xxx.netlify.app`）。

---

### 4. 部署至 Cloudflare Pages

依托 Cloudflare 全球 CDN 的极速静态资源加载平台。

> [!WARNING]
> 请务必检查 Cloudflare Pages 中的构建命令配置，**确保其为 `npm run build` 或 `npx vp build`**，不要使用默认的 `vite build`，以保证正确启用 Vite+ 的打包流程。

#### 步骤：
1. 登录 [Cloudflare 控制台](https://dash.cloudflare.com/)，在左侧菜单栏选择 **Workers 和 Pages** -> **Pages**。
2. 点击 **连接到 git** 并选择导入仓库 `bbylw/pvue`。
3. 选择 **框架预设** 为 **`Vite`**，并覆盖以下配置：
   - **构建命令**：`npm run build`（或 `npx vp build`）
   - **输出目录**：`dist`
4. 点击 **保存并部署**。
5. **绑定自定义域名**：
   - 构建成功后，点击 **自定义域** 选项卡。
   - 输入 `pvue.ndjp.net`，根据指引通过 CNAME 接入 Cloudflare（若域名本身就在 CF 托管，一键即可激活）。

---

## 🔧 技术选型生态

- **核心视图框架**：Vue 3.5.x
- **前端工具链**：Vite+ (vite-plus@0.2.4)
- **底层编译器生态**：
  - **Rolldown**：基于 Rust 的极速打包器（打包产物位于 `dist/`）
  - **Oxlint & Oxfmt**：基于 Rust 的超快 Linter & Formatter
- **样式方案**：原生 CSS 响应式布局
- **图标集**：FontAwesome Free (v7.3.0)
