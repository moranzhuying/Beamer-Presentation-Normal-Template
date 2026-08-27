# Beamer-Presentation — 数学演示模板（Yukina 主题）

与「笔记写作」模板配套的 Beamer 演示文稿模板，面向数学讲座与课程演示。继承「笔记写作」的学术配色、定理环境语义与数学符号库，并新增深浅双模式切换。

## 目录结构

```
.
├── main.tex              # 主文件：加载主题/宏包，汇总各节帧
├── beamerthemeYukina.sty # 主题：配色、字体、帧模板（进度条/导航/frametitle/itemize）
├── structure.sty         # 样式包：数学宏包、定理环境、符号库
├── quiver.sty            # 交换图支持（与「笔记写作」相同）
├── update_cwl.py         # 从 structure.sty 生成 TeXStudio 环境补全（可选）
├── cwl/                  # 生成的补全文件 yukina-beamer.cwl
├── Content/              # 内容目录（三层: 节 → 小节 → 帧文件）
│   ├── 01_Test_Section/  # 第一节
│   │   ├── index.tex     #   节汇总: 只做 \input
│   │   ├── 01_Basic/     #   小节 1.1 基础排版
│   │   │   ├── index.tex #     \subsection + \input 帧文件
│   │   │   ├── 01_text_list.tex
│   │   │   └── 02_formula.tex
│   │   ├── 02_Theorem/   #   小节 1.2 定理环境
│   │   └── 03_Exercise/  #   小节 1.3 习题与提示
│   └── 02_Test_Section/  # 第二节（交换图/布局/代码，同上三层结构）
└── Figures/              # 插图目录（含 test_figure.png 测试图）
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

### 5. 页脚三行结构

页脚（footline）自下而上三行：

1. **作者信息**：作者 · 单位 · 日期（`main.tex` 中 `\author{}` / `\institute{}` / `\date{}` 设置，为空项自动省略），右端页码 n/N；
2. **章节导航**：所有 `\section` 带编号水平排列（`1. 基础帧`），**当前节高亮为主题色**，点击节名可直接跳转到该节；
3. **进度条**：页面底部一条细线，按「当前帧/总帧」比例填充主题色。

另外，页面右下角有一排**导航符号**（前进/后退/开始/结束等箭头图标，Beamer 内置，可点击跳转）。不想要时在主题文件里写 `\setbeamertemplate{navigation symbols}{}` 即可隐藏。

### 6. 内容按帧拆分

`Content/` 下每节一个子目录，**每帧一个 `.tex` 文件**，`index.tex` 只做 `\input` 汇总（与「笔记写作」的 Chapter → Section 结构同思路）。新增帧时：在节目录里新建文件，然后在 `index.tex` 加一行 `\input`。避免单文件过大，方便定位与维护。

### 7. 章节编号与层级

支持三级结构，编号自动生成（目录、标题帧、页脚导航一致）：

```latex
\part{第一部分}          % 第 1 部分
\section{节}             % 1. 节
\subsection{小节}        % 1.1 小节
```

* 每级开头自动插入标题帧（不需要某级可写 `\AtBeginXXX{}` 关闭）；
* 目录帧须放在 `\part` 之后（beamer 的 `\tableofcontents` 默认只显示当前 part 的节）。

### 8. 帧内容不可跨页

Beamer 帧超出一页时**不会自动分页**，溢出内容会被裁掉。内容较多时应拆成多帧（参考 `Content/01_Test_Section/03_Exercise/` 的习题与引用两帧示例）。

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

## TeXStudio 提示「Beamer 主题：Yukina 没找到」

这是误报，不影响编译。原因：主题文件 `beamerthemeYukina.sty` 在项目本地目录，TeXStudio 的静态检查只搜 TeX 发行版目录。

**解决方案**（已执行）：把主题安装到用户级 TeX 目录（`~/texmf/tex/latex/beamer/themes/theme/`），TeXStudio 重启后即可识别。编译时仍优先使用项目本地版本（当前目录优先于 texmf），两者保持一致。

修改主题后若需同步到 texmf，重新执行：

```bash
cp beamerthemeYukina.sty ~/texmf/tex/latex/beamer/themes/theme/
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
