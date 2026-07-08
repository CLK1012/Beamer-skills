# TikZ Diagram Components

Use this reference for Beamer pages where arrows or diagrams are the main visual element.

## Preamble

```tex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,calc,positioning,shapes.geometric}

\definecolor{maincolor}{RGB}{14,44,95}
\definecolor{accentblue}{RGB}{37,99,235}
\definecolor{accentgreen}{RGB}{5,150,105}
\definecolor{accentred}{RGB}{225,29,72}
\definecolor{accentamber}{RGB}{217,119,6}
\definecolor{bgcolor}{RGB}{248,250,252}

\tikzset{
  processbox/.style={
    rounded corners=4pt,
    minimum width=2.15cm,
    minimum height=1.05cm,
    align=center,
    font=\bfseries\small,
    text=white,
    inner sep=7pt
  },
  flowarrow/.style={
    -{Stealth[length=7pt,width=8pt]},
    line width=2.5pt,
    maincolor!70,
    rounded corners=4pt
  },
  note/.style={
    align=center,
    font=\scriptsize,
    text=maincolor!75,
    fill=bgcolor,
    inner sep=1.5pt
  },
  stage/.style={
    circle,
    minimum size=1.34cm,
    align=center,
    font=\bfseries\small,
    text=white,
    inner sep=2pt
  },
  ringarrow/.style={
    -{Stealth[length=8pt,width=9pt]},
    line width=5pt,
    line cap=round
  }
}
```

## Horizontal Process With Feedback Arrow

```tex
\begin{frame}{箭头为主要视觉元素的进程图}
  \centering
  \begin{tikzpicture}[x=1cm,y=1cm]
    \node[processbox,fill=maincolor] (a) at (0,0) {需求\\识别};
    \node[processbox,fill=accentblue] (b) at (3.15,0) {方案\\设计};
    \node[processbox,fill=accentgreen] (c) at (6.3,0) {执行\\落地};
    \node[processbox,fill=accentamber] (d) at (9.45,0) {复盘\\优化};

    \draw[flowarrow] (a.east) -- (b.west);
    \draw[flowarrow] (b.east) -- (c.west);
    \draw[flowarrow] (c.east) -- (d.west);

    \node[note] at ($(a)!0.5!(b)+(0,0.62)$) {输入转化};
    \node[note] at ($(b)!0.5!(c)+(0,0.62)$) {路径确认};
    \node[note] at ($(c)!0.5!(d)+(0,0.62)$) {结果回收};

    \draw[flowarrow,accentred]
      ($(d.south)+(0,-0.35)$) .. controls +(0,-1.35) and +(0,-1.35) .. ($(a.south)+(0,-0.35)$);
    \node[note,text=accentred!85] at (4.7,-2.35) {反馈回路：将复盘结论带回下一轮需求识别};
  \end{tikzpicture}
\end{frame}
```

Move the feedback label down if it overlaps the arrow.

## Circular Arrow Loop

Keep nodes and arrows on the same radius. Leave angle gaps around nodes to avoid overlap.

```tex
\begin{frame}{圆环箭头的闭环图}
  \centering
  \begin{tikzpicture}[x=1cm,y=1cm]
    \coordinate (center) at (0,0);

    \draw[ringarrow,maincolor!82] (74:2.75) arc[start angle=74,end angle=34,radius=2.75];
    \draw[ringarrow,accentblue!82] (2:2.75) arc[start angle=2,end angle=-38,radius=2.75];
    \draw[ringarrow,accentgreen!82] (-70:2.75) arc[start angle=-70,end angle=-110,radius=2.75];
    \draw[ringarrow,accentamber!88] (-142:2.75) arc[start angle=-142,end angle=-182,radius=2.75];
    \draw[ringarrow,accentred!82] (146:2.75) arc[start angle=146,end angle=106,radius=2.75];

    \node[stage,fill=maincolor] at (90:2.75) {观察};
    \node[stage,fill=accentblue] at (18:2.75) {判断};
    \node[stage,fill=accentgreen] at (-54:2.75) {行动};
    \node[stage,fill=accentamber] at (-126:2.75) {评估};
    \node[stage,fill=accentred] at (162:2.75) {修正};

    \node[align=center,text=maincolor,font=\bfseries\Large] at (center) {闭环\\机制};
    \node[align=center,text=maincolor!65,font=\small] at (0,-0.78) {持续迭代};
  \end{tikzpicture}
\end{frame}
```

## Curved Gradient Arrow With Horizontal Text

Approximate a white-to-blue gradient and left-thin/right-thick arrow body with many short line segments. Keep the final arrowhead aligned with the curve tangent.

```tex
\begin{frame}{弧形渐变箭头介绍页}
  \centering
  \begin{tikzpicture}[x=1cm,y=1cm]
    \foreach \i in {0,...,55} {
      \pgfmathsetmacro{\t}{\i/56}
      \pgfmathsetmacro{\tn}{(\i+1)/56}
      \pgfmathsetmacro{\xa}{-4.85+9.55*\t}
      \pgfmathsetmacro{\ya}{-1.70+2.95*\t-1.0*sin(180*\t)}
      \pgfmathsetmacro{\xb}{-4.85+9.55*\tn}
      \pgfmathsetmacro{\yb}{-1.70+2.95*\tn-1.0*sin(180*\tn)}
      \pgfmathsetmacro{\lw}{4+14*\t}
      \pgfmathtruncatemacro{\mix}{round(100*\t)}
      \draw[line width=\lw pt,line cap=round,draw=accentblue!\mix!white]
        (\xa,\ya) -- (\xb,\yb);
    }

    \draw[-{Stealth[length=24pt,width=30pt]},line width=18pt,line cap=butt,draw=accentblue]
      (4.46,1.10) -- (5.34,1.66);

    \node[align=left,text=maincolor,font=\scriptsize,inner sep=0pt,text width=2.25cm]
      at (-3.15,-0.55) {\bfseries 识别现状\\梳理对象边界\\明确问题来源};
    \node[align=left,text=maincolor,font=\scriptsize,inner sep=0pt,text width=2.25cm]
      at (-0.9,-0.15) {\bfseries 聚焦问题\\筛选关键变量\\形成判断依据};
    \node[align=left,text=maincolor,font=\scriptsize,inner sep=0pt,text width=2.25cm]
      at (1.35,0.45) {\bfseries 推动行动\\配置执行路径\\沉淀阶段成果};
    \node[align=left,text=maincolor,font=\scriptsize,inner sep=0pt,text width=2.25cm]
      at (3.35,1.92) {\bfseries 形成提升\\汇总反馈信息\\进入下一轮优化};

    \node[align=center,text=maincolor,font=\bfseries\Large] at (0,-2.65) {从起点到目标的连续推进};
    \node[align=center,text=maincolor!65,font=\small] at (0,-3.12)
      {适合表达发展路径、能力跃迁、战略推进或研究脉络。};
  \end{tikzpicture}
\end{frame}
```

Tuning rules:

- Change `-1.0*sin(180*\t)` to control downward convexity.
- Increase `\lw` slope to make the arrow more dramatically thick on the right.
- Align the arrowhead to the last curve tangent. If it separates, start the arrowhead line slightly before the final body endpoint.
- Move text blocks vertically away from the arrow body; keep them horizontal unless the user asks for sloped labels.
