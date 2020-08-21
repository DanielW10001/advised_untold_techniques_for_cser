# LaTeX

## 1. Introduction

- TeX: 排版引擎及其标记语言 Markup Language 的名称
    - 排版引擎: 根据所输入的内容及命令, 将内容加以排版后输出的程序
        - 输出`dvi`格式文件
    - 标记语言: 将控制命令与文本内容结合起来的语言
- 编译, 排版: 从TeX源代码文件生成排版后文件的过程: `xelatex TEX_FILE`
- plain TeX: 基于TeX定义的再封装标记语言
- LaTeX
    - Documentation Preparation System: 基于TeX的排版系统
        - 再封装: LaTeX将控制命令翻译为plain TeX控制命令, 并交予TeX引擎执行
        - LaTeX排版引擎 = TeX + LaTeX->TeX转换程序
    - Source Code - Render -> Formatted Documentation
        - tex File: Content & Format
        - tex File: Content; cls File: Format
- 变种排版引擎
    - XeTeX: 支持UTF-8编码的再实现TeX引擎
    - XeLaTeX: 基于XeTeX的支持LaTeX语言的再封装排版引擎
        - XeLaTeX = XeTeX + LaTeX->TeX转换程序
    - pdfTeX: 基于TeX的直接输出pdf的再封装排版引擎
        - pdfTeX = TeX + dvi->pdf转换程序
    - pdfLaTeX: 基于pdfTeX的支持LaTeX语言的再封装排版引擎
        - pdfLaTeX = pdfTeX + LaTeX->TeX转换程序
    - LuaTeX: 正在开发的TeX引擎
- TeX发行 (版), TeX系统, TeX套装: 包括TeX系统的一组可执行程序, 辅助程序, 宏包文档, 模板, 字体文件的集合
    - TeX Live: TeX User Group出品的跨平台发行版
    - MiKTeX
    - CTeX发行: 基于MiKTeX的支持中文排版的发行 (已弃用)
- TeXworks: TeX Live 自带TeX源代码编辑器
- 在TeX系统中, 每个字符处在一个盒子中, 对盒子的排版决定了字符的排版

## 2. 控制序列

- TeX源代码文件: `.tex`
    - 注意: 路径和文件名不能包含非ASCII字符 (如中文字符)
- TeX源码中的内容并不会全部输出
- 注释: 从百分号`%`开始直到行尾的部分
    - 被编译器忽略, 不会输出, 也不会影响输出效果
    - 使源代码易于人类阅读
- 标识符
    - 大小写敏感
- 控制序列, 命令, 标记: 反斜杠`\`, 标识符 (控制序列名), 包围在方括号`[]`中的可选参数列表, 包围在花括号`{}`中的参数列表组成: `\NAME[OPTIONAL_ARGUMENT]{ARGUMENT}`
    - 不会被作为文档内容输出
    - 控制输出文档的效果 或 输出元字符
- `\%`: 输出`%`本身

## 3. 框架

- `% !TEX program = xelatex`: Specify TEX Program
- `\documentclass[ENCODING]{DOCUMENT_CLASS}`: 调用由参数指定的文档类
    - `ENCODING`: 字符编码
        - `UTF8`: UTF-8编码
    - 文档类: TeX系统预设的或用户自定的格式的集合
        - 文档类定义了内容的输出效果
    - `DOCUMENT_CLASS`
        - `article`
        - `ctexart`: 通过`CTeX`支持中文排版
- `\begin{ENV}`: 进入`ENV`环境
    - 总与`\end{ENV}`成对出现
- `\end{ENV}`: 退出`ENV`环境
    - 总与`\begin{ENV}`成对出现
    - `\begin{ENV}`与`\end{ENV}`及其之间的代码称为环境, 两控制序列的第一个必要参数`ENV`必须一致, 称为环境名
        - 次环境: 包含在环境中的环境
    - `ENV`
        - `document`: 输出到结果文档的部分
            - 在`\end{document}`之后的代码是无效的

## 4. 导言区

- 导言区: `\documentclass{article}`与`\begin{document}`之间的代码
    - 进行全局设置的区域
    - 导言区的控制序列往往决定整篇文档的格式
- `\title{TITLE}`: 设置标题内容为`TITLE`
- `\author{AUTHOR}`: 设置作者为`AUTHOR`
- `\date{DATE}`: 设置日期为`DATE`
- `\renewcommand{COMMAND}{CONTENT}`: 重载命令
- `\addtolength{VAR}{VALUE}`: 在原有基础上将`VAR`的值增加`VALUE`

### 4.1. 宏包

- 宏包, 宏集, 巨集套件: 某些常用控制序列的集合
- `\usepackage{PACKAGE}`: 调用宏包, 即执行其中的控制序列
    - 相当于将宏包中的控制序列写入源代码中
    - 类似于C语言中的`#include`
- 使用`CTeX`宏包进行中文排版, 避免使用`CJK`宏包`xeCJK`进行中文排版
    - 注意使用`UTF-8`编码及`XeLaTeX`编译器
    - 使用`CJK`宏包进行中英文排版: 在导言区:
        - `\usepackage{xeCJK}`: 调用`xeCJK`宏包
        - `\setCJKmainfont{SimSun}`: 设置中文字体为宋体`SimSun`
            - `\setCJKmainfont{FONT}`
                - 定义在`xeCJK`中的控制序列, 用于设置CJK主字体
                - `FONT`: 系统字体表示名
- 辨析: `CTeX`宏包 与 `CTeX`发行版
    - `CTeX`宏包: LaTeX宏的集合, 包含若干文档类文件`.cls`和宏包文件`.sty`
    - `CTeX`发行版: **过时**的`TeX`发行版

### 4.2. 版面设置

#### 4.2.1. 页边距

- `geometry`宏包
- 纸张长宽: `\geometry{papersize={LENGTHcm, WIDTHcm}}`
- 页边距: `\geometry{left=LEFTcm, right=RIGHTcm, top=TOPcm, bottom=BOTTOMcm}`

#### 4.2.2. 页眉页脚

- `fancyhdr`宏包

设置页眉页脚:

```tex
\usepackage{fancyhdr}
\pagestyle{fancy}
\lhead{CONTENT}
\chead{CONTENT}
\rhead{CONTENT}
\lfoot{CONTENT}
\cfoot{CONTENT}
\rfoot{CONTENT}
\renewcommand{\headrulewidth}{HEAD_RULE_WIDTH} % 设置页眉分割线宽度, E.g.: 0pt, 0.4pt
\renewcommand{\headwidth}{\textwidth} % 设置页眉宽度为正文宽度
\renewcommand{\footrulewidth}{FOOT_RULE_WIDTH} % 设置页脚分割线宽度, E.g.: 0pt, 0.4pt
\renewcommand{\footwidth}{\textwidth} % 设置页脚宽度为正文宽度
```

#### 4.2.3. 首行缩进

- `CTeX`宏包自动控制

#### 4.2.4. 行间距

- `setspace`宏包
- `\onespacing`: 行距是字号的一倍
- `\onehalfspacing`: 行距是字号的1.5倍

#### 4.2.5. 段间距

- `\parskip`: 段间距
- `\addtolength{\parskip}{ADD}`: 在原有基础上将段间距增加`ADD`
    - `ADD`
        - 可正可负
        - E.g.: `.4em`

## 5. 文章

- `\maketitle`: 插入标题
    - 包括导言区中定义的标题, 作者, 日期
    - 可通过`titling`宏包自定义标题格式
- `\tableofcontents`: 插入目录
- `\textwidth`: 正文宽度
- `\thepage`: 当前页码
- `\today`: 当日日期
- `\author`: 文章作者
- `\date`: 文章日期

### 5.1. Character Shape

- Bold: `\textbf{CONTENT}`
- Italic: `\textit{CONTENT}`
- Underline: `\underline{CONTENT}`

### 5.2. Space

- `\hfill`: Horizonly Fill
- `\vspace{LENGTH}`: Vertical Space
- `\ `, `\;`: 1 Space
- `\quad`: 4 Space

### 5.3. List

With package `enumitem`:

Unordered List:

```tex
\begin{itemize}
    \item CONTENT{
    \item CONTENT}
\end{itemze}
```

Ordered List:

```tex
\begin{enumerate}
    \item CONTENT{
    \item CONTENT}
\end{enumerate}
```

List can be nested

```tex
\begin{itemize}
    \item CONTENT
    \begin{itemize}
        \item CONTENT
    \end{itemize}
\end{itemize}
```

### 5.4. 组织

- Part
    - `\part{NAME}`
        - 定义在`book`, `ctexbook`宏包中
- Chapter
    - `\chapter{NAME}`
        - 定义在`report`, `ctexrep`宏包中
- Section
    - `\section{NAME}`
    - `\subsection{NAME}`
    - `\subsubsection{NAME}`
- Paragraph
    - `\paragraph{CONTENT}`: 主段落, `CONTENT`会被加粗
    - `\subparagraph{CONTENT}`: 次级段落, `CONTENT`会被加粗
    - `CONTENT`: 段落
        - 段落中的换行符
            - 单个换行符: 输出空格
            - 连续多个换行符: 输出换行符

### 5.5. 数学公式

- AMS LaTeX宏包: `amsmath`
- 行内 Inline 公式
    - 在正文内容中插入公式
    - 插入行内公式
        - 美元符: `$ EQUATION $`
        - 转义圆括号: `\( EQUATION \)`
        - `math`环境: `\begin{math} EQUATION \end{math}`
- 行间 Display 公式
    - 独立排列, 单独成行, 自动居中
    - 插入无编号的行间公式:
        - 转义方括号: `\[ EQUATION \]`
        - `displaymath`环境: `\begin{displaymath} EQUATION \end{displaymath}`
        - `equation*`环境: `\begin{equation*} EQUATION \end{equation*}`
            - `*`表示该环境中公式不编号
        - 双美元符: `$$ EQUATION $$`
            - 不推荐: 在LaTeX中该表示将改变默认行间距
    - 插入有编号的行间公式
        - `equation`环境: `\begin{equation} EQUATION \end{equation}`
- 公式中无需通过控制序列, 可以直接使用的符号: $+ - = ! / ( ) [ ] < > | ' : *$
    - 其余符号均需要通过控制序列使用
    - 空格仅作为源码中标识符的分隔符, 在输出中被忽略
- 公式与标点
    - 行内公式的标点应放在数学模式限定符外
    - 行间公式的标点应放在数学模式限定符内
- 字符组 Group: 将花括号内的字符序列视为一个字符: `{ STRING }`
- 上下标
    - 上标: `[BASE]^INDEX`
    - 下标: `[BASE]_INDEX`
    - 只作用于与`^`, `_`相邻的字符
        - 通过字符组作用于多个字符
- 多行控制
    - 使用`\\`换行
    - 使用`&`对齐: `&`所标记的位置会竖直排列

#### 5.5.1. Control Sequence Reference

- [Detecify LaTeX](http://detexify.kirelabs.org/classify.html "Detecify LaTeX")

- Space:
    - 1 Letter: $A\ B$, $A\;B$
    - 4 Letter: $A \quad B$
- $\sqrt[INDEX]{BASE}$
- $\frac{SUP}{SUB}$
    - 行内公式与行间公式中的显示效果不同: 行内模式分式高度为一行, 行间模式分式高度为多行
        - 强制行内模式分式: $\dfrac{SUP}{SUB}$
        - 强制行间模式分式: $\tfrac{SUP}{SUB}$
    - `xfrac`宏包提供的`\sfrac{SUP}{SUB}`
    - 繁分式: $\cfrac{SUP}{SUB}$
- 小型运算符: $\pm$, $\times$, $\div$, $\cdot$, $\cap$, $\cup$, $\geq$, $\leq$, $\neq$, $\approx$, $\equiv$, $\to$
- 大型运算符: $\sum$, $\prod$, $\lim$, $\int$, $\iint$, $\iiint$, $\iiiint$, $\idotsint$
    - 上下标控制: 大型运算符的上下标在行内为非压缩模式以适应行高, 在行间为压缩模式
        - $\limits$: 限定前一个大型运算符的上下标为压缩模式
            - E.g.: $\sum \limits _{i=0}^{n}$
        - $\nolimits$: 限定前一个大型运算符的上下标为非压缩模式
            - E.g.: $\sum \nolimits _{i=0}^{n}$
- 字面定界符: $()$, $[]$, $\{\}$, $\langle \rangle$, `amsmath`宏包$\lvert \vert \rvert$, `amsmath`宏包$\lVert \Vert \rVert$
    - 定界符大小控制: $\big$, $\Big$, $\bigg$, $\Bigg$
        - 控制紧随其后的定界符的大小
        - 可选: 通过`\SIZEl`, `\SIZEr`修饰左, 右定界符
        - E.g.: $\Bigg ($, $\Biggl ($
- 省略号: $\dots$, $\cdots$, $\vdots$, $\ddots$
    - $\dots$与$\cdots$
        - $x_1, x_2, \dots x_n$: 有下标的序列
        - $1, 2, \cdots n$: 一般序列
- 矩阵: `amsmath`宏包的`pmatrix`, `bmatrix`, `Bmatrix`, `vmatrix`, `Vmatrix`, `smallmatrix`环境
    - E.g.: $\begin{pmatrix} a & b \\ c & d \end{pmatrix}$
- 多行公式
    - 长公式
        - 无对齐: `multline` (有编号), `multline*` (无编号) 环境
            - E.g.: $\begin{multline} x = \\ a + b \end{multline}$
        - 有对齐: `aligned`次环境 (必须包含在数学环境内)
            - E.g.: $\begin{aligned} x & = \\ a & + b \end{aligned}$
    - 公式组
        - 无对齐: `gather` (有编号), `gather*` (无编号) 环境
        - 有对齐: `align` (有编号), `align*` (无编号) 环境
    - 分段函数: `cases`次环境 (必须包含在数学环境内)
        - E.g.: $y = \begin{cases} -x, x \leq 0 \\ x, x > 0 \end{cases}$
    - 避免使用`eqnarray`环境
- $\int_{...}^{...}$
- $\sum_{...}^{...}$
- $\prod_{...}^{...}$
    - Limits:$\sum\limits_{...}$
- ${...} \mapsto {...}$
- $\mathop{}^{...}_{...}$
    - ${|}^{a}_{b}$
- $\underrightarrow{...}$
- $\overrightarrow{...}$
- $\in$
- $\times$
- $\{$
- $\}$
- $\forall$
- $\exists$
- Label:

$$
\begin{equation}
{..}
\end{equation}
$$

- $\overline{...}$
- $\underline{...}$
- $\overbrace{...}^{...}$
- $\underbrace{...}_{...}$
- $\vec{Vector}$
- $< \le = \ge >$
- $\equiv$
- $\sim$
- Logic
    - And:$\land$
    - Or:$\lor$
    - Not:$\lnot$
    - Xor:$\oplus$
    - Implication:$\to$, $\rightarrow$
    - Equivalent:$\leftrightarrow$
        - 等值: $\Leftrightarrow$
    - 推出
        - $\Rightarrow$
        - $\models$
        - $\vdash$
- Enter and align
    - Enter:$\\$
    - Align:$&$
$$
\begin{aligned}
&... \\
&... \\
\end{aligned}
$$
        - Can be nested:
$$
\begin{aligned}
...:\ &...\\
&\begin{aligned}
... &= ... \\
&= ... \\
\end{aligned}\\
& ... \\
\end{aligned}
$$
- Not:$\not<ControlSequence>$
    - E.g.:$\not\equiv$
- Dot
    - $\cdot$
    - $\cdots$
- $\bullet$
- $\infty$
- $\%$
- $%< Comment >$
- Set
    - $\cup$
    - $\cap$
    - $\oslash$
- $\approx$

![1](_v_images/20190608130727303_25250.gif)
![2](_v_images/20190608130811349_8139.gif)
![3](_v_images/20190608130838768_21376.gif)
![4](_v_images/20190608130847293_10490.gif)
![5](_v_images/20190608130854802_22381.gif)
![6](_v_images/20190608130901674_32340.gif)
![7](_v_images/20190608130908407_18934.gif)

### 5.6. 图片

- 通过`graphicx`宏包中的`\includegraphics[width = PROPORTION\textwidth]{PIC_DIR}`插入图片
    - 基目录为当前源代码文件的所在目录

### 5.7. 表格

- `tabular`环境
    - `\begin{tabular}{\|((l|c|r)\|)+}`
        - `{\|((l|c|r)\|)+}`: 列控制
            - `|`: 竖线
            - 列对齐方式: 居左`l`, 居中`c`, 居右`r`
    - `\hline`: 横线
    - `&`: 分列符

E.g.:

```tex
\begin{tabular}{|l|c|r|}
    \hline
    操作系统 & 发行版 & 编辑器 \\
    \hline
    Windows & MikTeX &TexMakerX \\
    \hline
\end{tabular}
```

### 5.8. 浮动体

- 浮动体 Float: 自动调整位置的环境
- 使用浮动体: `\begin{FLOAT}[POSITION]`, `\end{FLOAT}`
    - `FLOAT`: 浮动体名称
        - `figure`: 图片浮动体
        - `table`: 表格浮动体
    - `POSITION`: 可选位置参数
        - `h`, `here`
        - `t`, `top`
        - `b`, `bottom`
        - `p`, `page`: 单独成页/栏
- `\contering`: 使内容在浮动体中居中
- `\caption{CAPTION}`: 设置标题为`CAPTION`
- `\label{LABEL}`: 为浮动体设置用于引用的标识符, 同时在输出文档中进行编号
    - `LABEL`: 标识符, 通常采用`TYPE:NAME`的形式
        - 在文档中必须唯一
        - `TYPE`: 浮动体类型
            - `fig`
            - `table`
    - 必须位于`\caption`之后
- `\ref{LABEL}`: 引用浮动体编号
- `\pageref{LABEL}`: 引用浮动体所在页码
- `~`: 不换行空格 Non-Breaking Space: 确保不自动换行的空格
    - 通常用法: `REF_TYPE~REF_COMMAND`

使用浮动体:

```tex
\begin{FLOAT}[POSITION]
\centering
CONTENT
\caption{CAPTION}
\label{LABEL}
\end{FLOAT}
% ...
\ref{LABEL}
\pageref{LABEL}
```

## 6. Document Class Definition

- Define style and command of a document class
- Source File: `.cls`
- `\ProvidesClass{CLASS_NAME}[DESCRIPTION]`: Define class name and description
- `\LoadClass{CLASS_NAME}`: Specify Base Class
- `\RequirePackage{PACKAGE}`: Use Package
- `\[re]newcommand{COMMAND_NAME}[ARGUMENT_COUNT]{DEFINITION}`
    - `DEFINITION`: Command to execute when invoked
        - Use `#NUMBER` to reference `NUMBER`th argument
    - E.g.:
        - Definition: `\newcommand{\name}[1]{ \centerline{\Huge\scshape{#1}} \vspace{1.25ex}`
        - Usage: `\name{Daniel}`
