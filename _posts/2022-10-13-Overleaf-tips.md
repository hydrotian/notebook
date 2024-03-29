---
layout: post
title: Overleaf Tips
date: 2022-10-13 15:19
author: Tian
comments: true
categories: [Other]
tags: Null
---
Some tips about Overleaf LaTex manuscript preparation

## Track Change Document

Overleaf is a great online LaTeX tool. There's a trend to use it in future paper development with other coauthors. Recently I submitted a manuscript using Overleaf. The process was smooth but when it comes to revision, I have to provide a track-change document that is not very easy to generate in Overleaf. Although I can use the `trackchange` package to track changes in the document, there's no way to include the changes in the compiled pdf file. After some googling, here's my solution:

- First using [latexdiff](http://www.ctan.org/pkg/latexdiff) to generate the diff.tex file between the two documents. There's an [online tool](https://3142.nl/latex-diff/) for this step.
- Second add the following header to the diff.tex document (right before `\begin{document}` line if using overleaf). A package called [ulem](https://ctan.org/pkg/ulem) is called.

```latex
{% raw %}
%DIF PREAMBLE EXTENSION ADDED BY LATEXDIFF
%DIF UNDERLINE PREAMBLE %DIF PREAMBLE
\RequirePackage[normalem]{ulem} %DIF PREAMBLE
\RequirePackage{color}\definecolor{RED}{rgb}{1,0,0}\definecolor{BLUE}{rgb}{0,0,1} %DIF PREAMBLE
\providecommand{\DIFadd}[1]{{\protect\color{blue}\uwave{#1}}} %DIF PREAMBLE
\providecommand{\DIFdel}[1]{{\protect\color{red}\sout{#1}}} %DIF PREAMBLE
%DIF SAFE PREAMBLE %DIF PREAMBLE
\providecommand{\DIFaddbegin}{} %DIF PREAMBLE
\providecommand{\DIFaddend}{} %DIF PREAMBLE
\providecommand{\DIFdelbegin}{} %DIF PREAMBLE
\providecommand{\DIFdelend}{} %DIF PREAMBLE
%DIF FLOATSAFE PREAMBLE %DIF PREAMBLE
\providecommand{\DIFaddFL}[1]{\DIFadd{#1}} %DIF PREAMBLE
\providecommand{\DIFdelFL}[1]{\DIFdel{#1}} %DIF PREAMBLE
\providecommand{\DIFaddbeginFL}{} %DIF PREAMBLE
\providecommand{\DIFaddendFL}{} %DIF PREAMBLE
\providecommand{\DIFdelbeginFL}{} %DIF PREAMBLE
\providecommand{\DIFdelendFL}{} %DIF PREAMBLE
%DIF END PREAMBLE EXTENSION ADDED BY LATEXDIFF
{% endraw %}
```

## Line Number

Adding following lines to activate line number

```latex
\usepackage{lineno}
\linenumbers
```

## Font

Change font of the document

```latex
\usepackage[T1]{fontenc} % this line will allow using accented characters as individual glyphs
\usepackage{tgbonum} % load a font family
\begin{document}
\fontfamily{cmss}\selectfont % cmtt is "Computer Modern Sans Serif"

% change font for a section
%{\fontfamily{pcr}\selectfont % pcr is "courier"
%enter text here
%}

\end{document}
```

See more info [here](https://www.overleaf.com/learn/latex/Font_typefaces#Changing_the_default_document_fonts). About the accented characters, see [this](https://tex.stackexchange.com/a/677/283616).