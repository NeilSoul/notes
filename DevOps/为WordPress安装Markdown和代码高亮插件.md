wordpress插件安装方法有几种。可以在后台中，选择在线安装；或者到插件官网下载zip文件，然后通过后台来上传；或者在服务器中通过wget命令下载zip文件，然后解压到wp-content/plugins/。
## 安装markdown
**Jetpack**是一个很强大的插件工具，其中markdown只是一个小功能。
## 安装代码高亮插件
**Crayon Syntax Highlighter**,与**Jetpack**完全兼容。
样例
```cpp
#include <iostream>
using namespace std;
int main() {
    cout << "hello world" << endl;
    return 0;
}
```
## 更改设置让二者完美兼容
按默认配置，会出现代码转义问题，诸如将代码块中的>号，变成转义的符号。原因大概是：在后台编辑框中提交的文本被保存到数据库中，在前台展示时才会经过Markdown转码。但是做的是先由Markdown根据语法转码后交由Crayon Syntax Highlighter进行代码高亮的渲染。而Markdown会将代码中的特殊符号经由HTML进行转义，而Highlighter会原封不动得显示`<pre>`标签中的代码，于是转义过后的代码就被原封不动地展示出来了。所以Highlighter必须在渲染时将转义过后的代码再转义回来。后台选项如下：
![图片出错](http://adai.today/wp-content/uploads/2015/11/1.jpg)

## 更简单的高亮插件

**WP Code Highlight.js**经测试，完美兼容。个人喜欢Rainbow风格。