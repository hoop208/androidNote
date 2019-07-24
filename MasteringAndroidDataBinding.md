# 参考 #



# 点击事件 #

1.定义点击事件执行者

    public class Presenter {

        public void onClickSimpleDemo(View view) {
            startActivity(new Intent(MainActivity.this, SimpleActivity.class));
        }

        ...

    }

2.布局中指定

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools">
	
	    <data>
	
	        <variable
	            name="presenter"
	            type="com.example.hoop.databindingdemo.MainActivity.Presenter"/>
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:gravity="center_horizontal"
	        android:orientation="vertical"
	        tools:context=".MainActivity">
	
	        <Button
	            android:layout_width="match_parent"
	            android:layout_height="50dp"
	            android:onClick="@{presenter.onClickSimpleDemo}"
	            android:text="@string/demo_simple"
	            android:textColor="@color/colorPrimary"/>
	
			...

	    </LinearLayout>

	</layout>

3.Activity中使用

    public class MainActivity extends AppCompatActivity {

	    public class Presenter {
	
	        public void onClickSimpleDemo(View view) {
	            startActivity(new Intent(MainActivity.this, SimpleActivity.class));
	        }
	
	    }
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        ActivityMainBinding dataBinding = DataBindingUtil.setContentView(this, R.layout.activity_main);
	        dataBinding.setPresenter(new Presenter());
	    }

	}

# 简单使用 #

目标效果:文本框内容跟随输入框内容同步变化

![](imgs/databinding/1.gif)

1.定义一个对象实现BaseObservable,要观察的字段添加@Bindable注解

    public class User extends BaseObservable {

	    private String name;
	
	    public User(String name) {
	        this.name = name;
	    }
	
	    @Bindable
	    public String getName() {
	        return name;
	    }
	
	    public void setName(String name) {
	        this.name = name;
	        notifyPropertyChanged(BR.name);
	    }
	}

2.定义监听文本变化的presenter

    public class Presenter {

        public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
            user.setName(charSequence.toString());
        }

    }

3.布局中使用

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context="com.example.hoop.databindingdemo.SimpleActivity">
	
	    <data>
	
	        <variable
	            name="user"
	            type="com.example.hoop.databindingdemo.User"/>
	
	        <variable
	            name="presenter"
	            type="com.example.hoop.databindingdemo.SimpleActivity.Presenter"/>
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical">
	
	        <EditText
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:onTextChanged="@{presenter.onTextChanged}"
	            android:hint="输入姓名"/>
	
	        <TextView
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:text="@{user.name}"
	            tools:text="示例文字"/>
	
	    </LinearLayout>

	</layout>

4.Activity中绑定

    public class SimpleActivity extends AppCompatActivity {
	
	    User user = new User("abc");
	
	    public class Presenter {
	
	        public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
	            user.setName(charSequence.toString());
	        }
	
	    }
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        ActivitySimpleBinding dataBinding = DataBindingUtil.setContentView(this, R.layout.activity_simple);
	        dataBinding.setUser(user);
	        dataBinding.setPresenter(new Presenter());
	    }
	}

# 基本使用 #

目标效果:

展示人物姓名年龄,名字首字母大写,如果大于18岁展示"成人标记"

![](imgs/databinding/1.png)

1.定义人物实体类

    /**
	 * 人物对象
	 */
	public class Person {
	
	    /**
	     * 名
	     */
	    private final String firstName;
	    /**
	     * 姓
	     */
	    private final String lastName;
	    /**
	     * 显示名称(全名)
	     */
	    private String displayName;
	    /**
	     * 年龄
	     */
	    private int age;
	
	    /**
	     * 构造一个人物
	     *
	     * @param firstName 名
	     * @param lastName  姓
	     */
	    public Person(String firstName, String lastName) {
	        this.firstName = firstName;
	        this.lastName = lastName;
	    }
	
	    /**
	     * 构造一个人物
	     *
	     * @param firstName 姓
	     * @param lastName  名
	     * @param age       年龄
	     */
	    public Person(String firstName, String lastName, int age) {
	        this.firstName = firstName;
	        this.lastName = lastName;
	        this.age = age;
	    }
	
	    public String getFirstName() {
	        return firstName;
	    }
	
	    public String getLastName() {
	        return lastName;
	    }
	
	    public String getDisplayName() {
	        return firstName + " " + lastName;
	    }
	
	    public int getAge() {
	        return age;
	    }
	
	    public boolean isAdult() {
	        return age >= 18;
	    }

	}

2.布局中使用

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context=".BasicActivity">
	
	    <data>
	
	        <import type="com.example.hoop.databindingdemo.util.MyStringUtils" />
	
	        <import type="android.view.View" />
	
	        <variable
	            name="person"
	            type="com.example.hoop.databindingdemo.Person" />
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical">
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="全名:"
	            android:textColor="#0000ff" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="@{person.displayName??person.lastName}" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="firstName:"
	            android:textColor="#0000ff" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="@{MyStringUtils.capitalize(person.firstName)}" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="lastName:"
	            android:textColor="#0000ff" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="@{person.lastName}" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="年龄:"
	            android:textColor="#0000ff" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="@{String.valueOf(person.age)}" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="成人"
	            android:textColor="#0000ff"
	            android:visibility="@{person.isAdult?View.VISIBLE:View.GONE}" />
	
	    </LinearLayout>

	</layout>

3.Activity中执行绑定

    public class BasicActivity extends AppCompatActivity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_basic);
	        ActivityBasicBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_basic);
	        Person person = new Person("hoop", "zeng", 27);
	        binding.setPerson(person);
	    }
	}

# 自定义binding类名 #

1 定义class全名

    <data class=".UserBinding">

        <variable
            name="user"
            type="com.example.hoop.databindingdemo.User"/>

    </data>

2 使用:

    UserBinding userBinding = DataBindingUtil.setContentView(this, R.layout.activity_custom_binding);


# include #

1 include中定义binding对象

    <layout xmlns:android="http://schemas.android.com/apk/res/android">

	    <data>
	
	        <variable
	            name="btnText"
	            type="String"/>
	
	        <variable
	            name="presenter"
	            type="com.example.hoop.databindingdemo.IncludeActivity.IncludePresenter"/>
	
	    </data>
	
	    <RelativeLayout
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content">
	
	        <Button
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:text="@{btnText}"
	            android:onClick="@{presenter.clickBtn}"/>
	
	    </RelativeLayout>

	</layout>

2.父布局中也需要定义相关的变量,include的时候传递下去

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context=".IncludeActivity">
	
	    <data>
	
	        <variable
	            name="btnText"
	            type="String" />
	
	        <variable
	            name="presenter"
	            type="com.example.hoop.databindingdemo.IncludeActivity.IncludePresenter" />
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical">
	
	        <include
	            layout="@layout/include_btn"
	            app:btnText="@{btnText}"
	            app:presenter="@{presenter}" />
	
	    </LinearLayout>

	</layout>

3.调用

    public class IncludeActivity extends AppCompatActivity {

	    public class IncludePresenter {
	
	        public void clickBtn(View view) {
	            Toast.makeText(IncludeActivity.this, "clickBtn", Toast.LENGTH_SHORT).show();
	        }
	
	    }
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_include);
	        ActivityIncludeBinding activityIncludeBinding = DataBindingUtil.setContentView(this, R.layout.activity_include);
	        activityIncludeBinding.setBtnText("确定");
	        activityIncludeBinding.setPresenter(new IncludePresenter());
	    }
	}

# 操作集合 #

变量定义需要制定泛型,尖括号用转义符代替,使用的方法有[],get()等

    <?xml version="1.0" encoding="utf-8"?>
	<layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context=".CollectionActivity">
	
	    <data>
	
	        <variable
	            name="list"
	            type="java.util.List&lt;String&gt;" />
	
	        <variable
	            name="map"
	            type="java.util.Map&lt;String,String&gt;" />
	
	        <variable
	            name="sparse"
	            type="android.util.SparseArray&lt;String&gt;" />
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical"
	        android:padding="16dp">
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="list[index]" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:textColor="#0000ff"
	            android:text="@{list[0]}"/>
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="sparse.get()" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:textColor="#0000ff"
	            android:text="@{sparse.get(0)}"/>
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="map.get()" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text='@{map.get("firstName")}'/>
	
	    </LinearLayout>

	</layout>

在activity中绑定

    public class CollectionActivity extends AppCompatActivity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        ActivityCollectionBinding activityCollectionBinding = DataBindingUtil.setContentView(this, R.layout.activity_collection);
	        ArrayList<String> list = new ArrayList<>();
	        list.add("hoop");
	        activityCollectionBinding.setList(list);
	        SparseArray<String> sparseArray = new SparseArray<>();
	        sparseArray.put(0, "jack");
	        activityCollectionBinding.setSparse(sparseArray);
	        HashMap<String, String> map = new HashMap<>();
	        map.put("firstName", "tom");
	        activityCollectionBinding.setMap(map);
	    }

	}

# 操作资源 #

	<dimen name="text_large">48sp</dimen>
    <dimen name="text_small">12sp</dimen>
	
	<string name="nameformat">全名:%s.%s</string>

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context=".ResourceActivity">
	
	    <data>
	
	        <variable
	            name="largeText"
	            type="Boolean" />
	
	        <variable
	            name="firstName"
	            type="String"/>
	
	        <variable
	            name="lastName"
	            type="String"/>
	
	    </data>
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical"
	        android:padding="16dp">
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:background="#000000"
	            android:text="helloworld"
	            android:textColor="#ffffff"
	            android:textSize="@{largeText?@dimen/text_large:@dimen/text_small}" />
	
	        <TextView
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content"
	            android:text="@{@string/nameformat(firstName,lastName)}"/>
	
	
	    </LinearLayout>

	</layout>

# 带id的控件 #

布局文件:

    <?xml version="1.0" encoding="utf-8"?>
	<layout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context=".ViewWithIdActivity">
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical">
	
	        <Button
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:text="showName"
	            android:onClick="showName"/>
	
	        <TextView
	            android:id="@+id/firstName"
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content" />
	
	        <TextView
	            android:id="@+id/lastName"
	            android:layout_width="wrap_content"
	            android:layout_height="wrap_content" />
	
	    </LinearLayout>

	</layout>

activity中使用:

    public class ViewWithIdActivity extends AppCompatActivity {

	    private ActivityViewWithIdBinding binding;
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_view_with_id);
	        binding = DataBindingUtil.setContentView(this, R.layout.activity_view_with_id);
	    }
	
	    public void showName(View view) {
	        binding.firstName.setText("micheal");
	        binding.lastName.setText("jordan");
	    }
	}





