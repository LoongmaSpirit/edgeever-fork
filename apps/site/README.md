# EdgeEver 官方网站

本目录是 **EdgeEver** monorepo 内的官方网站应用，位于 `apps/site`。

> **EdgeEver：基于 Cloudflare 全家桶自托管的开源『印象笔记』。**
>
> 核心项目仓库：[GitHub - tianma-if/edgeever](https://github.com/tianma-if/edgeever)
> 
> 官方网站地址：[https://edgeever.org](https://edgeever.org) (演示环境：[https://demo.edgeever.org](https://demo.edgeever.org))

---

## 技术栈

本官网基于以下技术构建：
- **框架**：[Astro v5](https://astro.build/) (静态站点生成)
- **样式**：[Tailwind CSS v4](https://tailwindcss.com/)
- **内容管理**：Astro Content Collections (使用 Markdown 编写指南与日志)

## 开发与构建

### 1. 安装依赖

在仓库根目录安装依赖：

```bash
bun install
```

### 2. 启动开发服务器

```bash
bun run dev:site
```

### 3. 构建静态站点

```bash
bun run build:site
```

构建产物将输出在 `apps/site/dist/` 目录中。

### 4. 本地预览构建产物

```bash
bun run preview:site
```

## 目录结构

- `src/pages/`：官网主要页面（首页、联系我们、开发日志）。
- `src/components/`：可复用的 UI 与区块组件。
- `src/content/blog/`：存储与项目相关的技术指南和更新日志（Markdown 格式）。
- `public/`：存放静态图片、图标与 Robots.txt。
