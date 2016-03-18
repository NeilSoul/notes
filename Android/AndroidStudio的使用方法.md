Overview of Android Studio

## project & module

### module - Open Module Setting 

can configure mudule dependency

### delete module

Open Module Setting - '-' operator - delete module

## module

### build.gradle

set sdk api

### AndroidMainiFest.xml

in `main/`, register all activities, set laucher activity
```xml
<activity
    android:name=".MyActivity"
    android:label="@string/app_name" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

## 引用第三方
一、引用jar文件
    1.将jar文件复制、粘贴到app的libs目录中；
    2.右键点击jar文件，并点击弹出菜单中的“Add As Library”，将jar文件作为类库添加到项目中；新版本中则是选中app，在上栏的路径框选择libs，sync，
    3.选择指定的类库。
    注：如果不执行2、3步，jar文件将不起作用，并且不能使用import语句引用。
二、引用so文件
    网上有很多引用so文件的方法，多数都很麻烦，在KYLE THIELK的博客中找到了一种简单的方法。
    1.在“src/main”目录中新建名为“jniLibs”的目录；
    2.将so文件复制、粘贴到“jniLibs”目录内。
    注：如果没有引用so文件，可能会在程序执行的时候加载类库失败，有类似如下的DEBUG提示：
    java.lang.UnsatisfiedLinkError: Couldn't load library xxxx from loader dalvik.system.PathClassLoader


## Gradle

problem  :  'Gradle DSL method not found: 'runProguard()'
gradle 版本不对

old:
```
buildTypes {
    release {

        runProguard false // 已经被废弃并且停止使用了

        ......
    }
}
```
new:
```
buildTypes {
    release {

        minifyEnabled false // 替代的方式

        ......
    }
}
```




