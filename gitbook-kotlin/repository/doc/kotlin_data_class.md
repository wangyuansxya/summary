**2017-07-21-kotlin 数据类**

数据类是一种非常强大的类，它可以让你避免创建Java中的用于保存状态但又操作非常简单的POJO的模版代码。它们通常只提供了用于访问它们属性的简单的getter和setter。定义一个新的数据类非常简单：

	data class KotlinBean (
	    var id : Int,
	    var name : String,
	    var desc : String
	)

1，额外的函数

通过数据类，我们可以方便地得到很多有趣的函数，一部分是来自属性，我们之前已经讲过（从编写getter和setter函数） ：

- equals(): 它可以比较两个对象的属性来确保他们是相同的。
- hashCode(): 我们可以得到一个hash值，也是从属性中计算出来的。
- copy(): 你可以拷贝一个对象，可以根据你的需要去修改里面的属性。我们会在稍后的例子中看到。
- 一系列可以映射对象到变量中的函数。我也很快就会讲到这个。


2，复制一个数据类

如果我们使用不可修改的对象，就像我们之前讲过的，假如我们需要修改这个对象状态，必须要创建一个新的一个或者多个属性被修改的实例。这个任务是非常重复且不简洁的。

	var adapter : KotlinBean = KotlinBean(StaticValue.VIEW, "View", "测试kotlin View");
    val a1 = adapter.copy(name = "person");

我们拷贝了第一个adapter对象然后只修改了 name 的属性而没有修改这个对象的其它状态

3,映射对象到变量上：映射对象的每一个属性到一个变量中，这个过程就是我们知道的多声明

	val bean : KotlinBean = KotlinBean(1, "name", "age");
    val (id, name, age) = bean;

上面的额声明会被编译成下面的代码：

	val id = bean.id;
	val name = bean.name;
	val age = bean.age;

这个特性背后的逻辑是非常强大的，它可以在很多情况下帮助我们简化代码。举个例子， Map 类含有一些扩展函数
的实现，允许它在迭代时使用key和value：

	for ((key, value) in map) {
		Log.d("map", "key:$key, value:$value")
	}

4,执行一个请求

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

5,在主线程以外执行请求

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

6,转换json到数据类

数据类修改如下：
	
	data class KotlinBean (
	    var id : Int,
	    var name : String,
	    var desc : String) {
	    constructor() : this(0, "", ""){
	    }
	
	    companion object {
	        val ID_0 = 0;
	        val ID_1 = 1;
	        val ID_2 = 2;
	    }
	
	    fun parse(o : JSONObject) : KotlinBean {
	        var bean : KotlinBean = KotlinBean();
	        bean.id = o.optInt("id");
	        bean.name = o.optString("name");
	        bean.desc = o.optString("desc");
	        return bean;
	    }
	}

请求的类相关：

	class Request(val zipcode : String) {
	    companion object {
	        val APP_ID = "b1b15e88fa797225412429c1c50c122a1";
	        val URL = "http://samples.openweathermap.org/data/2.5/weather?zip=94040,us";
	        val COMPLETE_URL = "$URL&appid=$APP_ID&q="
	    }
	
	
	    fun run() : KotlinBean {
	        val result = URL(COMPLETE_URL + zipcode).readText();
	        Log.d("result", "result : $result");
	        val bean = KotlinBean().parse(JSONObject(result));
	        return bean;
	    }
	}


7,构建domain层

我们现在创建一个新的包作为 domain 层。这一层中会包含一些 Commands 的实现来为app执行任务。

	interface Command<T> {
	    fun execute() : T
	}

这个command会执行一个操作并且返回某种类型的对象，这个类型可以通过范型被指定。你需要知道一个有趣的概念，一切kotlin函数都会返回一个值。如果没有指定，它就默认返回一个 Unit 类。所以如果我们想让Command不返回数据，我们可以指定它的类型为Unit。


当我们使用了两个相同名字的类，我们可以给其中一个指定一个别名，这样我们就不需要写完整的包名了
	
	import com.antonioleiva.weatherapp.domain.model.Forecast as ModelForecast

这些代码中另一个有趣的是我们从一个forecast list中转换为domain model的方法

	list!!.map { System.out.print(it.id) }

我们就可以循环这个集合并且返回一个转换后的新的List。Kotlin在List中提供了很多不错的函数操作符，它们可以在这个List的每个item中应用这个操作并且任何方式转换它们

8,with函数

with是一个非常有用的函数，它包含在Kotlin的标准库中。它接收一个对象和一个扩展函数作为它的参数，然后使这个对象扩展这个函数。这表示所有我们在括号中编写的代码都是作为对象（第一个参数） 的一个扩展函数，我们可以就像作为this一样使用所有它的public方法和属性。当我们针对同一个对象做很多操作的时候这个非常有利于简化代码

	val textView1 = TextView(this@TestActivity)
	with( textView1 ) {
	    textSize = sp(10f).toFloat()
	    textColor = Color.BLUE
	    leftDrawable(R.drawable.icon_user,10)
	}
