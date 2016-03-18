Markdown 是一种方便记忆、书写的纯文本标记语言。

##  安装Package Control

安装包控制扩展可以方便地为st添加拓展。打开st，按下组合键Control + `，出现控制台，输入

```python
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

当看到代码最后一行提示的时候说明安装成功，此时重启st，可在Preferences -> Package Settings看到Package Control。

## 安装markdown preview

按下键Ctrl+Shift+p调出命令面板，找到Package Control: install Pakage这一项。搜索markdown preview，点击安装。

## 关于编辑

 － 按Ctrl + N 新建一个文档

 － 按Ctrl + Shift + P

 － 使用Markdown语法编辑文档

 － 语法高亮，输入ssm 后回车(Set Syntax: Markdown)

## 在浏览器预览Markdown文档

 - 按Ctrl + Shift + P

 - 输入mp 后回车(Markdown Preview: current file in browser)

 - 此时就可以在浏览器里看到刚才编辑的文档了;

 - 若设置了快捷键,直接(alt+m/..)即可.

## 关于快捷键

st支持自定义快捷键，markdown preview默认没有快捷键，我们可以自己为preview in browser设置快捷键。

方法是在Preferences -> Key Bindings User打开的文件的中括号中添加以下代码(可在Key Bindings Default找到格式):

```
{ "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} }
```

`alt+m`可设置为您自己喜欢的按键。