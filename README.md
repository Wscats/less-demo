# Less CSS Demo

一个用于学习和演示 [Less](https://lesscss.org/) 核心特性的静态 Demo 项目。

---

## 目录结构

```
less-demo/
├── 20160727/               # 原始版本（保留参考）
│   ├── index.html
│   ├── styles.less
│   ├── styles1.less
│   └── less.js / less.min.js
├── refactored/             # 重构版本
│   ├── public/
│   │   └── index.html      # 演示页面入口（14 个特性分区展示）
│   └── src/
│       ├── js/
│       │   └── less.min.js # Less.js 浏览器端编译器
│       └── less/
│           ├── variables.less  # 变量定义（颜色、字体、布局、插值、循环）
│           ├── mixins.less     # Mixin 定义（基础、参数化、默认值、可变参数、命名空间、守卫条件）
│           └── main.less       # 主样式，@import 并演示全部 14 个特性
└── README.md
```

---

## 演示的 Less 特性

| # | 特性 | 说明 |
|---|------|------|
| 1 | **Variables（变量）** | `@nice-blue`、`@font-size-base` 等，集中管理设计 token |
| 2 | **Color & Math Functions（颜色/数学函数）** | `percentage()`、`saturate()`、`spin()`、`lighten()` |
| 3 | **Mixins（混合）** | 普通 mixin（输出到 CSS）与 `()` 参数化 mixin（不输出） |
| 4 | **Nesting & Parent Selector（嵌套与父选择器）** | `&:before`、`&-modifier` 等嵌套写法 |
| 5 | **Variable Scope（变量作用域）** | 局部变量覆盖全局变量 |
| 6 | **Parametric Mixins（参数化混合）** | `.bg(@color, @size)` 传参复用样式块 |
| 7 | **Guarded Mixins（守卫条件混合）** | `when (@size <= 19) and (@size >= 13)` 实现条件分支 |
| 8 | **Extend（继承）** | `:extend()` 让多个选择器共享同一组规则，避免重复输出 |
| 9 | **Default Parameters & Variadic（默认参数与可变参数）** | `.bordered(@width: 1px, ...)` 默认值；`.shadow(@shadows...)` 接收任意数量参数 |
| 10 | **Mixin with !important** | 调用 mixin 时追加 `!important`，批量覆盖所有属性 |
| 11 | **Namespace（命名空间）** | `#bundle.button()` 将相关 mixin 分组，避免命名冲突 |
| 12 | **String Interpolation（字符串插值）** | `@{variable}` 嵌入变量到选择器名、属性值、URL 中 |
| 13 | **Loops（循环）** | 递归 mixin 生成 `.col-1` ~ `.col-N` 网格列 |
| 14 | **each()（列表/Map 迭代）** | 遍历 Map 批量生成 `.text-*` / `.bg-*` 主题色工具类 |

---

## 快速开始

本项目为纯静态页面，无需构建工具，直接在浏览器中打开即可运行。

### 方式一：直接打开

```bash
open refactored/public/index.html
```

> Less 样式由 `less.min.js` 在浏览器端实时编译，无需预编译步骤。
> ⚠️ 部分浏览器对本地 `file://` 协议有跨域限制，推荐使用方式二。

### 方式二：本地服务器（推荐）

```bash
# 进入项目根目录
cd /path/to/less-demo

# 使用 Python 3
python3 -m http.server 8080

# 或使用 Node.js serve
npx serve .
```

然后访问 [http://localhost:8080/refactored/public/index.html](http://localhost:8080/refactored/public/index.html)

---

## 文件说明

### `refactored/src/less/variables.less`
集中定义所有设计变量：
- **颜色**：`@nice-blue`、`@light-blue`、`@color-dark` 等
- **字体**：`@font-size-base`、`@font-size-nav` 等
- **布局**：`@logo-width`、`@border-radius`、`@base-padding`
- **字符串插值用变量**：`@theme`、`@assets-url`、`@selector`
- **循环用变量**：`@columns`
- **Extend 用变量**：`@shared-border`

### `refactored/src/less/mixins.less`
定义所有可复用的 mixin，分为四类：

| Mixin | 类型 | 说明 |
|-------|------|------|
| `.my-mixin` | 普通 | 输出到编译后的 CSS |
| `.my-other-mixin()` | 无输出 | 带括号，仅作调用模板 |
| `.my-font-mixin` | 普通 | 字体大小复用 |
| `.bg(@color, @size)` | 参数化 | 背景色 + 字体大小 |
| `.bordered(@width, @style, @color)` | 默认参数 | 三个参数均有默认值 |
| `.shadow(@shadows...)` | 可变参数 | 接收任意数量的阴影值 |
| `.reset-box()` | !important | 批量追加 `!important` |
| `#bundle { .button(); .link() }` | 命名空间 | 分组避免命名冲突 |
| `.cat(@size) when (...)` | 守卫条件 | 三路条件分支（if/else if/else）|

### `refactored/src/less/main.less`
主样式入口，通过 `@import` 引入 `variables` 和 `mixins`，按 14 个功能模块组织演示样式，每个模块均有详细注释。

### `refactored/public/index.html`
演示页面，按 14 个章节分区展示每个 Less 特性的实际渲染效果。

---

## 原始版本 vs 重构版本

原始代码位于 `20160727/` 目录，保留作为参考。

| 对比项 | 原始版本（`20160727/`） | 重构版本（`refactored/`） |
|--------|------------------------|---------------------------|
| Less 文件数 | 2 个（`styles.less` + `styles1.less`） | 3 个（职责分离） |
| 演示特性数 | 7 个 | 14 个 |
| 变量管理 | 硬编码值散落各处 | 全部提取至 `variables.less` |
| Mixin 组织 | 与样式混写 | 独立 `mixins.less`，分类注释 |
| HTML 结构 | 无分区 | 14 个 `<section>` 对应 14 个特性 |
| 注释 | 零散 | 每个功能块有标题注释 |

---

## 参考资料

- [Less 官方文档](https://lesscss.org/features/)
- [Less 函数参考](https://lesscss.org/functions/)
