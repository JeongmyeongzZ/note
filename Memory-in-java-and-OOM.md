Out Of Memory 오류가 발생해 서비스가 정상적으로 동작하지 않는 상황이 생겼다. 데이터가 과도하게 많아서 이를 WAS내 메모리로 올리는 과정에서 문제가 발생했다고 한다. OOM에 대해 알아보자

# 자바에서 메모리 관리는 어떻게 이루어지나?


## Stack

Stack 에는 heap 영역에 생성된 Object 타입의 데이터들에 대한 참조를 위한 값들이 할당된다. 

또한, 원시타입(primitive types) - byte, short, int, long, double, float, boolean, char 타입의 데이터들이 할당된다. 

이때 원시타입의 데이터들에 대해서는 참조값을 저장하는 것이 아니라 실제 값을 stack 에 직접 저장하게 된다.

Stack 영역에 있는 변수들은 visibility 를 가진다. 변수 scope 에 대한 개념이다. 

전역변수가 아닌 지역변수가 foo() 라는 함수내에서 Stack 에 할당 된 경우 해당 지역변수는 다른 함수에서 접근할 수 없다. 

예를들어, foo() 라는 함수에서 bar() 함수를 호출하고 bar() 함수의 종료되는 중괄호 } 가 실행된 경우 (함수가 종료된 경우), bar() 함수 내부에서 선언한 모든 지역변수들은 stack 에서 pop 되어 사라진다.

Stack 메모리는 Thread 하나당 하나씩 할당된다. 즉, 스레드 하나가 새롭게 생성되는 순간 해당 스레드를 위한 stack 도 함께 생성되며 각 스레드에서 다른 스레드의 stack 영역에는 접근할 수 없다.


---

## Heap

모든 Object 타입(Integer, String, ArrayList, ...)은 heap 영역에 생성된다.

몇개의 스레드가 존재하든 상관없이 단 하나의 heap 영역만 존재한다.

Heap 영역에 있는 오브젝트들을 가리키는 레퍼런스 변수가 stack 에 올라가게 된다.




# OOM (Out Of Memory)?


OOME(Out Of Memory Error)는 JVM의 메모리가 부족하여 발생한 에러이다.

자바 애플리케이션 환경인 WAS 기반에서 수행된 서비스들에 대해서는 흔히 JVM Heep 메모리 관련 오류들을 흔하게 접할수 있다.

- Java.lang.OutOfMemoryError: Java heap space

Java의 Heap Memory 공간이 부족하여 발생한다. 공간 부족의 원인으로는 Heap Memory의 크기가 작아서 발생하는 경우와 애플리케이션 로직의 문제로 발생하는 경우가 있다.

해당 오류를 해결하기 위한 가장 쉬운 방법으로는 -Xmx 옵션을 사용하여 Heap Memory의 크기를 증가시키는 방법이 있지만 이는 GC Time의 증가를 동반하기 때문에 충분한 사전 테스트가 필요하다.

OOME가 발생한 시점에 생성된 Heap Dump 분석을 기반으로 쓸데없이 많은 Memory를 사용하거나 Memory Leak을 유발하는 로직을 찾아내어 수정해야 한다.


- Java.lang.OutOfMemoryError : PermGen space

Java Heep Memory 영역 중 Permanent 영역은 Class Method와 관련된 각종 Meta Data 등을 저장하는 용도로 사용된다. 

따라서 JVM 기동시 로딩되는 Class 또는 String의 수가 많다면 Java.lang.OutOfMemoryError : PermGen space의 원인이 됩니다. 또한 Classloader Leak에 의해 OOME가 발생될 수 있다.

Permanent 영역은 Heap 영역과는 달리 일반 비즈니스 프로그램으로 핸들링 할 수 없기때문에 JVM Option 튜닝으로도 해결이 되지 않는다면 WAS 혹은 JDK 버그를 의심해봐야 한다.

PermGen 영역은 Class를 로딩하고 해제하지 않으면 누수가 일어나며 PermGen 영역에 OutOfMemory가 발생할 수 있다. 

Class Loading 현황을 분석하려면 컨텍스트 메뉴에서 Java Basics > Class Loader Explorer를 선택해서 확인 할 수 있다.



_출처_
- https://yaboong.github.io/java/2018/05/26/java-memory-management/
- https://doohong.github.io/2019/05/16/Java-outofmemory/
