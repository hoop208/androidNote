# First Chapter

\# 参考 \#

  


  


 项目地址:

\[https://github.com/luxiaoming/dagger2Demo\]\(https://github.com/luxiaoming/dagger2Demo\)

  


  


 相关博客:

\[https://www.jianshu.com/p/cd2c1c9f68d4\]\(https://www.jianshu.com/p/cd2c1c9f68d4\)

  


  


  


\# 注释思路 \#

  


  


 !

\[\]\(imgs/1.png\)

  


  


\# 基本用法 \#

  


  


 1创建一个类,用了注解方式，使得Dagger2能找到它

  


  


public class Test1 {

  


  


@Inject

  


public Test1\(\) {

  


}

  


}

  


  


 2 定义连接器

  


  


@Component\(\)

  


public interface MainActivityComponent {

  


  


void inject\(MainActivity activity\);

  


  


}

  


  


 3 对象注入

  


  


public class MainActivity extends AppCompatActivity {

  


  


public static final String TAG = 

"

MainActivity

"

;

  


  


@Inject

  


Test1 test1;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_main\);

  


DaggerMainActivityComponent.builder\(\).build\(\).inject\(this\);

  


Log.d\(TAG,

"

test1=

"

+test1\);

  


}

  


}

  


  


\# 常规使用方法 \#

  


  


 1.1.实现一个类,标记为Module

  


 1.2.实现一些提供方法,作为这个模块能给外界提供的构造类\(这里能提供一个上下文和一个LocationManager对象\)

  


  


//标注 是一个模块 ，里面实现了一些提供的实例构造，后面使用Component来将这些连接起来。

  


@Module

  


public class AndroidModule {

  


private final DemoApplication application;

  


  


public AndroidModule\(DemoApplication application\) {

  


this.application = application;

  


}

  


  


@Provides

  


@Singleton

  


Context ApplicationContext\(\) {

  


return application;

  


}

  


  


@Provides

  


@Singleton

  


LocationManager provideLocationManager\(\) {

  


return \(LocationManager\) application.getSystemService\(LOCATION\_SERVICE\);

  


}

  


  


  


}

  


  


 2.1

标记一个Component,声明它依赖的module

  


 2.2

在用到的地方声明相应的inject接口

  


  


@Singleton

  


@Component\(modules = AndroidModule.class\)

  


public interface ApplicationComponent {

  


void inject\(DemoApplication application\);

  


Context getContext\(\);

  


}

  


  


 3.1

要用的LocationManager示例,用@inject标注

  


 3.2

调用DaggerApplicationComponent\(生成类\)的构建方法,并提供它依赖的module实例

  


 3.3

调用component的inject方法,完成注入

  


  


/\*\*

  


\*

常规使用方法

  


\*/

  


public class Test1Activity extends AppCompatActivity {

  


  


@Inject

  


LocationManager locationManager;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test2\);

  


DaggerApplicationComponent.builder\(\).androidModule\(new AndroidModule\(App.getApplication\(\)\)\)

  


.build\(\).inject\(this\);

  


Log.d\(Tag.TAG, 

"

locationManager=

"

 + locationManager\);

  


}

  


}

  


  


\# 带一个参数 \#

  


  


 1 定义带参数的构造函数

  


  


public class Test2 {

  


  


private Context context;

  


  


@Inject

  


public Test2\(Context context\) {

  


this.context = context;

  


}

  


}

  


  


 2 module中提供该参数的实例生成方案

  


  


@Module

  


public class AndroidModule {

  


  


private App app;

  


  


public AndroidModule\(App app\) {

  


this.app = app;

  


}

  


  


@Provides

  


Context provideContext\(\) {

  


return app;

  


}

  


  


@Provides

  


LocationManager provideLocationManager\(\) {

  


return \(LocationManager\) app.getSystemService\(Context.LOCATION\_SERVICE\);

  


}

  


  


}

  


  


 3.使用依赖该module的注入器注入

  


  


@Inject

  


Test2 test2;

  


  


@Inject

  


Test2 test2a;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test2\);

  


DaggerApplicationComponent.builder\(\).androidModule\(new AndroidModule\(App.getApplication\(\)\)\)

  


.build\(\).inject\(this\);

  


Log.d\(Tag.TAG, 

"

test2=

"

 + test2\);

  


Log.d\(Tag.TAG, 

"

test2a=

"

 + test2a\);

  


}

  


  


 tips:这两个对象的地址是不同的

  


  


 !

\[\]\(imgs/2.png\)

  


  


\# 组件依赖 \#

  


  


 1 定义一个类,构造函数需要locationmanager的参数

  


  


public class Test3 {

  


  


LocationManager locationManager;

  


  


public Test3\(LocationManager locationManager\) {

  


this.locationManager = locationManager;

  


}

  


}

  


  


 2 定义一个module提供该Test3示例

  


  


@Module

  


public class AModule {

  


  


@Provides

  


Test3 provideTest3\(LocationManager locationManager\){

  


return new Test3\(locationManager\);

  


}

  


  


}

  


  


 3 定义相应组件,依赖模组为Amodule,依赖组件为ApplicationComponent

  


  


@Component\(dependencies = ApplicationComponent.class, modules = AModule.class\)

  


public interface Test3ActivityComponent {

  


  


void inject\(Test3Activity test3Activity\);

  


  


}

  


  


 4 依赖组件暴露相应接口提供LocationManager实例

  


  


@Component\(modules = AndroidModule.class\)

  


public interface ApplicationComponent {

  


  


void inject\(App app\);

  


  


void inject\(Test1Activity test1Activity\);

  


  


void inject\(Test2Activity test2Activity\);

  


  


LocationManager getLocationManager\(\);

  


  


}

  


  


 5 使用

  


  


public class Test3Activity extends AppCompatActivity {

  


  


@Inject

  


Test3 test3;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test3\);

  


DaggerTest3ActivityComponent.builder\(\).aModule\(new AModule\(\)\)

  


.applicationComponent\(App.getApplication\(\).getApplicationComponent\(\)\)

  


.build\(\).inject\(this\);

  


Log.d\(Tag.TAG, 

"

test3=

"

 + test3\);

  


}

  


}

  


  


\# 单例 \#

  


  


 1.构造类加上@singleton注解

  


  


@Singleton

  


public class Test4 {

  


  


@Inject

  


public Test4\(\) {

  


}

  


}

  


  


 2.组件加上@singleton注解

  


  


@Singleton

  


@Component\(\)

  


public interface Test4ActivityComponent {

  


  


void inject\(Test4Activity test4Activity\);

  


  


}

  


  


 3.使用

  


  


public class Test4Activity extends AppCompatActivity {

  


  


@Inject

  


Test4 test4;

  


  


@Inject

  


Test4 test4a;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test4\);

  


DaggerTest4ActivityComponent.builder\(\)

  


.build\(\)

  


.inject\(this\);

  


Log.d\(Tag.TAG, 

"

test4=

"

 + test4\);

  


Log.d\(Tag.TAG, 

"

test4a=

"

 + test4a\);

  


}

  


}

  


  


 日志打印:

  


  


 !

\[\]\(imgs/3.png\)

  


  


\# 作用域 \#

  


  


 1.默认情况:

  


  


public class Test5 {

  


  


public Test5\(\) {

  


}

  


}

  


  


@Module

  


public class Module5 {

  


  


@Provides

  


Test5 provideTest5\(\){

  


return new Test5\(\);

  


}

  


  


}

  


  


@Component\(modules = Module5.class\)

  


public interface Test5ActivityComponent {

  


  


void inject\(Test5Activity test5Activity\);

  


  


}

  


  


public class Test5Activity extends AppCompatActivity {

  


  


@Inject

  


Test5 test5;

  


  


@Inject

  


Test5 test5a;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test5\);

  


DaggerTest5ActivityComponent.builder\(\)

  


.module5\(new Module5\(\)\)

  


.build\(\)

  


.inject\(this\);

  


Log.d\(Tag.TAG, 

"

test5=

"

 + test5\);

  


Log.d\(Tag.TAG, 

"

test5a=

"

 + test5a\);

  


}

  


}

  


  


 日志打印:

  


  


 !

\[\]\(imgs/4.png\)

  


  


 2.添加自定义作用域

  


  


@Scope

  


public @interface PerActivity {

  


}

  


  


@Module

  


public class Module5 {

  


  


@PerActivity

  


@Provides

  


Test5 provideTest5\(\){

  


return new Test5\(\);

  


}

  


  


}

  


  


@PerActivity

  


@Component\(modules = Module5.class\)

  


public interface Test5ActivityComponent {

  


  


void inject\(Test5Activity test5Activity\);

  


  


}

  


  


 日志打印:

  


  


 !

\[\]\(imgs/5.png\)

  


  


\# @name标记 \#

  


  


/\*\*

  


\*

学生

  


\*/

  


public class Student {

  


  


/\*\*

  


\*

性别

  


\*/

  


private String sex;

  


  


/\*\*

  


\*

构造一个学生对象

  


\*

@param sex

性别

  


\*/

  


public Student\(String sex\) {

  


this.sex = sex;

  


}

  


  


public String getSex\(\) {

  


return sex;

  


}

  


}

  


  


@Module

  


public class StudentModule {

  


  


/\*\*

  


\*

标记-女孩

  


\*/

  


public static final String QUALIFIER\_GIRL = 

"

gril

"

;

  


/\*\*

  


\*

标记-男孩

  


\*/

  


public static final String QUALIFIER\_BOY = 

"

boy

"

;

  


  


@Named\(QUALIFIER\_GIRL\)

  


@Provides

  


Student provideGirl\(\){

  


return new Student\(

"

女孩

"

\);

  


}

  


  


@Named\(QUALIFIER\_BOY\)

  


@Provides

  


Student provideBoy\(\){

  


return new Student\(

"

男孩

"

\);

  


}

  


  


}

  


  


@Component\(modules = StudentModule.class\)

  


public interface Test6ActivityComponent {

  


  


void inject\(Test6Activity test6Activity\);

  


  


}

  


  


public class Test6Activity extends AppCompatActivity {

  


  


@Named\(StudentModule.QUALIFIER\_BOY\)

  


@Inject

  


Student boy;

  


  


@Named\(StudentModule.QUALIFIER\_GIRL\)

  


@Inject

  


Student girl;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test6\);

  


DaggerTest6ActivityComponent.builder\(\)

  


.studentModule\(new StudentModule\(\)\)

  


.build\(\)

  


.inject\(this\);

  


Log.d\(Tag.TAG, 

"

boy=

"

 + boy.getSex\(\)\);

  


Log.d\(Tag.TAG, 

"

girl=

"

 + girl.getSex\(\)\);

  


}

  


}

  


  


 日志打印:

  


  


 !

\[\]\(imgs/6.png\)

  


  


\# 自定义标记 \#

  


  


public class Animal {

  


  


/\*\*

  


\*

物种

  


\*/

  


private String species;

  


  


public Animal\(String species\) {

  


this.species = species;

  


}

  


  


public String getSpecies\(\) {

  


return species;

  


}

  


  


}

  


  


@Qualifier

  


public @interface Cat {

  


}

  


  


@Qualifier

  


public @interface Dog {

  


}

  


  


@Component\(modules = AnimalModule.class\)

  


public interface Test7ActivityComponent {

  


  


void inject\(Test7Activity test7Activity\);

  


  


}

  


  


public class Test7Activity extends AppCompatActivity {

  


  


@Cat

  


@Inject

  


Animal cat;

  


  


@Dog

  


@Inject

  


Animal dog;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test7\);

  


DaggerTest7ActivityComponent.builder\(\)

  


.animalModule\(new AnimalModule\(\)\)

  


.build\(\)

  


.inject\(this\);

  


Log.d\(Tag.TAG, 

"

cat=

"

 + cat.getSpecies\(\)\);

  


Log.d\(Tag.TAG, 

"

dog=

"

 + dog.getSpecies\(\)\);

  


}

  


}

  


  


 日志打印:

  


  


 !

\[\]\(imgs/7.png\)

  


  


\# 子组件 \#

  


  


\# 懒加载 \#

  


  


/\*\*

  


\*

网站

  


\*/

  


public class Website {

  


  


/\*\*

  


\*

网址

  


\*/

  


private String url;

  


  


/\*\*

  


\*

构造一个网页对象

  


\*

@param url

网址

  


\*/

  


public Website\(String url\) {

  


Log.d\(Tag.TAG,

"

构造一个网站=

"

+url\);

  


this.url = url;

  


}

  


}

  


  


@Module

  


public class WebsiteModule {

  


  


@Provides

  


Website provideWebsite\(\){

  


return new Website\(

"

www.baidu.com

"

\);

  


}

  


  


}

  


  


@Component\(modules = WebsiteModule.class\)

  


public interface Test8ActivityComponent {

  


  


void inject\(Test8Activity test8Activity\);

  


  


}

  


  


public class Test8Activity extends AppCompatActivity {

  


  


@Inject

  


Lazy

&lt;

Website

&gt;

 websiteLazy;

  


  


//

@Inject

  


//

Website website;

  


  


@Override

  


protected void onCreate\(Bundle savedInstanceState\) {

  


super.onCreate\(savedInstanceState\);

  


setContentView\(R.layout.activity\_test8\);

  


DaggerTest8ActivityComponent.builder\(\)

  


.websiteModule\(new WebsiteModule\(\)\)

  


.build\(\)

  


.inject\(this\);

  


Log.d\(Tag.TAG, 

"

注入完成:Test8Activity

"

\);

  


}

  


  


public void getWeb\(View view\) {

  


//

Log.d\(Tag.TAG, 

"

website=

"

 + website\);

  


Website web = websiteLazy.get\(\);

  


Log.d\(Tag.TAG, 

"

web=

"

 + web\);

  


}

  


  


}

  


  


 tips:当调用websiteLazy.get\(\)时才会创建Website对象.

  


  


\# 多个绑定方式 \#

  


