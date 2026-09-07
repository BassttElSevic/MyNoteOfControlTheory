# Basstt 的控制理论笔记

用 LaTeX（XeLaTeX + ctexbook）记的控制理论笔记，配合 Neovim 的 vimtex + Zathura 实现「写一行 → 编译 → 看 PDF」的实时笔记流。

仓库：<https://github.com/BassttElSevic/MyNoteOfControlTheory>

## 目录结构

```
MyNoteOfControlTheory/
├── main.tex                 # 主文档（ctexbook，XeLaTeX，封面 + \include 章节）
├── preamble.tex             # 导言区：字体、配色、页面、页眉页脚、知识块宏、代码框宏
├── chapters/
│   └── 00_getting_started.tex   # 第零章：如何记笔记（使用说明）
├── images/                  # 外部图片（目前几乎不用，图多用 TikZ 内绘）
├── build/                   # 辅助文件（.aux/.log/.toc/.synctex），latexmk 自动生成
├── main.pdf                 # 编译产物，在根目录（不进 build/）
├── .gitignore               # 忽略 LaTeX 辅助文件 + build/
└── README.md
```

## 风格（都在 preamble.tex 里改）

- **正文中文**：思源宋体（`Noto Serif CJK SC`）；**中文斜体自动映射成楷体**（`\textit`/`\emph`，霞鹜文楷）。
- **标题中文**：思源黑体（`Noto Sans CJK SC`）；**代码等宽**：JetBrains Mono Nerd Font（`JetBrainsMono NF`）；中文等宽用霞鹜文楷 Mono。
- **拉丁正文/数学**：Computer Modern。
- **页面**：A4，11pt 正文，单面排版（`oneside`，无空白页）。
- **知识块**：tcolorbox「饱和色标题条 + 同色左边线 + 极淡底 + 方角」，自动编号（章.序）。
- **代码框**：listings 语法高亮 + tcolorbox，断行不溢出。

## 使用模板

### 知识块（彩色框）

- 带编号：`\begin{definition}{标题}{标签}...\end{definition}`（不写标题只显示编号）。已有：`definition`(蓝) `theorem`(红) `lemma`(靛) `proposition`(紫) `corollary`(粉) `example`(绿) `remark`(灰)。
- 通用块：`\begin{notebox}[颜色]{标题}...\end{notebox}`（颜色缺省蓝，可选 `bl/tl/gr/og/pu/rd/in/pk/gy`）。
- 加一种新颜色：在 `preamble.tex` 用 `\definecolor{名字}{HTML}{十六进制}`，再 `\newcoloredtheorem{环境名}{显示名}{名字}{标签前缀}` 一行。

### 代码框（高亮+不溢出）

```latex
\begin{codebox}[语言]{标题}
...代码...
\end{codebox}
```

语言支持 `bash/tex/latex/text/c/python/matlab` 等（`latex`/`tex`、`sh`/`bash` 互为别名）。

### 插图

- TikZ 直接画（`tikzpicture`，已在 preamble 加载 `arrows.meta/positioning/calc/shapes.geometric`）。
- 外部图片：`\includegraphics[width=...]{images/xxx.png}`。

### 数学

行内 `$...$`；独立 `equation`；多行 `align`；引用 `\label`/`\ref`/`\eqref`。公式与图按章编号。

## 编译 / 查看（nvim）

| 按键 | 作用 |
| ------ | ------ |
| `\ll` | 开始 / 停止连续编译（latexmk） |
| `\lv` | 打开 PDF 并跳到光标位置（zathura） |
| `\lc` | 清理辅助文件 |
| `\le` | 查看编译错误 |
| `\lt` | 章节/目录 |
| `\li` | 编译信息 |

> `<localleader>` 是 `\`（在 `~/.config/nvim/init.lua` 里设的反斜杠）。

## 新增章节

1. 复制 `chapters/00_getting_started.tex` 为 `chapters/NN_名字.tex`。
2. 文件首行保留 `% !TEX root = ../main.tex`。
3. 在 `main.tex` 正文区加一行 `\include{chapters/NN_名字}`。

## 输出布局说明

- **辅助文件** → `build/`（`~/.config/nvim/lua/plugins/latex.lua` 的 `aux_dir = "build"`，`\ll` 编译时自动创建，已 gitignore）。
- **PDF** → 根目录 `main.pdf`（`out_dir = ""`，留在源码旁，方便 `\lv`）。

## 祝我好运
