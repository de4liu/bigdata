# Issues with Line Endings in Text Files

<!-- MarkdownTOC -->

- [How would I know the line endings were `\r\n` instead of '\n'?](#how-would-i-know-the-line-endings-were-rn-instead-of-n)
- [How to fix it?](#how-to-fix-it)
- [How to prevent this?](#how-to-prevent-this)

<!-- /MarkdownTOC -->

Windows text files by default have line endings of `\r\n`, but linux/unix text files have line endings of `\n`. Often times, files originated from windows may not work for linux because of this (e.g. for python-based mappers and reducers). 

<a id="how-would-i-know-the-line-endings-were-rn-instead-of-n"></a>
### How would I know the line endings were `\r\n` instead of '\n'?

A quick way to see if the file has windows line ending is through vim.
```shell
vi file.py 
```
type `:set list` to see line endings.  If it is windows line endings, you will see `^M` at end of each line. Type `:q` to exit the vi editor.

<a id="how-to-fix-it"></a>
### How to fix it?
To replace windows line endings with linux ones, use linux command:
```shell
sed -i 's/\r//g' file.py
```
<a id="how-to-prevent-this"></a>
### How to prevent this?

If you have to edit a file in windows, please create the file in Linux or copy an existing linux-originated file (so it has the right endings), then edit it.

- **Notepad++**: You can view line endings using tool bar button "&para;". At the status bar, you can switch between "Unix(LF)" and "Windows (CR LF)".
- **Sublime Text**: This page explains [how to set default line endings to linux](https://blog.hostonnet.com/how-to-set-unix-as-default-line-ending-in-sublime-text). Here is how you [may convert it](https://github.com/vontio/sublime-line-endings-unify).
