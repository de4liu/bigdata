# How to Convert Markdown to PDF

Our assignments are distributed in markdown files. If you want to leverage the markdown file, you will need a workflow that converts markdown to pdf. Here are some of the options:

<!-- MarkdownTOC -->

- [1. Online Markdown to PDF Converter](#1-online-markdown-to-pdf-converter)
- [2. R-Studio - Requires Pandoc](#2-r-studio---requires-pandoc)
- [3. Jupyter](#3-jupyter)
- [4. Specialized Markdown Editor](#4-specialized-markdown-editor)
- [5. Sublime Text + Markdown Preview + Chrome](#5-sublime-text--markdown-preview--chrome)

<!-- /MarkdownTOC -->

<a id="1-online-markdown-to-pdf-converter"></a>
## 1. Online Markdown to PDF Converter

The easiest is perhaps the cloud-based markdown to PDF converters

- [https://cloudconvert.com/md-to-pdf](https://cloudconvert.com/md-to-pdf): multipurpose converter including md to pdf.
- [Dillinger.io](https://dillinger.io/): editing and expert to pdf, html
- [Markdowntopdf.com](http://www.markdowntopdf.com/): quick and easy

<a id="2-r-studio---requires-pandoc"></a>
## 2. R-Studio - Requires Pandoc

You can open the `md` file in R-studio, and save as PDF (this is done through Pandoc).

To install Pandoc, you can download it from [Pandoc](http://pandoc.org/installing.html). Note that Latex software, such as MikTex is also required.

<a id="3-jupyter"></a>
## 3. Jupyter

You can open the `md` file in jupyter notebook, and save as PDF (this is done through Pandoc).

Alternatively, you may choose to print the file, then using the browser's built-in save as PDF capability. Specifically, go to File > Print Preview > Using the Browser's Save As PDF (e.g. Chrome) or Print + Microsoft Print to PDF (Internet Explorer). 

<a id="4-specialized-markdown-editor"></a>
## 4. Specialized Markdown Editor

Specialized Markdown Software such as [Typora](https://typora.io/#download) or [Markdown Monster](https://markdownmonster.west-wind.com/) tend to have their own markdown to PDF converter.

<!-- ```
pandoc -o filename.pdf -f markdown --latex-engine=xelatex -V  \
geometry:"margin=1in" filename.md
``` -->

<a id="5-sublime-text--markdown-preview--chrome"></a>
## 5. Sublime Text + Markdown Preview + Chrome

If use [sublime text](https://www.sublimetext.com/3) as your text editor. There are markdown editing and preview packages you can install.

- Use sublime package control to install
    + [MarkdownEditing](https://github.com/SublimeText-Markdown/MarkdownEditing)
    + [Markdown Preview](https://github.com/revolunet/sublimetext-markdown-preview#using-package-control-recommended)
- <kbd>Alt+M</kbd> to view markdown in Chrome, <kbd>Ctrl+p</kbd> to print to PDF.
