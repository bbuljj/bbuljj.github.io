---
layout: post
title:  "인터페이스"
categories: kotlin
tags: kotlin, class, object, interface
---

## 인터페이스의 선언과 구현
 - 코틀린에서는 인터페이스 선언은 자바와 매우 비슷하다.
{% highlight kotlin %}
interface Foo { 
  fun bar()
}

class FooBar: Foo {
  override fun bar() = println("foo bar")
}

>>> FooBar().bar()
foo bar
{% endhighlight %}

## 특징
 - 자바에서는 `extends`와 `implements`를 사용하지만 코틀린에서는 `:` 콜론을 붙인다.
 - 자바와 마찬가지로 클래스는 인터페이스를 원하는 만큼 구현가능하지만, 클래스는 오직 하나만 확장 가능하다.
 - 자바에서 `@Override`는 optional 하지만 코틀린에서는 반드시 `override`를 사용해야한다.
 - 자바의 `interface default method` 처럼 구현이 가능하다. 
 - 인터페이스는 항상 `open` 이다.
{% highlight kotlin %}
interface Foo {
  fun bar()
  fun defaultBar() = println("Default bar")
}
{% endhighlight %}

## 인터페이스에 선언된 프로퍼티 구현
 - 추상 프로퍼티를 선언에 넣을 수 있다.
{% highlight kotlin %}
interface Status{
  val type: String
}

class MyStatus(val current: String): Status {
  override val type: String
    get() = "$current type"
}

>>> val status = MyStatus("happy")
>>> println(status.type)
{% endhighlight %}