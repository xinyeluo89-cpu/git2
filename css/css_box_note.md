# CSS 盒子模型（Box Model）笔记

## 1. 什么是盒子模型？

在 CSS 中，**每个 HTML 元素都可以看作一个矩形的"盒子"**。理解盒子模型是掌握 CSS 布局的基础。

每个盒子由四个部分组成（从内到外）：

```
┌──────────────────────────────────┐
│         margin（外边距）          │
│   ┌──────────────────────────┐   │
│   │     border（边框）        │   │
│   │   ┌──────────────────┐   │   │
│   │   │ padding（内边距） │   │   │
│   │   │   ┌──────────┐   │   │   │
│   │   │   │ content  │   │   │   │
│   │   │   │（内容区） │   │   │   │
│   │   │   └──────────┘   │   │   │
│   │   └──────────────────┘   │   │
│   └──────────────────────────┘   │
└──────────────────────────────────┘
```

| 层级 | 英文名 | 说明 |
|------|--------|------|
| 内容 | content | 实际显示文字/图片的区域 |
| 内边距 | padding | 内容到边框之间的距离（在盒子内部） |
| 边框 | border | 盒子的边界线 |
| 外边距 | margin | 盒子与其他盒子之间的距离（在盒子外部） |

---

## 2. 边框（border）详解

边框有三个关键属性，通常合写在一起：

```css
border: 粗细  样式  颜色;
```

### 2.1 边框样式（border-style）

| 值 | 效果 | 视觉效果 |
|----|------|----------|
| `solid` | 实线 | `───────` |
| `dashed` | 虚线 | `- - - -` |
| `dotted` | 点线 | `·······` |
| `double` | 双实线 | `═══════` |
| `none` | 无边框 | （隐藏） |

### 2.2 实例代码解读

> 参考文件：[5.0盒子模型.html](./5.0盒子模型.html)

```css
/* Box 1：红色背景 + 黑色实线边框 */
.box1 {
    background-color: red;
    border: 3px solid black;
}

/* Box 2：绿色背景 + 黑色虚线边框 */
.box2 {
    background-color: green;
    border: 3px dashed black;
}

/* Box 3：蓝色背景 + 黑色点线边框 */
.box3 {
    background-color: blue;
    border: 3px dotted black;
}
```

三个盒子的**共同特征**（通过属性选择器统一设置）：
```css
div[class*="box"] {
    width: 200px;   /* 内容区宽度 */
    height: 200px;  /* 内容区高度 */
}
```

> **知识点**：`[class*="box"]` 是 CSS 属性选择器，意思是"选择 class 属性中**包含** 'box' 字符串的所有 div"。

---

## 3. 盒子实际占用空间的计算

默认情况下（`box-sizing: content-box`）：

```
盒子实际宽度 = width + padding-left + padding-right + border-left + border-right
盒子实际高度 = height + padding-top + padding-bottom + border-top + border-bottom
```

> ⚠️ **margin 不计算在盒子尺寸内**，它只影响盒子与周围元素之间的**间距**。

### 3.1 box-sizing 属性

如果你觉得上面的计算方式太麻烦，可以用：

```css
* {
    box-sizing: border-box;
}
```

这样 `width` 和 `height` 就包含了 content + padding + border，**设置多大就是多大**，更符合直觉。

| 值 | 含义 |
|----|------|
| `content-box`（默认） | width/height 只指 content 区域 |
| `border-box`（推荐） | width/height 包含 content + padding + border |

---

## 4. 常用相关属性速查

| 属性 | 作用 | 示例 |
|------|------|------|
| `width` / `height` | 内容区宽高 | `width: 200px;` |
| `padding` | 内边距（四边统一） | `padding: 10px;` |
| `padding-top/right/bottom/left` | 单边内边距 | `padding-left: 20px;` |
| `border` | 边框（三合一） | `border: 2px solid red;` |
| `border-radius` | 圆角 | `border-radius: 10px;` |
| `margin` | 外边距（四边统一） | `margin: 20px;` |
| `margin-top/right/bottom/left` | 单边外边距 | `margin-top: 10px;` |

### 4.1 简写规则（上右下左，顺时针）

```css
margin: 10px;                    /* 四边一样 */
margin: 10px 20px;               /* 上下 10px，左右 20px */
margin: 10px 20px 30px;          /* 上 10px，左右 20px，下 30px */
margin: 10px 20px 30px 40px;     /* 上 10px，右 20px，下 30px，左 40px（顺时针）*/
```

> padding 的简写规则与 margin 完全一样。

---

## 5. 学习建议

1. **打开浏览器开发者工具**（F12），选中任意元素，在 "Computed" 面板中可以直观看到盒子模型图
2. 用 `border` 做可视化调试：给元素临时加个 `border: 1px solid red;` 可以看清盒子的边界
3. 初学者建议**全局面设置 `box-sizing: border-box`**，避免尺寸计算上的困惑
