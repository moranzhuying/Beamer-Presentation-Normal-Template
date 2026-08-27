# Beamer-Presentation — 数学演示模板（Yukina 主题）

与「笔记写作」模板配套的 Beamer 演示文稿模板，面向数学讲座与课程演示。继承「笔记写作」的学术配色、定理环境语义与数学符号库，并新增深浅双模式切换。

## 目录结构

```
.
├── main.tex              # 主文件：加载主题/宏包，汇总各节帧
├── beamerthemeYukina.sty # 主题：配色、字体、帧模板（frametitle/footline/itemize）
├── structure.sty         # 样式包：数学宏包、定理环境、符号库
├── quiver.sty            # 交换图支持（与「笔记写作」相同）
├── Content/              # 内容目录（按节组织帧文件）
│   ├── 01_Test_Section/  # 基础测试帧：文字/列表/公式/定理/引用
│   └── 02_Test_Section/  # 进阶测试帧：交换图/两栏/代码/表格
└── Figures/              # 插图目录
```

## 模板特点

### 1. 中文 Beamer 演示

基于 `ctexbeamer`（16:9，10pt），中文由 ctex 自动处理，西文主字体 TeX Gyre Termes（与「笔记写作」一致）。

### 2. 深浅双模式切换

`main.tex` 中一行切换：

```latex
\usetheme[light]{Yukina}   % 浅色模式（默认，白底）
\usetheme[dark]{Yukina}    % 深色模式（深底，主色自动亮化）
```

两套模式下定理盒、标题、列表符号自动适配。

### 3. 定理环境体系（与「笔记写作」语义一致）

22 种环境、11 色配色全部保留，用法完全相同：

```latex
\begin{theorem}{欧拉恒等式}{thm:euler}
  $e^{i\theta} = \cos\theta + i\sin\theta$
\end{theorem}
\begin{proof}
  由泰勒展开直接得到。
\end{proof}
```

* 编号类环境共用 `mathcount` 计数器（按 section 编号，如 定理 1.1），习题使用独立计数器；
* 引用用 `\ref{label}`（Beamer 内置 hyperref，无需 cleveref）；
* 每个环境为「顶部细色条 + 主色标题 + 浅色盒底」，内部支持 `\pause` 等 overlay 指令。

环境清单（与「笔记写作」一一对应）：

| 类别 | 环境 |
|------|------|
| 基础陈述 | 定义（蓝）、公理（靛蓝）、假设（青） |
| 推演结论 | 定理（红）、引理（橙）、命题（紫）、推论（绿）、元定理（靛蓝）、准则（茶） |
| 补充说明 | 问题（黄）、例（绿）、注记（灰） |
| 习题 | 习题（品红，独立编号）、解答（灰，题后即答） |
| 其他 | 算法、约定、警示、证明（自动加 ∎）、回答、分析、提示（可选标题）、代码（listings） |

### 4. 数学符号库与交换图

* `structure.sty` 模块 IV 的符号库（`\N \Z \Q \R \C`、`\Hom \End \Aut`、`\GL \SL \SO` 等）与「笔记写作」完全一致，直接复用；
* `quiver.sty` 支持 q.uiver.app 导出的交换图。

## Beamer 使用要点

### 帧与 overlay

```latex
\begin{frame}{帧标题}
  \begin{itemize}
    \item 第一项
    \item<2-> 第二项分步出现
  \end{itemize}
  \pause
  更多内容
\end{frame}
```

* `\pause`：分步显示；`\only<2->` / `\uncover<1,3>`：按页（overlay）控制内容；
* `\section` / `\subsection`：自动生成导航，每节开头自动插入节标题帧（不需要可删主题文件中 `\AtBeginSection`）。

### 两个容易踩的坑

1. **含 tikz-cd 交换图或 verbatim/listings 的帧必须标记 `[fragile]`**：

   ```latex
   \begin{frame}[fragile]{交换图}
     \[ \begin{tikzcd} A \arrow[r, "f"] & B \end{tikzcd} \]
   \end{frame}
   ```

   这是 Beamer 与 tikz-cd 的 catcode 兼容要求，不加会报 `Single ampersand used with wrong catcode`。

2. **帧内容要能放进一页**：Beamer 没有跨页断行机制，内容超出一页不会自动分页，需要拆分帧或缩小内容。

### 编译

```bash
latexmk -xelatex main.tex
```

## 与「笔记写作」的对应关系

| 「笔记写作」 | 本模板 |
|------|------|
| `ctexbook` + `structure.sty` | `ctexbeamer` + `beamerthemeYukina.sty` + `structure.sty` |
| tcolorbox 定理环境（breakable） | Beamer 原生盒子（支持 overlay，每帧一页） |
| titlesec / fancyhdr / geometry | `\setbeamertemplate` / `\setbeamercolor` / `\setbeamerfont` |
| cleveref（\cref） | Beamer 内置 \ref（演示中引用需求弱） |
| 数学符号库 / quiver.sty | 原样复用 |

环境名与配色完全一致，意味着**把笔记内容改写成幻灯片时，定理环境写法零修改**。

## 更新日志

见 [ChangeLog.md](./ChangeLog.md)。
