---
layout: post
title:  "코틀린의 타입시스템 [1]"
categories: kotlin
tags: kotlin, null, type, NPE
---

## 타입시스템 
### 널이 될 수 있는 타입
 - 코틀린은 `null` 이 될 수 있는지 여부를 타입시스템에 추가하고 컴파일러가 컴파일 시 미리 감지하여 예외의 가능성을 줄일 수 있게 했다.
 - 코틀린 타입 시스템에서는 널이 될 수 있는 타입을 *명시적*으로 지원한다.
  - 이로 인해서 널에 대한 많은 오류들을 사전에 방지할 수 있는 큰 이점이 있다.

{% highlight kotlin %}
fun stringToInt(param: String) : Int {
    return param.toInt()
}

>>> stringToInt(null) // Kotlin: Null can not be a value of a non-null type String
{% endhighlight %}
 - 위 예제에서 `param` 에 `null` 을 넣고 실행하면 **(컴파일조차 되지 않는다)** `Kotlin: Null can not be a value of a non-null type String` 오류가 발생한다.
 - 코틀린에서는 `null` 이거나 `null`이 될 수 있는 인자를 넘기는 것은 금지 되어 컴파일 오류가 발생하는 것이다.

{% highlight kotlin %}
fun stringToInt(param: String?) : Int {}
{% endhighlight %}
 - 위 처럼 `?` 를 타입 뒤에 넣으면 널이 될 수 있는 타입이라고 명시를 해줄 수 있다.
 - 이처럼 코틀린의 타입들은 기본적으로 `null`이 될 수 없으며, 타입시스템을 통해 좀 더 견고한 프로그래밍을 작성할 수 있게 도움을 준다.

### 안전한 호출 연산자 [*?.*]
 - `?.` 연산자는 `null`검사와 메소드 호출을 한번의 연산으로 수행할 수 있게 해준다.
{% highlight kotlin %}
>>> param?.toInt()
>>> if (param != null) param.toInt() else null
{% endhighlight %}
 - 위의 두 코드는 서로 같은 기능을 하는 코드다. 확실히 먼저 선언한 `param?.toInt()` 가 가독성도 뛰어나고 간결하다.

#### 안전한 호출 연산자의 연쇄
 - method Chaning처럼 `?.`연산자도 Chaning이 가능하다.

{% highlight kotlin %}
class One(val two:Two?)
class Two(val three: Three?)
class Three() {
  fun last() {
    println("last!")
  }
}

fun One.testMethod() {
  this.two?.three?.last()
}
{% endhighlight %}

### 엘비스 연산자 [*?:*]
 - 엘비스 연산자는 `null` 일 경우 디폴트 값을 지정할 때 편리하게 사용할 수 있는 연산자다.

{% highlight kotlin %}
fun getText(str: String?) = str ?: "default"
>>> println(getText(null))
{% endhighlight %}
 - 위 예제처럼 엘비스 연산자(`?:`)로 디폴트 값을 지정하여 `null`에 대해 유연하게 대처할 수 있다.

#### 엘비스 연산과 throw
 - 코틀린에서는 `throw`도 `식(expression)`이기 때문에 엘비스 연산의 디폴트 값으로 사용 가능하다.

{% highlight kotlin %}
fun elvisAndThrow(str: String?) = str?: throw IllegalArgumentException("invalid argument!!")

>>> println(elvisAndThrow()) // Exception in thread "main" java.lang.IllegalArgumentException: invalid argument!!
{% endhighlight %}

### 널이 아님에 대한 단언 [*!!*]
 - 코틀린에서는 널이 될 수 없다고 단언할 수 있다. 이를 적용하면 `null` 이 들어왔을 때 `NPE`가 발생한다.

{% highlight kotlin %}
fun noNull(str: String?) {
	val notNull = str!!
	println(notNull)
}

>>> noNull(null) // Exception in thread "main" java.lang.NullPointerException
{% endhighlight %}

### let 함수
 - `let` 함수를 사용해서 `null` 에 대한 체크를 할 수 있다.
   - `let` 함수는 널일 경우 아무 작동도 하지 않고, `null`이 아닐 경우 함수블럭을 실행한다.
   - 하지만 `let`은 수신객체가 `null`인지 알 수 없기 때문에(타입 추론이 되지 않음) 반드시 `?.` 를 사용해야 한다.

{% highlight kotlin %}
fun showLengthOrNothing(str: String?) {
  str?.let {
    println(it.length)
  }
}

>>> showLengthOrNothing("test") // 4
>>> showLengthOrNothing(null) // 아무 작동도 하지 않음.
{% endhighlight %}

 - 위 예제처럼 `let`함수를 사용하면 수신 객체를 람다로 처리할 수 있다. 
