# CSS 选择器（Selector）笔记

## 1. 什么是选择器？

CSS 由两部分组成：**选择器** + **声明块**。选择器的作用是告诉浏览器"我要对哪个元素应用样式"。

```css
选择器 {
    属性: 值;
}
```

---

## 2. 基础选择器

### 2.1 元素选择器（标签选择器）

按 HTML 标签名选择，**影响范围最大**。

```css
p  { color: red; }     /* 所有 <p> 变红色 */
h1 { font-size: 24px; } /* 所有 <h1> 字号变 24px */
```

### 2.2 类选择器（class）

以 `.` 开头，**最常用**。同一个 class 可以用在多个元素上。

```css
.box    { border: 1px solid black; }  /* 选 class="box" 的元素 */
.active { background-color: yellow; } /* 选 class="active" 的元素 */
```

```html
<div class="box active">我同时有 box 和 active 两个 class</div>
```

### 2.3 ID 选择器

以 `#` 开头，**一个页面中同一个 ID 只能出现一次**（唯一性）。

```css
#header { height: 60px; }  /* 选 id="header" 的元素 */
#submit { background: blue; }
```

### 2.4 通配符选择器

`*` 选中**所有元素**，通常用于"全局重置"。

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

---

## 3. 组合选择器（关系选择器）

以下面这段 HTML 为例来理解：

```html
<div class="container">
    <p>直接子元素 p</p>
    <section>
        <p>嵌套在 section 里面的 p（孙子元素）</p>
    </section>
    <p>另一个直接子元素 p</p>
</div>
```

### 3.1 后代选择器 `A B`（中间有空格）

选中 A 内部**所有层级**的 B。

```css
.container p { color: red; }
/* ↑ container 里面所有的 p，不管嵌套多深都会被选中 */
/* 结果：上面 HTML 中两个 p 都会被选中 */
```

### 3.2 子代选择器 `A > B`

只选 A 的**直接子元素** B，不包含孙子及更深层级。

```css
.container > p { color: blue; }
/* ↑ 只选 container 的直接子 p */
/* 结果：只有"直接子元素 p"和"另一个直接子元素 p"被选中 */
/*      嵌套在 section 里的 p 不会被选中 */
```

### 3.3 相邻兄弟选择器 `A + B`

选中**紧跟在 A 后面**的第一个兄弟 B。

```css
h1 + p { font-style: italic; }
/* 只选紧挨着 h1 后面的第一个 p */
```

### 3.4 通用兄弟选择器 `A ~ B`

选中 A 后面**所有**同级的兄弟 B。

```css
h1 ~ p { font-style: italic; }
/* h1 后面所有的兄弟 p 都会被选中 */
```

---

## 4. 分组选择器（并集）

用逗号 `,` 把多个选择器合在一起，**共享同一套样式**。

```css
h1, h2, h3 {
    font-family: "Microsoft YaHei", sans-serif;
    color: #333;
}
/* 等价于分别写三遍，省代码 */
```

---

## 5. 属性选择器

根据元素的**属性**来筛选，非常灵活。

| 写法 | 含义 | 示例 |
|------|------|------|
| `[attr]` | 有该属性即可 | `[title]` → 所有带 title 属性的元素 |
| `[attr="值"]` | 属性**精确等于**某值 | `[type="text"]` → type 恰好为 text |
| `[attr*="值"]` | 属性值**包含**某字符串 | `[class*="box"]` → class 里有 "box" |
| `[attr^="值"]` | 属性值**以…开头** | `[href^="https"]` → https 开头的链接 |
| `[attr$="值"]` | 属性值**以…结尾** | `[src$=".png"]` → .png 结尾的图片 |

### 实例

```css
/* 选 class 中包含 "box" 的所有 div */
div[class*="box"] {
    width: 200px;
    height: 200px;
}

/* 选所有外部链接（href 以 http 开头） */
a[href^="http"]::after {
    content: " ↗";  /* 外部链接后面加个小箭头 */
}

/* 选所有 PDF 文件链接 */
a[href$=".pdf"] {
    color: red;
}
```

---

## 6. 伪类选择器

伪类表示元素的**特定状态**，用一个冒号 `:`。

### 6.1 状态伪类

```css
a:link    { color: blue; }    /* 未访问的链接 */
a:visited { color: purple; }  /* 已访问的链接 */
a:hover   { color: red; }     /* 鼠标悬停 */
a:active  { color: orange; }  /* 鼠标按下瞬间 */

input:focus {                  /* 输入框获得焦点（正在输入） */
    outline: 2px solid blue;
}
```

> **记忆口诀（LVHA 顺序）**：`:link` → `:visited` → `:hover` → `:active`（爱恨原则：Love Hate）

### 6.2 结构伪类

根据元素在父容器中的**位置**来选择。

```css
li:first-child      { }  /* 第一个孩子 */
li:last-child       { }  /* 最后一个孩子 */
li:nth-child(3)     { }  /* 第 3 个孩子 */
li:nth-child(odd)   { }  /* 奇数项（1, 3, 5…）*/
li:nth-child(even)  { }  /* 偶数项（2, 4, 6…）*/
li:nth-child(2n+1)  { }  /* 奇数项，等价于 odd */
```

> **nth-child 公式速记**：
> - `odd` / `2n+1` → 奇数
> - `even` / `2n` → 偶数
> - `3n` → 每 3 个选一个（3, 6, 9…）

### 6.3 否定伪类 `:not()`

排除某些元素。

```css
/* 除了最后一个，其他 li 都加下边框 */
li:not(:last-child) {
    border-bottom: 1px solid #ccc;
}

/* 没有 "disabled" class 的按钮才响应 hover */
button:not(.disabled):hover {
    background-color: blue;
}
```

---

## 7. 伪元素选择器

伪元素是**不存在的虚拟元素**，用两个冒号 `::`，需要通过 `content` 属性来插入内容。

```css
/* 在每个提示前面加一个灯泡 */
.tip::before {
    content: "💡";
}

/* 在每个提示后面加一个箭头 */
.tip::after {
    content: " →";
}

/* 外部链接后面自动加图标 */
a[href^="http"]::after {
    content: " ↗";
    font-size: 12px;
}

/* 段落首字下沉效果 */
p::first-letter {
    font-size: 3em;
    float: left;
}
```

> **伪类 vs 伪元素**：
> - 伪类 `:` → 元素的**状态**（hover、focus、nth…）
> - 伪元素 `::` → **创造**出来的虚拟元素（before、after…）

---

## 8. 优先级（权重 / 特异性）

当多个选择器同时作用于一个元素，冲突了怎么办？**权重高的说了算**。

### 8.1 权重等级

| 优先级 | 选择器 | 权重值 |
|--------|--------|--------|
| 最高 | `!important` | ∞（禁用！不要滥用） |
| 很高 | 行内样式 `<div style="">` | 1000 |
| 高 | ID 选择器 `#id` | 100 |
| 中 | class / 属性 / 伪类 | 10 |
| 低 | 元素 / 伪元素 | 1 |
| 最低 | 通配符 `*` | 0 |

### 8.2 计算示例

```css
div              { }  /* 权重：1 */
.box             { }  /* 权重：10 */
div.box          { }  /* 权重：1 + 10 = 11 */
#title           { }  /* 权重：100 */
div #title       { }  /* 权重：1 + 100 = 101 */
div #title .box  { }  /* 权重：1 + 100 + 10 = 111 */
```

### 8.3 三条规则

1. **权重不同**：权重高的生效
2. **权重相同**：写在后面的生效（后来居上）
3. **`!important`**：直接无视权重规则，但不推荐频繁使用（会让代码很难维护）

---

## 9. 常见场景速查表

| 我想要选… | 写法 |
|-----------|------|
| 所有元素 | `*` |
| 所有段落 | `p` |
| class 为 "red" 的元素 | `.red` |
| id 为 "app" 的元素 | `#app` |
| 同时有 "btn" 和 "primary" 两个 class | `.btn.primary`（连着写，无空格） |
| div 里面所有的 span（任意深度） | `div span` |
| div 的直接子 span | `div > span` |
| 同时设置 h1 和 h2 | `h1, h2` |
| 紧挨 h1 后面的第一个 p | `h1 + p` |
| h1 后面所有兄弟 p | `h1 ~ p` |
| 鼠标悬停在链接上 | `a:hover` |
| 输入框获得焦点 | `input:focus` |
| 列表第 3 项 | `li:nth-child(3)` |
| 列表的偶数项 | `li:nth-child(even)` |
| 除了最后一个以外的所有 | `li:not(:last-child)` |
| 每个段落的首字 | `p::first-letter` |
| 元素前面插入内容 | `.x::before { content: "…"; }` |
| href 以 ".pdf" 结尾的链接 | `a[href$=".pdf"]` |
| class 包含 "box" 的 div | `div[class*="box"]` |

---

## 10. 回顾：HTML 文件中的选择器

```css
/* 属性选择器 — 选 class 中包含 "box" 的所有 div（3 个都选中） */
div[class*="box"] {
    width: 200px;
    height: 200px;
}

/* 类选择器 — 分别选中各自 */
.box1 { background-color: red; }
.box2 { background-color: green; }
.box3 { background-color: blue; }
```
