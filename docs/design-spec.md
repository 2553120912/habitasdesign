# 设计规范

## 设计原则

- **克制** — 装饰服务于内容，不做多余效果
- **留白** — 让空间呼吸，不堆砌信息
- **清晰** — 浏览动线明确，用户一眼知道去哪看什么

## 色彩体系

| 角色 | 色值 | 用途 |
|------|------|------|
| 背景色 | `#fafaf8` | 页面主背景（暖白） |
| 深色背景 | `#1a1a1a` | Hero 区域背景 |
| 主文字 | `#1a1a1a` | 正文标题 |
| 次级文字 | `#555555` | 段落文字 |
| 辅助文字 | `#999999` | 标签、日期等 |
| 浅灰 | `#e8e6e1` | 占位图、分割线 |
| 白色文字 | `#fafaf8` | 深色背景上的文字 |

## 字体

| 角色 | 字体 | 备选 |
|------|------|------|
| 标题 / 正文 | Georgia | Noto Serif SC, Source Han Serif SC, SimSun, serif |
| 导航 / 标签 | 继承 body 字体 | — |

- 字重：以 `normal` (400) 为主，不使用粗体
- 字间距：标题和导航适当加宽（`letter-spacing: 2-8px`）

## 间距

| 场景 | 值 |
|------|-----|
| 页面左右边距（桌面） | `48px` |
| 页面左右边距（手机） | `24px` |
| 板块上下间距 | `120px` 桌面 / `80px` 手机 |
| 板块标题与内容间距 | `48px` |
| 卡片间距 | `32px` |

## 箭头 CTA 按钮（Arrow CTA）

带圆圈箭头 + 悬停下划线的引导链接（样板：Hero「Start Your Journey」）。

### 设计逻辑

- **文字大小（`font-size`）是唯一基准**：圆圈、箭头、间距、下划线的尺寸与距离全部用 `em` 换算，随字体等比缩放。
- 使用时**只需设定 `font-size` 和颜色**，其余自动成比例，不要写死 `px`。
- **箭头柄的中心对齐圆圈中心**。

### 比例（以文字为 `1em`）

| 元素 | 尺寸 / 距离 |
|------|------------|
| 圆圈直径 | `1.45em` |
| 圆圈描边 | `1px solid currentColor`，`border-radius: 50%` |
| 箭头图标（svg） | `0.9em` |
| 文字 ↔ 圆圈 间距（gap） | `0.55em` |
| 下划线右缩进（`right`） | `2em`（＝圆圈 `1.45` ＋ 间距 `0.55`，使线只压在文字下方、避开圆圈） |
| 下划线底距（`bottom`） | `-0.09em` |
| 下划线粗细 / 颜色 | `1px` / `currentColor` |

> 校验（`font-size: 22px` 时）：圆圈 ≈32px、箭头 ≈20px、gap ≈12px、下划线右缩进 =44px、底距 ≈−2px。

### 箭头几何（柄中心对圆心）

- 正方形 `viewBox`（如 `0 0 24 24`）。
- 横柄画在**垂直中线**（`y = 高度 / 2`，如 `y=12`）、水平居中；箭头尖落在中线右端。
- `stroke-width ≈ 1.35`，`stroke-linecap/linejoin: round`。
- 圆圈用 `display:flex; align-items:center; justify-content:center` 居中放 svg → 柄中心即圆圈中心。

### 动效

- 悬停 / 聚焦：整体 `opacity: 0.68`。
- 下划线：默认 `transform: scaleX(0)`，悬停 `scaleX(1)`；`transform-origin: center`；过渡 `0.35s`。
- 圆圈：悬停向右平移约 `0.18em`（细微）；过渡 `0.3s`。

### 颜色

- 统一用 `currentColor`，由父级 `color` 决定：深色背景用白 `#fafaf8`，浅色背景用深 `#1a1a1a`。

### 参考实现

```css
.arrow-cta {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 0.55em;
  color: inherit;              /* 深底给 #fafaf8，浅底给 #1a1a1a */
  text-decoration: none;
  /* font-size 由使用处设定，例如 22px 或 16px */
  transition: opacity 0.3s ease;
}
.arrow-cta:hover { opacity: 0.68; }

.arrow-cta-icon {
  width: 1.45em;
  height: 1.45em;
  border: 1px solid currentColor;
  border-radius: 50%;
  display: flex;
  align-items: center;         /* 柄中心 = 圆心 */
  justify-content: center;
  transition: transform 0.3s ease;
}
.arrow-cta-icon svg { width: 0.9em; height: 0.9em; display: block; }
.arrow-cta:hover .arrow-cta-icon { transform: translateX(0.18em); }

.arrow-cta::after {            /* 下划线只压在文字下方 */
  content: '';
  position: absolute;
  left: 0;
  right: 2em;                  /* = 圆圈 1.45 + 间距 0.55 */
  bottom: -0.09em;
  height: 1px;
  background: currentColor;
  transform: scaleX(0);
  transform-origin: center;
  transition: transform 0.35s ease;
}
.arrow-cta:hover::after { transform: scaleX(1); }
```

```html
<a class="arrow-cta" style="font-size: 22px; color: #fafaf8;">
  <span>Start Your Journey</span>
  <span class="arrow-cta-icon" aria-hidden="true">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"
         stroke-width="1.35" stroke-linecap="round" stroke-linejoin="round">
      <line x1="4" y1="12" x2="20" y2="12"/>       <!-- 横柄在中线 y=12 -->
      <polyline points="14,6 20,12 14,18"/>         <!-- 箭头尖落在中线右端 -->
    </svg>
  </span>
</a>
```

## 响应式

| 断点 | 布局变化 |
|------|----------|
| 默认（>768px） | 三列作品网格、两列联系信息 |
| ≤768px | 单列堆叠、减小字号和间距 |

## 设计参考方向

- 日本建筑师事务所网站（隈研吾、SANAA 等）的极简风格
- 瑞士平面设计风格（大量留白、网格系统）
- 待补充更多参考

## 图片规范（待定）

- 占位色：`#e8e6e1`
- 悬停变深：`#d4d1cb`
- 项目图片建议比例：4:3 或 16:9
