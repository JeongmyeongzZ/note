- 우리가 클래스를 선언하고 구현할 때 따로 상속을 하지 않는다면 자동적으로 java.lang.Object 클래스가 상속되게 된다.
- Object 의 java.lang 패키지는 import 를 하지 않더라도 컴파일러에 의해 자동으로 import 가 되어진다. 
- 모든 클래스는 Object 클래스의 자식이거나 자손인 클래스로 정의할 수 있다. 

# 최상위 클래스인 Object 클래스에 있는 대표적인 메서드들

## toString()
객체를 문자열로 표현한 값(객체의 문자 정보)를 반환

```java
public String toString() {
	return getClass().getName() + "@" + Integer.toHexString(hashCode());
}

// package명.StringStudy$MyObject@3f0ee7cb
```

toString()을 오버라이딩함으로써 객체의 정보를 문자열로 표현하는데 효과적으로 사용할 수 있습니다.

---

## equals()
두 객체의 값이 같은지 boolean 값 반환
물리적으로 메모리 주소가 같지 않더라도, 값이 동일하면 true를 반환

```java
// 만약 전달받은 객체의 메모리 주소가 같다면 true를 반환
// 그렇지 않다면 값을 비교한 뒤 true / false 반환
public boolean equals(Object anObject) {
  if (this == anObject) {
      return true;
  }
  if (anObject instanceof String) {
      String aString = (String)anObject;
      if (coder() == aString.coder()) {
          return isLatin1() ? StringLatin1.equals(value, aString.value)
                            : StringUTF16.equals(value, aString.value);
      }
  }
  return false;
}
```

```java
String strA = new String("abc");
String strB = new String("abc");

System.out.println(strA == strB);
System.out.println(strA.equals(strB));

// false
// true
```

- 동일성
    - 두 객체가 완전히 같은지 판단할 수 있는 성질
    - 으로 주소값을 비교
- 동등성
   - 객체가 같은 정보를 가지고 있는지 판단할 수 있는 성질
   - equals()로 내용을 비교

코틀린에서도 == 연산자가 기본이다. 그러나 자바와는 동작 방식에 조금 차이가 있다. 
원시 타입 두개를 비교할 때는 == 연산자가 동일하게 동작하지만, 참조 타입을 비교할 때 다르게 동작한다.

==는 내부적으로 equals를 호출한다. 따라서 참조 타입인 두 개의 String을 ==연산으로 비교하면 주소값이 아닌 값(동등성)비교를 한다.

주소 값을 비교(reference comparision)하고 싶다면?
코틀린은 자바에는 없는 ===연산자를 지원한다. 참조 비교를 위해서 === 연산자를 사용하면 된다. 즉, 자바의 주소 값 비교인 ==와 코틀린의 ===가 동일한 역할을 한다.

--- 

## hashCode()
해시코드란 객체를 식별할 하나의 정수값을 의미한다. 
Object 클래스의 hashCode() 는 객체의 메모리 주소를 이용해서 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가지고 있다.

내부적으로는 JVM이 인스턴스를 생성할 때 메모리 주소를 10진수로 변환하여 해시코드를 부여합니다.
> 실제 메모리 주소와는 별개의 값이므로 실제 메모리 주소를 알고 싶을 때는 System 클래스의 identityHashCode()로 확인할 수 있습니다.

```java
Drink drink1 = new Drink("삼다수", "물");
Drink drink2 = new Drink("오아시스", "물");

System.out.println(drink1.hashCode());
System.out.println(drink2.hashCode());

// 1057941451
// 1975358023
```
