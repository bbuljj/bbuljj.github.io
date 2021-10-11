---
layout: post
title:  "Kotlin Coroutine 기본"
categories: oop
tags: coroutine, async-programming, kotlin-coroutine
---

## 동시성 프로그래밍의 종류
 - Threading
 - Callback
 - Futures, Promise
 - Reactive Programming
 - Coroutine

## 코루틴이란?
### runBlocking
 - 예제

### suspend fun
### scope builder `coroutineScope`
#### differenece runBlocking and coroutineScope
 - runBlocking 메소드는 현재 쓰레드를 블록한다. 
 - 반면 coroutineScope 는 다른 작업을 위해 suspend하고, 주 메소드를 해제한다. 
 - 그런 이유로 runblocking 은 일반 function이고, coroutine scope는 suspend function 이다

#### Scope builder and concurrency﻿
 - coroutineScope Builder는 다중 동시처리 테스크를 수행하기 위해 suspend function 내부에 중첩해서 사용을 할 수 있다. 

```kotlin
fun main() = runBlocking {
  multiWorld()
  println("Done")
}

suspend fun multiWorld() = coroutineScope {
  launch {
    delay(2000L)
    println("second")
  }

  launch {
  	delay(1000L)
    println("first")
  }
  println("init")
}

init
first
second
Done
```

#### variable Job
 - `launch` 코루틴 빌더는 Job을 반환한다.
 - 이 Job은 실행된 코루틴을 핸들링하고, 작업이 완료될 때까지 기다리는데 사용될 수 있다.
 - 예를들면 아래와 같이 하위 코루틴이 완료될 떄까지 기다릴 수 있다. 그리고 나서 Done을 출력한다.

```kotlin
suspend fun explicitJob() = coroutineScope {
    val job = launch {
        delay(1000L)
        println("World")
    }

    println("hello")
    job.join() // 하위 코루틴이 완료될 때까지 기다림.
    println("Done")
}
```