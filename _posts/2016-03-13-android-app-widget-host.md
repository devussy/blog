---
layout: post
title:  App Widget Host(번역)
date:   2016-03-13
categories: Android
---
대부분의 Android device 들의 Home screen은 사용자들이 App Widget 을 통해 Content 에 빠르게 접근할 수 있도록 한다. 만약에 사용자들에게 개발하고자 하는 App의 특성에 맞춰 최적화된 Widget 을 제공하고자 한다면 AppWidgetHost를 implement 함으로써 구현할 수 있다.

- App Widget Host: AppWidget 와 상호작용작용 할 수 있도록 도와준다. `AppWidgetHost` 는 반드시 package내에서 unique한 ID를 가져야 한다.
- App Widget ID: 각 Widget의 instance는 binding되는 시점에 unique ID가 할당된다 `bindAppWidgetIdIfAllowed()`. unique ID는 `allocateAppWidgetId()`를 통해 얻을 수 있다. 할당된 ID는 Widget의 lifetime동안(Host로부터 삭제될 때 까지) 유지된다. Size 조절이나 Widget의 위치 변경 등의 Host-Specific state는 할당된 widget ID를 통해 접근한다.
- App Widget Host View: `AppWidgetHostView`는 widget의 Container 개념이라 생각하면 될 것 같다. App Widget은 inflated 될 때 마다 `AppWidgetHostView`에 할당된다.
- Options Bundle: `AppWidgetHost`는 options bundle을 사용하여 `AppWidgetProvider`와 widget이 어떻게 보여지는지에 대한 정보(size 범위, lockscreen에 있는지 homescreen에 있는지 등)를 주고 받는다. 이러한 정보들을 기반으로 `AppWidgetProvider`가 widget의 모습을 조절할 수 있다. `updateAppWidgetOptions()`와 `updateAppWidgetSize()`를 사용하여 app widget의 bundle을 수정하고, `AppWidgetProvider` 내의 Callback method들을 호출한다.


##Binding App Widget
*Android 4.1 이상 기준으로 작성됨*
우선 Manifest파일에 `BIND_APPWIDGET`을 선언해줘야 한다.

{% highlight xml %}
<uses-permission android:name=“android.permission.BIND_APPWIDGET” />
{% endhighlight %}

Widget을 host에 추가하기 위해서는 런타임에 사용자가 App 에게 권한을 부여해 줘야만 한다. App 이 Widget 추가에 대한 권한을 가지고 있는지 체크하려면 `bindAppWidgetIdIfAllowed()` 메소드를 사용해야 한다.

{% highlight java %}
Intent intent = new Intent(AppWidgetManager.ACTION_APPWIDGET_BIND);

intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, appWidgetId);

intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_PROVIDER, info.componentName);

// 여기가 위에서 말한 options bundle 입니다.
intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_OPTIONS, options);

startActivityForResut(intent, REQUEST_BIND_APPWIDGET);
{% endhighlight %}

혹시나 App Widget 설정이 필요하다면, Host에서 체크해주는데, App Widget 설정 Activity에 관련해서는 [Creating an App Widget Configuration Activity](http://developer.android.com/intl/ko/guide/topics/appwidgets/index.html#Configuring)을 참고.
