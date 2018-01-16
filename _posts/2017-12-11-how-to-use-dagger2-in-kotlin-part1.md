---
layout: post
title: Kotlin中使用Dagger2 PART1
date: 2017-12-11
categories: Android
tags: [android]
description: 
---

### 概述


Dagger2作为依赖注入神器，相信很多朋友都听说过它的大名。只不过它的有些概念，理解起来并不是那么清晰，并且在使用的过程中，也比较迷糊。我将把自身对Dagger2的理解、使用经验分享给大家，希望对大家有所帮助。我将分几节详细介绍Dagger2在Kotlin在如何使用，因为在Java中使用方式大同小异，所以大家理解了Dagger2，无论在Java还是Kotlin都能运用自如。


### 本节内容
这一小节，我们先简单介绍一下Dagger2的基本使用，主要包括：


* Dagger2环境配置
* 依赖注入（DI）
* @Inject
* @Component

### Dagger2环境配置

首先，需要在Module中build.gradle引入插件（Java中使用apt，Kotlin使用kapt）

``` 
apply plugin: 'kotlin-kapt'
```

接下来，引入依赖包和编译器
```  
compile "com.google.dagger:dagger:2.14.1"
kapt "com.google.dagger:dagger-compiler:2.14.1"
```

### 依赖注入

大家记住一点，Dagger2是一个依赖注入框架，它能做的事就是完成依赖注入。那么什么是依赖注入。我们通过两段代码，直观的看一下。

* 第一种：传统方式，主动实例化
``` 
     lateinit var mPresenter:MainPresenter
     
     override fun onCreate(savedInstanceState: Bundle?) {
         super.onCreate(savedInstanceState)
         setContentView(R.layout.activity_main)
         mPresenter = MainPresenter()
     }
```

可以看到，成员变量mPresenter是通过构造方法直接实例化。

* 第二种：通过注解，注入实例化
``` 
 @Inject
    lateinit var mPresenter:MainPresenter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
``` 

可以看到，这次我们没有调用构造方法，而是在成员变量声明时，添加了一个注解@Inject，这样也能实例化成员变量mPresenter。后面大家会看，当调用mPresenter时，它并不为空，说明我们实例化是成功的。

这就是两种实例化方式，一种需要主动实例化，一种是被动接收，通过注解，自动实例化。

### @Inject
在上面我们看到，注解@Inject能够实例化对象，那它到底是什么？

* @Inject是Dagger2内置的一个注解；
* @Inject用来标注目标对象（如上面的mPresenter）；
* @Inject用来标注依赖类的构造方法（如上面的MainPresenter）；
* 上面三点，前两点都好理解，第三点“标注依赖类的构造方法”是什么意思，直接看代码：

```
   class MainPresenter @Inject constructor() {
        fun doSomething():String{
            return "This is result"
        }
    }
```

可以看到，我们定义了一个类MainPresenter，在它的构造方法上添加了@Inject注解（由于有注解出现，显示无参构造器constructor），同时定义了一个方法doSomething，返回一个字符串。

为什么需要在MainPresenter构造方法上添加@Inejct，大家看下，mPresenter的类型是不是MainPresenter。这就告诉编译器，MainPresenter可通过Dagger2注入到目标类为MainPresenter类型的变量，通过构造方法实例化。


### @Component
   
现在我们有@Inject标注的目标类，也有@Inject标注的依赖构造方法，那它们是不是就可以直接实例化了？答案是否定的，它们现在是独立的，并不知道对方的存在，如何将它们联系起来， 那就需要一个桥梁---Component，把依赖类连接到目标类。

首先，我们看一下Component如何声明
```
    @Component
    interface MainComponent {
        fun inject(activity:MainActivity)
    }
```

可以看到，我们定义了一个接口(必须是接口或抽象类)，使用@Component进行标注，同时提供了一个方法inject。inject中的参数就是我们需要注入开始的类。

OK，现在我们桥梁Component也有了，如何让桥梁起作用。编译一下，编译一下，编译一下，说三遍。编译完成后，会生成一个以“Dagger”开头的Component文件，如DaggerMainComponent，接下来咱们使用一下：

```
 class MainActivity : AppCompatActivity() {

        @Inject
        lateinit var mPresenter:MainPresenter

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            initInjection()

            mClickBtn.setOnClickListener {
                toast(mPresenter.doSomething())
            }
        }

        /*
            Dagger2注入注册
         */
        private fun initInjection() {
            DaggerMainComponent.builder().build().inject(this)

        }
    }
```  

可以看到，要想桥梁起作用，需要调用DaggerMainComponent的inject方法进行“注册”。
以上代码，在使用mPresenter之前，需要Dagger2注册，其中有一个按钮，点击的话，会弹一个Toast,
内容就是mPresenter的返回值。如果mPresenter为空，肯定报异常了。如果一切正常，说明注入是成功的。
 