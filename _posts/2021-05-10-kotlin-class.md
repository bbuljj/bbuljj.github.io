---
layout: post
title:  "클래스"
categories: kotlin
tags: kotlin, class, object, interface
---

## open, final, abstract
### open, final
 - `open`은 상속을 허용한다는 키워드이며, `final`은 상속을 허용하지 않는다.
 - 코틀린에서 클래스는 자바와 다르게 기본적으로 `final` 이다
 - 어떤 클래스가 자신을 상속하는 방법(어떤 메소드를 어떻게 오버라이드해야하는지) 을 제공하지 않으면 클래스를 작성한 개발자가 의도하고 생각한 것과 다르게 메소드를 오버라이드할 위험이 있다. 이 때문에 정확하게 **의도하고 설계되어** 오버라이드하게 만든 클래스와 메소드가 아니라면 모두 `final`로 막아야 한다.
 - 만약 상속을 허용하려면 `open` 변경자를 붙여야 한다.

{% highlight kotlin %}
class HellWorld {
  open fun world() {}
}

class Test: HelloWorld() { // 상속 허용
  fun hello() {} // 기본적으로. final 이므로 오버라이드 불가능
  open fun world() {} // 메소드 오버라이드 허용
}

{% endhighlight %}

 - `override` 는 기본적으로 열려있다. 때문에 오버라이드를 막고싶다면 final 키워드를 붙여야한다.

{% highlight kotlin %}
class Test: HelloWorld() {
 final override fun world() {} // final을 사용하여 오버라이드 막기
}
{% endhighlight %}

### abstract
 - 추상클래스는 인스턴스화할 수 없다.
 - 기본적으로 구현이 없는 추상 맴버이기 때문에 하위 클래스에서 추상맴버를 오버라이드 해야한다.
 - 추상 맴버는 항상 열려있다. 때문에 open 을 명시할 필요가 없다.
{% highlight kotlin %}
abstract class UserAction {
  abtract fun register()
}
{% endhighlight %}


## sealed Class (봉인 클래스)
 - 클래스 계층의 확장을 제한할 때 사용된다.(하위 클래스의 종류를 제한)
 - `abstract class` 이므로 객체화할 수 없다.
 - `sealed class` 와 상속받은 하위 클래스들은 같은 파일에 위치해야한다.
 - 아래 처럼 하위 클래스를 `object`로 선언할 수 있습니다.(`object` 선언 시 `singleton` 이 됩니다.)

{% highlight kotlin %}
sealed class Marine

object Move: Marine
object Jump: Marine
object Stop: Marine
{% endhighlight %}

 - 또는 `sealed` 내부에 선언이 가능합니다.
{% highlight kotlin %}
sealed class Marine {
 class Move(val step: Int) :Marine()
 class Jump(val step: Int): Marine()
}
{% endhighlight %}

 - `sealed`를 `when` 과 함께 사용하면 이점이 있습니다.
 - `when` 은 기본적으로 값이 없을 경우에 대한 `else` 구문을 명시해야 하는데 `sealed`를 사용하게 되면 값이 없을 경우가 없기 때문에 `else`를 선언하지 않아도 되며, 컴파일 단계에서 잡을 수 있어 실수할 가능성이 없어집니다.

{% highlight kotlin %}
 fun play(marine: Marine): Int =
 when(marine) {
  is Marine.Move -> marine.step
  is Marine.Jump -> marine.step
 }
{% endhighlight %}

{% highlight kotlin %}
fun play(marine: Marine): Int =
  when(marine) {
    is Marine.Move -> marine.step
  }

// Compile Error : 'when' expression must be exhaustive, add necessary 'is Jump' branch or 'else' branch instead
{% endhighlight %}

# 접근 제어자
 - 코틀린의 접근제어자는 자바와 조금 다르다.

|변경자|클래스맴버|
|------|---|
|public|모든 클래스/모듈에서 볼수 있음|
|internal|같은 모듈에서만 볼 수 있음|
|protected|하위 클래스에서만 볼 수 있음|
|private|같은 클래스에서만 볼 수 있음|

> 자바 `protected`에서는 상속한 `하위 클래스`와 `현재의 패키지에 있는 곳`에서 볼 수 있지만<br/>
> 코틀린에서는 상속한 `하위클래스만` 볼 수 있다.

# data class
 - 자바에서 `equals, hashCode(), toString`을 모두 정의해줘야한다. 혹은 `lombok` 어노테이션을 명시해야한다.
 - 코틀린에서는 `data class`를 정의하면 `equals, hashCode(), toString` 가 자동적으로 구현된다.

# object
## 싱글턴 객체 선언 (object)
 - 객체 선언이 싱글턴으로 정의된다.
{% highlight kotlin %}
 object Person {
  val people = listOf("철수", "영희")

  fun printPeople() {
    for (p in people) {
    	println(p)
    }
  }
 }

 >>> Person.printPeople()
 철수
 영희
{% endhighlight %}

### 동반 객체 선언 (companion)
 - 코틀린에서는 기본적으로 static을 지원하지 않는다. 이를 코틀린에서 사용하려면 `최상위 함수`를 사용하거나 `객체` 선언을 사용할 수 있다. 대부분의 경우 최상위 함수를 더 권장한다.
 - 하지만 최상위 함수는 함수내부에 private 비공개 맴버에 접근할 수 없는 단점이 있다. 이때 사용하는 것이 **동반객체(compainon)** 이다.
 - 사용법은 클래스 안에 정의된 `object` 에 `companion` 을 붙이면 된다.
{% highlight kotlin %}
  class Apple {
   companion object {
    fun taste() {
  	 println("sweet!!")
  	 }
  	}
  }

 >>> Apple.taste()
 sweet!!
{% endhighlight %}
 - 동반객체는 모든 private 맴버에 접근할 수 있다. 따라서 바깥쪽 클래스의 private 생성자도 호출할 수 있다. 
 - 동반객체는 위의 이유로 팩토리 패턴을 구현하기 가장 적합한 것 같다.

{% highlight kotlin %}
 class Device {
   val type: String
   constructor(version: Int) {
     type = "IOS version $version"
   }

   constructor(version: String) {
     type = "ANDROID $version"
   }
 }

 class CompanionDevice private constructor(val type: String) {
   companion object {
     fun getIOS(version: String) = CompanionDevice("IOS $version")
     fun getAOS(version: Int) = CompanionDevice("AOS $version")
   }
 }

 >>> val aos = CompanionDevice.getAOS(1)
 >>> val ios = CompanionDevice.getIOS("m1")
 >>> println(aos.type)
 >>> println(ios.type)

 AOS 1
 IOS m1
{% endhighlight %}
#### 동반객체를 일반 객체처럼 사용
{% highlight kotlin %}
class Converter {
  companion object StringToIntConverter {
    fun toInt(type: String) = type.toInt()
  }
}

>>> println(Converter.StringToIntConverter.toInt("123"))
123

{% endhighlight %}

#### 동반객체 인터페이스 구현
{% highlight kotlin %}
interface ConvertInterface {
  fun toNum(type: String): Int
}

class Converter {
  companion object StringToIntConverter : ConvertInterface{
    override fun toNum(type: String) = type.toInt()
  }
}


{% endhighlight %}