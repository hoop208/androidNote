# 输入子系统

## 知识点

[Android input 子系统:input进程的创建 监听线程的启动](https://mp.weixin.qq.com/s/h0kGWizIYn6FqBItwQJ7kQ)

[我把Android 10手势导航的侧滑返回效果优化了一波](https://mp.weixin.qq.com/s/1a9VAjsKW25rJUxH5pBG9A)

[Android input之ims初始化](https://mp.weixin.qq.com/s/Q9-T5fYg3TC3eMMfOl1Zhg)

[Android设备按键详解](https://mp.weixin.qq.com/s/3wPz7kO96RH8ZMJpuEceFA)

[开启全面屏体验 \| 手势导航 \(一\)](https://zhuanlan.zhihu.com/p/92906684)    
[处理视觉冲突 \| 手势导航 \(二\)](https://zhuanlan.zhihu.com/p/94082454)

[实现边到边的体验 让你的软键盘动起来\(一\)](https://mp.weixin.qq.com/s/U_c1l25kSUwnVLcIWYVBcQ)     
[响应窗口视图动画 让你的软键盘动起来\(二\)](https://mp.weixin.qq.com/s/RhTHRh41xDSx-wBtuZvP8w)    
[如何处理手势冲突 \| 手势导航连载 \(三\)](https://zhuanlan.zhihu.com/p/97576915)    
[沉浸模式 \| 手势导航连载 \(四\)](https://zhuanlan.zhihu.com/p/99696248)

## 软键盘  

[Android软键盘原理篇](https://mp.weixin.qq.com/s/vq0V1JWpyg6T5USCvGx_zA)

[Keyboard Handling on Android](https://pspdfkit.com/blog/2016/keyboard-handling-on-android/)

### 事件分发机制

[事件如何到达activity](https://mp.weixin.qq.com/s/cDuSEUAXjZ5vXBdeqjLlyQ)

[从安卓的事件分发说起](https://juejin.cn/post/6874589638925746190#heading-4)

[Android触摸事件源码解析](https://mp.weixin.qq.com/s/LmyXVFs9wvToAYHpqEYEMA)

[Touch事件分发响应机制](http://hukai.me/android-deeper-touch-event-dispatch-process/)

[Android View 事件分发机制 源码解析 （上）](https://blog.csdn.net/lmj623565791/article/details/38960443)

[Android ViewGroup事件分发机制](https://blog.csdn.net/lmj623565791/article/details/39102591)

[Android MotionEvent详解](https://www.jianshu.com/p/0c863bbde8eb)

[Android Deeper\(00\) - Touch事件分发响应机制](http://hukai.me/android-deeper-touch-event-dispatch-process/)

[Android中MotionEvent的来源和ViewRootImpl](https://blog.csdn.net/singwhatiwanna/article/details/50775201)

[重学安卓：学习 View 事件分发，就像外地人上了黑车](https://juejin.im/post/5d3140c951882565dd5a66ef)

[安卓基础：事件传递及滑动冲突的处理](https://blog.piasy.com/2017/02/12/Android-Basics-Touch-Event-System-Touch-Conflict/index.html)

[可能是讲解Android事件分发最好的文章](https://www.jianshu.com/p/2be492c1df96)

[Andriod 从源码的角度详解View,ViewGroup的Touch事件的分发机制](https://blog.csdn.net/xiaanming/article/details/21696315)

[彻底理解View事件体系！](https://blog.csdn.net/huachao1001/article/details/51766225)

[Android中TouchEvent触摸事件机制](https://blog.csdn.net/iispring/article/details/50364126)

[Android中的MotionEvent](https://blog.csdn.net/iispring/article/details/7208031)

[Android事件分发完全解析之为什么是她](https://blog.csdn.net/aigestudio/article/details/44260301)

[一个跟随手指移动的 View 产生的抖动问题](https://mp.weixin.qq.com/s/iJyErTxtsKrqFV-J7p96Lg)

[图解 Android 事件分发机制](https://www.jianshu.com/p/e99b5e8bd67b)

[Android事件分发之源码分析](https://mp.weixin.qq.com/s/vciuuN7LzY4Tp3saqPsYWg)

[图解Android View的事件传递](https://www.jianshu.com/p/bea1bb4aac95)

[讲解Android事件分发最好的文章](https://mp.weixin.qq.com/s/8mA7ZZbFEex7BMorBcgOqA)

[必问的事件分发 你答的上来吗](https://mp.weixin.qq.com/s/jkxMaN3v2xohjBuSfNdV8g)

[反思\|Android 事件分发机制的设计与实现](https://mp.weixin.qq.com/s/9rycOCr5GyfiQwJy0YSUUw)

[View 事件分发机制，看这一篇就够了](https://mp.weixin.qq.com/s/MhHTKwywee_GQBOKqZv3Eg)

[TouchDelegate 的这些盲区了解一下？](https://mp.weixin.qq.com/s/FOjnC8djhrBfFtoIWvMk5A)

[Android change touch area of View by TouchDelegate](https://android.jlelse.eu/android-change-touch-area-of-view-by-touchdelegate-fc19f2a34021)

[事件分发一 activity对触摸事件的分发流程](https://mp.weixin.qq.com/s/-Y3yCl2ONVQnBkWnp2UVGQ)  
[事件分发二 viewgroup和view对触摸事件的分发流程](https://mp.weixin.qq.com/s/yyShC5KlucD4I_u-hb1Zyw)

[Android输入系统的事件传递流程和IMS的诞生](https://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649843048&idx=1&sn=816b7ebf3e5301af44167130445d98ad&chksm=83bf6c33b4c8e52556e77b3a12b9a1d1a1b48a9bb6dc7ccb8800d79fc8315f3981db3299b942&scene=21#wechat_redirect)  
[只了解View的事件分发是不够的，来看下输入系统对事件的处理](https://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649843199&idx=1&sn=dbbffd681f32303f3761335ee9453907&chksm=83bf6ca4b4c8e5b2a39774e355558c0b1ac61af2666334511f9f9379985eaee13d33ce41e19c&scene=21#wechat_redirect)  
[你需要掌握的事件分发高阶知识](https://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649843337&idx=1&sn=ba9d6e61d5cff4f0ab83a70965d9ea0c&scene=19#wechat_redirect)  
[你所不知道的输入事件分发机制](https://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649843501&idx=1&sn=407eb2afc10c8de3f6b5b04f844b4ee0&scene=19#wechat_redirect)

[Android事件分发机制完全解析，带你从源码的角度彻底理解\(上\)](https://blog.csdn.net/guolin_blog/article/details/9097463)  
[Android事件分发机制完全解析，带你从源码的角度彻底理解\(下\)](https://blog.csdn.net/guolin_blog/article/details/9153747)

