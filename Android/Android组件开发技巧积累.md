## View设置透明度

1.在xml文件中设置背景颜色
```xml
// 前两位表示透明度，0~255，后面依次为RGB
android:background="#ff6495ED"
```
2.在java中获取该控件的Drawable，设置透明度
```java
convertView.getBackground().setAlpha(80);
```

## Action Bar版本

v7版本，要用 getSupportActionBar(), ActionBar也要用类v7
v7版本，作为home as up 的id， 前面必须加上android.R.id.home 的 android

## Layout的可见问题
 - gone的意思是“隐藏”，在布局中消失；
 - invisible的意思是“不可见”，在布局中不显示，但是保留空间。

## Menu如何动态设置函数

onPrepareOptionsMenu，与onCreateOptionsMenu不同的是，每次点击Menu都会调用。在这个函数中，动态更改。

## 两个很有用的开源库（Http和image）
android-async-http
picasso
下载jar包，放在应用程序libs文件夹中