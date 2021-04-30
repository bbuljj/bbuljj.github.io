---
layout: post
title:  "Kotlin 소개"
categories: kotlin
---

# 코틀린 기초
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

#### var 키워드를 사용하면 변수의 값을 변경할 수 있지만 타입은 변경이 불가능하다.
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



## enum
## 프로퍼티 선언방법



```
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].
