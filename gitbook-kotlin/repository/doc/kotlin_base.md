**2017-07-18-kotlin基础**

1，实现一个基本的数据类

	/**
	 * kotlin定义的数据类，它会自动生成所有属性和它们的访问器，以及一些有用的方法，比如， toString()
	 */
	data class KotlinBean(
		var id : Int,
		var name : String,
		var desc : String
	)

2，空安全: Kotlin，如很多现代的语言，是空安全的，因为我们需要通过一个 安全调用操
作符 （写做 ? ） 来明确地指定一个对象是否能为空。

	//这里不能通过编译，list不能为空	
	private var list : List<KotlinBean> = null; 
	
	//这里通过编译，bean可以为空
	private var bean : KotlinBean? = null; 
	
	//无法编译, list可能是null，我们需要进行处理
	list.toString()

	// 只要在list != null时才会打印
	list?.print()
	
	// 智能转换. 如果我们在之前进行了空检查，则不需要使用安全调用操作符调用
	if (list != null) {
		list.print()
	}
	
	
	// 只有在确保artist不是null的情况下才能这么调用，否则它会抛出异常
	list!!.toString()
	// 使用Elvis操作符来给定一个在是null的情况下的替代值
	val name = bean?.name ?: "empty"

3,扩展方法
我们可以给任何类添加函数。它比那些我们项目中典型的工具类更加具有可读性。
举个例子，我们可以给fragment增加一个显示toast的函数：

	fun Fragment.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
		Toast.makeText(getActivity(), message, duration).show()
	}

我们现在可以这么做：

	fragment.toast("Hello world!")

4,函数式支持（Lambdas）
每次我们去声明一个点击所触发的事件，可以只需要定义我们需要做些什么，而不
是不得不去实现一个内部类？我们确实可以这么做，这个（或者其它更多我们感兴
趣的事件） 我们需要感谢lambda：

	view.setOnClickListener { toast("Hello world!") }

这里只是挑选了很小一部分Kotlin可以简化我们代码的事情。现在你已经知道这门
语言的一些有趣的特性了，你可以考虑它是否是适合你的
