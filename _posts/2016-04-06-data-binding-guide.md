---
layout: post
title:  Data Binding Guide(1)
date:   2016-03-19
categories: Android
---

이 문서는 구글의 [Data Binding Guide](http://developer.android.com/intl/ko/tools/data-binding/guide.html)를 참고하여 작성하였습니다.

Data Binding Library를 사용한 프로젝트는 다음과 같은 장점이 있습니다.

1. 서술적인 레이아웃(xml 파일)을 작성할 수 있다.
2. 코드의 로직과 레이아웃의 분리가 가능하다.

Data Binding Library는 Android 2.1 (API level 7 이상)에서 사용하기 때문에, 특수한 경우가 아니라면 거의 대부분의 프로젝트에서 활용할 수 있을 것으로 보입니다.

*data binding을 사용하려면, 안드로이드 Gradle 플러그인 버전이 1.5.0-alpha1 이상이어야함에 주의해주세요*


## 환경 구성하기

data binding을 사용하기 위해서는 Android SDK Manager에서 Support library를 다운 받아야 합니다.

또한 app이 data binding을 사용하기 위해서는 app 모듈 내에 위치한 build.gradle 파일 내에 dataBinding이라는 요소를 넣어주어야 합니다.

{% highlight groovy %}
android {

	// ...

	dataBinding {
		enabled = true
	}

	// ...

}
{% endhighlight %}

*만약에 data binding에 의존적인 라이브러리를 사용한다면 역시나 build.gradle 파일 내에 작성해줘야 합니다.*

*data binding에 대한 Android Studio의 호환성을 위해 Android Studio의 최소 버전은 1.3 이상을 권장합니다*


## 레이아웃 파일에 Data 바인딩하기

Data-binding을 하려면 최상위 레이아웃 태그는 layout이 되어야 하며, 그 아래 data태그, 그리고 실제 레이아웃의 rootView와 관련된 태그가 와야합니다.

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<layout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context="pe.devyunsy.databindingplayground.MainActivity">

    <data>

        <variable
            name="dog"
            type="pe.devyunsy.databindingplayground.Dog"/>
    </data>

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingBottom="@dimen/activity_vertical_margin"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{dog.name}"/>
    </RelativeLayout>
</layout>
{% endhighlight %}

data태그 내에 위치한 variable태그는 현재 레이아웃에서 사용될 요소를 나타냅니다.

실질적으로 data binding이 레이아웃 내에서 사용하기 위해서는 @{} 구문을 사용해야합니다.


### 객체 데이터

{% highlight java linenos %}
public class Dog {
    public final String name;

    public Dog(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public String name() {
        return name;
    }
}
{% endhighlight %}

위 코드를 예제로 들어서 설명 하겠습니다. data binding 관점에서 볼 때, POJO형태나, JavaBeans Object형태나 다 동일하게 @{dog.name}으로 접근이 가능합니다. name()이라는 메서드가 존재한다면 해당 메서드로도 접근이 가능합니다. 접근 방법이 여러가지로 제공 될 때는 getName() > name() > name 순으로 우선순위를 두고 있습니다.

단, POJO형태에서 바로 접근할 때에 Class 멤버 변수의 컨벤션인 mName이라는 형태로 변수명을 작성하게 되면, xml내에서도 mName으로 접근해야되므로, POJO 일때를 제외하고는 getter를 별도로 지정해 주는 것이, 가장 무난한 방법이 될 것 같습니다.


### 데이터 Binding

레이아웃 내에서 Binding을 구성하게 되면 자동으로 해당 레이아웃 파일명을 기반으로 한 Binding클래스가 생성됩니다. 만약 activity_main.xml이라는 레이아웃에 binding을 구성했다면, 생성된 클래스명은 ActivityMainBinding이 된다. 이 클래스가 바로 레이아웃 내에서 선언한 변수와, 뷰와 결합시켜주는 클래스가 된다. 실질적으로 바인딩을 구성하기 위해서는 다음과 같이 inflate할 때 함께 해주면 된다.

{% highlight java %}
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);

        Dog dog = new Dog("Happy");
        binding.setDog(dog);
    }
{% endhighlight %}


만약에 RecyclerView adapter에서 data binding을 사용한다면, 이런 방식으로 사용하면 된다.

{% highlight java %}
ListItemBinding binding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false);
{% endhighlight %}


### 이벤트 Binding

변수 뿐만 아니라 `android:onClick`같이 xml에서 묶을 수 있는 Handler메서드의 경우에 해당 메서드에 Event를 대입할 수 있다.

{% highlight java %}

// ...

public void onClickDog(View view) {
    Toast.makeText(view.getContext(), "Bark !", Toast.LENGTH_SHORT).show();
}

// ...

{% endhighlight %}

{% highlight xml %}

<!--중략-->

<Button
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:onClick="@{dog.onClickDog}"
	android:text="@{dog.name}"/>

<!--중략-->

{% endhighlight %}

어떤 Widget의 경우에는 `android:onClick`과의 충돌을 피하기 위해 특별한 Handler 메서드가 따로 존재한다.

| ------ | ------ | ------- |
|SearchView|`setOnSearchClickListener(View.OnClickListener)`|`android:onSearchClick`|
|ZoomControls|`setOnZoomInClickListener(View.OnClickListener)`|`android:onZoomIn`
|ZoomControls|`setOnZoomOutClickListener(View.OnClickListener)`|`android:onZoomOut`
