# 更新日志 (ChangeLog)

**版本日期**: 2026-10-17
**上一版日期**: 2026-10-10
**当前状态**: 模板 v1.0 已完成全部功能并推送 GitHub（Beamer-Presentation-Normal-Template）

---

## 2026-10-17 更新概览

按四套模板的联合排版审查结论修复默认示例与主题的缺陷。

### 缺陷修复

* **默认示例漏参数导致吞掉正文首字**
    * `02_more_theorem.tex`、`01_exercise.tex` 里 6 处编号环境只给了一个空参数
      （`\begin{lemma}{}`），第二参数把正文首字当成了标签；补齐为 `{}{}`。
      修复前 `.aux` 里多出 `若 / 连 / 闭 / 上 / 跨 / 同` 六个意外标签。
* **同一帧的 overlay 重复递增编号**
    * `mathcount` / `exercisecount` 加 `\resetcounteronoverlays`，同一帧翻页时编号不再漂移，
      `\ref` 也不再停在首个 overlay 的值上。
* **标题页不再因短字段为空而隐藏全名**
    * 标题页改按「完整字段有无内容」判断（`\insertauthor` 恒非空宏，`\ifx…\empty` 判不出来，
      故用内容盒宽度判定）；短作者 / 短机构只决定页脚是否显示。
* **页脚边界**
    * 日期为空时不再留下多余的分隔符；元信息超宽时等比收缩到可用宽度，页码保持独立与字号。

### 优化与改进

* **大算子跟随数学样式缩小**
    * `\bigast` / `\bigtop` / `\bigbot` / `\bigboxplus` 改用 `\mathop{\mathchoice…}` 实现。
* **撇号与前字符的间距**
    * 重定义 `'` 与 `\prime`，在撇号前插入 `\primeKern`。


## 2026-10-10 更新概览

统一文档命名：更新日志由 `ChangeLog.md` 改名为 `CHANGELOG.md`，`README.md` 内的引用同步。

### 优化与改进

* **文档命名统一**
    * `ChangeLog.md` → `CHANGELOG.md`，与家族其他模板一致。
    * `README.md` 内对该文件的引用同步更新。

## 2026-10-06 更新概览

同步 ctexbook 版（笔记写作）的记号调整，并更新 `README.md`：环境清单改用英文环境名，补入把笔记/教材改写为幻灯片的要点。

### 优化与改进

* **新增与调整记号（`structure.sty`）**
    * `\Re`、`\Im` 改为直立算子（`\operatorname`），覆盖内核默认的花体 ℜ / ℑ；与「笔记写作」保持一致。
    * 新增 `\i`（虚数单位，正体）与 `\dd`（微分算子，前面自动留细空）。
* **`README.md`**
    * 环境清单改用**英文环境名**（`definition`、`axiom` …），与「笔记写作」及环境实际名称一致。
    * 在「定理环境体系」「数学符号库与交换图」两节补入改写要点：环境类型服从论证角色、拆帧时不得丢失假设与结论或证明边界、一帧内的并列显示公式合并进 `align` 系环境、有序列举一律用 `enumerate`、箭头—节点型数学图用 `tikz-cd` 重建（含图帧须标记 `[fragile]`）、标签唯一且引用不硬编码编号。

### 其他

* 本次「定理环境清单改用英文环境名」的做法与 ctexbook 版、ctexart 版保持统一。

---

## 2026-08-27 更新概览

基于「笔记写作」模板搭建的 Beamer 数学演示模板 v1.0：继承其 11 色学术配色、22 种定理环境语义与数学符号库，新增深浅双模式切换、三级章节结构（Part/Section/Subsection）、进度条页脚等；全部功能经深浅两模式编译验证，Overfull 警告清零。

### 初始搭建

* **`beamerthemeYukina.sty` 双模式主题**
    * `\usetheme[light]{Yukina}`（默认）与 `\usetheme[dark]{Yukina}` 一键切换；深浅模式下主色自动亮化、盒底按背景混合；
    * 定制 frametitle（左侧竖色条 + 底部细线）、footline、itemize 符号（◇ ▷ ∗）、标题页（居中，作者/日期/机构为空自动省略）、进度条。
* **`structure.sty`（Beamer 版）**
    * 22 种定理环境移植：环境名、11 色配色、编号体系（`mathcount` 按 section、习题独立 `exercisecount`）与「笔记写作」完全一致；
    * 实现从 tcolorbox 改为 Beamer 原生盒子（`beamercolorbox`），环境内支持 `\pause` 等 overlay 指令；
    * 数学符号库（模块 IV）与 `\noteref` 宏原样复用；移除与 Beamer 冲突的 enumitem / titlesec / fancyhdr / longtable / cleveref。
* **`quiver.sty`** 交换图支持原样复用；`update_cwl.py` 从 `structure.sty` 自动生成 TeXStudio 环境补全（22 环境）。

### 功能迭代

* **作者信息**：`main.tex` 中 `\author{}` / `\institute{}` / `\date{}` 设置，标题页居中显示；页脚居中展示 作者 · 单位 · 日期（空项自动省略）+ 页码。
* **三级章节结构**：`\part` / `\section` / `\subsection` 自动编号（目录帧、各级标题帧一致显示），每级开头自动插入标题帧（`\AtBeginPart` / `\AtBeginSection` / `\AtBeginSubsection`）。
* **内容三层目录**：`Content/` 按 节 → 小节文件夹 → 帧文件 组织（仿「笔记写作」Chapter → Section → 小节 结构），`index.tex` 逐级 `\input` 汇总。
* **页脚简化**：进度条（当前帧/总帧填充）+ 居中作者信息；章节导航与导航符号默认隐藏（主题中可恢复）。

### 缺陷修复（本轮全部实测）

* Beamer 内置 `problem` / `solution` 环境与模板重名：清理清单补齐，避免 `Command already defined`。
* `\renewenvironment{proof}` 报环境未定义：清理后改用 `\newenvironment`。
* `beamercolorbox` 不支持 `top/bottom` 选项：改用 `sep` 内边距。
* beamer 无 `\shorttitle` 命令：改用 `\title[短标题]{长标题}` 可选参数写法。
* tikz-cd 在 Beamer 中报 `Single ampersand used with wrong catcode`：含交换图/verbatim 的帧标记 `[fragile]`。
* `\AtBeginPart` 不支持 `[]` 可选参数（与 `\AtBeginSection`/`\AtBeginSubsection` 不同），去掉 `[]` 即可。
* `\tableofcontents` 默认只显示当前 part 的节：目录帧移到 `\part` 之后。
* 目录小节编号：subsection 需用 `subsections numbered` 变体（`sections numbered` 对小节不显示编号）。
* 帧内容不可跨页（溢出被静默裁切）：定理盒间距减小（`\smallskipamount`）+ 环境测试帧拆分，Overfull 警告 16 处 → 0。

### 其他

* 主题已安装到用户级 texmf（消除 TeXStudio「主题没找到」误报），README 记录同步命令。
* `Figures/test_figure.png` 测试图；全量测试覆盖 22 种定理环境、跨小节/同帧引用、图/表/代码/交换图插入。
