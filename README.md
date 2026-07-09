# WebNav Hub

WebNav Hub 是一个响应式、高性能的个人及团队常用网站导航中心。本项目已使用 **Vue 3** 及 VoidZero 的统一前端开发工具链 **Vite+** (`vite-plus`) 进行全面重构。

---

## ⚡ 特性亮点

- **Vue 3 驱动**：使用 Vue 3 最新稳定版（Composition API 与 `<script setup>`）重构，数据与视图彻底解耦。
- **Vite+ 极速构建**：底层采用 VoidZero 的 Rust 编译器（Rolldown），生产环境构建耗时仅需约 **240ms**，提供秒级冷启动与极致的热重载 (HMR) 体验。
- **智能实时搜索**：
  - 支持对**网站名称**或**网址 URL** 进行不区分大小写的实时关键字搜索。
  - **动态分类折叠**：若某个分类下无符合搜索词的链接，该分类标题及网格会自动折叠隐藏，保持界面清爽。
  - 提供友好的“未找到匹配结果”空白状态提示。
- **高级交互体验**：
  - 点击分类链接实现平滑滚动定位（Smooth Scroll），并保持浏览器地址栏哈希 (`#hash`) 完美同步。
  - 监听浏览器哈希变化（如前进/后退），自动定位并点亮对应导航标签。
- **无障碍支持 (A11y)**：原生绑定 `:aria-label` 属性，对屏幕阅读器十分友好。
- **全新黑橙科技感 favicon**：精心设计了带有中心 Hub 拓扑网格和字母 `W` 的黑橙高对比度矢量 SVG 浏览器标签图标。

---

## 📁 目录结构

```text
net/
├── public/
│   └── favicon.svg       # 重新设计的黑橙渐变 SVG favicon
├── src/
│   ├── data/
│   │   └── links.js      # 结构化的导航链接数据模块 (ESM)
│   ├── App.vue           # 导航站主 App 组件 (搜索、渲染及滚动监听)
│   ├── main.js           # Vue 应用初始化入口
│   └── style.css         # 页面全局及组件响应式样式 (Vanilla CSS)
├── index.html            # Vite 挂载模板页
├── package.json          # 依赖配置与 Vite+ 重写指令
├── vite.config.js        # Vite+ 配置文件
└── README.md             # 本说明文档
```

---

## 🛠️ 本地开发与部署

### 1. 安装依赖
本项目使用 Vite+ 统一包管理器托管，推荐直接使用 npm 执行安装：
```bash
npm install
```

### 2. 本地开发服务
启动本地开发服务器并启用 Vite+ 热重载：
```bash
npm run dev
# 或直接使用全局 vp 命令（若已全局安装 vp）
vp dev
```
启动后在浏览器打开 `http://localhost:3000/` 即可预览。

### 3. 生产环境构建
利用 Rolldown 极速构建打包静态文件：
```bash
npm run build
# 或
vp build
```
打包文件将输出至 `dist/` 目录。

### 4. 预览构建版本
在本地启动服务预览 `dist/` 目录的构建产物：
```bash
npm run preview
# 或
vp preview
```

---

## 🔧 技术选型

- **核心框架**：Vue 3.5.x
- **构建工具**：Vite+ (vite-plus@0.2.4)
- **编译/打包核心**：Rolldown / Oxc (Oxlint & Oxfmt)
- **样式方案**：原生 CSS (Vanilla CSS) 响应式布局
- **图标集**：FontAwesome Free (v7.3.0)
