 当变量作为类的成员使用时，java才确保给定默认值

方法参数引用传值
```java
class Letter{
    char c;
}
public class PassObject{
    static void f(Letter y){
        y.c='z';
    }
    public static void main(String[] args){
        Letter x=new Letter();
        x.c='a';
        print("1:x.c: "+x.c);
        f(x);
        print("2:x.c: "+x.c);
    }
}
out:
1:x.c:a
2:x.c:z
 ```
==和！=比较的为对象的引用（比较俩个引用的地址是否相等，也就是是否指向一个对象，）equals（）的默认行为是引用，需要覆盖

continue label :同时中断内部外部循环，继续执行迭代，从外部迭代开始

基本类型重载，如果没有，提升至较大类型，char提升至int

禁止在其他方法调用构造器

static不能调用非静态方法，但可传递一个对象的引用到静态方法，然后通过引用来调用非静态方法成员，可以new 构造方法。不能作用于局部变量，只用作用于域

应该总是只在重载方法的一个版本上使用可变参数列表

   除内部类外，对类的访问权限只有两个：包访问权限或public，如果不希望其他任何人对该类拥有访问权限，可以吧构造器指定为private

包访问权限的类的摸个static成员是public的话，客户端程序员仍可调用改static成员，尽管不能生产该类的对象

final方法不会被覆盖

final类不可被继承

所有的private方法都隐式的指定为final

java中除了static和final方法外，其他所有方法都是后期绑定。

只有非private方法才可以被覆盖

访问域，在编译器进行解析。静态方法不具备多态性

接口也可以包含域，但是这些域隐式的是static和final的.接口中被定义的方法必须为public

普通内部类的方法和字段，只能放在类的外部层次上，普通内部类不能有static字段和数据，也不能包含嵌套类。嵌套类可以

内部类的继承：构造函数中使用
```java
enClosingClassReference.super()
```