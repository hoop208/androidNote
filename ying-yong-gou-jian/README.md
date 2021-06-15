# 应用构建

## 知识点

[Android执行文件apk的组成结构](https://blog.csdn.net/itachi85/article/details/6460158)

[搭建 Maven 私有仓库](https://juejin.im/entry/58eb3e3da22b9d0058aafc28)

[Android Studio 的 build 过程](https://segmentfault.com/a/1190000015373074)

[Android R.java类的手动生成](https://blog.csdn.net/ccpat/article/details/50738811)

[关于Android编译，你需要了解什么](https://segmentfault.com/a/1190000020866199)

[浅谈androiddex文件](https://tech.youzan.com/qian-tan-android-dexwen-jian/)

[andresguard原理介绍](https://mp.weixin.qq.com/s?__biz=MzAwNDY1ODY2OQ==&mid=208135658&idx=1&sn=ac9bd6b4927e9e82f9fa14e396183a8f#rd)

[android apk 资源加载流程](https://mp.weixin.qq.com/s/dlsPCIHsVKrh1lARU3sdSg)

[applychanges背后的秘密](https://mp.weixin.qq.com/s/6wsK0nGJkEEI5kiECvaE6A)

[Android打包流程详解](https://mp.weixin.qq.com/s/er8sVGi7cZo_7p4jFoiPGg)

[Android增量编译总结](https://mp.weixin.qq.com/s/PiccKf2_ue6OXfwlDk1EBA)

[Android-研究Apk重打包](https://mp.weixin.qq.com/s/apQ47eXausldjKNsFO2o8A)

[Android 打包过程](https://mp.weixin.qq.com/s/XN6-6h5sBxCdbGrRD9uBZg)

[Android主项目和Module中R类的区别](https://juejin.im/post/5a98f240f265da23830a5193)

[从R文件索引看资源优化](https://linjiang.tech/2020/01/20/r-resources-arsc/)

[Getting inside APK files](https://android.jlelse.eu/getting-inside-apk-files-21dbd01529d4)

[How to manage dependencies in a multi module project?](https://proandroiddev.com/how-to-manage-dependencies-in-multi-module-project-84620afbb415)

[To ∞ \(~65K\) and beyond!](https://speakerdeck.com/dextor/to-65k-and-beyond)

[Instant Run: How Does it Work?!](https://medium.com/google-developers/instant-run-how-does-it-work-294a1633367f#.qw7xnzrpm)

[Dissection of an APK](https://proandroiddev.com/dissection-of-an-apk-7ae9321c7b9a)

[The Dex File Format](https://www.bugsnag.com/blog/dex-and-d8)

[HB Blog 42: What Is The Purpose Of R.java File In Android Application?](https://harshalbenake.blogspot.com/2014/12/hb-blog-42-what-is-purpose-of-rjava-file.html)

[Optimizing Android bytecode with ReDex](https://engineering.fb.com/android/optimizing-android-bytecode-with-redex/?refid=8&_ft_=qid.6200742327944805904%253Amf_story_key.6249203789055394671%253AeligibleForSeeFirstBumping.1&__tn__=H)

[Efficiently reducing your method count](https://jeroenmols.com/blog/2016/05/06/methodcount/)

[聊聊 APK —— 直接运行 Dex](https://geminiwen.com/archives/77/)  
[聊聊 APK —— Dex 热修复与 Classpath](https://geminiwen.com/archives/79/)  
[聊聊 APK —— aapt 编译资源](https://geminiwen.com/archives/82/)  
[聊聊 APK —— 脱离 AS 手工创造 APK 文件](https://geminiwen.com/archives/89/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

## makefile

[Android Build cookbook\| Build Apps or Libraries with Make file](https://gizmomind.com/android-make-file-android_mk-cookbook/)

## 签名机制

[Android签名机制 v1 v2 v3](https://mp.weixin.qq.com/s/D1V4k-pakJLwgEa90_WjWQ)

[Android V1及V2签名原理简析](https://juejin.im/post/5cd239386fb9a0320f7dfcbe)

## 代码混淆

[Android如何优雅地防止Bean类混淆](https://blog.csdn.net/dbs1215/article/details/53358406)

[Debugging Proguard configuration issues](https://krossovochkin.com/posts/2021_01_18_debugging_proguard_configuration_issues/)

[How to generate Proguard/R8 rules for Navigation component arguments](https://android.jlelse.eu/how-to-generate-proguard-r8-rules-for-navigation-component-arguments-466e72e75ca7)

[Proguard in \(Multi-Module Android App \|\| Custom Library\).](https://medium.com/native-mobile-bits/proguard-in-multi-module-android-app-custom-library-4893a2143e8b)

[Troubleshooting ProGuard issues on Android](https://medium.com/androiddevelopers/troubleshooting-proguard-issues-on-android-bce9de4f8a74)

[Practical ProGuard rules examples](https://medium.com/androiddevelopers/practical-proguard-rules-examples-5640a3907dc9)

[Android's Built-in ProGuard Rules: The Missing Guide](https://www.zacsweers.dev/android-proguard-rules/)

[Distinguishing between the different ProGuard “-keep” directives](https://jebware.com/blog/?p=418)  
[Reading ProGuard’s Outputs](https://jebware.com/blog/?p=484)  
[Feeding ProGuard’s inputs: where are all of these rules coming from?](https://jebware.com/blog/?p=498)  
[ProGuard pro-tip: Don’t use ProGuard’s “-include” with Gradle builds](https://jebware.com/blog/?p=434)

## Multidex

[Android的multidex带来的性能问题－减慢app启动速度](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/1223/3796.html)

[Multidex（一）之源码解析](https://www.jianshu.com/p/e164ee033928)

[Multidex（二）之Dex预加载优化](https://www.jianshu.com/p/2891599511ff)

[MultiDex（三）之异步加载优化](https://www.jianshu.com/p/50019766e893)

[谈谈MultiDex启动优化](https://neyoufan.github.io/2017/01/20/android/Multidex%E5%8A%A0%E9%80%9F/)

[Android Multidex导致的App启动缓慢](https://github.com/hehonghui/android-tech-frontier/blob/master/issue-39/Android%20dex%E5%88%86%E5%8C%85%E5%AF%BC%E8%87%B4%E7%9A%84App%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E4%B8%8B%E9%99%8D.md)

## APT

[Android APT（编译时代码生成）最佳实践](https://joyrun.github.io/2016/07/19/AptHelloWorld/)

[Android 通过 APT 解耦模块依赖](https://www.kymjs.com/code/2018/08/12/01/)

[Android编译期代码生成之apt实践入门](https://segmentfault.com/a/1190000005100468)

## 编译速度优化

[Android 应用构建速度提升的十个小技巧](https://mp.weixin.qq.com/s?__biz=MzAwODY4OTk2Mg==&mid=2652050269&idx=1&sn=079e9e84cda81b24ee4446d404276772&chksm=808cb318b7fb3a0e22a6411ba606b4161790a0a8c17f5f5b579099ae6b0dd5eafc8f4ae6e1c5&scene=178&cur_album_id=1339618889533292546#rd)

[Android 优化APP 构建速度的17条建议](https://www.jianshu.com/p/a1cc8f2e0877)

[Android全量编译加速——（透明依赖）](https://mp.weixin.qq.com/s/tK9z6QjMvMeakpTHdTBBBg)

[Free Up System Resources For Faster Gradle Builds](https://goobar.io/free-up-system-resources-for-faster-gradle-builds/)

[Benchmarking Gradle Builds Using Gradle-Profiler](https://goobar.io/benchmarking-gradle-builds-using-gradle-profiler/)

[Understanding your build with the Build Analyzer](https://medium.com/androiddevelopers/understanding-your-build-with-the-build-analyzer-5c15688ec72e)

[Improving build speed in Android Studio](https://medium.com/androiddevelopers/improving-build-speed-in-android-studio-3e1425274837)

## D8

[The D8 dexer](https://proandroiddev.com/the-d8-dexer-6736deb55fb8)

## r8

[使用r8压缩应用](https://mp.weixin.qq.com/s/zDx-SdsqargT4JB6oMIrTw)

[r8编译器 为kotlin库和应用瘦身](https://mp.weixin.qq.com/s/E-OPviIm-ndK1I_PRNostA)

[Shrinking Kotlin Libraries and Applications using Kotlin Reflection with R8](https://medium.com/androiddevelopers/shrinking-kotlin-libraries-and-applications-using-kotlin-reflection-with-r8-6fe0a0e2d115)

[How to break your Android App with proguard / R8](https://medium.com/@woitaschek/how-to-break-your-android-app-with-proguard-r8-6566bc387b63)

[Proguard/R8 in the world of modularity](https://android.jlelse.eu/proguard-r8-in-the-world-of-modularity-f599650b4553)

[When using enums and R8…](https://medium.com/androiddevelopers/when-using-enums-and-r8-3f8f314c0a13)

[Shrinking Your App with R8](https://medium.com/androiddevelopers/shrinking-your-app-with-r8-909efac25de4)

