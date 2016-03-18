## 单例模式

```java
class SingletonClass {
    private static SingletonClass mInstance;
    private SingletonClass() {}
    public static SingletonClass instance() {
        if (mInstance == null) {
            mInstance = new SingletonClass();
        }
        return mInstance;
   }
}
```

## Java的静态设计

```java
class StaticClass {
    static String a = "a string";
    static String b = "b string";
}
```