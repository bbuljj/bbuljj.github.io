---
layout: post
title:  "with, apply"
categories: kotlin
tags: kotlin, with, apply
---

## 코틀린의 표준 라이브러리 *with* 와 *apply*
 - 이 두 함수는 수식객체 지정 람다라고 불리며, 매우 유용하게 쓰인다.

### with
 - 어떤 객체의 이름을 반복하지 않고, 다양한 연산을 수행가능하게 해준다.

{% highlight kotlin %}
fun getAppleAndFrom1To10(): String {
  return with(StringBuilder()) {
    this.append("a")
    this.append("p")
    this.append("p")
    this.append("l")
    this.append("e")
    
    for (num in 1 until 10) {
      this.append(num)
    }
    
    this.toString()
  }
}

>>> println(getAppleAndFrom1To10())
apple123456789
{% endhighlight %}
- 위 예제처럼 `with()` 안에 수신객체를 지정해서 그 객체를 블록안에서 `this` 혹은 수신객체의 함수를 바로 호출할 수 있다.(`this`를 붙이지 않아도 된다.)


{% highlight kotlin %}
fun getAppleAndFrom1To10() = with(StringBuilder()) {
    append("a")
    append("p")
    append("p")
    append("l")
    append("e")
  
    for (num in 1 until 10) {
      append(num)
    }
  
  toString()
}
{% endhighlight %}

- 또한 위 예제처럼 `with`를 식으로 사용 가능하다.

--- 

### apply
 - `apply`는 `with`와 같지만 차이점은 항상 자신에게 전달된 객체를 리턴한다.
{% highlight kotlin %}
fun getAppleString() = StringBuilder().apply {  // StringBuilder() 수신객체 
  append("a")
  append("p")
  append("p")
  append("l")
  append("e")
} // 리턴 StringBuilder

>>> val appleSb = getAppleString()
>>> appleSb.toString() // 전달된 객체를 리턴하기 때문에 StringBuilder()를 리턴함.
apple
{% endhighlight %} 
 - 위 예제처럼 전달된 객체를 리턴하여 `StringBuilder.toString()` 해서 결과를 얻을 수 있다.


