# MSBA 6330 Homework - General Submission Guidelines

<!-- MarkdownTOC -->

- [1. Clearly indicate which question you're answering.](#1-clearly-indicate-which-question-youre-answering)
- [2. Credit sources to avoid plagiarism](#2-credit-sources-to-avoid-plagiarism)
- [3. Submit all your answers in acceptable file formats \(html, pdf, docx\), preferrably in one file.](#3-submit-all-your-answers-in-acceptable-file-formats-html-pdf-docx-preferrably-in-one-file)
- [4. How to submit a Jupyter notebook](#4-how-to-submit-a-jupyter-notebook)
- [5. How to take a screenshot](#5-how-to-take-a-screenshot)

<!-- /MarkdownTOC -->

There are typically two types of questions:short answers and hands-on. The hands-on part is usually a list of to-dos (e.g. calculate the distribution of income), to which you respond with commands/scripts/answers. 

With our computing environment, 

- perhaps the easiest way of preparing a homework is to write in **Jupyter Notebook** then export it to PDF or HTML for submission. 
- Alternatively, you may prepare your homework in a **Mark Down** file and convert it to PDF or HTML for submission. If you're new to Mark Down, a good software to start is [Typora](https://typora.io/). 

<a id="1-clearly-indicate-which-question-youre-answering"></a>
### 1. Clearly indicate which question you're answering. 

The best practice is to copy the original question and provide your answer below. For examples:

- 1\. show the content of the `mydir` directory in HDFS: 
    ```bash
    hadoop fs -ls mydir
    ```
- 2\. show the content of the `mydir` directory in HDFS and the view the first five lines of data.csv, e.g. 
    ```bash
    hadoop fs -ls mydir # show content of mydir
    hadoop fs -head -n 5 mydir/data.csv   # show first five lines
    ```

When there are many codes, it may be convenient to write it in [**Markdown**](https://guides.github.com/features/mastering-markdown/). You use any text editor, or markdown editors such as Typora to write markdown.

<a id="2-credit-sources-to-avoid-plagiarism"></a>
### 2. Credit sources to avoid plagiarism

For short answers, if you use others' work as part of your answer, please properly cite your source. While it is fine to use web or other sources as a reference, you are **prohibited from to lift whole paragraphs** from the Internet. This is considered plagiarism, especially when you do not indicate the boundary of the copied content. When you quote a whole sentence or more, please add quotation marks around the copied content and indicate sources, so that we know you did not write that. 

Please refer to the following example for inline citation and bibliography styles. Please use the author-year format for inline citations. 

This phenomenon has been mentioned in several sources include a web page [^1] and a journal paper **(Yeh 1996)**. [^2]

[^1]: "Zen and the Art of the Internet." http://freenet.buffalo.edu/~popmusic/zen10.txt 

[^2]: Yeh, Michelle. "The 'Cult of Poetry' in Contemporary China." Journal  of Asian Studies  55 (1996): 51-80. 


<a id="3-submit-all-your-answers-in-acceptable-file-formats-html-pdf-docx-preferrably-in-one-file"></a>
### 3. Submit all your answers in acceptable file formats (html, pdf, docx), preferrably in one file.

The acceptable file formats include  **Word, PDF, and HTML** but *not the markdown file (.md)*. If your homework is written with markdown, convert it to HTML or PDF. 

**Do not zip your files**: If your submission includes multiple files, attach them individually. 

<a id="4-how-to-submit-a-jupyter-notebook"></a>
### 4. How to submit a Jupyter notebook 

Convert a Notebook to HTML or PDF, using one of the following ways: 

- Within jupyter, use "File > Export as > HTML (or PDF)". If you're using PDF, please make sure there is no cut off of long lines.

<a id="5-how-to-take-a-screenshot"></a>
### 5. How to take a screenshot
Ocasionally you may need to submit a screenshot. Here are some tips

**Windows** : In windows 7 or above, you may use the built-in **Snipping tool** , which allows you to select an area on screen and capture it. In Windows 10, you may use shortcut "**Win+Shift+S**" to select a screen area to save.

**Mac** : On Mac OS, Press **Command-Shift-4**, and then drag the cross-hair pointer to select the area. 

