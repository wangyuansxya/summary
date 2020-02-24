**2017-07-20-使用Kotlin编写的第一个类**

1，变量和属性
在Kotlin中，一切都是对象。没有像Java中那样的原始基本类型。这个是非常有帮
助的，因为我们可以使用一致的方式来处理所有的可用的类型
	
2，基本类型

当然，像integer，float或者boolean等类型仍然存在，但是它们全部都会作为对象
存在的。基本类型的名字和它们工作方式都是与Java非常相似的，但是有一些不同
之处你可能需要考虑到：

- 数字类型中不会自动转型。举个例子，你不能给 Double 变量分配一
个 Int 。必须要做一个明确的类型转换，可以使用众多的函数之一：
	
	    val i:Int=7 <br>
	    val d: Double = i.toDouble()
    
- 字符（Char） 不能直接作为一个数字来处理。在需要时我们需要把他们转换为一个数字：

    	val c : char = "c";<br>
    	val i : Int = c.toInt();

- 位运算也有一点不同。在Android中，我们经常在 flags 中使用“或”，所以我使用"and"和"or来举例：
    
    	// Java <br>
    	int bitwiseOr = FLAG1 | FLAG2; <br>
    	int bitwiseAnd = FLAG1 & FLAG2; <br>
    	// Kotlin <br>
    	val bitwiseOr = FLAG1 or FLAG2 <br>
    	val bitwiseAnd = FLAG1 and FLAG2 <br>

>还有很多其他的位操作符，比如 sh1 , shs , ushr , xor 或 inv 。当我们需要的时候，可以在[Kotlin官网](http://kotlinlang.org/docs/reference/basic-types.html#operations)查看。

- 字面上可以写明具体的类型。这个不是必须的，但是一个通用的Kotlin实践时省略变量的类型（我们马上就能看到） ，所以我们可以让编译器自己去推断出具体的类型。
    
    	val i = 12 // An Int
    	val iHex = 0x0f // 一个十六进制的Int类型
    	val l = 3L // A Long
    	val d = 3.5 // A Double
    	val f = 3.5F // A Float
	 

- 一个String可以像数组那样访问，并且被迭代：

	    val name = bean?.name ?: "empty"
	    val c = name[1];// 这是一个字符'm'
    	//迭代String
	    for(c in name) {
	    print(c);
	    }

3,变量

变量可以很简单地定义成可变( var )和不可变（ val ） 的变量。这个与Java中使
用的 final 很相似。但是不可变在Kotlin（和其它很多现代语言） 中是一个很重要
的概念。

一个不可变对象意味着它在实例化之后就不能再去改变它的状态了

不可变对象也可以说是线程安全的，因为它们无法去改变，也不需要去定义访问控
制，因为所有线程访问到的对象都是同一个

一个重要的概念是：尽可能地使用 val 

如果我们需要使用更多的范型类型，则需要指定类型：

	val a: Any = 23
	val c: Context = activity

4,属性

属性与Java中的字段是相同的，但是更加强大。在kotlin中我们不需要自己去实现getter，setter和tostring方法。kotlin已经帮我们完成。在Kotlin中，只需要一个属性就可以了：
	
	public class Person {
		var name: String = ""
	}
	...
	val person = Person()
	person.name = "name"
	val name = person.name

如果没有任何指定，属性会默认使用getter和setter。当然它也可以修改为你自定义的代码，并且不修改存在的代码：

	public classs Person {
		var name: String = ""
			get() = field.toUpperCase()
			set(value){
				field = "Name: $value"
			}
	}

