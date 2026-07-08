# Beamer Navigation And Numbering

Use this reference when a deck needs a custom top navigation bar, automatic section/subsection numbering in frame titles, or clickable subsection dots.

## Required Packages And Theme Order

For Chinese decks:

```tex
\documentclass[aspectratio=169]{ctexbeamer}
\usepackage{xcolor}
\usepackage{graphicx}

\definecolor{maincolor}{RGB}{14,44,95}
\definecolor{accentcolor}{RGB}{37,99,235}
\definecolor{bgcolor}{RGB}{248,250,252}

\setbeamercolor{background canvas}{bg=bgcolor}
\setbeamercolor{structure}{fg=maincolor}
\setbeamerfont{frametitle}{series=\bfseries}

% Load smoothbars before redefining frametitle.
\useoutertheme[subsection=false]{smoothbars}
```

## Automatic "2.1 Title" Frame Titles

Use `\subsection` before each content node. Chapter title pages should appear after `\section` and before the first `\subsection`.

```tex
\makeatletter
\newcommand{\CurrentSubsectionNumber}{}
\newcommand{\ClearFrameSectionNumber}{\gdef\CurrentSubsectionNumber{}}
\newcommand{\FrameSectionNumber}{\CurrentSubsectionNumber}

\let\nav@orig@beamer@subsection\beamer@subsection
\def\beamer@subsection[#1]#2{%
  \nav@orig@beamer@subsection[{#1}]{#2}%
  \xdef\CurrentSubsectionNumber{\number\c@section.\number\c@subsection\space}%
}
\makeatother

\setbeamercolor{frametitle}{fg=maincolor,bg=bgcolor}
\setbeamertemplate{frametitle}{%
  \nointerlineskip%
  \begin{beamercolorbox}[wd=\paperwidth,ht=3.2ex,dp=1.0ex,leftskip=3ex,rightskip=3ex]{frametitle}
    \usebeamerfont{frametitle}\FrameSectionNumber\insertframetitle
  \end{beamercolorbox}%
}
```

Use `\ClearFrameSectionNumber` before appendix/end pages that should not show a stale `3.1` prefix.

## Single-Block Top Navigation Bar

This navigation pattern uses one blue bar, short section titles as chapter labels, and subsection dots as progress nodes.

```tex
\setbeamercolor{section in head/foot}{fg=white,bg=maincolor}
\setbeamercolor{subsection in head/foot}{fg=white!90,bg=maincolor!90!black}
\setbeamerfont{section in head/foot}{size=\small}

\makeatletter
\newbox\nav@barbox
\newbox\nav@currsecname
\newbox\nav@currsecdots
\newbox\nav@currsecbox
\newbox\nav@tempbox
\newdimen\nav@w
\newdimen\nav@barheight
\newdimen\nav@bartopskip

\nav@barheight=10.0ex
\nav@bartopskip=2.9ex

\def\nav@flushcurrentsection{%
  \ifvoid\nav@currsecname\else
    \nav@w=\wd\nav@currsecname
    \ifdim\wd\nav@currsecdots>\nav@w \nav@w=\wd\nav@currsecdots \fi
    \setbox\nav@currsecbox=\vbox{%
      \hbox to \nav@w{\hss\box\nav@currsecname\hss}%
      \vskip1.5pt
      \hbox to \nav@w{\hss\unhbox\nav@currsecdots\hss}%
    }%
    \ifvoid\nav@barbox
      \setbox\nav@barbox=\hbox{\box\nav@currsecbox}%
    \else
      \setbox\nav@barbox=\hbox{\unhbox\nav@barbox\hskip 2em plus 2fill\box\nav@currsecbox}%
    \fi
  \fi
}

\def\insertnavigation#1{%
  \vbox{{%
    \usebeamerfont{section in head/foot}\usebeamercolor[fg]{section in head/foot}%
    \setbox\nav@barbox=\box\voidb@x
    \setbox\nav@currsecname=\box\voidb@x
    \setbox\nav@currsecdots=\box\voidb@x
    \dohead%
    \nav@flushcurrentsection%
    \hbox to #1{\hskip 2em plus 1fill\unhbox\nav@barbox\hskip 2em plus 1fill}%
  }}%
}

\defbeamertemplate*{headline}{custom template navbar}{%
  \begin{beamercolorbox}[wd=\paperwidth,ht=\nav@barheight,dp=0ex]{section in head/foot}
    \vbox to \nav@barheight{%
      \vskip\nav@bartopskip
      \insertnavigation{\paperwidth}%
      \vfil
    }%
  \end{beamercolorbox}%
}
\setbeamertemplate{headline}[custom template navbar]

\def\sectionentry#1#2#3#4#5{%
  \ifnum#5=\c@part%
    \nav@flushcurrentsection
    \setbox\nav@currsecname=\hbox{%
      \def\insertsectionhead{#2}%
      \def\insertsectionheadnumber{#1}%
      \def\insertpartheadnumber{#5}%
      \usebeamerfont{section in head/foot}%
      \usebeamercolor[fg]{section in head/foot}%
      \ifnum\c@section=#1%
        \hyperlink{Navigation#3}{{\usebeamertemplate{section in head/foot}}}%
      \else%
        \hyperlink{Navigation#3}{{\usebeamertemplate{section in head/foot shaded}}}%
      \fi%
    }%
    \setbox\nav@currsecdots=\box\voidb@x
  \fi\ignorespaces
}

% Dots represent subsections, linked to subsection start pages.
\def\beamer@subsectionentry#1#2#3#4#5{%
  \ifnum#1=\c@part\ifnum#2>0\ifnum#3>0%
    \setbox\nav@tempbox=\hbox{%
      \hyperlink{Navigation#4}{%
        \usebeamerfont{mini frame}\usebeamercolor{mini frame}%
        \ifnum\c@section=#2%
          \ifnum\c@subsection=#3%
            \usebeamertemplate{mini frame}%
          \else%
            \usebeamertemplate{mini frame in current subsection}%
          \fi%
        \else%
          \usebeamertemplate{mini frame in other section}%
        \fi%
      }%
    }%
    \ifvoid\nav@currsecdots
      \setbox\nav@currsecdots=\hbox{\box\nav@tempbox}%
    \else
      \setbox\nav@currsecdots=\hbox{\unhbox\nav@currsecdots\hskip3pt\box\nav@tempbox}%
    \fi
  \fi\fi\fi\ignorespaces
}

% Do not let pages generate dots.
\def\slideentry#1#2#3#4#5#6{}%
\makeatother
```

## Usage Pattern

```tex
\section[第一部分]{第一部分：背景与目标}
\begin{frame}[c]\centering{\huge\bfseries\insertsection}\end{frame}

\subsection{导航说明}
\begin{frame}{导航条说明}
  Content...
\end{frame}
```

Never put `\subsection` before a chapter title page if that title page should not create a dot.
