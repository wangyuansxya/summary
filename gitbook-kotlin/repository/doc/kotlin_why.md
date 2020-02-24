**2017-08-08-1.为什么你应该完全切换到Kotlin？**

> 是时候开始使用现代的编程语言了!

　我想告诉你一门叫做**Kotlin**的新的编程语言，以及为什么你应该为你的下一个项目考虑它。我过去钟爱于Java。但是去年，我发现我无论什么时候我都在尽可能的用Kotlin进行编程。基于这一点，我真的想不出任何一个Java会成为更好的选择的情形。

　Kotlin是由**JetBrains**开发的。事实上，是IDE（诸如**IntelliJ**和**ReSharper**）套件背后的人们让Kotlin闪闪发光的。它非常的优雅，非常的简洁。它让编程成为一种惬意和高效的体验。

尽管Kotlin可以编译成**JavaScript**和机器码，但是我会关注于它的主要环境：**JVM**。

因此，这里有一大堆理由，为什么你应该完全切换到Kotlin（而不是出于某种的命令）：

## 0# Java互操作性

　Kotlin是100%可以和Java互操作的。你可以继续在你的Java遗留项目中使用Kotlin。所有你喜欢的**Java框架依然是可用的**。无论你用Kotlin编写什么样的框架，你的Java死忠朋友都会愉快的接受。

## 1# 熟悉的语法

　Kotlin不是某些为学术而生的怪异语言。它的语法对任何来自OOP（面向对象编程）领域的程序员来说都是熟悉的。对于入门者而言，或多或少是容易理解的。当然，它也有跟Java不一样的东西。比如，重新设计的构造函数和`val`、`var`变量声明。下面展示的代码大部分是基础的：

```kotlin
class Foo {

    val b: String = "b"     // val means unmodifiable
    var i: Int = 0          // var means modifiable

    fun hello() {
        val str = "Hello"
        print("$str World")
    }

    fun sum(x: Int, y: Int): Int {
        return x + y
    }

    fun maxOf(a: Float, b: Float) = if (a > b) a else b

}
```

## 2# 字符串插入

它好像是Java版的`String.format()`。但是它更加智能、更为可读。而且内置进了语言里面：

```kotlin
val x = 4
val y = 7
print("sum of $x and $y is ${x + y}")  // sum of 4 and 7 is 11
```

## 3# 类型推断

Kotlin会推断你对类型，无论在什么地方，你都会感觉到它会提升代码的可读性：

```kotlin
val a = "abc"                         // type inferred to String
val b = 4                             // type inferred to Int

val c: Double = 0.7                   // type declared explicitly
val d: List<String> = ArrayList()     // type declared explicitly
```

## 4# 智能类型转换

Kotlin编译器会跟踪你的代码逻辑。如果可能的话，它会自动转换类型。这意味着`instanceof`检查后面不需要显式的类型转换了：

```kotlin
if (obj is String) {
    print(obj.toUpperCase())     // obj is now known to be a String
}
```

## 5# 直观的等价性判断

你可以不再显式的调用`equals()`了。因为现在`==`操作符会检查结构的等价性了：

```kotlin
val john1 = Person("John")
val john2 = Person("John")
john1 == john2    // true  (structural equality)
john1 === john2   // false (referential equality)
```

## 6# 默认参数

你不需要定义多个参数不同的相似方法了（不需要函数重载了）：

```kotlin
fun build(title: String, width: Int = 800, height: Int = 600) {
    Frame(title, width, height)
}
```

## 7#  命名参数

跟默认参数一起（使用），命名参数消灭了对建造者的需求：

```kotlin
build("PacMan", 400, 300)                           // equivalent
build(title = "PacMan", width = 400, height = 300)  // equivalent
build(width = 400, height = 300, title = "PacMan")  // equivalent
```

## 8# When表达式

`switch...case...`被更加可读、更加灵活的`when`表达式替换了：

```kotlin
when (x) {
    1 -> print("x is 1")
    2 -> print("x is 2")
    3, 4 -> print("x is 3 or 4")
    in 5..10 -> print("x is 5, 6, 7, 8, 9, or 10")
    else -> print("x is out of range")
}
```

它可以用作表达式，也可以用作语句。它可以有参数，也可以不带参数：

```kotlin
val res: Boolean = when {
    obj == null -> false
    obj is String -> true
    else -> throw IllegalStateException()
}
```

## 9# 属性

你可以添加定制的`set`和`get`行为到公开的字段中。这意味着不再有毫无意义的`getter`和`setter`让我们的代码膨胀了：

```kotlin
class Frame {
    var width: Int = 800
    var height: Int = 600

    val pixels: Int
        get() = width * height
}
```

## 10# 数据类

数据类是一个包含了``toString()`, `equals()`, `hashCode()`,和 `copy()`的`POJO`。不同于Java的是，它不会超过100行代码：

```kotlin
data class Person(val name: String,
                  var email: String,
                  var age: Int)

val john = Person("John", "john@gmail.com", 112)
```

## 11# 操作符重载

预定义的操作符合集可以被重载以提升可读性：

```kotlin
data class Vec(val x: Float, val y: Float) {
    operator fun plus(v: Vec) = Vec(x + v.x, y + v.y)
}

val v = Vec(2f, 3f) + Vec(4f, 1f)
```

## 12# 析构函数声明

某些对象可以被销毁。比如，这对迭代映射(`map`)非常管用：

```kotlin
for ((key, value) in map) {
    print("Key: $key")
    print("Value: $value")
}
```

## 13# 范围

为了可读性：

```kotlin
for (i in 1..100) { ... } 
for (i in 0 until 100) { ... }
for (i in 2..10 step 2) { ... } 
for (i in 10 downTo 1) { ... } 
if (x in 1..10) { ... }
```

## 14# 扩展函数

　还记得你第一次使用Java对一个`List`对象进行排序的情形吗？你找不到`sort`函数。所以你必须问你的导师或者向Google学习`Collections.sort()`。之后，当你必须把一个`String`对象转换为大写时，你编写了自己的助手函数。因为你并不知道`StringUtils.capitalize()`这个函数。

如果有一种方法来添加新的函数到旧的类中，那么这个方法就是你的IDE能够在代码填充的时候帮你找到正确的函数。在Kotlin中，你完全可以这样做：

```kotlin
fun String.replaceSpaces(): String {
    return this.replace(' ', '_')
}

val formatted = str.replaceSpaces()
```

Kotlin标准库扩展了Java原始类型的功能。这是`String`特别需要的啊：

```kotlin
str.removeSuffix(".txt")
str.capitalize()
str.substringAfterLast("/")
str.replaceAfter(":", "classified")
```



## 15# 空指针安全

　我们经常把Java佳作是一个静态类型的语言。它里面的`String`类型的变量并不能保证指向一个`String`对象。它可能指向`null`。尽管我们以往经常这么做，但是它被静态类型检查否定了。结果，Java开发者被迫活在永久的[`NullPointerException`](https://stackoverflow.com/questions/218384/what-is-a-nullpointerexception-and-how-do-i-fix-it)恐惧中。

Kotlin通过区分**不为空类型**和**为空的类型**来解决这个问题。类型默认是不为空的。但是可以通过添加`?`来使其可为空。就像这样：

```kotlin
var a: String = "abc"
a = null                // compile error

var b: String? = "xyz"
b = null                // no problem
```

Kotlin强迫你无论在什么时候访问一个可为空的类型，都要提防**NPE**:

```kotlin
val x = b.length        // compile error: b might be null
```

然而，有时候这看起来可能有点多余了。但多少要感谢一下这个特性。我们依然会有智能类型转换。无论何时，尽可能把可为空的类型转换为不为空的类型的智能转换：

```kotlin
if (b == null) return
val x = b.length        // no problem
```

我们也可以使用安全调用`?.`。它会当做空指针而不是抛出**NPE**:

```kotlin
val x = b?.       // type of x is nullable Int
```

　安全调用可以链在一起。这样可以避免嵌套的`if not null`检查。我们有时候在其他的语言中会这么写。如果我们想要一个别的默认值，而不是`null`，我们可以使用三元操作符`?:`：

```kotlin
val name = ship?.captain?.name ?: "unknown"
```

如果以上没有一个你喜欢的，你绝对需要一个NPE！你将不得不显式的询问：

```kotlin
val x = b?.length ?: throw NullPointerException()  // same as below
val x = b!!.length                                 // same as above
```

## 16# 更好的Lambda

嘿，伙计，这是一个不错的lambda系统--完美的平衡了可读性和简洁性。感谢那些明智的语言设计选择吧！语法直接了当：

```kotlin
val sum = { x: Int, y: Int -> x + y }   // type: (Int, Int) -> Int
val res = sum(4,7)                      // res == 11
```

来看看明智在哪里：

1. 如果lambda放在最后或者方法只有一个参数，你可以移除方法调用的圆括号。
2. 如果我们选择不去什么单一参数的lambda的参数，它的默认名字为`it`。

上面的描述合起来就是下面的三行代码：

```kotlin
numbers.filter({ x -> x.isPrime() })
numbers.filter { x -> x.isPrime() }
numbers.filter { it.isPrime() }
```

这可以让我们编写更加精简的函数式代码--来看看它的动人之处吧：

```kotlin
persons
    .filter { it.age >= 18 }
    .sortedBy { it.name }
    .map { it.email }
    .forEach { print(it) }
```

Kotlin的lambda系统加上了扩展函数，使得它成为创建DSL的理想选择。检出增强Android开发的Anko代码作为DSL的案例：

```kotlin
verticalLayout {
    padding = dip(30)
    editText {
        hint = “Name”
        textSize = 24f
    }
    editText {
        hint = “Password”
        textSize = 24f
    }
    button(“Login”) {
        textSize = 26f
    }
}
```

## 17# IDE支持

如果你想上手Kotlin，你有多个选择。但是我强烈推荐使用配套了Kotlin的IntelliJ。它的特性解释了编程语言语言和IDE的设计者为同一批人的好处。

就给你一个小而美的例子吧！当我第一次从Stack Overflow复制Java代码时，它会弹出（这样的对话框）：

<img src="https://github.com/wangyuansxya/wangyuansxya.github.io/blob/master/assets/1-zpE0UpuBDGW7Mk-Vtx2uKw.png?raw=true" style="zoom:50%" />

**扩展阅读：**

- [**Kotlin on Android. Now official**](https://blog.jetbrains.com/kotlin/2017/05/kotlin-on-android-now-official/)

- [**Why Kotlin is my next programming language**](https://medium.com/@octskyward/why-kotlin-is-my-next-programming-language-c25c001e26e3)

- [**Scala vs Kotlin**](https://agilewombat.com/2016/02/01/scala-vs-kotlin/)

- [**Swift is like Kotlin**](http://nilhcem.com/swift-is-like-kotlin/)

- [**The Road to Gradle Script Kotlin 1.0**](https://blog.gradle.org/kotlin-scripting-update)

- [**Introducing Kotlin support in Spring Framework 5.0**](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0?utm_content=buffer546ec&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)

- [**10 cool things about Kotlin**](http://frozenfractal.com/blog/2017/2/10/10-cool-things-about-kotlin/)

- [**Kotlin full stack application example**](https://github.com/Kotlin/kotlin-fullstack-sample)

- [**Why I abandoned Java in favour of Kotlin**](https://hashnode.com/post/why-i-abandoned-java-in-favour-of-kotlin-ciuzhmecf09khdc530m1ghu6d)

- [**I used Kotlin at Hackathon**](https://medium.com/@johnnytn.mail/i-used-kotlin-at-a-hackathon-cd9eab1725f9)

- [**From Java to Kotlin**](https://fabiomsr.github.io/from-java-to-kotlin/)

- [**Kotlin Idioms**](https://kotlinlang.org/docs/reference/idioms.html)

  ​

翻译原文： [Why you should totally switch to Kotlin
](https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5)


