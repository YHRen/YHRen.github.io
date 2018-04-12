---
layout: post
title: Latex Trick Collections
published: true
date: 2018-04-11
tag: [latex]
---

### Introduction
Latex is commonly used by researchers in science and engineering for writing papers.
There are always some new tricks I learned from here and there. 
I intend to use this post to gather as many as these fragmented tricks as possible.


### A Makefile for all my drafts
One drawback of latex is that one has to compile several times using `pdflatex`
and `bibtex` to make sure the cross-references are correct. One can find some 
explanations [here](https://tex.stackexchange.com/questions/342464/using-bibtex-and-pdflatex-why-are-three-latex-runs-needed).
So, it makes sense to me to pack all these commands in a `makefile`, and simply 
type `make` to compile the `.tex` file and generate the `.pdf`. Then, I realize 
that we can do much more using `make`. For example, we can open the `pdf` file 
from the terminal (on Mac, we can simply say `open`), and count the number of
words we have put (`-inc -total` is for counting the included files in the main 
`tex` file).

``` bash
fn = epiSen_main
exe = pdflatex

.PHONY : all compile open count clean purge

all : compile
compile :
  $(exe) $(fn).tex
  bibtex   $(fn).aux
  $(exe) $(fn).tex
  $(exe) $(fn).tex
  $(exe) $(fn).tex

open :
  open $(fn).pdf

count :
  texcount $(fn).tex -inc -total

clean :
  rm -f $(fn).log $(fn).out $(fn).aux  $(fn).toc $(fn).blg $(fn).bbl

purge :
  rm -f $(fn).log $(fn).out $(fn).aux $(fn).pdf $(fn).toc $(fn).blg $(fn).bbl
```

### A command for making comments
Sometime I need to leave some comments for myself or my coauthors. 
The following `newcommand` becomes very handy, i.e. 
`\newcommand{\ray}[1]{\textcolor{blue}{Ray: {#1}}}`. This will turn my comments
into blue and prefix my name. An example of usage is something like this: 
`\ray{I suggest we should do the following...}`.


