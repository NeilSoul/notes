Ghost默认是没有代码高亮功能的，我们可以通过hightlightjs这个插件做到代码高亮。[highlightjs](https://highlightjs.org/)是一个css和js库。

## 通过官网CDN链接引入
编辑`content/themes/[specified_themes]/default.hbs`文件，在`<head></head>`中插入如下代码
```html
<link href="http://cdn.bootcss.com/highlight.js/8.0/styles/monokai_sublime.min.css" rel="stylesheet">  
<script src="http://cdn.bootcss.com/highlight.js/8.0/highlight.min.js"></script>  
```

## 也可以使用源码
从官网下载源文件，解压到`content/themes/[specified_themes]/assets`。而相应的代码则变为
```html
<link rel="stylesheet" href="/assets/css/highlight/monokai_sublime.css" /> <!-- 这里指定的 css 文件是代码高亮的主题，可以根据你的喜好自由选择-->  
<script src="/assets/js/highlight.pack.js"></script>  
```

## 启动highlightjs
在上述script位置下面一行插入
```html
<script >hljs.initHighlightingOnLoad();</script>  
```