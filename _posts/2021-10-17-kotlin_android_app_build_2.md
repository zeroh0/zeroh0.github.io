---
title: "코틀린을 활용한 안드로이드 앱 개발_2차시"
excerpt: "코틀린을 배워보자"

categories:
    - kotlin
tags:
    - [kotlin]

toc: true
toc_sticky: true

date: 2021-10-17
last_modified_at: 2021-11-10
---

```kotlin
// 탑레벨
val data1: Int = 10
var data2: Int = 10

class Test1_variable {
    // 클래스의 멤버 변수를 초기화하지 않고 사용할 경우 에러 발생
    // 클래스 멤버 변수
    var data4: Int = 10
    val data3: Int = 10

    fun someFun() {
        //함수 레벨 로컬 변수
        val data5: Int
        var data6: Int

        data5 = 10
        data6 = 10

        val result = data5 + data6;
    }
}
```

<br>

```kotlin
val data1 = 10
val data2: Double = data1.toDouble() //데이터 타입의 캐스팅 : Int와 Double은 상하위 관계에 있지 않기 때문에
val data3: Int = data2.toInt()

val data4: String = data1.toString()
val data5: Int = data4.toInt()
```

<br>

```kotlin
fun main() {
    val array1 = arrayOf(10, 20 ,true, "hello")
    array1[2] = false
    array1.get(1)

    val array2 = arrayOf<Int>(10, 20)
    val array3 = intArrayOf(10, 20)

    Array(3, {i -> i*10})

    val array4 = arrayOfNulls<Int>(4)

    val array5 = Array(3, {0})
    val array6 = IntArray(2)
}
```