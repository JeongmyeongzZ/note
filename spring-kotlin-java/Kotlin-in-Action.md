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

