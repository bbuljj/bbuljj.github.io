---
layout: post
title:  "Kotlin의 기초"
categories: kotlin
---

# Table Of Contents
  * [함수](#_2)
    + [함수의 구조](#_3)
  * [변수](#_4)
    + [`val` 와 `var`](#val-var)
    + [문자열 템플릿](#_5)
  * [클래스](#_6)
    + [클래스](#_7)
  * [enum, when](#enum-when)
  	+ [enum](#enum)
  	+ [when으로 enum 다루기](#when-enum)
	+ [스마트 캐스트](#_8)
	+ [iteration: while, for loop](#iteration-while-for-loop)
	
---

## 함수
 - 코틀린 `main` 함수는 아래와 같은 형태이다.
```kotlin
fun main(args: Array<String>) {
	println("Hello World")
}
```
 - 코틀린은 함수를 선언할때 `fun` 키워드를 사용한다. 
 - 자바와 다르게 파라미터이름이 먼저오며, 타입은 그뒤에 붙는다.
 - 자바와 다르게 함수를 최상위 수준에 정의할 수 있다. (클레스 안에 넣지 않아도 됨)
 - 줄 끝에 `;` 세미콜론을 붙이지 않아도 된다.

### 함수의 구조
 - 아래는 코틀린의 기본 함수 구조이다.
    ```kotlin
	fun compareInt(a: Int, b: Int) : Int {
		return if (a > b) a else b
	}
    ```
 > 코틀린의 `if` 는 식이다.
 >
 > 식(expressing) 과 문(statement) 의 차이<br/>
 > `expresson`(식)은 값을 만들어내며, 다른 식의 하위로 계산에 참여할 수 있다.<br />
 > `statement`(문)은 값을 만들어내지 않으며, 블록의 최상외 요소로 존재한다.

 - 위의 `compareInt` 함수에서 if 가 식이기 때문에 아래처럼 간결하게 함수를 표현할 수 있다.
 ```kotlin
 fun compareInt(a: Int, b: Int) : Int = if (a > b) a else b
 ```
 - 처음 예제와 같이 본문이 중괄호 `{}` 로 둘러싸인 함수를 **블록이 본문인 함수**라 부르고, 등호와 식으로 이루어진 함수를 **식이 본문인 함수** 라 부른다.

 - **식이 본문인 함수**가 가능한 이유는 반환타입을 적지않아도 컴파일러가 함수 본문식을 분석해 타입을 함수 반환타입으로 정해준다. 이를 **타입추론**이라 한다.


---
## 변수
 - 코틀린의 변수 선언은 타입을 명시하거나 생략해도 된다.
```kotlin
>>> val name: String = "bbuljj"
>>> val name = "bbuljj"
```

### `val` 와 `var`
 - `val` : 변경 불가능한 변수이다. 자바로 따지면 `final` 이다.
 - `var` : 변경 가능한 변수이다.
 - 되도록 `val` 로 선언하여 `sideEffect` 를 최소화하자.
 -  var 키워드를 사용하면 변수의 값을 변경할 수 있지만 타입은 변경이 불가능하다.
```kotlin
>>> var name = "test"
>>> name = 1 // the integer literal does not conform to the expected type String
```

### 문자열 템플릿
 - 자바에서 `String.format(...)` 을 코틀린에서는 쉽고 간단하게 사용 가능하다.
 ```kotlin
 >>> val name = "Hello"
 >>> println("$name, World")
 ```

 - 조건식도 수행 가능하다
```kotlin
>>> val i = 1
>>> println("Hello ${if (i ==1) "World" else "bbuljj"}")
```
```output
hello world
```

---

## 클래스
### 클래스
 - 클래스 선언은 아래와 같다.
 ```kotlin
 class Student {
 	val name: String,
 	var grade: Int
 }
 ```
 - 자바와 다르게 코틀린에서는 `getter`와 `setter`를 따로 구현하지 않아도 된다.
	- `Getter`
	```kotlin
	>>> val student = Student("bbuljj", 10)
	>>> student.name
	>>> println(student.name)
	```
```shell
bbuljj
```
	- `Setter`
	```kotlin
	>>> val student = Student("bbuljj", 10)
	>>> student.grade = 12
	>>> println(student.grade)
	```
	```shell
	12
	```
	- Setter 사용시 프로퍼티가 반드시 `var`(Mutable)만 변경이 가능하다.
- 커스텀 접근자
	- 아래와 같이 프로퍼티의 `getter` 혹은 `setter`를 직접 작성 가능하다.
	```kotlin
	class Party(val name: String, val hour: Int) {
		val isStarted: Boolean
			get() {
				return hour == 10
			}
	}
	```

---

## enum , when
### enum
 - 코틀린 enum 선언은 자바 enum 선언과 조금 다르다
 	- java
 ```java
 public enum Color{
 	...
 }
 ```
 	- kotlin
 ```kotlin
 enum class Color{
 	...
 }
 ```

 - 프로퍼티 메소드가 있는 enum 클래스 선언
 ```kotlin
 enum class Color (var red: Int, val green: Int, val blue: Int) {
 	RED(0,0,0),
 	GREEN(1,1,1),
 	ORANGE(1,2,3); // enum 요소 마지막에 ';' 필수

 	fun getSum() = red + green + blue
 }
 ```

### when 으로 enum 다루기
 - `if`와 마찬가지로 `when`도 식이다. 따라서 식이 본문인 함수에 바로 사용 가능하다.
 ```kotlin
 fun getKoreanByColor(color: Color) =
 	when(color) {
 		Color.RED -> "빨간색",
 		Color.ORANGE -> "주황"
 	}

 >>> println(getKoreanByColor(Color.ORANGE))
 주황
 ```
 - 형태는 자바의 `switch`와 비슷하다. 하지만 자바와 다르게 각 조건마다 `break`를 하지 않아도 된다.
 - 여러 조건이 필요한 경우 `,`로 구분하여 사용 가능하다.
 ```kotlin
 fun getKoreanByColor(color: Color) =
 	when(color) {
 		Color.RED, Color.GREEN -> "빨초",
 		Color.ORANGE -> "주황"
 	}

 >>> println(getKoreanByColor(Color.GREEN))
 빨초
 ```
 - 또한 자바 `switch`에서는 상수만 가능하지만 `when`에서는 임의의 객체를 사용 가능하다.

 - `when`을 인자없이 사용 가능하다.
```kotlin
fun noParamWhen(color: Color, color2: Color) =
 	when {
 		(color == Color.RED && color2 == Color.ORANGE) -> "빨주"
 		(color == Color.GREEN && color2 == Color.ORANGE) -> "초주"
		else -> throw Exception("not found")
	}
```
 	- 인자 없이 사용하게되면 불필요한 객체 생성을 하지 않아 성능 향상에 도움이 된다.

### 스마트 캐스트
 - 코틀린에서는 해당 타입의 검사와 동시에 캐스팅이 가능하다.
 ```kotlin
 val s = "test"
 if (s is String) {
 	println(s.length)
 }
 ```

 - `when` 을 이용한 스마트 캐스트
```kotlin
interface Fruit
class Apple : Fruit
class Banana : Fruit

fun getTextFruit(e: Fruit): String =
	when (e) {
	    is Apple -> "사과"
	    is Banana -> "바나나 "
	    else -> throw IllegalArgumentException("not found")
}

>>> val smartCast = SmartCast()
>>> println(smartCast.getTextFruit(Apple()))
사과
```
 - `when` 스마트 캐스트 시 블록을 사용할 수도 있다.
```kotlin
 when (e) {
 	is Apple -> {
 		println("e 는 사과임돠")
 		"apple"
 	}
	is Banana -> {
		println("e 는 사과임돠")
 		"apple"
	}
	else -> throw IllegalArgumentException("not found")

>>> println(smartCast.getTextFruit(Apple()))
사과임돠
apple
```

---

## iteration: while, for loop
### 숫자 interation 
 - 숫자 iteration은 아래와 같이 간단하게 생성할 수 있다.
 ```kotlin
 >>> val numbers = 1..10 // 1 부터 10까지의 수열을 생성한다.
 ```
 - 자바와 다르게 코틀린에서는 `forEach` 를 짧게 구현 가능하다. *개인적으로 매우 맘에 든다.*
 ```kotlin
 for (i in 1..5) {
 	println(i)
 }
 ```
 - 코틀린에서는 `증가값`과 `역순`에 대해서도 쉽게 구현가능하다.
 ```kotlin
 for (i in 50 downTo 1 step 2) {
        println(i)
 }
 ```
```
20
18
16
14
12
10
8
6
4
2
```

### Collection iteration
 - map의 key, value 형태를 쉽게 사용할 수 있다.
```kotlin
val map = mapOf("x" to 1, "y" to 2)
for ((key, value) in map ) {
    println("$key : $value")
}
```
```
x : 1
y : 2
```
 - list를 iteration 할 때 `index` 를 쉽게 포함시킬 수 있다.
```kotlin
for ((index, element) in arrayListOf("1", "2", "3").withIndex()) {
	...
}
```
 - `.withIndex()` 는 ***확장함수*** 인데 아주 유용합니다. 나중에 관련 포스팅을 하도록 하겠습니다.

 - `in` 을 통해 iteration 에 조건이 포함되는 지 확인할 수 있다. (반대는 `!in`)

{% highlight kotlin %}
val lists = listOf("kotlin", "java")
println("kotlin" in lists)
true
{% endhighlight %}
