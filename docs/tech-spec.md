# 技术规范

## 技术栈

| 层级 | 选型 | 说明 |
|------|------|------|
| 结构 | HTML5 | 语义化标签 |
| 样式 | CSS3（内联于 HTML） | 不使用预处理器的纯 CSS |
| 交互 | 原生 JavaScript（内联于 HTML） | 无框架、无库 |
| 部署 | 静态文件托管 | 任何 Web 服务器或直接打开 |

## 原则

1. **零依赖** — 不引入任何外部 CSS/JS 库或框架
2. **单文件优先** — 当前阶段所有代码集中于 `index.html`，后续根据需要拆分
3. **渐进增强** — 基础 HTML 结构确保内容可读，CSS/JS 增强体验
4. **语义化** — 使用 `<header>`, `<nav>`, `<section>`, `<footer>` 等语义标签

## 浏览器兼容

- Chrome / Edge / Firefox / Safari 近两年版本
- 移动端 iOS Safari / Android Chrome

## 文件命名

- 全部小写英文 + 连字符：`index.html`, `project-detail.html`
- 图片资源：`images/` 目录，描述性命名如 `shanshui-01.jpg`
- 开发日志：`devlog/YYYY-MM-DD.md`

## 编码规范

- HTML: 2 空格缩进
- CSS: 属性按布局 → 尺寸 → 外观 → 其他排序
- 中文内容直接使用，不转义
