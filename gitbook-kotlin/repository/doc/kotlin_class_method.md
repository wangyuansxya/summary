**2017-07-19-kotlin 类和函数**

1，怎么定义一个类
如果你想定义一个类，你只需要使用 class 关键字
	
	class MainActivity{}

它有一个默认唯一的构造器。如果你要添加构造方法的参数，只需要在类名后面写上它的参数

	class MainActivity(name: String, surname: String)

那么构造函数的函数体在哪呢？你可以写在 init 块中：

	class MainActivity(name: String, surname: String) {
		init{
		...
		}
	}

2，类继承

默认任何类都是基础继承自 Any （与java中的 Object 类似）。所有的类默认都是不可继承的（final） ，所以我们只能继承那些明确声明 open 或者 abstract 的类

	open class Animal(name: String)
	class Person(name: String, surname: String) : Animal(name)

当我们只有单个构造器时，我们需要在从父类继承下来的构造器中指定需要的参数。这是用来替换Java中的 super 调用的。

3，函数
函数（我们Java中的方法） 可以使用 fun 关键字就可以定义:

	fun onCreate(savedInstanceState: Bundle?) {
	}

如果你没有指定它的返回值，它就会返回 Unit ，与Java中的 void 类似，但
是 Unit 是一个真正的对象。你当然也可以指定任何其它的返回类型：

	fun add(x: Int, y: Int) : Int {
		return x + y
	}

> 提示：分号不是必须的
> >就想你在上面的例子中看到的那样，我在每句的最后没有使用分号。当然
> >你也可以使用分号，分号不是必须的，而且不使用分号是一个不错的实践。当你这么做了，你会发现这节约了你很多时间。
> 

然而如果返回的结果可以使用一个表达式计算出来，你可以不使用括号而是使用等号：

	fun add(x: Int,y: Int) : Int = x + y

我们可以给参数指定一个默认值使得它们变得可选，这是非常有帮助的

	fun toast(message: String, length: Int = Toast.LENGTH_SHORT) {
		Toast.makeText(this, message, length).show()
	}

如你所见，第二个参数（length） 指定了一个默认值。这意味着你调用的时候可以
传入第二个值或者不传，这样可以避免你需要的重载函数

	toast("Hello")
	toast("Hello", Toast.LENGTH_LONG)

下面增加第三个默认值是类名的tag参数。如果在Java中总数开销会以几何增长

	fun niceToast(message: String,tag: String = javaClass<MainActivity>().getSimpleName(),
		length: Int = Toast.LENGTH_SHORT) {
			Toast.makeText(this, "[$className] $message", length).show()
	}

现在可以通过以下方式调用：

	toast("Hello")
	toast("Hello", "MyTag")
	toast("Hello", "MyTag", Toast.LENGTH_SHORT)

而且甚至还有其它选择，因为你可以使用参数名字来调用，这表示你可以通过在值
前写明参数名来传入你希望的参数：

	toast(message = "Hello", length = Toast.LENGTH_SHORT)

>小提示：String模版
>>你可以在String中直接使用模版表达式。它可以帮助你很简单地在静态值和
>>变量的基础上编写复杂的String。在上面的例子中，我使用了"
>>[$className] $message"。
>>如你所见，任何时候你使用一个 $ 符号就可以插入一个表达式。如果这个
>>表达式有一点复杂，你就需要使用一对大括号括起来："Your name is
>>${user.name}"。

