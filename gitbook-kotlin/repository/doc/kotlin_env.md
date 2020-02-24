**2017-07-11-kotlin 开发环境搭建**

1，第一件事就是安装Android Studio。就如你知道的，Android Studio是官方的
Android IDE，它是2013年发布的预览版，并在2014年发布了正式版。

2,Gradle成为Android官方的系统构建工具，这意味着版本构建和部署的新的
可能性。最有趣的两点是系统构建和flavours，它可以让你使用相同的代码库来创
建无限的版本（甚至是不同的应用） 。

3,安装Kotlin插件
IDE它本身并不能理解Kotlin。就像前面部分讲到，它是为Java开发设计的。但是
Kotlin团队创建了一系列强大的插件让我们更轻松地实现。前往Android Studio
的 Preferences 中 Plugin 栏，然后安装如下两个插件：

	Kotlin：这是一个基础的插件。它能让Android Studio懂得Kotlin代码。它会每
	次在新的Kotlin语言版本发布的时候发布新的插件版本，这样我们可以通过它
	发现新版本特性和弃用的警告。这是你要使用Kotlin编写Android应用唯一的插
	件。但是我们现在还需要另外一个。
	
	Kotlin Android Extensions：Kotlin团队还为Android开发发布了另外一个有趣的
	插件。这个Android Extensions可以让你自动地从XML中注入所有的View到
	Activity中，举个例子，你不需要使用 findViewById() 。你将会立即得到一
	个从属性转换过来的view。你将需要安装这个插件来使用这个特性。我们会在
	下一章中深入地去讲解这个。

因为从Intellij 15开始，插件是被默认安装了的，但是你的Android Studio可能并没
有。所以你需要进入Android Studio 的Preferences的plugin栏，然后安装Kotlin插
件。如果你不会就去搜索下。

现在我们的环境已经可以理解Kotlin语言了，可以就像我们使用Java一样无缝地编
译它，执行它

4,配置Gradle
首先修改project的build.gradle,增加如下内容。版本号可以查一下最新的版本：

	buildscript {
		ext.support_version = '23.1.1'
		ext.kotlin_version = '1.1.1'
		ext.anko_version = '0.8.2'
		repositories {
			jcenter()
			dependencies {
				classpath 'com.android.tools.build:gradle:1.5.0'
				classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
			}
		}
	} 
	allprojects {
		repositories {
			jcenter()
		}
	}

对应的模块的build.gradle增加 Kotlin 标准库， Anko 库，以及 Kotlin 和 Kotlin Android
Extensions plugin 插件到dependencies。

	apply plugin: 'com.android.application'
	apply plugin: 'kotlin-android'
	apply plugin: 'kotlin-android-extensions'
	android {
		...
	} 
	dependencies {
		compile "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
		compile "com.android.support:appcompat-v7:$support_version"
		compile "org.jetbrains.anko:anko-common:$anko_version"
		compile 'org.jetbrains.anko:anko-sdk15:$anko_version'
	    compile 'org.jetbrains.anko:anko-support-v4:$anko_version'
	    compile 'org.jetbrains.anko:anko-appcompat-v7:$anko_version'
	} 
	buildscript {
		repositories {
			jcenter()
		} 
	
		dependencies {
			classpath "org.jetbrains.kotlin:kotlin-android-extensions:$kotlin_version"
		}
	}
	
Anko是一个用来简化一些Android任务的很强大的Kotlin库

5, 增加kolin代码sourceSet

	sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
            main.java.srcDirs += 'src/main/kotlin'
        }
    } 

6，测试是否一切就绪

首先，打开 activity_main.xml ，然后设置TextView的id：

	<TextView
		android:id="@+id/message"
		android:text="@string/hello_world"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"/>

在 onCreate 中，你现在可以直接得到并访问这个TextView了。

	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)
		message.text = "Hello Kotlin!"
	}

多亏Kotlin和Java之间的互操作性，我们可以在Kotlin中像操作属性一样去操作Java
库中的getter/setter方法。我们之后再去讲解属性，但是我想提醒的是，我们可以使
用 message.text 来代替 message.setText 。编译器将会把它转换成一般的
Java代码，所以这样使用是没有任何性能开销的