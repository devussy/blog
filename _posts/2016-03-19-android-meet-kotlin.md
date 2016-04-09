---
layout: post
title:  Kotlin과 Android와의 만남
date:   2016-03-19
categories: Android
---

Kotlin은 JetBrains에서 2011년도에 만든 새로운 프로그래밍 언어입니다. Android를 개발하시는 분들은 대부분 사용하시는AndroidStudio가 JetBrains에서 만든 IntelliJ IDEA기반으로 동작하고 있죠.

그런데 최근에(최근이라고 하기엔 훨씬 전부터) Kotlin이 Android개발 언어로서 대두되고 있습니다. 그 이유로 여러가지가 있는데, 저로서는 **해소 되지 않을 것만 같은 Java의 묵직함**, 그리고 **Android와 Java와의 서먹함** 이 그 이유라고 판단됩니다.

사실 Java는 계속 발전하고 있습니다. Java 8에서는 lambda expressions같은 멋진 것들을 추가해나가고 있습니다(기존 자바 문법에 익숙한 사람들에게는 또 다른 귀찮음이겠지만). 하지만 문제는 Android에서는 Java 8을 지원하지 않습니다. 즉, 발전하고 있는 Java의 장점을 그림의 떡 보듯 봐야한다는 것이죠.

게다가 Oracle과 Google과의 소송전 때문에 Google이 Java를 버리..진 못하고(이미 JAVA로 만든 API들의 양이 어마어마하니까) 결국에 Android N부터는 OpenJDK에 의존하게 될 것 같습니다.

이러던 중에 안드로이드 오픈소스의 거장 Jake Wharton이 Twitter에 Kotlin이라는 언어를 소개하게 됩니다. 아마 누군가는 이미 Kotlin에 대해서 알고 있었겠지만, 저는 Jake Wharton의 이 트윗 덕분에 Kotlin이 많이 대중화 됐다고 생각합니다.

Kotlin은 Android의 개발 언어로 사용하기에 많은 장점을 가지고 있습니다.
1. JVM기반의 언어이다. 즉 Java랑 동일한 환경에서 프로그램을 돌릴 수 있다.
2. **Java로 작성된 기존 Android와의 호환성이 다른 JVM언어보다 높다.** 예전에 Scala가 한번 Android 대체 개발 언어로 잠깐 뜬 적이 있었는데, 기존 Android 코드를 Scala코드에서 호출하는 것이 매우 불편하게 되어있어서 잠잠해졌었죠. Kotlin은 언어 자체의 핵심을 Java와의 상호 호환성에 두고 있어서 Kotlin코드 내에서 쉽게 Java 메서드를 호출 할 수 있습니다.
3. 매력적인 Lambdas식을 사용할 수 있습니다.
4. 기존에 안드로이드 개발자들은 다들 알고 계신 공포의 Null Point Exception으로부터 자유롭습니다.
5. 메서드 Extension이 가능합니다.

여기까진 Kotlin에 관심 있으신 분들이라면 대부분 아실 내용이고, 위에서 언급한대로 Kotlin을 만든 회사가 JetBrains인 덕분에, AndroidStudio에서 너무나 자연스럽게 Kotlin을 서포트 해줍니다. 덕분에 여러가지 강점이 추가되는데

6. **Kotlin관련 Plugin만 설치하면 AndroidStudio에서 바로 사용이 가능합니다.**
7. **AndroidStudio에서 제가 정말 잘 쓰고 있는 기능 중 하나인 `auto-suggested`기능이 Kotlin에도 적용됩니다.**
8. **기존에 작성한 Java코드를 Kotlin으로 자동 변환해주는 기능을 제공합니다.**

본의 아니게 Kotlin을 엄청 멋진 언어로 소개한 것 같은데, 아직까지 Kotlin이 Android 프로젝트의 전체를 담당할 수 있을 정도로 완전한 언어는 아니라고 판단됩니다. 직접 사용해보신 분들이 그렇게 이야기들 하시네요. 하지만 JetBrains에서 열심히 밀고 있는 언어라는 점, 그리고 빠른 속도로 발전하고 있다는 점 등을 미루어 보았을 때, **안드로이드의 대체 언어로서 한번 쯤은 살펴봐도 좋지 않을까** 하는 것이 제 생각입니다.

저도 실제 프로젝트에서 UI단이 아닌 Model단에서 조금씩 도입해볼 생각입니다. 혹시나 시간이 된다면 제가 공부한 내용을 조금씩 정리해서 공유하도록 하겠습니다.
