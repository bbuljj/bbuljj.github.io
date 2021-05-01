---
layout: post
title:  "Kotlin의 확장함수"
categories: kotlin
tags: extension function
---

# 코틀린의 확장함수에 대해 알아보자
## 확장함수란?
 - 클래스의 맴버 메소드를 클래스밖에서 호출할 수 있도록 해주는 기능이다.

### 확장함수의 구조
 - 확장함수는 아래와 같이 작성할 수 있다. 자바의 `static` 이라 생각하면 쉬울 것 같다.
{% highlight kotlin %}
class TestClass
fun TestClass.helloWorld() = println("hello world")
{% endhighlight %}

 - 확장함수는 클래스의 맴버메소드는 **오바라이드** 할 수 없다.
{% highlight kotlin %}
class TestClass {
    fun helloWorld() = println("hello world")
}
fun TestClass.helloWorld() = println("world Hello?")

>>> TestClass().helloWorld()
hello world
{% endhighlight %}


