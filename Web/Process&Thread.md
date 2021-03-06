## Process
- 컴퓨터에서 연속적으로 실행되고 있는 프로그램
- 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)
- 운영체제로부터 시스템 자원을 할당받는 작업의 단위
- 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap)를 할당 받는다.
- 기본적으로는 프로세스당 최소 1개의 스레드(메인 스레드)를 갖는다.
- 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.


## Thread
- 프로세스 내에서 실행되는 여러 흐름의 단위
- 프로세스가 할당받은 자원을 이용하는 실행의 단위
- 스레드는 프로세스 내에서 각각 Stack만 따로 할당받고 Code, Data, Heap 영역은 공유한다.
- 스레드는 한 프로세스 내에서 동작되는 여러 실행의 흐름으로, 프로세스 내의 주소 공간이나 자원들(Heap 등)을 같은 프로세스 내에 스레드간에 공유한다.
- 각 스레드는 별도의 Stack을 갖지만, Heap은 서로 공유한다.


## Java Thread
- 일반 스레드와 거의 동일하며, JVM이 OS의 역할을 한다.
- 자바에는 프로세스가 존재하지 않고 스레드만 존재하며, 자바 스레드는 JVM에 의해 스케줄되는 실행 단위 코드 블록이다.
- JVM은 스레드 갯수, 스레드로 실행되는 프로그램 코드의 메모리 위치, 스레드의 상태, 우선순위 등을 관리한다.
- 개발자는 자바 스레드로 작동할 스레드 코드를 작성하고, 스레드 코드가 실행하도록 JVM 요청하는 일이다.


## Multi Process
- 하나의 응용 프로그램을 여러개의 프로세스로 구성하여 각 프로세스가 하나의 작업(Task)을 처리하도록 하는것이다.   

**[장점]**
- 여러개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스만 죽는것으로 끝난다. (다른 영향 확산 X)    

**[단점]**
- Context Switching 과정에서 캐시 메모리 초기화 등 무거운 작업이 진행되고 많은 시간이 소모되는 등의 오버헤드가 발생하게 된다.   
- 프로세스는 각각의 독립된 메모리 영역을 할당 받았기 때문에 프로세스 사이에서 공유하는 메모리가 없어, Context Switching가 발생하면
 캐시에 있는 모든 데이터를 리셋하고 다시 캐시 정보를 불러와야 한다.

**Context Switching**
: CPU에서 여러 프로세스를 돌아가면서 작업을 처리하는 과정. 동작 중인 프로세스가 대기를 하면서 해당 프로세스의 상태(Context)를 보관하고, 대기하고 있던 다음 순서의
프로세스가 동작하면서 이전에 보관했던 프로세스의 상태를 복구하는 작업을 말한다.

## Multi Thread
- 하나의 응용 프로그램을 여러개의 스레드로 구성하고 각 스레드로 하여금 하나의 작업을 처리하도록 하는 것이다.
- 윈도우, 리눅스 등 많은 운영체제들이 멀티 프로세싱을 지원하고 있지만 멀티 스레딩을 기본으로 하고 있다.
- 웹 서버는 대표적인 멀티 스레드 응용 프로그램이다.   
 
**[장점]**
- 시스템 자원 소모 감소(자원의 효율성 증대)
- 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다.
- 시스템 처리량 증가(처리 비용 감소) 
- 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.
- 스레드 사이의 작업량이 작아 context Switching이 빠르다.
- 간단한 통신 방법으로 인한 프로그램 응답 시간 단축
- 스레드는 프로세스 내의 Stack 영역을 제외한 모든 메모리를 공유하기 때문에 통신의 부담이 적다.   

**[단점]**
- 주의 깊은 설계가 필요하다.
- 디버깅이 까다롭다.
- 단일 프로세스 시스템일 경우 효과를 기대하기 어렵다.
- 다른 프로세스에서 스레드를 제어할 수 없다. (프로세스 밖에서 스레드 각각을 제어할 수 없다.)
- 멀티 스레드의 경우 자원 공유의 문제가 발생한다. (동기화 문제)
- 하나의 스레드에 문제가 생기면 전체 프로세스가 영향을 받는다.

## Multi Process 대신 Multi Thread를 사용하는 이유?
- 프로그램을 여러개 키는 것보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이다.
- 멀티 프로세스로 할 수 있는 작업들을 하나의 프로세스에서 여러 스레드로 나눠서 하면 좋은점?
    - 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다.
    - 스레드는 프로세스 내의 메모리를 공유하기 때문에 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어든다.
    - 프로세스간의 통신(IPC)보다 스레드 간의 통신 비용이 적으므로 작업들 간의 통신의 비용이 줄어든다.
    - 스레드는 Stack 영역을 제외한 모든 메모리를 공유하기 때문.
    - 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.
    - Context Switching 시 스레드는 Stack 영역만 처리하기 때문이다.
- [주의점] : 스레드 간의 자원 공유는 전역 변수(데이터 세그먼트)를 이용하므로 함께 사용할 때 충돌이 발생할 수 있다. 


출처 : https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html  
