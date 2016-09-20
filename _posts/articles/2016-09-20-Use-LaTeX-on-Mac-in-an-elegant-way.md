---
layout: post
title: Use LaTeX on Mac in an elegant way
excerpt: "Install MacTeX + Sublime Text + Skim on Mac."
modified: 2016-09-020T14:17:25-04:00
categories: articles
tags: [Mac, LaTex, Sublime Text]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---


Recently I am sick of using [Word for Mac](https://products.office.com/en-us/mac/microsoft-office-for-mac) (though 2016 is much better) to edit CVs and articles, and I quickly decided to turn to LaTeX instead, which is more flexible and logical.


I work most of the time on MacBook. The most popular LaTeX distribution for Mac is [MacTeX](https://tug.org/mactex/) (for Windows: [MiKTeX](http://miktex.org/) or [TeXlive](https://www.tug.org/texlive/)). 

Once I had this installed I needed an editor. [TeXworks](https://www.tug.org/texworks/) is okay, but it is not a very decent environment and it is hard to manage documents with more than one `.tex` file either. So the idea of using [Sublime Text](https://www.sublimetext.com/) (my favourite editor) just popped into my head. As it says on the website, Sublime Text is 

>   "The text editor you'll fall in love with." 

It has a neat environment with a huge number of plugins, and most of all, fast.

[Skim](http://skim-app.sourceforge.net/)is an application, which detects .pdf updates automatically. Moreover, [Skim](http://skim-app.sourceforge.net/) can be integrated with [Sublime Text](https://www.sublimetext.com/) in such a way that it checks for updates every time, you perform build in [Sublime Text](https://www.sublimetext.com/).

All the tools are settled, let’s get them work on Mac.


### LaTeX + SublimeText + Skim setup

#### Step 0
Install **LaTeX** distribution (for Mac OS X: MacTeX, for Windows: MiKTeX or TeXlive).

#### Step 1
Install **SublimeText**

Optionally: Install SublimeText Package Control (if you didn’t do that already) – it will be easier to install LaTeXTools package.

Install [LaTeXTools](https://github.com/SublimeText/LaTeXTools) plugin. With [SublimeText Package Control](https://packagecontrol.io/) installed: click `Command+SHIFT+P` on Mac.

#### Step 2
Install **Skim** 


In Skim: go to Preferences->Sync and set ‘Preset’ to SublimeText.

<img src="/images/skim.png" alt="image">
	


After that you just need to build LaTeX document in SublimeText with Command+B (Mac). Open the generated `.pdf` in Skim, then every time you rebuild it in SublimeText – it will be refreshed automatically.

If you have multiple documents add ` %!TEX root = <master file name>`  at the beginning of every file.

*Enjoy your **LaTeX** on **Mac**!* 



PS: A **brief** but **useful** introduction to **LaTeX** 

[The Not So Short Introduction to LATEX 2ε](https://tobi.oetiker.ch/lshort/lshort.pdf)




