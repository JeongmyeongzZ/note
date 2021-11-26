# @Async


[비동기 : Asynchronous] 
우선 두가지 맥락을 이해하자. 비동기로 '동시성(Concurrency)' 혹은 '병렬성(Parallelism)' 을 구현할 수 있다. 


```
@Configuration
@EnableAsync
public class SpringAsyncConfig {
    
    @Bean(name = "threadPoolTaskExecutor")
    public Executor threadPoolTaskExecutor() {
        return new ThreadPoolTaskExecutor();
    }
}
```

Executors.newFixedThreadPool 메서드를 사용해서 쓰레드풀을 정의할 수 있다.

- newFixedThreadPool : 처리할 작업이 등록되면 그에 따라 실제 작업할 스레드를 하나씩 생성한다. 생성할 수 있는 쓰레드의 최대 개수는 제한되어 있으며, 제한된 개수까지 쓰레드를 생성한 후 쓰레드를 유지한다
- newCachedThreadPool : 캐시 쓰레드풀은 현재 갖고 있는 쓰레드의 수가 처리할 작업의 수보다 많아서 쉬는 쓰레드가 많이 발생할 때 쉬는 쓰레드를 종료시켜 훨씬 유연하게 대응할 수 있다. 처리할 작업의 수가 많아지면 그만큼 쓰레드를 생성한다. 반면에 쓰레드의 수에는 제한을 두지 않는다.
- newSingleThreadExecutor : 단일 쓰레드로 동작하는 Executor 로서 작업을 처리하는 쓰레드가 단 하나뿐이다.
- newScheduledThreadPool : 일정 시간 이후에 실행하거나 주기적으로 작업을 실행할 수 있으며, 쓰레드의 수가 고정되어 있는형태의 Executor.Timer 클래스의 기능과 유사하다.


실제로 @Async 애노테이션 동작을 살펴보면, return type 에 따라 동작이 차이가 있다.

- void 의 경우: ThreadPoolTaskExecutor 에 의해 submit 후 바로 return null (spring boot 2.1 이상에선 이 executor 설정도 가능. pool 의 max-size 도 설정 가능)
- Future 의 경우 (리턴값이 필요한): future.get() 실행 후 조회까지 계속 기다린다(블록킹) 논블록킹방식으로 동작하게 하려면 callback 으로 동작시켜야함
- ListenableFuture 의 경우: 위의 케이스의 callback 을 구현한 케이스
- CompletableFuture 의 경우: 완벽한 논블록킹 구현 가능

# EventListener w. @Async

기본적으로 Spring 이벤트는 동기식. (doStuffAndPublishAnEvent() 메소드는 모든 리스너가 이벤트 처리를 마칠 때까지 차단 됨)

@Async 어노테이션이 아니라 Configuration 추가를 통해 별도의 스레드에서 이벤트를 비동기식으로 처리하게 만들 수도 있다. 

```
@Configuration
public class AsynchronousSpringEventsConfig {
    @Bean(name = "applicationEventMulticaster")
    public ApplicationEventMulticaster simpleApplicationEventMulticaster() {
        SimpleApplicationEventMulticaster eventMulticaster =
          new SimpleApplicationEventMulticaster();
        
        eventMulticaster.setTaskExecutor(new SimpleAsyncTaskExecutor());
        return eventMulticaster;
    }
}
```

---


물론, CPU코어 개수는 제한되어있기 때문에, 컨텍스트 스위칭이 발생하면서 순간적으로 CPU 수치가 높게 올라갈 수는 있다.

적정 스레드 계산에는 여러 선행 확인이 필요하다.

1. OS에서 지원하는 Thread의 최대 갯수 (OS 전체에서 사용가능한 Threa갯수이다)
 
 > cat /proc/sys/kernel/threads-max
 >  kernel/fork.c  max_threads = mempages / (8 * THREAD_SIZE / PAGE_SIZE);

2. Process당 허용가능한 최대 Thread 갯수

> Linux는 프로세스당 Thread를 제한 하는 것이 아니라. 단지 전체 Thread 갯수만 관리한다.

3. Process당 적정 수준의 Thread 갯수
 
 > 바쁜 시스템  : cpu core개수 + 1 한가한 시스템 : cpu core개수 * 2  + 1

4. Thread 생성 에러가 나는 경우

> 메모리의 한계(out of memory 발생) 확인

_출처_
- https://blog.naver.com/kkforgg/221366212128
- https://brunch.co.kr/@springboot/401
- https://www.baeldung.com/spring-events
- https://www.baeldung.com/spring-async
 
