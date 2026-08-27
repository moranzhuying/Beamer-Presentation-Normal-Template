# 更新日志 (ChangeLog)

**版本日期**: 2026-08-27
**当前状态**: 模板初始版本

---

## 2026-08-27 更新概览

基于「笔记写作」模板搭建 Beamer 演示模板：继承其学术配色、22 种定理环境语义与数学符号库，新增深浅双模式切换，全部功能经编译验证。

### 新增功能

* **`beamerthemeYukina.sty` 双模式主题**
    * `\usetheme[light]{Yukina}`（默认）与 `\usetheme[dark]{Yukina}` 一键切换；
    * 深浅模式下主色自动亮化、盒底与细线按背景色混合，两套配色自动适配；
    * 定制 frametitle（左侧竖色条 + 底部细线）、footline（作者 · 标题 + 页码/总页数）、itemize 符号（◇ ▷ ∗）、标题页与节标题帧。
* **`structure.sty`（Beamer 版）**
    * 22 种定理环境移植：环境名、11 色配色、编号体系（`mathcount` 按 section、习题独立 `exercisecount`）与「笔记写作」完全一致；
    * 实现从 tcolorbox 改为 Beamer 原生盒子（`beamercolorbox`），环境内支持 `\pause` 等 overlay 指令；
    * 数学符号库（模块 IV）与 `\noteref` 宏原样复用；移除与 Beamer 冲突的 enumitem / titlesec / fancyhdr / longtable / cleveref。
* **测试内容 `Content/`**
    * `01_Test_Section`：文字列表（overlay 分步）、基础公式、定理环境、引用；
    * `02_Test_Section`：交换图（tikz-cd，fragile 帧）、两栏布局、代码环境（listings）、复杂公式、表格。

### 缺陷修复

* Beamer 内置 `problem`/`solution` 环境与模板重名：清理清单补齐，避免 `Command already defined`。
* `\renewenvironment{proof}` 报环境未定义：清理后改用 `\newenvironment`。
* `beamercolorbox` 不支持 `top/bottom` 选项：改用 `sep` 内边距。
* beamer 无 `\shorttitle` 命令：改用 `\title[短标题]{长标题}` 可选参数写法。
* tikz-cd 在 Beamer 中报 `Single ampersand used with wrong catcode`：含交换图的帧标记 `[fragile]`（Beamer 与 tikz-cd 的 catcode 兼容要求），README 已记录该要点。

---

## 后续计划

* 按需补充帧内目录（`\tableofcontents[currentsection]`）样式定制；
* 如需深色/浅色以外的第三套配色，可在主题模块 II 扩展。
