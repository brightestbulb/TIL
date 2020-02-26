# Java의 메모리 관리
Java에서 메모리 관리를 어떻게 하는지 알아보자.

## Stack
- Stack 에는 Heap 영역에 생성된 Object 타입의 데이터의 참조값이 할당된다.
- 원시 타입(byte, short, int, long, float, boolean, char) 타입의 데이터들이 할당된다.
- 원시 타입의 데이터들에 대해서는 참조값을 저장하는 것이 아니라 실제 값을 stack에 직접 저장한다.
- Stack 영역에 저장된 지역변수가 쓰인 함수가 종료되면 Stack에서 Pop되어 사라진다.
- Stack 메모리는 Thread 하나당 하나씩 생성되며 서로 다른 스레드들 간에 Stack 영역에 침범할 수 없다.

예시를 통해 Stack이 어떻게 활용되는지 살펴보자.

```java
public class Main {
 public static void main(String[] args) {
     int param = 10;
     int result = calculator(param);
 }

 private static int calculator(int num){
     int multiple = num * 3;
     int divide = multiple / 3;
     return divide;
 }
}
```

### 과정
1. int param = 10; 을 통해 Stack에 **param**이라는 변수명으로 공간이 할당되고, 값인 10이 할당된다.
2. calculator() 함수가 호출되는데, 인자로 변수 param을 넘겨주면 변수의 scope가 calculator() 함수로 이동한다. 기존의 param 값은 기존 scope에서 벗어나게 되면서 사용할 수 없다.
3. 인자로 넘겨받은 값은 파라미터인 num에 복사되어 전달되어 stack에 할당된 공간에 값이 할당된다.
4. 다음으로 multiple, divide 변수에 공간이 할당되어 값이 저장된다.
5. calculator() 함수가 닫히는 **}** 가 실행되어 함수가 종료되고, 호출함수 scope에서 사용되었던 모든 지역 변수들은 stack에서 pop 된다.
6. result 변수가 stack에 할당되어 결과값이 담기게 되고, param 변수는 다시 사용할 수 있는 상태가 된다.
7. main 함수가 종료되는 순간 stack에 있는 모든 데이터들은 pop 되면서 프로그램이 종료된다. 
