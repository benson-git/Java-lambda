# Java-lambda

---
lambda表达式可以这样定义：一段带有输入参数的可执行语句块。

lambda表达式的一般语法:
```java
(Type1 param1, Type2 param2, ..., TypeN paramN) -> {
  statment1;
  statment2;
  //.............
  return statmentM;
}
```
绝大多数情况，编译器都可以从上下文环境中推断出lambda表达式的参数类型, 参数类型省略。 这样lambda表达式就变成了：
```
(param1,param2, ..., paramN) -> {
  statment1;
  statment2;
  //.............
  return statmentM;
}
```
当lambda表达式的参数个数只有一个，可以省略小括号。lambda表达式简写为:
```
param1 -> {
  statment1;
  statment2;
  //.............
  return statmentM;
}
```
当lambda表达式只包含一条语句时，可以省略大括号、return和语句结尾的分号。lambda表达式简化为:
```
param1 -> statment
```

expression lambda和statement lambda的区别是，expression lambda不需要
写return关键字，Java runtime会将表达式的结果作为返回值返回，而statement lambda是
写在{}中的表达式，需要使用return关键字，比如：
```
// expression lambda
Comparator<String> comp1 = 
  (first, second) -> Integer.compare(first.length(), second.length());
 
// statement lambda
Comparator<String> comp2 = (first, second) -> 
  { return Integer.compare(first.length(), second.length());};
```

使用Method Reference:
```
List<String> lowercaseNames = names.stream().map(String::toLowerCase).collect(Collectors.toList());
```
---
如果将lambda表达式的参数作为参数传递给一个方法，他们的执行效果是相同的，则该lambda表达式
可以使用Method Reference表达，以下两种方式是等价的：
```java
(x) -> System.out.println(x)
//等价于：
System.out::println
```
其中System.out::println被称为Method Reference。
Method Reference主要有三种形式：
object::instanceMethod
Class::staticMethod
Class::instanceMethod
对于前两种方式，对应的lambda表达式的参数和method的参数是一致的，比如：
```
System.out::println
(x) -> System.out.println(x)
 
Math::pow 
(x, y) -> Math.pow(x, y)
```
对于第三种方式，对应的lambda表达式的语句体中，第一个参数作为对象，调用method，将其它参数
作为method的参数，比如：
```
String::compareToIgnoreCase
(s1, s2) -> s1.compareToIgnoreCase(s2)
```
使用Constructor Reference:
Constructor Reference与Method Reference类似，只不过是特殊的method：new，具体调用的是哪个构造函数，由上下文环境决定，比如：
```
List<String> labels = ...;
Stream<Button> stream = labels.stream().map(Button::new);
```
Button::new等价于(x) -> Button(x)，所以调用的构造函数是：Button(x);
除了创建单个对象，也可以创建对象数组，如下面两种方式等价：
```
int[]::new
(x) -> new int[x]
```


函数式接口（Functional Interface）:
所谓的函数式接口，当然首先是一个接口，然后就是在这个接口里面只能有一个抽象方法。这种类型的接口也称为SAM接口，即Single Abstract Method interfaces.

Java 8为函数式接口引入了一个新注解@FunctionalInterface，主要用于编译级错误检查，加上该注解，当你写的接口不符合函数式接口定义的时候，编译器会报错。加不加@FunctionalInterface对于接口是不是函数式接口没有影响，该注解只是提醒编译器去检查该接口是否仅包含一个抽象方法。 

```java
@FunctionalInterface
interface Print<T> {
    public void print(T x);
}
public class Lambda {   
    public static void PrintString(String s, Print<String> print) {
        print.print(s);
    }
    public static void main(String[] args) {
        PrintString("test", (x) -> System.out.println(x));
    }
}
```

 编译后等价于：
 
```java
@FunctionalInterface
interface Print<T> {
    public void print(T x);
}
public class Lambda {   
    public static void PrintString(String s, Print<String> print) {
        print.print(s);
    }
    private static void lambda$0(String x) {
        System.out.println(x);
    }
    final class $Lambda$1 implements Print{
        @Override
        public void print(Object x) {
            lambda$0((String)x);
        }
    }
    public static void main(String[] args) {
        PrintString("test", new Lambda().new $Lambda$1());
    }
}
```
java.util.function 它包含了很多类，用来支持 Java的 函数式编程，该包中的函数式接口有：

+ BiConsumer<T,U>
代表了一个接受两个输入参数的操作，并且不返回任何结果
+ BiFunction<T,U,R>
代表了一个接受两个输入参数的方法，并且返回一个结果
+ BinaryOperator<T>
代表了一个作用于于两个同类型操作符的操作，并且返回了操作符同类型的结果
+ BiPredicate<T,U>
代表了一个两个参数的boolean值方法
+ BooleanSupplier
代表了boolean值结果的提供方
+ Consumer<T>
代表了接受一个输入参数并且无返回的操作
+ DoubleBinaryOperator
代表了作用于两个double值操作符的操作，并且返回了一个double值的结果。
+ DoubleConsumer
代表一个接受double值参数的操作，并且不返回结果。
+ DoubleFunction<R>
代表接受一个double值参数的方法，并且返回结果
+ DoublePredicate
代表一个拥有double值参数的boolean值方法
+ DoubleSupplier
代表一个double值结构的提供方
+ DoubleToIntFunction
接受一个double类型输入，返回一个int类型结果。
+ DoubleToLongFunction
接受一个double类型输入，返回一个long类型结果
+ DoubleUnaryOperator
接受一个参数同为类型double,返回值类型也为double 。
+ Function<T,R>
接受一个输入参数，返回一个结果。
+ IntBinaryOperator
接受两个参数同为类型int,返回值类型也为int 。
+ IntConsumer
接受一个int类型的输入参数，无返回值 。
+ IntFunction<R>
接受一个int类型输入参数，返回一个结果 。
+ IntPredicate
：接受一个int输入参数，返回一个布尔值的结果。
+ IntSupplier
无参数，返回一个int类型结果。
+ IntToDoubleFunction
接受一个int类型输入，返回一个double类型结果 。
+ IntToLongFunction
接受一个int类型输入，返回一个long类型结果。
+ IntUnaryOperator
接受一个参数同为类型int,返回值类型也为int 。
+ LongBinaryOperator
接受两个参数同为类型long,返回值类型也为long。
+ LongConsumer
接受一个long类型的输入参数，无返回值。
+ LongFunction<R>
接受一个long类型输入参数，返回一个结果。
+ LongPredicate
R接受一个long输入参数，返回一个布尔值类型结果。
+ LongSupplier
无参数，返回一个结果long类型的值。
+ LongToDoubleFunction
接受一个long类型输入，返回一个double类型结果。
+ LongToIntFunction
接受一个long类型输入，返回一个int类型结果。
+ LongUnaryOperator
接受一个参数同为类型long,返回值类型也为long。
+ ObjDoubleConsumer<T>
接受一个object类型和一个double类型的输入参数，无返回值。
+ ObjIntConsumer<T>
接受一个object类型和一个int类型的输入参数，无返回值。
+ ObjLongConsumer<T>
接受一个object类型和一个long类型的输入参数，无返回值。
+ Predicate<T>
接受一个输入参数，返回一个布尔值结果。
+ Supplier<T>
无参数，返回一个结果。
+ ToDoubleBiFunction<T,U>
接受两个输入参数，返回一个double类型结果
+ ToDoubleFunction<T>
接受一个输入参数，返回一个double类型结果
+ ToIntBiFunction<T,U>
接受两个输入参数，返回一个int类型结果。
+ ToIntFunction<T>
接受一个输入参数，返回一个int类型结果。
+ ToLongBiFunction<T,U>
接受两个输入参数，返回一个long类型结果。
+ ToLongFunction<T>
接受一个输入参数，返回一个long类型结果。
+ UnaryOperator<T>
接受一个参数为类型T,返回值类型也为T。



