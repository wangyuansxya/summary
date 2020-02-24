**2017-07-20-anko是什么**

Anko是JetBrains开发的一个强大的库。它主要的目的是用来替代以前XML的方式来使用代码生成UI布局

Anko包含了很多的非常有帮助的函数和属性来避免让你写很多的模版代码

尽管Anko是非常有帮助的，但是我建议你要理解这个背后到底做了什么。你可以在
任何时候使用 ctrl + 点击 （Windows） 或者 cmd + 点击 （Mac） 的方式跳转
到Anko的源代码。Anko的实现方式对学习大部分的Kotlin语言都是非常有帮助的。

1，开始使用anko

让我们来使用Anko来简化一些代码。就像你将看到的，任何时候你使用了Anko库中的某些东西，它们都会以属性名、方法等方式被导入。这是因为Anko使用了扩展函数在Android框架中增加了一些新的功能

一个Anko扩展函数可以被用来简化获取一个RecyclerView：

	val mRecycleView : RecyclerView = find<RecyclerView>(R.id.mRecycleView);
	
但是Anko能帮助我们简化代码，比如，实例化Intent，Activity之间的跳转，Fragment的创建，数据库的访问，Alert的创建等等

2，扩展函数

扩展函数数是指在一个类上增加一种新的行为，甚至我们没有这个类代码的访问权限

例如：我们可以创建一个toast函数，这个函数不需要传入任何context，它可以被任何Context或者它的子类调用，比如Activity或者Service：

	fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
		Toast.makeText(this, message, duration).show()
	}

Kotlin中扩展函数的一个优势是我们不需要在调用方法的时候把整个对象当作参数传入

这个方法可以在Activity内部直接调用：

	toast("Hello world!")
	toast("Hello world!", Toast.LENGTH_LONG)

扩展函数也可以是一个属性。所以我们可以通过相似的方法来扩展属性

Kotlin由于互操作性的特性已经提供了这个属性
	
	var RecyclerView.Adapter: MainAdapter
        get() = MainAdapter();
        set(MainAdapter) = setAdapter(MainAdapter);

扩展函数并不是真正地修改了原来的类，它是以静态导入的方式来实现的。扩展函数可以被声明在任何文件中，因此有个通用的实践是把一系列有关的函数放在一个新建的文件里。这是Anko功能背后的魔法

3,执行一个请求

对于感受我们要实现的想法而言，我们目前的文本是很好开始，但是现在是时候去请求一些显示在RecyclerView上的真正的数据了

多亏Kotlin非常强大的互操作性，你可以使用任何你想使用的库，比如用Retrofit来执行服务器请求

当只是执行一个简单的API请求，我们可以不使用任何第三方库来简单地实现

	class Request(val url : String) {
	    public fun run() {
	        val result = URL(url).readText();
	        Log.d("result", "result : $result");
	    }
	}

我们使用 readText ，这是Kotlin标准库中的扩展函数。这个方法不推荐结果很大的响应

4,在主线程以外执行请求

如你所知，HTTP请求不被允许在主线程中执行，否则它会抛出异常。这是因为阻塞住UI线程是一个非常差的体验。Android中通用的做法是使用 AsyncTask，AsyncTasks 会非常危险，因为当运行到 postExecute 时，如
果Activity已经被销毁了，这里就会崩溃。

Anko提供了非常简单的DSL来处理异步任务，它满足大部分的需求。它提供了一个基本的 async 函数用于在其它线程执行代码，也可以选择通过调用 uiThread 的方式回到主线程

	//执行一个异步的请求
    async {
        Request("www.baidu.com").run();
        uiThread {  toast("从async回到了mainThread") }
    }

UIThread 有一个很不错的一点就是可以依赖于调用者。如果它是被一个 Activity 调用的，那么如果 activity.isFinishing() 返回 true ，则 uiThread 不会执行，这样就不会在Activity销毁的时候遇到崩溃的情况了。

假如你想使用 Future 来工作， async 返回一个Java Future 。而且如果你需要一个返回结果的 Future ，你可以使用 asyncResult 。