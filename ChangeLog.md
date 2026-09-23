# 更新日志 (ChangeLog)

**版本日期**: 2026-08-27
**当前状态**: 模板 v1.0 已完成全部功能并推送 GitHub（Beamer-Presentation-Normal-Template）

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
