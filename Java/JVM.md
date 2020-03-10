# Java Virtual Machine

## JVM?
- JVM은 Java 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것이다.
- JVM은 Java와 OS 사이에서 중개자 역할을 수행하여 Java가 OS에 구애받지 않고 사용할 수 있는 환경을 만들어준다.
- 메모리 관리, Garbage Collection을 수행한다.
- 스택 기반의 가상머신이다.
- 한정된 메모리를 효율적으로 사용하기 위해 메모리 구조를 알아야 한다.
- 메모리가 관리되지 않은 경우 속도저하 현상이나 튕김 현상등이 일어날 수 있다.


## 자바 프로그램 실행 과정
1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받는다.
2. JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
3. 자바 **컴파일러(javac)** 가 자바 **소스코드(.java)** 를 읽어들여 자바 **바이트코드(.class)** 로 변환시킨다.
4. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.
5. 로딩된 class 파일들은 Excution engine을 통해 해석된다.
6. 해석된 바이트코드는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어진다.
7. 이러한 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC 등의 관리 작업을 수행한다.


![](https://github.com/brightestbulb/TIL/blob/master/java/img/JVM1.png?raw=true)


## JVM 구성

**Java Compiler**
- 자바 소스(.java) 코드를 Byte Code(.class)로 변환하는 역할을 한다.

**Class Loader**
- JVM내로 .class 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다. 
- Runtime 시에 동적으로 클래스를 로드하고 RuntimeDataArea에 배치한다.
- 이 동적 로드를 담당하는 부분이 JVM의 Class Loader이다.
- jar파일 내 저장된 클래스들을 JVM 위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제한다.


**Runtime Data Areas**
- JVM이라는 프로그램이 운영체제 위에서 실행되면서 할당받는 메모리 영역이다.
- 런타임 데이터 영역은 5개의 영역으로 나뉜다.
- PC register, JVM Stack, Native Method Stack은 스레드마다 하나씩 생성되고, Heap, Method Area는 모든 스레드가 공유해서 사용한다.
아래 그림은 실제 런타임 시에 어떻게 클래스들과 객체, 지역변수들이 할당되는지를 그림으로 표현한 것이다.
![](https://github.com/brightestbulb/TIL/blob/master/java/img/JVM2.png?raw=true)


**PC Register**
- 레지스터는 각 스레드마다 하나씩 존재하며 스레드가 시작될 때 생성된다.
- PC레지스터는 Thread가 어떤 명령어로 실행되어야 할지에 대해 기록하는 부분으로 현재 수행중인 JVM명령의 주소를 갖는다.


**Execution Engine(실행 엔진)**
- 클래스를 실행시키는 역할이다.
- 클래스 로더가 JVM내의 런타임 데이터 영역에 바이트 코드를 배치시킨 후 실행 엔진에 의해 실행된다.
