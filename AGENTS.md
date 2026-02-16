# Developer & Agent Guidelines (LLM-Friendly)

> AI Agent 在生成、修改、审查本目录代码时，必须遵守本规范。

## 0. 规则优先级（冲突处理）

1. 硬性禁令（禁止项）
2. MUST（必须）
3. SHOULD（建议）
4. 示例代码与说明文字（仅参考）

如规则冲突，按上面顺序执行。

---

## 1. 硬性禁令（MUST NOT）

- 禁止使用 ES Modules（`type="module"`、`import/export`）。
- 禁止引入构建流程（如 Vite/Webpack、本地打包产物、`node_modules` 依赖运行时）。
- 禁止创建独立 `.css` 文件。
- 禁止在页面内自实现 Toast；只能调用 `toast()`。
- 禁止从零创建新页面；必须以 `template.html` 复制为起点。
- 禁止直接修改 `template.html`（除非用户明确要求）。

---

## 2. 术语定义（机器可判定）

- **处理中（Pending）**：正在进行计算或 IO 操作。
- **接近 1000 行**：`<script>` 内 JS 行数 `>= 800`。

---

## 3. 技术与加载规则（MUST）

- 依赖通过 CDN 引入，无本地构建步骤。建议使用 `https://cdn.jsdelivr.net`，主版本号即可。
- 标准栈：[Tailwind CSS (Play CDN)](https://tailwindcss.com) + [DaisyUI](https://daisyui.com) + [Alpine.js](https://alpinejs.dev) + [Iconify](https://iconify.design) + [Day.js](https://day.js.org)。
- 遇到交互需求时优先查找 [Alpine 官方插件](https://alpinejs.dev/plugins)（如 `@alpinejs/mask` 处理输入掩码），插件必须在主库之前引入。
- 引入顺序：DaisyUI CSS → Tailwind → Alpine 插件 → Alpine 主库 → 其他库。
- 所有页面逻辑放在 `document.addEventListener('alpine:init', ...)` 中初始化。
- 核心算法或复杂逻辑应包含在页面 `<script>` 块中，并维护 `loading` 状态。

---

## 4. 逻辑归属决策表（MUST）

| 逻辑类型                 | 放置位置                |
| ------------------------ | ----------------------- |
| 页面 UI 状态、交互与逻辑 | 当前 HTML 的 `<script>` |
| 全局常量、通用工具函数   | 当前 HTML 的 `<script>` |

---

## 5. 交互反馈规则（MUST）

- **操作反馈**：
  - 处理中（计算/生成）：使用 `loading-spinner`。
  - 成功/失败反馈统一调用 `toast()`。
  - 触发按钮必须有 loading 态并 `disabled`。
- **弹窗（Modal）**：
  - 使用 DaisyUI `<dialog>` + Alpine `x-ref`，不使用全局 ID。
  - 打开：`@click="$refs.myModal.showModal()"`，关闭：`<form method="dialog">`。

---

## 6. 命名规范（SHOULD）

- Alpine 组件名：`[feature]Page`（如 `indexPage`、`chatPage`）。
- 布尔状态：`is/has/show` 前缀（如 `isLoading`、`showModal`）。
- 交互函数：`handle/toggle` 前缀（如 `handleSubmit()`、`toggleSidebar()`）。
- 处理函数：`process/generate` 前缀。
- 全局常量：`SNAKE_CASE`（如 `MAX_CHAR_COUNT`）。
- 图标：`<iconify-icon icon="lucide:icon-name"></iconify-icon>`，优先使用项目内已出现图标名。

---

## 7. 项目结构（MUST）

```text
/project-root
├── template.html    # 新页面模板（禁止直接修改）
├── index.html       # 页面
└── assets/          # 本地图片/字体等静态资源
```

- **template.html**：包含标准 CDN 引入顺序、Toast 容器、Modal 示例和 Alpine.js 骨架代码。

---

## 8. AI 执行检查清单（提交前逐项通过）

- [ ] 新页面是否基于 `template.html` 创建。
- [ ] JS 是否在 `alpine:init` 内初始化。
- [ ] 逻辑是否已整合入单文件（离线优先）。
- [ ] `<script>` JS 是否 `< 1000` 行；若 `>= 800` 是否已评估。
- [ ] 操作按钮是否具有 loading + disabled 状态。
