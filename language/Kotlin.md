

# Kotlin

### 1. 基础

```kotlin
    //list 定义 遍历
val list = listOf<String>("hello","world","hello world")
    println("1. ------")
    for (e in list) {
        println(e)
    }
    println("2. ------")
    list.forEach(Consumer { println(it) })
    println("3. ------")
    list.forEach { println(it) }
    println("4. ------")
    list.forEach(System.out::println)
```

#### 1.1 kt文件

```kotlin
//HelloKotlin.kt
fun main(){}
//kotlinc 编译后的生成 HelloKotlinKT.class
//javap 反编译
public final class HelloKotlinKT{
    public static void main(String[] args){}
}
//kotlinc HelloKotlin.kt -include-runtime -d HelloKotlin.jar kotlin打包成jar文件
//java -jar HelloKotlin.jar 在执行jar
```

#### 1.2 方法定义

```kotlin
//1. 声明式
fun sum(a: Int, b: Int): Int {
    return a + b
}
//2. 表达式
fun sum1(a: Int, b: Int) = a + b
//3. 字符串模板
fun print(a: Int,b: Int){
    pritlin("$a + $b = ${a + b}")
}
```

#### 1.3 变量定义

```kotlin
//常量 只读类型
val a:Int = 1
//变量
var b:Int = 1
b=2
//类型转换
var x= 10
var y:Byte = 20
//x = y （error）不能小类型转大类型
```

#### 1.4 包

> 类似于golang 逻辑包名，和目录没关系

```kotlin
//包别名
import com.anyu.study.sum as sum0
```

#### 1.5 流程控制

```kotlin
//类似三元表达式
val x = 10
val y = 20
var max = if (x > y) x else y
```

> 判断时自动类型转换

```kotlin
//Int? 类似于java Optional
fun convert2Int(str: String): Int? {
    return try {
        str.toInt()
    } catch (ex: NumberFormatException) {
        null
    }
}
fun mutiply(a: String, b: String) {
    //a2Int、b2Int 是Int? 类型
    val a2Int = convert2Int(a)
    val b2Int = convert2Int(b)
    //在判断时，自动进行类型转换
    if (a2Int == null) {
        println("a2Int is null")
        return
    }
    //在判断时，自动进行类型转换
    if (b2Int == null) {
        println("b2Int is null")
        return
    }
    println(a2Int + b2Int)
}
```

```kotlin
//is 类似于 java instanceof
//any 是kotlin 体系的根类型
fun convert2Uppercase(str: Any): String? {
    if (str is String)
        return str.toUpperCase()
    return null
}
```

> 集合遍历

```kotlin
val ints = intArrayOf(1, 2, 3, 4, 5)
for (int in ints) 
for ((i, v) in ints.withIndex()) 
for(i int ints.indices)
```

#### 1.6 when

```kotlin
fun autoPrint(str: String): String {
    return when (str) {
        "hello" -> "HELLO"
        "world" -> "WORLD"
        else -> "OTHER INPUT"
    }
}


fun autoPrint1(str: String)：String = when (str) {
    "hello" -> "HELLO"
    "world" -> "WORLD"
    else -> "OTHER INPUT"
}
var a = 10
//直接赋值
var result = when (a) {
    1 -> {
        println(a)
    }
    2->
    println(a)
    3,4,5 -> println("3.4.5")
    //.. 两边闭合
    in 6..10 -> println("6~10")
    else -> println("other")
```

#### 1.7 范围表示

> `..` 是一个运算符重写，本意rangeTo ，表示闭区间

```kotlin
//step 中缀表达式
val a = 5
val b = 10
//a 是否在2 至 6 
if (a in 2..b) println("in the range")
//a 不在2 至 6
if (a !in 2..b) println("out of range")
//2 至 10遍历
for (i in 2..10) println(i)
//2 至 10遍历
for (i in 2.rangeTo(10)) println(i)
////2 至 10遍历，每次条两个数
for (i in 2..10 step 2) println(i)
//倒序遍历10 到2 ，每次条4步
for (i in 10 downTo 2 step 4) println(i)
```

```kotlin
fun handle(array: List<String>) {
    array.filter { it.length > 2 }
        .map { it.toUpperCase() }
        .sorted()
        .forEach{ println(it)}
}
```

#### 1.7 类

> 一个类可以有一个primary构造方法以及一个或多个secondary构造方法

```kotlin
//主构造方法，它位于类名的后面，可以拥有若干个参数
//如果primary构造方法没有任何注解或可见性关键字修饰，可以省略constructor
class MyClass constructor(username: String) {
    //属性赋初值
    private val name :String = username.toUpperCase()
    //初始化代码块,构造方法执行
    init{
        pritlin(username)
    }
}
```

> 构造方法参数或属性区别

```kotlin
//参数是 参数名+参数类型
//参数需要被初始化
class Person constructor(username: String) 
//属性是访问修饰符 属性声明符 变量名 变量类型
class Man(private val name: String, private val age: Int)
```

> lateinit 延迟初始化，类的属性声明可以不服初值，使用前必须赋初值

可见性修饰符

- private （当前文件使用）
- protected（不能用于顶层函数或者类上）
- internel（同一个模块）
- public(默认)

```kotlin
class ExtensionTest {
    fun add(a: Int, b: Int) :Int{
        return a + b
    }
}
//扩展，解析是静态的  ，不支持多态，调用室友对象声明决定的，不是有实际类型决定
fun ExtensionTest.subtract(a: Int, b: Int): Int {
    return a - b
}
fun main() {
    val e = ExtensionTest()
    println(  e.add(1, 3))
    println(  e.subtract(1, 3))
}
```

### 2. 泛型

#### 2.1 关于协变和逆变概念及来源

>协变（covariance）用于读取 ,`Collection<? extends E> `, 如果A <= B ,那么f(A)<= f(B)
>
>java中数组是协变的，java中泛型是不变的。
>
>逆变（）用于写入,`Collectiong<? super E>`

- **生产者**：只从中读取数据，而不往里面写入数据，那么这样的对象叫做生产者

  生产者使用`Collection<? extends E> `,读取的数据类型小于等于声明类型，才安全。

- **消费者**：只从中写入数据，而不往里面读取数据，那么这样的对象叫做消费者

  消费者使用`Collectiong<? super E>`，写入的数据类型大于等于声明类型，才安全。

==写入的数据类型小于声明类型，但是读取==

```java
List<Object> list;
List<Sting> list1 = new ArrayList<>();
list = list1;//编译出错，List<Object>与List<Sting> 不是相同类型

//可以放入Oject 类型及其子类
//放入时声明其泛型最大
List<? extends Object>  list= null;

//
interface Collection<E>{
    //Collection<String> 是Collection<? extends E> 子类型
    void addAll(Collection<? extends E> items)
}
//Collectiong<? super E>
```



