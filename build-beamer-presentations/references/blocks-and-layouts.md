# Blocks, Text Layouts, And Images

Use this reference for Beamer boxed text blocks, equal-height columns, custom rounded tag blocks, and image/caption layouts.

## Layout Selection

- Prefer left-right side-by-side text blocks for most content slides. They scan well, make comparison easy, and keep reusable templates visually stable.
- Use two columns when each side carries a comparable idea, argument, method, evidence set, or result.
- Use three columns when presenting parallel categories such as "background / method / result" or "problem / action / outcome".
- Use vertical stacked blocks only when the content is inherently sequential, hierarchical, or too dense for columns.
- In side-by-side layouts, keep blocks equal height with fixed-height `minipage`s. If one block has less text, preserve empty space instead of shrinking the block.

## Standard Beamer Blocks

```tex
\setbeamertemplate{blocks}[rounded][shadow=false]
\setbeamercolor{block title}{bg=maincolor,fg=white}
\setbeamercolor{block body}{bg=maincolor!8,fg=maincolor!90}
\setbeamercolor{block title alerted}{bg=alertcolor,fg=white}
\setbeamercolor{block body alerted}{bg=alertcolor!8,fg=maincolor!90}
\setbeamercolor{block title example}{bg=examplecolor,fg=white}
\setbeamercolor{block body example}{bg=examplecolor!8,fg=maincolor!90}
```

Use standard `block`, `alertblock`, and `exampleblock` for most content.

## Equal-Height Horizontal Blocks

The key is a fixed-height `minipage` inside each block.

```tex
\begin{columns}[T]
  \begin{column}{0.31\textwidth}
    \begin{block}{常规块 (Block)}
      \begin{minipage}[t][2.15cm][t]{\linewidth}
        \raggedright
        这是一个普通块。短文本也保持同样高度。
      \end{minipage}
    \end{block}
  \end{column}
  \hfill
  \begin{column}{0.31\textwidth}
    \begin{alertblock}{警示块 (Alert Block)}
      \begin{minipage}[t][2.15cm][t]{\linewidth}
        \raggedright
        用于强调重要事项。
      \end{minipage}
    \end{alertblock}
  \end{column}
  \hfill
  \begin{column}{0.31\textwidth}
    \begin{exampleblock}{示例块 (Example Block)}
      \begin{minipage}[t][2.15cm][t]{\linewidth}
        \raggedright
        用于展示示例或结果。
      \end{minipage}
    \end{exampleblock}
  \end{column}
\end{columns}
```

If one block has less text, leave whitespace via fixed height instead of changing block height.

## Vertical Stacked Blocks

```tex
\begin{block}{第一层内容}
  这里放背景、方法或核心论点。
\end{block}

\begin{alertblock}{第二层强调}
  这里放关键提醒、主要发现或待解决问题。
\end{alertblock}

\begin{exampleblock}{第三层示例}
  这里放短案例、操作结果或后续工作。
\end{exampleblock}
```

## Rounded Tag Block With Partial-Width Label

Use `tcolorbox` when the top label should be narrower than the body and both should have rounded corners.

Preamble:

```tex
\usepackage[skins]{tcolorbox}

\newenvironment{TaggedBlock}[1]{%
  \par\noindent
  \begin{tcolorbox}[
    enhanced,
    width=.34\linewidth,
    colback=maincolor,
    colframe=maincolor,
    coltext=white,
    boxrule=0pt,
    arc=3pt,
    outer arc=3pt,
    boxsep=0pt,
    left=.7ex,
    right=.7ex,
    top=.45ex,
    bottom=.45ex,
    before skip=0pt,
    after skip=0pt,
  ]
    \scriptsize\bfseries\mbox{#1}
  \end{tcolorbox}%
  \vskip0pt
  \begin{tcolorbox}[
    enhanced,
    width=.96\linewidth,
    colback=maincolor!8,
    colframe=maincolor!8,
    coltext=maincolor!90,
    boxrule=0pt,
    arc=4pt,
    outer arc=4pt,
    boxsep=0pt,
    left=1ex,
    right=1ex,
    top=1ex,
    bottom=1ex,
    before skip=0pt,
    after skip=0pt,
  ]
}{%
  \end{tcolorbox}%
}
```

Use:

```tex
\begin{TaggedBlock}{左侧内容}
  \begin{minipage}[t][4.0cm][t]{.94\linewidth}
    \begin{itemize}
      \item 替换为你的第一条结论或要点。
      \item 替换为你的第二条结论或要点。
      \item 替换为你的第三条结论或要点。
    \end{itemize}
    \vfill
    可在此补充方法、数据来源或关键解释。
  \end{minipage}
\end{TaggedBlock}
```

Adjust `.34\linewidth` for label width. Keep label text short; use `\mbox` to prevent awkward wrapping.

## Image And Caption Layout

Use columns and match the left block height to the image height.

```tex
\begin{columns}[T]
  \begin{column}{0.42\textwidth}
    \begin{TaggedBlock}{左侧内容}
      \begin{minipage}[t][4.0cm][t]{.94\linewidth}
        \begin{itemize}
          \item 第一条结论。
          \item 第二条结论。
          \item 第三条结论。
        \end{itemize}
        \vfill
        可在此补充方法或数据来源。
      \end{minipage}
    \end{TaggedBlock}
  \end{column}
  \begin{column}{0.54\textwidth}
    \centering
    \includegraphics[width=\linewidth,height=4.0cm,keepaspectratio]{figures/example.png}

    \vspace{0.2cm}
    {\scriptsize\color{maincolor!65} 图 1：插图说明}
  \end{column}
\end{columns}
```

If the image is a PDF page snapshot, create it with a normal external tool first and include the resulting `.png`.
