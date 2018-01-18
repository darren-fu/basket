# 为什么Java中的匿名内部类只可以访问final的局部变量

先来看一下Java中的匿名内部类是如何实现的
定义一个接口
```java
public interface MyInterface {
    void doSomething();
}
```
创建匿名内部类
```java
public class TryUsingAnonymousClass {
    public void useMyInterface() {
        final Integer number = 123;
        System.out.println(number);

        MyInterface myInterface = new MyInterface() {
            @Override
            public void doSomething() {
                System.out.println(number);
            }
        };
        myInterface.doSomething();

        System.out.println(number);
    }
}
```
这个匿名子类会被编译成一个单独的类，反编译的结果是这样的
```java
class TryUsingAnonymousClass$1
        implements MyInterface {
    private final TryUsingAnonymousClass this$0;
    private final Integer paramInteger;

    TryUsingAnonymousClass$1(TryUsingAnonymousClass this$0, Integer paramInteger) {
        this.this$0 = this$0;
        this.paramInteger = paramInteger;
    }

    public void doSomething() {
        System.out.println(this.paramInteger);
    }
}
```
可以看到名为number的局部变量是作为构造方法的参数传入匿名内部类的（以上代码经过了手动修改，真实的反编译结果中有一些不可读的命名）。

如果Java允许匿名内部类访问非final的局部变量的话，那我们就可以在TryUsingAnonymousClass$1中修改paramInteger，但是这不会对number的值有影响，因为它们是不同的reference。

这就会造成数据不同步的问题。

所以，谜团解开了：**Java为了避免数据不同步的问题，做出了匿名内部类只可以访问final的局部变量的限制。**

> 此限制针对的是方法内的局部变量，类变量则没有此限制，因为匿名内部类会持有外部类对象this引用，从而直接访问类变量，不会出现不一致的问题。

---
> C#， Scala等其它语言没有此限制，编译器会将局部变量包装在一个对象内，lambda内外访问的是同一个对象，且对局部变量的变更也是通过对象的方法进行，从而保证了一致性。

