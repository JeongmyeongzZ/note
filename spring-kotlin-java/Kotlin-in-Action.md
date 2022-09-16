# Kotlin 1.3 대응 Kotlin In Action 

--- 

## 코틀린이란 ?

자바 플랫폼에서 돌아가는 새로운 프로그래밍 언어. 간결하고 실용적이며 자바코드와 상호 운용성을 중시한다. 

이 말은 기존의 자바기반 라이브러리나 프레임워크와 함께 잘 작동하며, 자바로 개발중인 곳이라면 코틀린을 사용할 수 있다는 의미이기도 하다.

---

## 1. 특성

* 정적 타입 지정 언어
  * Groovy, JRuby 같은 동적 타입 지정언어와는 다르게, 컴파일 시점에 모든 프로그램 구성 요소의 타입을 알 수 있고, 필드나 메서드를 사용할때 마다 컴파일러가 타입을 검증해주는 자바와 마찬가지인 정적 타입 지정 언어이다.
  * 코틀린에서는 자바와 달리 모든 변수의 타입을 직접 명시할 필요가 없다. 이는 타입 추론이라는 기능을 포함하기 때문이다. (var x = 1 을 보고 x 가 Int 임을 자동으로 알아내는 기능)
  * 정적 타입 지정 언어는 신뢰성 면에서도 좋고, 객체의 타입이 정해져있기에 유지보수성이나 성능 면에서도 장점이 있다.
  
* 함수형 프로그래밍, 객체지향 프로그래밍 
  * 불변성, 일급 시민 함수(일급 객체), 적은 사이드이펙트 등 많은 장점을 가지고 있는 함수형 프로그래밍
  * 그리고 자바 개발자에게 익숙한 객체지향 프로그래밍의 장점까지

* 실용성, 간결성, 안전성, 상호운용성
  * nullable 임을 `?`를 통해 표시할 수 있음. (PHP 개발을 해온 나에게는 익숙..) => NPE 방지

---

## 2. 문법

val => 변경 불가능한 참조를 저장하는 변수. 초기화 이후에 변경 불가능 (final 변수)
var => 변경 가능한 참조. 초기화 이후에 재할당은 가능하지만 타입이 변하면 type mismatch 오류가 발생한다.

문자열 탬플릿 (PHP 의 string literals 와 유사) => "Hello $someWorld" Or "Hello ${args[0]}" 과 같이 산식등도 넣을 수 있다.

클래스 생성 시 new 키워드를 생략할 수 있으며, public property 에 대한 getter 는 알아서 제공한다. 또한 getName() 과 같이 함수형태가 아닌 class.name 등과같이 간단하게 작성할 수 있다. (보일러 플레이트 코드가 상당히 줄어들 듯)

enum 은 자바와 달리 enum class Color 와 같이 class 키워드를 필수로 사용해야하며, 상수를 정의할 때 마지막 세미콜론이 유일하게 필수이다. (코틀린에서 유일하게 자바보다 코드를 많이 작성해야하는 게 enum 이라고 말한다고 한다)

자바와 달리 체크예외와 언체크예외가 자유롭다. 사실 최근 JVM 언어들은 이를 잘 구분하지 않는다고한다. 예외를 지정하고, 매번 잡아내고.. 예외처리를 강제화 하지 않는다는 것이다.

---

## 3. 함수 정의와 호출

```kotlin
val a = listOf(1, 2, 3);

println(a);
```

와 같은 코드가 있다. 자바 컬렉션에는 디폴트 toString 구현이 들어있다. 하지만 그 디폴트 toString 출력 형식은 고정돼 있고 우리에게 필요한 형식이 아닐 수 있다.
`[1, 2, 3]` 과 같이 출력되는게 아니라, `(1; 2; 3)` 처럼 노출시킬 순 없을까?

```kotlin
fun <T> Collection<T>.joinToString(separator: String = ",",
                                   prefix: String = "(",
                                   postfix: String = ")"
): String {

//사용
val list = listOf("1","2","3")
list.joinToString()
```

named parameter 와, 제네릭을 사용해 위와 같은 유연한 함수를 직접 만들 수 있다.

확장 함수는 static 메소드이기 때문에 오버라이드를 할 수 없습니다.
그리고 확장 함수는 클래스 밖에 선언되기 때문에 오버라이드를 할 수 없습니다.

```kotlin

StringJoin.kt //StringJoin.kt 파일에 정의된 확장함수

package com.zerogdev.mykotlin.function.other
fun String.lastChar():Char = this.get(this.length - 1)


ExtendFunction.kt //ExtendFunction.kt 파일에 정의된 확장함수

package com.zerogdev.mykotlin.function.extend
fun String.lastChar():Char = this.get(this.length - 1)

// =>

import com.zerogdev.mykotlin.function.other.lastChar
import com.zerogdev.mykotlin.function.extend.lastChar as last //last 로 이름을 변경

//확장 함수 사용
"zerog".lastChar() //other 패키지에 있는 lastChar() 사용
"zerog".last()       //extend 패키지에 있는 lastChar() 사용

```

또한 프로퍼티도 확장할 수 있습니다.
다만, 확장 프로퍼티는 상태를 저장할 수 없기때문에 초기화 할 수 없고 get() 을 구현 해야됩니다.

그리고 List 처럼 상태를 저장할 수 있는 클래스인경우 get()과 set()을 추가할 수 있습니다.
```kotlin

//get() 구현
val String.lastChar: Char
get() = get(length - 1)

//리스트인경우 get(), set() 구현
var List.lastChar: String
get() { return last()}
set(value) {
    var lastChar: String = last()
    lastChar = value
}
```

---

## 4. 클래스, 객체, 인터페이스

```kotlin
interface SomeInterface {
    fun test() = println("test..")
}

class TestClass: SomeInterface {
  override fun test() {
    super.test()

    println("something more..")
  }
}
```

인터페이스안에 디폴트 구현을 추가할 수 있다. 메소드 시그니처 뒤에 단순히 입력만 하면 된다. 자바에선 SomeInterface.super.test() 와 같이 super 앞에 기반타입을 적지만, 코틀린은 super<SomeInterface>.test 와 같이 이름을 지정한다.

코틀린에선 기본적으로 Final 이다. 취약한 기반클래스라는 문제가 발생하지 않기 위해, 이펙티브 자바에서는 상속을 위한 설계와 문서를 갖추거나, 그럴 수 없으면 상속을 금지하라. (모두 final 로 만들어라.) 라는 말이 있다.

코틀린도 이 철학을 동일하게 가져간다. 상속을 허용하려면 앞에 open 변경자를 붙여야 한다. 오버라이드를 허용하고 싶은 메서드나 프로퍼티 앞에도 동일하다.

추상(abstract) 클래스 및 메서드는 반드시 오버라이드 해야한다는 점에서 동일하다.

이번엔 가시성에 대해 알아보자. 코틀린 가시성 변경자는 자바와 비슷하다. 자바와 기본 가시성은 다르다. 아무 변경자도 없는 경우 모두 public 하다. *자바의 기본 가시성인 package-private 은 코틀린에 없다.*
_이 대안으론 internal 이라는 새로운 가시성 변경자가 도입됐다_

자바에서는 같은 패키지 안에서 protected 멤버에 접근 할 수 있다. (이건 몰랐네...) 코틀린에서는 그렇지 않다는 점에 유의하자. 상속한 클래스 안에서만 보인다. 클래스를 확장한 함수는 그 클래스의 private, protected 멤버에 접근 할 수 없다는 점은 다시 한 번 짚고 넘어가자.

sealed class 는 보통 아래와 같이 생성됩니다. 그리고 이와 같이 사용할 수 있습니다. 

`val color : Color = Red`

> object 클래스로 정의하면 singleton 패턴이 적용되어 single instance로 생성됩니다.

```kotlin
sealed class Color {
    data class Red(val r: Int, val g: Int, val b: Int) : Color()
    data class Orange(val r: Int, val g: Int, val b: Int) : Color()
    data class Yellow(val r: Int, val g: Int, val b: Int) : Color()
    data class Green(val r: Int, val g: Int, val b: Int) : Color()
    data class Blue(val r: Int, val g: Int, val b: Int) : Color()
    data class Indigo(val r: Int, val g: Int, val b: Int) : Color()
    data class Violet(val r: Int, val g: Int, val b: Int) : Color()
}

// 중첩 클래스로 정의하는 방법

sealed class Color {
  object Red: Color()
  object Green: Color()
  object Blue: Color()
}
```

```kotlin
fun eval(e: TestSealed) {
    when (e) {
        is TestSealed.Red -> println("test !")
        is TestSealed.Blue -> println("test !")
        is TestSealed.Green -> println("test !")
    }
}
```

else 가 없어도 sealed class 는 오류가 발생하지 않는다.
Sealed class는 Super class를 상속받는 Child 클래스의 종류 제한하는 특성을 갖고 있기 때문이다.

어떤 클래스를 상속받는 하위 클래스는 여러 파일에 존재할 수 있기 때문에 컴파일러는 얼마나 많은 하위 클래스들이 있는지 알지 못합니다. 하지만 Sealed class는 동일 파일에 정의된 하위 클래스 외에 다른 하위 클래스는 존재하지 않는다는 것을 컴파일러에게 알려주는 것과 같습니다.

이렇게 하위 클래스가 될 수 있는 클래스를 제한하여 얻을 수 있는 장점 중 하나는 when을 사용할 때 else를 사용하지 않는 것입니다.

이것은 코틀린에서 제공하는 Enum으로도 얻을 수 있는 이점입니다. 하지만 Enum은 Red라는 하위 객체를 Singleton처럼 1개만 생성할 수 있고 복수의 객체는 생성할 수는 없습니다. 반면에 Sealed class는 1개 이상의 객체를 생성할 수 있습니다.



코틀린에는 여러 생성자가 존재한다.

```kotlin
// 선언과 초기화를 한번에
class User(val nickname: String)
```

이렇게 클래스 이름 뒤에 오는 괄호로 둘러싸인 코드를 주 생성자(primary constructor)라고 부른다. 주 생성자는 생성자 파라미터를 지정하고 그 생성자 파라미터에 의해 초기화되는 프로퍼티를 정의하는 두 가지 목적에 쓰인다.
실제로는 아래와 같은 일이 벌어진다.

```kotlin
// 선언과 초기화를 각자 constructor, init 에서 한다.
class User constructor(_nickname:String){ // 파라미터가 하나만 있는 주 생성자
  val nickname: String
  init {
    nickname = _nickname
  }
}
```

그리고 named parameter 를 지원하며, 기반 클래스 (final 이기에 상속을 허용하는 open 키워드가 붙어있다.) 는 아래와 같이 생성한다.

```kotlin
open class User(val nickname: String) { ... } 
class TwitterUser(nickname: String): User(nickname) { ... }

// 아무 인자를 필요로 하지 않더라도 생성자를 호출해야한다.
open class User() { ... }
class TwitterUser(nickname: String): User() { ... }
```

데이터 클래스는 toString, equals, hashcode 메소드를 모두 포함하고 있다. 
데이터 클래스는 프로퍼티가 꼭 val 일 필요는 없다. 그러나 immutable 하게 만들라고 권장한다. 해시맵등의 컨테이너에 데이터 클래스 객체르르 담는 경우엔 불변성이 필수적이다. (hashcode 는 해시맵같은 해시기반 컨테이너의 키로 사용된다)

copy 메소드는 객체를 복사하며 일부 프로퍼티를 바꿀 수 있게 한다. 객체를 메모리상에서 직접 바꾸는 것 보다 복사본을 만드는게 훨씬 낫다라는 것이다.

---

코틀린엔 static 이 존재하지 않는다. 최상위 함수로 대부분 처리할 수 있기 때문이다.
그러나 최상위 함수는 당연히 어떤 객채의 Private 한 값엔 접근 할 수 없다.
이럴 때 object 를 고려한다. <컴파일시 java 의 static 으로 변환 됨>

코틀린은 object 키워드를 이용해서 별다른 코드없이 싱글턴 구현을 지원합니다.

```kotlin

object Payroll { //object 클래스 생성으로 싱글턴을 구현
    val allEmplyees = arrayListOf<Person>()

    fun calculateSalary() {
        //...
    }
}


//사용
Payroll.calculateSalary() //싱글턴처럼 . 으로 접근
Payroll.allEmplyees.add(Person("zerog", Company("zerog", Address(""))))

```

동반객체 (companion)는 팩터리 메서드 패턴을 구현할 때 용이하다.

```kotlin
class User private constructor(val name: String) { //private 생성자
    companion object {
        //이메일로 닉네임을 뽑아 User 를 생성
        fun newSubscribingUser(email:String) = User(email.substringBefore("@"))
        //id 로 User 를 생성
        fun newFacebookUser(id:Int) = User("${id}")
    }
}

//사용
UserObject.newSubscribingUser("zerogdevinfo@gmail.com")
UserObject.newFacebookUser(1)
```

상속/ 구현이 필요한 상황에서도 유용하게 쓸 수 있다.
```kotlin
interface JSONFactory {
    fun fromJSON(jsonText: String) : T
}


class Person(val name: String) {

    //동반 객체가 인터페이스를 구현한다
    companion object : JSONFactory<JSONPerson>{ 
        override fun fromJSON(jsonText: String): Person {
            return Person(jsonText)
        }
    }

}
```

## 5. 람다로 프로그래밍

코틀린에서의 람다

람다는 기본적으로 다른 함수에 넘길 수 있는 작은 코드 조각을 뜻한다. 람다를 자주 사용하는 경우로 컬렉션 처리를 예로 들 수 있다.
(람다라는 개념이 처음엔 생소 했는데, 코드 타이핑에 자연스레 녹아 있었다. 람다식을 직접 만드는 일이 거의 없이, 글로만 설명을 읽으니 생소 했던 듯)

람다식은 직접 구현 할 수도 있다.

```kotlin
val sum = { x: Int, y: Int -> x + y }
```

람다식은 중괄호로 둘러쌓여있다. -> 이전에는 인자절 (파라미터), 그 이후는 본문 (표현) 이다.

람다식을 직접 만들어 사용할 때는 run 메서드를 사용하면 된다. 

```kotlin
sum(1, 2)

run { x + y }
```

로컨 변수처럼 컴파일러는 람다 파라미터의 타입도 추론할 수 있으니, 타입도 명시할 필요가 없다.
원소가 하나일 때는 디폴트인 it 를 사용하여 본문 이전의 인자절도 제거할 수 있다 !

멤버 참조를 사용해 Person::age 와 같이도 사용할 수 있다.

코틀린에는 컬렉션을 다루는 편리한 함수들이 많으니 참고하자.

컬렉션 연산에는 지연(lazy) 연산이라는 게 있다.

filter, map 과 같은 함수들은 결과 컬렉션을 즉시 (eagerly) 생성한다. 이는 함수를 연쇄하면 매 단계마다 계싼 중간 결과를 새로운 컬렉션에 임시로 담는다는 말이다.
이는 원소가 많아질 수록 비효율 적이다. 이를 더 효율적으로 사용하려면 직접 컬렉션을 사용하는 대신 시퀀스를 사용하게 만들어야 한다.

```kotlin
people.asSequence()
  .map(Person::name)
  .filter { it.startsWith("A") }
  .toList()
```

이 시퀀스라는 인터페이스는 중간 연산과 최종 연산으로 나뉜다.
중간 연산은 map, filter 부분으로 다른 시퀀스를 반환한다. 최종 연산은 결과를 반환한다.

시퀀스를 사용하면서 주의할 점은 즉시 생성하는 게 아니기 때문에 첫 번째 함수가 다 실행되고, 두번째 함수가 실행 된다는 보장이 없다.

```kotlin
// 컬렉션 함수

val words = "The quick brown fox jumps over the lazy dog".split(" ")
val lengthsList = words.filter { println("filter: $it"); it.length > 3 }
    .map { println("length: ${it.length}"); it.length }
    .take(4)

println("Lengths of first 4 words longer than 3 chars:")
println(lengthsList)

// output:
// filter: The
// filter: quick
// filter: brown
// filter: fox
// filter: jumps
// filter: over
// filter: the
// filter: lazy
// filter: dog
// length: 5
// length: 5
// length: 5
// length: 4
// length: 4
// Lengths of first 4 words longer than 3 chars:
// [5, 5, 5, 4]

// 시퀀스
val words = "The quick brown fox jumps over the lazy dog".split(" ")
//convert the List to a Sequence
val wordsSequence = words.asSequence()

val lengthsSequence = wordsSequence.filter { println("filter: $it"); it.length > 3 }
  .map { println("length: ${it.length}"); it.length }
  .take(4)

println("Lengths of first 4 words longer than 3 chars")
// terminal operation: obtaining the result as a List
println(lengthsSequence.toList())

// output:
// Lengths of first 4 words longer than 3 chars
// filter: The
// filter: quick
// length: 5
// filter: brown
// length: 5
// filter: fox
// filter: jumps
// length: 5
// filter: over
// length: 4
// [5, 5, 5, 4]
```

with => 수신객체를 인자로 받아서 생략 가능
apply => 이와 동일하지만, 수신객체를 다시 반환하는 확장 함수



