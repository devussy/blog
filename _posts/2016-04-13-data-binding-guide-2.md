---
layout: post
title:  Data Binding Guide(2)
date:   2016-04-13
categories: Android
---

***
이 문서는 구글의 [Data Binding Guide](http://developer.android.com/intl/ko/tools/data-binding/guide.html)의 내용을 번역후, 일부 수정하여 작성되었습니다.
***

이번에는 지난 시간에 이어 xml 내에서 **Data Binding** 을 더욱 활용할 수 있는 다양한 표현식들에 대해서 알아보겠습니다.

# Layout Details

## Import

`<variable>` 태그로 바인딩할 변수를 지정한 것 만으로도 충분히 활용도가 높지만, 아무래도 xml내에서 크고 작은 연산을 하기에는 부족한 느낌이 있었습니다. 이 때 `<data>` 태그 내에 `<import>`요소를 넣음으로써 xml 파일 내에서 레퍼런스를 가지고 사용할 수 있게 됩니다.

{% highlight xml %}
<data>
	<import type=“android.view.View”/>
</data>
{% endhighlight %}

이렇게 요소를 지정해 주게 되면, View클래스는 binding expression 내에서 상수, static 변수와 메서드 등을 사용 가능하게 됩니다.

{% highlight xml %}
<TextView
	android:text="@{user.lastName}"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
{% endhighlight %}

만약에 클래스명이 중복이라면 *alias* 속성을 지정해줌으로써 클래스간의 구분이 가능합니다.

{% highlight xml %}
<import type="android.view.View"/>
<import type="com.example.real.estate.View"
				alias="Vista"/>
{% endhighlight %}

또한, import로 지정해둔 클래스는 `<variable>`태그의 *type* 속성에서도 사용이 가능합니다.

{% highlight xml %}
<data>
    <import type="com.example.User"/>
    <import type="java.util.List"/>
    <variable name="user" type="User"/>
    <variable name="userList" type="List&lt;User>"/>
</data>
{% endhighlight %}


## Includes

변수들은 xml내의 `<include>` 태그에서 지정된 레이아웃 파일로 전달 할 수 있습니다. 이 때, 해당 레이아웃 파일 내에 반드시 bind할 변수명과 동일한 `<variable>` 태그가 존재해야합니다.

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:bind="http://schemas.android.com/apk/res-auto">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <include layout="@layout/name"
           bind:user="@{user}"/>
       <include layout="@layout/contact"
           bind:user="@{user}"/>
   </LinearLayout>
</layout>
{% endhighlight %}


# Expression  Language

## Missing Operations

Data Binding 시, xml 내에서 사용 못하는 연산자가 있으므로 주의해야 합니다.

- this
- super
- new

## Null Coalescing Operator

Null Coalescing 연산자는 해당 변수가 null이 아닐 경우 좌항을, null일 경우에는 우항을 선택하는 특수 연산자입니다.

{% highlight xml %}
android:text="@{user.displayName ?? user.lastName}"
{% endhighlight %}

이 예시는 다음과 동일한 의미를 가집니다.

{% highlight xml %}
android:text="@{user.displayName != null ? user.displayName : user.lastName}"
{% endhighlight %}

만약에 Null에 대한 처리를 제대로 하지 않으면 각 타입의 기본 값인 null 또는 0이 들어가게 되므로 주의하셔야합니다.

## Collections

대부분의 Collections 타입을 사용할 수 있으며, 이때 요소에 대해 `[]` 연산자로 편리하게 접근할 수 있습니다.

{% highlight xml %}
<data>
    <import type="android.util.SparseArray"/>
    <import type="java.util.Map"/>
    <import type="java.util.List"/>
    <variable name="list" type="List&lt;String>"/>
    <variable name="sparse" type="SparseArray&lt;String>"/>
    <variable name="map" type="Map&lt;String, String>"/>
    <variable name="index" type="int"/>
    <variable name="key" type="String"/>
</data>
…
android:text="@{list[index]}"
…
android:text="@{sparse[index]}"
…
android:text="@{map[key]}"
{% endhighlight %}

## String Literals

만약에 Data Binding 표현식안에서 String을 사용해야될 경우에는 back quote (\`)를 사용하면 됩니다.

{% highlight xml %}
android:text="@{map[`firstName`}"
{% endhighlight %}

이 방법 외에도 2가지 방법이 더 있지만, 개인적으로 이 방법이 제일 가독성이 좋은 것 같습니다.

## Resource

표현식 내에서 일반 표현식을 통해 resource에 대한 접근도 가능합니다.

{% highlight xml %}
android:padding="@{large? @dimen/largePadding : @dimen/smallPadding}"
{% endhighlight %}

또한 format string이나 plurals도 사용이 가능합니다.

{% highlight xml %}
android:text="@{@string/nameFormat(firstName, lastName)}"
android:text="@{@plurals/banana(bananaCount)}"
{% endhighlight %}

몇몇 리소스들은 일반 접근이 아닌 다른 표현식으로 접근해야합니다.
자세한 내용은 [링크](http://developer.android.com/intl/ko/tools/data-binding/guide.html#resources)를 참고하시길 바랍니다.
