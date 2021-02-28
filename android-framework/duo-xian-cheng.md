# 知识点

[进程与线程的一个简单解释](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)

[Android几种进程通信方式的对比总结](https://blog.csdn.net/u011240877/article/details/72863432)

[Android为什么要设计出Bundle而不是直接使用HashMap来进行数据传递？](https://www.androidos.net.cn/doc/2019-10-28/39244.html)

[深入了解Bundle和Map](https://www.androidos.net.cn/doc/androidweekly/the-mysterious-case-of-the-bundle-and-the-map.html)

[Parcelable vs Serializable](http://www.developerphil.com/parcelable-vs-serializable/)

[Going multiprocess on Android](https://medium.com/@rotxed/going-multiprocess-on-android-52975ed8863c)

[Android IPC Mechanism](https://www.slideshare.net/nfsnfs/android-ipc-keynote11) 

[Demystify TransactionTooLargeException](https://medium.com/shopback-engineering/handle-transactiontoolargeexception-237961bd5ef8)

[Android IPC机制（一）开启多进程](http://liuwangshu.cn/application/ipc/1-process-start.html)  
[Android IPC机制（二）用Messenger进行进程间通信](http://liuwangshu.cn/application/ipc/2-messenger.html)  
[Android IPC机制（三）在Android Studio中使用AIDL实现跨进程方法调用](http://liuwangshu.cn/application/ipc/3-aidl.html)  
[Android IPC机制（四）用ContentProvider进行进程间通信](http://liuwangshu.cn/application/ipc/4-contentprovider.html)  
[Android IPC机制（五）用Socket实现跨进程聊天程序](http://liuwangshu.cn/application/ipc/5-socket.html) 

# aidl

[Android aidl Binder框架浅析](https://blog.csdn.net/lmj623565791/article/details/38461079)

[android跨进程通信（IPC）：使用AIDL](https://blog.csdn.net/singwhatiwanna/article/details/17041691)

[Android学习笔记——AS中使用AIDL](https://zhuanlan.zhihu.com/p/31460556)

[说一说aidl的oneway](https://mp.weixin.qq.com/s/wD3Io-ikS1VCljNkxEb6tQ)

[The legend about AIDL. Part 1. The roots](https://unbreakable-titan.medium.com/the-legend-about-aidl-part-1-the-roots-c144be38d9a4)  
[The legend about AIDL. Part 2. In Action](https://unbreakable-titan.medium.com/the-legend-about-aidl-part-2-in-action-f3dd552e4d17)  
[The legend about AIDL. Part 3. The crucial ingredient](https://unbreakable-titan.medium.com/the-legend-about-aidl-part-3-the-crucial-ingredient-71c5e8eea553)  

# Messenger

[Android应用进程间通信之Messenger信使使用及源码浅析](https://blog.csdn.net/yanbober/article/details/48373341)

[Android 基于Message的进程间通信 Messenger完全解析](https://blog.csdn.net/lmj623565791/article/details/47017485)

# Binder

[安卓Binder架构概述](https://www.androidos.net.cn/doc/android/issue/an-overview-of-android-binder-framework.html)

[浅谈Android系统进程间通信（IPC）机制Binder中的Server和Client获得Service Manager接口之路](https://www.androidos.net.cn/doc/androidos/15063.html)

[Android进程间通信（IPC）机制Binder简要介绍和学习计划](https://www.androidos.net.cn/doc/androidos/15168.html)

[为什么 Android 要采用 Binder 作为 IPC 机制？](https://www.zhihu.com/question/39440766/answer/89210950)

[写给 Android 应用工程师的 Binder 原理剖析](https://zhuanlan.zhihu.com/p/35519585)

[从 Android 6.0 源码的角度剖析 Binder 工作原理 | CSDN 博文精选](https://mp.weixin.qq.com/s/zdrIOqCBIofAjEnIck9gOQ)

[Android Binder机制浅析](https://blog.csdn.net/singwhatiwanna/article/details/19756201)

[从信息传递的角度来看Android中的广播和Binder](https://mp.weixin.qq.com/s/Jdgdq_PzKbI9hdGRPyNRBw)

[Android Binder Framework](https://android.jlelse.eu/android-binder-framework-8a28fb38699a)

[彻底理解Android Binder通信架构](http://gityuan.com/2016/09/04/binder-start-service/)

[Android Bander设计与实现 - 设计篇](https://blog.csdn.net/universus/article/details/6211589?utm_source=app)

[Binder学习指南](http://weishu.me/2016/01/12/binder-index-for-newer/)

[Binder原理系列博客](http://liuwangshu.cn/tags/Binder%E5%8E%9F%E7%90%86/)

[一篇文章了解binder进程间通信](https://mp.weixin.qq.com/s/lRLFzASMwEPKvLTWPiCzdQ)

[浅谈Binder](https://mp.weixin.qq.com/s/TK1AH5Dn90FgeRlDxlcqnQ)

[Binder线程优先级继承](https://mp.weixin.qq.com/s/omuuQQgJqQmK3qmDxUTa5w)

[图解Android中的binder机制](https://juejin.im/post/6844904115777044488)

[谈谈你对binder的理解](https://mp.weixin.qq.com/s/Ef2Qy_xFeI6WU3Q0wf5czA)  

[说说你对binder驱动的理解](https://mp.weixin.qq.com/s/LH_JR5Rwh1JL4B6qQkEv9Q)

[掌握binder机制，先搞懂这几个关键类](https://mp.weixin.qq.com/s/gHtZ9pjMJ-jXA12rvXA4cg)

[Binder 实现原理图解](https://juejin.cn/post/6844903882133356558)

[binder处理机制](https://mp.weixin.qq.com/s/M6IUYqQNp--oLRRpyP1ktQ)

# Parcelable

使用：

[Android中Parcelable接口用法](https://www.cnblogs.com/renqingping/archive/2012/10/25/Parcelable.html)

源码：

[详细介绍Android中Parcelable的原理和使用方法](https://www.jianshu.com/p/32a2ec8f35ae)

[Android Parcelable 源码解析](http://www.manongjc.com/article/62792.html)

# Intent

[将 Intent 序列化，像 Uri 一样传递 Intent！！！](https://juejin.im/post/59e0554951882578c6736ec8)

[Android组件系列----Intent详解](https://www.cnblogs.com/qianguyihao/p/3959204.html)











