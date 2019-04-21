出现在:字节跳动,腾讯,百度,阿里

单例模式三个要点:
1,私有的静态变量
2,私有的构造函数
3,公共可访问的静态函数

两种比较好的实现方法
1,双重验证
```
public class Singleton {
  private static Singleton instance;
  private Singleton() {
		
	}
  public static Singleton getInstance(){
    if(instance == null){
      synchronized(Singleton.class){
        if(instance == null){
          instance = new Singleton();
        }
      }
    }
    return instance;
  }
}
```

2,静态内部类实现
```
//既具有延迟加载的特性,jvm又能保证线程安全
public class Singleton {
  private static class InstanceHolder{
      public static Singleton instance = new Singleton();
    }

    //私有的构造函数
    private Singleton() {

    }
    //公共的可访问的静态函数
    public static Singleton getInstance() {
      return InstanceHolder.instance;
    }
}
```
