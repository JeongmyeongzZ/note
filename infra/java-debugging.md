
1. vmstat 등을 활용해 프로세스, 메모리, 스왑, usr CPU, sys CPU 정보등을 확인
> 스왑 영역은 RAM의 용량이 한계에 도달하여 추가적인 메모리 공간이 필요한 경우 OS가 일부를 이동시키는 공간 (보통 Disk 와 같은 보조기억장치)
> Usr cpu (GC, application logic 등), sys cpu (network 등 kernel level) 
2. jcmd pid 확인 (jps 와 같은 명령어로도 확인 가능)
> Java 가상 머신(JVM)에서 실행 중인 Java 애플리케이션을 진단하고 모니터링하기 위한 유틸리티 (jcmd 명령은 JDK에 포함되어 있음)
3. ps H -p <jvm_pid> -o pid,tid,pmem,pcpu,time,comm. 
>  JVM 프로세스의 모든 스레드에 대한 정보가 출력 됨 
4. Printf %x {tid} 와 같은 명령을 통해 스레드 id 를 16진수로 변환
5. jcmd <pid> Thread.print <16진수 tid> 명령을 활용해 특정 thread 에 대한 정보만 출력해서 확인 가능
